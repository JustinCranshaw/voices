# Voices

*The voices in your agent's head.*

A skill that spawns expert personas as independent agents — each researches your question through their own lens, explores different parts of your codebase, then comes together to debate what they found and drive toward a recommendation.

## How it works

You're deep in a problem. You've lost altitude. You type `/voices` and describe what you're stuck on.

The skill sharpens your question, selects 2-3 expert agents, and spawns them in parallel. Each agent independently explores your codebase through their own lens — Dmitri reads the schema, Mara examines the UI, Priya checks the git history. They can't see what the others found.

A facilitator then synthesizes their findings into a focused discussion: real disagreements grounded in real evidence from your actual code. It ends with a clear recommendation or a map of the key tension.

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

Not all eight show up. The skill picks the 2-3 whose expertise creates the most productive tension, plus a facilitator to synthesize.

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
3. **The discussion** — 2-3 rounds of debate grounded in real findings (~500-800 words)
4. **The closing** — a recommendation or tension map backed by specific evidence
5. **An invitation** — push back, bring in someone else, or go deeper on a thread

## Architecture

```
voices/
├── SKILL.md              # Coordinator — sharpens question, selects panel, synthesizes
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

The key insight: each agent's "How you think" section isn't flavor text — it's a functional research directive. Dmitri's "start from the data model" means he actually reads the schema first. Priya's "interrogate the framing" means she checks git history and past decisions before forming a position. The diversity of investigation is structural, not simulated.

## Design principles

- **Research independently, discuss together.** Agents explore different slices of the codebase in parallel, then a facilitator synthesizes the tensions.
- **Structural diversity.** Each agent has a different research approach baked into their system prompt. They literally look at different files and notice different things.
- **Grounded in your code.** Opinions are backed by specific file paths and evidence, not abstract positions.
- **Non-obvious angles.** Each agent looks for the perspective you haven't considered — not the standard domain take you already have.
- **Short enough to read.** The research phase does the heavy lifting; the discussion is tight.

## License

MIT
