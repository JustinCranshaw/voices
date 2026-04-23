# Voices

*The voices in your agent's head.*

A skill that spawns expert personas as independent agents — each researches your question through their own lens, explores different parts of your codebase, then speaks one at a time in a live discussion you can steer. Ends with a recommendation (convergent) or a map of the tensions (divergent).

## How it works

You're deep in a problem. You've lost altitude. You type `/voices` and describe what you're stuck on.

The skill sharpens your question, selects 2-3 expert agents, and spawns them in parallel. Each agent independently explores your codebase through their own lens — Dmitri reads the schema, Mara examines the UI, Priya checks the git history. They can't see what the others found.

Then a live discussion unfolds, one voice at a time. A facilitator opens the floor, each researcher speaks in their own voice with their own evidence, the facilitator interjects between them, and then they stop and ask you where to push. You reply, a continuation exchange follows, and the facilitator lands it with a recommendation or a map of the tensions. Every voice in the discussion is a real subagent — not a narrator. Evidence unfolds as the conversation happens.

## The bench

| Agent | Role | Research approach |
|-------|------|-------------------|
| **mara** | Principal Product Designer | Reacts to the UI; finds hierarchy breaks and mental model mismatches |
| **jonas** | Senior Frontend Engineer | Builds the implementation mentally; surfaces where components and CSS break |
| **priya** | Principal User Researcher | Questions the framing; checks how things are actually used |
| **dmitri** | Staff Backend Engineer | Starts from the data model; reads schema, queries, failure modes |
| **eleanor** | Distinguished Engineer | Maps system boundaries; looks upstream, downstream, and at constraints |
| **marcus** | Chief Product Officer | Zooms between strategy and detail; frames clear choices |
| **soleil** | Creative Director | Senses the product's character; examines naming, voice, and perception |
| **kai** | AI Engineer | Decomposes into judgment vs. deterministic; finds the simplest version |

Not all eight show up. The skill picks the 2-3 whose expertise creates the most productive tension, plus a facilitator (marcus or priya) to guide the discussion.

## Usage

```
/voices Should we use SSR or client-side rendering for the dashboard?
```

```
/voices We need to decide on the data model for multi-tenant workspaces.
```

```
/voices Is this the right API surface for the plugin system?
```

## What you get

1. **The setup** — the sharpened question, who's researching, and what each agent will look at
2. **Independent research** — agents explore your codebase in parallel through different lenses
3. **An unfolding discussion** — facilitator opens, each researcher speaks in turn with their own evidence, facilitator threads the tensions between them
4. **A pause for your steer** — the facilitator stops, names the sharpest tension, and asks where you want to push. You reply in natural language
5. **A continuation exchange** — press a researcher, bring in someone new, or reframe the question entirely
6. **The closing** — a recommendation (convergent mode) or a space map of the tensions (divergent mode), grounded in what was actually said
7. **An invitation** — push back, bring in someone else, or go deeper on a thread

## Architecture

```
voices/
├── SKILL.md              # Orchestrator — sharpens question, picks mode, selects panel, sequences spawns
├── agents/
│   ├── mara.md           # Each persona is an independent agent
│   ├── jonas.md           # with its own system prompt,
│   ├── priya.md           # research approach,
│   ├── dmitri.md          # and isolated context.
│   ├── eleanor.md         #
│   ├── marcus.md          # They explore the codebase independently
│   ├── soleil.md          # and can't see what the others found.
│   └── kai.md             #
└── README.md
```

The skill has two closing modes — **convergent** (lands a recommendation) and **divergent** (maps the space without choosing). Both share the same discussion flow; the mode only changes the final spawn. The orchestrator picks the mode based on the user's language, and asks when it's ambiguous.

The key insight: each agent's "How you think" section isn't flavor text — it's a functional research directive. Dmitri's "start from the data model" means he actually reads the schema first. Priya's "interrogate the framing" means she checks git history and past decisions before forming a position. The diversity of investigation is structural, not simulated.

## Design principles

- **Research independently, discuss serially.** Agents explore different slices of the codebase in parallel, then each one speaks into a live discussion one at a time — seeing only the turns before them.
- **The facilitator is a character, not a narrator.** They open the discussion, interject between researcher turns, pause to bring you in, and land the closing. Every one of those turns is a real subagent — never coordinator-synthesized.
- **Evidence unfolds.** No voice summarizes another voice's findings before that voice has spoken them aloud. The reader discovers the argument the same way the panel does.
- **You steer the middle.** After the opening round, the facilitator stops and asks you where to push. That's not a courtesy — it's the structural point of the skill.
- **Structural diversity.** Each agent has a different research approach baked into their system prompt. They literally look at different files and notice different things.
- **Grounded in your code.** Opinions are backed by specific file paths and evidence, not abstract positions.

## License

MIT
