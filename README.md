# EagleView_Assignment
Assignment submitted for EagleView Evaluation

<H1>EagleView ML/DL Assignment Documentation</H1>

<h3>Model name</h3>
	For the solution implementation, a YOLOv5 model has been used.

<h3>Links to dataset and framework</h3>
	The dataset was provided entirely by EagleView. The solution framework used is the YOLOv5 repository by Ultralytics.

<h3>About the model</h3>
	YOLOv5 is the latest version of the You-Only-Look-Once CNN architectures that perform one-stage object detection. The variant used here is the largest variant, YOLOv5-X. The basic YOLO architecture scales down input images into multiple feature maps of different sizes. Targets are derived using the anchor box (prior assumed boxes) sizes that are computed from clustering on the annotated bounding box sizes and object labels. The CNN is trained end to end to produce objectness (whether any object exists or not in that box) scores for each anchor box at each scale size, the offsets by which the anchor box must be deformed to match the ground truth box, and a class label for the box. With YOLOv5, many optimizations are implemented into the basic architecture so as improve speed and accuracy. The YOLOv5-X has 567 trainable layers and can achieve inference speeds of 1.5secs/image on a 4-core CPU.


<h3>Primary Analysis</h3>
	For primary analysis, the given COCO JSON annotations were converted to the text file format required by YOLO models. The class balance of box counts between the two classes, person and car, was checked. It was observed that while 10,800 boxes of person object exists, only 5972. Efforts were made to balance the data.
  
1.	It was observed that there were no samples with only person objects. This implied that person class could not be down-sampled without affecting the number of car objects.
2.	Thus, image samples with the maximum number of person objects, and minimum number of car objects were selected to be excluded from the dataset for modelling. This was done in a probabilistic manner. Refer to the code for more details.


<h3>Assumptions</h3>
	No significant assumptions are made about the data, except for that it is correctly (annotations were not verified). 
	The dataset after balancing was split into the three sets for training, validation (model selection) and testing using a 70:15:15 ratio.
	Due to resource limitations, the model was trained in two parts. First upto 70 epochs, and then the best weights from that run was used to initialize another training for 70 more epochs.

<h3>Inference</h3>
	The trained model was used for inferencing on the validation and test set of images, and the inference outputs were evaluated against the provided ground truth annotations.

<h3>False positives</h3>
	Higher false positive rates were seen with the person class. However, such high rates of FPs in person class could not be corroborated in a visual inspection of detection samples. This can be indicative of inconsistencies in annotations – there might have several (smaller) instances of person class in the images that were not annotated leading to FP.


<h3>Conclusion</h3>
	The given dataset was analysed, and split for training, validation, and testing. A YOLOv5-X model was trained on the dataset and evaluated using the validation and test sets. A precision of about 80% and a recall of about 70% was attainable. As Google Colab was used for the modelling purpose, compute capacity was limited, and further optimization experiments could not be done.

<h3>Recommendations</h3>
	Some measures that can be experimented with to improve model performance is as follows:
  
1.  Reviewing consistency of annotations across training, validation, and test sets
2.	Using higher image input sizes, like 640, in training since the dataset seems to contain objects with smaller bounding box sizes.
3.	Experimenting with several kinds of augmentation options, some which are available in the hyper-parameter YAML file used in the repository. Augmentation using various image transformations can also be included that are implemented in the repository
4.	Hyper-parameter tuning – The repository’s evolve functionality can be used to try to automatically find a better set of hyper-parameters to train the model on this dataset.


