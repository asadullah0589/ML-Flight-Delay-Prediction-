# Flight Departure Delay Prediction

This project predicts flight departure delays using raw flight records, weather data, and temporal features. The workflow includes preprocessing raw DOCX files, merging weather features, calculating delay values, and training models for binary classification, multi-class classification, and regression.

## Project Purpose

The goal is to analyze flight delays and build predictive models that can estimate whether a flight is delayed and how long the delay will be. This matches the project instructions in the PDF and the project report.

## Dataset Overview

The project uses data located in the `Data` folder:

- `Data/Train/` — raw flight training records in `.docx` format
- `Data/Test/` — raw flight test records in `.docx` format
- `Data/Weather/` — weather files in `.xlsx` format

The repository also includes generated CSV files:

- `Full_trained.csv` — processed training data after merging weather features
- `Full_test.csv` — processed test data after merging weather features
- `train_df.csv` — combined train data before final preprocessing
- `test_df2.csv` — combined test data before final preprocessing
- `train_handled.csv` — cleaned train data after handling missing values
- `binaryclass.csv` — binary classification submission file
- `multiclass.csv` — multi-class classification submission file
- `regression.csv` — regression submission file

## Raw Data Reading Method

The notebook `i220589_preprocessing.ipynb` processes the raw DOCX flight files by:

1. Reading each `.docx` file from the Train or Test folder.
2. Extracting the text content from the document.
3. Cleaning the text and isolating the JSON-style record array inside it.
4. Loading the records using `json.loads()`.
5. Extracting structured fields such as:
   - flight type
   - status
   - departure fields
   - arrival fields
   - airline fields
   - flight fields
   - codeshare fields
6. Converting the extracted data into a pandas DataFrame.
7. Saving each file as a CSV in the same folder.

## Combining All Data Files

After each raw file is converted into a CSV, the project concatenates them into a single DataFrame:

- all Train CSVs are combined into `train_df`
- all Test CSVs are combined into `test_df2`

This is done by iterating through all CSV files, sorting them by file number, reading each one with pandas, and using `pd.concat()`.

## Time and Feature Engineering

The preprocessing notebook standardizes time fields and computes additional features.

### Time conversion

The following fields were converted to datetime format:

- Departure Scheduled Time
- Departure Estimated Time
- Departure Actual Time
- Departure Estimated Runway
- Departure Actual Runway
- Arrival Scheduled Time
- Arrival Estimated Time

### Derived features

The following features were extracted from scheduled departure time:

- Day of Week
- Hour of Day
- Month of Year
- Day of Month
- Minute of Hour
- Year

These temporal variables are useful because delays often depend on time-of-day, weekday patterns, and seasonal effects.

## Weather Data Processing

The weather folder contains Excel files. These are processed as follows:

1. Reading each `.xlsx` file.
2. Extracting monthly/day-wise weather values.
3. Organizing them into structured columns such as:
   - Max_Temperature
   - Avg_Temperature
   - Min_Temperature
   - Max_Dew_Point
   - Avg_Dew_Point
   - Min_Dew_Point
   - Max_Humidity
   - Avg_Humidity
   - Min_Humidity
   - Max_Wind_Speed
   - Avg_Wind_Speed
   - Min_Wind_Speed
   - Max_Pressure
   - Avg_Pressure
   - Min_Pressure
   - Total_Precipitation
4. Combining all processed weather files into one `weather.csv`.
5. Converting the weather date column to datetime.

## Weather + Flight Merge

The flight data and weather data are merged using the date field:

- flight `Date` is matched with weather `TIME`
- `pd.merge(..., how='left')` is used to combine them

This creates:

- `Full_trained.csv`
- `Full_test.csv`

These final datasets include both the flight metadata and the corresponding weather information for each row.

## Departure Delay Calculation

The project computes the target variable:

- `Departure Delay = (Departure Actual Time - Departure Scheduled Time)` in minutes

This is used as the main target for regression and also converted into classification labels.

## Classification Tasks

The training notebook creates three prediction setups:

### 1. Binary Classification

Target:

- `on-time` if `Departure Delay <= 0`
- `delayed` if `Departure Delay > 0`

Model used:

- Random Forest Classifier

Output file:

- `binaryclass.csv`

### 2. Multi-Class Classification

Categories:

- No Delay
- Short Delay
- Moderate Delay
- Long Delay

Rules used by the notebook:

- `delay <= 0` → No Delay
- `delay < 45` → Short Delay
- `delay <= 175` → Moderate Delay
- `delay > 175` → Long Delay

Output file:

- `multiclass.csv`

### 3. Regression

Target:

- exact delay duration in minutes

Model used:

- Random Forest Regressor

Output file:

- `regression.csv`

## Evaluation and Validation

The notebook evaluates the models using:

- accuracy
- precision
- recall
- F1-score
- confusion matrix
- MAE
- RMSE
- cross-validation
- feature importance plots

This matches the project requirements from the instructions PDF.

## What Happened in this Project

This project was completed end-to-end:

1. Raw DOCX flight files were read and converted to CSVs.
2. All Train and Test files were combined.
3. Missing values were handled.
4. Time columns were standardized.
5. Weather files were processed and merged.
6. Departure delay labels were computed.
7. Temporal features were engineered.
8. Binary, multi-class, and regression models were trained.
9. Predictions were saved in Kaggle-compatible submission formats.
10. The result files were generated and submitted.

## Generated Result Files

The repository contains the final output files:

- `binaryclass.csv`
- `multiclass.csv`
- `regression.csv`

These are the model output files for the three competitions.

## Observed Submission Results

The project result files and report indicate that model submissions were created successfully.

From the available result screenshots:

- Binary classification submission: successful
- Multi-class submission: successful
- Regression submission: successful

Observed score ranges from the result images include:

- Binary: public ~0.27904, private ~0.28717
- Multi-class: public ~0.019750, private ~0.019218
- Regression: public ~5943.20685, private ~6411.03849

## Repository Structure

```text
i220589_A_MLProject/
├── Data/
│   ├── Train/
│   ├── Test/
│   └── Weather/
├── Project Instructions.pdf
├── ML PROJECT_report.docx
├── i220589_preprocessing.ipynb
├── i220589_training.ipynb
├── Full_trained.csv
├── Full_test.csv
├── train_df.csv
├── test_df2.csv
├── train_handled.csv
├── binaryclass.csv
├── multiclass.csv
├── regression.csv
├── binaryclass_result.PNG
├── multiclass_result.PNG
├── regression_result.PNG
├── README.md
└── ...
```

## GitHub Notes

This project is structured in a way that is ready to be uploaded to GitHub. The important parts to keep are:

- source notebooks
- processed CSV files
- submission files
- README documentation
- final report and result images

## Final Summary

This project successfully demonstrates a complete machine learning workflow for flight departure delay prediction:

- raw data ingestion
- cleaning and validation
- feature engineering
- weather integration
- model training
- evaluation
- submission generation

It is ready for GitHub publishing with documentation and final outputs included.
