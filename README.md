English| [简体中文](./README_cn.md)

# Feature Introduction

This Node is a 2D garbage object detection algorithm developed based on hobot_dnn, using the open-source framework [PaddlePaddle](https://github.com/PaddlePaddle/PaddleDetection.git) and the [PPYOLO](https://github.com/PaddlePaddle/PaddleDetection/tree/release/2.5) model for garbage detection task design and training. In order to achieve rapid deployment, this Node supports the replacement of garbage detection models in configuration files, allowing developers to focus more on the iteration of algorithm model capabilities, reduce deployment workload, and the AI information output can be not only published through topics but also rendered and displayed on web pages.



# Material List
| Material Option    | List      |
| ------- | ------------ |
| RDK X3  | [Purchase Link](https://developer.horizon.ai/sunrise) |
| Camera | [MIPI Camera](https://github.com/HorizonRDK/hobot_mipi_cam)、[USB Camera](https://github.com/HorizonRDK/hobot_usb_cam) |



# Instructions for Use

## Dynamic Recognition with MIPI Camera

```bash
# Set up ROS2 environment
source /opt/tros/setup.bash

# Copy the configuration files needed for the running example from the tros installation path.
cp -r /opt/tros/${TROS_DISTRO}/lib/mono2d_trash_detection/config/ .

# Configure MIPI camera
export CAM_TYPE=mipi

# Launch the launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_config_file:=config/ppyoloworkconfig.json dnn_example_msg_pub_topic_name:=ai_msg_mono2d_trash_detection dnn_example_image_width:=1920 dnn_example_image_height:=1080
```



## Dynamic Recognition with USB Camera

```bash
# Set up ROS2 environment
source /opt/tros/setup.bash

# Copy the configuration files needed for the running example from the tros installation path.
cp -r /opt/tros/${TROS_DISTRO}/lib/mono2d_trash_detection/config/ .

# Configure USB camera
export CAM_TYPE=usb

# Launch the launch file
ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_config_file:=config/ppyoloworkconfig.json dnn_example_msg_pub_topic_name:=ai_msg_mono2d_trash_detection dnn_example_image_width:=1920 dnn_example_image_height:=1080
``````



## Visualization

On PC, open a browser (chrome/firefox/edge) and enter [http://IP:8000](http://ip:8000/) (where IP is the Horizon RDK IP address). Click on the Web display on the top left to see the real-time camera output:

![realtime](image/realtime.gif)



# Interface Description

## Topics
| Topic Name | Message Type        | Description              |
| ---------- | ------------------- | ------------------------ |
|            | sensors/msg/Image   | Subscribe to input image topic |
|            |                     | Publish target recognition results |

The garbage target detection results supported by this Node include information such as length, width, and category. It publishes an AI Msg containing semantic segmentation and target detection information for users to subscribe and use for application development. The complete AI Msg description is as follows:

````
# Detection Message
Roi[] rois
Data structure:
std::string type
int rect.x_offset
int rect.y_offset
int rect.width
int rect.height

# Detection type name, e.g. trash
# trash
````



## Parameters

| Parameter Name     | Explanation                             | Mandatory           | Default Value       | Note                                                                  |
| ------------------ | --------------------------------------- | -------------------- | ------------------- | --------------------------------------------------------------------- |
| feed_type          | Image source, 0: local; 1: subscribed   | No                   | 0                   |                                                                       |
| image              | Local image address                     | No                   | config/test.jpg     |                                                                       |
| image_type         | Image format, 0: bgr; 1: nv12           | No                   | 0                   |                                                                       |
| image_width        | Width of the local nv12 format image    | Must be set for nv12 format images | 0                   |                                                                       |
| image_height       | Height of the local nv12 format image   | Must be set for nv12 format images | 0                   |                                                                       |
| is_shared_mem_sub  | Subscribe to image using shared mem communication | No         | 0                   |                                                                       |
| config_file        | Configuration file path                | No                   | ""                  | Change configuration file to call different models with different post-processing algorithms. By default, it enables the fastercnn model post-processing. |
| dump_render_img    | Render image or not, 0: no; 1: yes      | No                   | 0                   |                                                                       |
``` | msg_pub_topic_name | Topic name for publishing intelligent results, used for web display | No                   | hobot_dnn_detection |

## Configuration File

The configuration file for this Node is ppyoloworkconfig.json, with specific configurations as follows:

```json
{
  "model_file": Path to the model file,
  "dnn_Parser": Set the built-in post-processing algorithm, using the same parsing method as yolov3 as an example, select "yolov3",
  "model_output_count": Number of model output branches,
  "class_num": Number of detection classes,
  "cls_names_list": Specific labels of detection classes,
  "strides": Stride for each output branch,
  "anchors_table": Preset anchor scales,
  "score_threshold": Confidence threshold,
  "nms_threshold": NMS post-processing IOU threshold,
  "nms_top_k": Number of boxes selected in NMS post-processing
}
```

Note: The actual size of each preset anchor is anchors_table x strides

# References

- Model Training: [PPYOLO Garbage Detection + Horizon RDK Deployment (Part 1)](https://aistudio.baidu.com/aistudio/projectdetail/4606468?contributionType=1)
- Model Conversion: [PPYOLO Garbage Detection + Horizon RDK Deployment (Part 2)](https://aistudio.baidu.com/aistudio/projectdetail/4754526?contributionType=1)