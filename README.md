# Earnings Call Natural Language Processing Analysis

### Goal:
To use Natural Language Processing to analyze earning call transcripts and consequent ticker quarter growth to predict performance as compared to respective index. 

### Earnings Call
Earnings call is a conference call between the management of a public company, analysts, investors, and the media to discuss the company’s financial results during a given reporting period, such as a quarter or a fiscal year. An earnings call is usually preceded by an earnings report. This contains summary information on financial performance for the period.

https://www.investopedia.com/terms/e/earnings-call.asp

### Companies analyzed in the project

Low to mid market cap technology companies on the NASDAQ. $300 M -> $10B

['ACIW', 'CEVA', 'CMTL', 'COMM', 'CPSI', 'CRUS', 'CSGS', 'CSOD', 'CVLT', 'DCT', 'DGII', 'DIOD', 'DMRC', 'DSGX', 'EBIX', 'EPAY', 'ERII', 'EVBG', 'EXTR', 'FEYE', 'FORM', 'LSCC', 'LTRPA', 'MANH', 'MARA', 'MDRX', 'MGRC', 'MIDD', 'MITK', 'MTSI', 'NATI', 'NH', 'NTCT', 'NTNX', 'NVEC', 'NXGN', 'OMCL', 'OSIS', 'PCTY', 'PDFS', 'PEGA', 'SLAB', 'SLP', 'SMCI', 'SMTC', 'SPSC', 'SPWR', 'SSYS', 'SWIR', 'SYKE', 'SYNA', 'TCX', 'TRIP', 'TRUE', 'TTEC', 'TTMI', 'TWOU', 'UCTT', 'UPLD', 'VECO', 'VIAV']

Index - VGT (Vanguard Information Technology Index Fund ETF); measures the entire technology sector as a whole.

Shameless plug - ETFs are amazing investments as they have low fees(as compared to mutual funds and other managed accounts) and they are extremely diversified(low risk).

### Modern Portfolio Theory - Why an index?

* TLDR: Harry Markowitz won a Nobel Prize by theorizing that diversification in the stock market is an efficient method of investing. His entire theory revolves around reducing risk(Std dev) by splitting assets while maximizing return. 
* Mutual funds and ETFs are built around this theory. What combination of weights minimizes risk while maximizing return. 
* When building models you cannot compare straight returns. Earning 10% return in a year is garbage if the market as a whole increased 15% that year. Most likely the investments were riding the bullish market.

# Process
1. Gather Data
2. Feature Engineering
3. Machine Learning
4. Takeaways

# Gathering Data

* Financial Modeling Prep API - For transcripts

https://financialmodelingprep.com/api/v3/earning_call_transcript/AAPL?quarter=3&year=2020&apikey=demo

* yFinance for stock data

# Feature Engineering

### 1. Language Measurements
* Felsch-Kincaid : 100 = super ez, 0 = Difficult. 
* Gunning Fog : Grade level measurement for shorter passages. 
* SMOG: Grade level measurement for longer passages
### 2. Tf-Idf
* Top 1000 features from transcripts
* Included 2 ngrams

### 3. Loughran McDonald Master Dictionary
* Loughran McDonald is a professor of accounting and finance at University of Notre Dame. Built a dictionary of words and scores based off of words in company earning statements 
* Sentimental Analysis: Positive, negative, superfluous, uncertainty, litigious

# Random Forest Classifier
   RF = RandomForestClassifier(bootstrap= True, 
						max_depth= 4, 
						min_samples_leaf= 35,
						min_samples_split= 8,
						n_estimators= 100)
* Ran many grid searches on many combination of features. Best performing were the reading scores by themselves. 
* Used Precision Score to find best model. tp/(tp/fp)
* Predicted positive = buy ticker for quarter. Sell after 90 days
* Predicted negative = hold onto VGT

### Measurements of effectiveness
* The model is measured using by assuming the investor either holds VGT. If model predicts that the stock will do better, the investor will sell VGT and buy the ticker in that quarter.

* Closed Formula:
If predicted buy:	∑Δ𝑇𝑖𝑐𝑘𝑒𝑟−Δ𝑉𝐺𝑇
Else: 			∑Δ𝑉𝐺𝑇

Result: 0.0097

Or

Ticker outperformed VGT by 1% over the quarter. 

# Takeaways
* The data was not perfect. The Transcripts have the operator, questioners, and chief officers responses recorded in each article. The model could be improved by filtering out non-business member speakers.
* Did not account for technical or traditional forms of investment strategies. Therefore did not include key financial statistics such as market cap, EBITDA, EPS, etc.
* Models explored were exclusively logistic regressions and Random Forest Classifiers. Other models to explore are Gradient Boosting Classifiers and Neural Networks.
* This project was solely based on Small - Mid sized market cap tech companies from the NASDAQ, 2017-2020. Therefore the model is biased. Small to medium sized tech firms are characterized by their volatility and extreme growth. Additionally 2017 to 2020 saw multitude of extreme stock movements. To continue the project, additional exploration of other sectors and time periods are necessary. 






