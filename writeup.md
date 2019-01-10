# **Finding Lane Lines on the Road** 

## Writeup Of Project One


---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection


### 1. Pipline description

My pipeline contains the following steps.
1. Color selection
2. ROI finding
3. Gray scaling
4. Gaussian smoothing(optional)
5. Canny
6. Hough transform and line detection
7. Draw lanes


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when the lanes are not straight

Another shortcoming could be that I need to tune the parameters manually, it cost much time


### 3. Suggest possible improvements to your pipeline


A possible improvement would be to replace hough with other advanced algorithm

Another potential improvement could be to develop some functions to tune parameters automatically