# Student Average Score Prediction -- Model Evaluation Notes

## 1. Understanding the project

The project predicts a student's average score using two input features:

-   **Hours of study:** the number of hours a student spends studying.
-   **Days of absence:** the number of days a student is absent from
    school.

The target (the value the model predicts) is the **average score**.

Example:

  Input / output              Example value
  ------------------------- ---------------
  Hours of study                    6 hours
  Days of absence                    3 days
  Predicted average score          78 marks

The model uses the relationship it learns from previous student records
to estimate the average score of a student.

## 2. Is this regression or classification?

This is a **regression problem** because the model predicts a numerical
value, such as 78 marks.

-   Regression predicts numbers, such as scores, prices or temperatures.
-   Classification predicts categories, such as Pass/Fail or
    High/Medium/Low risk.

Therefore, regression metrics should be the main evaluation measures for
this project.

## 3. Regression evaluation metrics

Regression metrics compare the model's predicted scores with the
students' actual scores.

### 3.1 Mean Absolute Error (MAE)

**Meaning:** MAE tells us the average number of marks by which the
model's predictions differ from the actual scores.

Formula:

`MAE = average of |actual score - predicted score|`

Example:

  Student     Actual score   Predicted score   Absolute error
  --------- -------------- ----------------- ----------------
  A                     80                75                5
  B                     60                65                5
  C                     90                80               10

`MAE = (5 + 5 + 10) / 3 = 6.67`

**Interpretation:** The model's predictions are approximately 6.67 marks
away from the actual scores on average.

**How to explain it:** "MAE tells us the average number of marks our
model misses when predicting student scores."

-   Lower MAE is better.
-   MAE of 0 means the predictions are perfect.

### 3.2 Mean Squared Error (MSE)

**Meaning:** MSE calculates the average of the squared prediction
errors. Squaring makes large errors count more heavily.

Formula:

`MSE = average of (actual score - predicted score)^2`

Using the same example:

  Student     Error   Squared error
  --------- ------- ---------------
  A               5              25
  B               5              25
  C              10             100

`MSE = (25 + 25 + 100) / 3 = 50`

**Interpretation:** The model's mean squared error is 50 marks-squared.

**How to explain it:** "MSE measures prediction errors after squaring
them, so large mistakes receive more punishment."

-   Lower MSE is better.
-   MSE of 0 means perfect predictions.
-   MSE is expressed in squared units, so it is less intuitive to
    explain directly in marks.

### 3.3 Root Mean Squared Error (RMSE)

**Meaning:** RMSE is the square root of MSE. It measures error on the
same scale as the original scores.

Formula:

`RMSE = square root of MSE`

Using the example:

`RMSE = sqrt(50) = 7.07`

**Interpretation:** The model's RMSE is approximately 7.07 marks.

**How to explain it:** "RMSE tells us the typical size of prediction
errors in marks, while giving greater weight to large errors."

-   Lower RMSE is better.
-   RMSE of 0 means perfect predictions.
-   RMSE is in the same unit as the target score.

### 3.4 R-squared (R²)

**Meaning:** R² tells us how much of the variation in actual student
scores is explained by the model.

Example:

`R² = 0.85`

`0.85 × 100 = 85%`

**Interpretation:** The model explains approximately 85% of the
variation in student scores in the evaluated data.

**How to explain it:** "R-squared tells us how much of the differences
in student scores our model can explain."

General guide:

    R² value General meaning
  ---------- ---------------------------------------------------------
        1.00 Perfect fit on the evaluated data
        0.85 Explains 85% of score variation
        0.70 Explains 70% of score variation
        0.50 Explains 50% of score variation
        0.00 No improvement over always predicting the average score
     Below 0 Worse than always predicting the average score

A higher R² is generally better, but it does not mean the model is "that
percentage accurate". R² should be considered together with MAE and
RMSE.

## 4. Classification evaluation metrics

Precision, Recall, Accuracy, Confusion Matrix and ROC-AUC are mainly
used for **classification**, not direct numerical score prediction.

They can be used in this project if we create a separate Pass/Fail
classification task. For example, we could define:

-   Pass: average score of 50 or above.
-   Fail: average score below 50.

This is only an example threshold. Use the actual grading rule supplied
by the school or institution.

If we convert the actual scores and predicted scores into Pass/Fail
categories, we can compare those categories using classification
metrics. However, this loses information about the exact number of marks
predicted.

### 4.1 Confusion Matrix

A confusion matrix compares actual classes with predicted classes.

Example for 100 students:

                         Predicted Pass       Predicted Fail
  --------------- --------------------- --------------------
  Actually Pass      35 (True Positive)   5 (False Negative)
  Actually Fail     10 (False Positive)   50 (True Negative)

The four terms mean:

-   **True Positive (TP):** The model predicted Pass, and the student
    actually passed.
-   **True Negative (TN):** The model predicted Fail, and the student
    actually failed.
-   **False Positive (FP):** The model predicted Pass, but the student
    actually failed.
-   **False Negative (FN):** The model predicted Fail, but the student
    actually passed.

The word "false" means the prediction was wrong.

### 4.2 Accuracy

**Meaning:** Accuracy is the proportion of all predictions that were
correct.

Formula:

`Accuracy = (TP + TN) / (TP + TN + FP + FN)`

Using the example:

`Accuracy = (35 + 50) / 100 = 0.85 = 85%`

**Interpretation:** The model correctly classified 85 out of 100
students.

Accuracy can be misleading when one class is much more common than the
other. Therefore, also check precision, recall and the confusion matrix.

### 4.3 Precision

**Meaning:** Precision answers: Of all students the model predicted
would pass, how many actually passed?

Formula:

`Precision = TP / (TP + FP)`

Using the example:

`Precision = 35 / (35 + 10) = 0.7778 = 77.78%`

**Interpretation:** Of all students predicted to pass, approximately
77.78% actually passed.

**How to remember it:** Precision checks how trustworthy the model's
positive predictions are.

### 4.4 Recall

**Meaning:** Recall answers: Of all students who actually passed, how
many did the model correctly identify as passing?

Formula:

`Recall = TP / (TP + FN)`

Using the example:

`Recall = 35 / (35 + 5) = 0.875 = 87.5%`

**Interpretation:** The model identified 87.5% of all students who
actually passed.

**How to remember it:** Recall checks how many of the actual positive
cases the model finds.

### 4.5 Precision versus Recall

  -----------------------------------------------------------------------
  Precision                           Recall
  ----------------------------------- -----------------------------------
  Starts with the students predicted  Starts with the students who
  to pass.                            actually passed.

  Asks how many predicted passes were Asks how many actual passes were
  correct.                            identified.

  Measures the trustworthiness of     Measures how completely positive
  positive predictions.               cases are found.
  -----------------------------------------------------------------------

Memory tip:

-   **Precision:** When the model says "Pass", how often is it right?
-   **Recall:** Of the students who really passed, how many did it find?

The positive class must be stated clearly. In a student-support system,
it may be more useful to define **"at risk of failing"** as the positive
class. Then recall answers: "Of all students who were actually at risk,
how many did the model identify?"

### 4.6 ROC Curve and AUC

**ROC** means Receiver Operating Characteristic. It is a graph used for
binary classification.

The ROC curve plots:

-   **True Positive Rate (TPR):** This is Recall.
-   **False Positive Rate (FPR):** The proportion of actual negative
    cases incorrectly predicted as positive.

The curve shows how the classifier behaves at different decision
thresholds.

**AUC** means Area Under the Curve. It summarises how well the model
separates the two classes.

General guide:

     AUC value General interpretation
  ------------ -------------------------------
          1.00 Perfect separation
          0.90 Excellent separation
          0.80 Good separation
          0.70 Fair separation
          0.50 No better than random ranking
    Below 0.50 Ranking is worse than random

These descriptions are general guides, not strict rules for every
application.

**How to explain it:** "The ROC curve shows the trade-off between
finding positive cases and incorrectly identifying negative cases at
different thresholds. AUC summarises how well the model separates the
two classes."

ROC-AUC is not a direct evaluation metric for a regression score output.
It requires class labels and a classification score or probability.

## 5. Summary: Which metrics should this project use?

  -----------------------------------------------------------------------
  Metric            Task              What it tells us  Better result
  ----------------- ----------------- ----------------- -----------------
  MAE               Regression        Average absolute  Lower
                                      error in marks    

  MSE               Regression        Average squared   Lower
                                      error             

  RMSE              Regression        Error size in     Lower
                                      marks, penalising 
                                      large errors      

  R²                Regression        Variation in      Generally higher
                                      scores explained  

  Confusion Matrix  Classification    Counts of correct More correct,
                                      and incorrect     fewer incorrect
                                      class predictions 

  Accuracy          Classification    Overall           Higher
                                      proportion        
                                      correctly         
                                      classified        

  Precision         Classification    Correctness of    Higher, when
                                      positive          positive
                                      predictions       predictions must
                                                        be trustworthy

  Recall            Classification    Proportion of     Higher, when
                                      actual positives  missing positive
                                      identified        cases is costly

  ROC-AUC           Classification    Ability to        Higher
                                      rank/separate two 
                                      classes           
  -----------------------------------------------------------------------

### Recommended evaluation for this project

**Main task: Predict average score**

Use MAE, MSE, RMSE and R². These directly evaluate how close the
predicted numerical scores are to the actual scores.

**Optional additional task: Identify students at risk of failing**

If the project also needs to identify students who may fail, define a
justified Pass/Fail or at-risk threshold and evaluate that
classification task using a confusion matrix, accuracy, precision,
recall and ROC-AUC.

Do not report classification metrics as if they directly measure the
accuracy of the numerical score predictions.

## 6. How to explain the project to classmates

"My model predicts a student's average score using hours of study and
days of absence. Since the output is a numerical score, this is a
regression problem. I evaluate the model mainly using MAE, MSE, RMSE and
R-squared. MAE and RMSE help me understand prediction errors in marks,
MSE gives more weight to large errors, and R-squared shows how much
variation in scores the model explains. If I also convert scores into
Pass and Fail categories to identify students at risk, I can use
classification metrics such as the confusion matrix, accuracy,
precision, recall and ROC-AUC."

## 7. Important reminders

1.  Compare predictions with actual scores from data that was not used
    to train the model (the test set).
2.  Use the same test data when comparing different regression models.
3.  Lower MAE, MSE and RMSE are better; higher R² is generally better.
4.  Do not describe R² as percentage accuracy.
5.  Do not use precision, recall, confusion matrix or ROC-AUC directly
    on continuous score predictions.
6.  If creating a Pass/Fail task, state the threshold and which class is
    treated as positive.
7.  A good evaluation should explain what the metric means in the
    context of student scores, not just list its value.

------------------------------------------------------------------------

**Project:** Student Average Score Prediction\
**Input features:** Hours of study and days of absence\
**Target variable:** Average score
