# RSNA-Pneumonia-Detection

## 0. Note
Both the notebook and this file contains some explanation/tutorial. Basically, the notebook contains some simple explanation of the code and some direct interpretations of the output. This tutorial includes the explanation of several concepts which may help to understand the notebook.  <br />

## 1. Data 

### 1.1 Data uploading
Different from the two challenges we did for midterm project, the data size for this challenge is huge (greater than 3GB) and data are medical image. For midterm project I used local machine (my PC) to run the code, however it is not quite possible to use my PC to run code for this challenge. I used GPU provided by Google Colaboratory instead. Then the first thing we need to upload the data into google Colab. 

I have tried three different ways for this. The first is to downloaded the data from kaggle into my local computer and then upload the data to Colab. The upload speed is very slow. I tried several times, not successfully uploaded all the data for even once. The second is to store data into goole drive and mount google drive files directly from Colab. This is faster than uploading from local computer, but not fast enough. It is slowed down by downloading data from google drive to Colab. Also I noticed that it is not stable and sometimes the input/output error can pop out due to the number of files under one folder is too many. The last data uploading method is to directly download data from kaggle into Colab, and this is also the method I finally selected. The downloading speed is fast, it is stable, and the running speed is also fast. 

### 1.2 Data generator and preprocessing
To train our model, we can directly feed our training data into the training model all at once. However, this requires that our entire training set can fit into RAM and we do not need to do any data augmentation (What is 'data augmentation' is explained below). However, for this challenge here, the data set is too large and cannot be fitted into memory all at once. Also to get a better generalization result, we need data augmentation as a regularization method. Thus we need data generator. 

For Data generator, we start by initializing the number of epoches we are going to train our model and the batch size. The we need to define our augmentation method. 

The training dataset is too huge and the memory will not allow us to feed the model with all data all at once. We need to divide our data into bactches and feed each batch to our model. This is what data generator does. 

The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where *lr* is the initial learning rete, *x* is the index for current ecpoch, and *n* is the total number of epochs. 

.fit_generator : Should be used when either (1) the dataset is too large to fit into memory, (2) data augmentation needs to be applied, or (3) in any situation when it’s more convenient to yield training data in batches (i.e., using the flow_from_directory  function).
