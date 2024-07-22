# Exploratory Data Analysis Using R
# “Survival from malignant melanoma” Dataset
## Table of Contents
- [Introduction](#introduction)
- [Data cleaning](#data-cleaning)
- [Summary statistics](#summary-statistics)
- [Correlation and Regression Analysis](#correlation-and-regression-analysis)
- [Two sample significance test grouped by sex](#two-sample-significance-test-grouped-by-sex)
- [Observations](#observations)
- [Recommendations](#recommendations)
- [References](#references)
## Introduction
This report presents the exploratory data analysis of the “Survival from malignant melanoma” data set using R (R Core Team. 2020). It is the measurements recorded on patients with malignant melanoma whose tumors were removed at the Department of Plastic Surgery, University Hospital of Odense, Denmark. This procedure was done through surgery and the records were kept from 1962 to 1977.
A total of 7 variables were recorded on 205 patients, according to the data provided.
These variables are:
Time – Survival time (days) after the operation, Status – The patients' status when the study ended (1 = died from malignant melanoma, 2 = still alive, 3 = died from other causes not related to malignant melanoma), Sex – The patients’ sex (1 = male, 0 = female), Age – Age (years) when the operation was done, Year – Year of operation, Thickness – Tumor thickness (mm), Ulcer – To show if ulcer was present or absent (1 = present, 0 = absent).
The data set was imported into R by loading the tidyverse package by (Wickham et al. 2019), ggplot2 package by (Wickham 2016), and ggthemes by (Arnold 2021). A data frame called melanoma_df to import the melanoma.csv file was created. For more information on how to use R programming, readers are directed to [‘R for Data Science (2e).’](https://r4ds.hadley.nz/) (Wickham et al.) and [‘Basic R Guide for NSC statistics.’](https://bookdown.org/dli/rguide/) (Deanna 2020).
Importing the data into R is implemented using:
```r
melanoma_df <- read_csv("C:/Users/HP/Documents/statistics_assignment/melanoma.csv")
```
##	Data cleaning
- The data was viewed to see what it contains by simply writing
  ```r
  melanoma_df
  ```
  According to the data collected, 205 patients were operated on, and the number of variables is 8.
- The unimportant column was removed.
  The column on the extreme left of the data is just the serial number and is unimportant in this analysis. Therefore, the column was removed:
  ```r
  melanoma_df <- melanoma_df %>% select(-1)
  ```
- The data types of variables status, sex, and ulcer were changed to factor.
  These variables are categorical/ nominal variables. Therefore, the data types were changed from double to factor. Since the data inputs in these columns were represented in numbers, the labels were changed to the actual inputs for easy data exploration.
  ```r
   melanoma_df <- melanoma_df %>%
      mutate(status = factor(status, levels = c(1,2,3), labels = c("Died from melanomaa",
                                                                   "Still alive", "non_melanoma death")),
      sex = factor(sex, levels = c(0,1), labels = c("Female", "Male")),
      ulcer = factor(ulcer, levels = c(0,1), labels = c("Absent", "Present")))
  ```
- The data was checked for null values and there were none.
  ```r
  sum(is.na(melanoma_df))
  ```

## Summary statistics
![image](https://github.com/user-attachments/assets/9abb920c-25b4-4f3a-9e26-27e052fe6f81)

#### Time
Boxplot of the time variable
```r
boxplot(melanoma_df$time,main = "Survival time boxplot",
        ylab = "Survival time (days)")
```
![image](https://github.com/user-attachments/assets/9e6f3687-ccc9-42c7-9a66-b94c9fe46dd1)

Histogram of time variable
```r
hist(melanoma_df$time,main = "Survival time (in days)",
        xlab = "Number of days")
```
![image](https://github.com/user-attachments/assets/92f9a049-aba5-44a2-bf68-2824351904d4)


From the summary statistics for time,
Minimum days survived since operation = 10 days. Maximum days survived since operation = 5565 days. 1st Quartile = 1525 days. 3rd Quartile = 3042 days. Median = 2005 days. Mean (Average days survived) = 2152.8 = 2153 days.
From the box plot, there is the presence of an outlier in the time variable. This is equally observed in the histogram. This outlier is likely the maximum value of time previously observed to be 5565 days in the summary statistics of time.
The histogram is multimodal with modes at approximately 750 days, 1750 days, and 3250 days. It is not a normal distribution.
#### Status
Barplot of status
```r
ggplot(melanoma_df, aes(x = status)) +
    geom_bar(width = 0.5) +
    labs(title = "Total patients by status")
```
![image](https://github.com/user-attachments/assets/a6233534-7864-4102-b7ab-1068d1e69bcb)

The status variable is categorical and has a factor type. Due to this, the summary is the total observations in each category.
The total number of deaths from melanoma = 57. The total still alive = 134. Total number of deaths from causes not related to their melanoma = 14.
#### Sex
Barplot of Sex
```r
ggplot(melanoma_df, aes(x = sex)) +
    geom_bar(width = 0.5) +
    labs(title = "Total patients by sex")
```
![image](https://github.com/user-attachments/assets/6239f5d3-1c13-4ed4-a1bf-5bb2e0342bed)

This is a categorical variable with ‘factor’ as the data type. Therefore, the summary statistics is the total for each category. Female = 126, Male = 79
#### Age
Boxplot of Age variable
```r
boxplot(melanoma_df$age,main = "Age of patients boxplot",
        ylab = "Age (in years)")
```
![image](https://github.com/user-attachments/assets/66a063c0-3edf-4aff-8cb7-aed581c3b6f5)

From the age boxplot, There is a presence of an outlier which is observed in the summary statistics of age as the minimum value of 4 years.
Histogram of Age variable
```r
hist(melanoma_df$age,main = "Age of patients (in days)",
        xlab = "Age")
```
![image](https://github.com/user-attachments/assets/311ee206-2b35-4df7-852c-daa85998f3cd)

The age histogram is a uni-modal distribution. It has one peak. The most occurring age is approximately 56 years.
From the summary statistics for age,
Minimum age = 4 years. Maximum age = 95 years. 1st Quartile = 42 years. 3rd Quartile = 65 years. The median of the patient’s age (Middle age) = 54 years. Average (Mean) = 52 years.
#### Year
Line chart of Number of operations performed by year between 1962 and 1977
```r
melanoma_df %>%
    ggplot(aes(x = year)) +
    geom_line(stat = "count", group = "year") +
    labs(title = "Number of operations by years (from 1962 to 1977)",
         x = "Year",
         y = "Numer of operations")
```
![image](https://github.com/user-attachments/assets/8a605ea3-02af-4338-947c-b5f304208635)

This record indicates that the highest number of melanoma surgeries were done in the year 1972 with a total of 41 surgeries. While 1963, 1975, and 1976 had no melanoma surgery records.

#### Thickness
Boxplot of Thickness
```r
boxplot(melanoma_df$thickness,main = "Tumour thickness boxplot",
        ylab = "Thickness (in mm)")
```
![image](https://github.com/user-attachments/assets/70e6a490-c255-4698-8126-47cc0d1f3ed3)

The boxplot indicates that outliers are present in the data on tumor thickness. The outliers are
```r
boxplot.stats(melanoma_df$thickness)$out
```
![image](https://github.com/user-attachments/assets/92e3e2e9-54e1-4ce2-ad9c-c5a71d4984bb)

Histogram of Thickness
```r
hist(melanoma_df$thickness,main = "Tumour thickness histogram",
        xlab = "Thickness (in mm)")
```
![image](https://github.com/user-attachments/assets/abbaa4d1-06e9-4be9-8223-e6591cdf947a)

The thickness histogram is skewed to the right and is not normally distributed.
The smallest tumor thickness (Minimum thickness) = 0.10 mm. 1st Quartile = 0.97 mm. 3rd Quartile = 3.56 mm. Median (Middle thickness) = 1.94 mm. Average tumor thickness (Mean) = 2.92 mm. The presence of outliers makes the maximum thickness 17.42 mm. Using a boxplot, the maximum is seen to be approximately 7.50 mm. The variance of tumor thickness = 8.758242. The standard deviation of thickness = 2.959433
#### Ulcer
A bar graph showing the number of patients in groups whose tumors had ulcerations, or not.
```r
ggplot(melanoma_df, aes(x = ulcer)) +
    geom_bar(width = 0.5) +
    labs(title = "Number of patients by tumour ulceration")
```
![image](https://github.com/user-attachments/assets/2a9c3230-22ea-4f31-a32b-acea7a2e8fe3)

From the ulcer barplot, out of the total patients operated on (205), those with ulcerations absent were more in number than those with ulcerations.
Ulcer is a categorical variable with ‘factor’ as the data type. Therefore, each category's total number of observations is computed as summary statistics.
Tumor ulcerations absent = 115, Tumor ulcerations present = 90

Extracting records of patients with no ulcerations
  ```r
  absent <- melanoma_df |>
    filter(ulcer == "Absent")
  ```
  Viewing the summary statistics of those with no ulcerations
  ```r
  summary(absent$thickness)
  ```
  ![image](https://github.com/user-attachments/assets/5929baa2-09d7-4df6-aa02-834be90c7894)

  Extracting records of patients with ulcerations
  ```r
  present <- melanoma_df |>
    filter(ulcer == "Present")
  ```
  Viewing the summary statistics of those with ulcerations
  ```r
  summary(present$thickness)
  ```
  ![image](https://github.com/user-attachments/assets/1009a8bf-60fb-4e77-9b6a-e1cae3b04bff)


## Correlation and Regression Analysis
#### Time ~ Thickness
- Using Pearson’s correlation, the relationship has a weak negative correlation of -0.2354087. This means as the tumor thickness gets bigger, the smaller the survival time a patient lives after the procedure.
  ```r
  cor(melanoma_df$time, melanoma_df$thickness, method = "pearson")
  ```
- A scatterplot of the relationship between time and thickness
  ```r
  plot(melanoma_df$thickness, melanoma_df$time,
       main = "A scatterplot of survivl time Vs tumour thickness",
       xlab = "Tumour thickness (mm)",
       ylab = "Time (days)")
  ```
  ![image](https://github.com/user-attachments/assets/60ba16f9-e6ef-48af-b184-bef1234c6a0a)

- Regression Analysis
  ```r
  LR_model = lm(formula = melanoma_df$time ~ melanoma_df$thickness)
  ```
  ```r
  summary(LR_model)
  ```
  ![image](https://github.com/user-attachments/assets/87e33b9e-2319-445e-8ad4-cd960e53947a)

  The line of best fit is: 
  time = 2413.41 – 89.25 thickness
  here, the y-intercept is 2413.41 when the thickness is equal to zero. The gradient is -89.25. For every unit increase in the value of thickness, time is predicted to decrease by 89.25, on average. The residual standard   error is 1093 and the R-squared is 0.05542. Due to the unreliability of this model, using it for any predictive analysis of the relationship should be done with caution.

#### Time ~ Age
- The relationship has a weak negative correlation of -0.3015179. This means that as age increases, a patient lives fewer days after the operation.
  ```r
  cor(melanoma_df$time, melanoma_df$age, method = "pearson")
  ```
- A scatterplot of the relationship between time and age
  ```r
  plot(melanoma_df$age, melanoma_df$time,
     main = "A scatterplot of survivl time Vs patient's age",
     xlab = "Age (in years)",
     ylab = "Time (days)")
  ```
  ![image](https://github.com/user-attachments/assets/48afba8e-4329-4cb5-bde5-44a0666e2337)

- Regression Analysis
  ```r
  LR_model_2 = lm(formula = melanoma_df$time ~ melanoma_df$age)
  ```
  ```r
  summary(LR_model_2)
  ```
  ![image](https://github.com/user-attachments/assets/96f389d4-4af4-4172-9c76-3dd3a287636a)

  The line of best fit is:
  time = 3217.45 – 20.29 age
  here, the y-intercept is 3217.45 when the age is equal to zero. The gradient is -20.29. For every unit increase in the value of age, time is predicted to decrease by 20.29, on average. The residual standard error is      1072 and the R-squared is 0.09091. Due to the unreliability of this model, using it for any predictive analysis of the relationship should be done with caution.

#### Thickness ~ Age
- The relationship is a weak positive correlation of 0.2124798. This means as the age increases, the bigger the tumor thickness.
  ```r
  cor(melanoma_df$thickness, melanoma_df$age, method = "pearson")
  ```
- A scatterplot of the relationship between thickness and age
  ```r
  plot(melanoma_df$age, melanoma_df$thickness,
     main = "A scatterplot of patient's age Vs tumour thickness",
     xlab = "Age (in years)",
     ylab = "Thickness (mm)")
  ```
  ![image](https://github.com/user-attachments/assets/c242693a-3375-426e-8c01-7fb080a21561)

- Regression Analysis
  ```r
  LR_model_3 = lm(formula = melanoma_df$thickness ~ melanoma_df$age)
  ```
  ```r
  summary(LR_model_3)
  ```
  ![image](https://github.com/user-attachments/assets/60f603c5-7b78-4a36-b00d-339ec51c8ecb)

  The line of best fit is:
  thickness = 0.94105 + 0.03772 age
  here, the y-intercept is 0.94105 when the age is equal to zero. The gradient is 0.03772. For every unit increase in the value of age value, thickness is predicted to increase by 0.03772, on average. The residual          standard error is 2.899 and the R-squared is 0.04515. Due to the unreliability of this model, using it for any predictive analysis of the relationship should be done with caution.

## Two sample significance test grouped by sex
Using t-test
Where Ho = Null hypothesis, H1 = alternative hypothesis
The default level of significance α = 0.05

#### Time grouped by sex
A boxplot of time by sex
```r
boxplot(melanoma_df$time ~ melanoma_df$sex, main = "Time (in days) by sex",
        xlab = "Sex of patient",
        ylab = "Survival time")
```
![image](https://github.com/user-attachments/assets/b7e49983-ea93-408e-8f27-a413ffc9a741)

Ho: The mean survival time after the operation is the same for both sexes

H1: The mean survival time after the operation is different in both sexes.

t-test
```r
t.test(melanoma_df$time ~ melanoma_df$sex)
```
![image](https://github.com/user-attachments/assets/14196b2f-f50f-4f10-8a1c-1e5d67a8ab62)

The p-value = 0.03868 which is smaller than α = 0.05. Therefore, we can reject Ho and conclude that there is evidence that the true mean survival time after the operation is different depending on whether the patient is male or female.

#### Thickness grouped by sex
A boxplot of thickness by sex
```r
boxplot(melanoma_df$thickness ~ melanoma_df$sex, main = "Thickness (in mm) by sex",
        xlab = "Sex of patient",
        ylab = "Tumour thickness")
```
![image](https://github.com/user-attachments/assets/c3d37f33-7280-4c9b-9533-55279db58e22)

Ho: The mean thickness of the tumor is the same for both sexes

H1: The mean thickness of the tumor is different in both sexes.

t-test
```r
t.test(melanoma_df$thickness ~ melanoma_df$sex)
```
![image](https://github.com/user-attachments/assets/783197f0-3e7c-4ef0-96e2-9902535b7348)

The p-value = 0.01009 which is smaller than α = 0.05. Therefore, we can reject Ho and conclude that there is evidence that the true mean thickness of the tumor is different depending on whether the patient is male or female.

#### Age grouped by sex
A boxplot of thickness by sex
```r
boxplot(melanoma_df$age ~ melanoma_df$sex, main = "Age (in years) by sex",
        xlab = "Sex of patient",
        ylab = "Patient's age")
```
![image](https://github.com/user-attachments/assets/d2ef61d9-0e98-4abb-8296-072398f61cbf)

Ho: The mean age of the patients is the same in both sexes

H1: The mean age of the patients is different depending in both sexes.

t-test
```r
t.test(melanoma_df$age ~ melanoma_df$sex)
```
![image](https://github.com/user-attachments/assets/e441e7b1-07e8-4b2d-8f76-e739c7f8bffd)

The p-value = 0.3408 which is greater than α = 0.05. Therefore, we cannot reject Ho. We can conclude that there is evidence that the true mean age of patients is the same in male and female.

## Observations
- The findings gathered from the data show that malignant melanoma is likely to have ulcerations present depending on its tumor thickness. This implies that a larger tumor thickness is likely ulcerated. The following summary statistics show this:

  A table to compare their outcomes
  |Summary statistics|Ulcerations present|Ulcerations absent|
  |------------------|-------------------|------------------|
  |Min|0.160|0.100|
  |1st Quatile|2.245|0.650|
  |Median|3.540|1.290|
  |Mean|4.336|1.811|
  |3rd Quatile|5.160|1.940|
  Max|17.420|14.660|

- More people survive after getting their melanoma removed which means that fewer people are likely   going to die due to their melanoma.
- The risk of dying from melanoma increases when the tumor becomes ulcerated due to the increase in thickness of the tumor.

## Recommendations
People need to seek proper medical care and advice at the sight of any skin infection as it could be melanoma. This will hinder the growth and spread of such infection. The possibility of ulceration, if detected early, will decrease and cured without the risk of fatality.

## References
- R Core Team. 2020. R: A language and environment for statistics computing. R Foundation for Statistical Computing, Vienna, Austria. “https://www.R-project.org.
- Arnold, Jeffrey B. 2021. “Ggthemes: Extra Themes, Scales and Geoms for ’Ggplot2’.” https://CRAN.R-project.org/package=ggthemes.
- Deanna L. 2020. “Basic R Guide for NSC Statistics.” https://bookdown.org/dli/rguide/r-and-rstudio.html.
- Wickham, Hadley. 2016. “Ggplot2: Elegant Graphics for Data Analysis.” https://ggplot2.tidyverse.org.
- Wickham Hadley, Çetinkaya-Rundel Mine, Grolemund Garrett. “R for Data Science (2e).” https://r4ds.hadley.nz/.
- Wickham Hadley, Mara Averick, Jennifer Bryan, Winston Chang, Lucy D’Agostino McGowan, Romain François, Garrett Grolemund, et al. 2019. “Welcome to the Tidyverse” 4: 1686. https://doi.org/10.21105/joss.01686.
