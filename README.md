# Email Marketing Campaign 

Optimizing marketing campaigns is one of the most common data science tasks. Among the many marketing tools available, emails stand out as particularly efficient.Emails are great because they are free, scalable, and can be easily personalized. Email optimization involves personalizing the content and/or the subject line, selecting the recipients, and determining the timing of the sends, among other factors. Python libraries like NumPy, Pandas, Matplotlib, Seaborn, sklearn.ensemble, XGBoost and many other excel at analyzing the factors and maximize the probability of users clicking on the link inside the email.

## Dataset Description

This dataset contains information from an email marketing campaign and is composed of **three relational tables**:

### 1. `email_table.csv`
Provides metadata about each email sent during the campaign.

| Column Name           | Description                                                                 |
|-----------------------|-----------------------------------------------------------------------------|
| `email_id`            | Unique identifier for each email sent.                                      |
| `email_text`          | Describes whether the email had a "short" or "long" body text.              |
| `email_version`       | Indicates if the email was "personalized" (with user's name) or "generic".  |
| `hour`                | The local hour when the email was sent.                                     |
| `weekday`             | The day of the week the email was sent.                                     |
| `user_country`        | Country of the user receiving the email (based on IP address).              |
| `user_past_purchases` | Number of past purchases by the user.                                       |

### 2. `email_opened_table.csv`
Tracks which emails were opened by users (clicked and presumably read).

| Column Name | Description                     |
|-------------|---------------------------------|
| `email_id`  | ID of the email that was opened |

### 3. `link_clicked_table.csv`
Logs emails where the internal link was clicked by the user.

| Column Name | Description                              |
|-------------|------------------------------------------|
| `email_id`  | ID of the email whose link was clicked   |

---

## Model Selection

To maximize the probability of users clicking on the link inside the email, I have prefered Differential Evolution (SVM) model against multiple models. The Differential Evolution (SVM) is a metaheuristics approach that combines the power of Support Vector Machines (SVM) with Differential Evolution (DE), an evolutionary optimization algorithm. In this hybrid method, DE is used to automatically optimize the hyperparameters of the SVM, such as the penalty parameter (C), kernel type, and kernel-specific parameters like gamma. This helps in finding the most effective model configuration without manual tuning. The approach is particularly useful for handling complex, high-dimensional, or non-linear datasets, offering improved classification accuracy through global search and optimization.

I have implemented other models such as SVM, XGBoost, Random Forest Classifier, and Neural Network. I created a stimulate_ctr (Click-Through Rate) function to compare the predicted CTR with the original CTR and displayed the top 20% of the results to analyze the improvements achieved by each model.


### Model Performance: CTR Simulation Comparison

The following table summarizes the Click-Through Rate (CTR) improvement achieved by different models. The `stimulate_ctr` function was used to simulate the predicted CTR, and the top 20% of results were analyzed to estimate performance improvements.

| Model                        | Original CTR | Simulated CTR (Top 20%)   | Estimated CTR Improvement  |
|------------------------------|--------------|---------------------------|----------------------------|
| Random Forest Classifier     | 1.94%        | 2.44%                     | +0.50%                     |
| XGBoost Classifier           | 1.94%        | 3.94%                     | +2.00%                     |
| Logistic Regression          | 1.94%        | 4.76%                     | +2.82%                     |
| Neural Network (Keras)       | 1.94%        | 4.38%                     | +2.44%                     |
| Support Vector Machine       | 1.94%        | 2.00%                     | +0.06%                     |
| SVM (Differential Evolution) | 45.00%       | 95.00%                    | +50.00%                    |

### The following points confirm that model is not overfitting

1. **Cross-validated accuracy:** `0.866` - This score indicates that the model performs consistently well across different splits of the data, suggesting good generalization ability


2. **Class Distribution:** `y_test` - 

- 0 - 55<br>
- 1 - 45

The test set has a fairly balanced class distribution, which helps avoid biased accuracy.

3. **Sample Predicted Probabilities**

```
    [[0.8839, 0.1160],
    [0.9033, 0.0966],
    [0.9811, 0.0188],
    [0.7224, 0.2775],
    [0.8557, 0.1442],
    [0.0137, 0.9862],
    [0.0029, 0.9970],
    [0.2867, 0.7132],
    [0.2407, 0.7592],
    [0.6353, 0.3646]]

```

The predicted probabilities are well-distributed and not overly confident in one class, suggesting the model is making informed predictions.

#### The above combinations of a high cross-validated accuracy (0.866), a balanced test set, and reasonable probability outputs confirms that the model is not overfitting and generalizes well across unseen data.
---
## Question and answer

### 1. What percentage of users opened the email and what percentage clicked on the link within the email?

![image](https://github.com/user-attachments/assets/4f79b3db-7c79-4e10-a142-e96971d2f33d)

**Thus,**
- Percentage of users who opened the email: `10.41%`
- Percentage of users who clicked the link: `2.10%`
---
### 2. The VP of marketing thinks that it is stupid to send emails in a random way. Based on all the information you have about the emails that were sent, can you build a model to optimize in future how to send emails to maximize the probability of users clicking on the link inside the email? 

The VP of Marketing raised a valid concern — sending emails randomly is inefficient and fails to capitalize on user behavior data. To address this, I built a `predictive model` that uses past campaign data to `optimize email targeting` and `maximize Click-Through Rate (CTR)`.

#### I experimented with multiple classifiers:
   - Logistic Regression
   - Random Forest
   - XGBoost
   - Neural Networks
   - SVM
   - SVM (with Differential Evolution optimization)

##### The results will show the following details:

**Original CTR:** The baseline CTR for the entire dataset.

**Simulated CTR:** The CTR of the top 20% of predicted users.

**Improvement:** The difference between the simulated and original CTR.

Conclusion - Through the above CTR table (present under model selection) we can clearly note that SVM DE model has predicted the best result among all the other model. The improved CTR score of DE model is 50% more than the orginal CTR.

---

### 3. By how much do you think your model would improve click through rate. How would you test that? 

By implementing the simulate_ctr function, we are able to evaluate how much the model can improve the Click-Through Rate (CTR) by focusing on the top 20% of predicted probabilities. Here's how the process works:

#### Steps to Test and Simulate CTR Improvement:
**Predict Probabilities:** The model predicts probabilities for the test set using predict_proba, which gives the likelihood of each click (binary classification).

**Create Test DataFrame:** I create a DataFrame containing actual click labels (y_test) and the predicted probabilities (predicted_proba).

**Top 20% Users:** I simulate targeting the top 20% of users with the highest predicted probabilities (the users most likely to click).

##### Calculate CTR:

**Original CTR:** The proportion of actual clicks in the entire test set.

**Simulated CTR:** The proportion of actual clicks in the top `20%` based on the predicted probabilities.

**CTR Improvement:** The improvement is calculated as the difference between the Simulated CTR and the Original CTR.

#### Testing the Improvement:
We can run this simulation for different models (e.g., Random Forest, XGBoost, SVM) to compare their performance in improving CTR.

**Results**
Models like XGBoost and Neural Networks significantly outperformed random targeting, improving simulated CTR by `up to 2.82%` compared to the baseline. The best results were achieved using SVM with metaheuristic optimization (Differential Evolution), improving CTR by up to `50%` in a balanced test set.

#### This helps in assessing the model’s ability to target the most likely users for a click, which is very important for effective email marketing campaign.
---
### 4. Did you find any interesting pattern on how the email campaign performed for different segments of users? Explain. 

#### Insights from Email Campaign Performance by User Segments

Yes, several interesting patterns were discovered when analyzing how the email campaign performed across different user segments. These insights help us understand **which groups are more likely to engage** with email content and can guide future targeting strategies.

##### Based on `email_text` (Long vs Short):

 **Email Acknowledged**
- **Short emails** resulted in slightly higher acknowledgment:
  - **Short email acknowledgment count**: 918
  - **Long email acknowledgment count**: 762
- This suggests users may prefer concise messages when deciding to act on an email.

**Email Opened**
- **Short emails** again performed better:
  - **Short email open count**: 1832
  - **Long email open count**: 1484
- Users appear more likely to open short emails, possibly due to quick readability or clearer subject lines.

##### Based on `email_version` (Generic vs Personalized):

**Email Acknowledged**
- **Personalized emails** had significantly higher acknowledgment:
  - **Personalized acknowledgment count**: 1150
  - **Generic acknowledgment count**: 410
- Personalized content nearly **tripled** the user acknowledgment rate, showing a clear benefit to customization.

**Email Opened**
- **Personalized emails** were opened far more often:
  - **Personalized open count**: 2291
  - **Generic open count**: 769
- This reinforces that **personalization increases user engagement** even at the open stage.

**Common Patterns Across Segments:**
- **Optimal open time** appears around **9–10 AM**, consistent across all categories.
- **User past purchases** and engagement history slightly differ but show similar median values, suggesting other factors (like content type) might play a bigger role.

##### Country-wise Engagement Analysis

**Email Interaction by Country (`email_status`)**
| Country     | Total Emails Sent | Email Status Sum | Mean Status | Engagement Rate |
|-------------|-------------------|------------------|-------------|-----------------|
| ES (Spain)  | 4031              | 184              | 0.05        | **Low**         |
| FR (France) | 3998              | 192              | 0.05        | **Low**         |
| UK          | 7963              | 1179             | 0.15        | **Moderate**    |
| US          | 23932             | 3441             | 0.14        | **Moderate**    |

- **Insight**: The **UK and US** show **significantly higher user engagement** compared to **Spain and France**.
- **Possible reason**: Differences in user preferences, localization, or cultural factors might be influencing engagement.

##### Time-based Metrics by Country
- Median `hour` of interaction: `9 AM` for all countries.
- Users tend to interact with emails in the `morning`, regardless of their location.
- Average `user_past_purchases` is slightly higher in `Spain (3.98)` and `France (3.87)` than in `UK (3.81)` and `US (3.89)`, but this doesn’t necessarily translate to higher email interaction.

##### Weekday-wise Email Performance

**Email Status Breakdown by Weekday**
| Day       | Status 0 (Ignored) | Status 1 (Opened) | Status 2 (Clicked)  | Total Engagement (1+2) |
|-----------|--------------------|-------------------|---------------------|------------------------|
| Monday    | 5169               | 523               | 130                 | **653**                |
| Tuesday   | 5049               | 570               | 137                 | **707**                |
| Wednesday | 4936               | 556               | 159                 | **715**                |
| Thursday  | 5003               | 503               | 148                 | **651**                |
| Friday    | 5215               | 331               | 73                  | **404**                |
| Saturday  | 5196               | 388               | 99                  | **487**                |
| Sunday    | 5200               | 445               | 94                  | **539**                |

- **Best performing day**: **Wednesday**, with `highest total engagement (715)`.
- **Worst performing day**: **Friday**, with only `404 engaged users`.
- **Insight**: Sending marketing emails mid-week (especially **Tuesday to Thursday**) could yield better click-through and open rates.


##### Summary of Key Insights

- Personalized content and short email perform better across the board.
- UK and US audiences are more responsive than Spain and France.
- Midweek emails (Tue–Thu), especially on Wednesday, show higher engagement.
- Morning hours (around 9 AM) are optimal for sending campaigns.
- Although purchase history is relatively consistent, content and timing appear to drive CTR more than historical purchases.

These insights can guide smarter email targeting strategies, improve CTR, and reduce campaign waste by avoiding less responsive segments or timings.

## ThankYou
