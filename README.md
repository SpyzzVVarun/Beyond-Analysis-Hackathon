# Beyond-Analysis-Hackathon
My work for Beyond Analysis, a data science hackathon organized by Techniche, IIT Guwahati.
We chose this Damage Detection dataset as it seemed interesting to work on and we wanted to try out a computer vision problem.

## Problem Statement Discussion

We are given a dataset of images of damaged cars and we need to classify the images in terms of type of damage and extent of damage of the car. We have 800 unique RGB images of dimensions 224 X 224 pixels. The problem statement involves multi label classification where each car can have multiple types of damages. The total number of classes of damage type were 6 which included “unknown” mostly representing the cars having no damage.

## Thought Process

1. This is the dataset we have and as we can see here, we have a multilabel classification problem, a relatively uncommon problem.

2. Usually when it comes to detecting multiple objects in an image, the approach is image segmentation or object detection but in this case it is neither of them since the images were not labeled that way.

3. When we encounter a multi label classification problem, one might think it is similar to a multi class problem and go forward with the approach of using softmax as the activation layer but in this case that would not be useful as softmax function predicts the most probable label and ignores the other labels and this does not allow the selection of multiple labels.

4. Thus, we used sigmoid as our activation function because it would allow us to find probabilities of each label and pick multiple labels for an image based on some threshold. We took outputs from a linear layer with number of nodes = number of labels and applied sigmoid activation to each node to get the outputs. Then we calculate loss by Binary Cross Entropy loss function.

5. However, for this particular problem, we were not able to set a proper threshold for the output function to identify multi labels as in most of the cases the model will predict high probabilities for multiple labels in selected scenarios and lower probabilities for some labels in some different scenarios. For eg : In case of head lamp damage, the output value of sigmoid activation for tail lamp damage is also high, because the model cannot easily distinguish between head and tail lamp, and in some other cases the sigmoid values were on the lesser side and were less than the threshold we set.
, thus making it impossible to set a common threshold for all labels.


6. Having less training data, we thought of using transfer learning and as we have previously worked with vgg19 we started with it. We also used different models like vision transformer, vgg16, resnet50, but vgg19 showed the highest validation score.
We tried unfreezing some of the layers and unfreezing the last 2 worked best. It makes sense since we have such less training data and the initial layers, trained on imagenet data are well set for general feature extraction.

7. After all this, we used data augmentation and looked for external data to increase our dataset as 800 images were not enough for our model to work well. Computer vision models require a large collection of training data in order to effectively learn and prevent Overfitting, whereas the collection of such training data is expensive and laborious. Data augmentation overcomes this issue by artificially inflating the training set with label-preserving transformations and makes the model robust.
 
8. Talking about further improvements, we thought about the process of object detection using YOLO or detectron. These models would detect areas of damage in each vehicle. This detected region would then go through a CNN trained on damaged dataset to classify the type of damage. But due to lack of time, we were unable to implement it.

9. Another way to implement multi label classification was to use one versus rest classification (eg: head_lamp broken vs not broken)  but this was something for which we didn't have the time to fully try and implement as we would have had to look at how we chose our samples for each binary classification problem.


