# COMS W3132 Individual Project

## Project Title
*Quantifying News Sentiment and Its Correlation with Stock Price Changes*

## Project Description
This project aims to answer the question, "Are news sentiment and stock prices correlated?" by developing a program to 1) analyze sentiment about a public company in news, and 2) determine whether there is a correlation between sentiment and stock price. The motivation for the project is to utilize computational power and machine learning algorithms to answer the research question. The question is important as it is widely believed that market sentiment is one of the main factors that drive price (note that due to the limitation of lack of market sentiment data, I am assuming that market sentiment can be extracted from financial news). Therefore, being able to answer this question analytically may provide insight to stock price fluctuations.

The analytical procedure is broken down into 3 parts: 1) train a model to quantify sentiment in news, 2) use the model to predict sentiment in historical news articles, and 3) calculate and visualize the correlation between sentiment and stock price changes.

Model-training involves data wrangling, feature extraction, and model selection. Train and test data is from FinancialPhraseBank-v1.0 (https://huggingface.co/datasets/takala/financial_phrasebank). Feature extraction is done via TF-IDF (Term Frequency-Inverse Document Frequency), where a feature matrix is produced. Then, features will be fed to a Multinomial Logistic Regression classifier for training. The classifier is trained to classify a sentence to have positive sentiment (+1), neutral sentiment (0), or negative sentiment (-1). GridSearch is used to perform cross validation and select the best combination of parameters for the classifier. The best model is tested for accuracy using test data. This is a very basic model. If time permits, other models such as bert will also be considered.

Historical news sentiment prediction involves data wrangling and applying the selected model from step 1 to processed text. Historical financial news articles are from https://huggingface.co/datasets/ashraq/financial-news-articles. Data cleaning utilizes regular expression functions to extract information such as date, newspaper, and public company name, from text such as url and article title. The cleaned data is stored in and returned as data frames. In terms of sentiment prediction, each article is broken into sentences, and each sentence is evaluated for sentiment (+1, 0, -1). Non-zero sentiments are averaged to obtain an overall sentiment score for the article. After that, data is again aggregated by date and the mean is taken, so that the final data frame contains time series of sentiment scores. For visualization, a bar chart where blue bars represent positive sentiment and red bars represent negative sentiment can also be obtained.

Step 3 uses the sentiment scores obtained from the step above and historical prices to calculate and visualize correlation. Stock prices can be obtained via API at https://www.alphavantage.co/query?. Requested data is downloaded in json file and then transformed to a data frame. Sentiment scores and stock price data are merged into one data frame. Prices for the dates when the news were published are joined, as well as prices two days prior to the publication, and prices three days prior. The rationale behind this algorithm is that the articles in the historical news data all come from sources such as wsj, reuters, where publication of information is delayed by 2 to 3 days, meaning journalists receive information from sources other than wsj and reuters on day 0 and then write an artile. By the time the publication is out in wsj, the information has already been available to the public for about 2 to 3 days. Such information should already be reflected in the stock price. Therefore, price change percentage is calculated by: (the price on the date of publication (aka sentiment date) - the price two or three days prior)/ the price two or three days prior * 100. Lastly, correlation between sentiment and stock prices is calculated using Python built-in function, and scatter plot and time series plot are produced as visual aids.

The main.ipynb file contains functions and classes designed to analyze any major public companies. This is possible because the historical news dataset is diverse and stock prices of different symbols are available through API at https://www.alphavantage.co.


## Timeline
*To track progress on the project, we will use the following intermediate milestones for your overall project. Each milestone will be marked with a tag in the git repository, and we will check progress and provide feedback at key milestones.*

| Date               | Milestone                                                                                              | Deliverables                        | Git tag    |
|--------------------|--------------------------------------------------------------------------------------------------------|-------------------------------------|------------|
| **July&nbsp;15**   | Submit project description                                                                             | README.md                           | proposal   |
| **July&nbsp;17**   | Update project scope/direction based on instructor/TA feedback                                         | README.md                           | approved   |
| **July&nbsp;22**   | Basic project structure with empty functions/classes (incomplete implementation), architecture diagram | Source code, comments, docs         | milestone1 |
| **August&nbsp;2**  | More or less complete implementation. The goal is to have something you can share with others.         | Source code, unit tests             | milestone2 |
| **August&nbsp;9**  | Complete implementation. Final touches (conclusion, documentation, testing, etc.)                      | Source code, Conclusion (README.md) | conclusion |

*The column Deliverables lists deliverable suggestions, but you can choose your own, depending on the type of your project.*

## Technical Specification
In the environment, pip install the following: pandas, numpy, sklearn, seaborn, matplotlib, pyarrow, torch, transformers
Other modules required: re, requests, json.

## Development Methodology
I will use GitHub projects board to track progress on tasks and milestones and GitHub issues to keep track of issues or problems. I will test the program manually by sampling a well-known publicly traded company.

## Potential Challenges and Roadblocks
Getting relevant data such as market sentiment data.

## Conclusion and Future Work

Things that can be improved in this project: 
- Training data for model_1 and historical financial news may not be the same kind of data because they are from different sources. This might have led to unreliable prediction. A way to remediate would be to use news articles that are as close to the training data as possible. Or, to use model_2, FinBERT, which is trained on more diverse data - financial news and the FiQA dataset.
- How to accurately calculate the sentiment of a newspaper article: The "weights" of sentiments in non-neutral sentences differ, as they contain different kinds of information. For example, sentence 1 might discuss progress in product development, while sentence 2 might mention the hiring of a new director. The former could have a greater impact on traders' decisions than the latter. However, the current model assumes equal weights, and assigning both a score of 1. As the next step, I plan to explore using a regression model that assigns continuous numerical values as scores, provided training data is available. 
- Identifying relevant articles given a company name: the current method involves checking if the company name appears in the title of an article to determine if the content pertains to that company. However, this approach can be misleading, especially when an article discusses multiple public companies. In such cases, the sentiments expressed in the article may not be solely directed at one company. A more sophisticated approach would involve using Named Entity Recognition (NER) to extract the names of organizations or companies mentioned in the article titles. If more than one company name is identified, the article would be excluded, as it becomes challenging to ascertain which company the article primarily addresses.
- High percentage of missing data in the final dataframe: after sentiments and stock prices are combined into one data frame, 2- or 3- day price differences are calculated to caputure price fluctuations. However, due to missing stock prices on certain dates, 45% of the multiday-lag price changes cannot be calculated. A potential solution would be to obtain more complete financial data from other sources.
