# Convolutional Neural Networks for Steady Flow Approximation

This repository contains an re-implementation of the paper [Convolutional Neural Networks for Steady Flow Approximation](https://www.autodeskresearch.com/publications/convolutional-neural-networks-steady-flow-approximation). The premise is to learn a mapping from boundary conditions to steady state fluid flow. There a few differences and improvements from this work the original paper that are discussed bellow.

# Getting data and making TFrecords
This is the most difficult part of this project. [Mechsys](http://mechsys.nongnu.org/) was used to generate the fluid simulations necessary for training however it can be difficult to set up requires a fair number of packages. In light of this, I will attempt to make a compressed dataset and place it on google drive (should be around 800 MB). Until then, all the source code to generated the fluid flow is in the `fluid_generator` directory.

# Training
To train enter the `train` directory and run
```
python flow_train.py
```

# Tensorboard
Some training information such as the loss is recorded and can be viewed with tensorboard. The checkpoint file is found in `checkpoint` and has a name corresponding to the parameters used.

# Evaluation
Once the model is trained sufficiently you can evaluate it by running
```
python flow_test.py
```
This will run through the car dataset provided and do side by side comparisons. Here are a few cool images it will generated (model still training)!

![alt tag](https://github.com/loliverhennigh/Steady-State-Flow-With-Neural-Nets/blob/master/test/figs/car_flow_1.png)


# Model details
As mentioned above, this work deviates from that seen in the original paper. Instead of using Signed Distance Function as input we use a binary representation of the boundary conditions. This simplifies the input greatly. We also use a U-network approach with residual layers similar to that seen in [Pixel-CNN++](https://github.com/openai/pixel-cnn). This seems to make learning incredibly fast and decreases the requirement of a large dataset. Notably, our model is trained on only 3,000 flow images instead of the 100,000 listed in the paper and still produces comparable performance.

# Performance
Still training it up! The time pre image in a batch size of 8 is 0.00875 seconds on a GTX 1080 GPU. This is comparable to their reported time of 0.0085 seconds. While our network is more complex we are able to achieve similar levels of speed by not relying on any fully connected layers and keep our network all convolutional.



