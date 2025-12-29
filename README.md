# Regression Analysis: Analyzing Factors Associated with Online Customer Purchase Intention
---

## 1/ Project Overview ##
**Goal:** In this project, I analyze the factors associated with online customer purchase intention. Understanding these factors helps businesses make effective marketing decisions and focus their efforts on customers who are more likely to make a purchase.

**Method:** Interpreting logistic regression coefficients to quantify the effect of predictors on purchase intention.

**Metadata:** The dataset contains 12,330 sessions, where **Revenue** indicates whether an online visit results in a purchase or not.

| Feature | Type | Description |
|--------|------|-------------|
| Revenue | Target/Categorical | Whether the session resulted in a purchase |
| Administrative | Numerical | Number of administrative pages visited |
| Administrative_Duration | Numerical | Time spent (seconds) on administrative pages |
| Informational | Numerical | Number of informational pages viewed |
| Informational_Duration | Numerical | Time (seconds) on informational pages |
| ProductRelated | Numerical | Count of product-related pages |
| ProductRelated_Duration | Numerical | Time (seconds) on product-related pages |
| BounceRates | Numerical | Avg. bounce rate of visited pages |
| ExitRates | Numerical | Avg. exit rate at pages |
| PageValues | Numerical | Avg. page value before purchase |
| SpecialDay | Numerical | Closeness to a special day |
| Month | Categorical | Session month |
| OperatingSystems | Categorical | Visitor OS |
| Browser | Categorical | Visitor browser type |
| Region | Categorical | Visitor geographic region |
| TrafficType | Categorical | Source of traffic |
| VisitorType | Categorical | New, Returning, Other |
| Weekend | Categorical | Weekday vs Weekend visit |

## 2/ Project Workflow ##
<p align="center">
  <img src="Images/Flowchart.png" alt="Flowchart" width="600">
</p>

**Train a Logistic Regression Model:** Fit the data with a logistic regression model.

**Assumption check:** Check whether the model satisfies the six assumptions of logistic regression:

**1.** The response variable is binary.

**2.** The observations are independent.

**3.** There is no multicollinearity among explanatory variables.

**4.** There are no extreme outliers.

**5.** There is a linear relationship between the explanatory variables and the logit of the response variable.

**6.** The sample size is sufficiently large.

**Model refinement:** When any assumption is violated, appropriate tests or feature engineering techniques are applied.

**Interpretation of coefficients:** Model coefficients and p-values are interpretable once all assumptions are satisfied.

## 3/ Assumption Check and Refinement ##
### Assumption 1: The response variable is binary ###
<p align="center">
  <img src="Images/Target.png" alt="Flowchart" width="200">
</p>

The Target feature **Revenue** is a binary feature with 2 values: **Yes** and **No**, where **Yes** means the session resulted in a purchase and **No** means the session has no purchase. **=> Assumption 1 is satisfied**

### Assumption 2: The observations are independent ###

According to the dataset information from the UCI Machine Learning Repository, the dataset was constructed so that each session belongs to a different user within a one-year period. This design prevents any bias toward a specific campaign, special day, user profile, or time period. ([SOURCE](https://archive.ics.uci.edu/dataset/468/online+shoppers+purchasing+intention+dataset)) **=> Assumption 2 is sastified**

### Assumption 3: There is No Multicollinearity Among Explanatory Variables ###

To check for multicollinearity among independent features, I examined the correlation matrix and calculated the Variance Inflation Factor (VIF) for each feature.
<p align="center">
  <img src="Images/Correlation.png" alt="Flowchart" width="500">
</p>

According to the Pearson correlation, *BounceRates* and *ExitRates* are highly correlated **(0.91)**, and *ProductRelated* and *ProductRelated_Duration* also show a strong correlation **(0.86)**.

<table align="center">
  <tr>
    <th>Feature</th>
    <th>VIF</th>
  </tr>
  <tr>
    <td align="center">ProductRelated</td>
    <td align="center">4.430363</td>
  </tr>
  <tr>
    <td align="center">ProductRelated_Duration</td>
    <td align="center">4.330691</td>
  </tr>
  <tr>
    <td align="center">BounceRates</td>
    <td align="center">6.410740</td>
  </tr>
  <tr>
    <td align="center">ExitRates</td>
    <td align="center">7.151757</td>
  </tr>
</table>

Besides Pearson correlation, the Variance Inflation Factor (VIF) test is also a useful method to detect multicollinearity. As expected, the features *ProductRelated*, *ProductRelated_Duration*, *BounceRates*, and *ExitRates* exhibit relatively high VIF scores, ranging from **4.4** to **7.1**.

**=> These four features should be preprocessed to satisfy Assumption 3.**

**Refinement:** 

While both *BounceRates* and *ExitRates* are related to how customers leave a page and are measured on the same scale (0–1), I created a new feature called *EngagementIndex*. This feature reflects the level of user engagement, with higher values indicating that users are more likely to stay and interact with the page. It can be calculated using the following formula:
<p align="center">
  <img src="Images/EngamentIndex.png" alt="Flowchart" width="500">
</p>

For *ProductRelated* and *ProductRelated_Duration*, I could not merge these two features because they are not measured on the same scale. Therefore, I removed one of them; in this case, *ProductRelated* was removed because it had a higher **VIF score (4.4 > 4.3)**.

After that, I used the Pearson correlation again to check for correlations. This time, none of the independent features had **a correlation higher than 0.7**. I also performed the VIF test again, and none of the features had **a VIF score higher than 1**.

<table align="center">
  <tr>
    <th>Variable</th>
    <th>VIF</th>
  </tr>
  <tr>
    <td align="center">ProductRelated_Duration</td>
    <td align="center">1.081645</td>
  </tr>
  <tr>
    <td align="center">Month</td>
    <td align="center">1.016627</td>
  </tr>
  <tr>
    <td align="center">OperatingSystems</td>
    <td align="center">1.158841</td>
  </tr>
  <tr>
    <td align="center">Browser</td>
    <td align="center">1.148238</td>
  </tr>
  <tr>
    <td align="center">Region</td>
    <td align="center">1.022907</td>
  </tr>
  <tr>
    <td align="center">TrafficType</td>
    <td align="center">1.077766</td>
  </tr>
  <tr>
    <td align="center">Weekend</td>
    <td align="center">1.007259</td>
  </tr>
  <tr>
    <td align="center">VisitorType_Other</td>
    <td align="center">1.300107</td>
  </tr>
  <tr>
    <td align="center">VisitorType_Returning_Visitor</td>
    <td align="center">1.167817</td>
  </tr>
  <tr>
    <td align="center">EngagementIndex</td>
    <td align="center">1.149975</td>
  </tr>
</table>


**=>> Assumption 3 is sastified**



