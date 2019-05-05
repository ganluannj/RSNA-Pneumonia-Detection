# RSNA-Pneumonia-Detection

## 0. Note
Both the notebook and this file contains some explanation/tutorial. Basically, the notebook contains some simple explanation of the code and some direct interpretations of the output. This tutorial includes the explanation of several concepts which may help to understand the notebook.  <br />

## 1. Data 

### 1.1 Data uploading
Different from the two challenges we did for midterm project, the data size for this challenge is huge (greater than 3GB) and data are medical image. For midterm project I used local machine (my PC) to run the code, however it is not quite possible to use my PC to run code for this challenge. I used GPU provided by Google Colaboratory instead. Then the first thing we need to upload the data into google Colab. 

I have tried three different ways for this. The first is to downloaded the data from kaggle into my local computer and then upload the data to Colab. The upload speed is very slow. I tried several times, not successfully uploaded all the data for even once. The second is to store data into goole drive and mount google drive files directly from Colab. This is faster than uploading from local computer, but not fast enough. It is slowed down by downloading data from google drive to Colab. Also I noticed that it is not stable and sometimes the input/output error can pop out due to the number of files under one folder is too many. The last data uploading method is to directly download data from kaggle into Colab, and this is also the method I finally selected. The downloading speed is fast, it is stable, and the running speed is also fast. 

### 1.2 Data generator
Before discussing data generator I would like to talk about Data augmentation. The best way to make a machine learning model generalize better is to train it on more data. One way to get more data is to create fake data and add it to the training set. This method is data augmentation. It has been a very effective technique for object detection and segmentation. For image data augmentation, some common used techniques includes: translating the training images a few pixels in each direction, rotaing the image or scaling image. Also alerting the image by augmentation introudce some variation and can be treated as a regularization method. This will further help to reduce the generation error. 

To train our model, we can directly feed our training data into the training model all at once. However, this requires that our entire training set can fit into RAM and we do not need to do any data augmentation (What is 'data augmentation' is explained below). However, for this challenge here, the data set is too large and cannot be fitted into memory all at once. Also to get a better model performance, we want to apply data augmentation. Thus we need data generator. 

For Data generator, we start by initializing the number of epoches we are going to train our model and the batch size. Here is the epochs selected is 25 :+1: and batch size is 32.  The we need to define our augmentation method. We read the image data as a numpy array. Here I did a horizontal flip for half of the images as the augmentation. 


The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where *lr* is the initial learning rete, *x* is the index for current ecpoch, and *n* is the total number of epochs. 

.fit_generator : Should be used when either (1) the dataset is too large to fit into memory, (2) data augmentation needs to be applied, or (3) in any situation when it’s more convenient to yield training data in batches (i.e., using the flow_from_directory  function).
