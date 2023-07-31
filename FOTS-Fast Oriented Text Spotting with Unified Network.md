# [FOTS: Fast Oriented Text Spotting with a Uniﬁed Network](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9141296&casa_token=OL909AFJ3IMAAAAA:IwSEm1ZWHYuyJYBbufI5NOjCfj1qcjPpNxON-wEp-dFsB9AUQVtXiNOefN0jmK1vOn1nEc4UcVQ&tag=1)
The authors introduce a end to end recognition system using a method called RoIrotate.



### Proposed Method : 
![](https://hackmd.io/_uploads/SJZqbyro2.png)
- The shared convolution block which uses ResNet50 backbone is used to extract features, the low-level feature maps are concatenated with high-level semantic feature maps.
![](https://hackmd.io/_uploads/HyCdzkBo3.png)
- The features are used by text detection branch which predicts dense per pixel text.
- The features are converted by RoIRotate to fixed height representation with original aspect ratio.
- The text recognition branch uses CNN and LSTM as encoder and CTC as the decoder

**Text Detection:**
- A FCN is used as text detector which takes in input of feature map as 1/4 of image size.
- Pixels in shrunk version of original text regions are considered as positive samples, it has 5 channels, first channel determines if is positive and the other 4 give its distance from top,bottom,left and right side of bounding box.
- The final output is obtained after thresholding and NMS
- The online hard example mining is used, it uses 2 loss functions; classification and regression.
![](https://hackmd.io/_uploads/ryRLSkSj3.png)
![](https://hackmd.io/_uploads/B1J_Skrsn.png)
![](https://hackmd.io/_uploads/S1suHkSjn.png)

**RoI Rotate:**
- RoI rotate uses bilinear interpolations to produce outputs, it prevents misalignment between RoI and extracted features and makes the output feature length variable.
- The affine transformation used is given by:
![](https://hackmd.io/_uploads/HJQ9I1Sih.png)
![](https://hackmd.io/_uploads/HkxT8ySin.png)
- Ground truth is used for training the text recogntion model and the predicted text region is used while testing.

**Text Recognition:**
- The text recognition branch consists of VGG-like sequential convolutions, poolings with reduction along height axis only, one bi-directional LSTM,one fully-connection and the ﬁnal CTC decoder.
- The spatial features are fed into convolutions and pooling along height axis with dimension reduction to extract high level features, these features are permuted to time major form and fed into a RNN for encoding.
- Here a bidirectional LSTM is used and the hidden states are summed and fed into fully connection to obtain a distribution at each state over character class S.
- Finally, CTC is used to transform frame-wise classiﬁcation scores to label sequence. 
- It uses same detection loss but a different recognition loss given by:
![](https://hackmd.io/_uploads/H1op_krs2.png)
![](https://hackmd.io/_uploads/r14JY1ri3.png)

### Experiments : 
- The datasets used are ICDAR 2013,ICDAR 2015 and ICDAR 2017 MLT.
- The models are evaluated in three setting:
    - Strong:100 words per image that appear in the image
    - Weak:Includes all words that appear in the test set
    - Generic: Uses a 90K lexicon vocabulary
- The model achieves SOTA performance on each setting