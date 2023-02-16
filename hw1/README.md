### CSE 582 Homework 1

In this homework, 
* we analyze the source code for [word2vec](https://code.google.com/archive/p/word2vec/) to understand the CBOW algorithm
* Train the model on [1 Bilion Word Language Model Benchmark](https://www.statmt.org/lm-benchmark/)
* Compute similarity scores for common query words like 'cat', 'she', 'like'


#### NOTE 

* To train the model, run [./word2vec/trunk/demo-word-big-data.sh](./word2vec/trunk/demo-word-big-data.sh)
* [similarity_scores.txt](./similarity_scores.txt) file has the similarity scores for common query words.
* [analogy.txt](./analogy.txt) file has the analogy scores.



#### Points to consider 

* How does CBOW compose context embeddings?  
* How does it compute word probability given context?    
    - For each context word, we mutliply the one-hot encoding of the word with the hidden layer weight matrix, and mutliply their result with the final weight matrix. We apply softmax on the above product and compute the probability of the word given the context word. 
* How does it implement negative sampling?
    - The probability of selecting a negative sample word is given by the   
    $P(w_i) = \frac{f(w_i)^{\frac{3}{4}}}{\sum_{i=1}^{|V|} f(w_i) ^{\frac{3}{4}}}$, where $f(w_i)$ is the frequency of word $w_i$.  
    -  The code base implements this using `UnigramTable`, which is large 1D array of size 1e8. The entries in this table are word's index, and the number of times a word's index appears in the table is determined by $P(w_i) * table\_size$.
    - To select a negative sample, we just choose a random index between 0 and $table\_size$. Based on the properties of the table, we are more likely to choose words with high probability. 
* Any other parameters apart from the word embeddings and context
embeddings?
    - vector dimensionality
    - size of the context window
    - size of the `UnigramTable` 
    - learning rate
* What input format does Word2Vec require?
    - Word2Vec expects a text file as an input. After the training, each word in the text file has a vector associated with it. 
    - It is better to give input as lowercase with all stop words, special characters removed. The current codebase expects words to be seperated by space, tab-space or new-line.
