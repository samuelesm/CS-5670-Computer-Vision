# Red-Barrel-Classifier-Detector
Name: Samuel Choi

Date: Feb 6, 2020


In this project, I trained a model to detect barrels in images and find the relative world coordinates of
the barrel from images. To do this, I implemented algorithms for learning the color model, segmenting the
target color, and finally localizing the target object from images. 
Given a set of training images, I hand-labeled training examples. From these training examples, I built
a color classifier and red barrel detector. The center of the detected barrel and the distance to the 
barrel are displayed on the new test images. 

For best results, upload files to Google Drive and run on Google colab. 

Instructions for running the code:
1) Insert test images to folder called /test
2) Run non_barrel_data.ipynb:
		This program gets the necessary information from the non-barrel class saved into a concise format
3) Run bayes.ipynb:
		This program gets the necessary information from the barrel class
		and calculates the Gaussian and Bayes equations
		and selects regions that meet the model derived from Bayes equations
		and outputs the masks to the folder /classified_imgs
4) Run identify.ipynb:
		This program detects regions and filters them to finalize determining what regions are barrels
		and outputs labeled images of them with their distances
		and outputs the centroids and the distances for the barrels of each image

# Intro
The purpose of this project is to create a model that detects red barrels in
real-world images and estimates their distances based on a training model. Using
Gaussian models classified with color segmentation, we are able to detect regions
of pixels that lie within the Gaussian probabilistic space of the color of the pixels
from the training data. Using Bayes rule, used the Gaussian probability of a pixel
belonging to each class with the Gaussian formula being

# Problem Statement
One can identify the regions of red barrels with training images and approximate
pixels in test images that lie within that same color range. Using Gaussian
mixture models, one can create several classes and create a Bayesian probabilistic
model to classify each pixel of the test images. However, the problem is that the
variation in lighting condition and presence of objects with similar colors may
interfere with that classification. These issues may be resolved by using a
different color space, changing the number of GMM or number of classes, filtering,
or a combination.

# Approach

## Number of GMM:

The number of GMMs used was one. There were two different classes for
the model: barrel and not barrel. Although more GMM classes could have
been created, just using the two classes was enough to distinguish the red
regions and highlight the red barrels in all images except for one, where the
lighting conditions were poor. As a result, a two class model was the one
implemented for this object detection algorithm. However, if the region
detection was poor with the one GMM, one could relabel the training images
with more than two classes to see if that helps. This project only calls for
the detection of red barrels, so the separation of classification for non-barrel
objects would have been redundant as this model only looks at color and
not the shape of the objects. An EM algorithm could have been used, but it
was not deemed necessary for this case.

## Color Space:

The color space used was RGB. Initially, my barrel detection classification
performed very poorly, even detecting green turf as red barrel pixels.
However, this was fixed once I relabeled my training data pixels more
carefully. Initially, I traced the barrels very roughly, leading to a very poor
classification model. However, after reselecting my training image pixels,
my classification algorithm performed very well. If it still performed poorly
after relabeling, perhaps it would have been a good idea to change the color
space so that red regions were more distinguished from the rest. If this were
the case, I would use Photoshop or some photo editing tool with a histogram
to see what sort of adjustments to the color space or rgb saturation levels
would help distinguish the red pixels. Since my classification model
distinguished the regions well, I stuck with the RGB color space.

## Filtering:

After detecting regions of pixels detected as red barrel pixels from the
classification images outputted as red in the output images from the
algorithm in bayes.ipynb using the regionprops tool from the skimage
library, several steps were used to filter images.
● First, there were many small regions of pixels that were classified as
an area of interest. To filter those out, I took the parameter of
minor_axis_length from each region and deleted the regions with a
minor_axis_length less than 5.
● Next, I determined the minimum value of the region attribute
bbox_area by taking the bbox_area of the smallest detected barrel as a
starting point and then reduced it until it started detecting the next
smallest red item that wasn’t a barrel. I set my threshold between
these values at 1250.
● Next, I used the area of each region and got the percent of pixels that
were classified as red barrel. This step was useful in getting rid of
arbitrary detected areas with sparse detection area coverage. The
threshold of this was set so that if under 50% of the area was filled,
the region would be eliminated using filled_area / bbox_area as the
calculation.
● Next, I used the ratio between the height and width of each region
detected. Taking into account regions that may be cut off, the range
was set to keep the regions with a ratio lying in between 1 and 3. The
calculation for this was ​major_axis_length/​minor_axis_length.
● Last, the ratio of the remaining regions to the largest region was used
as the last filtering mechanism. This was done as some areas cut off
from the barrels were still identifying as barrels. However, we want to
not eliminate all barrels smaller than the largest one, because there
could be multiple barrels. As a result, all boxes with bbox_area that
are at 30% the size of the largest barrel were kept.

## Distance:

In the file, get_distance.ipynb, the estimated height and width of the barrels
of each classified training image were determined with regionprops
attribute, bbox, and the width and height were multiplied with the given
distance from the camera in the first number from the file name. Then I
calculated the mean for both the height*distance and the width* distance
The calculated means were then divided by the respective estimated height
and width in the identify.ipynb file and divided by 2 to get their average.
This was then rounded and determined as the estimated distance.
Although the detected height and width may not be perfect, if one
dimension is skewed, the other is there to compensate for the discrepancy.
This model works, because the ratio between the pixels and the actual
image has a linear correlation due to the geometry of similar triangles. The
similar triangles enable the ratio to remain constant, enabling an average of
values from the training data to be used to estimate the distance of the test
images. The following was the line used to calculate the estimated distance.
np.round((450.3636363636364/w + 648.3863636363636/h)/2)

# Results

Below is a sample. Full results can be viewed in the pdf file in this repository

