# Voices

*The voices in your agent's head.*

A Claude Code skill that splits your agent into a panel of opinionated experts who debate your question, disagree productively, and land on a recommendation — or tell you exactly why they can't agree and what the deciding factor is.

## What it does

You're deep in a problem. You've lost altitude. You type `/voices` and describe what you're stuck on.

The skill reads your session context and codebase, selects 3-4 experts from a bench of eight, and runs a focused discussion. They argue. They push back on each other. They surface the angle you stopped seeing three hours ago. A facilitator drives the conversation and closes with a clear recommendation or a map of the key tension.

The whole thing is short enough to read in one sitting.

## The bench

| Name | Role | Domain |
|------|------|--------|
| **Mara** | Principal Product Designer | UI/UX, design systems, visual hierarchy, accessibility |
| **Jonas** | Senior Frontend Engineer | React, CSS architecture, component systems, animation |
| **Priya** | Principal User Researcher | User needs, problem reframing, usability, research methods |
| **Dmitri** | Staff Backend Engineer | APIs, data modeling, performance, failure modes |
| **Eleanor** | Distinguished Engineer | System design, architecture tradeoffs, scalability |
| **Marcus** | Chief Product Officer | Product strategy, prioritization, market positioning |
| **Soleil** | Creative Director | Brand, messaging, naming, creative direction |
| **Kai** | AI Engineer | LLM integration, agent design, prompt engineering, evals |

Not all eight show up. The skill picks the 3-4 whose expertise creates the most productive tension for your specific question.

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

Or just describe what you're stuck on — the skill sharpens the question for you.

## What you get

1. **The setup** — the sharpened question and who's in the room (and why), so you can correct course before the discussion starts
2. **The discussion** — 2-3 rounds of real back-and-forth, ~400-600 words, dense with insight
3. **The closing** — either a clear recommendation or a tension map naming exactly what the tradeoff is and what it depends on
4. **An invitation** — push back, bring in someone who wasn't in the room, or go deeper on a thread

## Design principles

- **Productive friction over false consensus.** The personas disagree. That's the point.
- **Non-obvious angles.** Each voice looks for the perspective you haven't considered, not the standard domain take you already have.
- **Grounded in your code.** The discussion references your actual codebase and session context, not hypotheticals.
- **Short enough to read.** Dense contributions, tight pacing, no throat-clearing.
- **Distinct reasoning, not just distinct vocabulary.** Each persona thinks differently — Mara reacts viscerally then rationalizes, Priya interrogates the framing before taking a position, Dmitri starts from the data model and works outward.

## Installation

Add the `voices/` directory to your Claude Code skills path.

## License

MIT
