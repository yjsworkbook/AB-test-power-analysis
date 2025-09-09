# A/B Test Power Analysis for Conversion Rates

This repository contains a Jupyter Notebook that performs a power analysis and a statistical significance test for an A/B test on website conversion rates. The analysis uses a t-test to compare the performance of a new landing page (treatment) against an old one (control).

## Project Overview

The goal of this analysis is to determine if a new landing page leads to a statistically significant increase in the user conversion rate compared to the old page.

The notebook covers the following key steps:
1.  **Data Loading and Cleaning**: Imports the A/B test data and cleans it by removing inconsistent user assignments (e.g., control group seeing the new page).
2.  **Exploratory Data Analysis (EDA)**: Calculates and visualizes the conversion rates for both the control and treatment groups.
3.  **Power Analysis**: Determines the required sample size to detect a Minimum Detectable Effect (MDE) with a given statistical power and significance level.
4.  **Hypothesis Testing**: Conducts a one-tailed independent t-test to evaluate the statistical significance of the observed difference in conversion rates.
5.  **Confidence Interval Calculation**: Computes the confidence interval for the difference in means to quantify the range of the true effect.

## Dataset

The analysis uses the `ab_data.csv` dataset, which contains the following columns:
- `user_id`: A unique identifier for each user.
- `timestamp`: The time when the user visited the page.
- `group`: The group the user was assigned to (`control` or `treatment`).
- `landing_page`: The page the user saw (`old_page` or `new_page`).
- `converted`: A binary indicator of whether the user converted (1) or not (0).

## Methodology

### Power Analysis
Before running the experiment, it's crucial to determine the necessary sample size. This analysis calculates the sample size required to detect a **Minimum Detectable Effect (MDE)** of 2% with 80% power at a 5% significance level.

- **Baseline Conversion Rate**: ~12.0% (from the control group)
- **Minimum Detectable Effect (MDE)**: 2%
- **Alpha (Significance Level)**: 0.05
- **Beta (1 - Power)**: 0.20

The `statsmodels.stats.power.tt_ind_solve_power` function is used for this calculation.

### Hypothesis Testing
A one-tailed independent t-test is performed to test the hypothesis that the new page has a higher conversion rate.

- **Null Hypothesis (H₀)**: The conversion rate of the treatment group is less than or equal to the conversion rate of the control group (`p_treatment ≤ p_control`).
- **Alternative Hypothesis (H₁)**: The conversion rate of the treatment group is greater than the conversion rate of the control group (`p_treatment > p_control`).

The test is conducted using `scipy.stats.ttest_ind` with `equal_var=False` to perform Welch's t-test, which does not assume equal variances between the two groups.

## Results

- **Observed Conversion Rates**:
  - Control Group: **12.0%**
  - Treatment Group: **11.9%**
- **Required Sample Size**: The power analysis indicated a required sample size of **4,451** per group. The actual sample size (~145,000 per group) is more than sufficient.
- **T-test Results**:
  - **T-statistic**: 1.3116
  - **P-value**: 0.0948

The p-value (0.0948) is greater than the significance level (0.05). Therefore, we **fail to reject the null hypothesis**.

## Conclusion

There is not enough statistical evidence to conclude that the new landing page has a significantly higher conversion rate than the old one. The observed difference is likely due to random chance.

### Suggestions for Next Steps
*   **Analyze User Segments**: Investigate if the new page performed better for specific user segments (e.g., new vs. returning users, different demographics).
*   **Iterate on Design**: Use user feedback or behavior analytics to refine the new page and run another test.
*   **Evaluate Other Metrics**: Consider the impact on other metrics like bounce rate, time on page, or user engagement.

## How to Run

1.  Ensure you have Python and Jupyter Notebook installed.
2.  Install the required libraries:
    ```bash
    pip install numpy pandas scipy matplotlib seaborn statsmodels
    ```
3.  Place the `ab_data.csv` file in the correct path as specified in the notebook.
4.  Run the `Power_Analysis_in_T_Test.ipynb` notebook.