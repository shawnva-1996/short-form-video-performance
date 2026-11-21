# Predicting Short-Form Video Performance

Machine learning capstone · Singapore Institute of Technology · Python

Can a creator predict a video's engagement before publishing it? I evaluated models on an external dataset of 60 short videos, split evenly between Singapore and international samples. The most important decision was which features a creator could actually know at prediction time.

| Experiment | Recorded result | Interpretation |
| --- | --- | --- |
| [Earlier regression](7c_final_modelling_results.csv) | Random forest test R² of 0.37 | The model used the retrospective viral label among its most influential features. This score overstates what a creator could know before publishing. |
| [Regression after removing leakage](8_modelling_results_TRUE_PREDICTION.csv) | All six test R² scores were negative. Random forest scored −4.72. | On this split, even the best recorded model performed worse than the held-out mean benchmark. |
| [High versus low classification](9_train_model_results_explained.md) | Seven videos flagged, one true positive, six false positives | The test set contained only one high-performing video. More independently labelled positives would be needed to assess recall reliably. |

The original run compared classifiers on the ten-video holdout to choose the winner. Its score is therefore exploratory. The revised training script selects the model using cross-validation on the training data before evaluating it on the holdout. A fresh external sample would be needed for independent confirmation.

On 21 November 2026, rerunning the documented classification steps from the committed feature CSV produced 48 training rows and 12 holdout rows. Training-fold selection chose random forest (mean weighted F1 0.8083). On the holdout it flagged 11 videos, including the sole high performer and ten false positives. It classified 2 of 12 correctly. The earlier report used ten holdout rows, so the scores from the two runs cannot be compared directly. Neither run supports deployment.

## Inspect or rerun the final experiments

The result tables above can be inspected directly. To rerun training from the committed feature CSV, use a fresh Python environment and run these commands from the repository root. The training scripts write results into the working directory.

```sh
python -m pip install pandas numpy scikit-learn lightgbm matplotlib seaborn
python 7_train_model_v2.py
python 9_high_vs_low_perf.py
python 9_train_model.py
```

The numbered files preserve successive experiments. [Feature removal](5_remove_data_leakage.py) records an earlier cleaning pass. The [final regression script](7_train_model_v2.py) shows the inputs and exclusions used for the reported regression results.

## Optional audio and semantic features

To rebuild transcripts and semantic features from the original media, install FFmpeg and the additional Python packages used by the [extraction script](7_extract_audio_transcription_and_feature_engineering.py).

```sh
python -m pip install openai-whisper moviepy requests tqdm emoji google-generativeai
```

Supply `GEMINI_API_KEY` through the process environment before running the extraction script. It processes audio with Whisper locally and sends transcript text to Gemini for semantic analysis. The saved features allow the final experiments to run without that API call.
