# Contrastive Predictive Coding

Link : https://arxiv.org/pdf/1807.03748.pdf

## Gist
* Learn representations by predicting future in latent space
* Key insight : Learn representations that are maximally useful for predicting the future using a contrastive loss

* "We hypothesize that these approaches are fruitful partly because the context from which we predict related values are often conditionally dependent on the same shared high-level latent information." : Consider an image patch I and context C (rest of image), then I,C are dependent when conditioned on a common latent variable Z which produces the image. In other words, once you fix a value of Z, say controlling color, then I and C will have similar color distributions and hence they are dependent on each other.