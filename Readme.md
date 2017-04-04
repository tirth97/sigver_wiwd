# Learned representation for Offline Handwritten Signature Verification

This repository contains the code and instructions to use the trained CNN models described in [1] to extract features for Offline Handwritten Signatures.

[1] Hafemann, Luiz G., Robert Sabourin, and Luiz S. Oliveira. "Learning Features for Offline Handwritten Signature Verification using Deep Convolutional Neural Networks" http://dx.doi.org/10.1016/j.patcog.2017.05.012

# Installation

## Pre-requisites 

The code is written in Python 2, and requires the following libraries:

* Scipy version 0.18
* Pillow version 3.0.0
* OpenCV
* Theano
* Lasagne

We recommend using the Anaconda python distribution [link](https://www.continuum.io/downloads), and create a new environment using: 
```
conda create -n sigver -y python=2
source activate sigver
```

The following commands install the dependencies 
```
conda install -y opencv "scipy=0.18.0" "pillow=3.0.0"
conda install -y jupyter notebook matplotlib # Optional, to run the example in jupyter notebook
pip install "Theano==0.9"
pip install https://github.com/Lasagne/Lasagne/archive/master.zip
```

This code can be used with or without GPUs. To use a GPU with Theano, follow the instructions in this [link](http://deeplearning.net/software/theano/tutorial/using_gpu.html). Note that Theano takes time to compile the model, so it is much faster to instantiate the model once and run forward propagation for many images (instead of calling many times a script that instantiates the model and run forward propagation for a single image).

## Downloading the models

* Clone (or download) this repository
* Download the pre-trained models from [link](https://www.etsmtl.ca/Unites-de-recherche/LIVIA/Recherche-et-innovation/Projets)
 * Save / unzip the models in the "models" folder

## Testing 

Run ```python example.py```. This script pre-process a signature, and compares the feature vector obtained by the model to the results obtained by the author. If the test fails, please check the versions of Scipy and Pillow. I noticed that different versions of these libraries produce slightly different results for the pre-processing steps.

# Usage

The following code (from example.py) shows how to load, pre-process a signature, and extract features using one of the learned models:

```
from scipy.misc import imread
from preprocess.normalize import preprocess_signature
import signet
from cnn_model import CNNModel

canvas_size = (952, 1360)  # Maximum signature size

# Load and pre-process the signature
original = imread('data/some_signature.png', flatten=1)

processed = preprocess_signature(original, canvas_size)

# Load the model
model_weight_path = 'models/signet.pkl'
model = CNNModel(signet, model_weight_path)

# Use the CNN to extract features
feature_vector = model.get_feature_vector(processed)

# Multiple images can be processed in a single forward pass using:
# feature_vectors = model.get_feature_vector_multiple(images)
```

We also included an interactive example, using jupyter notebook:
```
jupyter notebook
```

Look for the notebook "interactive_example.ipynb"