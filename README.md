# **Traffic Sign Recognition** 


**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:

* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)
[image1]: visualize.jpeg "Visualization"
[image2]: preprocessed.jpeg "Grayscaling"
[image3]: ./test/random_noise.jpg "Random Noise"
[image4]: test-images.jpeg "Traffic Sign 1"



## Data Set Summary & Exploration

### Basic Summary of the dataset

I used the vanilla python and pandas library to calculate summary statistics of the traffic signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is 32 x 32 pixels with 3 channels
* The number of unique classes/labels in the data set is 43

### Exploratory visualization of the dataset

I then plotted 5 samples (images) for each class to visually inspect the data. A sample of 5 classes is shown below. 

This exercise showed that some classes of images were very poorly lit and were barely visible.

![alt text][image1]

## Design and Test a Model Architecture

### Preprocessing steps

I first ran the Lenet model without just normalizing the images to make sure each pixel had similar data distribution. The model did not perform well. 

As some of the images were really dark, I brightened them up [1]. This made the model better but still the performance was not up to par.

So, I converted the image to greyscale [2] as suggested and it made a lot of difference to the model accuracy. 

An example of some of the traffic sign images as they went through the transformation steps.

![alt text][image2]

### Model

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   | 
|	Normalized  Grayscale					| 32x32x3 Gray Image |
| Convolution  	| 1x1 stride, valid padding, outputs 28x28x50 |
| RELU				|
|	Dropout				|	0.95 keep		|
| Max pooling	  | 2x2 stride,  outputs 14x14x50|
| Convolution  	| 1x1 stride, valid padding, outputs 10x10x10|
| RELU					|						|
|	Dropout				|	0.85 keep		|
| Max pooling	  | 2x2 stride,  outputs 5x5x100|
| Flatten     	|	outputs 2500			|
|	Fully Connected| outputs 120     	|
|	RELU			    |						|
|	Fully Connected| outputs 84			|
|	RELU			    |						|
|	Fully Connected| outputs 43   	|
| Softmax	
 


### Training the model

To train the model, I used an Google Compute Engine VM with GPU. The model was LENET5 model with more filters per layer. The final model had a learning rate of 0.001, batch size of 128 and was trained for 20 epochs.

### Approach to getting the desired accuracy

My final model results were:

* training set accuracy of 99.6%
* validation set accuracy of 93.9 %
* test set accuracy of 93.8 %

I chose the LENET5 model to start with as I was the most familiar with it. I improved the entire model iteratively until I got the desired accuracy. The  steps I took are summarized below :

1. I started with the LENET model with normalized images. It had very good training accuracy but bad validation test accuracy. I realized **that the model was overfitting**
2. To fix the **overfitting** problem, I added regularization by adding dropout to all layers. After a few runs and parmeter tuning, it reduced **overfitting by a lot.** But the accuracy of the model stayed low.
3. To improve accuracy, I started by adding more filters to the model. After tuning the parameters, it made a lot of improvement on the validation accuracy but the model performed badly on the test set and unseen images.
4. I then tried a few techiques for preprocessing images. I changed normalization methods, converted the images to a different color space and tried increasing the layers in the model. **After trying a lot of combinations, I got the desired accuracy by converting the images to grayscale after increasing brightness and using 2 layers.** 

 

### Test a Model on New Images


Here are five German traffic signs that I found on the web:

![alt text][image4] 

The first image might be difficult to classify because it loses details when it is downsized to 32 x 32 pixels/

Here are the results of the prediction:

```
Image 0 : test/children-crossing.jpeg
Actual = 28 (Children crossing)
Predicted = 23 (Slippery road)

Image 1 : test/stop.jpeg
Actual = 14 (Stop)
Predicted = 14 (Stop)

Image 2 : test/30kph.jpeg
Actual = 1 (Speed limit (30km/h))
Predicted = 1 (Speed limit (30km/h))

Image 3 : test/keep-right.jpeg
Actual = 38 (Keep right)
Predicted = 38 (Keep right)

Image 4 : test/yield.jpeg
Actual = 13 (Yield)
Predicted = 13 (Yield)
```

The model was able to correctly guess 4 of the 6 traffic signs, which gives an accuracy of 80%. 

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

The top five soft max probabilities for all the 5 images are :

```
Image 0 : test/children-crossing.jpeg
Actual : Children crossing
Predicted  Slippery road
Top 5 softmax probablililies
   0.8611 23 Slippery road 
   0.0795 11 Right-of-way at the next intersection 
   0.0592 28 Children crossing 
   0.0001 20 Dangerous curve to the right 
   0.0000 38 Keep right 

Image 1 : test/stop.jpeg
Actual : Stop
Predicted  Stop
Top 5 softmax probablililies
   1.0000 14 Stop 
   0.0000  3 Speed limit (60km/h) 
   0.0000 34 Turn left ahead 
   0.0000  5 Speed limit (80km/h) 
   0.0000 13 Yield 

Image 2 : test/30kph.jpeg
Actual : Speed limit (30km/h)
Predicted  Speed limit (30km/h)
Top 5 softmax probablililies
   1.0000  1 Speed limit (30km/h) 
   0.0000  0 Speed limit (20km/h) 
   0.0000 11 Right-of-way at the next intersection 
   0.0000  2 Speed limit (50km/h) 
   0.0000  6 End of speed limit (80km/h) 

Image 3 : test/keep-right.jpeg
Actual : Keep right
Predicted  Keep right
Top 5 softmax probablililies
   1.0000 38 Keep right 
   0.0000  0 Speed limit (20km/h) 
   0.0000  1 Speed limit (30km/h) 
   0.0000  2 Speed limit (50km/h) 
   0.0000  3 Speed limit (60km/h) 

Image 4 : test/yield.jpeg
Actual : Yield
Predicted  Yield
Top 5 softmax probablililies
   1.0000 13 Yield 
   0.0000 15 No vehicles 
   0.0000  9 No passing 
   0.0000 35 Ahead only 
   0.0000 12 Priority road
```
## References
[1] https://stackoverflow.com/questions/32609098/how-to-fast-change-image-brightness-with-python-opencv

[2] https://medium.com/@REInvestor/converting-color-images-to-grayscale-ab0120ea2c1e


## How to run the notebook ?

1. Setup the environment 
    
   ``` 
   conda env create -f environment-gpu.yml
   conda activate carnd-term1
   ```

2. Download and uncompress the data
    
    ```
    wget https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/traffic-signs-data.zip
    unzip traffic-signs-data.zip
    ```
3. Run the notebook
    