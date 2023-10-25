# NLP Text Classification
Analyzing Twitter Posts Regarding Apple and Google

David Hartsman
Reyn Chagami
Andrew Hendricks (andrewhhendricks@gmail.com)

Find our non-technical presentation [here](https://docs.google.com/presentation/d/12J7u8S0OZltTBgUNQsBwZ3RKaHuetXo6/edit?usp=sharing&ouid=106491021188736703963&rtpof=true&sd=true).

## Overview
Social media provides organizations with more data than ever about how the public is engaging with their products and brands. Using text classification to process all of that data into useable insights gives organizations the opportunity to incorporate public perceptions into their operations in new and unprecedented ways.

In this project, we take on the role of a consulting firm creating an NLP text classification prototype that we are pitching to the heads of the analytics and operations teams for SXSW 2024. We use Twitter data from the 2013 SXSW conference to create a model that classifies positive, negative, and neutral Tweet sentiment. We conclude by sharing ideas for how the SXSW operations team could use the results of our model to enhance future conferences and ideas for next steps.

## Business Problem and Data
The stakeholders for this project are the SXSW 2024 operations team and analytics team, and the business problem is developing a model that classifies Tweets based on positive, negative, or neutral sentiment. Based on the business context, we develop three separate models: a binary model to classify positive Tweets, a binary model to classify negative Tweets, and a multi-class model to classify positive, negative, and neutral Tweets. We train these models on 8,936 labeled Tweets from the 2013 SXSW conference. The Tweets reference products from Apple and Google, and 60% of them are labeled neutral, 33% are positive, and 6% are negative. 

Only about a third of the records original records include brand labels. While we do not use the brand labels in our models, we impute the missing brands using keywords from the Tweet text to allow us to disaggregate the data by brands in future analyses.

<img width="419" alt="Screenshot 2023-10-25 140158" src="https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/91885d05-61d0-47f2-a7e1-5e335acd0227">

Before modeling, we create a preprocessing function to prepare the Tweets for classification. The function uses a regex pattern and tokenizer to tokenize each Tweet. After tokenization, the function lower cases each token and removes stopwards and punctuation from the token list. The function then applies part of speech tags to each of the tokens before using the WordNetLemmatizer to lemmatize them. 

<img width="506" alt="Screenshot 2023-10-25 142043" src="https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/7037a72f-e7cf-429f-ae70-8e8196e29584">


## Modeling

Our project includes three models that are each intended to serve different use cases. To develop each of the models, we use an iterative approach that includes Multinomial Naive Bayes, Random Forest, logistic regression, ADA Boost, and XG Boost models. For each model type, we try separate models using count vectorization and TFIDF vectorization. We also try models that use SMOTE to address the imbalance in the data.

The purpose of our first model is to detect positive Tweets. The operations team could use this to share positive public response with participating companies, which they could repost on their social media accounts, or to publicize positive reactions to the event to attract new companies to present at SXSW in the future.  For positive Tweet classification, we use a one-versus-rest binary classification model and maximize precision because we wanted to make sure that if the operations team is sharing positive social media posts or statistics with companies or publicing them on their own social media accounts that the Tweets actually are positive. 

Here is the ROC-AUC curve for our top 5 models:
![precision_top_5](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/8a6cb247-9dee-4995-a862-82e181aa64ba)

Our strongest model for this purpose is a Mutlinomial Naive Bayes classifier that uses tfidf vectorization. The model precision for train data is 68% and the precision for test data is 73%. Of the Tweets that it classified as positive, 163 actually were positive and 59 were false positives.

![conf_mat_precision_winner](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/af39d80e-042c-473a-ba53-c329e2a4b4ef)

![individual_precision_winner_OVR](https://github.com/dvdhartsman/NLP-Sentiment-Analysis/assets/141271148/af6d5cc3-2415-4cd2-9625-971d54cd94ab)

Our second model detects negative Tweets to support the operations team in detecting operational issues that people are complaining about publicly on social media and responding efficiently. Once again, we use a one-versus-rest binary classification model, but for this model, we focus on recall score to optimize the percentage of actually negative Tweets that the model detects. We chose this metric because we want to minimize the negative Tweets that the model does not detect.

Here is the ROC-AUC curve for our top 5 models:

Our strongest model for this purpose is a Mutlinomial Naive Bayes classifier that uses tfidf vectorization. The model precision for train data is 68% and the precision for test data is 73%. Of the Tweets that it classified as positive, 163 actually were positive and 59 were false positives.

Finally, our third model is a multi-class classifier that distinguishes between positive, negative, and neutral Tweets. The operations team could use this model to gain insights about public reaction to the products and presentations that companies share throughout the conference. This would allow the team both to share these insights with companies to help them quantify the impact of presenting at SXSW on social media response. The analysis could also be used internally to factor into the decision of determining which companies to prioritize recruiting for future conferences along with any companies that may have generated negative response and should not be invited back. Given the purpose of the model, our key metric is accuracy score to make sure . 



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
