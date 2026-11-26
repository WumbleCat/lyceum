# 03 · Data model

Postgres on Supabase. Every table that holds a person's data has a `user_id` and row-level security.

This schema is a starting point. Adjust types and syntax to what current Supabase and pgvector documentation recommends, and keep it in migration files.

## Tables

```sql
-- One row per person, created on sign-up.
create table profiles (
  id uuid primary key references auth.users on delete cascade,
  created_at timestamptz not null default now()
);

-- Categories are created by Athena. People can rename or merge them.
create table categories (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references profiles on delete cascade,
  name text not null,
  icon_key text not null default 'generic',
  created_by text not null default 'athena' check (created_by in ('athena', 'user')),
  seen boolean not null default false,          -- false shows the NEW tag
  created_at timestamptz not null default now(),
  unique (user_id, name)
);

-- One statement per day, the same for everyone.
create table daily_statements (
  id uuid primary key default gen_random_uuid(),
  publish_date date not null unique,
  statement text not null,
  claim_types text[] not null,                  -- empirical, causal, ethical, philosophical, predictive, personal
  suggested_category text not null,
  opening_questions jsonb not null default '[]',-- pre-generated first questions, cached for everyone
  status text not null default 'draft' check (status in ('draft', 'approved')),
  reviewed_by text,
  created_at timestamptz not null default now()
);

-- A belief is a container. Its wording lives in belief_versions.
create table beliefs (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references profiles on delete cascade,
  kind text not null default 'belief' check (kind in ('belief', 'question', 'assumption')),
  origin text not null check (origin in ('daily', 'own', 'from_node')),
  daily_statement_id uuid references daily_statements,
  revisit_on date,
  created_at timestamptz not null default now(),
  archived_at timestamptz
);

-- Append-only. Never update or delete a row here.
create table belief_versions (
  id uuid primary key default gen_random_uuid(),
  belief_id uuid not null references beliefs on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  statement text not null,
  reasoning text,
  change_my_mind_if text,
  change_note text,                             -- the person's note on what changed
  embedding vector,                             -- set the dimension to match the embedding model
  created_at timestamptz not null default now()
);

create table belief_categories (
  belief_id uuid not null references beliefs on delete cascade,
  category_id uuid not null references categories on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  assigned_by text not null default 'athena' check (assigned_by in ('athena', 'user')),
  primary key (belief_id, category_id)
);

-- Every rating is kept. The latest one is the current position.
create table ratings (
  id uuid primary key default gen_random_uuid(),
  belief_id uuid not null references beliefs on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  agreement smallint check (agreement between 0 and 100),
  founded smallint check (founded between 0 and 100),
  dont_know boolean not null default false,
  based_on text[] not null default '{}',        -- experience, evidence, heard, intuition
  context text not null check (context in ('initial', 'synthesis', 'revisit')),
  created_at timestamptz not null default now()
);

create table dialogues (
  id uuid primary key default gen_random_uuid(),
  belief_id uuid not null references beliefs on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  focus_node_id uuid,                           -- set when started from a node on the map
  length text not null default 'quick' check (length in ('quick', 'deep')),
  mode text not null default 'clarify' check (mode in ('clarify', 'other_side', 'evidence', 'strongest')),
  current_stage smallint not null default 1 check (current_stage between 1 and 6),
  status text not null default 'open' check (status in ('open', 'done')),
  started_at timestamptz not null default now(),
  finished_at timestamptz
);

create table turns (
  id uuid primary key default gen_random_uuid(),
  dialogue_id uuid not null references dialogues on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  idx smallint not null,
  stage smallint not null check (stage between 1 and 6),
  family text not null,                         -- familiarity, definition, origin, reasoning, certainty, scope, alternatives, reconsideration
  format text not null check (format in ('single', 'multi', 'slider', 'open')),
  question text not null,
  options jsonb not null default '[]',          -- the options exactly as shown
  answer jsonb,                                 -- {picked:[...]} or {value:n} or {text:"..."}
  answered_in_own_words boolean not null default false,
  skipped boolean not null default false,
  created_at timestamptz not null default now(),
  unique (dialogue_id, idx)
);

create table nodes (
  id uuid primary key default gen_random_uuid(),
  belief_id uuid not null references beliefs on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  type text not null check (type in ('main', 'reason', 'assumption', 'evidence', 'counter', 'open_question', 'revised')),
  text text not null,
  source text not null check (source in ('user_said', 'user_picked', 'athena_guess')),
  confirmed boolean not null default true,      -- false for a new athena_guess
  examined boolean not null default false,      -- evidence that has been looked at
  turn_id uuid references turns,
  created_at timestamptz not null default now(),
  removed_at timestamptz
);

create table edges (
  id uuid primary key default gen_random_uuid(),
  belief_id uuid not null references beliefs on delete cascade,
  user_id uuid not null references profiles on delete cascade,
  from_node uuid not null references nodes on delete cascade,
  to_node uuid not null references nodes on delete cascade,
  relation text not null check (relation in ('supports', 'depends_on', 'contradicts', 'alternative_to'))
);

-- "A connection worth exploring". Always a suggestion.
create table connections (
  id uuid primary key default gen_random_uuid(),
  user_id uuid not null references profiles on delete cascade,
  belief_a uuid not null references beliefs on delete cascade,
  belief_b uuid not null references beliefs on delete cascade,
  shared_text text,
  status text not null default 'suggested' check (status in ('suggested', 'explored', 'dismissed')),
  created_at timestamptz not null default now()
);
```

## Row-level security

- Enable row-level security on every table.
- For every table with `user_id`: select, insert, update and delete are allowed only where `user_id = auth.uid()`. For `profiles` use `id = auth.uid()`.
- `belief_versions`: allow select and insert only. No update or delete policy, so history cannot be rewritten from the client.
- `daily_statements`: any signed-in user can select rows where `status = 'approved'` and `publish_date <= current_date`. Only the service role can write.
- Edge functions that call the model use the person's token for reads and writes wherever possible, so row-level security still applies.

## Derived values

Compute these in a database view or a tested TypeScript function. Do not store them.

Ledger mark for one belief:

| Mark | Rule |
| --- | --- |
| Guess to confirm (amber square) | The belief has at least one node with `source = 'athena_guess'`, `confirmed = false` and `removed_at is null` |
| Examined (green circle) | Not the above, and the belief has at least one dialogue with `status = 'done'` |
| Not explored (hollow circle) | Neither of the above |

Other derived values:

- Current wording: the latest `belief_versions` row for the belief.
- Current position: the latest `ratings` row for the belief.
- Agreement word: 0 to 19 strongly disagree, 20 to 39 mostly disagree, 40 to 59 on the fence, 60 to 79 mostly agree, 80 to 100 strongly agree.
- Agreement meter: `ceil(agreement / 20)` segments out of five, minimum one.
- "Holds up n of your beliefs": the number of distinct beliefs with a confirmed assumption node whose text matches, by embedding similarity above a threshold set during testing.

## Rules the code must enforce

1. A new `athena_guess` node is always inserted with `confirmed = false`.
2. "That's mine" sets `confirmed = true`. "Reword" updates `text`, sets `source = 'user_said'` and `confirmed = true`. "Remove" sets `removed_at`.
3. "Save as its own belief" creates a belief with `origin = 'from_node'` and a first version using the node's text.
4. Revising a belief inserts a new `belief_versions` row. It never updates an old one.
5. Finishing synthesis inserts a `ratings` row with `context = 'synthesis'`, sets the dialogue to done, and sets `revisit_on` to about a month ahead.
6. Deleting an account deletes every row for that person. Provide a full export as JSON.

## Offline

Cache the person's beliefs, latest versions, ratings, nodes, edges, categories and today's statement in local SQLite. Writes made offline are queued and replayed in order. Dialogue questions need the network; show a clear message when it is missing and let the person answer in their own words to be processed later.
