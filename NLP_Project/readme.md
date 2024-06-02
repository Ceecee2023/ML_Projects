# Electronic Medical Records Analysis with R

## Overview

This project involves exploratory data analysis, named entity recognition, and topic modeling of unstructured medical note free-text data derived from electronic medical records (EMR). The dataset used in this project is sourced from medical transcription data, providing a rich ground for natural language processing (NLP) techniques in R.

## Project Structure

- **Data Parsing**: Loading and exploring the dataset.
- **Text Processing**: Cleaning, tokenizing, and lemmatizing the text data.
- **Exploratory Data Analysis (EDA)**: Analyzing the dataset to uncover insights.
- **Topic Modeling**: Applying Latent Dirichlet Allocation (LDA) for unsupervised classification of the documents.

## Dependencies

Ensure you have the following R packages installed:

- `readr`
- `dplyr`
- `tidyr`
- `ggplot2`
- `tidytext`
- `textstem`
- `clinspacy`
- `topicmodels`
- `reshape2`

## Data Parsing

The dataset is obtained from the `clinspacy` library and includes various fields such as `note_id`, `description`, `medical_specialty`, `sample_name`, `transcription`, and `keywords`.

## Text Processing

Key steps in text processing include filtering the data to focus on specific medical specialties (Orthopedic, Radiology, and Surgery), tokenizing the text, and performing lemmatization to standardize words.

## Exploratory Data Analysis

### Unigram and Bigram Analysis

Analyzing the frequency and distribution of individual words (unigrams) and pairs of words (bigrams) within the text data provides insights into common terms and their usage across different medical specialties.

### Unique Words by Specialty

Identifying the unique unigrams and bigrams in the dataset helps in understanding the vocabulary specific to each medical specialty.

## Lemmatization

Lemmatization is applied to convert words to their base forms, which helps in reducing redundancy and improving the quality of the text analysis.

## Topic Modeling

Latent Dirichlet Allocation (LDA) is used to model topics within the dataset. This method identifies patterns in the text data, grouping words into topics that represent the underlying structure of the medical notes.

### Visualization of Topics

The top terms for each topic are visualized to illustrate the key themes and terminology associated with different medical specialties.

## Credits

This project draws heavily on techniques and examples from Julia Silge's *tidytext* textbook.
