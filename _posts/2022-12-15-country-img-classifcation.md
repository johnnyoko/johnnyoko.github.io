---
title: Classifying Images Country of Origin using Machine Learning
date: 2022-12-15 22:53:24.000000000 -07:00
layout: post
post-image: /assets/images/campaign_finance.jpg
description: Analysis of Modeling Techniques for Country Image Classification
keywords: machine learning, data visualization, image classifcation, computer vision
tags:
- Machine Learning
- data visualization
- computer vision
- image classifcation
author:
  display_name: Johnny Okoniewski
  first_name: Johnny
  last_name: Okoniewski
permalink: "/2022/12/15/image_classifcation/"

---

This project came from the efforts of Albert Crooks, Santiago Rodriguez-Papa, Simon Situ, and myself. we attempted to create a machine learning model that can accurately determine the location of an image. This was inspired by the game GeoGuessr, in which players have to determine where they are on the planet solely through images of the surrounding environment. We developed two different multi-model pipelines: one using an autoencoder and SVM; and the other using a convolution neural network based on VGG-16. To simplify the learning model, we attempted to predict the country of origin rather than the precise coordinates where the images were taken. Previous works have used models trained extensively on a very large number of samples, but due to memory and space constraints, we opted for a pre-existing data set of 49,997 images. Since this data set was quite skewed (24.03% of images belonged to one class and 48 classes had less than 50 samples) we subsampled the data set to 11 countries that had over a thousand images. To further improve upon our VGG-16 model we used data augmentation to simulate a larger data set. We only used 11 classes of locations, so we cannot directly compare with the related works of DeepGeo and PlaNet.

The method used for image preprocessing involved converting the images to grayscale, cropping them to remove graphical elements from the Google Street View UI, and resizing them to a square aspect ratio. The images were then further resized to 128 by 128 pixels, flattened into a 16,384 length 1-D array, and normalized to improve memory usage and accelerate the training process. Overall, this method helped to improve the efficiency of the training process and allowed for more accurate results to be obtained from the machine learning model.

![flowchart](/assets/images/img_classifcation/fig1.png)
Figure 1: The original image of a road in Aland
 
![flowchart](/assets/images/img_classifcation/fig2.png)
Figure 2: Sized down and grey-scaled image of a road in Aland, used as input into our models


We started our experimentation by applying Principal Component Analysis (PCA) from the scikit-learn library to the data set (?). This algorithm is used to reduce the dimensionality of a data set while preserving its most salient features. Our initial experiments aimed to achieve a final dimensionality of d = 128, which is equivalent to a 128× reduction in feature size. However, the PCA algorithm was unable to converge to a solution as our large data set could not fit fully in memory. As such, we then opted to instead use Incremental Principal Component Analysis (IPCA). IPCA is a batched variation of PCA used to perform feature reduction on the images while avoiding the need to keep the whole data set in working memory.
The next step in our original pipeline was to sim- ply classify the data set using its principal compo- nents. However, we first wanted to fine-tune the optimal number of principal components to aim for [other word], as this would result in a more robust model. For this initial exploration, we settled on a Support Vector Machine (SVM) classifier. This classifier aims to find decision boundaries that maximize the gap between categories. The implementation used was Scikit-learn’s sklearn.svm.SVC with a constant regularization parameter C = 1.0 and a Radial Basis Function (RBF) kernel. We then shuffled the data set and split it into 80% testing and 20% training. Finally, we varied the target number of principal components d ∈ {64,128,256} and measured the training and testing accuracy while keeping the classifier constant.
The above resulted in a training accuracy of 27.62% and a testing accuracy of 25.31% for 64 components, a training accuracy of 24.26% train- ing accuracy and testing accuracy of 23.21% for 128 components, and a training accuracy of 24.13% training accuracy and a testing accuracy of 23.81% testing accuracy for 256 components. Despite d = 64 having higher training and testing accuracy, we opted to work with 128 dimensions instead. This is because the speed improvements were not consider- able compared to 64 dimensions, and we feared the lower feature size would impede later classification.
Once we had settled on the feature reduction part of our model, we moved onto classification. We began by harnessing the Perceptron Learning Algorithm (PLA, or most commonly, Perceptron). We used the Perceptron algorithm from sklearn which handles multi-class classification. We used the default hyperparameters for this model. This yielded a training accuracy of 3.46% and a testing accuracy of 3.38%.
Given our quite poor results, we determined the resulting data set after IPCA was not linearly separable. We thus moved onto using a Support Vector Machine together with the kernel trick in order to be able to classify nonlinear data. We performed a grid search with the Support Vector Machine on the hyperparameters of C and kernel function. The values of C used were: 0.01, 0.05, 0.1, 0.5, 1, 2, 5, 10; the kernels used were: RBF, sigmoid, quadratic polynomial, cubic polynomial, and quartic polynomial. The grid search found that the best testing accuracy was achieved with a C set to 2 and the kernel set to the RBF kernel.

| Syntax        Description |
| ----------- | ----------- |
| Header      | Title       |
| Paragraph   | Text        |


Figure 3: Training metrics for the SVM on the 128-component PCA transformed images.

Figure 4: Testing metrics for the SVM on the 128- component PCA transformed images.

---
