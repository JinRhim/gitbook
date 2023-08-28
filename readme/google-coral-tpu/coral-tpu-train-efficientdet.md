---
description: Object Classification for Coral Dev Board
---

# Coral TPU: Train EfficientDet

[Google tutorial](https://colab.research.google.com/github/google-coral/tutorials/blob/master/retrain\_efficientdet\_model\_maker\_tf2.ipynb) shows how to train your own model for Coral Dev Board.

However, trying to follow this official Google tutorial has inflicted 3 hours of agony and pain on me, as it caused incessant amounts of numpy, tensorflow, and many other library configuration errors. If Google's tutorial code worked for you, you have no need to read this post.&#x20;



After several attempts to fix the problem through google colab, I eventually created jupyter notebook and modified the code a bit.&#x20;



Below is my method of training and deploying the model.&#x20;

It will only detect two objects for testing purposes:&#x20;

* Apple
* The exoglove model that I am trying to detect&#x20;

### Dataset if you need&#x20;

If you need a simple dataset that you want to test, use the below dataset [https://drive.google.com/file/d/1Ji2fiGojwNSJEInsZf1sLoGN2WA3f8ql/view?usp=sharing](https://drive.google.com/file/d/1Ji2fiGojwNSJEInsZf1sLoGN2WA3f8ql/view?usp=sharing) which I have been labeling with Microsoft VoTT.&#x20;

However, this code will require you to make separate folders for images and XML file. Use the below script to separate the image and annotation folder if needed

```
import os
import shutil

def copy_files(source_dir, dest_images_dir, dest_annotations_dir):
    for filename in os.listdir(source_dir):
        if filename.endswith('.jpg'):
            shutil.copy(os.path.join(source_dir, filename), os.path.join(dest_images_dir, filename))
        elif filename.endswith('.xml'):
            shutil.copy(os.path.join(source_dir, filename), os.path.join(dest_annotations_dir, filename))

def main():
    images_folder = "images"
    annotations_folder = "annotations"
    os.makedirs(images_folder, exist_ok=True)
    os.makedirs(annotations_folder, exist_ok=True)

    while True:
        folder_name = input("Enter folder name (Enter '0' to mark the end): ")
        if folder_name == '0':
            break
        if not os.path.isdir(folder_name):
            print(f"Folder '{folder_name}' does not exist. Skipping...")
            continue

        copy_files(folder_name, images_folder, annotations_folder)
    
    print("Copying complete.")

if __name__ == "__main__":
    main()
```

### Create a separate conda environment for efficientDet. Below is the environment.yml for my env.&#x20;

```
name: coral
channels:
  - conda-forge
  - defaults
dependencies:
  - _libgcc_mutex=0.1=main
  - _openmp_mutex=5.1=1_gnu
  - ca-certificates=2023.7.22=hbcca054_0
  - cudatoolkit=11.8.0=h6a678d5_0
  - ld_impl_linux-64=2.38=h1181459_1
  - libffi=3.4.4=h6a678d5_0
  - libgcc-ng=11.2.0=h1234567_1
  - libgomp=11.2.0=h1234567_1
  - libstdcxx-ng=11.2.0=h1234567_1
  - ncurses=6.4=h6a678d5_0
  - openssl=3.0.9=h7f8727e_0
  - pip=23.2.1=py39h06a4308_0
  - python=3.9.17=h955ad1f_0
  - readline=8.2=h5eee18b_0
  - setuptools=68.0.0=py39h06a4308_0
  - sqlite=3.41.2=h5eee18b_0
  - tk=8.6.12=h1ccaba5_0
  - wheel=0.38.4=py39h06a4308_0
  - xz=5.4.2=h5eee18b_0
  - zlib=1.2.13=h5eee18b_0
  - pip:
      - absl-py==1.4.0
      - anyio==3.7.1
      - argon2-cffi==21.3.0
      - argon2-cffi-bindings==21.2.0
      - array-record==0.4.0
      - arrow==1.2.3
      - asttokens==2.2.1
      - astunparse==1.6.3
      - async-lru==2.0.3
      - attrs==23.1.0
      - audioread==3.0.0
      - babel==2.12.1
      - backcall==0.2.0
      - bcrypt==4.0.1
      - beautifulsoup4==4.12.2
      - bleach==6.0.0
      - cachetools==5.3.1
      - certifi==2023.7.22
      - cffi==1.15.1
      - charset-normalizer==3.2.0
      - click==8.1.6
      - comm==0.1.3
      - cryptography==41.0.2
      - cycler==0.11.0
      - cython==3.0.0
      - dataclasses==0.6
      - debugpy==1.6.7
      - decorator==5.1.1
      - defusedxml==0.7.1
      - dm-tree==0.1.8
      - etils==1.4.0
      - exceptiongroup==1.1.2
      - executing==1.2.0
      - fastjsonschema==2.18.0
      - fire==0.5.0
      - flatbuffers==23.5.26
      - fqdn==1.5.1
      - gast==0.4.0
      - gin-config==0.5.0
      - google-api-core==2.11.1
      - google-api-python-client==2.95.0
      - google-auth==2.22.0
      - google-auth-httplib2==0.1.0
      - google-auth-oauthlib==0.4.6
      - google-cloud-bigquery==3.11.4
      - google-cloud-core==2.3.3
      - google-crc32c==1.5.0
      - google-pasta==0.2.0
      - google-resumable-media==2.5.0
      - googleapis-common-protos==1.59.1
      - grpcio==1.56.2
      - grpcio-status==1.48.2
      - h5py==3.9.0
      - httplib2==0.22.0
      - idna==3.4
      - importlib-metadata==6.8.0
      - importlib-resources==6.0.0
      - ipykernel==6.25.0
      - ipython==8.14.0
      - isoduration==20.11.0
      - jax==0.4.13
      - jedi==0.18.2
      - jinja2==3.1.2
      - joblib==1.3.1
      - json5==0.9.14
      - jsonpointer==2.4
      - jsonschema==4.18.4
      - jsonschema-specifications==2023.7.1
      - jupyter-client==8.3.0
      - jupyter-core==5.3.1
      - jupyter-events==0.6.3
      - jupyter-lsp==2.2.0
      - jupyter-server==2.7.0
      - jupyter-server-terminals==0.4.4
      - jupyterlab==4.0.3
      - jupyterlab-pygments==0.2.2
      - jupyterlab-server==2.24.0
      - kaggle==1.5.16
      - keras==2.8.0
      - keras-preprocessing==1.1.2
      - kiwisolver==1.4.4
      - libclang==16.0.6
      - librosa==0.8.1
      - llvmlite==0.36.0
      - lxml==4.9.3
      - markdown==3.4.4
      - markupsafe==2.1.3
      - matplotlib==3.4.3
      - matplotlib-inline==0.1.6
      - mistune==3.0.1
      - ml-dtypes==0.2.0
      - nbclient==0.8.0
      - nbconvert==7.7.3
      - nbformat==5.9.1
      - nest-asyncio==1.5.6
      - neural-structured-learning==1.4.0
      - notebook==7.0.0
      - notebook-shim==0.2.3
      - numba==0.53.0
      - numpy==1.23.4
      - nvidia-cublas-cu11==2022.4.8
      - nvidia-cublas-cu117==11.10.1.25
      - nvidia-cudnn-cu11==8.6.0.163
      - oauthlib==3.2.2
      - opencv-python-headless==4.8.0.74
      - opt-einsum==3.3.0
      - overrides==7.3.1
      - packaging==20.9
      - pandas==2.0.3
      - pandocfilters==1.5.0
      - parso==0.8.3
      - pexpect==4.8.0
      - pickleshare==0.7.5
      - pillow==10.0.0
      - platformdirs==3.9.1
      - pooch==1.7.0
      - prometheus-client==0.17.1
      - promise==2.3
      - prompt-toolkit==3.0.39
      - proto-plus==1.22.3
      - protobuf==3.19.6
      - psutil==5.9.5
      - ptyprocess==0.7.0
      - pure-eval==0.2.2
      - py-cpuinfo==9.0.0
      - pyasn1==0.5.0
      - pyasn1-modules==0.3.0
      - pybind11==2.11.1
      - pycocotools==2.0.6
      - pycoral==2.0.0
      - pycparser==2.21
      - pygments==2.15.1
      - pyparsing==3.1.0
      - python-dateutil==2.8.2
      - python-json-logger==2.0.7
      - python-slugify==8.0.1
      - pytz==2023.3
      - pyyaml==6.0.1
      - pyzmq==25.1.0
      - referencing==0.30.0
      - requests==2.31.0
      - requests-oauthlib==1.3.1
      - resampy==0.4.2
      - rfc3339-validator==0.1.4
      - rfc3986-validator==0.1.1
      - rpds-py==0.9.2
      - rsa==4.9
      - scann==1.2.6
      - scikit-learn==1.3.0
      - scipy==1.11.1
      - send2trash==1.8.2
      - sentencepiece==0.1.99
      - six==1.16.0
      - sniffio==1.3.0
      - sounddevice==0.4.6
      - soundfile==0.12.1
      - soupsieve==2.4.1
      - stack-data==0.6.2
      - tensorboard==2.8.0
      - tensorboard-data-server==0.6.1
      - tensorboard-plugin-wit==1.8.1
      - tensorflow==2.8.4
      - tensorflow-addons==0.21.0
      - tensorflow-datasets==4.9.0
      - tensorflow-estimator==2.8.0
      - tensorflow-hub==0.12.0
      - tensorflow-io-gcs-filesystem==0.32.0
      - tensorflow-metadata==1.13.0
      - tensorflow-model-optimization==0.7.5
      - tensorflowjs==3.18.0
      - termcolor==2.3.0
      - terminado==0.17.1
      - text-unidecode==1.3
      - tf-models-official==2.3.0
      - tf-slim==1.1.0
      - tflite-model-maker==0.4.2
      - tflite-runtime==2.5.0.post1
      - tflite-support==0.4.4
      - threadpoolctl==3.2.0
      - tinycss2==1.2.1
      - toml==0.10.2
      - tomli==2.0.1
      - tornado==6.3.2
      - tqdm==4.65.0
      - traitlets==5.9.0
      - typeguard==2.13.3
      - typing-extensions==4.5.0
      - tzdata==2023.3
      - uri-template==1.3.0
      - uritemplate==4.1.1
      - urllib3==1.25.11
      - wcwidth==0.2.6
      - webcolors==1.13
      - webencodings==0.5.1
      - websocket-client==1.6.1
      - werkzeug==2.3.6
      - wrapt==1.14.1
      - zipp==3.16.2
prefix: /home/jin/anaconda3/envs/coral
```

This is the environment setting that did not cause the numpy or tensorflow library dependency error&#x20;

### Create Jupyter Notebook&#x20;

```
conda install jupyter
jupyter notebook 
```

### Create a new file and paste the following code:&#x20;

```
import numpy as np
import os
import random
import shutil


from tflite_model_maker.config import ExportFormat
from tflite_model_maker import model_spec
from tflite_model_maker import object_detector

import tensorflow as tf
assert tf.__version__.startswith('2')

print(f"TensorFlow version: {tf.__version__}")
print(f"NumPy version: {np.__version__}")

tf.get_logger().setLevel('ERROR')
from absl import logging
logging.set_verbosity(logging.ERROR)

# Images folder input 
images_in = 'Dataset_apple_hand/images'
# XML folder input 
annotations_in = 'Dataset_apple_hand/annotations'


label_map = {1: 'hand', 2: 'apple'}
```

<pre><code>UserWarning: Tensorflow Addons supports using Python ops for all Tensorflow versions above or equal to 2.11.0 and strictly below 2.14.0 (nightly versions are not supported). 
<strong>
</strong><strong>TensorFlow version: 2.8.4
</strong>NumPy version: 1.23.4
</code></pre>

Modify the image and annotation path based on where your dataset is.&#x20;

Although it gave me an outdated tensorflow version warning, it worked well&#x20;

### Paste code for dataset generation

```
def split_dataset(images_path, annotations_path, val_split, test_split, out_path):
    # Remove preexisting folder
    current_dir = os.getcwd() 
    folder_name = os.path.join(current_dir, 'dataset')
    if os.path.exists(folder_name) and os.path.isdir(folder_name):
        shutil.rmtree(folder_name)
        print(f"Prexisting Folder '{folder_name}' deleted.")
    else:
        print(f"Folder '{folder_name}' does not exist. Create new dataset folder")

    
    """Splits a directory of sorted images/annotations into training, validation, and test sets.

    Args:
        images_path: Path to the directory with your images (JPGs).
        annotations_path: Path to a directory with your VOC XML annotation files,
            with filenames corresponding to image filenames. This may be the same path
            used for images_path.
        val_split: Fraction of data to reserve for validation (float between 0 and 1).
        test_split: Fraction of data to reserve for test (float between 0 and 1).
    Returns:
        The paths for the split images/annotations (train_dir, val_dir, test_dir)
    """
    _, dirs, _ = next(os.walk(images_path))

    train_dir = os.path.join(out_path, 'train')
    val_dir = os.path.join(out_path, 'validation')
    test_dir = os.path.join(out_path, 'test')

    IMAGES_TRAIN_DIR = os.path.join(train_dir, 'images')
    IMAGES_VAL_DIR = os.path.join(val_dir, 'images')
    IMAGES_TEST_DIR = os.path.join(test_dir, 'images')
    os.makedirs(IMAGES_TRAIN_DIR, exist_ok=True)
    os.makedirs(IMAGES_VAL_DIR, exist_ok=True)
    os.makedirs(IMAGES_TEST_DIR, exist_ok=True)

    ANNOT_TRAIN_DIR = os.path.join(train_dir, 'annotations')
    ANNOT_VAL_DIR = os.path.join(val_dir, 'annotations')
    ANNOT_TEST_DIR = os.path.join(test_dir, 'annotations')
    os.makedirs(ANNOT_TRAIN_DIR, exist_ok=True)
    os.makedirs(ANNOT_VAL_DIR, exist_ok=True)
    os.makedirs(ANNOT_TEST_DIR, exist_ok=True)

    print(f"Created directories for train, validation, and test data.")
    print(f"Train directory: {train_dir}")
    print(f"Validation directory: {val_dir}")
    print(f"Test directory: {test_dir}")

    # Get all filenames for this dir, filtered by filetype
    filenames = os.listdir(os.path.join(images_path))
    filenames = [os.path.join(images_path, f) for f in filenames if f.endswith('.jpg')]
    # Shuffle the files, deterministically
    filenames.sort()
    random.seed(42)
    random.shuffle(filenames)
    # Get the exact number of images for validation and test; the rest is for training
    val_count = int(len(filenames) * val_split)
    test_count = int(len(filenames) * test_split)
    for i, file in enumerate(filenames):
        source_dir, filename = os.path.split(file)
        annot_file = os.path.join(annotations_path, filename.replace("jpg", "xml"))
        if i < val_count:
            shutil.copy(file, IMAGES_VAL_DIR)
            shutil.copy(annot_file, ANNOT_VAL_DIR)
            print(f"Copied {filename} and its annotation to {IMAGES_VAL_DIR} and {ANNOT_VAL_DIR}")
        elif i < val_count + test_count:
            shutil.copy(file, IMAGES_TEST_DIR)
            shutil.copy(annot_file, ANNOT_TEST_DIR)
            print(f"Copied {filename} and its annotation to {IMAGES_TEST_DIR} and {ANNOT_TEST_DIR}")
        else:
            shutil.copy(file, IMAGES_TRAIN_DIR)
            shutil.copy(annot_file, ANNOT_TRAIN_DIR)
            print(f"Copied {filename} and its annotation to {IMAGES_TRAIN_DIR} and {ANNOT_TRAIN_DIR}")
    return (train_dir, val_dir, test_dir)
```

Above code is function for generating dataset. The code will generate train, validation, test folder

```
train_dir, val_dir, test_dir = split_dataset(images_in, annotations_in,val_split=0.2, test_split=0.2,out_path='dataset')
train_data = object_detector.DataLoader.from_pascal_voc(os.path.join(train_dir, 'images'),os.path.join(train_dir, 'annotations'), label_map=label_map)
validation_data = object_detector.DataLoader.from_pascal_voc(os.path.join(val_dir, 'images'),os.path.join(val_dir, 'annotations'), label_map=label_map)
test_data = object_detector.DataLoader.from_pascal_voc(os.path.join(test_dir, 'images'),os.path.join(test_dir, 'annotations'), label_map=label_map)

print(f'train count: {len(train_data)}')
print(f'validation count: {len(validation_data)}')
print(f'test count: {len(test_data)}')
```

At the end, it will print&#x20;

```
Copied apple_37.jpg and its annotation to dataset/train/images and dataset/train/annotations
Copied hand_787.jpg and its annotation to dataset/train/images and dataset/train/annotations
train count: 832
validation count: 277
test count: 277
```

### Train model

```
spec = object_detector.EfficientDetLite0Spec()

model = object_detector.create(train_data=train_data,
                               model_spec=spec,
                               validation_data=validation_data,
                               epochs=1000,
                               batch_size=32,
                               train_whole_model=True)
```

<figure><img src="../../.gitbook/assets/Screenshot from 2023-08-26 18-24-07.png" alt=""><figcaption></figcaption></figure>

### Evaluate model

```
model.evaluate(test_data)
```

### Export trained model

Change the filename to whatever you want&#x20;

```
# Now export trained model 

TFLITE_FILENAME = 'efficientdet_coral_v2.tflite' 
LABELS_FILENAME = 'coral_labels_v2.txt'
print("Done!")

model.export(export_dir='.', tflite_filename=TFLITE_FILENAME, label_filename=LABELS_FILENAME,
             export_format=[ExportFormat.TFLITE, ExportFormat.LABEL])
print("Done!")
```

### Convert exported mode using EdgeTPU compiler

Now, your folder should have&#x20;

<figure><img src="../../.gitbook/assets/Screenshot from 2023-08-26 18-27-07.png" alt=""><figcaption></figcaption></figure>

In the same directory, go to the terminal and type&#x20;

```
edgetpu_compiler efficientdet_coral_v2.tflite --num_segments=1
```

It will print&#x20;

```
Edge TPU Compiler version 16.0.384591198
Started a compilation timeout timer of 180 seconds.

Model compiled successfully in 1524 ms.

Input model: efficientdet_coral_v2.tflite
Input size: 4.24MiB
Output model: efficientdet_coral_v2_edgetpu.tflite
Output size: 5.57MiB
On-chip memory used for caching model parameters: 4.21MiB
On-chip memory remaining for caching model parameters: 3.29MiB
Off-chip memory used for streaming uncached model parameters: 0.00B
Number of Edge TPU subgraphs: 1
Total number of operations: 267
Operation log: efficientdet_coral_v2_edgetpu.log

Model successfully compiled but not all operations are supported by the Edge TPU. A percentage of the model will instead run on the CPU, which is slower. If possible, consider updating your model to use only operations supported by the Edge TPU. For details, visit g.co/coral/model-reqs.
Number of operations that will run on Edge TPU: 264
Number of operations that will run on CPU: 3
See the operation log file for individual operation details.
Compilation child process completed within timeout period.
Compilation succeeded! 

```

Now, the folder should have&#x20;

<figure><img src="../../.gitbook/assets/Screenshot from 2023-08-26 18-32-30.png" alt=""><figcaption></figcaption></figure>

The log file shows that most of the operations are supported.

```
Edge TPU Compiler version 16.0.384591198
Input: efficientdet_coral_v2.tflite
Output: efficientdet_coral_v2_edgetpu.tflite

Operator                       Count      Status

ADD                            42         Mapped to Edge TPU
CUSTOM                         1          Operation is working on an unsupported data type
RESHAPE                        10         Mapped to Edge TPU
CONCATENATION                  2          Mapped to Edge TPU
RESIZE_NEAREST_NEIGHBOR        12         Mapped to Edge TPU
DEPTHWISE_CONV_2D              80         Mapped to Edge TPU
DEQUANTIZE                     2          Operation is working on an unsupported data type
CONV_2D                        102        Mapped to Edge TPU
QUANTIZE                       1          Mapped to Edge TPU
LOGISTIC                       1          Mapped to Edge TPU
MAX_POOL_2D                    14         Mapped to Edge TPU
```

### Transfer file&#x20;

It is possible to transfer files through SSH communication from the desktop to the Coral Dev Board, but I used a more conventional way: USB

Copy 3 files to USB:&#x20;

* Your exported model.tflite&#x20;
* Your exported label.txt
* usb\_transfer.sh (You can manually cp everything, so skip if you want)

usb\_transfer.sh scripts are in the previous post&#x20;

{% content-ref url="coral-tpu-connect-usb-camera.md" %}
[coral-tpu-connect-usb-camera.md](coral-tpu-connect-usb-camera.md)
{% endcontent-ref %}

Now, turn on the Google Coral Dev Board and mount the USB. (you need to create /media/usb directory) The USB path might differ

```
sudo mount /dev/sda1 /media/usb 
cd /media/usb 
sudo chmod +x usb_transfer.sh
./usb_transfer.sh
```

Transfer those file to one of the directories that belong to /home

I created new folder called "demo\_v2"

```
mendel@undefined-eft:/media/usb$ ./usb transfer.sh
Files in the USB drive:
run_face_detection_inference.sh run_usb_camera_inference.sh usb_transfer.sh
Enter the destination directory on the Coral Dev Board: /home/jin/demo_v2
Enter a file name to copy (or 0 to exit): efficientDet_coral_v2_edgetpu.tflite
Enter a file name to copy (or 0 to exit): coral_labels_v2.txt
Enter a file name to copy (or 0 to exit): 0
Coping selected files to /home/jin/demo files:
Copied: run_face_detection_inference.sh
Copied: run_usb_camera_inference.sh
Copied: usb_transfer.sh
Selected files copied from USB to /home/jin/demo_v2
```



### Run the code&#x20;

Now, go to the directory where your files and models are moved. In my case, it is /home/jin/demo\_v2

<pre><code>mendel@undefined-eft: cd /home/jin/demo_v2
<strong>mendel@undefined-eft: ls
</strong><strong>efficientDet_coral_v2_edgetpu.tflite
</strong><strong>coral_labels_v2.txt 
</strong></code></pre>

There should be your transferred files. Create a script for running the code.&#x20;

```
vim run_effi_v2.sh
```

```
#!/bin/bash

echo "==== EfficientDet Demo ===="

echo "To exit, press win + tab to show terminal"

edgetpu_detect \
--source /dev/video1:YUY2:640x480:10/1 \
--model /home/jin/demo_v2/efficientDet_coral_v2_edgetpu.tflite \
--labels /home/jin/demo_v2/coral_labels.txt
```

ESC + :wq to save

If it causes E212: no permission to write then you should add permission to the home directory&#x20;

<pre><code><strong>sudo chmod +x run_effi_v2.sh
</strong>./run_effi_v2
</code></pre>

If the streaming stops after a few seconds showing gstream render\_overlay\_gen.send(tensor, layout, command)&#x20;

Check whether the script is running **edgetpu\_classify** instead of **edgetpu\_detect**

<figure><img src="../../.gitbook/assets/IMG_2135 Large.jpeg" alt="" width="375"><figcaption><p>my setting: apple and hand </p></figcaption></figure>

<figure><img src="../../.gitbook/assets/efficientdet_demo.gif" alt=""><figcaption></figcaption></figure>

Do not forget to&#x20;

```
sudo shutdown now 
```

Instead of unplugging
