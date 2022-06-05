# Transformer Paper

Link : https://arxiv.org/pdf/1706.03762.pdf

## Visualise in a Nuteshell

* Input : (N,T,D) : N : batch size, T : sequence length, D : dimension size in encoder,
(N,T',D) : in decoder
* Output : (N,T',D') : pass through layers in decoder and for hidden dimension size D' get this output
* The layers are effectively just linear layers along with self attention and cross attention layers for each timestep. Thus the timestep dimension is exactly the same even in the output and multiple outputs can be expected for the different timesteps. One can select these as per the requirement of the task at hand  

## 3. Model Architecture

* PS : Map input sequence (x1,...,xn) to output sequence (y1,...,ym).
* Figure 1 : Overall architecture. Think of inputs (batch size 1 for now) as NxD dimensional tensors and outputs as MxD' dimensional tensors where D and D' relate to the vocabulary size in case of translation tasks. Visualise the forward propagation with this.
* Encoder : N=6 identical layers, each layer has two sublayers. Sub layer has Residual Connection, Layer Normalization, Multi-Head Self Attenton, Feed Forward : FFN(x)=max(0,xW1+b1)W2+b2 is applied to each timestep vector separately and identically
* Decoder additionally has mutli-head attention with encoder output. Note here self attention computations are MASKED for decoder as each timestep is allowed to attend to only ones before it. This way no decoding timestep takes information from the future.
* Softmax applied at the end of the outputs after passing through a linear layer to predict the `next token probabilities`

### 3.2 Attention
* Query, Key, Values. Think of it as you query some data over a set of keys, get some information and use this to obtain a values from a list.
* Weights = f(Q,K), Output = f(Weights, Values)
* Take shapes of Q,K:=(T,d_k), V:=(T,d_v)
* Scaled Dot Product Attention : Attention(Q,K,V)=softmax(QK'/sqrt(d_k),dim=1)V
* Why scaling by sqrt(d_k) : The dot products in QK' become very large otherwise which can lead to extreme values in softmax computation region
* Why Dot Product Attention? : Computational efficient due to matrix multiplications involved
* Multi Head Attention : Assume Q,K,V:=(T,d_model) and there are hx3 weight matrices W_i^Q, W_i^K, W_i^V mapping W_i^Q.Q:=(T,d_k), W_i^K.K:=(T,d_k), W_i^V.V:=(T,d_v) for h heads. These computations are done in parallel and attention is applied to get h outputs and concatenated
* Applications of attention : 
1. Encoder Decoder Attention : Q: from previous decoder layer, K,V : from output of encoder
2. Encoder Self Attention : Q,K,V : from previous encoder layer
3. Decoder Self Attention : Q,K,V : from previous decoder layer, masking done with pre softmax values set -\inf for timesteps to right of given timestep in the row in the QK' matrix

### 3.5 Positional Encoding
* A sequence of vectors does not contain any sequence information per se like convolutions or recurrent units do. Hence positional encoding is added to input embedding to give this information to the model
* Positional Encodings based on sine and cosine functions used : Intuition : For each dimension, have a different value of frequency such that this dimension can be distinguished from others as the timesteps progress
* https://nlp.seas.harvard.edu/images/the-annotated-transformer_49_0.png : Visualisation
* Notice that these functions are first normalized in the range [-1,1] by construction and allow for large sequence lengths as the encoding values try to give unique numbers across both time and dimension values
* Note : It is practically very less likely that two values will repeat themselves exactly. For instance, even for i=1, for pos and pos+k to have the exact same values : k=n*\pi*(1/10000^(2*i/d_model)) which is clearly not an integer for any integer value of `i` in this formulation. Hence these encodings produce distinct values across timesteps (positions) which helps the model to distinguish between these different positions and the dimensions as well

### 4 Why Self Attention
1. Reduces computational complexity
2. Parallelize computations
3. Maximum path length : Consider any two timesteps in the input sequence. How much does the forward propagation need to travel before it can connect these two? : For RNNs it is O(sequence length), for Self Attention it is O(1) and so on. The smaller this is, the eaiser to learn long range dependencies

### 5 Training Details
* Learning rate linear warmup followed by decrease by inverse square root of step number value
* Residual dropout, positional encoding sum then dropout, label smoothing for regularization
