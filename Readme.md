# CareLoop – Appointment No-Show Analysis

## Project Overview

CareLoop is an appointment no-show analysis project that uses data cleaning, SQL analytics, machine learning and Generative AI to understand why patients miss scheduled appointments and help clinics improve appointment attendance.

## Key Findings

- **Part 1 – Data Preparation:** Cleaned the original appointment dataset and prepared 990 records with 16 columns for analysis.
- **Part 2 – SQL Analysis:** Identified that appointments booked 31 or more days in advance had the highest no-show rate of 60.5%.
- **Part 3 – Machine Learning:** Selected Logistic Regression, achieving a ROC-AUC score of 0.789 on the test dataset.
- **Part 4 – GenAI Analysis:** Analysed 10 follow-up notes using Gemini and identified different reasons for missed appointments, including transport, cost, illness and work-related issues.

## Detailed Results

### Part 1 – Data Cleaning and Preparation

The original dataset contained 1,020 appointment records. After cleaning and preprocessing, 990 records were retained. Missing values, invalid ages, duplicate records and outliers were handled. Clinic weather information was also enriched using the Open-Meteo API.

The final cleaned dataset contained 990 appointments and 16 columns, including the target variable `no_show`.

### Part 2 – SQL Analysis

MySQL was used to analyse appointment attendance patterns.

Key observations:
- Overall no-show rate: 22.0% (218 out of 990 appointments).
- Appointments booked 1–7 days in advance had a 16.2% no-show rate.
- Appointments booked 15–30 days in advance had a 30.0% no-show rate.
- Appointments booked 31 or more days in advance had a 60.5% no-show rate.
- Clinic C08 recorded the highest no-show rate at 33.3%.

### Part 3 – Machine Learning

Logistic Regression, Decision Tree and Random Forest models were trained and evaluated.

Logistic Regression was selected because it achieved the highest cross-validation ROC-AUC score of 0.774 among the compared models.

Final test performance:
- Accuracy: 78.8%
- Precision: 57.1%
- Recall: 57.1%
- F1-score: 57.1%
- ROC-AUC: 0.789

A probability threshold of 0.35 was selected using out-of-fold predictions to improve the identification of potentially missed appointments. This threshold provides a balance between precision and recall for the project.

### Part 4 – Generative AI Analysis

Google Gemini was used to classify the first 10 follow-up notes into six predefined root-cause categories.

The analysis identified reasons such as:
- Forgot or no reminder
- Distance or transport
- Cost or fee
- Illness or emergency
- Work or schedule
- Other

One blank note was skipped, and nine non-blank notes were analysed. Pydantic was used to validate the structured JSON responses.

The prompt comparison showed that the CRAFT-based V2 prompt returned valid categories for both N007 and N009, while the simple V1 prompt returned descriptions that failed the allowed-category validation.

The results were merged with appointment records using `appointment_id`. Both notes classified as `forgot_or_no_reminder` belonged to appointments where no reminder had been sent.

## Recommendation

Clinics should introduce targeted interventions based on the findings. Appointments booked 31 or more days in advance had a 60.5% no-show rate, compared with 16.2% for appointments booked 1–7 days in advance. Therefore, clinics could consider additional reminders for patients booking well in advance. The selected Logistic Regression model achieved a test ROC-AUC of 0.789, with a 0.35 risk threshold and 57.1% recall, which can help identify appointments requiring additional follow-up. In the GenAI analysis, both patients classified under forgot_or_no_reminder had no reminder recorded. Clinics could also consider transport assistance, flexible appointment timings and fee-related support based on the reported reasons for missed visits.

## Limitations

1. **Dataset size:** The analysis used 990 cleaned appointment records, and the GenAI analysis covered only the first 10 follow-up notes. These findings may not represent all patients or clinics.
2. **Observational data:** Relationships between reminders, booking lead time and no-shows do not establish cause and effect.
3. **GenAI classification:** Gemini may misunderstand patient notes or assign incorrect categories. Although Pydantic and allowed-category checks validate the output structure and labels, they cannot guarantee that the classification is factually correct.

## Technologies Used

- Python
- Google Colab
- Pandas and NumPy
- Matplotlib and Seaborn
- MySQL and MySQL Workbench
- Scikit-learn
- Google Gemini API
- Pydantic
- Open-Meteo API

## Conclusion

CareLoop combines data analytics, machine learning and Generative AI to understand appointment no-shows from both numerical appointment records and patient feedback. The findings can help clinics identify higher-risk appointments and develop more targeted patient follow-up strategies.