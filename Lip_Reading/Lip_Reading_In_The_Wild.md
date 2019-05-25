# Lip Reading In the wild (https://www.robots.ox.ac.uk/~vgg/publications/2016/Chung16/chung16.pdf)

## Terms used 

    Homophemes - Phonemes which sound different but have similar lip movement
    
## Summary

    They provide a big dataset and state-of-the-art results on it by using 2-D convolutions and combining frames with shared weights in temporal dimension to predict words from a fixed lexicon corpus
    
    Doubts which remain - How do they find clips which contain only one-word?
    
## Abstract

    Aim to recognise words given just video in the wild for full sentences, speaker independent. 
    Two Contributions - 
        Better Model
        Large Dataset creation pipeline
        
## Introduction
    
    Applications - 
        
        Understanding archival silent films
        Understanding what people are saying using just their videos when audio is not available to eavesdrop
       
    Homophemes create ambiguity, interestingly the same is available also in audio wherein 'm' and 'n' sound similar but 
    visually they look very different.
    
    Problems faces in Lip Reading - 
    
        Accents
        Speed of speaking
        mumbling
        Poor Lighting 
        Strong Shadows
        Motion
        Resolution
        Foreshortening
        
    The standard approach in this field is to Use Hidden Markov Models or Recurrent Neural Networks but they use Convolutional Neural Networks to directly predict the words.
    
    Comparisions are done on the standard OuluVS benchmark dataset
    
### Related Work

    Too many works have been referenced without giving their details. Need to study each and every one of them.
    
## Building the data-set

    Stage 1 
    
    Selected Program Types - News and current affairs - clear faces
    
    Stage 2
    
    Subtitle Processing and alignment - 
    
    The only noise which remains is when the interviewer or person is dubbed in another language
    
    Stage 3
    
    Face Tracking/Detection - Used HOG features and KLT tracker
    
    Stage 4
    
    To check whether the person is speaking or not the frequency with which he opens or closes the mouth is used and compared with the frequency of the speech
    
    Stage 5
    
    Removes very small words from the dataset because they can have many variations. This might be bad
    Longer words are also removed which are more than 1 second in length. (This might not hinder that much)
    
## Network architecture and Training

### Architecture

    Use VGG-M as it is faster to train and experiment on
    Experiment on 4 models which primarily differ on how they "ingest" the T input frames
    
    EF3 (3D Conolutio with Early Fusion) - Just Fuse the temporal images and have a 3-D convolution
    MT-3 (3D Convolution with Multiple Towers) - Before the 3-D convolution normal 2-D convolution is done on each from with shared weights
    Early Fusion (EF) - Concatenate all the frames and apply 2-D convolution, which would require a lot of parameters and overfitting
    Multiple Towers (MT) - first convolution filter with shared weights for all the frames. Then concatenate the output across channels and apply 2-D convolution
    
    Although intutively 3-D convolutions might sound better, 2-D convolutions give better results emperically.
    Image size input is just 112*112 as mouth region is quite small
    
### Training

    Consistent Data augmentation techniques across all frames from the same video.
    Standard augmentation - color jittering, random crops, flipping
    *What do they mean by random shifts in the time domain by up to 0.2 secs
    The training was stopped in 20 epochs or whenever the validation error did not improve in 3 epochs
    
## Experiments

### Comparision of architectures

    Evaluation Protocol - 
    
    Recall@K - proportion of time s that the correct class is found in the top-K predictions for the word
    Edit Distance
    
    Results - 
    
    2-D works better than 3-D and multiple tower gives better results. So MT works the best.
    Top -10 error rate is low but top -1 error rate is high due to challenges in lip reading which were mentioned in the introduction
    
### Analysis of confusions

    Plurals are wrongly classified - Ok
    Cannot be distinguished (Homoheme) - OK
    
    For better testing they mixed up these Homoheme and Plurals to get 333 Word-classes
    
    The model is fine-tuned on this grouped dataset for 1 epoch and the accuracy increases.
    
    Still there are errors in the model specially between words which share many common phonetics
    
    Also errors present due to bad quality of videos due to compression artifacts, dropped frames and international accents
    
### Visualisation of salient mouth shapes

    They have visualised which part of the video actually contributed most towards reducing the loss function
    
### Comparision to state of the art

    They have a much larger lexicon still get the same accuracy in top-1 as compared to other papers
    
    The pre-processing steps are really bad in the paper. They repeat the frames if the video is shorter than 1 sec ( 25 frames ) and random crop if it is larger than 1 sec
    They are claiming that their model generalises well even on videos having bad lighting conditions
