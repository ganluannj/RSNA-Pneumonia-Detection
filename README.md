# RSNA-Pneumonia-Detection

## 0. Note
Both the notebook and this file contains some explanation/tutorial. Basically, the notebook contains some simple explanation of the code and some direct interpretations of the output. This tutorial includes the explanation of several concepts which may help to understand the notebook.  <br />

## 1. Data 

### 1.1 Data upload
Different from the two challenges we did for midterm project, this challenge 


The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where *lr* is the initial learning rete, *x* is the index for current ecpoch, and *n* is the total number of epochs. 

.fit_generator : Should be used when either (1) the dataset is too large to fit into memory, (2) data augmentation needs to be applied, or (3) in any situation when it’s more convenient to yield training data in batches (i.e., using the flow_from_directory  function).
