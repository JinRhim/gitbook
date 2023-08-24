---
description: convert trained tensorflow model
---

# Create Onnx and TensorRT

I have a ssdmobilenet .tflite file that I trained on the RTX3080. Although my initial goal was to run the tensorflow lite on the OpenMV H7, I realized that it is impossible

* model.tflite file size --> 3MB&#x20;
* OpenMV H7 RAM --> 1MB SRAM

It means that the OpenMV H7 will run out of memory in first 3 \~ 4 layers of convolution because of big model size. MIT's Han Song Lab managed to run TinyML to the OpenMV H7 (which me and my lab partner is still struggling to understand) but obviously they are from MIT.&#x20;

<figure><img src="../.gitbook/assets/Screen Shot 2023-07-12 at 2.42.04 PM.png" alt=""><figcaption><p>Exception: Arena size is too small for all buffers.Needed 351408 but only 269040 was available</p></figcaption></figure>

Edge Impulse managed to put a extremely compressed tflite model to the microcontrollers with less than 1MB memory. However, I wanted to run the full .tflite model which I trained.



So I chose Jetson Nano 4GB model to run demo.&#x20;



However, there was a problem&#x20;

* Jetson Nano 4GB is old model so its JetPack Version was stuck at 4.6.4 and ubuntu version of 18.04&#x20;
* Official Jetson Nano starter guide was written based on Jetpack 5.1&#x20;

If you just follow guideline for Jetpack 5.1, it will cause endless "failed build"

I tried to follow old youtube vide tutorials that was based on Jetpack 4.5 after resetting micoSD card, but it still failed\
\
So my only choice was to convert trained tensorflow model to ONNX model or TensorRT model and then run it on the Jetson Nano.



This is Nvidia github link to convert models: [https://github.com/NVIDIA/TensorRT/tree/main/samples/python/tensorflow\_object\_detection\_api#build-tensorrt-engine](https://github.com/NVIDIA/TensorRT/tree/main/samples/python/tensorflow\_object\_detection\_api#build-tensorrt-engine)



In this Nvidia github, copy 4 files:&#x20;

* build\_engine.py
* common.py
* create\_onnx.py
* image\_batcher.py
* onnx\_utils.py

and put them inside the _(your\_model\_path)/workspace/training\_demo/exported-models_

now, set the conda environment. Clone the conda environment that you used to train the tensorflow model and add those:

```
(activate your tensorflow training environment)
conda install prptobuf=3.20.*
conda install -c conda-forge onnx
conda install -c conda-forge onnxruntime
```

You need to downgrade protobuf version (Protobuf version above 4.0 will cause error)

Run below is the code to create onnx file. (dont forget to change the path)

```
python create_onnx.py 
    --pipeline_config /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_tensorrt/pipeline.config 
    --saved_model /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_tensorrt/saved_model 
    --onnx /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_onnx/model.onnx
```

![](<../.gitbook/assets/image (1).png>)![](https://lh6.googleusercontent.com/shS4fcCzlnZckZkNWPBdcYW9dxInXVM82CPb-zOWGicO9eXu8HYEcDPoIGejZJXz3bR-purBpOzuy9jU-dvq7UleMqku7hqqiMXuzNrU22d2JBmnMpqCYiHlSqZnmgqkh75NvMXQJvqR2wUs0pemIc0)

In the my\_model\_onnx folder, there should be model.onnx



Now, we need to convert the onnx model to TensorRT version.&#x20;

```
# this is for Float16 model 
python build_engine.py 
    --onnx /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_onnx/model.onnx 
    --engine /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_tensorrt/engine.trt 
    --precision fp16

# this is for int8 model
python build_engine.py 
--onnx /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_onnx/model.onnx 
--engine /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_tensorrt_int8/engine_int8.trt 
--precision int8 
--calib_input /home/tensorflow_train_folder_path/workspace/training_demo/images/train 
--calib_cache /home/tensorflow_train_folder_path/workspace/training_demo/exported-models/my_model_tensorrt_int8/calibration.cache

```

However, If your CUDA, CUDNN version and TensorRT version does not match, it will give&#x20;

```
[TFT] CUDA ERROR 35 Device not detected… 
```

In my case, TensorRT 8.6.1 was installed which is suppose to support CUDA 11.7 but it kept causing error so I had to downgrade to TensorRT 8.5

<figure><img src="../.gitbook/assets/Screenshot 2023-08-20 at 6.43.58 PM.png" alt=""><figcaption><p>[07/22/2023-21:03:09] [TRT] [I] [MemUsageChange] Init CUBLAS/CUBLASLt: CPU +852,</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-20 at 6.44.17 PM.png" alt=""><figcaption><p>[TRT] [I] [MemUsageChange] TensorRT-managed allocation in building engine: CPU +6, GPU +6, now: CPU 6, GPU 6 (MiB)</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-20 at 6.48.06 PM.png" alt=""><figcaption><p>TensorRT generating Image Cache</p></figcaption></figure>

If it was successful, it should create&#x20;

<figure><img src="../.gitbook/assets/Screenshot 2023-08-20 at 6.50.42 PM.png" alt=""><figcaption><p>tensorrt file</p></figcaption></figure>

<figure><img src="../.gitbook/assets/Screenshot 2023-08-20 at 6.50.49 PM.png" alt=""><figcaption><p>calibration cache</p></figcaption></figure>

