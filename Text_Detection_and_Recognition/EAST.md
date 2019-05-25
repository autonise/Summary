# EAST

## Summary
	This paper was the previous state of the art.This paper has an easy understand pipeline compared to the other text_detection algorithms

## Ideas
	1. The problem with text in its varying size.For text having large size features from latter layers will be more useful
	1. SSD

## Architecture
	The model is based on VGG16 model.We use conv1,conv2,conv3,conv4 and conv5 of VGG16.Conv_6 and Conv_7 of the SSD replaces the fully-connected layers of the VGG16 model.After that we 8 convolution layers divided in four layers(conv8-conv11) we take the output.Multiple output layers are inserted after every layer known as the text box layer.There is an nms layer after the text box layer Since the entire network is made up of only pooling  and convulation layers the model can adapt to images of variouse size and shape.

## Training and implementation
	We represent the non rotated boxes in the form of rectangle(x,y,w,h) or a qudriateral(point1,point2,point3,point4) and for a rectangular box we represent it by 