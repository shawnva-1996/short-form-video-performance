# Held-out classification results

Selected model: logistic regression. Reported weighted F1: 0.475.

```text
                  precision    recall  f1-score   support

High Performance       0.14      1.00      0.25         1
 Low Performance       1.00      0.33      0.50         9

        accuracy                           0.40        10
       macro avg       0.57      0.67      0.38        10
    weighted avg       0.91      0.40      0.47        10

```

Confusion matrix:
[View the confusion matrix](final_confusion_matrix.png)

The test split contained one high-performing video and nine low-performing videos. The classifier flagged seven videos as high-performing. One was correct and six were false positives. Predicting the majority class for every video would have been correct nine times out of ten, while missing the sole high-performing video. With one positive example, recall is too uncertain to support a deployment decision.

The original run compared classifiers on the ten-video holdout to choose the winner. Its score is therefore exploratory. The revised training script selects the model using cross-validation on the training data before evaluating it on the holdout. A fresh external sample would be needed for independent confirmation.
