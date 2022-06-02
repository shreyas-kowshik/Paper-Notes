# Word Embeddings Summary

Link : https://ruder.io/word-embeddings-1/index.html, https://ruder.io/word-embeddings-softmax/index.html

* Language Model : Probaility of a sequence of words
* Bengio et.al (https://ruder.io/word-embeddings-1/index.html#fn1) introduced the first neural language model based on the following building blocks :
1. Embedding Layer
2. Intermediate layer(s)
3. Softmax Layer

Idea : Take a window around current word as context and try to predict the current word. Context words are first passed through embedding layers to get their embeddings which is then averaged to get a vector `h`. This is used to maximise probablity obtained from the dot product as : `P(wt|c) = exp(h'v_wt)/(Sum_V exp(h'v_w))` where `v_wt` is obtained from an output embedding matrix

Maximise : Next word probaility from previous words as input to network

Challenge : Softmax at output layer requires a sum over all words in vocabluary, Computaitionally Expensive! => Use various approaches to mitigate this

## Word2Vec

### CBOW
* Use context from both before and after to predice current word
* Instead of feeding `n`previous words into the model, the model receives a window of `n` words around the target word `wt` at each time step `t`
* The objective is then to maximise `1/T \Sum_t (logP(wt|wt-n,...,wt-1,wt+1,...,wt+n))

### Skip Gram
* Uses center word to predict surrounding words
* $$J_\theta = \frac{1}{T}\sum\limits_{t=1}^T\ \sum\limits_{-n \leq j \leq n , \neq 0} \text{log} \space p(w_{t+j} \: | \: w_t)$$