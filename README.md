# Python_for_data_science_23224532_Presentation1

# FBI Wanted Persons Data Analysis

This repository contains two Jupyter notebooks (`Hypothesis_1.ipynb` and `Hypothesis_2.ipynb`) that perform exploratory data analysis and hypothesis testing on data acquired from the FBI's publicly available Wanted Persons API.

## Project Overview

This project delves into a comprehensive analysis of the FBI's publicly available Wanted Persons list. By acquiring, cleaning, and analyzing this dataset through two distinct Jupyter notebooks, we aim to uncover patterns and relationships between various attributes of wanted individuals, such as crime types, demographic factors (age), and physical characteristics (tattoos). Each notebook investigates a specific, testable hypothesis using statistical methods to draw data-driven conclusions, contributing to a deeper understanding of trends within this critical law enforcement data.

## Notebooks

### `Hypothesis_1.ipynb`: Tattoo Prevalence in Violent vs. White-Collar Crimes

**Hypothesis:** Individuals wanted for certain types of crimes (e.g., violent crimes vs. white-collar crimes) are more likely to have tattoos than individuals wanted for other types of crimes.

**Detailed Summary:**
This notebook delves into a specific aspect of the FBI Wanted Persons dataset: the relationship between the type of crime an individual is wanted for and the presence of tattoos. The analytical workflow is meticulously structured as follows:

1.  **Data Acquisition:**
    *   **Initial Attempt & API Limitation:** An initial `GET` request to the FBI API with `pageSize=500` only returned 50 records, revealing an API-imposed page size limit.
    *   **Pagination Strategy:** To overcome this, a robust pagination strategy was implemented. A `while` loop iteratively fetches 50 records per page until the target of 1060 entries is successfully retrieved, ensuring a comprehensive dataset for analysis.

2.  **Exploratory Data Analysis (EDA):**
    *   The raw dataset, comprising 1060 entries across 54 features, undergoes initial inspection using `df.info()` and `df.shape`.
    *   **Key Insight:** A significant challenge identified during EDA was the substantial amount of missing data across many columns, particularly for age-related information (`age_min`, `age_max`), which was missing for a large majority of entries.

3.  **Data Cleaning:**
    *   **Missing Value Analysis:** The percentage of missing values for each column is meticulously calculated (`df.isnull().sum() / len(df) * 100`). This provides a standardized measure to assess data sparsity.
    *   **Column Dropping:** Columns with more than 80% missing values are systematically dropped to improve data quality and focus the analysis on more complete features.

4.  **Crime Type Categorization:**
    *   **Keyword Definition:** Based on n-gram analysis and domain understanding, specific keywords for 'violent crimes' (e.g., 'murder', 'assault', 'robbery', 'fugitive') and 'white-collar crimes' (e.g., 'fraud', 'money laundering', 'identity theft', 'cyber's most wanted') are defined.
    *   **Categorization Logic:** New boolean columns (`is_violent_crime`, `is_white_collar_crime`) are created by checking for the presence of these keywords within the `description`, `title`, and `subjects` columns.

5.  **Tattoo Identification:**
    *   The `scars_and_marks` column is meticulously analyzed to identify entries that explicitly mention 'tattoo'. A new boolean column, `has_tattoo`, is generated to flag these individuals.

6.  **Hypothesis Testing:**
    *   **Proportion Calculation:** The proportion of individuals with tattoos is calculated independently for both the 'violent crime' and 'white-collar crime' groups.
    *   **Chi-squared Test:** A Chi-squared test of independence is performed on a contingency table constructed from the counts of tattooed vs. non-tattooed individuals within each crime category. This statistical test assesses whether the observed difference in proportions is statistically significant, helping to determine if the presence of tattoos is independent of the crime type.

**Key Finding:** The analysis revealed that approximately 13.93% of individuals categorized with violent crimes had tattoos, compared to approximately 3.35% for white-collar crimes. The Chi-squared test yielded a p-value of 0.0000 (less than the significance level of 0.05), leading to the rejection of the null hypothesis. This provides statistically significant evidence to support the hypothesis that individuals wanted for violent crimes are more likely to have tattoos than those wanted for white-collar crimes in this dataset.

### `Hypothesis_2.ipynb`: Cyber Crime Association with Age Groups

**Hypothesis:** Are cyber crimes more frequently associated with young adults (age 18-25) compared to older individuals (age 40-65)?

**Detailed Summary:**
This notebook investigates a demographic hypothesis, specifically exploring the relationship between age and involvement in cyber crimes within the FBI Wanted Persons dataset. The analytical process is structured as follows:

1.  **Data Acquisition & Initial Cleaning:**
    *   Similar to `Hypothesis_1.ipynb`, data is acquired from the FBI API using a robust pagination strategy to retrieve all 1060 entries.
    *   Initial data cleaning involves handling missing values and dropping columns deemed irrelevant for this specific hypothesis (e.g., `files`, `reward_max`, `reward_min`, etc., and those with over 90% missing values).
    *   The cleaned DataFrame is also saved to an Excel file (`fbi_wanted_500.xlsx`) for potential external use.

2.  **Exploratory Data Analysis (EDA):**
    *   Initial data overview is performed, including shape and info, followed by categorical analysis of columns like `hair`, `eyes`, `field_offices`, `sex`, and `subjects`.
    *   Word frequency and n-gram analyses (bigrams and trigrams) are conducted on text-based columns (`description`, `title`, `publication`) to identify common terms and phrases, which are crucial for subsequent crime identification.

3.  **Age Group Identification:**
    *   **Young Adults:** Entries are categorized as 'young adults' (age 18-25) by filtering individuals where `age_min` and `age_max` overlap with this range.
    *   **Older Adults:** Entries are categorized as 'older adults' (age 40-65) using similar filtering logic on `age_min` and `age_max`.
    *   New columns (`age_group`) are created in the DataFrame to reflect these categorizations.

4.  **Cyber Crime Identification:**
    *   **Keyword Definition:** Keywords related to cyber crimes (e.g., 'cyber', 'computer fraud', 'hacking', 'identity theft', 'wire fraud', 'cyber's most wanted') are defined based on n-gram analysis and common cyber crime terminology.
    *   **Categorization Logic:** A new boolean column (`is_cyber_crime`) is created by checking for these keywords within the `description`, `title`, and `subjects` columns.

5.  **Hypothesis Testing:**
    *   **Proportion Calculation:** The proportion of cyber crime entries is calculated independently for both the 'young adult' and 'older adult' groups.
    *   **Chi-squared Test:** A Chi-squared test of independence is performed to statistically compare these proportions and determine if the observed differences are significant.

**Key Finding:** The analysis revealed that the proportion of cyber crime entries in young adults (approximately 1.56%) was very low, and for older adults, it was 0%. The Chi-squared test yielded a high p-value of 0.9817 (greater than the significance level of 0.05), leading to a failure to reject the null hypothesis. This indicates that, based on this dataset, there is no statistically significant evidence to support the hypothesis that cyber crimes are more frequently associated with young adults (age 18-25) compared to older individuals (age 40-65). The observed differences are likely due to random variation.

## Setup and Usage

To effectively run and replicate the analyses presented in these notebooks, please follow the setup and usage instructions below:

1.  **Clone the repository:**
    Begin by cloning this repository to your local machine using Git:
    ```bash
    git clone <repository-url> # Replace <repository-url> with the actual URL of your GitHub repository
    cd <repository-directory> # Navigate into the cloned project directory
    ```

2.  **Create and activate a Python virtual environment** (highly recommended):
    A virtual environment ensures that project dependencies are isolated from your system's Python environment, preventing potential conflicts.
    ```bash
    python -m venv venv # Creates a new virtual environment named 'venv'
    # Activate the virtual environment:
    # On Windows:
    # .\venv\Scripts\activate
    # On macOS/Linux:
    # source venv/bin/activate
    ```
    You will know the environment is active when `(venv)` appears at the beginning of your terminal prompt.

3.  **Install the required Python packages:**
    Install all necessary libraries using pip. The notebooks rely on several common data science and machine learning packages.
    ```bash
    pip install requests pandas scikit-learn matplotlib seaborn scipy jupyter
    ```
    *Note: If you have a `requirements.txt` file in your repository (which is good practice for Python projects), you would typically install dependencies using `pip install -r requirements.txt`.*

4.  **Open and run the Jupyter Notebooks:**
    Once all dependencies are installed, you can launch Jupyter Notebook and access the analysis files.
    ```bash
    jupyter notebook
    ```
    Your web browser will automatically open a new tab displaying the Jupyter interface. From there, navigate to `Hypothesis_1.ipynb` or `Hypothesis_2.ipynb` and execute the cells sequentially to reproduce the analysis. Ensure you run all cells in order to avoid errors related to missing variables or dataframes.
