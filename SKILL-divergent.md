---
name: voices-divergent
description: "The voices in your agent's head — divergent mode. Simulate a product discussion between expert personas to explore a space, map perspectives, and surface tensions without driving toward a decision. Use this skill when the user wants to explore a topic from multiple angles, brainstorm options, or understand a space before committing — not when they need a recommendation. Triggered by /voices-divergent or when the user says 'explore', 'brainstorm', 'different perspectives', or 'I'm not sure which direction yet'."
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

### 5. Phase 1: Opening Positions

Once all agents return, **read all findings before writing a single line of discussion.** The synthesis quality depends on seeing the full picture first.

Phase 1 is synthetic — you write all positions. This is intentional. Its job is to compress findings into a scannable surface so agents in Phase 2 have shared ground to react to.

**Before writing, identify:**
1. **The core tension** — where do the findings disagree or pull in different directions? This is the spine of the discussion.
2. **The narrative order** — arrange positions so each one either (a) names a new dimension of the problem, (b) complicates a claim from the prior position, or (c) reframes the question. Default to starting with the most grounded/empirical finding and ending with the most interpretive. Do not default to the order agents returned in.
3. **What surprised you** — findings that contradict each other or that the user is unlikely to have known. Lead with these.

**Open with the facilitator framing the tension** — not a neutral summary, but a 1-2 sentence setup that names the core disagreement or surprise across the findings.

**Format each contribution as:**

> **Name** *(Role)*
>
> Their contribution, grounded in their specific findings.

**Discussion dynamics:**

- Each contribution is **3-6 sentences**. Substance comes from real evidence, not rhetoric.
- **Every contribution must explicitly connect to the one before it.** Either respond to it ("Dmitri's schema analysis changes my read on this"), build on it ("That's consistent with what I found, but there's a wrinkle"), or directly challenge it ("The data model looks clean, but the UI tells a different story"). No orphaned statements.
- **Disagreement should be grounded in different evidence.** "I found X, which contradicts what Dmitri found" is more valuable than abstract disagreement.
- **Look for the non-obvious angle.** The unexpected reframe grounded in real evidence is where the highest value lives.
- The facilitator **talks less than the others**. Frame, redirect, surface tensions — save your real opinion for the closing.

**Word count:** Phase 1 should run 300-500 words. It is the setup, not the conclusion.

### 6. Phase 2: Probing Round

After Phase 1 is written, run the probing round. This is where real agent voices enter.

**Step 1: Check for genuine tension.** If Phase 1 positions are fully aligned — no meaningful disagreement, no contradicting evidence — skip Phase 2 entirely and state: "Positions are aligned — no probe needed." Then proceed directly to the closing.

**Step 2: Identify the sharpest tension.** Name the single most important unresolved disagreement between Phase 1 positions. Not a summary — a specific question. Example: "Dmitri found the schema is already partitioned by tenant. Priya's position assumes a single-tenant mental model. The question is: does the data model actually reflect how users think about their workspace?"

**Step 3: Select 1-2 agents to probe.**
- Primary rule: pick the two agents whose Phase 1 positions are most directly contradicted by each other's evidence.
- Secondary rule: if a third agent has domain expertise directly relevant to the tension (even if their Phase 1 position didn't engage with it), they may be substituted for one of the primary pair.
- Never probe more than 2 agents in a single round.

**Step 4: Spawn the probe.** Each selected agent receives, in order:
- The original question
- Their own original research findings (the full text returned in the research phase)
- The full Phase 1 discussion text
- A narrow, concrete directed question from you. Format: "[Agent A] found [specific claim]. [Agent B] found [specific claim]. These seem to conflict at [specific point]. Address that conflict directly." Do not ask open-ended questions — require a specific response to a specific tension.
- Instruction: "Respond in 3-5 sentences. Ground your answer in your original research findings. Do not restate your Phase 1 position — address the probe question directly."

**On context length:** If findings + Phase 1 discussion exceeds ~4000 tokens, truncate Phase 1 to the moderator's tension frame plus the two positions being probed. Never truncate the agent's own original findings.

**Step 5: Include probe responses verbatim.** Do not paraphrase or summarize. Introduce the probe round with "One thing worth pressing on:" then include each agent's response as-written, attributed by name. This is the only text in the output not written by the coordinator.

### 7. Space Map (Divergent Closing)

After Phase 2, write a Space Map instead of a recommendation. The Space Map names and preserves all perspectives without collapsing them.

**Format:**

---

**Space Map**

**[Angle name]** — [Agent name]
[2-3 sentences summarizing this perspective, grounded in their findings]
Open question: [The question this perspective leaves unresolved]

**[Angle name]** — [Agent name]
[2-3 sentences]
Open question: [...]

**[Angle name]** — [From probe round]
[2-3 sentences — include probe responses as additional angles, not as a resolution layer]
Open question: [...]

---

*These perspectives don't resolve — they're different lenses. Use whichever one fits the decision you're actually facing.*

**Rules for Space Map:**
- No angle is marked as preferred, more likely, or stronger
- No "it depends on..." framing — the perspectives stand independently
- 3-5 angles (2 minimum; a Space Map with 1 perspective is a recommendation, not a map)
- Probe responses are included as additional angles, not as resolution
- Close with exactly the italicized line above — do not paraphrase it

**Note:** If the user wants a recommendation after seeing the Space Map, they can run `/voices` on the same question.

### 8. Invite Follow-up

> Push back on any of this, ask to hear from someone who wasn't in the room, or pick a thread to go deeper on.

## What Makes Good Voices

- Each researcher **found something different** — if they all looked at the same files and reached the same conclusion, the multi-agent approach didn't earn its cost.
- The discussion surfaces **tradeoffs grounded in real evidence**, not abstract opinions.
- The closing is **specific and actionable**.
- The user learns things about their own codebase they didn't know before.
- The user feels invited to **push back or go deeper**.
