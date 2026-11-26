# 02 · Design

A dark, calm thinking space with warm ivory type. It should feel like a private notebook, not a feed.

## Tokens

Colour:

| Token | Hex | Use |
| --- | --- | --- |
| `bg` | `#12110F` | Screen background |
| `surface` | `#1C1A17` | Cards, inputs, bottom sheet |
| `border` | `#33302A` | Card outlines, dividers, tab bar top line |
| `borderStrong` | `#5A554B` | Input and secondary button outlines |
| `text` | `#EFE9DC` | Primary text; also the fill of primary buttons |
| `textSecondary` | `#C9C3B5` | Supporting text |
| `textMuted` | `#A39E92` | Labels, captions, inactive tabs |
| `onLight` | `#12110F` | Text on ivory or amber fills |
| `amber` | `#E0A93B` | Assumptions, Athena's guesses, links, active indicators |
| `blue` | `#6C9FE0` | Evidence outlines and the well-foundedness slider |
| `blueText` | `#8DB6EA` | Evidence labels and numbers |
| `green` | `#A8DDB0` | Revised beliefs, "examined", "holds together" |
| `hollow` | `#8A8477` | Dashed outlines for open questions and unexplored items |
| `meterEmpty` | `#3A372F` | Empty segments of a meter |

Colour never carries meaning alone. Every coloured element also has a text label, and status also differs by shape or line style.

Type:

| Role | Font | Size / line height |
| --- | --- | --- |
| Screen title | Newsreader 400 | 32 / 1.1 |
| Statement, question | Newsreader 400 | 24 to 34 / 1.15 |
| Belief text in cards | Newsreader 400 | 18 to 20 / 1.25 |
| Body | Inter 400 or 500 | 15 / 1.4 |
| Caption | Inter 400 | 13 / 1.4 |
| Section label | Inter 600, uppercase, letter spacing 0.08em | 12 |
| Button | Inter 600 | 15 to 17 |

Load both fonts through Expo's font tooling with system fallbacks.

Layout:

- Design baseline is a 390 by 844 point phone. Layouts must flex to other sizes.
- Screen padding 24 horizontal. Gaps of 8, 12, 14 or 16 between blocks.
- Corner radius 6 for buttons and cards, 8 for ledger tiles, 14 on the top corners of the bottom sheet.
- Touch targets at least 44 points high.
- Respect safe areas. Never draw a status bar.

Components:

| Component | Spec |
| --- | --- |
| Primary button | Ivory fill, dark text, height 52 to 56, full width, one per screen |
| Secondary button | Transparent, `borderStrong` outline, ivory text, height 44 |
| Text link button | No outline, amber text weight 600, height 44 |
| Choice button | Height at least 50, left-aligned, a letter (A to E) then the option. Selected: ivory fill, dark text |
| Card | `surface` fill, 1 point `border` outline, padding 14 to 18 |
| Suggestion card | No fill, 1 point dashed `borderStrong` outline, label, quoted text, two text buttons |
| Stage bar | Six equal segments, 4 points high, amber up to the current stage, `border` after |
| Tab bar | Four text labels, 60 high, top line `border`. Active tab: ivory text and a 2 point amber line at its top edge |
| Bottom sheet | `surface` fill, top line `borderStrong`, sits above the tab bar |

Line styles on the map and ledger:

- Solid outline: came from the person.
- Dashed outline: Athena's guess, or something not looked at yet.

## Screens

### Today

- Header row: "ATHENA" wordmark left, date right.
- Title: "What are you thinking about?"
- Two buttons side by side: "Write a belief" (primary, opens New belief) and "Speak" (secondary).
- Today's statement card with an amber outline: label, the statement, "How much do you agree?" and "Take a position". Opens Today's statement.
- Continue card: category and status label, the belief, how many assumptions are left to look at. Opens its map.
- Suggestion card: "Possible connection", one sentence, "Take a look" and "Not now".

### Today's statement

- Label with the date, then the statement in large serif.
- Agreement: question, a word for the current value (Strongly disagree / Mostly disagree / On the fence / Mostly agree / Strongly agree, in 20 point bands), the number in amber, the slider, end labels.
- Well-foundedness: same layout in blue. Words: A hunch / Fairly unsure / Fairly sure / Very sure, in 25 point bands. End labels "Pure hunch" and "I've looked into it".
- "I don't know enough yet" toggle button.
- Primary button: "Lock it in".

### Your position

- Label showing both ratings, then the statement.
- "Why do you think this?" multi-line input.
- "What are you basing it on?" four toggle buttons in a 2 by 2 grid.
- "What are you unsure about? · optional" single-line input.
- "How long have you got?" two large buttons: Quick (3 questions) and Deep (All six stages).
- Primary button: "Start the dialogue".

### New belief

- Title "New belief".
- Fields: I believe that (serif, multi-line), Because (multi-line), How sure (slider with the number), I'd change my mind if (multi-line).
- Caption: Athena files this in a category and draws the map; either can be corrected.
- Primary button: "Save and explore".

### Dialogue · tab home

- Title "Dialogue" and one line of description.
- "Pick up where you left off": a card per open dialogue with the belief, a stage bar, the stage name and "Continue".
- "Start a new one from": three rows: Today's statement, A belief in your ledger, Something new.
- "How Athena asks": four mode buttons in a 2 by 2 grid (Clarify, Other side, Evidence, Strongest version).
- Caption: Athena won't argue for the sake of it.

### Dialogue · question

- Stage bar, then "STAGE n OF 6 · NAME" in amber on the left and the question family in muted text on the right.
- One muted line with the belief and the agreement rating.
- The question in serif, then a hint ("Pick one." or "Pick all that apply.").
- Choice buttons, up to five.
- Primary button: "Next question", or "See what Athena mapped" on the last question.
- Below it: "Answer in my own words" (left) and "Skip" (right).

### Dialogue · own words

- Same header as a question.
- The question in serif, a multi-line input, "Show me choices instead" and a microphone button.
- A suggestion card may appear below the input.
- Primary button: "Next question" or "Finish the dialogue".

### Dialogue · recap

- Label "WHAT ATHENA HEARD" and title "Your reasoning, as you gave it."
- Rows, each with a left label (YOU PICKED or YOU SAID) and the content.
- A dashed amber box per unconfirmed guess with three buttons: "That's mine", "Reword", "Remove".
- A green-outlined box "Where it holds together" with two sentences.
- Two buttons: "See the map" (secondary) and "Re-rate" (primary).

### Map · tab home

- Title "Maps" and one line of description.
- A card per map: category and date labels, the belief, a count of each node type, and what is left to do in amber.
- A suggestion card when two maps appear to share an assumption.

### Map · tree view

- Header: a Tree / Canvas segmented control on the left, "Finish and re-rate" on the right.
- Main belief card with an ivory fill and dark text.
- A short vertical connector with the label "RESTS ON".
- A vertical list of node cards. Each has a label giving its type and source, then its text. Outline colour and style follow the tokens and line styles above.
- Tapping a node opens the bottom sheet. For a guess the sheet first asks "Is this guess what you think?" with "Yes, that's mine", then a 2 by 2 grid: Explore further, Reword it, Find evidence, Save as its own belief.

### Map · canvas view

- Same header, with Canvas selected and "All maps" on the right.
- The main belief at the top centre, children below, joined by 2 point lines. Pinch to zoom, drag to pan, tap a node for the same bottom sheet.
- A two-line key for solid and dashed outlines.
- Zoom out and zoom in buttons at the bottom right.
- This is the hardest screen. Build it last.

### Synthesis

- Label "STAGE 6 OF 6 · SYNTHESIS" in green, title "Where do you stand now?", the statement.
- Agreement slider showing "before → now".
- Well-foundedness slider showing "before → now".
- "What changed, if anything?" input with the placeholder "Staying where you were is a fine answer."
- "Athena will file this under": category chips and a "Change" link. A new category is marked as new.
- Primary button: "Save to ledger".

### Ledger · tab home

- Title "Ledger" with a search icon button on the right.
- A key with three marks: green circle "Examined", amber square "Guess to confirm", hollow circle "Not explored".
- A 2-column grid of category tiles. Each tile: a 36 point line icon top left, the belief count top right in serif, the category name, then one mark per belief using the key.
- A newly created category has an amber outline and a "NEW" tag.
- One caption line: Athena sorts every belief into a category; one dot is one belief.

Category icons are simple line icons, stroke 1.5, drawn in ivory: flame (Religion), flask (Science), sigma (Mathematics), question mark in a circle (Philosophy), die (Gambling), rising line chart (Economics), chip (Technology), person (Personal). Use a neutral fallback icon for categories without one. The flame is deliberate: it does not stand for any single faith.

### Ledger · inside a category

- Back link "Ledger".
- The category icon at 52 points beside the title, with the three counts and their marks.
- Entries grouped under icon headings: Beliefs, Open questions, Assumptions underneath.
- Belief card: the text, a five-segment meter for agreement with its word, and "Also in <category>" when it sits in more than one.
- Open question card: dashed outline, the question, "Explore".
- Assumption card: amber outline, the text, a link icon and "Holds up n of your beliefs".

### Ledger · one belief

- Back link to the category, and "Move to another category".
- First wording in muted serif with its date and rating.
- Current wording in a green-outlined card with its rating and one neutral caption.
- Rows: "Rests on n assumptions" (opens the map), the open question (opens a dialogue), "History" (opens the timeline).
- A suggestion card for a possible connection.

### Ledger · belief history

- Back link "Back to the belief".
- Title "How this belief has moved".
- A vertical timeline: first written, each dialogue (with "Read it back"), each revision in the person's words with their note, and the next revisit.
- Caption: nothing here is overwritten.
- Primary button: "Explore it again now".

## Copy rules

- Plain words and sentence case. Section labels are the only uppercase text.
- Name things by what the person controls: "That's mine", "Reword", "Not what I meant".
- Never tell the person they are wrong. Say what is unexamined.
- Empty states say what will appear and how to start.
