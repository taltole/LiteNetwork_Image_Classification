# CIFAR10 – SqueezeNet

In this work, I experiment with SqueezeNet neural network based on the following paper: https://arxiv.org/abs/1602.07360. <br/>
To solve a classification task using CIFAR 10 dataset, which can be found here:  https://www.cs.toronto.edu/~kriz/cifar.html.
<br/>
### CIFAR10:
-	60000 32x32 color images in 10 classes, with 6000 images per class. 
-	50000 training images and 10000 test images.
-	CLASSES = 'plane', 'car', 'bird', 'cat', 'deer', 'dog', 'frog', 'horse', 'ship', 'truck'
 <p align="center">
  <img src=https://github.com/taltole/CIFAR10_SqueezeNet/blob/master/saves/cifar.png? width="350" alt="accessibility text">
</p>
 
The dataset is divided into five training batches and one test batch, each with 10000 images.
The test batch contains exactly 1000 randomly-selected images from each class. 
The training batches contain the remaining images in random order, but some training batches may contain more images from one class than another. Between them, the training batches contain exactly 5000 images from each class. 

•	I placed in the notebook both options to load cifar10 dataset. One as I mentioned above and Keras’ load dataset API with the same inputs, for your convenient.
•	The results are found in Squeezenet_Classifier notebook and main.py app for the inference code.

## Squeeze Net:
I tried two strategies to improve model performance, since CIFAR images are considerably smaller than ImageNet used for the original work. I created and focus the rest of this task on a smaller model for CIFAR-10 data set inspired by the 'Squeeze Net' architecture proposed by Forrest Iandola et al. (2016) and the work of Zac Hancock. I used similar components (fire module, etc.) and add some additional dropout and batch normalization layers to deal with overfitting and slow learning rate, respectively.</br>
Essentially, the fire module implements a strategy wherein it minimizes the input parameters by utilizing a 'squeeze layer' that only uses 1x1 filters. After the 'squeeze layer' is a series of both 1x1 and 3x3 filters in the 'expand layer' where later the expand layer is then concatenated. 
The benefits from using 1x1 filters are as follow:</br>

- The 1×1 filter can be used to create a linear projection of a stack of feature maps.
- The projection created by a 1×1 can act like channel-wise pooling and be used for dimensionality reduction.
- The projection created by a 1×1 can also be used directly or be used to increase the number of feature maps in a model.</br>

Using the fire module outlined above, the architecture was completed. Max Pooling happens after the very first convolution layer, followed by 4 fire modules. After the last fire module, 20% dropout is committed before the last convolution layer. Global pooling is committed right before SoftMax activation into 10 classes. 
Model summary and complete layers configuration found in Notebook. Number of layers were the same as in the Iandola paper only with 1/6 of number of parameters and with comparable performance.

### Model Evaluation
After tweaking layers architectures and adding batch normalization and few dropouts along the net, I reach best accuracy score using Adam optimizer. Train and test - cross entropy and accuracy were consistent along the epochs. This even improved after I added Keras’ data generator for data augmentation which help with overfitting. 
After more than 200 epoch accuracy reaches >0.8 and loss below 0.8.


### Inference
Running inference on REST flask API by run main.py from terminal and paste the local server url provides from the output in the browser. </br>
 <p align="center">
  <img src=https://github.com/taltole/CIFAR10_SqueezeNet/blob/master/saves/deploy.png? width="350" title="hover text">
</p>
<!---
 ![alt text](https://github.com/[username]/[reponame]/blob/[branch]/image.jpg?raw=true)
--->

