                    
                                                Executive summary
                                                        of
                            Classifying Reddit Posts With Natural Language Processing and        
                                                Machine Learning


In this post I will walk you through a process using Natural Language Processing (NLP) and classification modeling to classify Reddit posts from Star War and Star Trek. Before I begin, let’s recall the data science process (outlined below)

•	Define the problem
•	Gather & Clean the data
•	Explore the data
•	Model the data
•	Evaluate the model
•	Answer the problem

***Define the Problem***

As you may have guessed I was tasked with using machine learning to do what you just tried to do above! In other words, creating a classification model that can distinguish which of two subreddits a post belongs to.
The assumption for this problem is that a disgruntled, Reddit back-end developer went into every post and replaced the subreddit field with “(·̿̿Ĺ̯̿̿·̿ ̿)”. As a result, none of the subreddit links will populate with posts until the subreddit fields of each post are re-assigned.

As you may have gathered, posts in Star War and Star Trek will definitely have a lot of crossover. Whether you’re boldly going where no man has gone before or you’re already in a galaxy far, far away, you’ve probably heard the age-old debate about which of science fiction’s two biggest franchises is better. Geeks, freaks, scientists and even casual fans have long been arguing in every corner of the galaxy about Star Wars and Star Trek. Which is better and why?

I like a challenge so I purposely picked two closely-related subreddits as I wanted to see how well I could leverage natural language processing and machine learning to accurately re-classify the posts to their respective subreddit.

***Gather & Clean the Data***

My data acquisition process involved using the requests library to loop through requests to pull data using Reddit’s API which was not pretty straightforward.I filtered those datas through coding so that i can get the valuable data. To get posts from Star Wars, all I had to do was add .json to the end of the url. Reddit only provides 25 posts per request and I wanted 1000 so I iterated through the process 10 times. 

My for loop outputted a list of nested json dictionaries of which I indexed to pull out my desired features, Post Text and Title, while simultaneously adding them to two Pandas DataFrames one for Star War-related posts and the other for Star Trek related posts.

After getting my posts in their respective DataFrames I checked for duplicate and null values, both of which occurred. For duplicate values I got rid of them by utilizing the drop_duplicates() function. Null values only occurred in my Post Text column, this happens when a Reddit user decides to use only the title field. I decided not to drop null values as I did not want to lose valuable information in the accompanying rows of my Title feat so I filled the nulls with unique and arbitrary text instead.
After cleaning and concatenating my data, my final DataFrame contained 1060 documents (rows) and 3 features (columns). The third feature was my target, which had a balance of classes of 48% for class 1 (Star Trek) and 51% for class 0 (Star Wars).

***Explore the data***

I created a word cloud because they’re fun and we’re working with text data!

This word cloud contained 100 words from both subreddits. I generated it to get a visual understanding of the frequencies (bigger/bolder words have higher frequencies) of words and how their commonality across subreddits might throw my model off; or how it may work in my model’s favor if it’s a word/phrase like “wars” which has a medium frequency and likely only appears or mostly appears in Star wars posts

***Model the data***

I began my modeling process by creating my X and my y and splitting my data into training and test sets. I then moved on to my feature engineering process by instantiating two CountVectorizers for my Post Text features. CountVectorizer converts a collection of text documents (rows of text data) to a matrix of token counts. The hyperparameters (arguments) I passed through them were:

•	stop_words=‘english’ (Post Text)
•	ngram_range=(1, 2), 
•	min_df=.03 (Post Text)
•	max_df=.95
•	max_features=5_000

Stop words removes words that commonly appear in the English language. Min_df ignores terms that have a document frequency strictly lower than the given threshold.
An n-gram is just a string of n words in a row. For example, if you have a text document containing the words “I love my cat.” — setting the n-gram range to (1, 2) would produce: “I love | love my | my cat”. Having n-gram ranges can be helpful in providing the models with more context around the text I’m feeding it.
I assumed that setting of my Post Text feature, I gave it a higher n-gram range since post texts tends to be lengthier.
This resulted in 350 features which I fed into two variations of the models listed below. I built four functions to run each pair of models and Gridsearched over several hyperparameters to find the best ones to fit my final model with.

•	Logistic Regression
•	Multinomial Naive Bayes

The difference in variations was the fit_prior argument which decides whether to learn class prior probabilities or not. If false, a uniform prior will be used. One was set to True, while the other was set to False

***Evaluate the model***

My second Multinomial Naive Bayes model performed the best. With the best parameters being — alpha=0 and fit_prior=False. The accuracy score was 99.0% on training data and 98.0% on unseen data. This means our model is slightly and probably inconsequentially overfit. This also means that 99.0% of our posts will be accurately classified by our model.

***Answer the problem***

Considering the small amount of data gathered and minimal amount of features used, the Multinomial Naive Bayes model was the most outstanding. It handled unseen data well and balanced the tradeoff between bias and variance the best among the eight models so I would use it to re-classify reddit posts.
However if given more time and data to answer the problem I would recommend two things: 1) spending more time with current features (e.g. engineering a word length feature) and 2) exploring new features (e.g. upvotes or post title).

***Submission***
Check out my code (which I split into 3 Jupyter notebooks — cleaning/exploration, modeling,EDA and recommendations — for readability and organization purposes) and my presentation. 

[Data_Cleaning](http://localhost:8888/notebooks/Part_1_Data_Cleaning.ipynb)
[Feature_Engineering](http://localhost:8888/notebooks/Part_2_EDA_Feature_Engineering.ipynb)
[EDA_Recommendation](http://localhost:8888/notebooks/API_project_3_EDA_Recommendation.ipynb)
[Presentation](http://localhost:8888/edit/API_presentation.pptx)

