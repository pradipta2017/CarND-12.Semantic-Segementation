# Semantic Segmentation
### Introduction
The goal of this project is to label/identify the pixels of a road images using a Fully Convolutional Network (FCN).
### Architecture/Approach
* A pre-trained **VGG-16** network is used to convert to a Fully Convolutional Network (FCN). This project is based on [FCN-8 architecture](https://people.eecs.berkeley.edu/~jonlong/long_shelhamer_fcn.pdf) to build the FCN.
* Encoder: The final layer or **VGG-16** Fully Connected layer is converted to 1x1 Convolution Network and depth is set to number of classes(in this case the number of classes=2; 1: road pixels, 2:non-road pixels). 1x1 Convolution Network is used to maintain the spatial information of the images.
* Decoder: To build the decoder portion of FCN-8, we’ll upsample the input to the original image size by using *convolution transpose* layer.
* Performance of the network is imporved by using *skip connections*. The skip connections are used as 1x1 convolution on previous **VGG-16** layers(in our case, layer 3 and layer 4).
* Define the loss function of the network.
* Train the network

#### Implementation
##### Build Layers
1. Vgg layer7 1x1 convolution: Vgg layer7 as input, number of classes=2, padding as same, kernel size=1.
1. Vgg layer7 upsampling(transposed convolution): Vgg layer7 1x1 convolution as input, number of classes=2, padding as same, kernel size=4 with strides(2,2).
1. Vgg layer4 1x1 convolution: Vgg layer4 as input, number of classes=2, padding as same, kernel size=1.
1. Vgg layer4 skip connection: Add layer7 output from step-2 and layer4_conv1x1 from step-3 to the tensorflow.
1. Vgg layer4 upsampling(transposed convolution): Vgg layer4 as input from step-4, number of classes=2, padding as same, kernel size=4 with strides(2,2).
1. Vgg layer3 1x1 convolution: Vgg layer3 as input, number of classes=2, padding as same, kernel size=1.
1. Vgg layer3 skip connection: Add layer4 output from step-5 and layer3_conv1x1 from step-6 to the tensorflow.
1. Final output layer = Vgg layer3 upsampling(transposed convolution): Vgg layer3 as input from step-7, number of classes=2, padding as same, kernel size=16 with strides(8,8).

**Important** *In each of the above convolution and transposed convolution, kerner initializer and kerner regularizer is used for better network classification.*

##### Optimize the Network
1. Make sure the layer and the labels are of same size
1. Define the loss function as softmax Cross Entropy with logits
1. Define the optimizer as Adam Optimizer
 
##### Train the Network
1. Download the pre-trained **VGG-16** network.
1. Load the Fully Convolution layers with optimizer.
1. Run the network with below hyper-parameters:
	- Epochs=20
	- batch size=2
	- learning rate=0.00001




### Setup
##### Frameworks and Packages
Make sure you have the following is installed:
 - [Python 3](https://www.python.org/)
 - [TensorFlow](https://www.tensorflow.org/)
 - [NumPy](http://www.numpy.org/)
 - [SciPy](https://www.scipy.org/)
##### Dataset
Download the [Kitti Road dataset](http://www.cvlibs.net/datasets/kitti/eval_road.php) from [here](http://www.cvlibs.net/download.php?file=data_road.zip).  Extract the dataset in the `data` folder.  This will create the folder `data_road` with all the training a test images.

### Start
##### Implement
Implement the code in the `main.py` module indicated by the "TODO" comments.
The comments indicated with "OPTIONAL" tag are not required to complete.
##### Run
Run the following command to run the project:
```
python main.py
```
**Note** If running this in Jupyter Notebook system messages, such as those regarding test status, may appear in the terminal rather than the notebook.

### Submission
1. Ensure you've passed all the unit tests.
2. Ensure you pass all points on [the rubric](https://review.udacity.com/#!/rubrics/989/view).
3. Submit the following in a zip file.
 - `helper.py`
 - `main.py`
 - `project_tests.py`
 - Newest inference images from `runs` folder  (**all images from the most recent run**)
 
 ## How to write a README
A well written README file can enhance your project and portfolio.  Develop your abilities to create professional README files by completing [this free course](https://www.udacity.com/course/writing-readmes--ud777).
