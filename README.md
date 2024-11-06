# Project Proposal

## Title
**Debunking the Myth: An Analysis of Fine Rates for Custom vs. Standard License Plates in Georgia Using Borbalo Data**

---

### 1. Description of the Project

I developed [**Borbalo**](https://borbalo.ge/) in October 2022 as a mobile application to help Georgian drivers stay informed about traffic fines. Borbalo provides real-time notifications of fines, detailed violation histories, and insights into traffic enforcement patterns across different regions. Over the past two years, Borbalo has collected extensive data on traffic fines, including those associated with both custom and standard license plates.

This project aims to investigate the common belief in Georgia that custom license plates are less likely to receive fines compared to standard plates. By analyzing the fine distribution data from Borbalo, I seek to determine whether this myth holds true and to provide evidence-based insights into traffic enforcement practices.

---

### 2. Clear Goals

- **Primary Goal:**  
  To determine if there is a significant difference in the fine rates between custom and standard license plates in Georgia.

- **Secondary Goals:**  
  - Identify factors that contribute to the perception that custom plates receive fewer fines.
  - Assess the fairness and consistency of traffic law enforcement across different license plate types.
  - Provide recommendations to improve public awareness and policy based on the findings.

---

### 3. Data Collection

- **Data Source:**  
  I will export data directly from Borbalo's production database, which records all traffic fines processed through the app.

- **Data to be Collected:**  
  - **License Plate Information:** Type (custom vs. standard), alphanumeric sequences.
  - **Fine Details:** Number of fines issued, types of violations (e.g., speeding, illegal parking), fine amounts, fine locations, fine type.
  - **Vehicle Information:** Make, model, year, vehicle type.

- **Data Collection Method:**  
  I will use secure SQL queries to export the necessary data from Borbalo's database, ensuring data integrity and compliance with privacy standards. All data will be anonymized to protect individual privacy.

---

### 4. Data Modeling Plan

To analyze the data, I plan to use the following modeling approaches:

- **Descriptive Statistics:**  
  Summarize the distribution of fines across license plate types, including measures like mean and variance.

- **Inferential Statistics:**  
  - **Chi-Square Tests:** To assess the association between license plate type and the occurrence of fines.
  - **T-Tests:** To compare the average number of fines between custom and standard plates, depending on data distribution.

- **Regression Analysis:**  
  - **Logistic Regression:** To evaluate the probability of receiving a fine based on license plate type while controlling for other variables.
  - **Poisson Regression:** To model the count of fines per vehicle in relation to license plate type and other predictors.

- **Random Forest Classification**
  - Use ensemble learning to identify factors most predictive of receiving fines
  - Can handle non-linear relationships and provide feature importance rankings

- **Geographically Weighted Regression (GWR)**
  - Analyze spatial variability in the relationship between license plate types and fine rates
  - Account for geographic differences in enforcement patterns across Georgia

- **Propensity Score Matching**
  - Control for confounding variables when comparing fine rates between custom and standard plates
  - Reduce bias in estimating the effect of plate type on fine likelihood
 
*Note:* I may refine these techniques as I explore the data further during the project.

---

### 5. Data Visualization Plan

To effectively communicate my findings, I plan to use the following visualization methods:

- **Bar Charts:**  
  Compare the total number of fines issued to custom versus standard license plates.

- **Pie Charts:**  
  Show the proportion of different types of violations within each license plate category.

- **Scatter Plots:**  
  Explore relationships between fine amounts and variables such as time of day or location, differentiated by license plate type.

- **Heat Maps:**  
  Visualize the geographic distribution of fines, highlighting areas with higher enforcement rates for each license plate type.

- **Interactive Dashboards:**  
  Potentially use tools like Tableau or Power BI to create dynamic visualizations for deeper data exploration.

---

### 6. Test Plan

To ensure the validity and reliability of my analysis, I will implement the following test plan:

- **Data Splitting:**  
  - **Training Set:** 80% of the data will be used to develop and train the statistical models.
  - **Testing Set:** 20% of the data will be withheld to evaluate model performance.

- **Temporal Validation:**  
  - **Training on Historical Data:**  
    Use data from January 2023 to August 2024 for training.
  - **Testing on Recent Data:**  
    Reserve data from September 2024 to October 2024 for testing to assess the model's generalizability.

- **Cross-Validation:**  
  - **K-Fold Cross-Validation:**  
    Perform 5-fold cross-validation on the training set to evaluate model stability and performance consistency.

- **Performance Metrics:**  
  - **For Classification Models (e.g., Logistic Regression):**  
    Accuracy, Precision, Recall, F1-Score, ROC-AUC.
  - **For Count Models (e.g., Poisson Regression):**  
    Deviance, Akaike Information Criterion (AIC), goodness-of-fit measures.

- **Hypothesis Testing:**  
  - **Null Hypothesis (H₀):** There is no difference in fine rates between custom and standard license plates.
  - **Alternative Hypothesis (H₁):** There is a significant difference in fine rates between custom and standard license plates.
  - Conduct statistical tests to accept or reject these hypotheses based on the data.

- **Assumptions Checking:**  
  Ensure that the assumptions underlying each statistical method are satisfied, such as normality for t-tests or appropriate distributional assumptions for regression models.

*Note:* I may adjust the test plan based on preliminary findings and the specific characteristics of the dataset.

---

### 7. Conclusion

By leveraging the comprehensive data collected through the Borbalo app, this project aims to provide a clear, evidence-based analysis of fine rates associated with custom and standard license plates in Georgia. Through rigorous statistical modeling and thoughtful visualization, I intend to debunk the existing myth and contribute valuable insights to traffic law enforcement practices and public perceptions.

---


# Midterm Report (November 5, 2024)

[![Borbalo Data Analysis](https://img.youtube.com/vi/LXDBtRZ7g9Y/0.jpg)](https://www.youtube.com/watch?v=LXDBtRZ7g9Y)

## Initial Data Analysis Findings

After exporting and analyzing approximately 500,000 traffic violation records from Borbalo's production database, several key insights and challenges have emerged:

### Data Distribution and Representation Issues
- Custom plates are significantly underrepresented in our sample (only 724 out of 502,451 records, or 0.14%)
- This appears to be lower than the actual proportion in Georgia - need to validate against vehicle registration data  
- Potential sampling bias needs to be addressed in our analysis

### Privacy Measures Implemented
Successfully implemented privacy-preserving data export using:
```sql
REGEXP_REPLACE(REGEXP_REPLACE(license_plate, '[A-Za-z]', 'X', 'g'), '[0-9]', '0', 'g') AS "LicensePlateTemplate"
```

### Key Challenges Identified

1. **Vehicle Identification**:
   - Current templating system makes it difficult to track individual vehicles
   - Need to implement a consistent hashing mechanism to maintain privacy while enabling per-vehicle statistics
   - Planning to add unique vehicle identifiers while preserving anonymity

2. **Data Quality**:
   - Missing data analysis revealed significant gaps:
     - 74.06% missing payment dates
     - 21.22% missing discount dates  
     - 12.41% missing regional data
   - Need to investigate the reasons for these gaps

3. **Next Steps**:
   - Implement vehicle tagging system for accurate per-car statistics
   - Research actual custom plate proportions in Georgia
   - Investigate and clean outlier data 
   - Develop more robust data validation processes

### Detailed Analysis and Visualization

A comprehensive walkthrough of the initial data analysis, including code and visualizations, can be found in the video above.

## Future Work

1. **Data Enhancement**:
   - Implement consistent vehicle identification system
   - Cross-reference with vehicle registration data for validation
   - Develop more sophisticated anonymization techniques

2. **Analysis Expansion**:
   - Create temporal pattern analysis
   - Develop geographic distribution models
   - Analyze fine type distributions by plate category

3. **Statistical Validation**:
   - Compare sample distributions with population data
   - Implement bias correction methods
   - Develop confidence intervals for key metrics

The initial findings suggest that while our privacy measures are effective, they've created some analytical challenges that need to be addressed in the next phase of the project.




