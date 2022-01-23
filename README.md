# Red-Barrel-Classifier-Detector

Name: Samuel Choi
Date: Feb 6, 2020
ECE 5242 Intelligent Autonomous Systems

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
