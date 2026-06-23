# DAY 16 — MODULE 8
Plan and prepare a GenAIOps solution

## MODULE LINK
- [Plan and Prepare GenAiOps](https://learn.microsoft.com/en-us/training/modules/plan-prepare-genaiops/)

## TASKS
- [ ] Complete module
- [ ] Learn GenAI architecture

## KEY CONCEPTS
- Model selection
- Foundry environments

## NOTES
# Detailed Notes: Plan and prepare a GenAIOps solution - Training | Microsoft Learn

## 1) Module overview

This module is an **intermediate** Microsoft Learn module with **8 units** in the learning path Operationalize generative AI applications (GenAIOps) - Training | Microsoft Learn. Its stated goal is to teach how to **develop chat applications with language models using a code-first approach**, and it frames that approach as foundational to **Generative AI Operations (GenAIOps)** because code-first development makes flows more **robust** and **reproducible**. The explicit learning objectives are to identify generative AI use cases, select an appropriate model, and describe what GenAIOps is and how it defines the application lifecycle. The module also recommends a prerequisite: the fundamentals module [Introduction to generative AI and agents](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/)

### What this means in plain English
 
The module is less about “how to prompt once” and more about **how to prepare a generative AI application so it can be built, tested, deployed, and maintained responsibly**. The big mindset shift is that a GenAI solution is not just “pick a model and call an API.” It is a system with architecture, deployment choices, data strategy, monitoring, governance, and operational lifecycle considerations. [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

---

## 2) The core idea behind GenAIOps

A generative AI application is more than the LLM itself. Microsoft’s generative AI guidance explains that LLMs are the core, but production solutions also need components for user interaction, security, orchestration, data access, deployment monitoring, privacy, and lifecycle management. The playbook describes this broader solution as a **Generative AI application**, and it recommends using a standard **AI lifecycle** plus **LLMOps** or GenAIOps-style practices to manage it. [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

Microsoft’s Azure architecture guidance further explains that GenAIOps extends existing **MLOps** investments to cover **generative patterns** such as **prompting**, **fine-tuning**, and **retrieval-augmented generation (RAG)**. The article emphasizes that with generative AI, the operational scope expands beyond model hosting to include prompts, orchestrators, indexes, data stores, evaluations, and monitoring. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

The Azure Well-Architected guidance adds that AI workload operations span **application development**, **data handling**, and **AI model management**. It specifically says GenAIOps is a specialized subset of MLOps for generative AI, and that operational success depends heavily on **automation**, **repeatability**, **governance**, and **monitoring**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

### Key takeaway

**GenAIOps = the disciplined operational lifecycle for generative AI applications.**  
It covers not only the model, but also prompts, orchestration logic, data pipelines, deployment, security, evaluation, tracing, and monitoring. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

---

## 3) Unit theme: Explore use cases for GenAIOps

The module title suggests one of the early units is about identifying **where generative AI makes sense**. Microsoft’s generative AI playbook says enterprise GenAI applications are commonly centered on **language-to-language** and **language-to-action** solutions. It also explains that LLMs can power applications that generate content, answer questions, and support tasks. [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/), [Introducti...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/fundamentals-generative-ai/)

The architecture guidance on GenAIOps identifies several common solution patterns and use cases:

- **Prompt-based solutions** can support **classification**, **translation**, and **summarization**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)
- **RAG solutions** improve answers by grounding the model in proprietary or domain-specific content. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)
- **Fine-tuned solutions** are useful when existing foundation models are not sufficiently specialized for the target domain or task. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

### What to note for study purposes

When evaluating a use case, ask:

1. **Is this primarily language generation, understanding, retrieval, or reasoning?**
2. **Does the app need domain grounding (RAG), customized behavior (prompting), or a specialized model (fine-tuning)?**
3. **Will the app live in production long enough that monitoring, governance, and iteration matter?** If yes, the use case belongs in a GenAIOps mindset. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

### Useful example (worked example, created from the source concepts)

- **Summarization assistant for internal engineering tickets** → likely a **prompting + RAG** use case.
- **Internal policy Q&A bot** → likely a **RAG** use case because you need grounding in private documents.
- **Highly domain-specific classification model for legal intake** → may start with prompting, but if output consistency becomes critical, it may need **fine-tuning** or stronger evaluation pipelines.

---

## 4) Unit theme: Select the right generative AI model

This is one of the most important sections for AI-300-style thinking.

The Azure OpenAI models page shows that model selection should be based on **capabilities**, **modality**, **context window**, **maximum output tokens**, **region availability**, and whether the model is intended for **general use**, **reasoning**, **embeddings**, **image generation**, or **audio**. Microsoft explicitly lists model families such as **GPT-4.1**, **o-series reasoning models**, **GPT-4o / GPT-4o mini / GPT-4 Turbo**, **Embeddings**, **DALL-E**, and **Audio**. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)

A few directly useful facts from the page:

- **GPT-4.1 series** supports text and image input, text output, streaming, function calling, and structured outputs in chat completions. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **o-series models** are designed for **reasoning and problem-solving tasks**, with emphasis on science, coding, and math. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Embeddings** exist specifically to convert text to vectors for similarity and retrieval scenarios. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Preview models should not be used in production**; Microsoft explicitly says preview models don’t follow the standard model lifecycle. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)

### Deployment choice is part of model selection

The Azure OpenAI deployment types page adds another layer: after choosing a model, you also choose the **deployment type** based on **data processing location** and **call volume**. It states that Azure OpenAI offers **standard** and **provisioned** deployment families, with options such as **Global Standard**, **Data Zone Standard**, **Global Provisioned**, and **Global Batch**. Microsoft says **Global Standard is the recommended starting point**. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

Important design notes from that page:

- **Global Standard** gives high default quota and broad availability, but higher-consistency high-volume workloads may see latency variability. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)
- **Provisioned** is intended for high and predictable throughput. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)
- **Global Batch** is for asynchronous, large-scale tasks with a 24-hour target turnaround and 50% lower cost than Global Standard, making it useful for bulk document processing or large summarization jobs. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

### Model-selection checklist

When picking a model, note:

- **Task type**: chat, reasoning, summarization, translation, retrieval, image, audio. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Modalities**: text-only or text+image. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Reasoning need**: if complex reasoning matters, the **o-series** is specifically described for that. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Production readiness**: avoid preview models in production. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Scale and latency requirements**: choose deployment type accordingly. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)
- **Data residency / processing boundaries**: Global vs Data Zone vs geography-based choices matter. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

### Useful example (worked example, created from the source concepts)

- **A policy chatbot with moderate traffic and broad availability needs** → start with **Global Standard** and a chat-capable model.
- **A coding or troubleshooting assistant where deeper reasoning matters** → evaluate an **o-series reasoning model**.
- **A nightly bulk summarization pipeline over large archives** → consider **Global Batch**.
- **A RAG app** → pair a response model with an **embedding model** and search infrastructure.

---

## 5) Unit theme: Understand the development lifecycle of a language model application

This is the “operations” backbone of the module.

The Microsoft generative AI guide says the lifecycle is iterative and includes preparing, deploying, and improving the application over time. It also identifies the main building blocks of a GenAI solution:

- managed services for model access and adaptation,
- AI solution framework / orchestration,
- client applications,
- deployment monitoring and observability,
- security and privacy,
- data platform,
- and LLMOps to coordinate them all. [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

The GenAIOps-for-MLOps architecture article describes the lifecycle using **inner-loop** and **outer-loop** concepts:

- **Inner loop**: DataOps, experimentation, evaluation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)
- **Outer loop**: deployment, inferencing and monitoring, feedback loop. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

The Well-Architected article reinforces that successful lifecycle management requires:

- repeatable processes,
- automation,
- deployment pipelines,
- monitoring,
- and model maintenance to prevent drift or decayed relevance. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

### Lifecycle study notes

You should think of a language model app as moving through these phases:

#### A. Planning / design

Define use case, choose model pattern, determine data needs, and decide where orchestration and grounding happen. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

#### B. Data preparation

For GenAI, this can include not just training data but also **grounding data** for RAG. Reproducibility and versioning matter. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

#### C. Experimentation

You test prompts, chunking strategies, embedding choices, search configurations, and model variants. The GenAIOps architecture article explicitly calls out experimentation across prompts, chunking, index design, embedding models, and query patterns. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

#### D. Evaluation

Evaluation needs metrics appropriate to the workload. For RAG and prompting, Microsoft lists metrics such as **groundedness**, **relevance**, **coherence**, and **fluency**. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

#### E. Deployment

Deployment in GenAIOps includes more than the model. It can also include the orchestrator, gateways, data stores, and index changes. Microsoft recommends rigorous testing and controlled rollouts such as canary or blue-green strategies. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

#### F. Monitoring and learning from production

Operational monitoring tracks latency, token usage, error rates, and system health. Product-quality monitoring tracks answer quality, drift, relevance, and safety. Microsoft specifically calls out the need to monitor metrics like latency, token usage, 429 errors, and content safety. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

---

## 6) Unit theme: Explore available tools and frameworks to implement GenAIOps

The module title suggests this unit focuses on the practical toolchain.

Across the linked Microsoft pages, the repeatedly emphasized tools and services are:

- **Microsoft Foundry / Foundry model catalog** for discovering and evaluating models. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [Baseline M...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-landing-zone)
- **Azure OpenAI** for model deployment and inferencing. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models), [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)
- **Azure AI Search** for RAG, including classic hybrid search and newer agentic retrieval patterns. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)
- **Azure Machine Learning pipelines** for orchestration, experimentation, and evaluation workflows. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)
- **Prompt flow** (called out in the Well-Architected article) for prototyping, iterating, and prompt engineering. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)
- **MLflow** for experiment tracking. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)
- **GitHub / source control** as part of operational workflow and reproducibility. The surrounding learning path explicitly includes prompt versioning and GitHub-based operational practices. [Operationa...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/paths/operationalize-gen-ai-apps/)
- **Azure Monitor** and broader observability tooling for performance and health. [Baseline M...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-landing-zone), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

### Architecture note

The Foundry landing-zone architecture page makes it clear that production GenAIOps involves platform decisions too: networking, private endpoints, shared platform resources, monitoring resources, change management, and governance boundaries between workload teams and platform teams. [Baseline M...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-landing-zone)

### RAG-specific tooling notes

The Azure AI Search RAG article says Azure AI Search supports two approaches:

1. **Agentic retrieval (preview)** for LLM-assisted query planning and multi-source retrieval.
2. **Classic RAG pattern** using hybrid search and semantic ranking. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

It also maps tools to common RAG challenges:

- user query understanding,
- multi-source access,
- token constraints,
- response-time expectations,
- security and governance. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

---

## 7) Unit theme: Exercise — compare language models from the model catalog

Even without the exact lab steps, the available Microsoft content strongly suggests what this exercise is trying to teach: **don’t compare models only by name — compare them by capability, deployment fit, and workload requirements**. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models), [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

### What the exercise is likely training you to observe

- Which models support **text only** vs **text + image**. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- Which models are optimized for **general chat** vs **deep reasoning**. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- Context window and max output token differences. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- Whether a model is **preview** or suitable for production. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- Which **regions** and **deployment types** are available. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models), [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

### Practical interpretation

A good comparison is not “Model A is newer than Model B.”  
A good comparison is:

- “Model A supports multimodal input and structured outputs.”
- “Model B is a better fit for reasoning-heavy workloads.”
- “This deployment type gives better throughput consistency.”
- “This option fits data-processing-location requirements better.” [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models), [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

---

## 8) Summary of the module in one pass

This module teaches that **planning a GenAIOps solution starts before coding the app**. You must first define the use case, decide whether the solution needs prompting, RAG, or fine-tuning, choose a model that matches the task and operational constraints, and then design the lifecycle that will let the application be evaluated, deployed, governed, and improved safely over time. The broader Microsoft Learn guidance reinforces that GenAIOps extends traditional MLOps to include prompts, orchestrators, grounding systems, retrieval design, and production observability. [Plan and p...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/training/modules/plan-prepare-genaiops/), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

---

# Vocabulary

## GenAIOps

Operational practices for generative AI solutions, extending MLOps to cover generative patterns such as prompting, fine-tuning, orchestration, evaluation, deployment, and monitoring. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

## LLMOps

The set of practices, techniques, and tools that facilitate development, integration, testing, deployment, and monitoring of LLM-based applications. [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

## RAG (Retrieval-Augmented Generation)

A pattern that improves LLM responses by grounding them in proprietary or domain-specific information retrieved from data sources. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

## Grounding data

The domain-specific data supplied to the model—often through retrieval—to make responses more relevant and accurate for the organization or use case. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

## Orchestrator

The component that manages logic around prompt construction, data retrieval, back-end calls, and model invocation in a generative AI system. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)

## Prompting

Designing inputs to language models, including system prompts and user prompts, to shape response quality and behavior. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Fine-tuning

Further training a foundation model for a more specific domain or task. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Embeddings

Numerical vector representations of text used for similarity comparison and retrieval scenarios. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)

## Semantic ranking

A search capability that re-scores results based on meaning, not just literal keyword overlap. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

## Agentic retrieval

An Azure AI Search approach that uses LLM-assisted query planning, parallel subqueries, and structured results optimized for agent use. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

## Inner loop

The iterative build-and-improve phases of a GenAI system, such as DataOps, experimentation, and evaluation. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Outer loop

Post-build stages such as deployment, inferencing, monitoring, and feedback collection. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Global Standard deployment

A recommended Azure OpenAI starting deployment option that offers broad availability and high default quota, with possible latency variability at very high volume. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

## Provisioned deployment

An Azure OpenAI deployment type with reserved processing capacity for high and predictable throughput. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

## Groundedness

A quality metric used in RAG-style systems to measure whether the answer is supported by the retrieved context. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Document-level security trimming

A security approach in search/retrieval systems that ensures only authorized content is returned. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

---

# Useful examples

## Example 1: Choosing a model for a support chatbot

If you’re building an internal support chatbot that answers employee questions from policy docs, the likely pattern is **RAG**. You would need:

- a response model for the chat experience,
- an embeddings/search layer for retrieval,
- and evaluation focused on **groundedness**, **relevance**, and **coherence**.  
    This lines up with the RAG and evaluation guidance in Microsoft Learn. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops)

## Example 2: Choosing a deployment type

If the app is just being piloted and needs broad availability, **Global Standard** is the recommended starting point. If the app becomes high-volume and requires more predictable throughput, you should evaluate **Provisioned** deployment. If the app does overnight bulk summarization, **Global Batch** may be a better match. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)

## Example 3: When to use a reasoning model

If the workload involves complex troubleshooting, code reasoning, or math-heavy workflows, the **o-series reasoning models** are specifically described as stronger in problem-solving. If the app is simpler, a general-purpose model may be enough. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)

## Example 4: Why RAG beats “paste the whole doc”

The Azure AI Search article explains that token limits are real, and retrieval should return only the most relevant chunks, not giant document dumps. So a better design is to chunk and retrieve the best matching sections instead of sending entire manuals into the prompt. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview)

## Example 5: What GenAIOps changes compared to classic MLOps

Traditional MLOps might stop at model training, deployment, and monitoring. GenAIOps adds responsibilities for:

- prompt versioning,
- orchestrator logic,
- grounding data freshness,
- index maintenance,
- chat quality evaluation,
- and production tracing/observability. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops)

---

# Best “study in one page” takeaway

If you want the shortest possible retention summary:

- **Use case first**: Decide whether you need prompting, RAG, or fine-tuning. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)
- **Model second**: Choose based on modality, reasoning, context window, output token needs, region availability, and production readiness. [Azure Open...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/concepts/models)
- **Operations always**: Design for evaluation, deployment, monitoring, access control, and reproducibility from the beginning. [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/guide/genaiops-for-mlops), [learn.microsoft.com](https://learn.microsoft.com/en-us/azure/well-architected/ai/mlops-genaiops), [Baseline M...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/architecture/ai-ml/architecture/baseline-microsoft-foundry-landing-zone)
- **RAG is usually the enterprise winner** when answers must be grounded in internal content. [RAG and Ge...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/search/retrieval-augmented-generation-overview), [learn.microsoft.com](https://learn.microsoft.com/en-us/ai/playbook/technology-guidance/generative-ai/)
- **Deployment type matters** just as much as model family when scale, latency, and data-processing location are important. [Understand...soft Learn | Learn.Microsoft.com](https://learn.microsoft.com/en-us/azure/ai-services/openai/how-to/deployment-types)
