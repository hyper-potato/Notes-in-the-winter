

**Topics**: Basics of data pre-processing and exploration

 

## Missing Values

- Missing completely at random(MCAR) 
  - presence of missing data is unrelated to any other observed or unobserved variable 

- Missing at random(MAR) 
  - presence of missing data is related to other values but not to its own unobserved value 
- Not missing at random(NMAR) 
  - hard to deal with 

• Many approaches fo rdealing with missingness 

​	Simple: Omit rows with missing values or Impute missing values 

- Pro: easy

		- Con: create bias, resulting in biased analysis; sometime sample size is power

More sophisticated ideas are covered in Data Camp Assignments 



## Cleaning: Noisy Data 

- Noise–random error or variance in a measured variable 
- Approaches to dealing with noise 
  - Smoothing
    - Techniques: binning, regression, moving average 
  - Outlier detection 

- There are domain-specific tools available for data cleaning, e.g.,

  - Phone, postal address, email verification 

  - Spell check 

    

## Detecting Outliers 

- An outlier is an observation that is “extreme”, being distant from the rest of the data 
  - definition of “distant” is deliberately vague 
- Outliers can have disproportionate influence on models 
  - a problem if it is spurious 
- An important step in data pre-processing is detecting outliers; but once detected, domain knowledge is often required to determine if it is an error, or truly extreme 
- In some contexts, finding outliers is the purpose of the DM exercise (e.g., airport security screening) 
  - This area of analytics is called “anomaly detection” 



### outlier detection tech

Boxplot:

- Mild outliers (1 out of 150 in normally distributed data): 
  - < *Q*1 – 1.5**IQR**
  - \> Q3 + 1.5**IQR**
- Extreme outliers (1 out of 425,000 in n. d. data): 
  - < *Q*1 – 3**IQR**
  - ***> Q*3 + 3**IQR* 







