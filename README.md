# **Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)


Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


Environment
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/83ec35ee-1e02-48a5-bdb7-d244bd47c2dc/lessons/8c82408b-a217-4d09-b81d-1bda4c6380ef/concepts/4f1870e0-3849-43e4-b670-12e6f2d4b7a7) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.  

Project: Finding Lane Lines on the Road
---
## Objectives
* Detect lane lines in image
* Detect lane lines in video
* Show the result in the video

<img src="https://user-images.githubusercontent.com/40875720/51319804-081dcd00-1a99-11e9-99a1-50c3532587de.PNG" width="600">

The following is the test image, my object is to find the lane lines in the image.
<img src="https://user-images.githubusercontent.com/40875720/51320842-0e617880-1a9c-11e9-8e5d-c74470c10a27.PNG" width="600">


## Overview
The project contains the following steps:
* Color selection
* ROI & Markout lane with red color
* Gray Scaling
* Gaussion smoothing
* Canny
* Hough transform lane detection
* Draw lanes

##  Step 1: Color selection
The first step is color selection, lane lines' color are write, based on this situation, we can roughly fiter out the lanes according to the R G B value in the color space. The realization are as below:

```
#### First Step:Color selection

#Define color selection criteria
color_select = np.copy(image)
red_threshold = 200
green_threshold = 200
blue_threshold = 200

rgb_threshold = [red_threshold, green_threshold, blue_threshold]

# Do a boolean or with the "|" character to identify
# pixels below the thresholds
thresholds = (image[:,:,0] < rgb_threshold[0]) \
            | (image[:,:,1] < rgb_threshold[1]) \
            | (image[:,:,2] < rgb_threshold[2])
color_select[thresholds] = [0,0,0]

plt.imshow(color_select)
plt.show()
cv2.imwrite('test_images_output/color_selection.png', color_select)
```
After the first step, we can get the result like below. From the result we can tell that the code can fiter out some write part but also include some noise(the write car)

<img src="https://user-images.githubusercontent.com/40875720/51321213-28e82180-1a9d-11e9-9f7d-3b8117b84f18.PNG" width="600">

## Step 2: ROI & Markout lane with red color
The purpose of this step is to reduce the computering resouce needed. We can only foucs on the ROI.

```
#### Second Step: ROI finding and mark out the lane with Red color

vertices = np.array([[(0,539),(440, 330), (520, 330), (939,539)]], dtype=np.int32)
line_image = np.copy(image)
roi_image = region_of_interest(color_select, vertices)
plt.imshow(roi_image)
plt.show()
cv2.imwrite('test_images_output/roi_image.png', roi_image)
```
After this step, we can see that the code fiter out most noise due to the fact that we only foucs on ROI and fill in other parts as 0.

<img src="https://user-images.githubusercontent.com/40875720/51321452-c6dbec00-1a9d-11e9-8f9f-083a648ce5d8.PNG" width="600">

## Step 3: Gray Scaling
The purpose of this step is quite obvious, Canny need to run on gray image.

```
gray = grayscale(roi_image)
plt.imshow(gray, cmap='gray')
plt.show()
cv2.imwrite('test_images_output/gray.png', gray)
```

After this step, we can not see much difference.But the image has been changed to gray in the background.

<img src="https://user-images.githubusercontent.com/40875720/51322072-65b51800-1a9f-11e9-9820-08b26abfe8a4.PNG" width="600">
