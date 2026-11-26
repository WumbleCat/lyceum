# Prompts for Claude Code

Put `CLAUDE.md` in the root of an empty repository and the five spec files in `docs/`. Start Claude Code in that folder and paste these one at a time. Wait for each to finish and check its work before pasting the next.

## 1 · Orient

```
Read CLAUDE.md and every file in docs/. Do not write any code yet.
Then tell me: (1) your understanding of the product in five sentences,
(2) anything in the specs that is contradictory or missing,
(3) what you need from me before starting, such as accounts or keys,
(4) which versions of Expo and Supabase tooling you will use after checking their current docs.
```

## 2 · Phase 0

```
Build Phase 0 from docs/05-build-plan.md. Work through the tasks in order and commit after each one.
Check current Expo and Supabase documentation before installing anything.
When you finish, run the acceptance criteria and show me the result of each one.
Stop and ask if anything would change the data model.
```

## 3 · Phase 1

```
Build Phase 1 from docs/05-build-plan.md, the daily loop. Follow docs/02-design.md for each screen
and docs/04-ai.md for the two edge functions. Use the tokens file for all colours and type.
Build the fallback questions before the model call so the loop works with the model switched off.
Commit after each task. When you finish, run the acceptance criteria and show me the result of each one.
```

## 4 · Phase 2

```
Build Phase 2 from docs/05-build-plan.md, the ledger. Write the derived ledger mark as a tested function first,
using the rules in docs/03-data-model.md, then build the screens on top of it.
Commit after each task. When you finish, run the acceptance criteria and show me the result of each one.
```

## 5 · Phase 3

```
Build Phase 3 from docs/05-build-plan.md, memory and depth.
Connection suggestions must follow the rules in docs/04-ai.md: never claim a contradiction, at most one new suggestion a day.
Commit after each task. When you finish, run the acceptance criteria and show me the result of each one.
```

## 6 · Phase 4

```
Build Phase 4 from docs/05-build-plan.md. Start with the accessibility pass and the canvas view.
For app store submission, follow current Expo documentation and list every step that needs me.
```

## Useful at any point

```
Review the code against the "Rules that must hold everywhere" section of CLAUDE.md.
List every place a rule is broken or untested, with file and line, then fix them.
```

```
Compare the <screen name> screen with its spec in docs/02-design.md and list every difference. Do not change anything yet.
```
