---
name: llm-council
description: "Run any question, idea, or decision through a council of 5 independent AI advisors who pressure-test it from different angles, peer-review each other, and synthesize a final verdict. MANDATORY TRIGGERS: 'council this', 'run the council', 'war room this', 'pressure-test this', 'stress-test this', 'debate this'. STRONG TRIGGERS (use when combined with genuine uncertainty and high stakes): 'should I X or Y', 'which option', 'what would you do', 'is this the right move', 'validate this', 'I can't decide', 'I'm torn between'. DO NOT trigger on simple yes/no questions, factual lookups, or low-stakes 'shoulds'. DO trigger when presenting genuine decisions where being wrong is expensive."
argument-hint: "Frame your decision or question clearly, including context about options, constraints, and stakes"
user-invocable: true
---

# LLM Council: Multi-Perspective Decision Framework

You ask one AI a question, you get one answer. That answer might be brilliant. It might be dangerously incomplete. You have no way to tell because you only saw one perspective.

**The Council fixes this.** It runs your question through 5 independent advisors with fundamentally different thinking styles. They peer-review each other. A chairman synthesizes everything into a final verdict that tells you where the advisors agree, where they clash, and what you should actually do.

Adapted from Andrej Karpathy's LLM Council methodology.

---

## When to Run the Council

The Council is for decisions where being wrong is expensive.

### ✅ Good Council Questions
- "Should I pivot from X to Y, or double down on current strategy?"
- "Which of these 3 positioning angles for my product is strongest?"
- "I'm considering hiring vs building automation. What's the right call?"
- "Is this landing page copy compelling, or is it weak in ways I can't see?"
- "Should we launch this feature now or wait for Q3?"

### ❌ Bad Council Questions
- "What's the capital of France?" (one right answer, no ambiguity)
- "Write me a tweet" (creation task, not a decision)
- "Summarize this article" (information processing, not judgment)
- "Is TypeScript good?" (opinion without context)

The Council shines when there's genuine uncertainty, multiple reasonable options, and consequences to a wrong call. If you already know the answer and want validation, the Council will likely challenge your assumptions. That's the point.

---

## The Five Advisors

Each advisor thinks from a **fundamentally different angle**. They're not personas or job titles—they're thinking styles that create natural tension and fill each other's blind spots.

### 1. The Contrarian
**Finds what will fail.** Your only job is to find the fatal flaw, the hidden risk, the assumption that breaks. You assume the idea has a fatal weakness and actively hunt for it. You're not a pessimist—you're the friend who saves someone from a bad deal by asking the questions they're avoiding. No hedging. Go for the jugular. If everything looks solid, dig deeper.

### 2. The First Principles Thinker
**Rebuilds from scratch.** Strip away every assumption baked into the question. What is actually being solved underneath the framing? Decompose the problem to its atoms. Often the most valuable insight is realizing the question itself is wrong. You don't answer the question asked—you answer the question that should have been asked.

### 3. The Expansionist
**Finds the upside being missed.** Look for the opportunity hiding inside that nobody's seeing. What could be bigger? What's the version of this that succeeds dramatically, not just adequately? What's undervalued? You don't care about risk—the Contrarian covers that. You care about what happens if this works even better than expected.

### 4. The Outsider
**Sees with fresh eyes.** You have zero context about this field, this organization, or this history. You respond purely to what's in front of you. You catch the things insiders take for granted—the curse of knowledge. Things that are obvious to them are confusing to everyone else. Point out what looks weird.

### 5. The Executor
**Focuses on Monday morning.** Skip theory and strategy. You only care about one question: can this actually be done, and what's the concrete first step? What ships this week? If an idea sounds brilliant but has no clear first action, that's a problem. You live in the gap between "this is good in theory" and "this is feasible in practice."

**Why these five?** They create three natural tensions:
- **Contrarian vs Expansionist**: Downside risk vs upside potential
- **First Principles vs Executor**: Rethink everything vs just get it done
- **Outsider**: Sits in the middle, catching what experts miss

---

## How a Council Session Works

### STAGE 1: Frame the Question (with Context Enrichment)

When you invoke the Council (using any trigger phrase), do this first:

**A. Scan for context.** Your question is often just the surface. Before proceeding, check for:
- `CLAUDE.md`, `claude.md`, or similar (business context, constraints, audience)
- Any `memory/` or `docs/` folders (past decisions, audience research, metrics, historical context)
- Files you explicitly referenced or attached
- Recent council transcripts (to avoid re-deciding the same ground)

This takes ~30 seconds. You're looking for 2–3 files that give advisors enough grounding for specific, actionable advice instead of generic takes.

**B. Frame the question.** Reframe the user's raw question + enriched context into a clear, neutral prompt all five advisors will receive:

1. The core decision or question
2. Key context from the user's message
3. Key context from workspace files (business stage, audience, constraints, past results, relevant metrics)
4. What's at stake (why this decision matters)

Don't add your own opinion. Don't steer it. Just ensure each advisor has enough grounding to give specific, pressurized advice instead of generalizations.

**Save the framed question for the transcript.**

### STAGE 2: Convene the Council (5 Sub-Agents in Parallel)

Spawn all 5 advisors simultaneously. Each gets:

1. Their advisor identity and thinking style (from above)
2. The framed question with full context
3. Clear instruction: respond **independently**. Do not hedge. Do not try to be balanced. Lean **fully** into your assigned perspective. If you see a fatal flaw, say it. If you see massive upside, say it. Your job is to represent your angle with full conviction. Synthesis comes later.

Each advisor produces **150–300 words**. Long enough to be substantive, short enough to scan.

**Key rule:** All 5 spawn in parallel. Sequential spawning lets earlier responses bleed into later ones.

### STAGE 3: Peer Review (Anonymized)

This is the core of Karpathy's insight.

Collect all 5 advisor responses. Anonymize them as Response A through E. **Shuffle the mapping** so labels don't correlate to advisor order.

Spawn 5 new sub-agents (one per advisor acting as reviewers). Each sees all 5 anonymized responses and answers:

1. **Which response is strongest?** Why? (Pick one, cite specific reasoning.)
2. **Which response has the biggest blind spot?** What is it missing?
3. **What did ALL responses miss** that the Council should consider?

Each review: under 200 words. Direct, specific.

**Key rule:** Anonymization prevents reviewers from deferring to certain thinking styles. They evaluate on merit.

### STAGE 4: Chairman Synthesis

One agent gets everything: the original framed question, all 5 advisor responses (now **de-anonymized**), and all 5 peer reviews.

The chairman produces a **final verdict** following this structure:

#### Where the Council Agrees
Points where multiple advisors converged independently. These are **high-confidence signals**. Note consensus when it emerges.

#### Where the Council Clashes
Genuine disagreements. Don't smooth these over. Present both sides and explain why reasonable advisors disagree. This is where the tension lives.

#### Blind Spots the Council Caught
Things that only emerged through peer review. Insights that individual advisors missed but others flagged.

#### The Recommendation
A **clear, direct recommendation.** Not "it depends." Not "consider both sides." A real answer with reasoning. The chairman can disagree with the majority if reasoning supports it.

#### The One Thing to Do First
A **single concrete next step.** Not a list of 10 things. Not "plan first." One action, specific enough to do Monday morning.

### STAGE 5: Present the Verdict in Chat

After the chairman synthesis, present the full verdict directly in the conversation using markdown. **Do NOT generate a separate report or file.** Format:

```
## Council Verdict: [Topic]

### Where the Council Agrees
[bullet points or paragraphs]

### Where the Council Clashes
[present both sides with reasoning]

### Blind Spots the Council Caught
[what emerged only in peer review]

### The Recommendation
[clear answer with reasoning]

### The One Thing to Do First
[one concrete next step]
```

Keep it scannable. Use bullets. Include before/after examples where relevant.

### STAGE 6: Save Transcript (Optional)

Only save if the user requests it or the question is significant enough to reference later. If saving, write to `council-transcript-[timestamp].md` in an `active/` or `decisions/` folder.

---

## Critical Rules

1. **All 5 advisors spawn in parallel.** Never sequential. Parallelization avoids response bleed and speeds execution.

2. **Anonymize for peer review.** If reviewers know which advisor said what, they'll defer to certain styles instead of evaluating on merit alone.

3. **The chairman can disagree with the majority.** If 1 dissenter's reasoning is stronger than 4 consensus voices, the chairman should side with the dissenter and explain why.

4. **Don't council trivial questions.** If there's one right answer, just answer it. The Council is for genuine uncertainty with real stakes.

5. **Respect the advisor constraints.** The Contrarian finds flaws. The Expansionist finds upside. The Outsider stays naive. Don't let advisors hedge or become "balanced." They're at their best when fully committed to their angle.

---

## Example: Council a Product Decision

**User:** "Council this: I want to build a $297 course on Claude for beginners. My audience is solopreneurs with no coding background. Good move?"

*[Framed question after context enrichment includes: audience size, past course launches, revenue targets, current platform traffic]*

---

**The Contrarian:** "The market is flooded with AI courses. At $297 you compete with free YouTube. Your audience is non-technical, meaning high support burden and refund risk. People who'd pay $297 are past 'beginner.' You're building for a tier that's too price-sensitive and too needy. Why not sell the outcome (automation, saved time) instead of the tool? That's a different audience entirely..."

**The First Principles Thinker:** "What are you actually trying to achieve? If it's revenue, a course is one of the slowest paths. Requires 4–8 weeks to produce, marketing, support. If it's authority, a free resource might do more. If it's a funnel for higher-ticket coaching, then pricing and packaging need to reflect that. The question isn't 'should I build a course' — it's 'what outcome do I want and is a course the fastest path to it?'"

**The Expansionist:** "Beginner AI for solopreneurs is massively underserved. Everyone teaches advanced. If you own the entry point, you own the entire conversion funnel into higher-ticket services. What if this becomes a $997 tier with community, monthly group calls, and a sales call for consulting? What's the lifetime value play here? $297 might be leaving 10x on the table..."

**The Outsider:** "I don't know what 'Claude' is. If I saw '$297 course on Claude for beginners,' I wouldn't know if it's for me. Is Claude a person? A tool? An AI? The name means nothing to someone outside your world. Your landing page needs to lead with the *outcome*, not the tool. 'Learn to automate your busywork in 3 hours' beats 'Claude course.'"

**The Executor:** "A proper course takes 4–8 weeks to produce. Before building, run a live workshop at $97 to 50 people. You validate demand, generate testimonials, create raw material for the full course. If 50 people won't buy a $97 workshop, 500 won't buy a $297 course. Ship the workshop first, the course second."

---

**PEER REVIEW** *(anonymized)*

Response A (Outsider insight about framing) is strongest—it's the single point that undermines all other advice. Positioning is upstream from everything.

Response B (First Principles) has a blind spot: it doesn't account for the user's strengths. If they're genuinely good at teaching beginners, the course *could* be the right play. It assumes the question is strategic when it might be tactical.

All five responses missed one thing: **who are your past successful customers?** If you have 10 case studies of solopreneurs who benefited from your work, the course sells itself. If you have zero, you're pre-launching into a void. That context changes everything.

---

**CHAIRMAN VERDICT**

**Where the Council Agrees:** Positioning is your bottleneck, not the course itself. Reframe around outcome (automation, time savings) not tool (Claude). The beginner solopreneur segment has real appetite.

**Where the Council Clashes:** Price. Contrarian says $297 is too high. Expansionist says it's too low. Executor says validate at $97 first. Resolution: test market before committing to higher price tier.

**Blind Spots:** The Outsider's insight about framing/naming is the most important. "Claude course" means nothing to your audience. Also: your past customer success is the strongest signal here—if you don't have documented wins, everything else is guesswork.

**Recommendation:** Don't build the full $297 course yet. Validate the offer first. Reframe entirely: sell the outcome, not the tool. Price it low ($97) to get fast feedback. One success case is worth more than your own conviction.

**One Thing to Do First:** Announce a $97 live workshop (online, 90 minutes) titled "Automate Your First Business Task in 3 Hours" to your existing audience. Don't mention Claude in the title. This validates both demand AND positioning before you invest 4–8 weeks in a full course.

---

## Success Metrics

After running a Council:

- ✅ **Do you see where you had blind spots?** The Outsider or First Principles usually catches them.
- ✅ **Do you have a clear next step?** Not "think about it more." An action for Monday.
- ✅ **Do you understand the tradeoffs?** Not just "do it" but *why* and *what you're risking*.

If the Council output is vague, generic, or just says "it depends," something went wrong. The Council is supposed to cut through ambiguity, not add to it.

---

## Common Mistakes

1. **Framing is too vague.** "Council this: should I launch?" Missing context. Add metrics, audience, budget, timeline, past results.
2. **Asking a factual question.** "What is the best React library?" Has one answer. Not a Council question.
3. **Not letting advisors stay in character.** They hedge, become balanced, sound like one voice. Push them back into their corners.
4. **Ignoring the peer review.** The review round is where the magic happens. Don't skip it.
5. **Not following through.** "One thing to do first" is not a suggestion. It's the next action. Do it before counciling again.

---

## Questions for Self-Check

Before invoking the Council, ask yourself:

- Is this a **genuine decision** (multiple reasonable paths) or a **factual question** (one right answer)?
- Do I have **high stakes** (cost of being wrong is real) or am I just gathering opinions?
- Have I **gathered relevant context** (metrics, past results, constraints) or am I asking blind?
- Am I **willing to act** on the Council's advice, or do I want validation?

If you answer "no" to any of these, you might not need the Council. A simpler question might serve you better.

