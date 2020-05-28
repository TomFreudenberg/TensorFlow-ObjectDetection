# README.md

* Tutorial inspired: https://www.edureka.co/blog/tensorflow-object-detection-tutorial/

# Create example folder

```bash
# use your projects folder
cd /home
# create folder for tutorial
mkdir tensorflow-tutorial
# open new folder
cd tensorflow-tutorial
```

# Setup environment

```bash
# prepare virtual environment
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

### Download latest protoc for OS

You need to Download Protobuf version 3.4 or above for this demo and extract it.

* https://github.com/google/protobuf/releases

##### OSX binary

```bash
cd venv
curl -L -O https://github.com/protocolbuffers/protobuf/releases/download/v3.12.2/protoc-3.12.2-osx-x86_64.zip
unzip protoc-3*.zip
cd ..
```

### prepare Source folder

```bash
mkdir -p src
cd src
mkdir -p tensorflow
mkdir -p tutorial
```

### Download TensorFlow models from github

Now you need to Clone or Download TensorFlow’s Model from Github. Once downloaded and extracted rename the “models-masters” to just “models“.

* https://github.com/tensorflow/models

```bash
cd tensorflow
curl -L -O https://github.com/tensorflow/models/archive/master.zip
unzip master.zip
mv models-master models
cd ..
```

Patch models to be compatible with TensorFlow 2 and newer tools
```bash
(
echo '--- input_reader.proto'
echo '+++ input_reader.proto'
echo '@@ -4 +4 @@'
echo '-import "object_detection/protos/image_resizer.proto";'
echo '+// import "object_detection/protos/image_resizer.proto";'
) | patch -s tensorflow/models/research/object_detection/protos/input_reader.proto

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
) | patch -s tensorflow/models/research/object_detection/utils/ops.py
```

### Create .py from proto files

Next, we need to go inside the research folder and run protobuf:

```bash
cd tensorflow/models/research/
../../../../venv/bin/protoc object_detection/protos/*.proto --python_out=.
cd ../../..
```

To check whether this worked or not, you can look at the protos folder and check that for every proto file there’s one python file created.
cd t

# Run tutorial

Now being able to run object_detection example:

```bash
cd tutorial
python image_object_detection.py
```
