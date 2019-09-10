Links : 

http://vision.stanford.edu/teaching/cs231b_spring1415/slides/lectureTracking.pdf
http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.112.8588&rep=rep1&type=pdf

In its simplest form, tracking can be defined as the problem of estimating the trajectory of an object in the image plane as it moves around a scene. In other words, a
tracker assigns consistent labels to the tracked objects in different frames of a video. Additionally, depending on the tracking domain, a tracker can also provide object-centric
information, such as orientation, area, or shape of an object.

Steps : 
1. Build a suitable representation of the object
2. Select image features as input to tracker

Object Representation : 

Can represent object as a point,curve,contour,etc. These are shape representations.

Appearence representations can be in the form of features of the object like contours and the region within,
multi-view appearence models, etc.

Feature Selection : 

Uniquely assigns a feature to the object to be tracked (we want it to be as unique as possible)

Depends a lot on the representation. Color is used as a feature for histogram-based appearance
representations, while for contour-based representation, object edges are usually used
as features.

Features can be color, edges, optical flow, texture (a measure of intensity variation of a surface quantifying smoothness
and regularity.

