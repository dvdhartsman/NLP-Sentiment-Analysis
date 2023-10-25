# NLP Text Classification
Analyzing Twitter Posts Regarding Apple and Google

David Hartsman
Reyn Chagami
Andrew Hendricks (andrewhhendricks@gmail.com)

Find our non-technical presentation [here](https://docs.google.com/presentation/d/12J7u8S0OZltTBgUNQsBwZ3RKaHuetXo6/edit?usp=sharing&ouid=106491021188736703963&rtpof=true&sd=true).

## Overview
In the age of social media, organizations have access to more data than ever about how the public is engaging with their products and brands. Using text classification to process all of that data into useable insights gives organizations the opportunity to incorporate public perceptions into their operations in new and unprecedented ways.

In this project, we take on the role of a consulting firm creating an NLP text classification prototype that we are pitching to the heads of the analytics and operations teams for SXSW 2024. We use Twitter data from the 2013 SXSW conference to create a model that classifies positive, negative, and neutral Tweet sentiment. We conclude by sharing ideas for how the SXSW operations team could use the results of our model to enhance future conferences and ideas for next steps.

## Business Problem and Data
The stakeholders for this project are the SXSW 2024 operations team and analytics team, and the business problem is developing a model that classifies Tweets based on positive, negative, or neutral sentiment. Based on the business context, we developed three separate models: a binary model to classify positive Tweets, a binary model to classify negative Tweets, and a multi-class model to classify positive, negative, and neutral Tweets. We trained these models on 8,936 labeled Tweets that came from the SXSW 2013 conference. The Tweets referenced products and presentations by Apple and Google. 60% of the Tweets were labeled as neutral, 33% were positive, and 6% were negative.

CELL 9

Only about a third of the records original records included brand labels. While we did not use the brand labels in our models, we imputed the missing brands using keywords from the Tweet text so that we would have the ability to disaggregate the data by brands in future analyses.

CELL 16

The purpose of our first model is to detect positive Tweets. The operations team could use this to share positive public response with participating companies, which they could repost on their social media accounts. It would also allow the operations team to publicize positive reactions to the event, which could be useful in attracting top level companies to present at SXSW in the future.  For positive Tweet classification, we used a one-versus-rest binary classification model that maximizes precision because we wanted to make sure that if the operations team is sharing positive social media posts or statistics with companies or publicing them on their own social media accounts that the Tweets actually are positive.

We designed our second model to detect negative Tweets. Once again, we used a one-versus-rest binary classification model, but for this model, we focused on recall score to optimize the percentage of actually negative Tweets that the model detects. The operations team could use this model during the week of the conference to detect operational issues that people are complaining about publicly on social media. For instance, if attendees are posting online about long lines or overflowing garbage or a lack of seating, the opertaions team could identify the posts, address the issues, and post responses to maintain the positive image of the conference.

Finally, we designed our third model as a multi-class classifier to distinguish between positive, negative, and neutral Tweets. Our key metric for this model was accuracy score to make sure the model classifies Tweet sentiment with the greatest possible overall accuracy. The operations team could use this model to gain insights about public reaction to the products and presentations that companies share throughout the conference. This would allow the team both to share these insights with companies to help them quantify the impact of presenting at SXSW on social media response. The analysis could also be used internally to factor into the decision of determining which companies to prioritize recruiting for future conferences along with any companies that may have generated negative response and should not be invited back. 

problem Because of the nature of the problem, my model predicts whether someone will get an H1N1 vaccine, and I use accuracy as my most important classification metric. There is no domain-specific need for the model to do a better job at avoiding false positives or false negatives, so my goal is to create a model that is as accurate as I can make it.
This analysis is based on the prompt from the Driven Data Flu Shot Competition. The data for the analysis was hosted there as well. The survey that the data comes from was conducted during 2009. The survey was conducted using random digit dialing and targeted all people ages 6 months and older. The survey contained over 26,000 records.

All of the columns in the data set could be treated as categoricals. After data cleaning, I used one hot encoding on all non-binary columns. I used a simple imputer set to most frequent to handle null values.
Modeling
For my model, class 1 was that someone got the H1N1 vaccine and class 0 was that they did not. I used test train split to train and test my model and used pipelines to handle one hot encoding and simple imputing. My most important classification metric was the accuracy score, but I also reviewed precision, recall, F1, ROC AUC, and log loss when evaluating the preformance of my models.
My first model was a dummy classifier. The majority class within the data set was class zero at 79%, so the classifier had a 79% accuracy score.

My second model used a logistic regression on all columns.

My third model used a logistic regression on columns that I selected after using lasso regularization on all columns to determine which variables had the weakest impact.

My final model used polynomial features and ridge regularization on the selected columns from the third model.

Evaluation
Overall, the most accurate models were the first model and the second model, which had accuracy scores of 84%. These models also had the lowest log loss. The third model had the highest recall score, but it had lower precision and accuracy than the first two models. Because accuracy score was my most important metric, the first two models were stronger. All of these models beat the baseline dummy classifier, which had an accuracy score of 79%.
To create my recommendations, I used investigated the strength of the coefficients for each variable from my first model. The first and second models had virtually identical performance in terms of all the classification metrics. I decided to use the first model because it was more inclusive.
Because the coefficients represented log odds, which have a low level of interpretability, I decided to convert them to probabilities. I used np.exp to transform the log odds into odds and then converted the odds into probabilities. This allowed me to use what I felt to be a more interpretable metric when communicating with a non-technical audience.
Conclusion
Based on my model, a doctor recommendation was the variable associated with the strongest increase in the probability that someone would take the vaccine. Further, survey questions about the risk of H1N1, the vaccine's effectiveness, and the likelihood of getting sick from the vaccine all were associated with strong shifts in the probability of someone getting the vaccine. When people responded that the risk of the virus was low, that response was associated with a 57% decrease in the probability of them getting the vaccine; when people responded that the risk of the virus was high, that response was associated with an 83% increase in the probability of them getting a vaccine. Finally, generally healthy behaviors like wearing a face mask were also associated with an increase in the probability that someone would get the vaccine.
Based on these findings, my recommendations to public health officials would be: 1: Doctors should be vocal supporters of vaccines when pandemics are going on. 2: Fight the war of opinion by messaging on pandemic risks, vaccine effectiveness, and vaccine safety. 3: Promote generally healthy behaviors like frequently washing hands when pandemics are not occurring to increase health consciousness.