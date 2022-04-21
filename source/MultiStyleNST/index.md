---
title: 
date: 2020-11-02 11:04:52
---

<div>
	<h1 style="display:inline;"> Multi-Style Semantic Style Transfer </h1> 
	<a href="https://github.com/PKhuang-TW/MultiStyle-Semantic-Style-Transfer"> [Code] </a> 
</div>

---

This project is aimed to transfer different semantic objects in one image into different styles. We use pretrained semantic segmentation model (DeepLab-V3) to get the foregound and backgound region, and apply style transfer on different style for each region. In addition to style loss and content loss in traditional neural style transfer, we futher add style-blending loss and total variance loss to make the result more harmony when blending very different style. Also, we provide custom control of blending effect. 

## Model Architecture
<img src = "./imgs/model_architecture.png" class="projectDetailImg">
Given one image I<sub>src</sub> and two target style images I<sub>s1</sub> and I<sub>s2</sub>, we want to transfer different semantic objects into different style accrodingly. We first produced two intermediated result image I<sub>i1</sub> and I<sub>i2</sub>, and compute the style loss between (I<sub>s1</sub>, I<sub>i1</sub>) and (I<sub>s2</sub>, I<sub>i2</sub>). The we merge these two intermediated images into final result image I<sub>result</sub> by semantic map predicted by DeepLab-V3 image. Finally compute the content loss between (I<sub>src</sub>, I<sub>result</sub>). Further more, to make the resulting image more harmony while blending different styles and prevent weird boundary after merging two intermediated images, we also compute the style-blending loss which is weighted style loss between (I<sub>s1</sub>, I<sub>i2</sub>) and (I<sub>s2</sub>, I<sub>i1</sub>), and smooth loss which is total variance of I<sub>result</sub>. The entire loss function can be written as follow:<br/>
L = &lambda;<sub>1</sub>L<sub>style</sub> + &lambda;<sub>2</sub>L<sub>blend</sub> + &lambda;<sub>3</sub>L<sub>content</sub> + &lambda;<sub>4</sub>L<sub>Smooth</sub>
&nbsp;

## Result 
### Compare with naive approach
<img src = "./imgs/compare.png" class="projectDetailImg">
To validate the effectiveness of additional loss function, we compare the result from removing both blending and smooth loss (left figure), the result from removing smooth loss (mid figure) and the result from using all loss function mentioned above (right figure). From the result we can observe that the naive method will result in weird boundary between different semantic objects. After adding the blending loss, this artifact would improve a lot, and the result of boundary is most clean if all loss function is applied. However, after adding the smooth loss, the resulting image will loss some detail of style (e.g., little spot and stripe). We consider it is an tradeoff between less artifact at boundary and more detail in style, and the effect can be control through adjust the weight of smooth loss.
<p>

### Style Blending Ratio
<img src = "./imgs/blending.png" class="projectDetailImg">
Furthermore, we observe that using naive method sometimes cause the resulting image to become very disharmonious if the two target style is very different (the first figure). After adding the style blending loss, the resulting will become much harmonious (the last figure). However, when the weight of blending loss become too large, it become hard to distinguish different style in different semantic object. So again, we consider it is a tradeoff between making the resulting image more distinguishable to different style and more harmonious.
<p>

### Sample Result
<img src="./imgs/result_1.png" width="40%"> <img src="./imgs/result_2.png" width="40%">
<p>

### Video Style Transfer
<img src = "./imgs/model_video.png" class="projectDetailImg"> 
Traditional neural style transfer use <b>On-line Image Optimization</b> approach, but it could take long time if we want to transfer many image (In our environment, optimize an image take a few minutes). To transfer a video that contain hundred of even thousand of frames, we first use <b>Off-line Model optimization</b> approach to train a transfer model, the for each frame in the video, we can feed it into model and get the resulting image in a few second, whcih make transfer style of video become more practicable. <br/>
<img src="./imgs/video_1.gif" width="40%"> <img src="./imgs/video_2.gif" width="40%"> <br/>

