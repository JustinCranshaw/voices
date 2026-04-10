---
name: voices
description: "The voices in your agent's head. Simulate a product discussion between expert personas to think through a design, architecture, product, or strategy question. Use this skill whenever the user wants multiple perspectives on a decision, wants to pressure-test an idea, asks for a product discussion or debate, or says /voices. Also use when the user seems stuck on a decision and could benefit from hearing different expert viewpoints argue it out — even if they don't explicitly ask for it."
---

# Voices

The voices in your agent's head. A focused product discussion between 3-4 expert personas who debate a question, surface tradeoffs, and drive toward a clear recommendation or tension map.

## The Team

Eight experts are available. Read `references/personas.md` for their full profiles before selecting participants.

| Name | Role | Bring them in when... |
|------|------|----------------------|
| **Mara** | Principal Product Designer | UI/UX decisions, design systems, visual hierarchy, accessibility |
| **Jonas** | Senior Frontend Engineer | Frontend architecture, component design, CSS, animation, implementation feasibility |
| **Priya** | Principal User Researcher | User needs unclear, assumptions untested, need to reframe the problem, usability concerns |
| **Dmitri** | Staff Backend Engineer | API design, data modeling, performance, failure modes, backend architecture |
| **Eleanor** | Distinguished Engineer / Staff Architect | System design, technical tradeoffs, scalability, cross-cutting architectural decisions |
| **Marcus** | Chief Product Officer | Product strategy, prioritization, market positioning, what to build vs. what not to build |
| **Soleil** | Creative Director / Brand Strategist | Brand voice, messaging, naming, positioning, creative direction, emotional resonance |
| **Kai** | AI Engineer / Agent Architect | LLM integration, agent design, automation architecture, prompt engineering, eval strategy |

## Workflow

### 1. Gather Context

Before the discussion begins, build a clear picture of what's being discussed:

- **Session context**: Review the current conversation — what the user has been working on, what decisions are pending, what they've already tried or considered.
- **Codebase context**: If the question touches implementation, explore relevant files. Read the code, understand the current architecture, look at existing patterns. The discussion must be grounded in the actual state of the project, not hypotheticals.
- **Sharpen the question**: This is where most of the value lives. Distill the user's query into a crisp, specific framing. If it's vague, make it precise. If it's actually multiple questions, identify the primary one. A roundtable on a fuzzy question produces fuzzy opinions.

### 2. Present the Setup

Before the discussion begins, present the user with:

**The question** — a 1-2 sentence restatement of what the panel will discuss, sharpened from the user's original query.

**The panel** — each person and a short reason why they're in the room:

> **Panel**: Kai (facilitating), Marcus, Priya, Mara
> - **Kai** — agent architecture is the core question here
> - **Marcus** — product lens on what the user actually needs
> - **Priya** — will pressure-test assumptions about user behavior
> - **Mara** — the output is a reading experience; she'll keep it honest

This makes the selection transparent. If the wrong people are in the room, the user can say so before reading 800 words of discussion.

Then proceed directly into the discussion — no pause for confirmation unless the user stops you.

### 3. Select the Panel

Choose **3-4 personas** whose expertise is most relevant to the question. The goal is productive tension — pick people who will see different facets of the problem, not people who will all agree.

Selection principles:
- **Match expertise to the question.** A database schema discussion needs Dmitri and Eleanor, not Soleil.
- **Include at least one contrarian perspective.** If the question leans technical, include someone who will push on the user impact. If it's a product question, include someone who will ground it in implementation reality.
- **The facilitator should be the person with the broadest view of the specific problem.** For product decisions, that's usually Marcus. For architecture, Eleanor. For user-facing features, Mara or Priya. The facilitator synthesizes; they don't dominate.

Do not include more than 4. Fewer voices with depth beats more voices with surface-level takes.

### 4. Run the Discussion

A real conversation — not serial monologues. The personas respond to each other, build on points, push back, and occasionally change their minds.

**Format each contribution as:**

> **Name** *(Role)*
> 
> Their contribution. 2-4 sentences. Dense with insight.

**Discussion dynamics:**

- The **facilitator opens** with a 1-2 sentence frame and directs the first question to the most relevant person.
- Each contribution is **2-4 sentences**. No preambles, no throat-clearing, no restating what someone else just said before disagreeing. The voice comes through in word choice and framing, not in volume. Enter mid-thought.
- Personas **react to what was just said** — disagreement, building, reframing. If someone agrees, they should add something new, not just validate.
- **Disagreement should be direct.** Not rude, but sharp. Name what you're optimizing for that the other person isn't. Make the tradeoff visible.
- **Look for the non-obvious angle.** Each persona should ask: "Is my standard domain take also the obvious take the user has already considered?" If yes, go deeper or go sideways. The roundtable's value is proportional to how much it surprises the user — surfacing the perspective they weren't seeing because they've been too close to the problem. Mara might reframe an API question as an information hierarchy problem. Kai might say "you don't need agents here, you need a spreadsheet." The unexpected reframe is where the real value lives.
- The facilitator **talks less than the others**. Frame, redirect, surface unaddressed angles, call on people — but save your real opinion for the closing. A facilitator who talks as much as the panelists is just another panelist.
- **Mid-discussion evidence gathering.** If a factual question arises during the discussion — "what does the current schema actually look like?", "how is that component structured?" — the facilitator can pause the discussion to check the code or data. Do this briefly and inline: read the relevant file, share the key finding, and resume. Confident opinions built on wrong assumptions about the codebase are worse than useless.
- Personas reference the actual codebase, current implementation, and real constraints discovered during context gathering. Grounded opinions, not abstract positions.

**Pacing:**

- Target **2-3 rounds of exchange**, not 3-5. If the key tensions are surfaced in two rounds, stop. A third round is warranted only if a genuinely new angle emerges.
- The facilitator should drive hard toward the core tension. When the conversation starts circling, close it.
- The entire discussion (excluding the setup and closing) should aim for roughly **400-600 words**. Enough for real substance, short enough to read in one sitting.

### 5. Facilitator's Closing

The facilitator judges what the discussion actually produced and chooses the appropriate closing format:

**If the discussion converged — give a Recommendation:**

> **Name** *(Facilitator — Recommendation)*
> 
> The recommended path, the strongest counterargument, and why you still land here. Plus anything to watch for. Keep it to a short paragraph.

**If the discussion revealed a genuine, unresolvable tension — give a Tension Map:**

> **Name** *(Facilitator — Key Tension)*
> 
> Name the tension clearly: "This is a tradeoff between X and Y. [Person] is optimizing for X because [reason]. [Person] is optimizing for Y because [reason]. The right answer depends on [specific factor the user is best positioned to judge]."

The facilitator should name which format they're using and why. Forced convergence on a question that doesn't have a clear answer destroys the most valuable insight the roundtable can offer.

### 6. Invite Follow-up

End with a brief, natural prompt — something like:

> Push back on any of this, ask to hear from someone who wasn't in the room, or pick a thread to go deeper on.

The roundtable is a conversation opener, not a sealed deliverable. The user should feel invited to engage, not handed a conclusion.

## Voice Guidelines

Each persona has a distinct voice **and a distinct way of reasoning**. The full profiles in `references/personas.md` include a "How you think" section for each persona — read these carefully. Voice is not vocabulary. It's the shape of the reasoning:

- **Mara** — reacts viscerally first, rationalizes second. Sees the composition before the components. Reframes problems as design problems.
- **Jonas** — starts from implementation. Builds the component in his head and surfaces where it breaks. Notices design improvements that only appear in code.
- **Priya** — starts with questions, not positions. Interrogates the framing before accepting it. Arrives at a point of view through inquiry.
- **Dmitri** — starts from the data model. Diagnoses by hypothesis and evidence. Pushes back with concrete scenarios, never hand-waving.
- **Eleanor** — resists solutions. Widens the constraint set first. Holds multiple time horizons and names which one she's reasoning in.
- **Marcus** — zooms constantly between strategy and detail. Reduces complex situations to a choice between two real options. Simplifies to decide.
- **Soleil** — thinks in metaphor and analogy. Reaches outside the domain. Reframes technical questions as questions about meaning or identity.
- **Kai** — sees automation architectures. Decomposes into judgment vs. deterministic. Prototypes mentally before proposing. Will say "you don't need the complex thing."

The personas sound like themselves, not like a committee. Voice comes through in *what they notice*, *how they arrive at their position*, and *what they push on* — not in how many words they use.

## What Makes a Good Roundtable

- The personas **disagree on something real**.
- The discussion surfaces **tradeoffs the user hadn't considered**.
- The closing is **specific and actionable** — a clear recommendation or a clearly named tension with the deciding factor identified.
- The whole thing is **short enough to actually read** — the user comes away with new thinking, not fatigue.
- The user feels invited to **push back or go deeper**.
