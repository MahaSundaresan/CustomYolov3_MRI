# CustomYolov3_MRI

Automated MRI prescription of livers:

1.	Problem Statement – 
Locate and draw a bounding box over the region having liver in MRI images using YOLO v3. 
Perform customized object detection using the YOLOv3 algorithm.
Since, the MRI images are Protected Health Information, I have not uploaded MRI images. Similar approach has been adopted to perform object detection for man and cycle with two custom classes. The image data and files related to this dataset are included in this repository.

2.	Data Collection- 
The liver images were shared by the Radiology department at UW Madison.
The image dataset with man and cycle were shared as a part of assignment in CS766 Computer Vision.
Augmentation of the available image dataset by performing rotation by 45 degrees, 90 degrees, 180 degrees and addition of gaussian noise is done to get at least 100 images to begin the training. 

Refer the data_augmentation.ipynb 
Download the Walking person folder and run the notebook.
Else use your image dataset folder and change the folder path in the notebook (Line 42 and 73) before execution. You can generate as many images as you want by changing the number at Line 44. (Set as 25 at present).

3.	Data Preparation- 

Step 1: The data should be labeled in the Darknet format. I used LabelImg (https://github.com/tzutalin/labelImg) to annotate the data. In the Darknet format, for each image file (.jpg) there will be a corresponding label file(.txt). This also means that there is no label file for an image with no object in it. The label file has the following specifications:
1.	Each row corresponds to one object.
2.	The first number corresponds to the class. The class numbers are zero-indexed (starts from 0). That is, 0 corresponds to man and 1 corresponds to cycle.
3.	The following 4 numbers are the normalized box coordinates ranging between 0 and 1. It indicates x_center, y_center, width and height. (If the boxes are in pixels, then x_center and width are divided by image width and y_center and height are divided by image height).
 
Step 2: Creating train.txt and test.txt files. Each row in these files contain the path to the image. 
  
Step 3: Creating *.names file to list the class names for our custom data. 

Step 4:  Creating *.data file with class count (2 corresponding to man and cycle),paths to train and test datasets (we use the same images twice here, but in practice you'll want to validate your results on a separate set of images), and the path to your *.names file.
 
Step 5: Updating yolov3.cfg.
By default each YOLO layer has 255 outputs: 85 values per anchor [4 box coordinates + 1 object confidence + 80 class confidences corresponding to the COCO dataset], times 3 anchors. 
Update the settings to filters= [5 + n] * 3 and classes=n, where n is your class count. 
That is, 21 for custom dataset with 2 classes and 18 for custom dataset with 1 class. Change this in all three YOLO layers of the .cfg file.
Optional: Update hyperparameter values in the .cfg for better results.

4.	Model Training and Inference:

Step 1: Create a folder “Dark” in your google drive.

       1.	Create the following folders –
          backup (the weights generated during training will get updated here),
          cuDNN and 
          bin.
        2.	Download the following folders from this repository and place it in your local “Dark”: 
            img 
            obj.data
            obj.names
            obj.txt
            test.txt
            train.txt
            yolov3.cfg
Step 2: Download https://pjreddie.com/media/files/darknet53.conv.74
 to start training the model initially. It is initial YOLO weights for training custom data.
Place this file  in “Dark”.

Step 3: Now open the google Colab notebook and execute the code to begin training.
https://colab.research.google.com/drive/1KQez3USRG77B17UQtijZCPTqX_9sZOCW?usp=sharing

5.	Results:
We see the predicted image, mAP calculation after every 1000 iterations and a chart indicating average loss, mAP and iterations. 

Note:
The img folder contains only 25 images and their corresponding labels due to size restrictions. Its a toy dataset and training can be performed with this too.

References:
[1] https://github.com/ultralytics/yolov3/wiki/Train-Custom-Data

[2] https://github.com/kriyeng/yolo-on-colab-notebook/blob/master/Tutorial_DarknetToColab.ipynb

[3] https://pjreddie.com/darknet/yolo/

[4] https://github.com/AlexeyAB/darknet/

[5] https://github.com/ivangrov/YOLOv3-GoogleColab/blob/master/YOLOv3_GoogleColab.ipynb
