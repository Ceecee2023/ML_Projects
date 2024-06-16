# EEG Eye State Detection with XGBoost

This repository contains the code and analysis for the practical assignment on EEG eye state detection using the XGBoost algorithm. The dataset and the analysis explore the relationship between eye states (open/closed) and EEG signals, leveraging various data processing and machine learning techniques in R.

## Project Overview

Electroencephalograms (EEGs) measure electrical signals within the brain, which are indicative of neuronal activity. This project uses a dataset consisting of 117 seconds of continuous EEG measurements collected from a single person. The data includes annotations for whether the person's eyes were open or closed during the recording.

## Repository Contents

- **sensor_XGboost.pdf**: PDF output of the R Markdown file for easy viewing.
- **sensor_XGboost.Rmd**: R Markdown file containing the complete analysis.
- **sensor_XGboost.html**: HTML output of the R Markdown file for easy viewing.

## Getting Started

### Prerequisites

Make sure you have the following R packages installed:

```r
install.packages(c("xgboost", "eegkit", "forecast", "tseries", "caret", "ggplot2", "dplyr", "reshape2"))
```

For Mac users, you may also need to install `quartz` if you encounter errors related to X11:

```r
install.packages("quartz")
```

### Loading the Dataset

The dataset is sourced from the H2O library’s test data S3 bucket:

```r
eeg_url <- "https://h2o-public-test-data.s3.amazonaws.com/smalldata/eeg/eeg_eyestate_splits.csv"
eeg_data <- read.csv(eeg_url)
```

### Data Preprocessing

Add timestamps and split the data into training, validation, and test sets:

```r
Fs <- 117 / nrow(eeg_data)
eeg_data <- transform(eeg_data, ds = seq(0, 116.99999, by = Fs), eyeDetection = as.factor(eyeDetection))
```

### Exploratory Data Analysis

Perform initial data checks and generate plots:

```r
# Check for missing data
sum(is.na(eeg_data))

# Reshape the data for ggplot2
melt <- reshape2::melt(eeg_data %>% dplyr::select(-split), id.vars = c("eyeDetection", "ds"), variable.name = "Electrode", value.name = "microvolts")

# Plot the EEG data
ggplot2::ggplot(melt, ggplot2::aes(x = ds, y = microvolts, color = Electrode)) +
  ggplot2::geom_line() +
  ggplot2::ylim(3500, 5000) +
  ggplot2::geom_vline(ggplot2::aes(xintercept = ds), data = dplyr::filter(melt, eyeDetection == 1), alpha = 0.005)
```

### Modeling Eye State Detection

We built and evaluated two models: XGBoost and Logistic Regression. The XGBoost model showed better performance.

#### XGBoost Model

```r
# Prepare data
eeg_train_matrix <- as.matrix(dplyr::select(eeg_train, -eyeDetection, -ds))
eeg_train_labels <- as.numeric(eeg_train$eyeDetection) - 1

# Build the model
model <- xgboost(data = eeg_train_matrix, label = eeg_train_labels, nrounds = 100, max_depth = 4, eta = 0.1, objective = "binary:logistic")
```

#### Logistic Regression Model

```r
# Prepare data
train_data <- data.frame(eeg_train_matrix)
train_data$eyeDetection <- eeg_train_labels

# Train the model
train_control <- trainControl(method = "cv", number = 10)
logistic_model <- train(eyeDetection ~ ., data = train_data, method = "glm", family = "binomial", trControl = train_control)
```

#### Model Evaluation

The XGBoost model outperformed the logistic regression model in terms of accuracy:

```r
# Evaluate XGBoost
xgboost_predictions <- predict(model, eeg_test_matrix)
xgboost_cm <- confusionMatrix(factor(xgboost_predictions, levels = c(0, 1)), eeg_test_labels)

# Evaluate Logistic Regression
logistic_predictions <- predict(logistic_model, newdata = eeg_test_matrix)
logistic_cm <- confusionMatrix(logistic_predictions, eeg_test_labels)
```
