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

* There are just two layers : `Input Embedding` and `Output Embedding`. Pass input word through the first embedding to get a vector `h` which is then taken as a dot product with vectors in the second embedding layer and softmax is applied to obtain probabilities
* Trained so as to maximise log probabilities such that words in similar contexts have vectors that are close i.e. dot product is large for such word vectors


### CBOW
* Use context from both before and after to predice current word
* Instead of feeding `n`previous words into the model, the model receives a window of `n` words around the target word `wt` at each time step `t`
* Obtain input embeddings of all the context words and sum them to get a vector `h` for further computation
* The objective is then to maximise `1/T \Sum_t (logP(wt|wt-n,...,wt-1,wt+1,...,wt+n))

### Skip Gram
* Uses center word to predict surrounding words
* Objective Function : $$J_\theta = \frac{1}{T}\sum\limits_{t=1}^T\ \sum\limits_{-n \leq j \leq n , \neq 0} \text{log} \space p(w_{t+j} | w_t)$$
* Computation of `P(w_t+j|wt) : $$p(w_{t+j} \: | \: w_t ) = \dfrac{\text{exp}({h^\top v'_{w_{t+j}}})}{\sum_{w_i \in V} \text{exp}({h^\top v'_{w_i}})}
$$
Here `h` is the word embedding of the current word `wt`
* To avoid confusion on the hidden layer or so, there is no intermediate hidden layer used in Skip Gram models, just two embedding/projection layers. Hence call `v` as the input embedding and `v'` as the output embedding denoting them in this manner and obtain :
$$p(w_{t+j} \: | \: w_t ) = \dfrac{\text{exp}({v^\top_{w_t} v'_{w_{t+j}}})}{\sum_{w_i \in V} \text{exp}({v^\top_{w_t} v'_{w_i}})}$$

## Dealing with Softmax
* For every training example softmax requires computation of sum over entire vocabulary which can be huge
1. Hierarchical Softmax
2. Differentiated Softmax
3. CNN Softmax

These keep softmax structure almost the same, just change the way of computaion

Sampling based aproaches
1. Importance Sampling
General Idea : Compute the gradient of the loss function and observe that it involves a term for current word and an expectation over all other words sampled from the networks distribution `P(w)` for the word

The idea is then to sample from a proposal distribution (unigram distribution of the words) and use a biased estimate of `P(w)` and obtain the gradient vector

2. Adaptive Importance Sampling
3. Target Sampling

These do away with softmax and approximate denominator by another easy to compute function. At test time however softmax still needs to be computed over all words in the vocabulary.

### Noise Contrastive Estimation (NCE)
Reference : https://arxiv.org/pdf/1410.8251.pdf

* Required : Predict the correct words among a given set of words in vocabulary
* Frame as Classification : Model tries to distinguish positive, genuine data (correct word) from noise samples (incorrect words) as binary classification
* For every (w,c) pair in the training set, `k` negative samples are obtained from a noise distribution `Q(w)` which can be taken as the unigram word probabilities in the corpus and as the given word is a positive sample, one has a `k+1` size (w,d) set for each (w,c) pair here
* The probabilities for the binary classification problem are defined as the following process :
Given a context `c` and denoting word `w` and label `d`, first sample a word for the context and based on the word sample the label : $P(d,w|c)=\frac{1}{k+1}P_model(w|c) + \frac{k}{k+1}Q(w)$
* Obtain $P(d={0,1}|w,c)$ based on this and maximize to increase the log likelihood of the (w,d) training data so obtained. Look at $L_{NCE_k}^{MC}$ in the reference above and maximise that
* Some notes : Partition factor set to 1 and practically network tends to learn self normalized values anyway, asymptotically Monte Carlo based objective gradient converges to true gradient

### Negative Sampling
* Similar to NCE just the probabilities $P(d|w,c)$ are defined using sigmoid (See reference)
* This is not asymptotically consistent to true gradient value however as a point to note

Number of samples in either case : Gensim uses 5