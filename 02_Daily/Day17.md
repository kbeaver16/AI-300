# DAY 17 — MODULE 9
Manage prompts for agents in Microsoft Foundry with GitHub

## MODULE LINK
- https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/

## TASKS
- [x] Complete module ✅ 2026-06-10
- [x] Learn prompt versioning ✅ 2026-06-10

## KEY CONCEPTS
- Prompt lifecycle
- Git-controlled prompts

## NOTES
## Module snapshot

- Manage prompts for agents in Microsoft Foundry with GitHub is an **intermediate** Microsoft Learn module in the broader Operationalize generative AI applications (GenAIOps) learning path, and it is listed as **53 minutes / 8 units**. [Manage pro...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- The module’s stated purpose is to teach you how to manage AI prompts as **versioned assets** by using **GitHub**, and to apply software engineering practices to create, test, and promote prompt versions used in Microsoft Foundry as part of a **GenAIOps** workflow. [Manage pro...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/)
- The official learning objectives are to: apply version control principles to prompts as code assets, understand how prompts integrate with Microsoft Foundry agents and versioning strategies, design a GitHub repository structure for prompt versioning and collaboration, and develop a workflow for safely testing and deploying prompts. [Manage pro...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/)
- The learning path positions this module between **planning/preparing GenAIOps** and **evaluation/automation/monitoring/tracing**, which means Microsoft expects prompt management to be part of a larger lifecycle rather than a one-off authoring task. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)

---

## Detailed notes

### 1) Core mental model: prompts are code, not just text

- The central idea of the module is that prompts should be treated as **versioned engineering artifacts**, not as ad hoc strings hidden inside code or portal configuration. That means prompts belong in source control, should be reviewed, compared, tested, and promoted the same way you treat application code or infrastructure-as-code. [Manage pro...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- In practice, this matters because agent behavior depends heavily on the quality and wording of instructions, and small prompt changes can materially affect outputs, tool usage, quality, and safety. The module’s place in the GenAIOps path reinforces that prompt changes should be managed as part of a repeatable lifecycle that includes evaluation and monitoring. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- The Agent development lifecycle page confirms this lifecycle framing by explicitly calling out creating agents, saving changes as versions, debugging with tracing, evaluating quality and safety, publishing stable endpoints, and monitoring performance in production. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)

### 2) How prompts fit into Microsoft Foundry agents

- The linked What is Microsoft Foundry Agent Service? page explains that an agent combines **three core components**: a **model**, **instructions**, and **tools**. In this model, prompts are not standalone artifacts; they serve as the agent’s **instructions** and define goals, constraints, and behavior. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)
- The same page distinguishes **prompt agents** from **hosted agents**. Prompt agents are declaratively defined through model selection, instructions, and tools, while hosted agents are code-based and containerized. That distinction matters because prompt versioning is especially central for prompt agents, but the same discipline still benefits hosted-agent instructions and evaluation flows. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- Foundry documentation also states that prompt agents can be authored in the portal or defined programmatically with SDKs and REST APIs to fit CI/CD workflows. That is the bridge between “prompt engineering” and “GenAIOps”: prompts become deployable, reviewable assets rather than portal-only configuration. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)

### 3) Why GitHub is the right control plane for prompt changes

- GitHub provides the mechanics that make prompt versioning practical: history, branching, pull requests, comparison, rollback, and promotion workflows. While the module overview doesn’t enumerate GitHub features one by one, its objective to “organize prompts in GitHub repositories” clearly frames GitHub as the source of truth for change tracking and collaboration. [Manage pro...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- The study guide for Study guide for Exam AI-300: Operationalizing Machine Learning and Generative AI Solutions explicitly includes **implement prompt versioning and management with source control**, **create prompt variants and compare performance across different prompts**, and **implement version control for prompts by using Git repositories** under the “Design and implement a GenAIOps infrastructure” objective. That is the strongest exam-alignment signal in the retrieved sources. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- The effect is that GitHub isn’t just “where prompt files live”; it becomes the mechanism for collaboration, governance, safe experimentation, and evidence-based promotion of prompt revisions. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

### 4) Prompt variants and controlled experimentation

- The module and the AI-300 study guide both emphasize **prompt variants**. This means you should expect to create multiple instruction versions for the same agent behavior and compare them instead of relying on intuition or a single “best guess” prompt. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- The learning path’s downstream module on structured experiments adds the missing operational context: optimization should use **clear metrics**, **Git-based workflows**, **evaluation rubrics**, and comparisons across versions. In other words, prompt changes should be validated like experiments, not accepted just because they “sound better.” [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- The How to run an evaluation in GitHub Action page then shows how this becomes operational: provide a dataset, identify one or more agents, run evaluators, and produce a summary report with confidence intervals and statistical significance tests. That supports a mature “prompt variant A vs. prompt variant B” workflow before production release. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### 5) Safe deployment workflows for prompt changes

- The module explicitly includes a unit called **Develop safe prompt deployment workflows**, which signals that prompt changes should go through validation gates rather than going straight to production. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- The Agent development lifecycle page makes that concrete: save changes as versions, use tracing to inspect tool calls and latency, run evaluations to catch regressions, publish stable endpoints, and then monitor after publishing. This is the operational pattern you should associate with “safe prompt deployment.” [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- The How to run an evaluation in GitHub Action page adds a CI/CD angle. It describes a GitHub Action that can run **offline evaluation** in your CI/CD pipeline, invoke your agents with test queries, apply evaluators, and generate a summary. The page explicitly recommends this as a pre-production quality check. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- The same evaluation page notes that you can compare multiple agent IDs and get **pairwise statistical comparison**, which is exactly the kind of control you want before promoting a prompt change to production. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### 6) Versioning inside Foundry vs. versioning in Git

- Foundry itself supports **agent versions**, and the lifecycle page says each saved version of an agent is immutable after save, with version history, comparisons, and controlled rollback. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- Git versioning and Foundry versioning are complementary rather than redundant. Git records the authoring history of your prompt artifacts and collaboration decisions, while Foundry versioning captures the deployed or saved state of the agent configuration in the runtime platform. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)
- A good mental model is: **Git = source-of-truth authoring and collaboration**, **Foundry = runtime/deployment state and operational lifecycle**. That mapping is not stated as a single sentence in one source, but it is directly supported by the documented Git-based prompt management objective and Foundry’s documented versioned agent lifecycle. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)

### 7) Prompt quality improvement and optimization

- The Optimize agent prompts with Prompt Optimizer (preview) page shows that Microsoft Foundry can automatically restructure and enhance agent instructions using prompt-engineering best practices, while also showing paragraph-level reasoning for changes. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer)
- It describes a workflow where you start with an initial prompt or existing instructions, add optional suggestions, run optimization, review highlighted changes and reasoning, iterate with more suggestions, and then apply the result back to the agent. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer)
- This is useful to study because it reinforces what “good prompt governance” looks like: make changes transparently, review them, test them, and validate them after optimization. It also reinforces the idea that improving prompts is iterative and evidence-driven. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- Important exam nuance: the page is marked **preview**, and the AI-300 study guide says most exam questions cover **GA features**, though preview features may appear if they are commonly used. So Prompt Optimizer is useful to understand conceptually, but it is not as directly exam-grounded as the source-control/versioning objectives explicitly listed in the study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

### 8) Quickstart implementation pattern

- The Quickstart: Create a prompt agent page shows the code-first pattern for creating a prompt agent in Foundry Agent Service. You define a project endpoint, authenticate, and create an agent version using a `PromptAgentDefinition` that includes a model and instructions. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)
- The quickstart then shows how a conversation is created and how the agent is invoked through the Responses API for multi-turn chat. This is important because it connects prompt design to actual runtime behavior and makes clear that prompts are part of the persisted agent definition, not just inline request text. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)
- The quickstart also demonstrates concrete SDK packages such as `azure-ai-projects` and the project endpoint format, which helps you connect the conceptual module content to actual implementation and deployment patterns. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)

---

## Vocabulary section

- **GenAIOps** — Operational practices for generative AI applications, covering planning, prompt management, evaluation, automation, monitoring, and tracing. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- **Prompt versioning** — Managing prompts as versioned assets so you can track history, compare changes, collaborate, and roll back safely. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Prompt variant** — One version of a prompt/instructions used for comparison against alternate versions to measure quality or behavior differences. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- **Prompt agent** — A declaratively defined agent in Microsoft Foundry that combines a model, instructions, and tools, with no runtime application code to maintain. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)
- **Hosted agent** — A code-based, containerized agent run by Foundry with managed hosting, scaling, and identity. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)
- **Responses API** — The documented single entry point used by Foundry agent types to access models and platform tools. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)
- **Agent version** — A saved, immutable version of an agent configuration in Foundry that can be compared, evaluated, published, or rolled back. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- **Tracing** — Capturing model calls, tool invocations, and end-to-end behavior for debugging and validation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- **Evaluation** — Running repeatable checks on an agent or prompt version to measure quality, safety, relevance, groundedness, fluency, and related metrics. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Baseline agent** — The chosen comparison point when evaluating multiple agent versions in the GitHub Action evaluation workflow. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- **CI/CD** — Continuous integration and continuous delivery pipelines used to automate validation and promotion of changes, including prompt-related changes. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/overview)
- **RBAC** — Role-based access control used to govern permissions in Foundry. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

---

## Useful examples

### Example 1: Treating prompts as code

- You maintain an `agent-instructions/` folder in a GitHub repo where each prompt version is stored as a file. A pull request changes the instructions to tighten scope, another reviewer checks the diff, and the team tests the new prompt against evaluation data before publishing the updated Foundry agent version. This example is directly aligned with the module’s objectives and the documented source-control/evaluation workflow. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### Example 2: Comparing prompt variants

- Variant A says “Be concise and answer only with approved product details.” Variant B adds “If data is uncertain, say you need verification instead of guessing.” You run both against the same test dataset and compare quality/safety results using evaluators in a GitHub Action workflow before selecting the version to promote. This maps to the study guide’s prompt-variant objective and the GitHub Action evaluation flow. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### Example 3: Prompt agent creation in code

- In the quickstart pattern, you create a `PromptAgentDefinition` with a model such as `gpt-5-mini` and instructions like “You are a helpful assistant that answers general questions,” then save that as an agent version and invoke it through the Responses API. That is the code-first form of prompt versioning in Foundry. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/quickstarts/prompt-agent)

### Example 4: Iterative instruction improvement

- You open Prompt Optimizer on an existing agent, ask it to make the wording more professional and to add off-topic guardrails, review its paragraph-level reasoning, then test the optimized result in the playground before deciding whether to keep it. This is a practical example of structured prompt refinement. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)

---

## Likely AI-300 exam callouts

### Highest-confidence exam topics

- **Implement prompt versioning and management with source control** is explicitly named in the AI-300 study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Design and develop prompts** is explicitly named in the AI-300 study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Create prompt variants and compare performance across different prompts** is explicitly named in the AI-300 study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Implement version control for prompts by using Git repositories** is explicitly named in the AI-300 study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Configure evaluation and validation for generative AI applications and agents**, including metrics like groundedness, relevance, coherence, and fluency, is explicitly named in the AI-300 study guide and connects directly to the prompt-variant workflow. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- **Configure detailed logging, tracing, and debugging capabilities** is explicitly named in the AI-300 study guide and is reinforced by the agent-development lifecycle content. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle)
- **Identity, RBAC, network security, and environment setup for Foundry projects** are part of the GenAIOps infrastructure section in the study guide, so they’re adjacent knowledge you should know even though this specific module centers on prompt versioning. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/what-is-foundry)

### Helpful but lower-confidence / preview-adjacent topics

- **Prompt Optimizer** is useful conceptually for structured instruction improvement, but the page is marked **preview** and the study guide notes that most questions target **GA** features, though preview features can appear if commonly used. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/prompt-optimizer), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **GitHub Action–based offline AI agent evaluation** is also marked **preview**, but it strongly reinforces exam-relevant ideas around evaluation, automation, and safe promotion. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

---

## Short study takeaway

- For AI-300, your strongest takeaway from this module should be: **treat prompts as source-controlled assets, create prompt variants, evaluate them with repeatable metrics, and promote changes safely through a GitHub/Foundry lifecycle**. That message is strongly supported by the module objective, the learning path structure, the Foundry lifecycle documentation, and the official study guide. [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/prompt-versioning-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/development-lifecycle), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

If you want, I can turn these into a **OneNote/Obsidian-ready study note** with cleaner headers and a short “memorize this for AI-300” section.