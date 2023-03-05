# You Only Look Once (YOLO): Pytorch Implementation

The project involved the implementation of the object detection algorithm Yolo defined in the paper: [YOLO](https://arxiv.org/pdf/1506.02640.pdf). YOLO poses the object detection problem as regression problem and uses a single neural network to predict bounding boxes and class probabilities directly from full images in one evaluation.

## Network Architecture
<img src="./static/images/Network architecture.png" align = "center">


## Results


<table>
  <tr>
      <td align = "center"> <img src="./static/images/1. Bounding box before elimination.png"> </td>
      <td align = "center"> <img src="./static/images/2. Bounding box after suppressing low confidence.png"> </td>
      <td align = "center"> <img src="./static/images/3. Bounding box after non-max suppression.png"> </td>
  </tr>
  <tr>
      <td align = "center"> Predictions from the network </td>
      <td align = "center"> Suppressing low confidence boxes </td>
      <td align = "center"> Boxes after non-max suppression </td>
  </tr>
</table>

<table>
  <tr>
      <td align = "center"> <img src="./static/images/6. Confidence error.png"> </td>
      <td align = "center"> <img src="./static/images/7. Localisation and classification error.png"> </td>
  </tr>
  <tr>
      <td align = "center"> Confidence error </td>
      <td align = "center"> Localisation and Classification error </td>
  </tr>
</table>

<table>
  <tr>
      <td align = "center"> <img src="./static/images/4. Precision recall curve.png"> </td>
      <td align = "center"> <img src="./static/images/5. MAP over training epochs.png"> </td>
  </tr>
  <tr>
      <td align = "center"> Precision-Recall curves over the three classes</td>
      <td align = "center"> Mean Average Precision over training epochs</td>
  </tr>
</table>

## Issues faced
One major issue I faced while implementing this project was to get the model to detect traffic lights.The dataset is imbalanced with 37000 instances of cars, 16000 instances of pedestrians and only 2800 instances of traffic lights. On top of that, the bounding boxes for traffic lights are quite small in dataset.

Thus, in the initial stages it is hard for the predicted boxes to have high IoU with the ground truth boxes. The confidence loss aims to reduce the gap between the predicted confidence and IoU.

Ideally, this should be fixed by the localization optimization as the network trains on. But in this case, the dataset for traffic lights is quite small so the network localization optimization may not be getting enough training instances to correct the localization to an extent where
the IoU is greater than the threshold. 

The failure of network to detect traffic lights is also shown by the average precision values of the 3 classes:
1. Pedestrian: 0.289
2. Traffic light: 0.034
3. Car: 0.533

The network works much better when the rather than using IoU, a target of 1 is used in confidence loss. The precision values for this case are:
1. Pedestrian: 0.389
2. Traffic light: 0.438
3. Car: 0.558
