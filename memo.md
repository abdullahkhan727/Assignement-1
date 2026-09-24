To: Devon Achebe, VP of Customer Retention
Subject: Recommended Churn-Risk Model for Retention Outreach

I recommend using the Boosted Trees model to identify customers for Summit Telecom's retention contact list. Boosted Trees had the highest final test AUC at 0.8497, compared to 0.8471 for Logistic Regression and 0.7373 for Contract Rule. Its AUC advantage over contract rule was 0.1124. The paired bootstrap interval was from 0.0959 to 0.1311 which did not cross zero. These results show that Boosted Trees ranked high-risk customers much better than contract rules.

The model also seems to be useful for the business constraint because the company can only contact 20 percent of customers. The top 20 selected by Boosted Trees had a churn rate of about 69.4% compared to the overall churn of 26.5%. This means the model concentrated many more likely churners into the group that would receive outreach.

One limitation is that the analysis predicts churn risk but does not show whether contacting a customer will actually prevent churn. The model can identify risk but the effectiveness of the retention offer still needs to be measured.

I would reconsider if future data showed weaker ranking performance, because logistic regression performed pretty similar while being easier to maintain. With our current data I believe Boosted Trees is the way to go. The next step I recommend is a randomized pilot comparing contacted and non contacted high-risk customers to measure if the outreach improves retention.

Thank you.