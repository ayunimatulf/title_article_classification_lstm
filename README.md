# title_article_classification_lstm
Model classification for article category with 4 categories funny, sport, entertainment and other (train data: 250K titles and test data: 60K).

Dataset include 4 categorical text, which the precentage of data is represent in the diagram below.
![Data precentage Each Category](https://raw.githubusercontent.com/ayunimatulf/title_article_classification_lstm/master/1.png)

In this discussion that will be split as 3 process, pre-processing data, prediction method, and implementation model.
1. Pre-processing data.
   1.	Import dataset .json and convert to data frame and drop column ‘id’ because it’s not important for making prediction model
   2.	Then clean the data from non-letters characters, setting lower case in each sentence, and removing stopwords and save into new clean data  in datafame
   3.	Replace each categorical to be numerical using dictonary : {'entertainment': 2, 'funny': 3, 'other': 1, 'sport': 4} 
   4. Create wordclouds from each categorical data to know what the frequent words appear in each category
2. Prediction method using LSTM model
   1.	Vectorizer article title text by turning each text title into sequenve of of integer
   2.	Set limit for number vocabulary that will use in model (I choose 5000 words)
   3.	Set maximum number of each word titile text (I choose 100)
   4.	Set word embedding maximum dimension (I choose 50)
   5.	Find unique token using Tokenizer from dataset (merge from data_train and data_test)
   6.	Convert text into sequence that contain index of word contains in each text and make the data as tensor with size respectively for data_train and data_test : (250116, 100) (62534, 100)
   7.	Convert categorical labels to vector that is : {'entertainment': [0,1,0,0], 'funny': [0,0,1,0], 'other': [1,0,0,0], 'sport': [0,0,0,1]}
   8.	Make deep learning architecture :
      + The first layer is embedded layer that has output 100 represent vector each text and 50 represent embedding dimension
      + SpatialDropout1D performs variational dropout in NLP models
      +	Next layer is LSTM with 100 memory units
      +	The output creates 4 output values for each class (probability)
      +	The loss function is categorical_crossentropy because it is classification problem with multi-class label
      <br />This is model.summary() from the deep learning architecture :
      ![Model Summary](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/2.png)
   9.	In the model has added callbacks using EarlyStopping for stopping epoh when min_delta of loss is 0.0001
   10. Result of model training
       ![Training Verbose](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/3.png)
   11. This is plot of Loss and Accuracy from train data and validate data where validate data set 20% from train data.<br />
       ![Training Loss](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/4.png)
       ![Training Accuracy](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/5.png)
   12. Model testing result :<br />
       ![Testing Report](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/6.png)
   14. Predict New Text Imput
       -	First teks input will cleaning using the function that has been created for cleaning data
       -	Convert text to sequence using word_index from the model
       -	Pad sequence so it has same length as the model input before
       -	Predict the padded array
       -	The output is probability of each class so find the greatest number that it’s mean probability the text is that category is biggest then others.
       ![New Text Predict](https://github.com/ayunimatulf/title_article_classification_lstm/blob/master/7.png)
