# Bert

Link : https://arxiv.org/pdf/1810.04805.pdf

* Bidirectional Encoder Representations from Transformers : Take a transformer with only an encoder and use self attention across all timesteps and build a language model. No decoder used
* Standard language models before this were unidirectional when used with transformers. For instance, GPT uses only words to left of currnet word as context in self attention layers. This restricts the representations that can be learned and is sub optimal for sentence level tasks and even token level tasks such as question answering where context after the current word is also equally important
* How to make bidirectional with transformers? : Masked Language Modelling [MLM]
* MLM : Mask a random token and ask model to predict what that token was. This allows fusing both left and right context in the representations
* Input : (N,T,V) where |V| size of vocabulary, Output : (N,T,D) where |D| is hidden dimension size (768 for bert-base, 1024 for bert-large)
* Bert allows for both single sentence and a pair of sentences as input to model
* [CLS] token : first token of every sequence. The final hidden state corresponding to this token is used as the aggregate sequence representation for classification tasks
* [SEP] : To separate between two sentences
* Figure 1 : Visualiton of computation
* Embeddings :
1. Token Embeddings : WordPiece embeddings already pretrained
2. Segment Embeddings : Learned embeddings added to each token to specify if it belongs to sentence A or B
3. Position Embeddings : For denoting position of each token
These three are summed up to get input embedding for token
* Masked LM Training :
If you condition on both left and right context and try to predict any word, it is trivial since the model can see the word itself in the input. Hence previous language models could only condition on left to right or right to left contexts

Proposed solution : Simply mask some tokens at random using [MASK] token and ask the model to predict them. In this case, the final hidden vectors corresponding to the mask tokens are fed into an output softmax over the vocabulary, as in a standard LM. In all of our experiments, we mask 15% of all WordPiece tokens in each sequence at random.

"Although this allows us to obtain a bidirectional pre-trained model, a downside is that we
are creating a mismatch between pre-training and
fine-tuning, since the [MASK] token does not appear during fine-tuning. To mitigate this, we do
not always replace “masked” words with the actual [MASK] token. The training data generator
chooses 15% of the token positions at random for
prediction. If the i-th token is chosen, we replace
the i-th token with (1) the [MASK] token 80% of
the time (2) a random token 10% of the time (3)
the unchanged i-th token 10% of the time. Then,
Ti (final representaion for the token) will be used to predict the original token with
cross entropy loss."

* Next Sentence Prediction Training :

"Many important downstream tasks such as Question Answering (QA) and Natural Language Inference (NLI) are based on understanding the relationship between two sentences, which is not directly captured by language modeling.

Specifically,
when choosing the sentences A and B for each pretraining example, 50% of the time B is the actual
next sentence that follows A (labeled as IsNext),
and 50% of the time it is a random sentence from
the corpus (labeled as NotNext). As we show
in Figure 1, C (final representation for [CLS] token) is used for next sentence prediction (NSP)."

Note : "The vector C is not a meaningful sentence representation
without fine-tuning, since it was trained with NSP."

* Fine Tuning Bert :

"For each task, we simply plug in the taskspecific inputs and outputs into BERT and finetune all the parameters end-to-end. At the input, sentence A and sentence B from pre-training
are analogous to (1) sentence pairs in paraphrasing, (2) hypothesis-premise pairs in entailment, (3)
question-passage pairs in question answering, and (4) a degenerate text-∅ pair in text classification
or sequence tagging. At the output, the token representations are fed into an output layer for tokenlevel tasks, such as sequence tagging or question
answering, and the [CLS] representation is fed
into an output layer for classification, such as entailment or sentiment analysis."

* Given in Figure 4 : For task specific usage of representations