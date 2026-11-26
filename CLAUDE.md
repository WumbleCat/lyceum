# Project Athena

A mobile app for personal intellectual growth. People state a belief, answer short questions about why they hold it, watch an assumption map get drawn from their answers, and keep every belief in a ledger sorted by category.

Working name: Project Athena. Stage: first build from specs. Nothing exists yet.

## Read these first

| File | What it holds |
| --- | --- |
| `docs/01-product.md` | Concept, principles, features, user journeys, the inquiry framework |
| `docs/02-design.md` | Design tokens and a spec for every screen |
| `docs/03-data-model.md` | Postgres schema, row-level security, derived values |
| `docs/04-ai.md` | The three AI jobs, their inputs, JSON outputs and rules |
| `docs/05-build-plan.md` | Phases, tasks and acceptance criteria |

Build one phase at a time, in the order given in `docs/05-build-plan.md`. Do not start a later phase until the current phase's acceptance criteria pass.

## Stack

- App: Expo (React Native) with TypeScript and Expo Router. One codebase for iOS and Android.
- Backend: Supabase. Postgres, auth, row-level security, edge functions.
- AI: a language model called only from Supabase edge functions, always returning JSON that is validated against a schema.
- Similarity search: pgvector inside the same Postgres database.
- Offline: local SQLite cache that syncs to Supabase.

These specs were written without checking current versions. Before installing anything, check the current Expo and Supabase documentation and use the versions and setup steps they recommend. Do not pin a version because this file implies one. Keep the model name and provider in environment variables, never hard-coded.

## Rules that must hold everywhere

1. Structured data, not transcripts. Every answer becomes rows (turns, nodes, edges). Never store a dialogue only as a blob of chat text.
2. Athena never rewrites a person's belief. Belief versions are append-only and only the person creates a new version.
3. Anything Athena infers is a guess until the person confirms it. Guesses are stored with `source = 'athena_guess'` and `confirmed = false`, and are always drawn dashed.
4. Questions are neutral. A question never presumes the person's motive or that their belief is wrong.
5. Every question can be skipped and every tap question offers "Answer in my own words".
6. No scores for intelligence, no streak pressure, no public feed, no leaderboards.
7. Private by default. Every user table has row-level security keyed to the signed-in user. The model API key never reaches the phone.
8. Touch targets are at least 44 points. Do not draw a fake status bar.

## Conventions

- TypeScript strict mode. No `any` without a comment saying why.
- Design tokens live in one file (`src/theme/tokens.ts`) and screens import from it. No hex values in screen files.
- Database changes go in Supabase migration files, never by hand in the dashboard.
- Every edge function validates its input and the model's output with a schema library, and returns a typed error when validation fails.
- Write tests for: the derived ledger status, the question-selection fallback, and every edge function's schema validation.
- Commit after each task in the build plan with a message that names the task.

## When something is unclear

Ask before guessing on anything that changes the data model or what gets stored about a person. For visual details not covered in `docs/02-design.md`, follow the nearest existing screen.
