### Historically...
Problems that didn't fit into a vector format were often forced into a vector. Then processing was implemented on it.

# NLP (Before Neural Networks)
- A blanket term for all tasks involving human language.
## Syntax vs Semantics
- Syntax: Structure of a valid sentence.
- Semantics: What your sentence actually means.
## Problems in NLP
- Text to speech.
- Sentiment Analysis.
- Translation.
- Entity extraction.
- Personal information redaction.
## Basic NLP Principles Through Topic Modeling
- Vocab:
	- Word: A word.
	- Document: A collection of words.
	- Corpus: A collection of documents.
	- Topic Modeling: Given a series of labeled documents, figure out which topic a new document belongs to.
### Old Way of Modeling
- Reading in Documents:
	- Fucked up way of representing each word as a unique vector. Converting ordinal data into categorical data so it's one-hot encoded into a giant table. This is called "unigram" or the "bag of words" approach.
	- Instead of encoding every single individual word, we can encode every pair of words (bigrams).
		- Makes the original much much larger than before.
		- There are also trigrams, where every triplet is encoded.
	- Data Cleaning:
		- Stemming and Lemmatization: converting a verb to its infinitive form.
		- Typo correction: Replace a word with the word in our dictionary with the nearest *edit distance*.
		- Stop Words: Decide how to handle words like '[the](https://gist.github.com/sebleier/554280)'.
- Modeling:
	- Now the data has been pruned and cleaned, as well as converted to tabular, we can now do KNN.
	- To find similarity, we don't use Euclidean distance, but instead Cosine Similarity:
		- ![[Pasted image 20241112095735.png|200]]
		- Smaller theta means more related. Higher theta means less related.
- Problem:
	- We lose so much information.
## TF-IDF (Term Frequency - Inverse Document Frequency)
- Newer process is just adding weights onto words: Rarer words have higher weights, common words have lower weights.
- We create a new table that keeps track how many documents a term occurs in.
	- We create weights based off of this.
	- ![[Pasted image 20241112100314.png|400]]
	- This lets us "downweight" the power of words that are common across all sources.
- Now, instead of using the one-hot encoded table, we can replace the number of times a word appears in a document with their proportional weight in the document (probably $wordCount * frequency$).
## Word Embeddings
- Words are not ordinal, but they do have relationships with other words.
	- ex: "anger" is closer related to "rage" than "cow".
- Overview:
	- A way of representing words as vectors.
	- Represents a word in 'concept space'.
	- Groups similar words.
	- ![[Pasted image 20241112101226.png]]
### Continuous Bag of Words
- Predicting a target word given the context surrounding it.
### Skip-Gram Model
- Creating word embeddings that focuses on predicting surrounding words based on a target word.
## General Process
- Clean the data:
	- Stemming.
	- Stop-word removal.
	- Lemmatization, etc.
- Vectorize word data:
	- Bag of words.
	- Bigrams (unigrams, trigrams).
	- TF-IDF
	- Word embeddings (You can usually just import word embeddings from somewhere else. Creating them is super intensive).
- Supervised Learning
	- Usually something similar to KNN.

# NLP (LLMs)
## LLMs
- The culmination of decades of work in NLP.
### Next Token Prediction
- Just predicting the next word given the immediate context.
### Transformers
- A complicated recurrent neural network that forms the basis of large language models.
### Training (of ChatGPT)
- Training Stage 1:
	- Result is very bad. Since the internet is BAD, it's really racist and a whole lot of other bad things.
- 3 more training stages to refine.
- Result:
	- Isn't always correct but it's optimized to sound correct.
	- Will sometimes hallucinate.
	- But nonetheless, super powerful tool.
