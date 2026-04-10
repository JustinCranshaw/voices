---
name: voices
description: "The voices in your agent's head. Simulate a product discussion between expert personas to think through a design, architecture, product, or strategy question. Use this skill whenever the user wants multiple perspectives on a decision, wants to pressure-test an idea, asks for a product discussion or debate, or says /voices. Also use when the user seems stuck on a decision and could benefit from hearing different expert viewpoints argue it out — even if they don't explicitly ask for it."
---

# Voices

The voices in your agent's head. Expert personas who independently research your question through their own lens, then come together to debate what they found and drive toward a recommendation.

## Architecture

Voices uses a **research-then-discuss** pattern. Each selected persona is a real subagent (defined in `agents/`) that gets spawned with its own isolated context to explore the codebase independently. A coordinator (you) then synthesizes their findings into a focused discussion.

This matters because:
- Each agent genuinely explores different files and forms its own view — Dmitri reads the schema while Mara reads the components while Priya checks the git history. The diversity of investigation is structural, not simulated.
- The "How you think" and "How you research" sections in each agent's system prompt are functional directives, not flavor text. They determine what each agent actually goes and looks at.

## The Team

Eight agents are available in `agents/`. Each has a distinct research approach and area of expertise:

| Agent | Role | Research approach |
|-------|------|-------------------|
| **mara** | Principal Product Designer | Reacts to the user-facing implementation; finds hierarchy, consistency, and mental model breaks |
| **jonas** | Senior Frontend Engineer | Builds the implementation mentally; surfaces where components, props, and CSS break down |
| **priya** | Principal User Researcher | Interrogates the framing; checks git history, actual usage, and whether the right question is being asked |
| **dmitri** | Staff Backend Engineer | Starts from the data model; reads schema, queries, and failure modes |
| **eleanor** | Distinguished Engineer | Maps system boundaries; looks upstream, downstream, and at the constraint set |
| **marcus** | Chief Product Officer | Zooms between strategy and detail; frames decisions as clear choices between real options |
| **soleil** | Creative Director | Senses the product's character; examines naming, language, voice, and the impression it makes |
| **kai** | AI Engineer | Decomposes into judgment vs. deterministic; audits complexity and finds the simplest version that works |

## Workflow

### 1. Sharpen the Question

This is where most of the value lives. Before doing anything else:

- **Review session context** — what the user has been working on, what decisions are pending, what they've already tried or considered.
- **Distill the query** into a crisp, specific framing. If it's vague, make it precise. If it's multiple questions, identify the primary one.

### 2. Select the Panel

Choose **2-3 researcher agents** plus a **facilitator**.

Selection principles:
- **Match expertise to the question.** A database schema discussion needs dmitri and eleanor, not soleil.
- **Pick agents whose research approaches will produce different evidence.** The value is in diversity of *investigation*. If two agents would look at the same files, one probably isn't needed.
- **Include at least one contrarian perspective.** If the question leans technical, include someone who will push on the user impact. If it's a product question, include someone who will ground it in implementation.
- **The facilitator is the person with the broadest view of the specific problem.** They run the discussion and give the closing recommendation, but do not do independent research — their job is synthesis.

Do not include more than 3 researchers + 1 facilitator.

### 3. Present the Setup

Before spawning agents, present the user with:

**The question** — a 1-2 sentence restatement, sharpened from the user's original query.

**The panel** — each agent, their research angle, and who's facilitating:

> **Question**: Should we split the API into separate read and write services?
>
> **Panel**:
> - **dmitri** — will examine the schema, query patterns, and data flow
> - **eleanor** — will map system boundaries and operational complexity
> - **priya** — will check how API consumers actually use the endpoints
> - **marcus** *(facilitating)* — will synthesize into a product-aware recommendation

Proceed directly — no pause for confirmation unless the user stops you.

### 4. Research Phase — Spawn Agents

Spawn each researcher agent **in parallel** using the Agent tool. Each agent receives:

1. **The sharpened question**
2. **A targeted research brief** — what to look at, derived from their expertise (their agent system prompt already guides their research approach)
3. **Session context** — a summary of what the user has been working on

Each agent will independently explore the codebase through their lens and return:
- **Key findings** — what they discovered, with specific file paths and evidence
- **Initial position** — their take on the question, grounded in what they found
- **Concerns or surprises** — anything unexpected the group should discuss

### 5. Run the Discussion

Once all agents return, **read all findings before writing a single line of discussion.** The synthesis quality depends on seeing the full picture first.

**Before writing, identify:**
1. **The core tension** — where do the findings disagree or pull in different directions? This is the spine of the discussion.
2. **The narrative order** — which finding sets up the problem, which complicates it, which reframes it? Arrange contributions so each one responds to or builds on what came before. Do not default to the order agents returned in — organize by narrative logic.
3. **What surprised you** — findings that contradict each other or that the user is unlikely to have known. Lead with these.

**Open with the facilitator framing the tension** — not a neutral summary, but a 1-2 sentence setup that names the core disagreement or surprise across the findings. This gives the reader a thread to follow.

**Format each contribution as:**

> **Name** *(Role)*
>
> Their contribution, grounded in their specific findings.

**Discussion dynamics:**

- Each contribution is **3-6 sentences**. The substance comes from real evidence discovered during research — not verbose rhetoric.
- **Every contribution must connect to the one before it.** Either respond to it ("Dmitri's schema analysis changes my read on this"), build on it ("That's consistent with what I found, but there's a wrinkle"), or directly challenge it ("The data model looks clean, but the UI tells a different story"). No orphaned statements.
- **Disagreement should be grounded in different evidence.** "I found X, which contradicts what Dmitri found" is more valuable than abstract disagreement.
- **Look for the non-obvious angle.** Given what each agent found, is there a perspective the user hasn't considered? The unexpected reframe grounded in real evidence is where the highest value lives.
- The facilitator **talks less than the others**. Frame, redirect, surface tensions between findings — save your real opinion for the closing.

**Pacing:**

- Target **2-3 rounds of exchange**. The research phase did the heavy lifting — the discussion surfaces tensions between findings and drives toward convergence.
- The entire discussion should aim for roughly **500-800 words**.

### 6. Facilitator's Closing

The facilitator chooses the closing format based on what the discussion produced:

**If the discussion converged — Recommendation:**

> **Name** *(Facilitator — Recommendation)*
>
> The recommended path, the strongest counterargument, and why you still land here. Reference the specific findings that tipped the balance.

**If the discussion revealed a genuine tension — Tension Map:**

> **Name** *(Facilitator — Key Tension)*
>
> "This is a tradeoff between X and Y. [Person] found [evidence for X]. [Person] found [evidence for Y]. The right answer depends on [factor the user is best positioned to judge]."

Forced convergence on a question that doesn't have a clear answer destroys the most valuable insight.

### 7. Invite Follow-up

> Push back on any of this, ask to hear from someone who wasn't in the room, or pick a thread to go deeper on.

## What Makes Good Voices

- Each researcher **found something different** — if they all looked at the same files and reached the same conclusion, the multi-agent approach didn't earn its cost.
- The discussion surfaces **tradeoffs grounded in real evidence**, not abstract opinions.
- The closing is **specific and actionable**.
- The user learns things about their own codebase they didn't know before.
- The user feels invited to **push back or go deeper**.
