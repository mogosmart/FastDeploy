# Quantization Config File on FastDeploy

The FastDeploy quantization configuration file contains global configuration, quantization distillation training configuration, post-training quantization configuration and training configuration.
In addition to using the configuration files provided by FastDeploy directly in this directory, users can modify the relevant configuration files according to their needs

## Demo

```
# Global config
Global:
  model_dir: ./yolov5s.onnx                   #Path to input model
  format: 'onnx'                              #Input model format, please select 'paddle' for paddle model
  model_filename: model.pdmodel               #Quantized model name in Paddle format
  params_filename: model.pdiparams            #Parameter name for quantized model name in Paddle format
  image_path: ./COCO_val_320                  #Data set paths for post-training quantization or quantized distillation
  arch: YOLOv5                                #Model Architecture
  input_list: ['x2paddle_images']             #Input name of the model to be quantified
  preprocess: yolo_image_preprocess           #The preprocessing functions for the data when quantizing the model. Developers can modify or write a new one in . /fdquant/dataset.py

#uantization distillation training configuration
Distillation:
  alpha: 1.0                                  # Distillation loss weight
  loss: soft_label                            #Distillation loss algorithm

Quantization:
  onnx_format: true                           #Whether to use ONNX quantization standard format or not, must be true to deploy on FastDeploye
  use_pact: true                              #Whether to use the PACT method for training
  activation_quantize_type: 'moving_average_abs_max'     #Activate quantization methods
  quantize_op_types:                          #OPs that need to be quantized
  - conv2d
  - depthwise_conv2d

#Post-Training Quantization
PTQ:
  calibration_method: 'avg'                   #Activate calibration algorithm of post-training quantization , Options: avg, abs_max, hist, KL, mse, emd
  skip_tensor_list: None                      #Developers can skip some conv layers‘ quantization

#Traning
TrainConfig:
  train_iter: 3000
  learning_rate: 0.00001
  optimizer_builder:
    optimizer:
      type: SGD
    weight_decay: 4.0e-05
  target_metric: 0.365
```

## More details

FastDeploy one-click quantization tool is powered by PaddeSlim, please refer to [Automated Compression of Hyperparameter Tutorial](https://github.com/PaddlePaddle/PaddleSlim/blob/develop/example/auto_compression/hyperparameter_tutorial.md) for more details.
