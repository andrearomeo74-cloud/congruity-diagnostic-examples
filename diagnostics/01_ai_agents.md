# 01 — AI Agents: When Autonomy Stops Being Worth It (Congruity Diagnostic)

This is a minimal diagnostic for agentic systems (tool use, MCP/function calling, multi-agent orchestration).
It does not propose a specific architecture.
It defines when scaling remains admissible.

---

## System
An agent (or a network of agents) that:
- uses tools (functions, MCP tools, APIs),
- stores memory (short/long-term),
- runs planning loops,
- potentially coordinates multiple agents.

---

## Diagnostic Lens (Congruity framing)
We treat "capability" as a function of proportionality between:

- **D** = distance to action (latency, hops, tool round-trips)
- **E** = energy/compute per decision (tokens, GPU time, calls)
- **C** = cost per decision (infra + human oversight)
- **V** = value produced (accuracy, throughput, user impact, risk reduction)

A scaling move is **admissible** only if the added autonomy produces **proportionate net value**.

---

## The Key Failure Mode (Silent)
Agent systems often fail silently by:
- increasing orchestration complexity (more agents, more steps),
- increasing tool chatter (more calls, retries),
- increasing memory depth (more retrieval + confusion),
while **marginal value decays**.

The system still "works" — but it becomes:
- slower,
- costlier,
- harder to govern,
- more brittle under distribution shift.

This is an inadmissible move masked as progress.

---

## Minimal Metrics (what to measure)
Track these before/after any scaling change:

1) **ΔV / ΔC**  
Value gained per unit cost increase.

2) **ΔV / ΔE**  
Value gained per unit compute/energy increase.

3) **Decision Latency Budget**
- median and tail latency (P95/P99)
- tool round-trips per answer

4) **Oversight Load**
- human interventions per 100 runs
- rate of "uncertain / needs review"

5) **Failure Tax**
- retries, dead-ends, tool errors
- hallucinated tool parameters
- partial task completion rates

---

## Admissibility Rules (practical)
A move is admissible if ALL hold:

- **Marginal utility stays positive:** ΔV > 0 and not collapsing
- **Cost slope is bounded:** ΔC does not grow faster than ΔV
- **Latency stays within envelope:** tail latency does not explode
- **Oversight does not increase:** humans are not becoming the scheduler
- **Failure tax does not dominate:** retries/errors remain stable or fall

If any of these break, you are scaling *surface capability* while losing *structural legitimacy*.

---

## Concrete Example (numbers)
Baseline (single agent + tools):
- Value score (V): 100 units/day
- Cost (C): $100/day
- Compute/Energy proxy (E): 1,000 "units"
- Tool calls: 6 per task
- P95 latency: 4.0s

Upgrade (multi-agent, deeper memory):
- V: 115 units/day (+15%)
- C: $160/day (+60%)
- E: 1,900 units (+90%)
- Tool calls: 15 per task (+150%)
- P95 latency: 11.0s (+175%)

Diagnosis:
- ΔV/ΔC worsened sharply (15% value for 60% cost)
- tail latency exploded
- tool chatter exploded
This scaling is **inadmissible** even though headline capability improved.

A better move would be:
- reduce tool round-trips,
- compress memory,
- simplify topology,
until ΔV/ΔC and tail latency return inside envelope.

---

## What Congruity Adds Here
Most frameworks help you scale *what agents can do*.
This diagnostic helps you decide *when scaling stops being worth it*.

It makes the execution envelope explicit:
- physical (latency),
- energetic (compute),
- economic (cost),
- governance (oversight).

Congruity does not limit flexibility.
It prevents brittle growth masked as progress.
