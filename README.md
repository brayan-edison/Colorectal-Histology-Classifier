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

This layer is used to introduce diversity in training dataset so that our model generalizes well for the testing phase. 
```
Input shape: (150, 150, 3)
Output shape: (150, 150, 3)
```
