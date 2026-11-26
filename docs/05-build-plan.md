# 05 · Build plan

Four phases. Finish each phase's acceptance criteria before starting the next. Commit after every task.

## Phase 0 · Foundations

Tasks:

1. Create the Expo app with TypeScript and Expo Router, following current Expo documentation.
2. Add `src/theme/tokens.ts` with every token from `docs/02-design.md`, and load Newsreader and Inter.
3. Build the shared components: primary, secondary and text buttons, choice button, card, suggestion card, stage bar, slider with a word and number readout, tab bar, bottom sheet.
4. Set up the four tabs: Today, Dialogue, Map, Ledger, each with a placeholder screen.
5. Create the Supabase project config, write the migrations from `docs/03-data-model.md`, and add row-level security.
6. Add sign-in. Email magic link is enough for now.
7. Set up linting, type checking and a test runner.

Acceptance criteria:

- The app runs on an iOS simulator and an Android emulator.
- The four tabs switch, and the active tab shows the amber indicator.
- A component gallery screen shows every shared component.
- Migrations apply cleanly to an empty database.
- A test proves a signed-in person cannot read another person's rows.

## Phase 1 · The daily loop

The whole product in its smallest form. This phase tests whether people come back.

Tasks:

1. Seed a week of approved daily statements with cached opening questions.
2. Today screen showing today's statement card.
3. Today's statement screen with both sliders and "I don't know enough yet".
4. Your position screen.
5. Edge function for job 1 (next question) with schema validation and the fixed fallback.
6. Dialogue question screen: single choice, multiple choice, Skip, the stage bar.
7. Edge function for job 2 (extractor), writing nodes and edges.
8. Dialogue recap screen with confirm, reword and remove for guesses.
9. Map tree view with the node bottom sheet. "Explore further" can be a stub.
10. Synthesis screen: re-rate, note, save.
11. A minimal Ledger tab: a flat list of saved beliefs.

Acceptance criteria:

- A new person can go from opening the app to a saved belief in under five minutes on a Quick session.
- Every answer is stored as a `turns` row with the options exactly as shown.
- A guess appears dashed, and becomes solid only after "That's mine" or Reword.
- With the model switched off, the dialogue still completes using fallback questions.
- Both the initial and the synthesis ratings exist for the belief.
- Leaving mid-dialogue and returning resumes at the same question.

## Phase 2 · The ledger

Tasks:

1. Edge function for job 3 (sorter): embeddings, category assignment.
2. Ledger tab home: category tiles with icons, counts and one mark per belief.
3. Derived ledger mark as a tested function or view.
4. Inside a category, grouped by beliefs, open questions and assumptions.
5. One belief screen and belief history timeline.
6. Rename a category, merge two categories, move a belief, add a belief to a second category.
7. New belief screen (structured entry) wired into the dialogue.
8. Dialogue tab home with open dialogues and the three ways to start.
9. Map tab home listing maps.
10. Revisit: set `revisit_on`, surface due beliefs on Today, store a new rating with `context = 'revisit'`.
11. Search across beliefs.

Acceptance criteria:

- Every saved belief lands in a category without the person choosing one.
- A new category shows the NEW tag until opened.
- The ledger marks match the rules in `docs/03-data-model.md` for a seeded set of test beliefs.
- Revising a belief adds a version and leaves earlier versions unchanged.
- A belief in two categories appears in both and is counted once in any total.

## Phase 3 · Memory and depth

Tasks:

1. Connection suggestions from job 3, shown as suggestion cards on Today, in a dialogue, and on a belief.
2. Own-words answers on every question, and "Show me choices instead".
3. Questioning modes, switchable mid-dialogue.
4. "Explore further" from a map node, starting a focused dialogue.
5. "Save as its own belief" from a node.
6. Offline cache and queued writes.
7. Account export as JSON and account deletion.
8. Cost and token logging per dialogue.

Acceptance criteria:

- A suggestion can be dismissed and does not come back.
- No more than one new connection is surfaced per day.
- The app opens and shows the ledger with no network.
- Export contains every row for the person; deletion removes every row.

## Phase 4 · Polish and launch

Tasks:

1. Map canvas view with pinch, pan and tap.
2. Voice input for own-words answers.
3. Optional daily reminder notification.
4. Review queue tool for daily statements.
5. Onboarding: three screens at most.
6. Accessibility pass: screen reader labels, dynamic type, contrast.
7. Privacy policy, data-use settings with the model provider, and a legal review of handling sensitive personal data.
8. App store builds and submission, following current Expo documentation.

Acceptance criteria:

- Canvas view stays smooth with 30 nodes.
- Every interactive element has a screen reader label.
- Text contrast is at least 4.5 to 1 throughout.

## What to measure from Phase 1

- Completed reflections per person per week. A reflection counts when a dialogue is done and a synthesis rating is saved.
- AI cost per completed reflection.
- Share of answers given by tap versus own words.
- Share of Athena's guesses confirmed, reworded and removed.
