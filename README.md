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

After these 2 steps, I extract the dataframe into a csv file to upload into Graphite Note, which provides us with many powerful models running by its AI.

### 3. Label Encoding

Most of the values in several columns were originally categorical but had been mapped from text labels to numeric codes. To ensure the predictive model interprets these variables correctly as categorical features rather than continuous numerical values, a reverse mapping process was applied to convert the numeric codes back to their original categorical labels. This step preserved the semantic meaning of the data and prevented the model from making false assumptions about ordinal relationships that do not actually exist.

Input:

<img width="608" height="132" alt="{3F81EDBF-CB65-49A5-B8DB-9ED0A6DEAE86}" src="https://github.com/user-attachments/assets/d764cb82-d36f-45e5-b6b5-90ce443ea1c1" />

## Building a Classfication Model

The process begins by examining the distribution of the target variable 'readmitted'. It revealed that the dataset is imbalanced, with most patients not being readmitted, followed by those readmitted after 30 days, and a smaller group readmitted within 30 days. Understanding this imbalance is essential for later handling class distribution during model training.

<img width="1385" height="436" alt="{38EF690B-6854-413A-8E12-6EA1FCC92DCB}" src="https://github.com/user-attachments/assets/c16bcea9-7292-4c52-a9ab-11c017afeb02" />

The next stage involves splitting the data into training and testing subsets using train_test_split from sklearn.model_selection. A test size of 20% is specified, and stratify=y ensures that the class distribution is preserved in both sets, which is particularly important when working with imbalanced target variables. A fixed random_state is used to make the split reproducible.

<img width="470" height="174" alt="{1FE621B6-C6B0-43BA-B76A-CF03806E3BCA}" src="https://github.com/user-attachments/assets/92aff85c-27a3-4d45-a151-87ab541930d5" />

To address the class imbalance identified earlier, the SMOTENC technique is implemented. First, the categorical columns are identified as those with 20 or fewer unique values. Their positions are stored in cat_idx, and passed to SMOTENC so that the algorithm knows which features require categorical handling. SMOTENC creates synthetic samples for the minority classes, generating numeric features through interpolation and categorical features by selecting the most frequent category among nearest neighbors. This results in a balanced training dataset without distorting categorical variables.

Finally, the RandomForestClassifier from sklearn.ensemble is imported in preparation for model training. This algorithm is a powerful and flexible ensemble learning method based on decision trees, capable of handling both categorical and numerical data, and robust to overfitting when tuned properly.

The dataset contained a mix of numerical and categorical variables, and the target variable was imbalanced, with significantly fewer cases of early readmission. To address this, we applied SMOTENC, an oversampling technique specifically designed for mixed data types. Unlike standard SMOTE, which works only with numerical data, SMOTENC generates synthetic samples by interpolating numerical features while assigning realistic values to categorical features based on their most frequent occurrences. This ensured the creation of high-quality synthetic data that preserved the original structure and meaning of each feature, helping the predictive model learn more effectively from a balanced dataset.

<img width="631" height="136" alt="{DCB09EB0-DA99-48CA-B86D-8F142C9F4FF7}" src="https://github.com/user-attachments/assets/65e9ec85-d2a4-41d9-8da0-2280af3cd394" />

## Output Report

### Python

The evaluation results indicate that the model performs unevenly across the three readmission classes. It achieves its best performance on patients with no readmission (Class 2), with a precision of 0.63, recall of 0.64, and an F1-score of 0.64, likely due to this class having the largest representation in the dataset. For patients readmitted after 30 days (Class 1), the model shows moderate performance, with precision and recall both at 0.45. However, for early readmissions within 30 days (Class 0), performance is poor, with precision and recall around 0.16 and 0.15 respectively, indicating substantial difficulty in detecting this minority class. The overall accuracy is 52%, with a macro average F1-score of 0.41 and a weighted average of 0.52, reflecting the impact of class imbalance despite the use of SMOTENC. These results suggest that further improvements are needed, particularly in enhancing the model’s ability to identify early readmissions, possibly through additional feature engineering, hyperparameter tuning, or alternative algorithms.

<img width="462" height="249" alt="{8C0C29FB-4291-4098-9C09-1B216FA0478A}" src="https://github.com/user-attachments/assets/4288abb6-87a6-4012-95d0-ef40accdb3d8" />

### [Graphite Note](https://app.graphite-note.com/#/model/edit/85a0ab412eb1/view_MultiClassClassificationResults)

The confusion matrix reveals that the model has a strong tendency to predict the “NO” readmission class, often at the expense of accurately identifying the other two classes. For the 10,764 actual “NO” cases, the model correctly predicted 9,156 but misclassified 1,590 as “>30 days” and 18 as “<30 days.” For the 7,035 actual “>30 days” cases, 2,552 were correctly classified, while a substantial 4,447 were misclassified as “NO” and 36 as “<30 days.” The most concerning result is with the 2,250 actual “<30 days” cases, where only 36 were correctly identified, while 1,362 were incorrectly labeled as “NO” and 852 as “>30 days.” These results indicate that the model is biased toward predicting the majority “NO” class and struggles significantly with detecting early readmissions within 30 days, highlighting the need for improved handling of class imbalance and stronger feature differentiation for the minority classes.

<img width="1225" height="380" alt="{A511904C-46DB-4602-BFB3-2851D60E655D}" src="https://github.com/user-attachments/assets/86bb41df-55e9-486f-b86d-1f560b340115" />


The model’s overall performance in predicting patient readmission shows moderate effectiveness, with an F1-score of 53.45% indicating a balanced but not exceptional trade-off between precision and recall. The accuracy of 58.58% suggests the model correctly predicts readmission status in slightly more than half of the cases, which is above random guessing for a multi-class problem but still leaves considerable room for improvement. The AUC score of 66.76% reflects a fair ability to distinguish between readmission outcomes, suggesting that the model captures some meaningful patterns in the data but struggles with finer distinctions, particularly in more challenging cases. Precision at 55.27% shows that slightly more than half of the predicted readmissions are correct, while a recall of 58.58% indicates that the model successfully identifies just over half of all actual readmissions. Overall, these metrics point to a model that is functional but would benefit from further optimization such as feature engineering, hyperparameter tuning, or advanced ensemble methods to improve discriminatory power and predictive reliability, especially for harder-to-detect classes.

<img width="1299" height="760" alt="{F48D4F5D-F883-4D96-AAA8-141DC86C278B}" src="https://github.com/user-attachments/assets/fd5e45b3-4220-4cfb-a037-64f6e3c0cd3a" />



