# word-processing-ml
machine learning with word processing.

currently reaches 60% accuracy with unseen data for project 2, 86% accuracy for project 1.


Project 2 Notes:

"""
A practical application in e-commerce applications is to infer sentiment (or polarity) from free-form review text submitted for a range of products, movies, places, etc.

For the purpose of this assignment, you have to implement a k-Nearest Neighbor Classifier to predict the sentiment for 18,000 reviews for restaurants provided in the test file (test.csv).

Positive sentiment is represented by a review rating of +1 and negative sentiment is represented by a review rating of -1.

In the test file, you are only provided the reviews but no ground truth rating which will be used for comparing your predictions.

Training data consists of 18000 reviews and exists in the file new_train.csv. Each row begins with the sentiment score followed by the text of the review.

format.csv shows an example file containing 18000 rows alternating with +1 and -1.

Your final submission should be similar to format.csv with same number of rows i.e., 18000 but the sentiment prediction should be generated by your developed model.

IDEA: Split the reviews into words, then count the number of times each word appears in each review.
Then, use the counts as features for the classifier.
Get rid of STOP WORDS (commonly appearing words that don't add much meaning to the sentence)

Sparse Matrices are used as most of the frequency would become 0. This does not work with KD tree or Ball Trees. 
-> Possible Solution: Locality sensitive hashing: Put similar input into same "buckets" with high probability. 
-> Solution: Use a hash function to map the input to a random vector, then take the dot product of the vector with the input.

[ Cos similarity should be used instead of Euclidean distance ]
       -----------------------------------------------------------------
When using program with large set of numbers, use numpy.array to store the data.

Note that the arrays begin as pandas, and iloc is used to restrict columns; but are converted to numpy arrays for faster processing.
[pandas consumes more memory, is better for 500k+ size datasets]

Not allowed to use KNN predict (own prediction algorithm used for KNN)

Other ideas to improve performance:
- Obtain bigrams (or ngrams) from the text -> Extract every two words for context, Ex. this is a sentence becomes ('this', 'is'), ('is', 'a'), ('a', 'sentence')
 --> Using (2,2) or (2,3) did not improve accuracy and turned it back into random chance... 
"""

""" In the programmer's own words:

I basically have the data that has (training data) 18000 rows of Classification, Review; ex:
	 1, "This restaurant was amazing, I would go there again".
	-1, "horrible service, waiter took too long, would not visit again."

The mission is to find the K nearest neighbor, predicting the classification of a new review.

I first replaced all punctuation and stop words to avoid redundancy or irrelevant features.

I am fitting it by putting it into array called train_data [2d array], then use train_data[:, 1] which is the review. I am comparing answers by splitting it into 80/20 train/test, and having test_answers = test_data[:, 0]

I'm using u,s,v = svds(test_data_features, k=40) due to 40 dimensions being a good compression.
where test_data is the fit transform of the (test_data[:,1])

so I use the tdidf vectorizer to put in the weighted occurence of each word
and then use cos similarity to return K closest neighbor and compare it to the answer

After normalizing the data you can simply take the Dot product, comparing the new row of the unseen review, to all the rows in the training data.


	Example: The above might become 1, ['amazing': 1, 'good': 2, '....' ]
		-1, ['horrible': 1, 'bad':2, ... ] 
"""