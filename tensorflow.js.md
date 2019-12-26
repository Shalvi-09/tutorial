# tensorflow.js
https://www.youtube.com/watch?v=imzedQr2tTc

- [tensorflow.js](#tensorflowjs)
  - [Playground](#playground)
  - [Pre-Trained Models](#pre-trained-models)
    - [Mobile net](#mobile-net)
    - [PoseNet](#posenet)
    - [CocoSSD](#cocossd)
    - [Body Pix](#body-pix)
    - [USE](#use)
  - [Getting started](#getting-started)

## Playground

[playground](playground.tensorflow.org)

## Pre-Trained Models

### Mobile net

Object recognition

### PoseNet

All about Human pose detection

### CocoSSD

Object Localization

### Body Pix

Human Segmentation

### USE

Text Classification

All pre trained models are vaialable at

[https://github.com/tensorflow/tfjs-models](https://github.com/tensorflow/tfjs-models)

## Getting started

import js 

```
<script src="cdn.jsdelivr.net/npm/@tensorflow/tfjs"></script>

<script src="cdn.jsdelivr.net/npm/@tensorflow-models/body-pix"></script>


// then use normal img, like

<img src="baby-jpg"></img>

// then load the model
// These models are hosted by google on their own GCP, so you do not need to pay for that

const net=await bodyPix.load();

// get the segmentation
const segmentation= await net.estimatePersonSegmentation(image);

// result, you will get json //object represent that baby boy in image
{
    width:640,
    height:480,
    data: Unit8Array(307200)[0,0,1,....]; // you do not need to understand this data part
}


// you can event draw mask on them

bodyPix.drawPixelatedMask(...)

```
