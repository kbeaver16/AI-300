# 📚 Azure Machine Learning — AutoML Study Notes

---

## Page 1: What Is Automated ML (AutoML)?

**Source:** `concept-automated-ml`

### Overview

AutoML automates the time-consuming, iterative tasks of machine learning model development. It allows data scientists, analysts, and developers to build ML models **at scale** with efficiency and productivity while maintaining model quality. It is grounded in Microsoft Research breakthroughs.

### How AutoML Works

1. AutoML creates **many pipelines in parallel**, each trying different algorithms + parameters.
2. It iterates through ML algorithms paired with **feature selections**.
3. Each iteration produces a model with a **training score** against a target metric.
4. The process stops when **exit criteria** are met.
5. The output is a **Python serialized `.pkl` file** containing the model and data preprocessing steps.

**Workflow Steps:**

1. Identify the ML problem (classification, regression, forecasting, computer vision, NLP)
2. Choose code-first (SDK v2 / CLI v2) or no-code (Azure ML Studio)
3. Specify labeled training data source
4. Configure AutoML parameters (iterations, hyperparameters, featurization, metrics)
5. Submit the training job
6. Review results

---

### Task Types

#### 🔵 Classification

- Supervised learning where the model predicts **which category** new data falls into.
- Uses deep neural network text featurizers.
- **Examples:** Fraud detection, handwriting recognition, object detection.
- **Real notebook example:** _Bank Marketing_ — predict if a client will subscribe to a term deposit.

#### 🔵 Regression

- Supervised learning predicting **numerical output values** based on independent predictors.
- Estimates the relationship among independent predictor variables.
- **Examples:** Predicting automobile price based on gas mileage and safety rating.
- **Real notebook example:** _Hardware Performance_ — predict CPU performance.

#### 🔵 Time-Series Forecasting

- Builds predictions about future values over time.
- Treats the problem as a **multivariate regression** — past values are "pivoted" into dimensions.
- Advanced features:
    - Holiday detection and featurization
    - Auto-ARIMA, Prophet, ForecastTCN (DNN learners)
    - Rolling-origin cross validation
    - Configurable lags and rolling window aggregate features
- **Real notebook example:** _Energy Demand_ — predict future electricity consumption.

#### 🔵 Computer Vision

- Generates models trained on **image data**.
- Integrates with Azure ML data labeling.

|Task|Description|
|---|---|
|Multi-class image classification|One label per image (e.g., cat OR dog OR duck)|
|Multi-label image classification|Multiple labels per image (e.g., cat AND dog)|
|Object detection|Locate objects with a **bounding box**|
|Instance segmentation|Identify objects at the **pixel level** using polygons|

#### 🔵 Natural Language Processing (NLP)

- Generates models trained on **text data**.
- Supports: Text classification, named entity recognition (NER).
- Uses the latest **pre-trained BERT models**.
- Multilingual support for **104 languages**.
- Distributed training via **Horovod**.

---

### Training, Validation & Test Data

- AutoML uses **validation data** during training to tune hyperparameters.
- This can introduce **model evaluation bias** (the model keeps fitting to validation data).
- **Test data** (separate from validation) evaluates the **final recommended model** to confirm there is no bias.
- > ⚠️ Test dataset evaluation is a **preview feature** and may change.
    

---

### Feature Engineering & Featurization

- **Featurization** = the combined application of feature engineering, scaling, and normalization.
- AutoML applies featurization **automatically** — but it can be customized.
- Featurization steps become **part of the model** — so when you use the model for predictions, the same steps are applied automatically to input data.
- Customization options: Encoding, transforms, enabled via Studio or SDK.

---

### Ensemble Models

- AutoML supports **ensemble models** (enabled by default).
- Ensemble = combining multiple models rather than relying on one.
- Two methods:
    - **Voting:** Weighted average of predicted class probabilities (classification) or targets (regression).
    - **Stacking:** Combines heterogeneous models; trains a **meta-model** on their outputs.
        - Default meta-model: `LogisticRegression` (classification), `ElasticNet` (regression/forecasting).
- Uses the **Caruana Ensemble Selection Algorithm** to pick which models to include.
    - Initializes with up to 5 best models, verifies they are within **5% threshold** of best score.
    - Adds models iteratively if they improve the ensemble score.

---

### AutoML & ONNX

- AutoML models can be converted to **ONNX format** for cross-platform deployment.
- ONNX enables use in **C# apps** (via ML.NET) without recoding or REST endpoint latency.
- The **ONNX Runtime** also supports C# API inference.

---

---

## Page 2: Data Featurization in AutoML

**Source:** `how-to-configure-auto-features` (SDK v1 reference, concepts apply broadly)

### Feature Engineering vs. Featurization

- **Feature engineering:** Using domain knowledge to create features that help ML algorithms learn better.
- **Featurization:** The umbrella term in AutoML for scaling + normalization + feature engineering applied automatically.

---

### Featurization Configuration Options

|Setting|Behavior|
|---|---|
|`"featurization": 'auto'`|Default — data guardrails + featurization applied automatically|
|`"featurization": 'off'`|No automatic featurization steps|
|`"featurization": FeaturizationConfig`|Custom featurization steps specified|

---

### Automatic Featurization Steps

|Step|Description|
|---|---|
|Drop high cardinality / no variance features|Removes columns that are all-missing, all-same, or are IDs/hashes/GUIDs|
|Impute missing values|Numeric → column mean; Categorical → most frequent value|
|Generate more features|DateTime → year, month, day, hour, etc.; Text → unigrams, bigrams, trigrams|
|Transform & encode|Numeric with few unique values → categorical; Low cardinality → one-hot encoding; High cardinality → one-hot-hash encoding|
|Word embeddings|Text tokens → sentence vectors via pretrained model|
|Cluster distance|k-means clustering; produces distance-to-centroid features|

> ⚠️ Only steps marked with `*` (impute missing values, impute with mean/most frequent, generate more features) are supported in **ONNX format exports**.

---

### Scaling & Normalization Techniques

|Technique|Description|
|---|---|
|StandardScaler|Removes mean, scales to unit variance|
|MinMaxScaler|Scales each feature between its column min and max|
|MaxAbsScaler|Scales by maximum absolute value|
|RobustScaler|Scales by quantile range (robust to outliers)|
|PCA|Linear dimensionality reduction via Singular Value Decomposition|
|TruncatedSVD|Linear dimensionality reduction — works on sparse matrices without centering|
|Normalizer|Rescales each sample so its L1 or L2 norm equals 1|

**Example:** A run using `LogisticRegression` might show `RobustScaler` in the pipeline steps:

```python
[('RobustScaler', RobustScaler(quantile_range=[10, 90])),
 ('LogisticRegression', LogisticRegression(C=0.184, class_weight='balanced'))]
```

---

### Data Guardrails

Data guardrails **detect and correct potential data problems** before training.

**Three states:**

|State|Meaning|
|---|---|
|Passed|No problems detected|
|Done|Changes were applied — review them|
|Alerted|Problem detected but couldn't be fixed — user action required|

**Supported Guardrails:**

|Guardrail|What it checks|
|---|---|
|Missing feature values imputation|Detects and imputes missing values|
|High cardinality feature detection|Detects and handles ID-like features|
|Validation split handling|Auto-splits data if < 20,000 rows (uses cross-validation)|
|Class balancing detection|Warns if classes are imbalanced|
|Memory issues detection|Detects if forecasting config may cause OOM|
|Frequency detection|Ensures time series data points align with detected frequency|
|Cross validation|Applied for datasets < 20,000 samples|
|Short series handling|Drops or pads series with insufficient data points|

---

### Customizing Featurization

```python
featurization_config = FeaturizationConfig()
featurization_config.blocked_transformers = ['LabelEncoder']
featurization_config.drop_columns = ['aspiration', 'stroke']
featurization_config.add_column_purpose('engine-size', 'Numeric')
featurization_config.add_column_purpose('body-style', 'CategoricalHash')
featurization_config.add_transformer_params('Imputer', ['engine-size'], {"strategy": "median"})
featurization_config.add_transformer_params('HashOneHotEncoder', [], {"number_of_bits": 3})
```

Supported customizations:

|Customization|Definition|
|---|---|
|Column purpose update|Override the auto-detected feature type for a column|
|Transformer parameter update|Update parameters for a transformer (e.g., Imputer strategy)|
|Block transformers|Prevent specific transformers from being used|

---

### BERT Integration in AutoML

- **BERT** (Bidirectional Encoder Representations from Transformers) is used in the AutoML featurization layer for text and other columns.
- To enable: Set `enable_dnn: True` and use a **GPU compute** (min `STANDARD_NC6`).
- AutoML compares BERT against a bag-of-words baseline; if BERT wins, it's used for full featurization.
- Supports ~100 languages; selects the appropriate BERT model:
    - English → English BERT
    - German → German BERT
    - Others → Multilingual BERT

**Example — triggering German BERT:**

```python
featurization_config = FeaturizationConfig(dataset_language='deu')
automl_settings = {
    "featurization": featurization_config,
    "enable_dnn": True
}
```

---

### Featurization Transparency

You can inspect what featurization was applied:

```python
# Get engineered feature names
fitted_model.named_steps['timeseriestransformer'].get_engineered_feature_names()
# Output: ['A', 'B', 'A_WASNULL', 'B_WASNULL', 'year', 'half', 'quarter', 'month', ...]

# Get featurization summary
fitted_model.named_steps['timeseriestransformer'].get_featurization_summary()
# Output: [{'RawFeatureName': 'A', 'TypeDetected': 'Numeric', 'Dropped': 'No',
#           'EngineeredFeatureCount': 2, 'Transformations': ['MeanImputer', 'ImputationMarker']}, ...]
```

---

---

## Page 3: Forecasting Methods in AutoML

**Source:** `concept-automl-forecasting-methods`

### Two Categories of Forecasting Models

#### 1. Time Series Models

- Use **historical values of the target** to predict future values.
- Formula: `y(t+1) = f(y_t, y_t-1, ..., y_t-s)`
- Where `s` = amount of history used (also a parameter).
- **Recursive** — forecasts are generated one period at a time; previous forecasts feed back into the model.
- **Risk:** Recursive models **compound prediction error** over many future periods.

#### 2. Regression (Explanatory) Models

- Use **predictor variables** to forecast target values.
- Formula: `y = g(price, day_of_week, holiday)`
- **Direct forecasters** — generate all forecasts up to the horizon in **one attempt** (no compounding error).
- **Limitation:** Predictor variables must be **known in advance** up to the forecast horizon.
- Can be augmented with **lagged quantities** (historical values of target/predictors) → creates a hybrid model.

**Example:** Forecasting orange juice demand:

- Time series: `demand(t+1) = f(demand_t, demand_t-1, ...)`
- Regression: `demand = g(price, day_of_week, is_holiday)` — but price must be known in the future!

---

### Supported Forecasting Models

|Category|Models|
|---|---|
|Time series models|Naive, Seasonal Naive, Average, Seasonal Average, ARIMA(X), Exponential Smoothing|
|Regression models|Linear SGD, LARS LASSO, Elastic Net, Prophet, K Nearest Neighbors, Decision Tree, Random Forest, Extremely Randomized Trees, Gradient Boosted Trees, LightGBM, XGBoost, **TCNForecaster**|
|Ensemble|Soft voting ensemble using Caruana Ensemble Selection Algorithm|

> ⚠️ **TCNForecaster** (deep neural network) **cannot** be included in ensembles. Stack ensemble is **disabled by default** for forecasting to prevent overfitting.

---

### Model Grouping

When a dataset has **multiple time series**, AutoML uses:

- **1:1 (each series in a separate group):** Naive, Seasonal Naive, Average, Seasonal Average, ARIMA, ARIMAX, Prophet, Exponential Smoothing.
- **N:1 (all series in one group):** LightGBM, XGBoost, Random Forest, etc.

**Example:** A retail dataset with products JUICE1 and BREAD3 → ARIMA trains one model per SKU, LightGBM trains one model for all SKUs.

---

### Data Format: "Wide" Tabular

**Simple format:**

|timestamp|quantity|
|---|---|
|2012-01-01|100|
|2012-01-02|97|

**Complex format (multiple series):**

|timestamp|SKU|price|advertised|quantity|
|---|---|---|---|---|
|2012-01-01|JUICE1|3.5|0|100|
|2012-01-01|BREAD3|5.76|0|47|

The `SKU` column is a **time series ID column** — grouping by it produces individual series.

---

### Missing Data Handling

AutoML handles two types of missing data:

1. **Missing cell values** — a value is absent in the table.
2. **Missing rows** — an entire observation row is absent for an expected time point.

**Default imputation:**

|Column type|Default method|
|---|---|
|Target|Forward fill (last observation carried forward)|
|Numeric feature|Median value|
|Categorical feature|Implicit — missing becomes its own category during encoding|

---

### Automated Feature Engineering (Forecasting)

**Default engineered features:**

- Calendar features from the time index (day of week, month, quarter, etc.)
- Categorical features from time series IDs
- Numeric encoding of categorical types

**Optional engineered features:**

- Holiday indicator features (region-specific)
- **Lags** of target quantity
- **Lags** of feature columns
- **Rolling window aggregations** (e.g., rolling 7-day average of target)
- **STL decomposition** (Seasonal and Trend Decomposition using Loess)

---

### Nonstationary Time Series

- **Nonstationary:** A series where mean and/or variance **change over time** (e.g., an upward-trending series).
- AutoML automatically detects nonstationarity and applies a **differencing transform** to mitigate it.
- **First differences:** `Δy_t = y_t - y_t-1` — converts a trending series into a roughly stationary one.

---

### Data Length Requirements

Minimum training observations (with user-provided validation data):

> `T = H + max(l_max, s_window) + 1` Where: H = forecast horizon, l_max = max lag order, s_window = rolling window size

With cross-validation:

> `T = 2H + (n_CV - 1) × n_step + max(l_max, s_window) + 1`

---

---

## Page 4: AutoML for Computer Vision

**Source:** `how-to-auto-train-image-models`

### Task Types & SDK Syntax

|Task|SDK v2 syntax|
|---|---|
|Multi-class image classification|`image_classification()`|
|Multi-label image classification|`image_classification_multilabel()`|
|Object detection|`image_object_detection()`|
|Instance segmentation|`image_instance_segmentation()`|

### Data Format: JSONL

Training data must be provided as **labeled images** in **JSONL (JSON Lines)** format, then wrapped in an **MLTable**.

**Classification JSONL example:**

```json
{"image_url": "azureml://.../Image_01.png",
 "image_details": {"format": "png", "width": "2230px", "height": "4356px"},
 "label": "cat"}
```

**Object detection JSONL example:**

```json
{"image_url": "azureml://.../Image_01.png",
 "label": {"label": "cat", "topX": "1", "topY": "0", "bottomX": "0", "bottomY": "1", "isCrowd": "true"}}
```

> ⚠️ Minimum 10 images to submit a job; **10–15 samples per label** recommended as a minimum for a usable model.

---

### Compute Requirements

- **GPU required** — NC or ND families.
- Multi-GPU VMs speed up training via parallelism.
- Multi-node compute enables faster hyperparameter tuning via parallelism.
- Recommended for best performance: `STANDARD_NC24r` or `STANDARD_NC24rs_V3` (RDMA capable).

---

### Primary Metrics

|Task|Primary metric|
|---|---|
|Image classification|Accuracy|
|Image classification multi-label|Intersection over Union (IoU)|
|Object detection|Mean Average Precision (mAP)|
|Instance segmentation|Mean Average Precision (mAP)|

---

### Experiment Modes

#### 1. Automatic Sweeps (AutoMode) — recommended starting point

- Set `max_trials > 1` and **do not** define a search space.
- AutoML automatically determines which hyperparameter region to explore.
- Running **10–20 trials** works well on many datasets.

#### 2. Individual Trials

- Directly control the model architecture via `model_name` parameter.
- Uses default hyperparameters for the chosen architecture.

#### 3. Manual Sweeps

- Define a search space of model architectures and hyperparameters.
- Specify a sampling method and early termination policy.

---

### Supported Model Architectures

**Legacy models:**

|Task|Architectures|
|---|---|
|Image classification|MobileNetV2, ResNet (18/34/50/101/152), ResNeSt, SE-ResNeXt50, ViT (small/base/large)|
|Object detection|YOLOv5*, Faster RCNN ResNet FPN, RetinaNet ResNet FPN|
|Instance segmentation|MaskRCNN ResNet FPN|

**HuggingFace / MMDetection models (new backend):**

|Task|Examples|
|---|---|
|Classification|BEiT (`microsoft/beit-base-patch16-224`), ViT (`google/vit-base-patch16-224`), DeiT, SwinV2|
|Object detection|Sparse R-CNN, Deformable DETR, VFNet, YOLOF|
|Instance segmentation|Mask R-CNN (`mmd-3x-mask-rcnn_swin-t-p4-w7_fpn_1x_coco`)|

---

### Hyperparameter Sweep Sampling Methods

|Method|Syntax|
|---|---|
|Random|`random`|
|Grid|`grid`|
|Bayesian|`bayesian`|

> Only `random` and `grid` support **conditional hyperparameter spaces**.

---

### Early Termination Policies

|Policy|Purpose|
|---|---|
|Bandit policy|Terminates runs that fall outside a slack factor from the best run|
|Median stopping policy|Terminates runs performing below the median of completed runs|
|Truncation selection policy|Cancels a percentage of the lowest-performing runs at each interval|

---

### Data Augmentation

AutoML automatically applies augmentation — no hyperparameter controls it.

|Task|Training augmentation|
|---|---|
|Image classification|Random resize/crop, horizontal flip, color jitter, normalization|
|Object detection / segmentation|Random crop around bounding boxes, expand, horizontal flip, normalization|
|YOLOv5|Mosaic, random affine (rotation, translation, scale, shear), horizontal flip|

**Disable augmentations (object detection / segmentation only):**

```yaml
training_parameters:
  advanced_settings: >
    {"apply_mosaic_for_yolo": false}
    {"apply_automl_train_augmentations": false}
```

---

### Incremental Training

- After a job finishes, you can further train using the **checkpoint** from a prior job.
- Pass the `checkpoint_run_id` parameter to continue training from that point.

---

---

## Page 5: AutoML for NLP

**Source:** `how-to-auto-train-nlp-models`

### Supported NLP Task Types

|Task|SDK v2 syntax|Description|
|---|---|---|
|Multiclass text classification|`text_classification()`|One class per sample (e.g., "Comedy")|
|Multilabel text classification|`text_classification_multilabel()`|Multiple classes per sample (e.g., "Comedy, Romance")|
|Named Entity Recognition (NER)|`text_ner()`|Tag tokens in sequences (e.g., extract person names, locations)|

---

### Data Formats

**Multiclass / Multilabel — CSV format:**

```
text,labels
"I love watching Chicago Bulls games.","NBA"
"Tom Brady is a great player.","NFL"
```

**Multilabel — multiple labels:**

```
"The four most popular leagues are NFL, MLB, NBA and NHL","football,baseball,basketball,hockey"
```

**NER — CoNLL format (two columns, space-separated):**

```
Hudson   B-loc
Square   I-loc
is       O
a        O
famous   O
place    O
```

---

### Data Validation Rules

|Task|Requirement|
|---|---|
|All tasks|At least **50 training samples**|
|Multiclass & Multilabel|Same columns, same order, same data types in train and validation sets; at least **2 unique labels**|
|NER|No empty lines at start; format `{token} {label}` with single space; labels must start with `I-`, `B-`, or `O`; exactly one empty line between samples|

---

### BERT Language Models Used

|Task|Language|Model|
|---|---|---|
|All NLP tasks|English (`eng`)|English BERT (cased or uncased)|
|All NLP tasks|German (`deu`)|German BERT|
|All NLP tasks|Other (104 languages)|Multilingual BERT|

**Example — setting dataset language:**

```python
text_classification_job.set_featurization(dataset_language='eng')
```

---

### Long-Range Text

- If >10% of samples contain **more than 128 tokens**, enable the long-range text feature.
- Requires `NC6` or higher GPU (NCv3 or NDv2 series).

---

### Supported Pretrained NLP Model Algorithms

|Model family|Examples|
|---|---|
|BERT|`bert-base-cased`, `bert-large-uncased`, `bert-base-multilingual-cased`|
|DistilBERT|`distilbert-base-cased`, `distilbert-base-uncased`|
|RoBERTa|`roberta-base`, `roberta-large`, `distilroberta-base`|
|XLM-RoBERTa|`xlm-roberta-base`, `xlm-roberta-large`|
|XLNet|`xlnet-base-cased`, `xlnet-large-cased`|
|HuggingFace (preview)|Any model from HuggingFace Hub (e.g., `microsoft/deberta-large-mnli`)|

> Large models (`bert-large`, `roberta-large`, etc.) are more performant but require **more GPU memory and time**. NDv2-series VMs recommended.

---

### Hyperparameters for NLP Sweeping

|Parameter|Description|
|---|---|
|`gradient_accumulation_steps`|Number of backward passes before optimizer step — effectively increases batch size|
|`learning_rate`|Initial learning rate (float in [0, 1])|
|`learning_rate_scheduler`|Type of LR scheduler (`linear`, `cosine`, `cosine_with_restarts`, `polynomial`, `constant`)|
|`model_name`|Which pretrained model to use|
|`number_of_epochs`|Training epochs|
|`training_batch_size`|Batch size during training|
|`warmup_ratio`|Fraction of training steps used for LR warmup|
|`weight_decay`|L2 regularization parameter|

**Example sweep config (YAML):**

```yaml
limits:
  timeout_minutes: 120
  max_trials: 4
  max_concurrent_trials: 2
sweep:
  sampling_algorithm: grid
  early_termination:
    type: bandit
    evaluation_interval: 10
    slack_factor: 0.2
search_space:
  - model_name:
      type: choice
      values: [bert_base_cased, roberta_base]
    number_of_epochs:
      type: choice
      values: [3, 4]
```

---

### Distributed Training

AutoML NLP supports **distributed training** on multi-GPU clusters:

```python
max_concurrent_iterations = number_of_vms
enable_distributed_dnn_training = True
```

- Scheduled in **powers of two** (e.g., 1, 2, 4, 8 VMs).
- Maximum: **32 VMs**.
- Unlike other tasks, NLP only supports **hold-out validation** (not cross-validation).

---

---

## 📖 Master Vocabulary Section

|Term|Definition|Example|
|---|---|---|
|**AutoML**|Automated Machine Learning — automatically trains and tunes ML models|Azure ML running 50 pipeline iterations to find the best fraud detection model|
|**Pipeline**|A sequence of data processing and model training steps chained together|`RobustScaler → LogisticRegression`|
|**Exit criteria**|Conditions that stop the AutoML training loop|Time limit reached or target accuracy achieved|
|**Featurization**|The combined process of feature engineering + scaling + normalization|Converting a DateTime column into year, month, day, hour features|
|**Feature engineering**|Creating new informative features from raw data using domain knowledge|Extracting "day of week" from a timestamp column|
|**Imputation**|Filling in missing values with statistical estimates|Replacing missing age values with the column mean|
|**High cardinality**|A feature with many unique values (like IDs or hashes)|A user_id column with millions of unique values — typically dropped|
|**One-hot encoding**|Converting a categorical variable into binary columns, one per category|`color: [red, blue, green]` → `color_red, color_blue, color_green`|
|**Ensemble model**|A model combining predictions from multiple individual models|Voting across LogisticRegression, LightGBM, and RandomForest predictions|
|**Voting ensemble**|Ensemble method using weighted average of predicted probabilities|Final prediction = 0.4 × Model_A + 0.35 × Model_B + 0.25 × Model_C|
|**Stacking ensemble**|Combines heterogeneous models using a meta-model trained on their outputs|Training ElasticNet on the outputs of LightGBM, RandomForest, and XGBoost|
|**Meta-model**|The second-level model in stacking that learns from base model outputs|ElasticNet (for regression) or LogisticRegression (for classification)|
|**ONNX**|Open Neural Network Exchange — cross-platform model format|Exporting an Azure ML model to run in a C# application via ML.NET|
|**Supervised learning**|Training a model on labeled input-output pairs|Training a model on images with "cat" and "dog" labels|
|**Classification**|Predicting a categorical label for input data|Spam vs. not spam email classification|
|**Regression**|Predicting a continuous numerical value|Predicting house price in dollars|
|**Time series**|A sequence of data points indexed in time order|Daily sales numbers recorded over 3 years|
|**Forecast horizon**|How far into the future predictions are made|Forecasting the next 30 days of sales|
|**Lag feature**|A historical value of a variable used as a predictor|Using demand from 2 days ago to predict today's demand|
|**Nonstationary**|A time series where mean/variance change over time|A trending upward sales series|
|**Differencing**|Subtracting consecutive observations to remove trends|`Δy_t = y_t - y_t-1` converts a trending series to stationary|
|**ARIMA**|Autoregressive Integrated Moving Average — a classical time series model|Forecasting monthly airline passenger counts|
|**Rolling window**|An aggregation over a sliding time window|7-day rolling average of website traffic|
|**STL decomposition**|Seasonal and Trend decomposition using Loess — separates trend, season, residual|Decomposing monthly retail sales into trend + holiday seasonality + noise|
|**Cross-validation**|Evaluating a model by training/testing across multiple data splits|10-fold CV for datasets with < 1,000 samples|
|**Data guardrail**|An automated check that detects and corrects data quality issues|AutoML detecting that class A has 95% of samples vs. class B's 5%|
|**Bounding box**|A rectangle around a detected object in an image|A box drawn around a detected car in a photo|
|**Instance segmentation**|Identifying objects at pixel level using polygons|Pixel-by-pixel outline of each car in a traffic image|
|**MLTable**|Azure ML's format for referencing structured training data|A YAML file pointing to image JSONL files for computer vision tasks|
|**JSONL**|JSON Lines — one JSON object per line, used for CV training data|Each line represents one labeled image with its URL and label|
|**BERT**|Bidirectional Encoder Representations from Transformers — pretrained NLP model|Encoding "The cat sat on the mat" into feature vectors|
|**NER**|Named Entity Recognition — tagging tokens with semantic labels|Tagging "New York" as B-loc, I-loc in text|
|**CoNLL format**|Two-column space-separated format for NER training data|`Hudson B-loc` / `Square I-loc`|
|**Hyperparameter**|A configuration value set before training that controls the learning process|Learning rate, number of epochs, batch size|
|**Hyperparameter sweep**|Searching over a range of hyperparameter values to find the best combination|Trying learning rates 0.0001–0.01 across 20 trials|
|**Bandit policy**|Early termination — stops runs that fall outside a slack factor from the best|Stops a trial if its accuracy is >20% below the best trial's at the same step|
|**Gradient accumulation**|Summing gradients over multiple batches before updating weights|Simulates a larger batch size on GPUs with limited memory|
|**Warmup ratio**|Fraction of training steps during which learning rate gradually increases|First 10% of steps ramp LR from 0 to the target, then decay|
|**Distributed training**|Training a model across multiple GPUs or machines simultaneously|4 VMs each training on a shard of the NLP dataset, using Horovod|
|**Horovod**|A distributed deep learning training framework|Synchronizing gradients across 8 GPU nodes during BERT fine-tuning|
|**MoE / Mosaic (YOLOv5)**|Data augmentation — stitching 4 images into one composite for training|Combining 4 different scene images into one training sample|
|**Data augmentation**|Artificially expanding training data with transformations|Flipping, cropping, color-jittering training images|
|**Incremental training**|Continuing training from a saved model checkpoint|Loading a YOLOv5 checkpoint after 50 epochs and training 20 more|
|**Caruana algorithm**|Ensemble selection algorithm that greedily adds models improving ensemble score|Starts with top 5 models, adds a 6th only if it improves the overall score|
|**PCA**|Principal Component Analysis — dimensionality reduction technique|Reducing 500 numeric features to 50 principal components|
|**StandardScaler**|Normalizes features to zero mean and unit variance|Height values scaled from [150cm, 200cm] to roughly [-1.5, 1.5]|
|**Predict_proba**|A model method returning class probability estimates|Returns `[0.1, 0.85, 0.05]` meaning 85% probability of class B|
|**ThresholdIng (NLP)**|Setting the cutoff probability for assigning a positive label in multilabel tasks|Threshold 0.3 → more labels assigned; Threshold 0.7 → fewer, more confident labels|

---

## 🔗 Pages Covered

1. **Main AutoML Concepts** — What is AutoML, task types, ensembles, ONNX
2. **Data Featurization** — Automatic featurization, scaling, data guardrails, BERT integration, transparency
3. **Forecasting Methods** — Time series vs. regression models, model grouping, missing data, feature engineering
4. **Computer Vision AutoML** — Task types, JSONL data format, model architectures, sweeping, augmentation
5. **NLP AutoML** — Text classification, NER, BERT models, CoNLL format, distributed training