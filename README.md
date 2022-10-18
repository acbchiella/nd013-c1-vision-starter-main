# Object Detection in an Urban Environment


### Project overview
[...]
This section should contain a brief description of the project and what we are trying to achieve. Why is object detection such an important component of self driving car systems?

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

For this project is used the SSD Resnet 50 640x640 model. It is necessary to download the pretrained model and move it to `/home/workspace/experiments/pretrained_model/`. Follow the steps boellow:
```
cd /home/workspace/experiments/pretrained_model/

wget http://download.tensorflow.org/models/object_detection/tf2/20200711/ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz

tar -xvzf ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz

rm -rf ssd_resnet50_v1_fpn_640x640_coco17_tpu-8.tar.gz
```

#### Requeriments

**This section should contain a brief description of the steps to follow to run the code for this repository.**

### Dataset

#### Dataset analysis
This section should contain a quantitative and qualitative description of the dataset. It should include images, charts and other visualizations.

#### Cross validation
This section should detail the cross validation strategy and justify your approach.

### Training
#### Reference experiment
This section should detail the results of the reference experiment. It should includes training metrics and a detailed explanation of the algorithm's performances.

#### Improve on the reference
This section should highlight the different strategies you adopted to improve your model. It should contain relevant figures and details of your findings.
