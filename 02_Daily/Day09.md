# DAY 09 — HANDS-ON: TRAINING PIPELINES

## TASKS
- [ ] Run AutoML experiment 
- [ ] Compare results
- [ ] Build pipeline:
  - [ ] Data prep
  - [ ] Train
  - [ ] Evaluate
- [ ] Schedule pipeline

## CHECKPOINT
- [ ] Can build training workflow end-to-end 

## NOTES
# Detailed Notes: Run pipelines in Azure Machine Learning - Training | Microsoft Learn

## 1) Module at a glance

This module is a **Beginner** Microsoft Learn module for the **Data Scientist** role and focuses on using **components** to build pipelines in Azure Machine Learning, then running and scheduling those pipelines to automate machine learning workflows. The module has **7 units**, lists **no prerequisites**, and its learning objectives are to **create components**, **build an Azure Machine Learning pipeline**, and **run an Azure Machine Learning pipeline**. The unit structure is: **Introduction**, **Create components**, **Create a pipeline**, **Run a pipeline job**, **Exercise - Run a pipeline job**, **Module assessment**, and **Summary**. [[Run pipeli...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/), [[Run pipeli...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/), [[Run pipeli...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/)

The same module also appears in the learning path Operationalize machine learning models (MLOps) - Training | Microsoft Learn, where it sits alongside modules on experimentation, hyperparameter tuning, GitHub Actions, branch strategy, environments, and deployment. In that learning path, the broader stated goal is to learn the full MLOps lifecycle for machine learning models, including pipeline automation and production deployment. [[Operationa...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/), [[Operationa...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/)

---

## 2) What the module is really teaching

At a high level, the module teaches that instead of running one script at a time, you can break a machine learning workflow into multiple reusable steps and orchestrate them as a pipeline. The related concept article What are Azure Machine Learning pipelines? - Azure Machine Learning | Microsoft Learn states that an Azure Machine Learning pipeline is a workflow that automates a complete machine learning task, standardizes best practices, supports collaboration, and improves efficiency. It also emphasizes that pipelines break a task into manageable steps so different team members can own different parts, and that outputs from unchanged steps can be reused to improve efficiency and reduce cost. [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2), [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2)

This means the module is not just about “running jobs.” It is about moving toward **repeatable**, **modular**, and **automated** ML workflows—very much an MLOps mindset. The linked learning path reinforces that these automation skills sit inside the operationalization part of the ML lifecycle. [[Operationa...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/), [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2)

---

## 3) Components: the building blocks of a pipeline

A major concept in the module is the **component**. The related Microsoft Learn concept page What is a component - Azure Machine Learning | Microsoft Learn explains that components are the building blocks of a machine learning pipeline and that each step in a multistep workflow can be represented as a component. It gives the example of splitting a task into steps such as data ingestion, cleaning, preprocessing, feature engineering, training, and evaluation. It also explains that components define inputs and outputs, and that those interfaces let different steps connect together. [[What is a...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-component?view=azureml-api-2)

The SDK v2 article Create and run machine learning pipelines by using components with the Azure Machine Learning SDK v2 - Azure Machine Learning | Microsoft Learn adds more concrete detail: each component is a **self-contained piece of code** that completes one step in a pipeline, and for each component you define execution logic, its interface, metadata, runtime environment, and command. In the example pipeline, Microsoft creates three components: one to prepare data, one to train a model, and one to score the trained model. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2)

The CLI-focused page Create and run machine learning pipelines using components with the Azure Machine Learning CLI - Azure Machine Learning | Microsoft Learn gives the same architectural idea in YAML terms: a pipeline YAML describes the workflow, while component YAML files define metadata, interface, command, code, and environment for each component. This is useful because it highlights that the same logical design can be authored through different interfaces—CLI, SDK, or Studio. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)

---

## 4) Creating a pipeline

The module’s “Create a pipeline” unit is supported by several linked pages. The CLI article shows that a pipeline YAML describes how to break a full ML task into a multistep workflow, and that each step can be developed, tested, and optimized independently. It also explains that the pipeline definition describes how child steps connect to one another—for example, a training step can output a model that is then used by an evaluation step. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)

The SDK v2 page shows this same idea in Python: prepare data → train model → score model. It explicitly describes building a pipeline from components and submitting that pipeline job to Azure Machine Learning. It also notes that pipelines optimize workflow with **speed, portability, and reuse**. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2)

The Studio UI article Create and run machine learning pipelines by using components in Azure Machine Learning studio - Azure Machine Learning | Microsoft Learn explains the Designer-based experience. It says that you can create a pipeline in Studio using registered components, drag them onto the canvas, connect data and components, configure component settings, promote selected inputs to pipeline-level inputs, and then submit the pipeline through a wizard. It also notes that components offer better flexibility and reuse than building pipelines without components. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)

---

## 5) Running a pipeline job

The module explicitly includes a unit called “Run a pipeline job,” and the lab page Run pipelines in Azure Machine Learning | mslearn-mlops provides strong practical detail for that exercise. It states that rather than doing tasks individually, you can use pipelines to orchestrate steps needed to prepare data, run training scripts, and complete other tasks. In the lab, you provision a workspace and compute with the Azure CLI, clone lab materials, open a notebook called **Run a pipeline job.ipynb**, authenticate if needed, verify the notebook is using the **Python 3.10 - AzureML** kernel, and then run all cells. [[microsoftl....github.io]](https://microsoftlearning.github.io/mslearn-mlops/docs/04-run-pipelines.html)

The same lab page also makes the architecture concrete: the Azure Machine Learning workspace is the central place for managing resources and assets, and the lab uses the **Azure CLI** to provision the workspace and compute while using the **Python SDK** to run the pipeline job. That distinction is useful for studying because it shows that infrastructure setup and job orchestration can be split across tools. [[microsoftl....github.io]](https://microsoftlearning.github.io/mslearn-mlops/docs/04-run-pipelines.html)

---

## 6) Scheduling pipelines

The module description says you learn to “run and schedule” pipelines, and the most relevant supporting page here is Schedule machine learning pipeline jobs - Azure Machine Learning | Microsoft Learn. That page explains that you can schedule routine tasks such as retraining models or updating batch predictions by creating a schedule that associates a job with a trigger. The trigger can be based on either a **recurrence pattern** or a **cron expression**. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)

The same page also lists several limitations and operational details that are worth memorizing:

- Azure Machine Learning **v2 schedules do not support event-based triggers**. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- The Studio UI supports only **v2 schedules** and cannot list or access v1 schedules based on published pipelines or pipeline endpoints. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- You can create a schedule for an **unpublished pipeline**. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- The page covers how to **list**, **view**, **update**, **disable**, **enable**, and **delete** schedules. It also notes that **a schedule must be disabled before it can be deleted**. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- It further notes that schedules are billed based on the **number of schedules**, because each schedule creates a hosted logic app on the user’s behalf. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)

That means scheduling is an operational layer on top of the pipeline itself: once a pipeline produces satisfactory outputs, you can automate its execution over time. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)

---

## 7) Authoring options: CLI, SDK, and Studio

A useful study takeaway from the related pages is that Azure Machine Learning pipelines can be authored in multiple ways:

- **CLI v2** using YAML definitions. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)
- **Python SDK v2** using code to define components and compose them into a pipeline. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2)
- **Azure Machine Learning Studio Designer** using a drag-and-drop interface with registered components. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)

The CLI page says the pipeline YAML defines the pipeline, while component YAML files define the components; the SDK page demonstrates Python-based component definitions and pipeline composition; and the Studio page focuses on registering components and visually wiring them together. Together these pages make it clear that the **same conceptual pipeline model** is available across different authoring experiences. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)

---

## 8) Versioning and reuse

Reusability is a repeated theme. The Studio page says registered components support **automatic versioning**, which lets you update components while preserving older versions for pipelines that still depend on them. The CLI page similarly explains that the big value of components is reuse and sharing, and it describes registering components in the workspace so they can be referenced later. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)

This is important from an MLOps perspective: components are not just disposable code wrappers. They are reusable assets that can be versioned, shared, and composed into future workflows. [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)

---

# Vocabulary

- **Pipeline** — A workflow that automates a complete machine learning task by chaining together multiple steps. [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2), [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2)
- **Component** — A self-contained piece of code that completes one step in a machine learning pipeline and exposes defined inputs and outputs. [[What is a...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-component?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2)
- **Pipeline job** — A submitted run of a pipeline in Azure Machine Learning. [[Run pipeli...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/), [[microsoftl....github.io]](https://microsoftlearning.github.io/mslearn-mlops/docs/04-run-pipelines.html)
- **Workspace** — The central place in Azure Machine Learning for managing resources and assets used to train and manage models. [[microsoftl....github.io]](https://microsoftlearning.github.io/mslearn-mlops/docs/04-run-pipelines.html)
- **Compute target** — The compute resource used to run a job or a specific pipeline step. [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)
- **Registered component** — A component stored in the workspace for reuse and versioning. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)
- **Recurrence schedule** — A schedule based on a repeating interval such as minutes, hours, days, weeks, or months. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- **Cron expression** — A time expression used to define a custom recurring schedule. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- **Default compute** — The pipeline-level compute used unless a component explicitly specifies a different compute target. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)
- **MLOps** — The practice of operationalizing and automating machine learning workflows, including training, pipelines, CI/CD, and deployment. [[Operationa...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/paths/build-first-machine-operations-workflow/), [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2)

---

# Examples

## Example 1: Simple three-step training pipeline

A common Azure Machine Learning pipeline pattern is:

1. **Prepare data**
2. **Train model**
3. **Score/evaluate model**

Microsoft’s SDK v2 example explicitly uses those three steps in an image classification pipeline, while the CLI example similarly shows a multistep workflow where outputs from one step become inputs to the next. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2)

## Example 2: Studio-based reusable pipeline

In Azure Machine Learning Studio, you can register components in your workspace, drag those registered components into the Designer canvas, connect them, set inputs and outputs, and submit the pipeline through the Studio wizard. This is a visual equivalent of the CLI/SDK pipeline authoring flow. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)

## Example 3: Scheduled retraining

Once a pipeline job has acceptable outputs, you can create a schedule that runs it every day, week, or according to a cron expression. Microsoft Learn specifically gives retraining models and regularly updating batch predictions as examples of why you would schedule a pipeline. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)

---

# Study takeaways for you

For your current AI-300 / MLOps study track, the core exam-relevant ideas from this module are:

- Know that **pipelines automate ML workflows**. [[What are m...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-ml-pipelines?view=azureml-api-2)
- Know that **components are reusable steps** with inputs, outputs, code, command, and environment. [[What is a...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/concept-component?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2)
- Know that pipelines can be authored through **CLI, SDK, or Studio**. [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-cli?view=azureml-api-2), [[Create and...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipeline-python?view=azureml-api-2), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-create-component-pipelines-ui?view=azureml-api-2)
- Know that pipelines can be **run now** or **scheduled later** with recurrence or cron triggers. [[Run pipeli...soft Learn | Learn.Microsoft.com]](https://learn.microsoft.com/en-us/training/modules/run-pipelines-azure-machine-learning/), [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)
- Know the operational caveats around **v2 schedules**, especially that they do **not** support event-based triggers. [[learn.microsoft.com]](https://learn.microsoft.com/en-us/azure/machine-learning/how-to-schedule-pipeline-job?view=azureml-api-2)

If you want, I can turn this into your usual **Obsidian/OneNote study-note format** with checklists, “exam tips,” and a short “what to memorize” section.