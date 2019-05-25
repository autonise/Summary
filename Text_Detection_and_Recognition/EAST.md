# EAST

## Summary
	This paper was the previous state of the art.This paper has an easy understand pipeline compared to the other text_detection algorithms

## Ideas
	1. The problem with text is its varying size.For text having large size features from latter layers will be more useful and for text having smaller size features from earlier layers will be more useful. Therefore we come up with an idea of skip connections i.e. extract features from various layers and concatenate them(Used in UNet)
	1. Instead of using the standard nms algorithm they came up with a faster algortihm namely locality-aware nms with the assumption that the bounding boxes predicted by the nearby pixels are correlated 
	1. At the end of the network we predict a score map and a geometry(A rotated bounding box or a quadrangle)

## Architecture
	The model can be divided into 3 parts namely feature extractor stem feature merging branch and output layer.The standard feature extractor used in the network is either PVA Net or the standard VGG Net.We extract features from the intermediate stages of the network and feed it to the merging branch.The merging branch upsamples the features to match the dimensions of the feature recieved from skip connections and contenates it with those features.After that a 1X1 convolution is applied to reduce the number of channels and then we apply a standard 3X3 convolution filter.At the output we apply a 1X1 filter to predict the scoremap and geometry(rotated bounding box or quadrangle).After this we apply locality-area nms and they are considered the final output of the product


## Loss Function
	The loss function=(sum of the losses in score map) + lambda*(sum of the losses in geometry)
	lambda is used to show which loss is more important.For testing lambda is set to 1 and The loss function used is The Dice Loss Coefficient

## Training Details
	1. Optimiser:-Adam
	1. lr:- 1e-3
	1. batch-size :- 24
	1. image-size :-512X512