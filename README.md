# COMS W3132 Individual Project

## Author
*Kaimi Huang kh2908@cumc.columbia.edu*

## Project Title
*Quantifying News Sentiment and Its Correlation with Stock Prices through Sentiment Analysis*

## Project Description
*This project aims to answer the question, "Are news sentiment and stock prices correlated?" by developing a program to 1) analyze sentiment about a stock in news, and 2) determine whether there is a correlation between sentiment and stock price. The motivation for the project is to utilize computational power and machine learning algorithms to answer the research question. The question is important as it is widely believed that market sentiment is one of the main factors that drive price. Due to the limitation of lack of market sentiment data, I am assuming that market sentiment can be extracted from financial news. Therefore, being able to answer this question analytically using data may provide insight to stock price fluctuations.

The analytical procedure is broken down into 3 parts: 1) train a model to quantify sentiment in news, 2) predict sentiment in historical news articles using the model, and 3) calculate and visualize the correlation between sentiment and stock prices.

Model-training involves data wrangling, feature extraction, and model selection. Train and test data is from FinancialPhraseBank-v1.0 (https://huggingface.co/datasets/takala/financial_phrasebank). Feature extraction is done via TF-IDF (Term Frequency-Inverse Document Frequency), where a feature matrix is produced. Then, features will be fed to a Multinomial Logistic Regression classifier for training. The classifier is trained to classify a sentence to have positive sentiment (+1), neutral sentiment (0), or negative sentiment (-1). GridSearch is used to perform cross validation and select the best combination of parameters for the classifier. The best model is tested for accuracy using test data. This is a very basic model. If time permits, other models such as bert will also be considered.

Historical news sentiment prediction involves data wrangling and applying the selected model from step 1 to processed text. Historical financial news articles are from https://huggingface.co/datasets/ashraq/financial-news-articles. Data cleaning utilizes regular expression functions to extract information such as date, newspaper, and public company name, from text such as url and article title. The cleaned data is stored in and returned as data frames. In terms of sentiment prediction, each article is broken into sentences, and each sentence is evaluated for sentiment (+1, 0, -1). Non-zero sentiments are averaged to obtain an overall sentiment score for the article. After that, data is again aggregated by date and the mean is taken, so that the final data frame contains time series of sentiment scores. For visualization, a bar chart where blue bars represent positive sentiment and red bars represent negative sentiment can also be obtained.

Step 3 uses the sentiment scores obtained from the step above and historical prices to calculate and visualize correlation. Stock prices can be obtained via API at https://www.alphavantage.co/query?. Requested data is downloaded in json file and then transformed to a data frame. Sentiment scores and stock price data are merged into one data frame. Prices for the dates when the news were published are joined, as well as prices two days prior to the publication, and prices three days prior. The rationale behind this algorithm is that the articles in the historical news data all come from sources such as wsj, reuters, where publication of information is delayed by 2 to 3 days, meaning journalists receive information from sources other than wsj and reuters on day 0 and then write an artile. By the time the publication is out in wsj, the information has already been available to the public for about 2 to 3 days. Such information should already be reflected in the stock price. Therefore, price change percentage is calculated by: (the price on the date of publication (sentiment date) - the price two or three days prior)/ the price two or three days prior * 100. Lastly, correlation between sentiment and stock prices is calculated using Python built-in function, and scatter plot and time series plot are produced as visual aids.

The main.ipynb file contains functions and classes designed to analyze any major public companies. This is possible because the historical news dataset is diverse and stock prices of different symbols are available through API at https://www.alphavantage.co.
*

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

## Requirements, Features and User Stories
*List the key requirements or features of your project. For each feature, provide a user story or a simple scenario explaining how the feature will be used. You don't have to get this section right the first time. Your understanding of the problem and requirements will improve as you work on your project. It is okay (and desirable) to come back to this section and revise it as you learn more about the problem you are trying to solve. The first version of this section should reflect your understanding of your problem at the beginning of the project.*

*Also list any required hardware, software, on online services you will need. In specific cases, we might be able to lend you hardware or obtain online services. Please email the instructor for more details.*

## Technical Specification
*Libraries that will be used: pandas, numpy, sklearn, seaborn, matplotlib, re, requests, json. *

## System or Software Architecture Diagram
*Include a block-based diagram illustrating the architecture of your software or system. This should include major components, such as user interface elements, back-end services, and data storage, and show how they interact. Tools like Lucidchart, Draw.io, or even hand-drawn diagrams photographed and uploaded are acceptable. The purpose of the diagram is to help us understand the architecture of your solution. Diagram asthetics do not matter and will not be graded.*

## Development Methodology
*Describe the methodology you'll use to organize and progress your work.*

*First, describe your plan for developing your project. This might include how (or if) you plan to use*
- *GitHub projects board to track progress on tasks and milestones*
- *GitHub issues to keep track of issues or problems*
- *Separate Git branches and/or GitHub pull requests for development*
- *GitHub actions for automated testing or deployment pipelines*
- *GitHub wiki for documentation and notes*

*Please also describe how (if) you plan test and evaluate your project's functionality. Do you plan to test manually or automatically? Any specific testing frameworks or libraries you plan to use?*

## Potential Challenges and Roadblocks
* - getting relevant data such as market sentiment data*

## Additional Resources
*Include any additional resources, tutorials, or documentation that will be helpful for this project.*

## Conclusion and Future Work (to be updated)

* Things that can be improved in this project: 
- data: training data may not be the same data that the model is used to predict. This may make the predicted sentiment scores unreliable. 
- quantifying sentiment: how to properly calculate the sentiment of a piece of newspaper article? The "weight" of sentiment of non-neutral sentences differ as they carry different kinds of information, e.g., two positive sentences both receive the score of 1 as the model assumes equal weights. However, sentence 1 may influence traders' decisions more than sentence 2. As the next step, will consider using a model that assigns continuous numerical values as scores. *
