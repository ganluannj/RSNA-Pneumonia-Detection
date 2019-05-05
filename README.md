# RSNA-Pneumonia-Detection

## 0. Note
Both the notebook and this file contains some explanation/tutorial. Basically, the notebook contains some simple explanation of the code and some direct interpretations of the output. This tutorial includes the explanation of several concepts which may help to understand the notebook.  <br />

## 1. Data 

### 1.1 Data upload
Different from the two challenges we did for midterm project, the data size for this challenge is huge (greater than 3GB) and data are medical image. For midterm project I used local machine (my PC) to run the code, however it is not quite possible to use my PC to run code for this challenge. I used GPU provided by Google Colaboratory instead. Then the first thing we need to upload the data into google Colab. 
I have tried three different ways for this. The first is to downloaded the data from kaggle into my local computer and then upload the data to Colab. The upload speed is very slow. I tried several times, not successfully uploaded all the data for even once. The second is to store data into goole drive and mount google drive files directly from Colab. This is faster than uploading from local computer, but not fast enough. Also I noticed that it is not stable and sometimes the input/output error can pop out due to the number of files under one folder is too many. The last data uploading method is 

The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where *lr* is the initial learning rete, *x* is the index for current ecpoch, and *n* is the total number of epochs. 

.fit_generator : Should be used when either (1) the dataset is too large to fit into memory, (2) data augmentation needs to be applied, or (3) in any situation when it’s more convenient to yield training data in batches (i.e., using the flow_from_directory  function).
