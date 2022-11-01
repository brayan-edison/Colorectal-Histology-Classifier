# Colorectal_Histology_Classifier
Classification of Colorectal Cancer Histology into 8 Classes.

### Dataset: 

The Dataset was taken from [Tensorflow Datasets Colorectal Histology](https://www.tensorflow.org/datasets/catalog/colorectal_histology) which contains 5000 images of colorectal cancer histology of size (150, 150) in RGB channel. 

Image Shape: (150, 150, 3).

### Target:
Multiclass Classification of Colorectal Cancer Histology images into 8 classes using a Convolutional Neural Network.

#### Classes:
```
- Tumor : Tumour epithelium
- Stroma : Simple stroma (homogeneous composition, includes tumour stroma, extra-tumoural stroma and smooth muscle)
- Complex : Complex stroma (containing single tumour cells and/or few immune cells)
- Lympho : Immune cells (including immune-cell conglomerates and sub-mucosal lymphoid follicles)
- Debris : Debris (including necrosis, hemorrhage and mucus)
- Mucosa : Normal mucosal glands
- Adipose : Adipose tissue
- Empty : No tissue

```
## Approach:

The Dataset is divided into training, validation and testing with the split ratio of 0.8/0.1/0.1 respectively. Each is passed through a preprocessing function that normalizes, generates a batch of 32 images and prefetches the batch. For training split, the images are further shuffled and cached for quick disk reading and writing.

### Convolutional Neural Network from Scratch:

#### Data Augmentation layer:
A data augmentation layer that performs random flip, random rotation and random zoom is used as the first layer of the network. 

This layer is used to introduce diversity in training dataset so that our model generalizes well for the testing phase. The layer is fed images in batches of 32.
```
Input shape: (32, 150, 150, 3)
Output shape: (32, 150, 150, 3)
```
#### Convolutional Layers:
2d convolutional layers are used with filter sizes 32 and 64. Activation function for each Conv2d layer is Rectified Linear function (ReLu) which is a piecewise linear function. ReLu is used to overcome the vanishing gradient problem that allows the model to learn faster and perform better.
The kernel for all convolutinal layers is of size (3x3) in order to maximize shape detection. 

2 stacks of 2 conv2d layers with filter size 32, where each stack is followed by a batchnormalization and a maxpooling layer. 
This is followed by another 2 stacks of 2 conv2d layers with filter size 64, where each stack is followed by a batchnormalization and a maxpooling layer.

##### BatchNormalization: 
Batch Normalization is used so that every layer of the network can do the learning more independently. It normalizes the output of the previous layers. It is also beenficial because it is used for regularization and hence prevents overfitting. 
###### Note: The initial batchnormalization paper used it before the activation function, but it has since been shown that batchnormalization should be used after the activation function.


![colorectal_histology](https://user-images.githubusercontent.com/47920247/199341500-b5b01544-cfab-40bc-acd7-32e33bf9afad.png)


#### Flatten, Dropout and Dense Layers 
##### Dropout:
The convolutional layers are followed by a dropout layer with rate = 0.26. Dropout layer is used to prevent overfitting. It randomly sets input to 0 with a frequency of rate (in this model: 0.26) Inputs not set to 0 are scaled up by 1/(1 - rate) such that the sum over all inputs is unchanged.
##### Flatten:
Flatten layer is used to collapse an ND Tensor to a 1D Tensor.
##### Dense:
2 dense layers with 512 units each, again with ReLu activation and finally a final output layer with 8 layers with a softmax activation function.
Softmax activation is used in the final layer because it calculates the relative probabilities for each class. 8 units/neurons for the final dense layer because there are 8 classes.

![Dense](https://user-images.githubusercontent.com/47920247/199346216-c7f80f0d-7d03-4321-b0ca-04d6f9fb3e66.png)

### Model Compilation:
#### Optimizer:
Rectified Adam is used as the optimizer instead of Adam or SGD. This is done due to the bad convergence problem that arises in Adam and SGD. Both Adam and SGD are also sensitive to learning rate and hence rectified Adam is used, which introduces a term to rectify the variance of adaptive learning rate. 

![radam_adam_comparison](https://user-images.githubusercontent.com/47920247/199347689-9e13c38c-d0a8-43b0-8bec-82372263879b.png)

The learning rate for RAdam is 0.001 for the first 10 epochs and then it increases exponentially till the training is complete. This is realized using a Learning Rate scheduler.
#### Loss Function:
Sparse categorical crossentropy is used as the loss function.
#### Metrics:
Accuracy is used as the metrics for the model.

### Model checkpoint:
A model checkpoint is created that saves the weights of neurons of the best performing epoch w.r.t validation accuracy in a file.

## Results:
The best model gives a validation accuracy of 93.8%. 

![acc](https://user-images.githubusercontent.com/47920247/199352327-489ec74b-70eb-4bb1-9098-5ebd4495658c.png)

On the test data, the model gives an accuracy of 91%
