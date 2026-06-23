# Lab: Configure Model Performance Monitoring and Data Drift Detection

## Key Concepts
- Data Drift Monitoring
- Prediction Drift
- Data Quality Monitoring
- Feature Attribution Drift
- Model Performance Monitoring
- Baseline vs Production Datasets
- Azure ML Monitoring Jobs

## Definitions

### Data Drift
A change in the statistical distribution of production data compared to the training dataset used to build the model.

### Prediction Drift
A change in the distribution of model predictions over time.

### Data Quality Monitoring
Validation of production data for issues such as null values, out-of-range values, and schema changes.

### Feature Attribution Drift
A change in feature importance patterns between training-time and production-time data.

### Baseline Dataset
The reference dataset used during model training that serves as the comparison point for monitoring.

### Production Dataset
The incoming real-world data used by the deployed model after deployment.

## Examples

- Monitoring a deployed Diabetes MLflow model.
- Comparing production data against the Diabetes MLTable training dataset.
- Detecting increased null-value rates in production.
- Identifying changes in accuracy, precision, and recall.
- Monitoring whether prediction distributions differ from historical patterns.

## Step-by-Step Processes

### Configure Data Drift Monitoring
1. Navigate to the Monitoring section in Azure Machine Learning.
2. Create a new monitor.
3. Select the deployed model.
4. Select the deployment that collects production data.
5. Specify the model task type.
6. Configure compute resources.
7. Configure the monitoring schedule.
8. Add a baseline data source.
9. Select the reference MLTable used during training.
10. Configure monitoring signals.
11. Define drift thresholds.
12. Save the monitor.

### Configure Feature Drift Monitoring
1. Select the production data asset.
2. Select the reference dataset.
3. Monitor all features.
4. Configure drift metrics.
5. Set acceptable thresholds.
6. Generate alerts when thresholds are exceeded.

### Configure Prediction Drift Monitoring
1. Select production prediction data.
2. Select the reference prediction dataset.
3. Compare prediction distributions.
4. Configure drift metrics.
5. Configure alert thresholds.

### Configure Data Quality Monitoring
1. Select production and reference datasets.
2. Monitor null-value rates.
3. Monitor out-of-bounds values.
4. Track schema consistency.
5. Trigger alerts when thresholds are exceeded.

### Configure Model Performance Monitoring
1. Select production data.
2. Select reference data.
3. Track classification metrics.
4. Monitor accuracy.
5. Monitor precision.
6. Monitor recall.
7. Generate notifications when performance degrades.

## Code Blocks

No code examples were included in the transcript.

## Summary
This lab demonstrates how to configure monitoring for deployed Azure Machine Learning models. Monitoring capabilities include feature drift detection, prediction drift detection, data quality validation, feature attribution drift, and model performance monitoring. These monitoring signals help identify situations where production behavior differs from training-time expectations and indicate when model retraining may be required.

## Actionable Takeaways

- Always establish a training dataset as a baseline reference.
- Store production inference data for comparison.
- Enable data drift monitoring on deployed endpoints.
- Track prediction distributions over time.
- Monitor null values and schema changes.
- Monitor feature importance drift.
- Configure alerts for model performance degradation.
- Retrain models when significant drift is detected.

## Tags

#AI300 #AzureML #ModelMonitoring #DataDrift #PredictionDrift #FeatureAttributionDrift #MLOps #MLflow #ModelPerformance

## Backlink Suggestions

- [[Understanding Data Drift]]
- [[Model Monitoring]]
- [[Azure ML Endpoints]]
- [[MLflow Deployment]]
- [[Model Retraining]]
- [[Feature Importance]]
- [[Responsible AI Dashboard]]
- [[MLOps Lifecycle]]
