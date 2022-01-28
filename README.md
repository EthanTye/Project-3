# Subreddit Classification: r/TheOnion and r/nottheonion


##Problem Statement
Fake news is a prevalent and harmful problem in modern society, often misleading the general public on important topics and polices, such as healthcare or taxes. 
Such misinformation can erode trust in government institutions or news agencies and result in deep seated and persistent societal issues that have a negative impact on public safety and well being.


Our team aims to develop an model using Natural Language Processing and Classification Models that can accurately classify if an
article contains real news or fake news based on the headline.

## Background

To create a model we collected the headlines of two subreddits, The Onion and Not The Onion. 
The onion subreddit is a collection of post that contains the headlines and links to articles produced by the satire website "The Onion" 
that publishes fictatious articles written to emulate real news articles.

Not the Onion on the other hand is a subreddit that contains posts that are true but hard for readers to believe.

By collecting the post data from these two subreddits we can build a collection of headlines of fake news articles and another with only real news.

## Datasets

To extract the data we used Pushshift API to compile 2 datasets of 5000 posts each from the onion and not the onion subreddits and stored them into a csv file.

The original datasets are:
- 'nottheonion.csv'
- 'theonion.csv'

The datasets were then combined into single file, cleaned, and un-utilised columns dropped.

The various datasets used in books 1 to 3 as follows.

| Dataset               | Description                      | clean/unprocessed | Remarks                                          |Created   |
|-----------------------|----------------------------------|-------------------|--------------------------------------------------|--------|
| combined.csv          | combination of onion and notonion| unprocessed       |                                                  | Book 1 |
| combined_cleaned.csv  | combination of onion and notonion| cleaned           |                                                  | Book 2 |
| combined_cleaned2.csv | combination of onion and notonion| cleaned           | Additional duplicates dropped after tokenization | Book 3 |

## Executive Summary

This project is split between the following code books:
1) Data Extraction
2) EDA/Cleaning
3) Modelling, tuning and model evaluation (with some light cleaning)

In book 1 we used Pushshift API to extract the details of 5,000 posts each from theOnion and NottheOnion subreddits.

During our EDA in book 2 we discovered that the dataset contained a small number of duplicated titles, titles that were not in english or had titles that contained a just a single word/character.
As such, some cleaning was required to drop these samples. Post cleaning the resultant dataset was slightly smaller consisting a combined total of ~9000 posts (~4.5k samples from each subreddit).

In book 3 we tokenized, lemmatized and vectorized the remaining posts titles before fitting them onto several classification models.

As the consequences of false positives and false negatives are equally severe, we model has to balance both precision and recall. 
F1 score works well as a metric as it combines both our requirements into a single metric.

The first task was to set a baseline model that we could use to assess model performance against. 
Since the number of samples from each class was balanced, we built a dummy classifier that used the mean weightage of each class as an estimator to produce a baseline score of 0.5.

Next we vectorized the titles of the various posts into several classification models and evaluated them based on accuracy score.
After the 1st round of evaluations the 3 best performing models were subsequently tuned to optimize performance as measure by f1 score.

Please refer to the following table for a breakdown of model performance.

| Model                   | Train - pretuning | Test - Pretuning | Train - Post Tuning(f1) | Test- Post Tuning(f1) |
|-------------------------|-------------------|------------------|-------------------------|-----------------------|
| Baseline                | 0.5               | 0.5              | -                       | -                     |
| Logistic Regression     | 0.98              | 0.82             | 0.81                    | 0.82                  |
| Multinomial Naive Bayes | 0.93              | 0.80             | 0.81                    | 0.82                  |
| Random Forest           | -                 | 0.78             | 0.80                    | 0.80                  |

With model performance as the primary condition Multinomial Naive Bayes paired with Countvectorizer was selected as it had the best performance,
edging out the LR model by a small margin.

## Conclusions

Overall the MNB, LR and RF models have generally comparable with f1 scores of ~ 0.8.

Multinomial Naive Bayes was chosen as the model was able to achieve the highest f1 score of 0.82. 
Plotting the ROC curve the model boasts an AUC of 0.9 or a 90% chance that it can distinguish between a post from the Onion and not the Onion.

Considering that the model is intended to be used as a way to distinguish between real and fake news, 
the stakes are relatively high since a single misclassified article/headline could have severe and far reaching consequences.

With a 20% error rate it would be risky to use the model to classify between real and fake news as it currently is and margin of error should be lowered further before 
using the model to classify between real and fake news.
In addition, as the headlines for fictional articles was taken from a single source, 
the model could be overfit to the the editorial choices (like content focus) of the Onion could negatively impact models ability to process fake news from other sources.


## Recommendations

Further improvements to lower the margin of error is recommended.

First, additional samples of both real and fake news headlines from a variety of sources should be collected and used to develop the model further.

Secondly, the model is built based on data collected before 01 Jan 2022, as such the relevancy of the corpus used to train the model will become an issue as time passes and 
new global developments (such as covid) dominate future news cycles or with the introduction of new terminology (such as the shift from the term sars-cov2 to the use of covid 19).

Finally, the Multinomial Naive Bayes is not ideal when interpretability is required or when the corpus is expected to grow.
For MNB feature log probs become problematic when there is a large number of unique features that result in very small probabilities for a given class, this problem is likely to grow as additional data points are introduced. 
In contrast, switching to a model like logistic regression, where feature regression coefficients
are easier to interpret that also has the added benefit of  regularization algorithms that can mitigate the impact of increasing the language corpus can be mitigated. 


