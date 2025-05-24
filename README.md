# Automotive Complaint Tagging & Analysis

This project focuses on processing customer complaints from a vehicle service dataset to uncover **common issues**, identify **frequent parts mentioned**, and tag each record with **meaningful keywords** extracted from free-text feedback. The goal is to help stakeholders quickly understand recurring problems and make data-driven decisions.

---

## 📁 Project Structure

```
├── Task 2.xlsx                         # Raw input dataset
├── cleaned_tagged_dataset.xlsx         # Cleaned and tagged dataset output
├── column_analysis_summary.xlsx        # Column-level metadata summary
├── causal_parts_barplot.png            # Visualization of top causal parts
├── platform_cost_boxplot.png           # Boxplot of cost distribution across platforms
├── verbatim_wordcloud.png              # Word cloud of most common words in customer verbatim
├── main_script.py                      # Python code for end-to-end processing
├── README.md                           # Project overview and instructions (this file)
```

---

## 🧠 Techniques Used

| Task | Technique | Description |
|------|-----------|-------------|
| **Text Cleaning** | Tokenization, Lemmatization, Stopword Removal | Breaks sentences into words, simplifies them (e.g., "running" → "run"), removes common words like “the”, “is” |
| **Tag Generation** | TF-IDF Vectorization | Finds the most important words across all feedback by measuring word frequency and uniqueness |
| **Missing Data Handling** | Median Imputation, Placeholder Text | Replaces missing numbers with typical (middle) values, missing text with `"Unknown"` |
| **Outlier Removal** | IQR Method | Removes cost or mileage values that are unusually high or low |
| **Categorical Cleanup** | Uppercasing, Whitespace Stripping | Standardizes category names for consistency |
| **Visualization** | Bar Plots, Box Plots, Word Clouds | Makes key insights easy to interpret visually |

---

## 📝 Steps Followed

### 1. Load the Dataset
We read the dataset `dataset.csv` into a DataFrame using pandas.

```python
df = pd.read_csv("dataset.csv")
```

---

### 2. Column-Wise Summary
Each column is analyzed for:
- Data type (text, number, date)
- Number of unique values
- Count of missing (null) values
- Most frequent entries (for text columns)
- Descriptive stats (for numeric columns)

Saved to: **`column_analysis_summary.xlsx`**

---

### 3. Data Cleaning

#### ➤ Drop All-Null Columns
Columns like `CAMPAIGN_NBR` which contain only missing values are dropped.

#### ➤ Handle Missing Values
- **Text columns**: Filled with `"Unknown"`
- **Numeric columns**: Filled with **median value**

#### ➤ Convert Date Format
`REPAIR_DATE` is converted into proper date-time format.

#### ➤ Standardize Categories
Fields like `PLATFORM`, `STATE`, and `PLANT` are:
- Converted to **string**
- Trimmed of whitespace
- Converted to **UPPERCASE**

#### ➤ Outlier Removal
Using **Interquartile Range (IQR)**, we detect extreme values in cost and mileage fields and remove them before filling in typical values.

---

### 4. Text Preprocessing for Tagging

#### ➤ Clean and Tokenize Verbatim Feedback
- Text is cleaned: punctuation, numbers, and noise removed
- Broken into words using `nltk`
- Common filler words (stopwords) removed
- Remaining words are simplified to root forms (lemmatized)

#### ➤ Generate Tags using TF-IDF
We use **TF-IDF (Term Frequency – Inverse Document Frequency)** to extract the top 20 unique and informative tags from the cleaned text.

Each record is then tagged with relevant words from this top list.

---

### 5. Visualizations

####  Top 10 Causal Parts
Shows the most common parts involved in customer complaints.

![image](https://github.com/user-attachments/assets/2db310f5-39ea-4669-9de5-3b781c910a85)


####  Cost by Platform
Box plot comparing total repair cost across vehicle platforms.

![image](https://github.com/user-attachments/assets/76844f75-9212-41de-bcf6-a799cac33466)


#### Word Cloud of Feedback
A cloud of words most frequently appearing in customer complaints.

![image](https://github.com/user-attachments/assets/c7360179-b9f9-41bc-9d10-3201df8cf5ce)


All plots are saved as PNGs.

---

### 6. Save Outputs

- Cleaned and tagged dataset → `cleaned_tagged_dataset.xlsx`
- Summary of dataset structure → `column_analysis_summary.xlsx`

---

##  Insights Derived

- **Top Issue Tags**: “steering”, “wheel”, “cust”, “state”, “customer”
- **Top Combinations**: (“steering”, “wheel”), (“cust”, “steering”), etc.
- **Frequent Parts**: Steering wheels are the most reported component.
- **Key Complaint Themes**: Vehicle handling, heated parts, state-wise complaints.

---

## Actionable Recommendations

1. **Investigate Steering Component Failures**  
   The recurring mention of *steering* and *wheel* suggests systemic issues needing inspection.

2. **Customer Service Mapping**  
   Tags like *cust*, *state*, *customer* suggest regional or service-related concerns – analyze by geography.

3. **Prioritize Heated Components**  
   *Heated* parts are often flagged, indicating potential overheating or failure issues.

4. **Refine Voice of Customer (VoC) Monitoring**  
   Use tagged outputs to improve proactive support by identifying emerging complaint themes.

---

##  How to Run

1. Install required libraries:
   ```bash
   pip install pandas numpy matplotlib seaborn wordcloud scikit-learn nltk openpyxl
   ```

2. Place your dataset as `Task 2.xlsx` in the working directory.

3. Run the script:
   ```bash
   python script.py
   ```

4. Outputs will be saved in the same folder.

---

##  Libraries Used

- `pandas`, `numpy`: Data manipulation
- `matplotlib`, `seaborn`: Visualization
- `wordcloud`: Text visualization
- `sklearn`: TF-IDF tagging
- `nltk`: Natural language processing (tokenization, lemmatization)


**~AvB**

