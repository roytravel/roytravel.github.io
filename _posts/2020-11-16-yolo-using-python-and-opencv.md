---
date: 2020-11-10
layout: post
title: Python과 Opencv를 활용한 YOLO 구현
subtitle: What is YOLO and how to implement?
description:
image: /assets/img/computer_vision.jpg
optimized_image:
  
category: AI
tags:
  - computer vision
  - object detection
  - yolo
  - opencv

  
author: Roytravel
paginate: true
use_math: true
---

이전 포스팅 : <a href="https://roytravel.github.io/insertion-sort/">Insetion Sort</a><br>
본 포스팅은 주로 <a href="https://deep-eye.tistory.com/6">[2]</a>를 참고하여 작성되었습니다.


### About YOLO
YOLO(You Only Look Once)는 컴퓨터 비전 분야의 하위 카테고리인 객체인식의 대표적인 모델로서 2D 객체를 탐지하는 모델이다. YOLO 모델의 자세한 내용은 <a href="https://pjreddie.com/">**https://pjreddie.com/**</a>에서 확인할 수 있다. 객체인식은 이미지나 동영상으로부터 추출되어 저장되는 정보를 인식하는 것으로 예를 들어 다음과 같다.

![alt text](/assets/img/object_detection_logo1.jpg)

YOLO를 포함하여 여러 객체인식 모델이 있는데 그 history는 다음과 같다.
![alt text](/assets/img/object_detection_history.png)

빨간색으로 표시된 모델의 경우 특히 주목되었던 모델들로 그 중에 YOLO가 포함되며 주로 CVPR, NIPS, ICCV, ICLR 등의 컴퓨터 비전과 관련하여 학회에서 주로 나오는 것을 확인할 수 있다.

단계적으로 YOLO 모델을 이해하기 위해 일반적으로 사용되는 Darknet 프레임워크가 아닌 OpenCV 내장 함수를 이용하여 별도의 복잡한 환경 구성을 필요로 하지 않고 간단히 실행해볼 수 있도록 하였다.

### 구현 절차
1. 가상환경 생성<br>
conda create -n yolo python=3.6

2. OpenCV 설치<br>
conda install opencv=3.4

3. YOLO 파라미터 & class 리스트 다운로드<br>
다운로드 받아야할 파일은 총 3개로 weight 파일, cfg 파일, names 파일이다. <a href="https://pjreddie.com/darknet/yolo/">link</a>에 들어가면 아래의 그림을 확인할 수 있는데, 아래 그림과 같이 weight 파일과 cfg 파일을 파이썬을 실행시키고자 하는 경로와 같은 위치에 다운로드 받으면 된다.
![alt text](/assets/img/cfg_weights.png)

이후 <a href="https://github.com/pjreddie/darknet/tree/master/data">link</a>에서  coco.names 파일은 다운로드 받아서 마찬가지로 파이썬을 실행시키고자하는 경로와 같은 위치에 저장한다. 이후 다음과 같은 코드를 통해 모델을 사용해볼 수 있다.


```python
# -*- coding:utf-8 -*-
import cv2
import numpy as np

def get_shape():
    ## Read the Web Camera
    # ret, frame = VideoSignal.read()
    # h, w, c = frame.shape

    # Read the figure
    image = cv2.imread('cat.jpg')
    height, width, channel = image.shape

    return image, height, width, channel


def create_network():
    
    # load YOLO weight and cfg file
    YOLO_net = cv2.dnn.readNet("yolov3-spp.weights","yolov3-spp.cfg")

    # reconstruct the YOOLO network
    classes = []

    # read the class list of object
    with open("coco.names", "r") as f:
        classes = [line.strip() for line in f.readlines()]
        layer_names = YOLO_net.getLayerNames()
        output_layers = [layer_names[i[0] - 1] for i in YOLO_net.getUnconnectedOutLayers()]

    return YOLO_net, classes, output_layers


def network_io(image):
    # Input image into YOLO
    blob = cv2.dnn.blobFromImage(image, 0.00392, (416, 416), (0, 0, 0), True, crop=False)
    # blob = cv2.dnn.blobFromImage(image, 0.00392, (320, 320), (0, 0, 0), True, crop=False)

    # Set the new input value for the network
    YOLO_net.setInput(blob)

    # Output from YOLO network
    outputs = YOLO_net.forward(output_layers)

    return outputs


if __name__ == '__main__':
    
    # get signal from web camera
    #VideoSignal = cv2.VideoCapture(0)
    
    # create network
    YOLO_net, classes, output_layers = create_network()

    while True:
        
        # get the image, height, width and channel
        image, height, width, channel = get_shape()

        # variables for inserting detected object data
        class_ids = []
        confidences = []
        boxes = []

        # input the image into network and get output
        outputs = network_io(image)
        
        for output in outputs:

            for detection in output:
                scores = detection[5:]
                class_id = np.argmax(scores)
                confidence = scores[class_id]

                if confidence > 0.5:
                    # Object detected
                    center_x = int(detection[0] * width)
                    center_y = int(detection[1] * height)

                    dw = int(detection[2] * width)
                    dh = int(detection[3] * height)

                    # Rectangle coordinate
                    x = int(center_x - dw / 2)
                    y = int(center_y - dh / 2)
                    boxes.append([x, y, dw, dh])
                    confidences.append(float(confidence))
                    class_ids.append(class_id)

        # Non-Maximum Suppression for removing false positive and getting final prediction
        indexes = cv2.dnn.NMSBoxes(boxes, confidences, 0.45, 0.4)

        for idx in range(len(boxes)):
            if idx in indexes:
                x, y, w, h = boxes[idx]
                label = str(classes[class_ids[idx]])
                score = confidences[idx]

                # Input the bounding box and class information into image
                cv2.rectangle(image, (x, y), (x + w, y + h), (0, 0, 255), 5)
                cv2.putText(image, label, (x, y - 20), cv2.FONT_ITALIC, 0.5, (255, 255, 255), 1)

        cv2.imshow("YOLOv3", image)

        if cv2.waitKey(100) > 0:
            break

```
별도의 Web Camera가 구비되지 않아 cv2.imread를 통해 읽은 단일 이미지를 가지고 테스트를 진행하였으며 추후 라즈베리파이의 카메라모듈과 연동하여 실시간 탐지를 진행해볼 예정이다.

### 탐지 결과
![alt text](/assets/img/cat_detection.png)


### Reference
[1] <a href="https://www.ctouniverse.com/2015/machine-learning/?open-article-id=9005984&article-title=introduction-to-object-detection&blog-domain=hackerearth.com&blog-title=hacker-earth-developers-blog">logo</a><br>
[2] <a href="https://deep-eye.tistory.com/6">https://deep-eye.tistory.com/6</a><br>
[3] <a href="https://github.com/hoya012/deep_learning_object_detection">https://github.com/hoya012/deep_learning_object_detection</a><br>