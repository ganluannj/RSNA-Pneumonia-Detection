# RSNA-Pneumonia-Detection

## 0 Note
Both the notebook and this file contains some explanation/tutorial. Basically, the notebook contains some simple explanation of the code and some direct interpretations of the output. This tutorial includes the explanation of several concepts which may help to understand the notebook.  <br />

## 1 Data 

### 1.1 Data Uploading
Different from the two challenges we did for midterm project, the data size for this challenge is huge (greater than 3GB) and data are medical image. For midterm project I used local machine (my PC) to run the code, however it is not quite possible to use my PC to run code for this challenge. I used GPU provided by Google Colaboratory instead. Then the first thing we need to upload the data into google Colab. 

I have tried three different ways for this. The first is to downloaded the data from kaggle into my local computer and then upload the data to Colab. The upload speed is very slow. I tried several times, not successfully uploaded all the data for even once. The second is to store data into goole drive and mount google drive files directly from Colab. This is faster than uploading from local computer, but not fast enough. It is slowed down by downloading data from google drive to Colab. Also I noticed that it is not stable and sometimes the input/output error can pop out due to the number of files under one folder is too many. The last data uploading method is to directly download data from kaggle into Colab, and this is also the method I finally selected. The downloading speed is fast, it is stable, and the running speed is also fast. 

### 1.2 Data Generator
Before discussing data generator I would like to talk about Data augmentation. The best way to make a machine learning model generalize better is to train it on more data. One way to get more data is to create fake data and add it to the training set. This method is data augmentation. It has been a very effective technique for object detection and segmentation. For image data augmentation, some common used techniques includes: translating the training images a few pixels in each direction, rotaing the image or scaling image. Also alerting the image by augmentation introudce some variation and can be treated as a regularization method. This will further help to reduce the generation error. 

To train our model, we can directly feed our training data into the training model all at once. However, this requires that our entire training set can fit into RAM and we do not need to do any data augmentation (What is 'data augmentation' is explained below). However, for this challenge here, the data set is too large and cannot be fitted into memory all at once. Also to get a better model performance, we want to apply data augmentation. Thus we need data generator. 

For Data generator, we start by initializing the number of epoches we are going to train our model and the batch size. Here is the epochs selected is 25 :+1: and batch size is 32. Both of these two hyperparameters are limitted by the fact that Colab can only run upto 12 hours and also the memory of Colab. We read each image data as a numpy array. For training and validataion data, a mask array with the same size of the image was also created to represent the bounding box. The values in mask arary of locations corresponding to bounding area are 1 while the rest are 0.  Both the images and masks are resized from (1024, 1024) to (256, 256) to reduce the running time. Also I did a horizontal flip for half of the images as the augmentation. 

## 2 Model
### 2.1 Convolutional Neutral Network
As we learned in class, convolutional neural network (CNN) is a specialized kind of neural network for processing data that a known grid-like topology, such as iamge data. Convolutional networks are simply neural networks that use convolution in place of general matrix mulitiplication in at least one of there layers. CNN has several advantages, such as sparse interactions, parameter sharing, and invariant to translations. The basic CNN architecture includes convluation layers, pooling layers, and one fully connected layer. We used CNN to build our model. 

### 2.2 Model Architecture
The total architecture is composed by blocks as shown below. The input layer is followed by a convolutional layer. After that there are :+1: repeats of the repeating blocks. The repeating block contains one block_1 and two block_2 (explained below). The last is the output layer.  
![CNN-2](https://user-images.githubusercontent.com/47232632/57198281-edd00800-6f3e-11e9-9263-702cc170187b.png)

The structure of block 1 is shown below (Please note that for a better visulation effect, the layer and size are not drawn based on real values, same for block 2). The input layer is followed by a batch normalization layer (Batch normalization is a technique for improving  the speed, performance, and stability of the neural network. It is achieved through a normalization step that fixes the means and variances of each layer's inputs. It can reduce internal covariate shift. Besides this, with batch normalization layer, the model can use higher learning rate without vanishing or exploding gradients. Furthermore, batch normalization regularizes the network such that it is easier to generalize, and it is thus unnecessary to use dropout to mitigate overfitting. The network also becomes more robust to different initialization schemes and learning rates. Please refer to [wiki](https://en.wikipedia.org/wiki/Batch_normalization) for more details). 

This Batch normalization layer is followed by a Leaky ReLu layer. Same as ReLu, Leaky ReLu is also an nonlinear layer. For Relu, f(x) = max(0, x), thus ReLu tranform all the negative values to 0. One downside with ReLu is the 'dying ReLu', which refer to the problem that one neuron is stuck in the negative part and output is always 0. Because the gradient for the negative part is 0, so once a neuron get negative, it is very hard for it to recover. Such neuron basically palys nothing in the network. Leaky Relu updated the ReLu by output values for negative x as alpha * x. Here alpha is a small constant. This Leaky ReLu solves the 'dying ReLu' problem. It is also shown that Leaky ReLu can speed up the training (Please refer to [guide](https://medium.com/tinymind/a-practical-guide-to-relu-b83ca804f1f7) for more information about Leaky ReLu). The Leaky ReLu layer is followed by a convolution layer and a Max pooling layer. 
![CNN-4](https://user-images.githubusercontent.com/47232632/57198648-21149600-6f43-11e9-9a34-e35a1d4fc92f.png)

The stucture of block 2 is shown below. 
![CNN-5](https://user-images.githubusercontent.com/47232632/57199150-cd597b00-6f49-11e9-994e-7af34fa35a14.png)




The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where *lr* is the initial learning rete, *x* is the index for current ecpoch, and *n* is the total number of epochs. 

.fit_generator : Should be used when either (1) the dataset is too large to fit into memory, (2) data augmentation needs to be applied, or (3) in any situation when it’s more convenient to yield training data in batches (i.e., using the flow_from_directory  function).
