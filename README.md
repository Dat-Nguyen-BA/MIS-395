# Artificial Intelligence for Business - MIS-395
### Final Project - Building A Predictive Model via Different Platforms

# Dataset Source: [Diabetes 130-US Hospitals for Years 1999-2008](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008)

## Business Context

The dataset contains ten years (1999–2008) of clinical care records collected from 130 hospitals and integrated delivery networks across the United States. It comprises more than 50 characteristics that capture detailed information on patient demographics, diagnoses, treatments, and hospital-related outcomes.

## Project Objective

The primary objective of this project is to develop a predictive model that determines the readmission status of diabetic patients. The target variable categorizes patients into three groups: those who are readmitted within 30 days, those readmitted after 30 days, and those not readmitted at all. This classification task holds significant value in healthcare settings, as early identification of high-risk patients enables hospitals to implement timely interventions, reduce preventable readmissions, and allocate resources more effectively.

## Dataset Overview

The dataset consists of 101,766 patient records and 50 attributes, offering a rich, structured view of inpatient encounters involving diabetic patients in the United States. These attributes cover a wide range of information, including demographic characteristics (such as race, gender, and age), hospitalization details (like admission type, discharge disposition, and number of inpatient visits), diagnostic codes, procedures, medications, and clinical outcomes. 

## Preprocessing Steps

### 1. Checking missing values

While the dataset originally appeared to have no missing values, further inspection reveals that 7 attributes contain missing data, encoded using the placeholder '?'. Therefore, I decided to replace ‘?’ with null values.

Sample Output:

<img width="1720" height="508" alt="{35CFA946-F14A-4946-9599-63AF56790CBF}" src="https://github.com/user-attachments/assets/e7ee0dcd-c615-4710-ad3f-34175965c7fe" />



After retrieving again all attributes and its missing row counts, I got some missing attributes.

Output:

<img width="333" height="247" alt="{18AC3C61-5DD7-4121-8235-E9872F2D755C}" src="https://github.com/user-attachments/assets/a50ea2b0-5e3a-467f-85ce-057620daf20a" />


The dataset contains several columns with a high proportion of missing values. In particular, weight (96.86%), max_glu_serum (94.75%), A1Cresult (83.27%), and medical_specialty (49.07%) each have more than 40% missing entries. Due to the significant amount of missing data, these columns were removed from the dataset to avoid introducing bias and to simplify model training. Additionally, other columns with limited predictive value, including those containing identification information (encounter_id, patient_nbr) and payer_code, were also excluded to ensure the model focuses on features that are relevant to predicting early patient readmission.

After removing the high-missing-value and non-predictive columns, the dataset still contained three diagnosis-related columns (diag_1, diag_2, and diag_3) with small proportions of missing values (0.02%, 0.35%, and 1.40% respectively). Given the complexity of accurately imputing medical diagnosis codes and the minimal impact of removing such a small portion of the data, these missing records were handled by deleting the corresponding rows. This approach ensured data integrity while preserving the overall dataset size for model training.

### 2. Categorizing the ICD-9 grouping code

The values in diag_1, diag_2, and diag_3 are inherently categorical, as each diagnosis code corresponds to a specific medical condition that can be grouped into broader disease categories. To standardize these variables, a Python mapping function was applied to categorize all diagnosis codes into 18 distinct groups based on the World Health Organization’s ICD-9 classification standard. This transformation not only simplified the representation of diagnosis information but also enhanced the dataset’s suitability for predictive modeling by consolidating granular codes into meaningful clinical categories.

Sample Input:

<img width="348" height="641" alt="{A4B5CDA0-212E-4CDE-90C7-365D3D40F7DF}" src="https://github.com/user-attachments/assets/5b2317ef-96a5-42be-814f-c6037797f519" />

### 3. Label Encoding

Most of the values in several columns were originally categorical but had been mapped from text labels to numeric codes. To ensure the predictive model interprets these variables correctly as categorical features rather than continuous numerical values, a reverse mapping process was applied to convert the numeric codes back to their original categorical labels. This step preserved the semantic meaning of the data and prevented the model from making false assumptions about ordinal relationships that do not actually exist.

Input:

<img width="608" height="132" alt="{3F81EDBF-CB65-49A5-B8DB-9ED0A6DEAE86}" src="https://github.com/user-attachments/assets/d764cb82-d36f-45e5-b6b5-90ce443ea1c1" />
