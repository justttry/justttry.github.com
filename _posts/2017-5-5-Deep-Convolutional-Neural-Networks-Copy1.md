---
layout: post
title: Deep CNN Models!
---

# Deep CNN Models

Constructing and training your own ConvNet from scratch can be Hard and a long task.

A common trick used in Deep Learning is to use a **pre-trained** model and finetune it to the specific data it will be used for. 

## Famous Models with Keras


This notebook contains code and reference for the following Keras models (gathered from [https://github.com/fchollet/keras/tree/master/keras/applications]())

- VGG16
- VGG19
- ResNet50
- Inception v3
- Xception
- ... more to come


## References

- [Very Deep Convolutional Networks for Large-Scale Image Recognition](https://arxiv.org/abs/1409.1556) - please cite this paper if you use the VGG models in your work.
- [Deep Residual Learning for Image Recognition](https://arxiv.org/abs/1512.03385) - please cite this paper if you use the ResNet model in your work.
- [Rethinking the Inception Architecture for Computer Vision](http://arxiv.org/abs/1512.00567) - please cite this paper if you use the Inception v3 model in your work.


All architectures are compatible with both TensorFlow and Theano, and upon instantiation the models will be built according to the image dimension ordering set in your Keras configuration file at `~/.keras/keras.json`. 

For instance, if you have set `image_data_format="channels_last"`, then any model loaded from this repository will get built according to the TensorFlow dimension ordering convention, "Width-Height-Depth".

# VGG16

<img src="https://justttry.github.io/images/vgg16.png" >

# VGG19

<img src="https://justttry.github.io/images/vgg19.png" >

# `keras.applications`


```python
from keras.applications import VGG16
from keras.applications.imagenet_utils import preprocess_input, decode_predictions
import os
```

    Using TensorFlow backend.



```python
# -- Jupyter/IPython way to see documentation
# please focus on parameters (e.g. include top)
VGG16??
```


```python
help(VGG16)
```

    Help on function VGG16 in module keras.applications.vgg16:
    
    VGG16(include_top=True, weights='imagenet', input_tensor=None, input_shape=None, pooling=None, classes=1000)
        Instantiates the VGG16 architecture.
        
        Optionally loads weights pre-trained
        on ImageNet. Note that when using TensorFlow,
        for best performance you should set
        `image_data_format="channels_last"` in your Keras config
        at ~/.keras/keras.json.
        
        The model and the weights are compatible with both
        TensorFlow and Theano. The data format
        convention used by the model is the one
        specified in your Keras config file.
        
        # Arguments
            include_top: whether to include the 3 fully-connected
                layers at the top of the network.
            weights: one of `None` (random initialization)
                or "imagenet" (pre-training on ImageNet).
            input_tensor: optional Keras tensor (i.e. output of `layers.Input()`)
                to use as image input for the model.
            input_shape: optional shape tuple, only to be specified
                if `include_top` is False (otherwise the input shape
                has to be `(224, 224, 3)` (with `channels_last` data format)
                or `(3, 224, 244)` (with `channels_first` data format).
                It should have exactly 3 inputs channels,
                and width and height should be no smaller than 48.
                E.g. `(200, 200, 3)` would be one valid value.
            pooling: Optional pooling mode for feature extraction
                when `include_top` is `False`.
                - `None` means that the output of the model will be
                    the 4D tensor output of the
                    last convolutional layer.
                - `avg` means that global average pooling
                    will be applied to the output of the
                    last convolutional layer, and thus
                    the output of the model will be a 2D tensor.
                - `max` means that global max pooling will
                    be applied.
            classes: optional number of classes to classify images
                into, only to be specified if `include_top` is True, and
                if no `weights` argument is specified.
        
        # Returns
            A Keras model instance.
        
        # Raises
            ValueError: in case of invalid argument for `weights`,
                or invalid input shape.
    



```python
vgg16 = VGG16(include_top=True, weights='imagenet')
```

    Downloading data from https://github.com/fchollet/deep-learning-models/releases/download/v0.1/vgg16_weights_tf_dim_ordering_tf_kernels.h5



    ---------------------------------------------------------------------------

    Exception                                 Traceback (most recent call last)

    <ipython-input-4-622c1ff23a67> in <module>()
    ----> 1 vgg16 = VGG16(include_top=True, weights='imagenet')
    

    /home/z/anaconda2/lib/python2.7/site-packages/keras/applications/vgg16.pyc in VGG16(include_top, weights, input_tensor, input_shape, pooling, classes)
        162             weights_path = get_file('vgg16_weights_tf_dim_ordering_tf_kernels.h5',
        163                                     WEIGHTS_PATH,
    --> 164                                     cache_subdir='models')
        165         else:
        166             weights_path = get_file('vgg16_weights_tf_dim_ordering_tf_kernels_notop.h5',


    /home/z/anaconda2/lib/python2.7/site-packages/keras/utils/data_utils.pyc in get_file(fname, origin, untar, md5_hash, file_hash, cache_subdir, hash_algorithm, extract, archive_format, cache_dir)
        201                             functools.partial(dl_progress, progbar=progbar))
        202             except URLError as e:
    --> 203                 raise Exception(error_msg.format(origin, e.errno, e.reason))
        204             except HTTPError as e:
        205                 raise Exception(error_msg.format(origin, e.code, e.msg))


    Exception: URL fetch failure on https://github.com/fchollet/deep-learning-models/releases/download/v0.1/vgg16_weights_tf_dim_ordering_tf_kernels.h5: None -- [Errno 110] Connection timed out



```python
from keras.utils.vis_utils import plot_model
from IPython.display import Image
plot_model(model, to_file="vgg16.png", show_shapes=True)
Image('vgg16.png')
```

If you're wondering **where** this `HDF5` files with weights is stored, please take a look at `~/.keras/models/`

#### HandsOn VGG16 - Pre-trained Weights


```python
IMAGENET_FOLDER = 'imgs/imagenet'  #in the repo
```


```python
!ls imgs/imagenet
```

    apricot_565.jpeg  apricot_787.jpeg	strawberry_1174.jpeg
    apricot_696.jpeg  strawberry_1157.jpeg	strawberry_1189.jpeg


<img src="https://justttry.github.io/images/strawberry_1157.jpeg" >


```python
from keras.preprocessing import image
import numpy as np

img_path = os.path.join(IMAGENET_FOLDER, 'strawberry_1157.jpeg')
img = image.load_img(img_path, target_size=(224, 224))
x = image.img_to_array(img)
x = np.expand_dims(x, axis=0)
x = preprocess_input(x)
print('Input image shape:', x.shape)

preds = vgg16.predict(x)
print('Predicted:', decode_predictions(preds))
```

    ('Input image shape:', (1, 224, 224, 3))



    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-7-49458049ff08> in <module>()
          9 print('Input image shape:', x.shape)
         10 
    ---> 11 preds = vgg16.predict(x)
         12 print('Predicted:', decode_predictions(preds))


    NameError: name 'vgg16' is not defined


<img src="https://justttry.github.io/images/apricot_696.jpeg" >


```python
img_path = os.path.join(IMAGENET_FOLDER, 'apricot_696.jpeg')
img = image.load_img(img_path, target_size=(224, 224))
x = image.img_to_array(img)
x = np.expand_dims(x, axis=0)
x = preprocess_input(x)
print('Input image shape:', x.shape)

preds = vgg16.predict(x)
print('Predicted:', decode_predictions(preds))
```

    Input image shape: (1, 224, 224, 3)
    Predicted: [[('n07747607', 'orange', 0.87526792), ('n07749582', 'lemon', 0.03620464), ('n07717556', 'butternut_squash', 0.021843448), ('n03937543', 'pill_bottle', 0.0126132), ('n03942813', 'ping-pong_ball', 0.0054204506)]]


<img src="https://justttry.github.io/images/apricot_565.jpeg" >


```python
img_path = os.path.join(IMAGENET_FOLDER, 'apricot_565.jpeg')
img = image.load_img(img_path, target_size=(224, 224))
x = image.img_to_array(img)
x = np.expand_dims(x, axis=0)
x = preprocess_input(x)
print('Input image shape:', x.shape)

preds = vgg16.predict(x)
print('Predicted:', decode_predictions(preds))
```

    Input image shape: (1, 224, 224, 3)
    Predicted: [[('n07718472', 'cucumber', 0.29338178), ('n07716358', 'zucchini', 0.2383192), ('n04596742', 'wok', 0.042132568), ('n07716906', 'spaghetti_squash', 0.038422), ('n07711569', 'mashed_potato', 0.036552209)]]


# Hands On:

### Try to do the same with VGG19 Model


```python
# from keras.applications import VGG19
```

# Residual Networks

<img src="https://justttry.github.io/images/resnet_bb.png" >

## ResNet 50

<img src="https://justttry.github.io/images/resnet34.png" >


```python
## from keras.applications import ...
```


```python

```
