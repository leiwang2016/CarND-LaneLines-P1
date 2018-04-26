# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I applied a gaussian kernel to smooth the image to reduce noise. Third, I used canny edge detector pre-process the image based low and high thresholds. After that, a region of interest is defined to search for lines in the hough space. Finally, the red lines are drawn superimposed on the original picture with opencv addWeighted function.  

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging over small lines detected. The lines detected in the hough domain are classified depending on the sign of the scale factor, with negative being left line and possitive being right line. For each class, the scale and bias of each line is averaged to represent a single final line. Exponential weighted average is implemented here with bias correction. Static variables are defined with the draw_lines function to remember the scale/bias variables between function calls. To get rid of noises, lines with absolute scale value between 0.55 and 0.85 are taken into consideration. Also, lines further from the vehicle are given slightly higher weight when doing exponential weighted average. Opencv line fuction draws final lines from the bottom of the image (img.shape[0]) to middle of the image (0.6\*image.shape[0]).   

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when vehicle takes sharp turns, the exponential weighted average does not respond to the change fast enough, although current program gives somewhat more weight to line pieces appearing further to the vehicle. 

Another shortcoming could be the initial position of the lines. The bias correction for the exponential weighted averages are tuned with some parameters, which may not be optimal for other scenarios. So, at the beginning of the video, the red lines may not appear at the correct locations.  

For noise reduction, scale value checking is used, which may not be reliable when a noise nine is detected crossing the threshold values. 

The ending location of the lines are fixed to be at 0.6\*image.shape[0].


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to initialize the lines at the beginning without using exponentially weighted average. 

Another potential improvement could be to filter out the noises by combining the scale value as well as spatial location of the line.

Also, since left lines and right lines are semi-symmetrical, this criterial can be used to remove noise.

The far end of the lines can be dynamically adjusted instead of always being the middle of the image.