# Sign Language Interpreter

## Introduction
The goal of this project is to build a system that acts as a translator for Sign Language, specifically American Sign Language (ASL). 

## How it Works
Frames from a video feed taken from a camera would be given as input to a pair of classification models. The camera would be positioned in front of the signer and as he/she is signing, one of the models would attempt to detect letters of the alphabet while the other would attempt to detect words/expressions in ASL. Both the models would be running on a Raspberry Pi and the video feed would be taken from a Pi camera. The exact architecture in which the two models are used is to be decided based on their individual performance and their composite performance.

## Install
```console 
git clone https://github.com/FOSSLife-foundation/Sign-language-interpreter.git
cd Sign-Language-Interpreter
pip install .
```

## Import structure
- ASLInterpreter
  - predict
    - edge_letter
      - showscore()
      - predict()
    - edge_word
      - showscore()
      - predict()
    - predict_mp
      - load_letters_model()
      - load_words_model()
      - predict_letters()
      - predict_words()
  - preprocess
    - edge_highlight
      - preprocess()
    - mp_data_augmentation
      - flip_keypoints()
      - scale_keypoints()
      - move_keypoints()
      - rotate_keypoints()
      - find_center()
      - rotate_point()

## Example
Mediapipe-based classifier example
```python
from ASLInterpreter.predict.predict_mp import *
import cv2

cap = cv2.VideoCapture(0)
cap.set(3, 320)
cap.set(4, 240)

load_letters_model("C:/Sign-language-interpreter/ASLInterpreter/models/model_letters")

while cap.isOpened():
    success, image = cap.read()
    if not success:
        break
    letter, confidence, image = predict_letters_from_image(image)
    if letter:
        print(letter, ":", confidence)
    
    cv2.imshow('Test', image)
    if cv2.waitKey(5) & 0xFF == 27:
        break
```
