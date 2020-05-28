# README.md

<img src="https://www.gstatic.com/devrel-devsite/prod/v050cadc3f3cf927d4089880349cc4dea1a9dab3bc6036e7a65cc361fddd65555/tensorflow/images/lockup.svg">

This is a running walkthru to use TensorFlows [(www.tensorflow.org)](https://www.tensorflow.org/) object detection capabilities.

# Clone tutorial project

```bash
# use your projects folder
cd /home
# clone this repository
git clone https://github.com/TomFreudenberg/TensorFlow-ObjectDetection.git tf-tutorial
cd tf-tutorial
```


# Run tutorial

If all tools and packages installed, you may run the object_detection example:

```bash
cd src/tutorial
python image_object_detection.py
```


# Setup environment

```bash
# prepare a virtual environment path
mkdir -p venv/bin
# install packages
pip install tensorflow
pip install Cython
pip install contextlib2
pip install pillow
pip install lxml
pip install jupyter
pip install matplotlib
```

### Download latest Protobuf

Download the latest Protobuf version for your OS and extract it.

* https://github.com/google/protobuf/releases

##### OSX binary (e.g.)

```bash
cd venv
curl -L -O https://github.com/protocolbuffers/protobuf/releases/download/v3.12.2/protoc-3.12.2-osx-x86_64.zip
unzip protoc-3*.zip
cd ..
```


### Download TensorFlow models from github

Clone or download TensorFlow’s Model from Github. The downloaded archive must be extracted as "models" folder.

* https://github.com/tensorflow/models

```bash
mkdir -p src/tensorflow
cd src/tensorflow
curl -L -O https://github.com/tensorflow/models/archive/master.zip
unzip master.zip
mv models-master models
cd ../..
```

Patch models to be compatible with TensorFlow 2 and newer tools
```bash
(
echo '--- input_reader.proto'
echo '+++ input_reader.proto'
echo '@@ -4 +4 @@'
echo '-import "object_detection/protos/image_resizer.proto";'
echo '+// import "object_detection/protos/image_resizer.proto";'
) | patch -s src/tensorflow/models/research/object_detection/protos/input_reader.proto

(
echo '--- ops.py'
echo '+++ ops.py'
echo '@@ -27,2 +27,2 @@'
echo '-import tensorflow.compat.v1 as tf'
echo '-import tf_slim as slim'
echo '+import tensorflow as tf'
echo '+'
echo '@@ -846 +846 @@'
echo '-        box_ind=tf.range(num_boxes),'
echo '+        box_indices=tf.range(num_boxes),'
) | patch -s src/tensorflow/models/research/object_detection/utils/ops.py
```

### Run protoc to create .py from proto files

Open the research folder and run protobuf:

```bash
cd src/tensorflow/models/research/
../../../../venv/bin/protoc object_detection/protos/*.proto --python_out=.
cd ../../../..
```

Check if this had worked, while looking at the protos folder. Every proto file should have a python file created.


# Credits

* Tutorial inspired from: https://www.edureka.co/blog/tensorflow-object-detection-tutorial/
