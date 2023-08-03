# [TextBoxes++: A Single-Shot Oriented Scene Text Detector](https://arxiv.org/abs/1801.02765)
They introduce a new model to detect arbitrarily oriented text
and propose a novel score to refine the combination of detection and recognition




### Proposed Method : 
![](https://hackmd.io/_uploads/S1uylRNo3.png)



- The proposed model uses arbitrary oriented quadrilaterals and rotated rectangles which are obtained from regressing on default anchor boxes for detection.
![](https://hackmd.io/_uploads/B1D9-CEs3.png)

- The architecture contains several convolutional and pooling layers followed by textbox layers which are then fed to undergo non-maximum suppression.
- The model uses default boxes of various aspect ratios to capture text efficiently and also uses vertical offset to improve text detection in a vertical manner
![](https://hackmd.io/_uploads/rkPd-ANsn.png)
- Both the representations are optimised using the dault boxes and the loss used combines a confidence score and location loss which are obtained using L1 and softmax.
![](https://hackmd.io/_uploads/B1nXzRVj2.png)
- X is match indication matrix, c is confidence, l is location and g is ground truth.
- To remove false detections due to texture similarity the model changes the ratios of negatives and positives to 3:1 and then 6:1.
- For data augmentation, they propose a new method based on Jacquard called object coverage given by
![](https://hackmd.io/_uploads/r1Vxm04oh.png)
- During testing, they use the NMS method in a 2-step fashion first on minimum horizontal textboxes then on quadrilaterals and rotated rectangles, this is done to save time as first step reduces a lot of false candidates.
- The model is refined using end-to-end recognition by using CRNN as the text recognizer with a given lexicon, the outputs of detection and recognition are combined to a threshold score S used to train the model which is given by:
![](https://hackmd.io/_uploads/S16gE04o3.png)

### Experiments : 
- The datasets used are SYnth-Text, IC13, SVT, IC15 andCOCO-Text.
- The metrics used are precision, recall and f-score(f-score used for end-to-end recognition and word spotting also)
- The model achieves SOTA on all the tasks(word spotting,end-to-end recognition and text localisation)
- The model fails to perform well when there is object occlusion, large spacing and for few instances of vertical text.
