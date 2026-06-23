# Automate Evaluation

# Detailed Notes — Automate AI evaluations with Microsoft Foundry and GitHub Actions

## 1) What this module is about

Automate AI evaluations with Microsoft Foundry and GitHub Actions is an **intermediate** Microsoft Learn module in the Operationalize generative AI applications (GenAIOps) learning path. It is listed as **1 hr 14 min** and **9 units**. The module says it teaches you how to implement automated evaluations for AI agent responses by using Microsoft Foundry evaluators, create evaluation datasets from production data and synthetic generation, run batch evaluations with Python scripts, and integrate evaluation workflows into GitHub Actions for continuous quality assurance. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)

The module’s official learning objectives are to explain why automated evaluations complement human evaluations, select evaluators that align with human evaluation criteria, create evaluation datasets with appropriate composition, implement batch evaluations using Python scripts with Microsoft Foundry, and integrate automated evaluation workflows into GitHub Actions for continuous testing. The prerequisites explicitly call out familiarity with fundamental generative AI concepts plus basic familiarity with Python and GitHub workflows. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/)

## 2) How this module fits into GenAIOps

The parent learning path describes a full GenAIOps lifecycle: planning and preparing a solution, managing prompts with version control, evaluating and optimizing through structured experiments, automating evaluations, monitoring performance and costs, and tracing/debugging complex AI workflows. That means this module is not just “about running tests”—it is the **automation bridge** between manual evaluation work and full operational quality gates in CI/CD. [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)

The retrieved learning path description explicitly places this module after Evaluate and optimize AI agents through structured experiments and before monitoring and tracing modules. That ordering matters: first define what “good” looks like, then automate it, then observe it in production. [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/)

## 3) The core message: automated evaluation complements human evaluation

The central point of this module is that automated evaluations are useful because they **scale**, **standardize**, and can be run continuously, but they are meant to **complement** human evaluations rather than fully replace them. The module learning objectives say you should be able to explain that relationship, and the broader observability guidance frames evaluation as part of a lifecycle focused on quality, safety, and reliability. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

The observability documentation explains why this matters operationally: without robust evaluation, AI systems can become inaccurate, inconsistent, poorly grounded, or harmful. Microsoft Foundry positions evaluation, monitoring, and tracing as the three core observability capabilities that work together across development and production. [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

### Practical takeaway

For your study notes: **human evaluation defines or validates what quality means; automated evaluation enforces and scales it repeatedly**. That framing is directly supported by the module objective to align evaluators with human criteria and by the observability guidance that turns evaluation into reusable lifecycle tooling. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## 4) Unit-level flow of the module

The module page lists these units: **Introduction**, **Understand why automated evaluations matter**, **Align evaluators with human criteria**, **Create evaluation datasets**, **Implement batch evaluations with Python**, **Integrate evaluations into GitHub Actions**, **Exercise - Set up automated evaluations**, **Knowledge check**, and **Summary**. Even without the full body text for each internal unit, the sequence clearly shows the intended learning progression: motivation → evaluator selection → dataset preparation → local/batch execution → CI/CD integration. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/)

That sequence also maps well to the operational documentation:

- evaluator choice is explained by the evaluator references,
- dataset and evaluation flow are explained by the Foundry evaluation pages,
- GitHub integration is explained by the How to run an evaluation in GitHub Action article. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)

## 5) Evaluators: what they are and how to choose them

The observability documentation defines evaluators as specialized tools that measure the quality, safety, and reliability of AI responses throughout the lifecycle. It groups them into categories such as general-purpose metrics, RAG metrics, safety/security metrics, and agent-specific metrics. [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

The How to run an evaluation in GitHub Action article lists the evaluator categories available in the GitHub Action workflow: **Agent evaluators**, **RAG evaluators**, **Risk and safety evaluators**, **General purpose evaluators**, **OpenAI-based graders**, and **Custom evaluators**. That’s a very direct indicator of what the module wants you to know when it says “select evaluators that align with human evaluation criteria.” [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

The General purpose evaluators page gives concrete meaning to two exam-relevant metrics:

- **Coherence** measures the logical and orderly presentation of ideas so the response is easy to follow. Higher scores mean better logical flow.
- **Fluency** measures clarity and written communication quality, including grammar, vocabulary, sentence complexity, coherence, and readability. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)

The Run evaluations from the Microsoft Foundry portal article adds more evaluator options depending on scope. For individual turns, it lists agent evaluators such as **Intent Resolution**, **Task Adherence**, **Tool Call Success**, **Tool Selection**, **Tool Output Utilization**, **Tool Input Accuracy**, and **Tool Call Accuracy**. It also lists quality evaluators such as **Customer Satisfaction**, **Task Completion**, **Coherence**, **Groundedness**, **Response Completeness**, **Fluency**, and **Relevance**, plus safety evaluators such as **Violence**, **Sexual**, **Self-harm**, and **Hate/Unfairness**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

### Practical takeaway

When choosing evaluators, you are matching the **kind of failure you care about** to the **metric that exposes it**. If your concern is writing quality, use coherence/fluency; if it is tool behavior, use agent evaluators; if it is harmful outputs, use safety evaluators. That mapping is explicitly supported across the evaluator and portal docs. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## 6) Evaluation datasets: what the module wants you to learn

The module states that you should learn to create evaluation datasets with “appropriate composition for comprehensive testing.” The portal documentation breaks that into explicit data-source patterns. For agent and model evaluation, Foundry supports **simulated data**, **existing conversations**, **existing datasets**, and **existing traces**, depending on the evaluation scope and target. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

The portal article says:

- for full-conversation agent evaluations, simulated data is good for controlled predeployment testing and existing conversations are good for real production monitoring,
- for individual-turn evaluations, you can use synthetic data, existing datasets, or existing traces,
- model evaluations can use synthetic or existing datasets,
- dataset evaluations can evaluate outputs that were already produced without rerunning the model or agent. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

It also explains required field mapping. For individual-turn evaluations, common fields include **query**, **response**, **context**, **ground_truth**, **tool_calls**, and **tool_definitions**. For conversation evaluations, fields such as **messages** and **tool_definitions** are required. This is exactly the sort of concrete implementation detail the module title “Create evaluation datasets” suggests you should know. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

The GitHub Action article adds a JSON data file structure with top-level fields like **name**, **evaluators**, **data**, and optional **openai_graders**, **evaluator_parameters**, and **data_mapping**. It also lists sample files such as datasets for built-in evaluators, OpenAI-based graders, custom evaluators, and explicit data mapping examples. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### Practical takeaway

A good evaluation dataset is not just “some prompts.” It is a structured asset that contains the right input fields for the evaluators you plan to run and enough variety to expose quality, safety, and behavior issues. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

## 7) Batch evaluations with Python

The module explicitly says you will learn to “implement batch evaluations using Python scripts.” The surrounding Microsoft Foundry docs reinforce that evaluations can be run programmatically and at scale. The portal documentation recommends using the Azure AI Evaluation SDK for programmatic evaluation workflows, and the observability page includes evaluation quick-reference guidance for running evaluations and then analyzing results. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

The general-purpose evaluators article provides a real configuration example showing Python-style evaluator definitions with `builtin.coherence` and `builtin.fluency`, initialization parameters like `deployment_name`, and field mappings such as `{{item.query}}` and `{{item.response}}`. That’s directly useful as a mental model for how batch evaluation scripts wire datasets to evaluators. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)

## 8) GitHub Actions integration

This is the most implementation-heavy part of the module and the part most clearly expanded by linked documentation. The How to run an evaluation in GitHub Action page says the GitHub Action is for **offline evaluation of Microsoft Foundry agents inside CI/CD pipelines** so you can identify problems and improve the agent before production release. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

The same page explicitly states:

- you provide a dataset with test queries and a list of evaluators,
- the action invokes the agent(s), runs evaluations, and generates a summary report,
- the results include **confidence intervals** and **tests for statistical significance**,
- if you evaluate multiple agents, you can compare them and see whether differences are meaningful or just random variation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

It also documents core required inputs:

- `azure-ai-project-endpoint`
- `deployment-name`
- `data-path`
- `agent-ids`  
    and an optional `baseline-agent-id` for multi-agent comparison. The page recommends authenticating with Microsoft Entra ID and the Azure Login GitHub action. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

The example workflow uses:

- `actions/checkout@v4`
- `azure/login@v2`
- `microsoft/ai-agent-evals@v3-beta`  
    inside a GitHub workflow triggered on `workflow_dispatch` or `push` to `main`. The page also includes a practical tip: **don’t run evaluation on every commit if you want to minimize costs**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

### Practical takeaway

The module is teaching you to turn evaluation into a **quality gate**. Instead of manually checking outputs after a change, you can push a change and have your CI/CD pipeline run evaluators, generate scores, compare against baseline behavior, and surface whether the change is statistically meaningful. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## 9) Viewing and interpreting evaluation results

The portal documentation says an evaluation can be started from the **Evaluation** page, a **Model** page, an **Agent** page, or the **Agent playground**. After submission, the status can be **In Progress**, **Completed**, **Partial**, or **Failed**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

The GitHub Action page says results appear in the run summary in GitHub Actions, including evaluation scores for each metric, confidence intervals, and statistical comparison for multiple agents. That makes the automation not just “run some tests,” but “produce decision-ready evidence.” [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

The observability guidance emphasizes using evaluation history and other observability signals together: evaluation gives you quality and safety measures, monitoring gives you performance/cost/runtime signals, and tracing gives you step-by-step execution visibility. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## 10) Observability connection: evaluation is one pillar, not the whole story

The observability article says Microsoft Foundry has three core observability capabilities:

- **Evaluation** for quality, safety, and reliability,
- **Monitoring** for operational metrics like token consumption, latency, error rates, and quality scores,
- **Tracing** for distributed execution visibility across LLM calls, tool calls, agent decisions, and service dependencies. [Observabil...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

It also outlines three stages of lifecycle evaluation:

- **Base model selection**
- **Pre-production evaluation**
- **Post-production monitoring** [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

That means this module is mainly about **pre-production automation**, but it exists inside a broader pattern where automated evaluation continues to matter after deployment through continuous evaluation and monitoring. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

---

# Notes on the related pages I followed

## How to run an evaluation in GitHub Action

This page is the most direct companion to the module. It documents a GitHub Action for offline agent evaluation, explains supported evaluator categories, gives the input schema, shows example dataset structure, and includes a YAML workflow example. It also explicitly says this feature is **preview** and notes that preview features don’t carry an SLA and aren’t recommended for production workloads. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)

## Run evaluations from the Microsoft Foundry portal

This page explains evaluation targets (**Agent**, **Model**, **Dataset**, **Traces**), scopes (**Full conversations** and **Individual turns** where applicable), data sources, evaluator selection, field mapping, and step-by-step creation flow in the portal. It is the clearest source for understanding how Foundry models agent evaluation conceptually and operationally. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

## Observability in Generative AI

This page explains how evaluation, monitoring, and tracing fit together and why automated evaluations matter beyond just developer convenience. It also positions evaluation across the AI lifecycle and describes how Foundry integrates these capabilities with Azure Monitor Application Insights. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## General purpose evaluators

This page gives explicit definitions for **coherence** and **fluency**, explains when to use them, shows sample data mapping, and says these evaluators use LLM-as-judge scoring and therefore incur model inference costs. It also states that both evaluators currently support **English-language responses** and that the default pass threshold is **3** on a **1–5 Likert scale**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)

---

# Vocabulary section

- **GenAIOps** — Operational practices for generative AI applications, including evaluation, automation, monitoring, and tracing. [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- **Automated evaluation** — Running evaluators programmatically and repeatedly to assess AI outputs for quality, safety, and behavior. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)
- **Evaluator** — A tool that measures some aspect of AI output quality, safety, reliability, or agent behavior. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)
- **Coherence** — Logical flow and organization of ideas in a response. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)
- **Fluency** — Readability and language quality of a response. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)
- **Groundedness** — Whether a response is grounded in the provided context. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Relevance** — Whether a response addresses the user’s query appropriately. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
- **Task Adherence** — Whether the agent followed instructions and constraints for the task. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
- **Tool Call Accuracy** — Overall accuracy of the agent’s tool use. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
- **Field mapping** — The mapping of columns in your dataset to the fields an evaluator expects, such as `query`, `response`, `context`, or `messages`. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
- **Baseline agent** — The agent version used as the reference point when comparing multiple agents in an evaluation workflow. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- **Confidence interval** — A statistical range shown in evaluation results to help interpret metric reliability. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- **Statistical significance** — A test used when comparing agents to assess whether differences are likely meaningful rather than random. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- **Observability** — The ability to monitor, understand, and troubleshoot AI systems throughout their lifecycle using evaluation, monitoring, and tracing. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)
- **Tracing** — Capturing execution flow, including model calls, tool calls, and dependencies, to debug complex systems. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

---

# Useful examples

## Example 1: Technical support agent regression check

You update an agent’s instructions and want to verify that the change didn’t reduce answer quality. You prepare a dataset of test queries, run **coherence**, **fluency**, **task adherence**, and a safety evaluator in a GitHub Action, and compare the updated agent to a baseline version. The GitHub Action summary then shows scores, confidence intervals, and whether the difference is statistically meaningful. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

## Example 2: RAG-style single-turn evaluation

You have a dataset with `query`, `response`, `context`, and `ground_truth`. In the portal, you choose **Agent > Individual turns > Existing dataset**, map the fields, and run **groundedness**, **relevance**, **response completeness**, **coherence**, and **fluency**. That setup directly matches the portal’s field-mapping and evaluator model. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)

## Example 3: Production monitoring follow-up

After deploying an agent, you continue evaluating quality while also monitoring latency, token usage, and error rates. That is not just “extra telemetry”—the observability docs describe it as part of post-production monitoring so teams can maintain safety and performance over time. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)

## Example 4: Synthetic predeployment conversation testing

Before launching a new agent, you use **Full conversations > Simulated data** in Foundry to generate conversations from scenario descriptions. The portal docs recommend this for controlled predeployment testing and say you can vary the number of simulated conversations per scenario and the number of turns per conversation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

---

# AI-300 exam callouts

## Highest-confidence exam topics from the official study guide

The official Study guide for Exam AI-300: Operationalizing Machine Learning and Generative AI Solutions says **Implement generative AI quality assurance and observability** is **10–15%** of the exam, and it explicitly includes:

- creating test datasets and data mapping,
- implementing quality metrics such as **groundedness**, **relevance**, **coherence**, and **fluency**,
- configuring **risk and safety evaluations**,
- setting up **automated evaluation workflows** by using built-in and custom metrics,
- monitoring performance metrics like **latency**, **throughput**, and **response times**,
- tracking and optimizing costs like **token consumption** and **resource usage**,
- configuring detailed **logging, tracing, and debugging**. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

That same study guide also says candidates should have experience with Microsoft Foundry, GitHub Actions, and Python-based operational workflows. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

## What I would star in your study notes

Based on the official study guide plus the module and linked pages, I would treat these as especially likely AI-300 material:

1. **Why automated evaluations complement human evaluations**. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/)
2. **How to choose evaluators that match your evaluation goal**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/general-purpose-evaluators)
3. **Dataset composition and field mapping** for `query`, `response`, `context`, `ground_truth`, and `messages`. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
4. **GitHub Actions integration** for pre-production automated evaluation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/)
5. **Confidence intervals and statistical significance** when comparing agent versions. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
6. **Observability triad**: evaluation + monitoring + tracing. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

## Preview feature caution

The GitHub Action evaluation page is explicitly marked **preview**, and the study guide says most exam questions cover **general availability (GA)** features, though preview features can appear if they are commonly used. So I would absolutely study the workflow concepts, but keep in mind the preview status when prioritizing memorization depth. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

---

# Condensed “study from this” summary

If you only keep a few lines from this module, keep these:

- Automated evaluation turns AI quality checks into a **repeatable CI/CD practice** instead of one-off manual review. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action)
- A good workflow is: define evaluation goals, choose the right evaluators, build the right dataset, run batch evaluation, then integrate it into GitHub Actions. [Automate A...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
- Use evaluator categories deliberately: agent, quality, RAG, safety, OpenAI-based graders, or custom evaluators. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-github-action), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
- Evaluation is only one pillar; production-grade GenAIOps also needs monitoring and tracing. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/observability)
- For AI-300, know the metrics, field mapping, automation patterns, and observability concepts extremely well. [learn.microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

If you want, I can turn this into a **clean OneNote/Obsidian-ready study note** with shorter bullets and a “memorize this for AI-300” cram section.