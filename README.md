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


## Midterm Report (November 5, 2024)


### Data Export and Processing

I've successfully exported part of the data from Borbalo's production database, capturing approximately 500 000 traffic violation records. The data extraction process was carefully designed to ensure privacy and security while maintaining analytical value.

#### Data Export Process

The following SQL query was used to export the data while implementing privacy measures:

```sql
SET TIME ZONE 'Asia/Tbilisi';
COPY (
SELECT 
    REGEXP_REPLACE(REGEXP_REPLACE(external_id, '[A-Za-z]', 'X', 'g'), '[0-9]', '0', 'g') as "FineType",
    agency as "Agency",
    violation_date as "ViolationDate",
    REGEXP_REPLACE(REGEXP_REPLACE(license_plate, '[A-Za-z]', 'X', 'g'), '[0-9]', '0', 'g') AS "LicensePlateTemplate",
    (clean_license_plate !~ '^[A-Z]{2}[0-9]{3}[A-Z]{2}$' and clean_license_plate !~ '^[A-Z]{2}[0-9]{4}$') as "IsCustom",
    ff.created_at as "ReceivedAt",
    final_discount_date as "FinalDiscountDate",
    final_payment_date as "FinalPaymentDate",
    original_value as "OriginalAmount",
    final_amount as "FinalAmount",
    article as "Article",
    REPLACE(REPLACE(fine_reason, E'\n', ''), E'\r', '') as "FineReasonText",
    region_name as "Region",
    raion_name as "Municipality",
    has_media as "HasMedia",
    city_coordinates as "CityCoords",
    evacuated as "WasTowed",
    evacuation_coordinates as "TowedFromCoords",
    evacuation_end_date as "TowingEndDate",
    evacuation_fee as "TowingFee",
    evacuation_start_date as "TowingStartDate"
FROM public.fines ff
left join fine_cameras fc on fc.id = ff.camera_id
order by ff.created_at
) TO 'd:/borbalo_fines.csv' WITH (FORMAT CSV, HEADER);
```

Key aspects of the data anonymization process:
- License plates are templated by replacing letters with 'X' and numbers with '0'
- Fine types (external IDs) are similarly templated to protect privacy
- Custom plate identification is done through pattern matching
- Temporal data is preserved for analysis purposes
- Geographic coordinates are maintained for spatial analysis


#### Privacy Enhancement Plans

After consulting with faculty members, I'm implementing additional privacy measures:
1. Adding controlled noise to geographic coordinates to prevent exact location identification
2. Implementing differential privacy techniques to protect against reverse engineering attempts
3. Aggregating temporal data into broader time bins to reduce identifiability
4. Creating synthetic data for sensitive patterns while maintaining statistical properties

#### Next Steps

1. **Data Cleaning**:
   - Remove duplicate entries
   - Handle missing values
   - Standardize geographic coordinates

2. **Advanced Analysis**:
   - Implement time series analysis
   - Develop spatial clustering models
   - Create predictive models for fine likelihood

3. **Visualization Development**:
   - Create interactive maps
   - Generate temporal trend visualizations
   - Design comparative analysis charts



