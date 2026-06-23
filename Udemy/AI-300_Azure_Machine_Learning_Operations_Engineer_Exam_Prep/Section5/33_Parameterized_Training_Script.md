# Lab: Parameterized Training Scripts

## Key Concepts
- Runtime arguments
- Hyperparameter injection
- Reusable scripts
- Experiment comparison
- MLflow tracking

## Definitions
### Parameterized Script
A script that accepts configuration values as runtime parameters.

### Argument Parser
A Python utility used to capture values passed into a script.

### Regularization Parameter
A hyperparameter used to reduce model overfitting.

## Examples
- Passing regularization values of 0.1 and 0.5.
- Comparing model performance across training runs.
- Using the same script with different configurations.

## Step-by-Step Process
1. Create a training script.
2. Use argparse to define parameters.
3. Create a --reg_rate argument.
4. Set default values.
5. Replace hardcoded hyperparameters.
6. Execute the script with runtime arguments.
7. Log metrics via MLflow.
8. Compare experiment runs.
9. Evaluate model performance.
10. Save model artifacts.

## Code Blocks
```python
parser.add_argument(
    '--reg_rate',
    type=float,
    default=0.01
)
```

```bash
python diabetes_training.py --reg_rate 0.1
```

## Summary
Parameterized scripts remove hardcoded hyperparameters and allow the same script to be reused for multiple model-training experiments.

## Actionable Takeaways
- Avoid hardcoding hyperparameters.
- Use runtime arguments to improve flexibility.
- Compare multiple parameter combinations.
- Track every execution with MLflow.

## Tags
#AI300 #AzureML #TrainingScripts #Hyperparameters #MLflow

## Backlink Suggestions
- [[MLflow]]
- [[Experiment Tracking]]
- [[Hyperparameter Tuning]]
- [[Training Scripts]]
