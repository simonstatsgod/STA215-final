Overview

This README provides the insstuctions for accessing, cleaning, and analyzing the song dataset used in my final prokect for STA 215 (Fall 2024). It details the cleaning process, the steps of operationalization of the variables, and how the data is used in R Studio.

Data Access

The dataset is stored in raw_data.csv. Load the data into your R environment with:

Precursors: 

Ensure the following packages are installed:

readr
dplyr
tidytext
tidyverse
ggplot2
haven
forcats
psych
xtable
You can install missing packages using R Studio itself
----------------------------------------------------------------------------------------------------------------------------------------
r
Copy code
raw_data <- read_csv("raw_data.csv")
First make sure the file is in your working directory before running this command.

Data Cleaning and Preprocessing
-----------------------------------------------------------------------------------------------------------------------------------------
Step 1: Checking for Missing Values
Use summary(raw_data) to check for missing values in the dataset.

Step 2: Transformations
This is used for clarity on the graphs depicting key signatures, and understanding which key is most prevalent.

Key Signature: Standardized key signatures (e.g., "Db" to "D♭") for clarity:
r
Copy code
raw_data$key <- fct_recode(as.factor(raw_data$key), 
                           "C" = "C", "D♭" = "Db", "D" = "D", "E♭" = "Eb", 
                           "E" = "E", "F" = "F", "G♭" = "Gb", "G" = "G", 
                           "A♭" = "Ab", "A" = "A", "B♭" = "Bb", "B" = "B")
Log Transformation: Applied a log transformation to the streams variable to improve data distribution:
r
Copy code
raw_data$lnstreams <- log(raw_data$streams)

Step 3. Final Cleaning
Next, we check for duplicates, outliers, and ensure all categorical variables are correctly formatted using summary(raw_data).

Operationalization of Variables

song_length: Song length in seconds (continuous).
streams: Number of streams (continuous).
key: Musical key signature (categorical: e.g., "C", "D♭").
artist: Artist name (categorical).
producer: Producer name (categorical).
-----------------------------------------------------------------------------------------------------------------------------------------
Analysis

Step 1. Descriptive Statistics
Get basic statistics for all variables:

Copy code
summary(raw_data)
Or use the describe() function from the psych package for a detailed summary:

Copy code
library(psych)
describe(raw_data)

Step 2. Visualizations
Boxplot (Song Length vs Key Signature):

Copy code
boxplot(song_length ~ key, data = raw_data, xlab = "Key Signature", ylab = "Song Length (seconds)", col = "lightblue")
Scatter Plot (Song Length vs Log of Streams):

Copy code
plot(raw_data$song_length, raw_data$lnstreams, xlab = "Song Length", ylab = "Log of Streams", col = "blue", pch = 16)

Step 3. Statistical Tests
Chi-Square Test: For categorical variables like artist and producer:

Copy code
contingency_table <- table(raw_data$artist, raw_data$producer)
chisq_test <- chisq.test(contingency_table)
print(chisq_test)
Requirements
-----------------------------------------------------------------------------------------------------------------------------------------

Thank you for reading this!
