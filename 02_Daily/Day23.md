# DAY 23 — MODULE
Analyze and debug your generative AI app with tracing

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/analyze-debug-generative-ai-app/

## TASKS
- [ ] Learn tracing
- [ ] Understand debugging flows

## KEY CONCEPTS
- OpenTelemetry
- Execution tracing

## NOTES
# 🔷 Module Overview

* Intermediate module (\~1 hr, 9 units)
* Focus: **Tracing generative AI apps using Microsoft Foundry + OpenTelemetry**
* Goal: Capture execution flows, debug behavior, and optimize performance/reliability [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/tracing-generative-ai-app/)

### Key learning objectives

* Set up tracing infrastructure (Foundry + Application Insights)
* Implement spans for model/tool/business logic
* Analyze traces to find **bottlenecks + failure patterns** [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/tracing-generative-ai-app/)

***

# 🔷 1) What tracing actually is (core concept)

### Definition

Tracing = **end-to-end visibility into how a request flows through your AI system**

It shows:

* Inputs
* Model calls
* Tool usage
* Intermediate steps
* Outputs

### From observability docs

Tracing captures the **execution flow** across:

* LLM calls
* tool invocations
* agent decisions    [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

***

# 🔷 2) Why tracing matters (VERY IMPORTANT)

GenAI apps are complex because:

* Multi-step reasoning
* Dynamic behavior (varies by input)
* Tool chaining/nesting
* Large context/payloads

### Problem

Without tracing:

> You only see the final output (black box)

### Solution

Tracing:

* Shows **step-by-step execution**
* Lets you identify exactly:
  * where latency occurs
  * where failures happen
  * why outputs are incorrect

### Foundry doc insight

Tracing helps you:

* identify root cause of issues
* understand reasoning flow
* debug complex agent behavior [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/concepts/trace-agent-concept)

***

# 🔷 3) Core tracing concepts (CRITICAL for AI‑300)

## 3.1 Trace

* A **full lifecycle of a request**
* Represents the **entire workflow**

Example:

```
User → Agent → Tool → API → Response
```

***

## 3.2 Span

* A **single operation inside a trace**
* Represents one step in execution

Examples:

* model call
* tool invocation
* function execution

### Important

Spans can be:

* nested
* hierarchical

 [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry-classic/how-to/develop/trace-agents-sdk)

***

## 3.3 Attributes (metadata)

Each span can include:

* duration
* inputs/outputs
* tokens
* model used
* errors

***

## 3.4 Trace hierarchy (mental model)

```
Trace
 ├── Span (Agent)
 │    ├── Span (LLM Call)
 │    ├── Span (Tool Call)
 │    │     └── Span (API Call)
 │    └── Span (Response processing)
```

👉 This is how you debug nested workflows

***

# 🔷 4) How tracing works in Microsoft Foundry

## Architecture

* Traces stored in **Azure Monitor Application Insights**
* Uses **OpenTelemetry (OTel)** standard    [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/trace-agent-setup), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/concepts/trace-agent-concept)

### What gets captured

* inputs/outputs
* prompts
* tool calls
* latency
* retries
* execution steps

 [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/concepts/trace-agent-concept)

***

## Key integration step

You must:

* connect Foundry → Application Insights    [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/trace-agent-setup)

***

## Types of tracing

### 1) Server-side tracing (default)

* enabled automatically once connected
* no code changes required

### 2) Client-side tracing

* track your **own application logic**
* requires instrumentation

 [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/trace-agent-setup)

***

# 🔷 5) Instrumentation (how traces are created)

## Two common approaches

### ✅ Automatic tracing

* minimal setup
* captures framework/model interactions

### ✅ Manual tracing

* define custom spans
* track:
  * business logic
  * custom workflows

***

## Key idea

You don’t just trace “the model”
You trace:

* the **entire pipeline**

***

# 🔷 6) What tracing reveals (practical insights)

Tracing helps you answer:

* Why was the response wrong?
* Which tool failed?
* Where is latency coming from?
* Why did token cost spike?
* Why did the agent choose a tool?

***

# 🔷 7) Analyzing traces

### From module + docs:

You use trace data to:

* identify **performance bottlenecks**
* detect **failure patterns**
* understand **control flow** [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/tracing-generative-ai-app/)

***

## What to look for

| Issue         | Trace Signal          |
| ------------- | --------------------- |
| Slow response | long span duration    |
| Bad output    | wrong tool/model span |
| High cost     | large token spans     |
| Failure       | error events in spans |

***

# 🔷 8) Tracing vs Monitoring vs Evaluation

SUPER important relationship (shows up everywhere):

| Capability | What it answers            |
| ---------- | -------------------------- |
| Monitoring | “Is it fast/cheap/stable?” |
| Evaluation | “Is the output good/safe?” |
| Tracing    | “What exactly happened?”   |

👉 This triad is explicitly part of observability    [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

***

# 🔷 9) Real-world complexity (key insight)

From Foundry docs:

GenAI agents are hard to debug because:

* many steps per request
* step order varies
* nested operations
* long inputs/outputs

 [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/concepts/trace-agent-concept)

Tracing solves this by:

* showing every step
* in execution order

***

# 🔷 Vocabulary section

* **Tracing** — capturing execution flow of a request
* **Trace** — full lifecycle of a request
* **Span** — single operation within a trace
* **Attribute** — metadata attached to spans
* **OpenTelemetry (OTel)** — standard for telemetry collection
* **Instrumentation** — adding tracing logic to your app
* **Server-side tracing** — auto-collected platform traces
* **Client-side tracing** — custom app-level traces
* **Hierarchy** — nested structure of spans
* **Execution flow** — step-by-step workflow
* **Observability** — combined view of monitoring, tracing, evaluation

***

# 🔷 Useful examples

## Example 1 — Debugging wrong output

Problem:

* AI gives incorrect answer

Trace shows:

* agent called wrong tool

Fix:

* update prompt/tool selection logic

***

## Example 2 — Latency issue

Problem:

* slow response

Trace shows:

* tool call taking 8 seconds

Fix:

* optimize external API / reduce tool usage

***

## Example 3 — Cost spike

Problem:

* token usage increased

Trace shows:

* long prompt passed to model

Fix:

* trim context

***

## Example 4 — Multi-agent workflow

Problem:

* intermittent failures

Trace shows:

* nested process failing inside tool call

Fix:

* isolate failing component

***

# 🔷 AI‑300 Exam Callouts (HIGH VALUE)

## ✅ MUST KNOW

* Trace vs Span vs Attribute
* Tracing purpose (debugging + understanding behavior)
* OpenTelemetry as standard
* Relationship:
  * monitoring vs evaluation vs tracing

## ✅ VERY LIKELY

* What tracing captures (inputs, outputs, tool calls)
* Server-side vs client-side tracing
* Using tracing to find:
  * latency issues
  * failures
  * inefficiencies

## ✅ IMPORTANT CONCEPT

Tracing = **step-by-step execution visibility**

***

# 🔷 Condensed “study this” summary

If you memorize one section:

* Tracing shows the **full execution flow of an AI request**
* Built on **OpenTelemetry + Application Insights**
* Uses:
  * traces = full request
  * spans = individual steps
* Helps debug:
  * latency
  * errors
  * incorrect outputs
* Part of observability:
  * monitoring + evaluation + tracing



