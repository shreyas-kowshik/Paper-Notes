Pix2Pix
Generally, a simple L2 loss produces blurry reconstructions between images. Using a loss function to produce sharp images is an open problem. GANs overcome this problem by jointly learning the loss intermediary to create the new image and also creating the new image at the same time.
Conditional GAN - the output is conditioned on the input. Both the generator and the discriminator observe the input in one domain.
Patch GAN - to capture local style statistics.

Architecture : 


Objective : 

LcGAN (G, D) =Ex,y[log D(x, y)]+ Ex,z[log(1 − D(x, G(x, z))]
LL1(G) = Ex,y,z[ky − G(x, z)k1]

The final objective : 
G ∗ = arg min G max D LcGAN (G, D) + λLL1(G)

The L1 distance is found to produce less blurring compared to the L2 distance. However both tend to leave out a considerable amount of high frequency detail compared to the low level details.

Without z in the generator : the network would only learn to map deterministically between two domains and the mapping would be nothing more than a delta function (Note : I am not sure what exactly they mean by a delta function here. I think they mean the dirac delta function and that the outputs are always the same). Papers previously used Gaussian Noise as z. But it is not an effective strategy according to 1. To introduce noise, dropout is applied at several layers of the generator at both train and test time. This however, only produces a slight stochasticity over the output distribution and does not increase the entropy by a large margin. The problem of designing GANs producing a large entropy distribution over the outputs ( in other words, one image can map to multiple looking images with small differences) is not addressed in the paper and is left as an open research problem.

Patch GAN : 
L2 and L1 losses when used directly end to end would produce outputs that have good content of the low frequencies. However the high frequency details are lost a lot here and it leads to blurring. Thus innovation was brought in only for capturing the high frequency detail in the generator as the low frequency part would be captured by the L1 loss.
A patch GAN is basically a modification of the discriminator wherein it looks only on a NxN patch of image and predicts whether the patch is real or fake instead of looking at the entire image at once. This is run over all possible patches of the image and the outputs are averaged to produce the final discriminator score. This gives a good result with a fewer number of parameters. 
The logic : the Patch GAN basically models the image as a Markov Random Field, assuming independence between pixels separated by more than a patch diameter. Thus the Patch GAN is a form of style texture loss.

Training and Inference :
First take a step on D and then on G. A peculiar thing done was the usage of dropout and batch-norm with the statistics from the test data at the test time.

Some takeaways : 
Adding skip connections as in UNet results in a much higher quality output.
Using only L1 - blurry. Using only LCgan - artifacts. Using both reduces artifacts.
A great resource to understanding why L2 causes this can be found here.
Note : L2 loss dosen’t do well because the actual model over the distribution of a class is multi model while minimising a L2 loss is equivalent to minimising the log likelihood of a Gaussian and a Gaussian is a unimodal distribution.So it basically finds a distribution that is centered at the mean of all the peaks.

Removing the conditioning on the input resulted in mode collapse wherein model only outputted the same image for all inputs.
Analysing Patch Size N : for N = 1 (pixel GAN), it leads to colorfulness of the results compared to the L1 loss. N = 70 worked best. N = 286 (Image size) did not achieve any better probably because it has many more parameters and would be difficult to train.

