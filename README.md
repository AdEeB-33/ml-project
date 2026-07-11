# Student Performance Prediction — ML Project Documentation

## Life Cycle of a Machine Learning Project

1. **Understanding the Problem Statement**
2. **Data Collection**
3. **Data Checks to Perform**
4. **Exploratory Data Analysis (EDA)**
5. **Data Pre-Processing**
6. **Model Training**
7. **Choose Best Model**

---

## 1. Problem Statement

This project studies how a student's performance (test scores) is influenced by variables such as:

- Gender
- Ethnicity
- Parental level of education
- Lunch type
- Test preparation course completion

---

## 2. Data Collection

**Dataset Source:** [Students Performance in Exams — Kaggle](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams?datasetId=74977)

- **Rows:** 1000
- **Columns:** 8

---

## 3. Model Training Pipeline (`model_trainer.py`)

The `ModelTrainer` class evaluates several regression algorithms and selects the best-performing one based on R² score.

### Models Evaluated
| Model | Notes |
|---|---|
| Random Forest Regressor | Ensemble of decision trees |
| Decision Tree Regressor | Baseline tree model |
| Gradient Boosting Regressor | Boosted trees |
| Linear Regression | Simple linear baseline |
| XGBRegressor | Extreme Gradient Boosting |
| CatBoosting Regressor | Handles categorical features well |
| AdaBoost Regressor | Adaptive boosting ensemble |

### Hyperparameter Grids
Each model (except Linear Regression) has an associated hyperparameter search space — e.g., `criterion`, `n_estimators`, `learning_rate`, `subsample`, `depth`, and `iterations` — used during tuning via `evaluate_models`.

### Selection Logic
1. Split `train_array` / `test_array` into `X_train, y_train, X_test, y_test`.
2. Run `evaluate_models()` to score every model/param combination.
3. Pick the model with the **highest test R² score**.
4. If the best score is **below 0.6**, raise a `CustomException("No best model found")`.
5. Save the winning model via `save_object()` to `artifacts/model.pkl`.
6. Return the final R² score on the test set.

### Error Handling
All logic is wrapped in a `try/except`, re-raising errors as a `CustomException(e, sys)` for consistent logging/traceability, alongside `logging.info()` calls at key steps.

---

## 4. Deployment — AWS Elastic Beanstalk

### Step-by-Step

1. Create a folder `.ebextensions/` containing `python.config`:
   ```yaml
   option_settings:
     "aws:elasticbeanstalk:container:python":
       WSGIPath: application:application
   ```
2. Create `application.py` — copy the full contents of your `app.py` into it.
3. Push the project to GitHub.
4. Log into the **AWS Console**.
5. Search for **Elastic Beanstalk** → open the application icon.
6. Click **Create Application**.
7. Set an **application name** and choose **Platform: Python**.
8. Select **Sample Application**, then click **Create Application**.
9. Search for **CodePipeline** in AWS → open it → **Create Pipeline** → name the pipeline → **Next**.
10. **Source Provider:** GitHub (Version 1) → connect your GitHub account and confirm.
11. Select the **repository name** and **branch (main)**, enable GitHub webhooks → **Next**.
12. **Build Provider:** Skip this step.
13. **Deploy Provider:** AWS Elastic Beanstalk → select region, application name, environment name → **Next**.
14. Review the pipeline settings → **Create Pipeline**.
15. **If an error occurs:** delete the `app.py` file (to avoid a naming conflict with `application.py`).
16. Deployment complete ✅

---

## 5. Deployment — Microsoft Azure

### Step-by-Step

1. Log into the **Azure Portal** → **Create a Resource**.
2. Choose **Create Web App**:
   - Subscription name
   - Resource group
   - App name
   - Publish: **Code**
   - Runtime stack: **Python 3.8**
   - Click **Next**
3. Under **Continuous Deployment**, enable it and configure with your GitHub account:
   - Organization: your GitHub username
   - Repository: your repo name
   - Branch: `main`
   - Click **Review + Create** → **Create**
4. Wait for provisioning, then reload your GitHub repository page.
5. A `.github/workflows/` folder will be auto-created in your repo — open it to view the generated `.yaml` workflow file.
6. Go to the **Actions** tab on GitHub to monitor the Azure deployment workflow run.
7. Deployment completes automatically once the workflow succeeds ✅

---

## 6. Tech Stack Notes

- **Backend/ML:** Flask
- **Frontend:** HTML, CSS
- **Model Serialization:** Pickle (`.pkl`) via `save_object()`

---

*Document generated from project notes — covers ML lifecycle, model training pipeline, and both AWS and Azure deployment workflows.*
