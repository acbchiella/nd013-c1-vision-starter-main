# Object Detection in an Urban Environment

## Table of contents
- [Project overview](#project-overview)
- [Set up](#set-up)
  - [Enviroment](#enviroment)
  - [Directory structure](#directory-structure)
- [Requeriments](#requeriments)
- [Dataset](#dataset)
  - [Dataset analysis](#dataset-analysis)
- [Training](#training)
  - [Reference experiment](#reference-experiment)
  - [Improve on the reference](#improve-on-the-reference)


## Project overview

Object detection is an important tool for autonomous navigation. This tool enables the recognition of objects in a scene, such as vehicles, and traffic signs. In this project, the main goal is to use a neural network model to predict three classes of objects, vehicles, cyclists, and pedestrians.

## Set up

### Enviroment
There are two options:
  1. **classroom environment** - all the necessary data and libraries are already available;
  2. **local environment** - follows [README](./build/README.md) to create a docker container and install all prerequisites.

For this project, I used the first option.

### Directory structure
The data for training, validation and testing is organized in the `/home/workspace/data/` directory as follows:

- **train**: contain the training data;
- **val**: contain the validation data;
- **test** - contains the files to test the model and create inference videos.

The `/home/workspace/experiments` folder is organized as follow:

- **pretrained_model**
- **reference** - reference training with the unchanged config file;
- exporter_main_v2.py - to create an inference model;
- model_main_tf2.py - to launch training;
- **experiment0** - first experiment folder;
- **experiment1** - second experiment folder;
- label_map.pbtxt.

## Requeriments

For this project is used the SSD Resnet 50 640x640 model. It is necessary to download the pretrained model and move it to `/home/workspace/experiments/pretrained_model/`. Follow the steps boellow:
```
cd /home/workspace/experiments/pretrained_model/

wget http://download.tensorflow.org/models/object_detection/tf2/20200711/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz

tar -xvzf ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz

rm -rf ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz
```

## Dataset

### Dataset analysis
Figure 1 shows 10 random images taken from the dataset. It is possible to notice that vehicles are the majority, followed by pedestrians, then cyclists. The analysis of class distribution can be verified in Figure 2, which corroborates with what is shown in the sample 10 images in Figure 1. 

Figure 3 shows the light distribution. In a manual inspection, I noticed that images with V<80 have poor light conditions. In the dataset, the amount of images with poor light conditions is inferior to those with good light conditions (normal sun day).

<figure>
<img src="./images/random_output.png" alt="Trulli" style="width:100%">
<figcaption align = "center">Figure 1. 10 randomly displayed images. Vehicles are labeled in red, pedestrians in green, and cyclists in blue.</figcaption>
</figure>

<figure>
<img src="./images/classes.png" alt="Trulli" style="width:100%">
<figcaption align = "center">Figure 2. Distribution of classes.</figcaption>
</figure>


<figure>
<img src="./images/light.png" alt="Trulli" style="width:100%">
<figcaption align = "center">Figure 3. Distribution of V (using HSV color model). Notice that, images with lower V have less light.</figcaption>
</figure>

## Training
### Reference experiment
The reference experiment uses the `ssd_resnet50_v1_fpn_keras` feature extractor, `random_crop_image` and `random_horizontal_flip` data augmentation approaches, and the `momentum_optimizer` optimizer. Figure 7 shows the optimizer learning rate.

Figures 5 and 6 show the precision and total loss. Notice that, for the reference experiment, the precision is the lowest and the total loss is the greatest.

### Improve on the reference
In order to improve the reference experiment, I present two approaches. The first one is to augment the data using `random_adjust_brightness`, `random_adjust_contrast`, and `random_black_patches` strategies. The idea is to have more data with poor light and partial objects occluded. In the second approach, besides the data augmentation performed in the first one, I changed the optimizer to `adam_optimizer`. Figure 8 shows the learning rate strategy for `adam_optimizer`.

Notice the improvements in the precision by using data augmentation strategies and another optimizer. However, the final experiment still has low precision. 

<table style="border-style:hidden">
<tr style="border-style:hidden">
  <td style="border-style:hidden">
    <img src="./images/aug1.png" alt="Trulli" style="width:100%%">
    <figcaption align = "center">Figure 4.1. Grayscale and black patches augmentations.</figcaption>
  </td>
  <td>
    <img src="./images/aug4.png" alt="Trulli" style="width:100%">
    <figcaption align = "center">Figure 4.4. Brightness adjusts and black patches augmentations.</figcaption>
  </td>
</tr>
<tr style="border-style:hidden">
  <td colspan="2" style="border-style:hidden"><figcaption align = "center">Figure 4. Data augumentattion.</figcaption></td>
</tr>
</table>


<table style="border-style:hidden">
<tr style="border-style:hidden">
  <td style="border-style:hidden">
    <img src="./images/prec.png" alt="Trulli" style="width:100%%">
    <figcaption align = "center">Figure 5. Detection precision. Green color for the final experiment, light blue for the experiment 0, and dark blue for the reference experiment.</figcaption>
  </td>
  <td>
    <img src="./images/total_loss.png" alt="Trulli" style="width:100%">
    <figcaption align = "center">Figure 6. Total loss. Orange and green colors for the final experiment, light blue and brown for experiment 0, and dark blue and magenta for the reference experiment.</figcaption>
  </td>
</tr>
<tr style="border-style:hidden">
  <td style="border-style:hidden">
    <img src="./images/lr_r.png" alt="Trulli" style="width:100%%">
    <figcaption align = "center">Figure 7. Learning rate for reference experiment and experiment 0.</figcaption>
  </td>
  <td>
    <img src="./images/lr_f.png" alt="Trulli" style="width:100%">
    <figcaption align = "center">Figure 8. Learning rate for the final experiment.</figcaption>
  </td>
</table>