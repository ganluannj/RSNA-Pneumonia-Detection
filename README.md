# RSNA-Pneumonia-Detection

The learning rate is updated by cosine annealing: <br />
<a href="https://www.codecogs.com/eqnedit.php?latex=lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" target="_blank"><img src="https://latex.codecogs.com/svg.latex?lr*\frac{cos(\pi&space;x/n)&plus;1}{2}" title="lr*\frac{cos(\pi x/n)+1}{2}" /></a>,
<br />
where lr is the initial learning rete, x is the index for current ecpoch, and n is the total number of epochs. 
