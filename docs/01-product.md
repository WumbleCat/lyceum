# 01 · Product

## Concept

Athena helps a person find out what they think, why they think it, and whether the reasoning holds. It does not tell them what to think.

The app has four tabs: Today, Dialogue, Map, Ledger. They are four views of the same underlying beliefs.

| Tab | Job |
| --- | --- |
| Today | Entry point: today's statement, a way to write a new belief, unfinished work, quiet suggestions |
| Dialogue | Athena asks one question at a time, mostly answered by tapping |
| Map | The assumption map Athena draws from the answers |
| Ledger | Every belief, sorted into categories Athena creates |

## Principles

1. Clarify before challenging. Understand what the person means first.
2. Question without leading. Options always include considerations on more than one side.
3. Adapt to what the person knows. Skip background for someone familiar with the topic.
4. Acknowledge sound reasoning. Athena never argues for the sake of it.
5. Uncertainty is a valid answer. "I'm not sure", "It depends" and "I don't know enough yet" are always available.
6. The person is the author. Athena's guesses wait for confirmation and can be reworded or removed.
7. Success is a completed reflection, not a changed mind. Staying where you were is a fine outcome.

## The daily statement

One statement per day, the same for everyone, for example "Palantir is doing more harm than good."

The person records two separate things:

- Agreement, 0 to 100, from strongly disagree to strongly agree.
- How well-founded that position is, 0 to 100, from pure hunch to "I've looked into it".

There is also an "I don't know enough yet" button, because the middle of the agreement bar should not have to mean both neutral and uninformed.

Then a short structured entry:

- Why do you think this? (required, a sentence or two)
- What are you basing it on? (multi-select: personal experience, evidence I've seen, something I heard, intuition)
- What are you unsure about? (optional)
- Session length: Quick (3 questions) or Deep (all six stages)

Statements about real companies or people must pass a human review queue before publishing and must be worded neutrally.

## Writing your own belief

Structured entry with four fields: I believe that, Because, How sure (0 to 100), I'd change my mind if. Athena then files it in a category and starts the map.

## The inquiry framework

Six reusable stages. Athena uses them as a framework and picks the next question from the person's answers. It may skip a stage or spend several questions on one.

| # | Stage | Question it answers |
| --- | --- | --- |
| 1 | Define | What are we discussing, and what do the key terms mean? |
| 2 | Context | What is the background: history, precedent, how it works? |
| 3 | Reasoning | Why do you believe it? What are the premises? |
| 4 | Evidence | What supports or challenges it, and how reliable is that? |
| 5 | Alternatives | What else could be true? Counterexamples, trade-offs, counterfactuals |
| 6 | Synthesis | What do you think now, and what is still uncertain? |

Emphasis depends on the kind of claim:

| Claim type | Example | Emphasis |
| --- | --- | --- |
| Empirical | Most gamblers lose money in the long run | Definitions, data, base rates |
| Causal | Social media causes loneliness | Mechanisms, confounders, alternatives |
| Ethical | Palantir does more harm than good | Stakeholders, values, trade-offs |
| Philosophical | Mathematics is discovered, not invented | Definitions, premises, competing frameworks |
| Predictive | AI will replace most jobs | Time horizon, probabilities, what would falsify it |
| Personal | I'm not naturally good at maths | Experiences, interpretations, self-assumptions |

A statement can have more than one type.

## Question families

Eight families of tap-to-answer questions. Athena writes topic-specific wording and options within a family.

| Family | General question | Typical options |
| --- | --- | --- |
| Familiarity | How well do you know this topic? | Very well / the basics / heard of it / not at all |
| Definition | What do you mean by this? | A few interpretations / something else |
| Origin | Where does this belief come from? | Experience / evidence / other people / intuition / unsure |
| Reasoning | What most supports your position? | Considerations on more than one side / unsure |
| Certainty | How confident are you in the reasoning? | Slider |
| Scope | How broadly does this apply? | Almost always / often / sometimes / rarely / depends |
| Alternatives | Have you considered another explanation? | Yes / somewhat / no / unsure |
| Reconsideration | What would change your mind? | New evidence / a counterexample / a different view / unsure |

Answer formats: single choice, multiple choice, slider, own words. Design target is roughly three quarters taps and one quarter own words. Treat that as a hypothesis to test.

## Questioning modes

The person can switch mode at any point in a dialogue.

| Mode | What Athena does |
| --- | --- |
| Clarify | Socratic questions that pin down meaning and premises |
| Other side | Presents the strongest opposing considerations |
| Evidence | Asks what would count as evidence and how reliable the sources are |
| Strongest version | Helps the person state the best form of their own view |

## The assumption map

Drawn automatically as the person answers. Every node records where it came from.

| Node type | Meaning |
| --- | --- |
| Main belief | The claim being examined |
| Reason | Why the person holds it |
| Assumption | What must be true for the reasoning to work |
| Evidence | What supports or challenges it |
| Counter | An objection or opposing consideration |
| Open question | Something unresolved |
| Revised | The person's updated wording |

| Source | Meaning | Drawn as |
| --- | --- | --- |
| You said | Typed or spoken by the person | Solid outline |
| You picked | Chosen from options | Solid outline |
| Athena's guess | Inferred by Athena | Dashed outline until confirmed |

Tapping a node opens a bottom sheet: Explore further (opens a dialogue focused on that node), Reword it, Find evidence, Save as its own belief. A guess also shows "Yes, that's mine", Reword and Remove.

Two views: Tree (default on a phone, a vertical list of cards) and Canvas (a zoomable graph).

## The ledger

- Athena creates categories and files each belief. The person can rename a category, merge two, or move a belief. There is no "new category" button.
- A belief can sit in more than one category without being duplicated.
- Entries are beliefs, open questions, or assumptions.
- Each belief keeps its full history: first wording, each dialogue, each revision, and the next revisit date.
- Athena offers a belief again after about a month.

## Memory suggestions

When something the person is doing relates to an earlier belief, Athena shows a small dismissible card: "A connection worth exploring". It quotes the earlier belief and asks whether the person sees a link. It never states that two beliefs contradict each other, and it never blocks the screen.

## User journeys

Daily loop:

1. Open Today and tap today's statement.
2. Set agreement and well-foundedness.
3. Explain the position and choose Quick or Deep.
4. Answer the dialogue questions.
5. Read the recap and confirm, reword or remove Athena's guesses.
6. Look at the map.
7. Re-rate both bars, note what changed, save to the ledger.

Own belief: Today, Write a belief, structured entry, dialogue, map, ledger.

Revisit: Ledger, category, belief, history, Explore it again, new rating stored alongside the old one.

A person can leave at any step and resume later.

## Out of scope for the first release

Public profiles or feeds, leaderboards, any intelligence or critical-thinking score, rewards for changing your mind, automatic evidence search with citations, voice conversation.
