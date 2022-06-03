# Contrastive Predictive Coding

Link : https://arxiv.org/pdf/1807.03748.pdf
https://ankeshanand.com/blog/2020/01/26/contrative-self-supervised-learning.html

## Gist
* Learn representations by predicting future in latent space
* Key insight : Learn representations that are maximally useful for predicting the future using a contrastive loss

* "We hypothesize that these approaches are fruitful partly because the context from which we predict related values are often conditionally dependent on the same shared high-level latent information." : Consider an image patch I and context C (rest of image), then I,C are dependent when conditioned on a common latent variable Z which produces the image. In other words, once you fix a value of Z, say controlling color, then I and C will have similar color distributions and hence they are dependent on each other.

* Basic Idea : given a context of data upto current timestep (xt-n,...,xt) aggregate them to get a context vector `ct`. Use this to predict a future timestep `xt+k` as `p(xt+k|ct)`
* The objective is to maximise the mutual information between the variables `ct` and `xt+k`. The idea is to obtain latent features that are `slow moving` over time. For instance the speaker id or the speaker voice quality and so on
* Instead of directly optimizing the mutual information objective, a lower bound is optimized
* NCE is used to optimize the lower bound loss function as well

* Basic Idea in contrastive learning : Use the current timestep to predict the future (or any other is also fine)
* Call current timestep as anchor point `x`
* Obtain a positive sample for anchor `x+`
* Sample negative points from a noise/proposal distribution `{x-}`
* For speech, aggregate points from past to present and get context vector `ct` as anchor. Now try to predict say `xt+k`. To do so, get latent `zt+k` through encoder. Use this as positive sample. Sample more points other than `xt+k` to get N negative samples and pass them through encoder to get `{z-}`
* Compute affinity scores with anchor which measure how close each point is to anchor in vector space : `f(x,x+)`, `{f(x,x-)}` and do a softmax with these scores (https://ankeshanand.com/blog/assets/img/cpc.svg) and try to maximise probability of positive sample and minimise probability of negative sample. This in turn brings `x+` closer and moves `{x-}` farther to anchor in vector space. Objective : $\Sigma_{(x,c)} \frac{f(x,x+)}{\Sigma_{t\in[\{x+\} \cup \{x-\}] f(x,t)}}$ summing over all `(xt,ct)` pairs
* Word2Vec also does a similar contrastive learning where instead of speech signal frames it samples words with proposal distribution as the unigram word distributions in the corpur `P(w)` values