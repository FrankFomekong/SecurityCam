# Real-time Facial Recognition using YOLO and FaceNet

## Description
We implemented a small real-time facial recognition system using a camera to take pictures and render real-time visuals to tell if the people in front of the camera are someone in our database (with their name as labels) or someone unknown. The main algorithms we used are **YOLO v3** (You Only Look Once) and **FaceNet**.


## Available Funtions
* **Face Alignment:** We have two versions of algorithms to detect and crop the faces in a picture — MTCNN and YOLO v3.
* **Training on FaceNet:** You can either train your model from scratch or use a pre-trained model for transfer learning. The loss function we use is triplet-loss.
* **Real-time Facial Recognition:** We use opencv to render a real-time video after facial recognition and labeling.

## Configuration
* OS: Windows 10
* GPU: NVIDIA GeForce GTX 1060
* CUDA TOOLKIT: v10.0
* cuDNN SDK: v7.5 (corresponding to CUDA TOOLKIT v10.0)
* Python: 3.x
* tensorflow-gpu: 1.13.1

We have successfully built this system in windows, but we are not sure if it will work under other operating system like Linux or Mac OS. However, all the repos we refer to can work under Linux, so we think Linux is also available.

## Usage

We only provide Windows version here, you can change the command for Linux. Remenber to check the dependencies in requirement.txt.

1. **Face Alignment.**

     You can use either ```align_dataset_mtcnn.py``` or ```align_dataset_yolo_gpu.py```.
     
     First, use ```get_models.sh``` in \align\yolo_weights\ to get the pre-trained model of YOLO if you want to use YOLO version. (The bash file only work under Linux, I will provide link for downloading directly later.)
     
     Then create a folder in \align and name it as "unaligned_faces", put all your images in this folder. In \align\unaligned_faces, one person has one folder with his/her name as folder name and all his/her images should be put in the corresponding folder. 
     
     Finally run
     ```bash
     $ python align_dataset_mtcnn.py
     ```
     or
     ```bash
     $ python align_dataset_yolo_gpu.py
     ```
     
     The result will be generated in \aligned_faces folder, copy all the results to /output folder for later use.
     
2. **Training FaceNet Model**

     * If you want to implement a tranfer learning with a pre-trained model and your own dataset, you need to first download this pre-trained [model](https://drive.google.com/file/d/1BQCoNWpRq_h2IAR2b8KMig-sulzatGms/view?usp=sharing), put it in /models and unzip it. Make sure that the directory /models/20170512-110547 has 4 files.
       
       Then run
       ```bash
       $ python train_tripletloss.py
       ```
     
       The trained model will be in the /models/facenet.
     
     * If you want to train your own model from scratch. In ```train_tripletloss.py``` line 433, there is an optional argument named "--pretrained_model", delete its default value.
     
       Then run again 
       ```bash
       $ python train_tripletloss.py
       ```
     
       The trained model will also be in the /models/facenet.

     
3. **Real-time Facial Recognition**

     There are two versions — MTCNN ```realtime_facenet.py``` and YOLO ```realtime_facenet_yolo_gpu.py```, you can also use ```realtime_facenet_yolo.py```, but the fps of this one is pretty low.
     
     First Modify the "modeldir" variable to your own path the same as step 3.
     
     Then run
     ```bash
       $ python realtime_facenet.py
     ```
     
     or
     
     ```bash
       $ python realtime_facenet_yolo_gpu.py
     ```
     

## References

* davidsandberg https://github.com/davidsandberg/facenet

  Provided FaceNet code for training and embedding generation


* sthanhng https://github.com/sthanhng/yoloface

  Provided a YOLO model trained on WIDER FACE for real-time facial detection

*https://github.com/AzureWoods/faceRecognition-yolo-facenet
* cryer https://github.com/cryer/face_recognition

  Provided a framework for moving images from webcam to model, model to real-time on-screen bounding boxes and names
