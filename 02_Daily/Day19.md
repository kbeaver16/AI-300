# DAY 19 — EVALUATE AGENTS
- https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/
## Detailed Notes — Evaluate and optimize AI agents through structured experiments

## 1) What this module is about

This module sits in the GenAIOps learning path and is explicitly about turning “guesswork” into evidence-based engineering decisions for AI agents. Its core promises are to help you design evaluation experiments, use Git-based workflows to organize variants, create scoring rubrics for consistent human evaluation, and compare results to make optimization decisions. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)

The prerequisites are modest but important: you should already understand AI agents and LLMs, be comfortable with Git workflows, and have experience with Microsoft Foundry or a similar AI development platform. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/)

## 2) Where it fits in the bigger GenAIOps story

The broader learning path frames this module as one part of a complete operational lifecycle for generative AI apps: plan and prepare the solution, manage prompts with GitHub, evaluate and optimize through structured experiments, automate evaluations, monitor app performance/cost, and analyze/debug with tracing. In other words, this module is the “deliberate experimentation” layer that sits between prompt/version management and full automation/observability. [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/), [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)

An internal AI-300 training deck reinforces this lifecycle framing: define agent specifications, test prompts and model choices, evaluate agent behavior, publish agents, and then operate with monitoring, compliance, and safety. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

## 3) The central idea: structured experiments

The module’s operational model is simple: compare controlled variants using measurable signals. The internal AI-300 material makes that concrete by saying evaluation metrics should span three dimensions:

- **Quality** — for example intent resolution, relevance, and groundedness. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Cost** — token usage and model pricing. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Performance** — response time and time-to-first-token. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

The variants you test can include a baseline prompt, prompt variations, different model choices (for example a larger model versus a cheaper/faster one), and configuration changes such as `max_tokens` or streaming behavior. The training emphasizes that your testing approach should include diverse prompts, defined success criteria/thresholds, comparison methodology, and documentation for reproducibility. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

### Practical takeaway

Think of every optimization attempt as an experiment with:

1. one controlled change,
2. a shared test set,
3. explicit success criteria,
4. logged results you can compare later.  
    This is the throughline of the whole module. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

## 4) Git-based experimentation workflow

One of the learning objectives is to apply Git workflows to agent optimization. The AI-300 slide deck makes the workflow concrete:

- create a separate branch per variant,
- store your test prompts in source control,
- run the agent and capture outputs,
- score results (manually or programmatically),
- compare branches and merge the winner. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

The same deck also stresses prompt/version discipline more broadly: prompts should be versioned for tracking and rollback, reviewed before deployment, separated by environment, and deployed through a development → validation → monitored production path. Without that discipline, the risks are silent degradation, environment drift, no crisis recovery path, and lost knowledge. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

### What to write down in your notebook

A “good” experiment branch should capture:

- the exact prompt/model/config change,
- the exact test prompt set,
- the scoring rubric or evaluator set,
- before/after results,
- final decision and rationale. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

## 5) Human scoring: rubrics are not optional

A key module objective is creating evaluation rubrics that keep human scoring consistent. The AI-300 training deck says that strong evaluation practices include:

- detailed rubrics with concrete anchors for each score level,
- calibration exercises so evaluators align on how to interpret the rubric,
- inter-rater reliability checks to ensure consistency over time, and
- alignment between human criteria and automated evaluators for eventual human-in-the-loop workflows. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

A separate internal playbook on model evaluation expands this nicely: good evaluation criteria come from signals such as ground truth pairs, golden datasets, human preferences, expert demonstrations, outcome/assertion checks, and safety/fairness checks. It also recommends balancing **system-level**, **task-level**, and **human-level** metrics rather than optimizing only one dimension. [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1)

### Strong note to memorize

Rubrics are how you turn fuzzy ideas like “helpful,” “clear,” or “good enough” into repeatable evidence. Without them, you can compare outputs, but you cannot reliably compare decisions. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1)

## 6) Human vs automated vs human-in-the-loop (HITL)

The related AI-300 material spends a lot of time on the tradeoffs:

- **Human evaluation** brings nuanced judgment and context, but it is slow, expensive, and hard to scale. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Automated evaluation** applies metrics consistently and scales quickly, but it can miss nuance and still needs validation. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Human-in-the-loop** combines automation for scale with targeted human review for critical or ambiguous cases. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

The operational pattern recommended by the training is:

1. run automatic scoring on every change,
2. route low-quality or sampled responses to people,
3. periodically recheck alignment,
4. refine evaluators based on disagreements. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

## 7) Shadow rating and validator trust

One of the smartest and most exam-relevant ideas in the training is **shadow rating**. Before you trust automated evaluators at scale, you should have humans and automated evaluators score the same examples and then compare them. The AI-300 internal training proposes a four-step pattern:

- shadow rate the same 100 examples,
- check correlation between human and automated scores,
- analyze disagreements,
- refine evaluator prompts/configuration. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

The training even gives a rough target: strive for a correlation around **0.7 or better** before leaning too heavily on automated scoring. It also shows an example of using Python with `pandas` and `pearsonr` from `scipy.stats` to compute correlation between human and automated intent-resolution ratings. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

### Study-worthy interpretation

Automation is not “trustworthy” because it is automatic. It becomes trustworthy only after you validate its alignment with human judgment and keep monitoring for drift. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

## 8) Built-in evaluator vocabulary you should know

The Evaluation and monitoring metrics for generative AI - Azure AI Foundry | Microsoft Learn reference is extremely useful because it defines the built-in metric families and what each one measures.

### Agent evaluators

- **Intent Resolution** — how well the agent identifies and clarifies user intent and stays in scope. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Tool Call Accuracy** — whether the agent selects the right tools and processes tool inputs correctly. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Task Adherence** — how well the final response meets the goal/request. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Response Completeness** — how comprehensive the response is compared with provided ground truth. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)

The related Agent evaluators page adds an important conceptual distinction: **system evaluation** checks end-to-end outcomes, while **process evaluation** checks whether the workflow steps were executed well. Foundry’s agent evaluators behave like unit tests for agentic systems and often output binary pass/fail results. [Agent Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/agent-evaluators), [Agent Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/agent-evaluators)

### RAG evaluators

- **Groundedness** — whether the response aligns with the given context. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Groundedness Pro** — whether the text is consistent/accurate relative to context. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Retrieval** — the quality of the retrieved context chunks without ground truth. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Relevance** — how effectively the response addresses the query. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)

### General evaluators

- **Coherence** — logical flow and organization of ideas. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Fluency** — clarity and grammatical/readability quality of writing. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)

### Risk and safety evaluators

The metrics reference also calls out harmful-content, jailbreak, code-vulnerability, protected-material, and ungrounded-attribute evaluators. That matters because the official docs repeatedly frame evaluation not just as “quality scoring” but as quality **plus safety**. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in), [Evaluate y...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/evaluate-agent)

## 9) Running evaluations in practice

The Evaluate your AI agents page is the clearest operational guide. It says evaluation is essential for establishing a baseline and setting acceptance thresholds before deployment. It recommends using built-in evaluators for quality, safety, and agent behavior; creating a test dataset; running the evaluation; and interpreting/integrating results into your workflow. It even gives a concrete example threshold: an 85% task-adherence passing rate. [Evaluate y...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/evaluate-agent)

The SDK-based setup requires:

- Python 3.8+,
- a Foundry project with an agent,
- an Azure OpenAI deployment that supports chat completion, and
- the right project role. [Evaluate y...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/evaluate-agent)

The Foundry portal article shows the four evaluation targets you can choose:

- **Agent**,
- **Model**,
- **Dataset**,
- **Traces**. [Run evalua...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)

The cloud evaluation article adds that cloud evaluations are especially appropriate for:

- large-scale testing,
- predeployment testing,
- CI/CD integration,
- scheduled evaluations,
- continuous evaluation in production. [Cloud Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/cloud-evaluation)

And if you are working in CI/CD, the Azure DevOps integration supports datasets plus evaluator lists and produces summary reports that include scores, confidence intervals, and statistical comparison when multiple agents are compared. [How to run...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-azure-devops)

## 10) Useful examples from the training

### Example 1: Customer support assistant

The training repeatedly uses a customer-support AI assistant to show why HITL matters. Automated checks can flag low groundedness or latency across thousands of responses, while humans review ambiguous or high-risk cases. This is the canonical example of when to combine scalable judgment with targeted expert review. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

### Example 2: Technical support agent

The AI-300 content suggests evaluating technical support answers with three quality signals:

- did the answer resolve the user’s actual problem (**intent resolution**)?
- did it stay on the actual question (**relevance**)?
- did it rely on trusted documentation (**groundedness**)? [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)

### Example 3: Experiment tradeoff decision

One knowledge-check scenario says: if a cheaper model cuts cost massively and improves response time but slightly misses your quality threshold, you should document the tradeoff and seek stakeholder input rather than automatically deploy or automatically reject it. That is a perfect picture of “evidence-based optimization decisions.” [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)

## 11) AI-300 exam callouts

Here’s the part I’d star in your notes.

### Official exam-domain relevance

The official Study guide for Exam AI-300: Operationalizing Machine Learning and Generative AI Solutions | Microsoft Learn says **Implement generative AI quality assurance and observability** is **10–15%** of the exam and includes all of the following:

- create test datasets and data mapping for evaluation,
- implement groundedness, relevance, coherence, and fluency metrics,
- configure risk and safety evaluations,
- set up automated evaluation workflows using built-in and custom metrics,
- monitor latency, throughput, response time, token usage/resource usage,
- configure logging, tracing, and debugging. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

The same study guide says **Optimize generative AI systems and model performance** is another **10–15%** and includes:

- optimize RAG retrieval performance (similarity thresholds, chunk sizes, retrieval strategies),
- select/fine-tune embeddings,
- implement hybrid search,
- evaluate and improve RAG with relevance metrics and A/B testing frameworks,
- design fine-tuning workflows and manage synthetic data. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

It also explicitly warns that most questions cover GA features, but the exam **may include preview features if they are commonly used**. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

### My strongest “likely examable” list

Based on the study guide plus the module/training assets, I would treat these as high-yield:

1. **Intent Resolution, Relevance, Groundedness, Coherence, Fluency** — know what each measures. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in), [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
2. **Test datasets + data mapping** — know why and how they are used. [Evaluate y...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/observability/how-to/evaluate-agent), [Run evalua...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
3. **Automated evaluations in Foundry/CI-CD** — Foundry portal, SDK, GitHub Actions/Azure DevOps. [Run evalua...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [Cloud Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/cloud-evaluation), [How to run...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluation-azure-devops), [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
4. **Observability metrics** — latency, throughput, response time, token usage, error rate. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1), [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)
5. **Tracing vs monitoring** — what each tells you and when to use them. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1), [Run evalua...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app)
6. **A/B testing / structured experiments / branch-based comparisons** — central optimization pattern. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300)

## 12) Vocabulary section

- **GenAIOps** — practices and tooling for optimizing, deploying, and maintaining LLM-based applications. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Structured experiment** — a controlled comparison of agent variants using defined metrics and thresholds. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Baseline** — the current or reference version you compare new variants against. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Variant** — a prompt/model/configuration alternative being tested. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Rubric** — anchored scoring criteria that define what each score means. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1)
- **Calibration exercise** — practice scoring so human raters interpret the rubric consistently. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Inter-rater reliability** — agreement level across evaluators over time. [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
- **Shadow rating** — humans and automated evaluators rate the same outputs to validate alignment. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Correlation** — statistical relationship between human scores and automated scores; used to validate trust in automation. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Intent Resolution** — whether the agent understood and addressed the user’s goal. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Tool Call Accuracy** — whether the agent picked the right tools and parameters. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Task Adherence** — whether the final response actually met the requested goal. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in), [Agent Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/concepts/evaluation-evaluators/agent-evaluators)
- **Groundedness** — whether the response is faithful to the provided context/source material. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Relevance** — whether the response directly addresses the query. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Coherence** — logical flow of the answer. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Fluency** — readability and linguistic quality of the answer. [Evaluation...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-foundry/concepts/evaluation-metrics-built-in)
- **Trace** — end-to-end request journey. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Span** — an individual step inside a trace. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Attribute** — diagnostic metadata attached to a span/trace. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
- **Continuous evaluation** — recurring or production-time evaluation rather than one-off testing. [Cloud Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/cloud-evaluation)

## 13) Condensed “study from this” summary

If you only keep five lines from this module, keep these:

1. Evaluate **quality, cost, and performance** together — never optimize in a single dimension. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
2. Use **Git branches + shared test sets + explicit thresholds** to make optimization decisions reproducible. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1)
3. Build **rubrics** first; otherwise comparisons are subjective and unstable. [Evaluate a...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/evaluate-optimize-agents/), [AI-300T00-...erPoint_03 | PowerPoint](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B9B9B2B91-A167-4A3C-99E6-7D812BF68EA0%7D&file=AI-300T00-ENU-PowerPoint_03.pptx&action=edit&mobileredirect=true&DefaultItemOpen=1), [05.2026 CA...ter Kit V2 | PDF](https://microsoft.sharepoint.com/teams/BICResearch/BAP%20Research%20Repository/AIFI/2026/01.2026%20BIC%20Model%20Eval%20Starter%20Kit%20&%20Presentations/05.2026%20CAP%20UXR%20Model%20Eval%20Starter%20Kit%20V2.pdf?web=1)
4. Don’t trust auto-evaluators blindly — validate them with **shadow rating** and monitor for drift. [AI-300T00-...erPoint_04 | Word](https://microsoft.sharepoint.com/teams/MTTVideoProduction2/_layouts/15/Doc.aspx?sourcedoc=%7B4187E813-C43A-4B03-A885-582076046273%7D&file=AI-300T00-ENU-PowerPoint_04.docx&action=default&mobileredirect=true&DefaultItemOpen=1)
5. For AI-300, know the metrics, evaluation targets, automation options, observability signals, and how to interpret tradeoffs. [Study guid...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/credentials/certifications/resources/study-guides/ai-300), [Run evalua...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/evaluate-generative-ai-app), [Cloud Eval...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/foundry/how-to/develop/cloud-evaluation)

If you want, I can next turn this into a **clean OneNote/Obsidian study note format** with headings, bullets, and a short cram sheet at the end.

# DAY 20 — AUTOMATE EVALUATION
- https://learn.microsoft.com/en-us/training/modules/automated-evaluation-genaiops/

# DAY 21 — MONITOR APPLICATION
- https://learn.microsoft.com/en-us/training/modules/monitor-generative-ai-app/