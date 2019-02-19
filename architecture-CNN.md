CNN-Architecture-Paper-Notes - Across a wide range of papers

ImageNet Paper : http://papers.nips.cc/paper/4824-imagenet-classification-with-deep-convolutional-neural-networks.pdf


RELU trains faster than tanh. 
3.3 - Local response normalization introduced for RELUs. 
3.4 - Strides (s) are such that the same pixel is pooled in more than one pooling operation. (s < kernel dimension)

Augmentation Introduced : 
Randomly crop patch in the image and add it to the training set. At test time, for all possible crops in the test image, run the CNN on the crop and average the results.
PCA based : For each pixel, calculate the 3x3 covariance matrix for the RGB intensities and compute it’s eigen values and eigen vectors. Then transform each pixel intensity as : 
	This captures the natural property of images that natural images have objects whose identity is invariant to changes in intensity and color of illumination.

For training, the learning rate was reduced by 10 whenever the validation accuracy stopped increasing.

Maxout Networks : https://arxiv.org/pdf/1302.4389v4.pdf
“ Dropout (Hinton et al., 2012) provides an inexpensive
and simple means of both training a large ensemble of
models that share parameters and approximately aver-
aging together these models’ predictions.” - Quote from the paper

Introduction - Model averaging with dropout is not that explicitly proved. Dropout is most effective to take large steps in parameter space. 
Advantages of maxout : Facilitates optimization and also leverages the fast model averaging introduced by dropout.


Unet: https://arxiv.org/pdf/1505.04597.pdf
Used for semantic segmentation - a class label is assigned to each pixel.
Previous approaches : Use a local image patch around each pixel as the input. This captures a pixel uniquely based on it’s surroundings and thus localises well. Drawbacks are present in terms of greater being slow as it needs to be trained for each patch. Moreover larger patches need greater max pooling layers leading to loss of context (less amount of context is visible from lower layers. Larger image patches with more max pooling layers reduce the localization accuracy. 

Contextual information in an image : the information for each pixel given encoding it’s surrounding pixels as well.

Greater number of feature channels in the upsampling layer here help to propagate context information as the network is used for improving the resolution. 
The network uses the valid padding operation. Thus for the border pixels where the kernel cannot go, there cannot be an output possible as the network requires the context for each patch to be completely within the input image. In such a case, we mirror the patch on the border to predict the pixels of the segmentation map.
