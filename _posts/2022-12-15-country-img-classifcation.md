---
title: Classifying Images Country of Origin using Machine Learning
date: 2022-12-15 22:53:24.000000000 -07:00
layout: post
post-image: /assets/images/img_classification/classification.jpg
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

This project came from the efforts of Albert Crooks, Santiago Rodriguez-Papa, Simon Situ, and myself. We attempted to create a machine learning model that can accurately determine the location of an image. This was inspired by the game GeoGuessr, in which players have to determine where they are on the planet solely through images of the surrounding environment. We developed two different multi-model pipelines: one using an autoencoder and SVM; and the other using a convolution neural network based on VGG-16. To simplify the learning model, we attempted to predict the country of origin rather than the precise coordinates where the images were taken. Previous works have used models trained extensively on a very large number of samples, but due to memory and space constraints, we opted for a pre-existing data set of 49,997 images. Since this data set was quite skewed (24.03% of images belonged to one class and 48 classes had less than 50 samples) we subsampled the data set to 11 countries that had over a thousand images. To further improve upon our VGG-16 model we used data augmentation to simulate a larger data set. We only used 11 classes of locations, so we cannot directly compare with the related works of DeepGeo and PlaNet.

The method used for image preprocessing involved converting the images to grayscale, cropping them to remove graphical elements from the Google Street View UI, and resizing them to a square aspect ratio. The images were then further resized to 128 by 128 pixels, flattened into a 16,384 length 1-D array, and normalized to improve memory usage and accelerate the training process. Overall, this method helped to improve the efficiency of the training process and allowed for more accurate results to be obtained from the machine learning model.

![fig1](/assets/images/img_classification/fig1.png)

Figure 1: The original image of a road in Aland.
 
![fig2](/assets/images/img_classification/fig2.png)

Figure 2: Sized down and grey-scaled image of a road in Aland, used as input into our models.

We started our experimentation by applying Principal Component Analysis (PCA) from the scikit-learn library to the data set. This algorithm is used to reduce the dimensionality of a data set while preserving its most salient features. Our initial experiments aimed to achieve a final dimensionality of d = 128, which is equivalent to a 128× reduction in feature size. However, the PCA algorithm was unable to converge to a solution as our large data set could not fit fully in memory. As such, we then opted to instead use Incremental Principal Component Analysis (IPCA). IPCA is a batched variation of PCA used to perform feature reduction on the images while avoiding the need to keep the whole data set in working memory.
The next step in our original pipeline was to simply classify the data set using its principal components. However, we first wanted to fine-tune the optimal number of principal components, as this would result in a more robust model. For this initial exploration, we settled on a Support Vector Machine (SVM) classifier. This classifier aims to find decision boundaries that maximize the gap between categories. The implementation used was Scikit-learn’s sklearn.svm.SVC with a constant regularization parameter C = 1.0 and a Radial Basis Function (RBF) kernel. We then shuffled the data set and split it into 80% testing and 20% training. 

Finally, we varied the target number of principal components d ∈ {64,128,256} and measured the training and testing accuracy while keeping the classifier constant. The above resulted in a training accuracy of 27.62% and a testing accuracy of 25.31% for 64 components, a training accuracy of 24.26% training accuracy and testing accuracy of 23.21% for 128 components, and a training accuracy of 24.13% training accuracy and a testing accuracy of 23.81% testing accuracy for 256 components. Despite d = 64 having higher training and testing accuracy, we opted to work with 128 dimensions instead. This is because the speed improvements were not considerable compared to 64 dimensions, and we feared the lower feature size would impede later classification.

Once we had settled on the feature reduction part of our model, we moved onto classification. We began by harnessing the Perceptron Learning Algorithm (PLA, or most commonly, Perceptron). We used the Perceptron algorithm from sklearn which handles multi-class classification. We used the default hyperparameters for this model. This yielded a training accuracy of 3.46% and a testing accuracy of 3.38%.

Given our quite poor results, we determined the resulting data set after IPCA was not linearly separable. We thus moved onto using a Support Vector Machine together with the kernel trick in order to be able to classify nonlinear data. We performed a grid search with the Support Vector Machine on the hyperparameters of C and kernel function. The values of C used were: 0.01, 0.05, 0.1, 0.5, 1, 2, 5, 10; the kernels used were: RBF, sigmoid, quadratic polynomial, cubic polynomial, and quartic polynomial. The grid search found that the best testing accuracy was achieved with a C set to 2 and the kernel set to the RBF kernel.

| 128x128 128-PCA + SVM Training Metrics |         |
|----------------------------------------|---------|
| Accuracy                               | 0.40206 |
| Macro-Precision                        | 0.75366 |
| Macro-Recall                           | 0.12557 |
| Macro-F1                               | 0.17639 |

Figure 3: Training metrics for the SVM on the 128-component PCA transformed images.

| 128x128 128-PCA + SVM Testing Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.26060 |
| Macro-Precision                        | 0.03894 |
| Macro-Recall                           | 0.02047 |
| Macro-F1                               | 0.01982 |

Figure 4: Testing metrics for the SVM on the 128- component PCA transformed images.

We wanted to improve the performance of our model by improving the quality of the features fed into our classification algorithm. To achieve this, we replaced the PCA with a Stacked Convolutional Autoencoder, which is better at dimensionality reduction and provides more accurate results. The autoencoder was modeled with TensorFlow/Keras and consisted of 11 layers that turned a 128×128 grayscale image into a 128-long vector. It had five maximum pooling layers and five convolutional layers. We used He Normal kernel initialization, L2 kernel regularization, and Dropout() layer to reduce overfitting, but none of these mechanisms worked effectively. The autoencoder achieved a training accuracy of 76.3% and a testing accuracy of 23.2% over 20 epochs, indicating overfitting.

![fig5](/assets/images/img_classification/fig5.png)

Figure 5: Sample count per class in the original data set, sorted by sample count; y-axis is logarithmic.

We found that training a Perceptron model on our highly-skewed data set was not possible due to overfitting caused by the disproportionate distribution of samples belonging to the United States class and the low percentage of samples belonging to other classes. To create a more robust data set, we decided to subsample the original data set and generate two new data sets: Data set A, which includes all classes with over 1000 samples, and Data set B, which includes all classes with over 1000 samples but only 1000 samples per class. We made this decision based on the distribution of the original data set, as shown in Figure 5.

We couldn't train a Perceptron model with our imbalanced dataset, so we decided to make a new one with more balanced classes by subsampling. Since training an autoencoder from scratch with the limited data was hard, we found an already-existing model with good performance and used it to encode our images. We tested the encoding on a few images and it worked well, so we used it to encode the whole dataset at different resolutions and colors. We then tried to classify the compressed images.

![fig6](/assets/images/img_classification/fig6.png)

Figure 6: Original image fed into the Stable Diffusion VAE: a road in Argentina.

![fig7](/assets/images/img_classification/fig7.png)

Figure 7: Reconstructed image from the VAE latent space.

We used the VAE to compress the images in the data set and trained new models using the compressed representations. For 64x64 images, we flattened the encoded representations into 256 feature sets. We tested the performance of Perceptron, SVM, and a fully connected feed forward neural network, and balanced the classes by weighting them inversely proportional to their representation in the data set. We used a bagging classifier to perform a grid search for the C hyper-parameter for SVM and found that a value of 1.5 performed the best. The RBF kernel was used with gamma set to "scale". The PCA + SVM model trained on 128x128 encoded images gave the best macro-F1 score (Figure 8).

![fig8](/assets/images/img_classification/fig8.png)

Figure 8: The confusion matrix on the testing set from the SVM trained on the 128x128 encoded images. The predicted labels are ordered from least represented to most represented in the data set.

| 64x64 SVM Training Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.79948 |
| Macro-Precision                        | 0.77927 |
| Macro-Recall                           | 0.91751 |
| Macro-F1                               | 0.83230 |

Figure 9: Training metrics for the SVM on the 64x64 encoded images.

| 64x64 SVM Testing Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.37826 |
| Macro-Precision                        | 0.31292 |
| Macro-Recall                           | 0.32608 |
| Macro-F1                               | 0.31403 |
 
Figure 10: Testing metrics for the SVM on the 64x64 encoded images.

| 64x64 PCA + SVM Accuracy  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.78868 |
| Macro-Precision                        | 0.76755 |
| Macro-Recall                           | 0.91002 |
| Macro-F1                               | 0.82145 |
 
Figure 11: Training metrics for the PCA + SVM pipeline on the 64x64 encoded images.

| 64x64 PCA + SVM Testing Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.37147 |
| Macro-Precision                        | 0.30861 |
| Macro-Recall                           | 0.32489 |
| Macro-F1                               | 0.31061 |

Figure 12: Testing metrics for the PCA + SVM pipeline on the 64x64 encoded images.


| 128x128 SVM Training Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.85958 |
| Macro-Precision                        | 0.84461 |
| Macro-Recall                           | 0.94816 |
| Macro-F1                               | 0.88819 |

Figure 13: Training metrics for the SVM on the 128x128 encoded images.

| 128x128 SVM Testing Metrics  |         |
|----------------------------------------|---------|
| Accuracy                               | 0.41532 |
| Macro-Precision                        | 0.33231 |
| Macro-Recall                           | 0.33949 |
| Macro-F1                               | 0.33214 |

Figure 14: Testing metrics for the SVM on the 128x128 encoded images.

We wanted to improve the accuracy of our model by exploring the local nature of the images in the data set. We realized that our data set was not large enough to train a convolutional model from scratch without overfitting, so we decided to use a pre-existing model (VGG-16) with transfer learning. We used the VGG-16 model without its top layers, added some new layers, and trained the model on our data set (Data Set B). To combat overfitting, we augmented the data set using Keras’ ImageDataGenerator. We froze the pre-trained VGG-16 section and trained the model for 20 epochs, achieving a training accuracy of 35.78% and a testing accuracy of 36.12%. Then, we unfroze the last two convolutional blocks of the VGG-16 model and continued training until we achieved a training accuracy of 76.21% and a testing accuracy of 61.21%.

![fig15](/assets/images/img_classification/fig15.png)

Figure 15: Graph of training (blue) and testing (orange) accuracy with respect to epochs during training of the top part of the custom VGG-16 model.

![fig16](/assets/images/img_classification/fig16.png)

Figure 16: Graph of training and testing accuracy with respect to epochs during fine-tuning of the custom VGG-16 model.

The SVM trained on PCA-transformed data performed worse than the SVM trained on data encoded by the VAE encoder. The VAE encoder performed feature extraction on the data, resulting in higher quality features than the feature reduction by PCA. The SVM models trained on encoded 128x128 images only slightly outperformed those trained on encoded 64x64 images. The VGG-16 model had the highest training and testing accuracy of any other model attempted, with a training accuracy of 76.21% and a testing accuracy of 61.21%. The model exhibited bias towards countries with a larger proportion of the data set, possibly due to inter-class similarity. The training precision, training recall, test precision, and test recall were also calculated, along with the training and testing macro F1-score and normalized confusion matrices. The model made mistakes similar to those a human player would make, such as confusing Canada for the United States and European countries with each other.

Our work led us to develop two models, the VAE Encoder + SVM and the Fine-tuned VGG-16, that achieve our goal of classifying Google Street View images by country. The need to reduce the number of classes and samples was due to the low quality of our original data set. However, we believe the Fine-tuned VGG-16 model can easily handle more classes with a more balanced data set. Although the model made human-like mistakes, we can infer that it may be challenging to improve beyond human-level play. Additionally, our models cannot be used unethically to locate a person beyond their general region or country. Lastly, we found that models preserving the spatial locality context, like our VAE encoder model and Fine-tuned VGG-16, outperform those disregarding this information.

---
