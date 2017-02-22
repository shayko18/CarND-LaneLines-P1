#**Finding Lane Lines on the Road** 

##Writeup  - Project 1

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
	* Make a pipeline that finds lane lines on the road
	* Reflect on your work in a written report

**Commited Files:**
	1) P1.ipynb - script for the project 
	2) P1_extra.ipynb - script for the "extra" project 
	3) P1_writeup.md - writeup report (this file)


[//]: # (Image References)

[image1]: test_images\output_solidYellowCurve2_grey.jpg "Grayscale"
[image2]: test_images\output_solidYellowCurve2_canny.jpg "Grayscale"
[image3]: test_images\output_solidYellowCurve2_hough.jpg 
[image4]: test_images\output_solidYellowCurve2_roi.jpg
[image5]: test_images\output_solidYellowCurve2.jpg


---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps:

	1) I converted the images to grayscale in order to work on one color level
	2) I ran the gaussian filter in order to low pass the picture
	3) I ran the Canny algorithm in order to find "good" edges
	4) I ran the Hough algorithm to find lines 
	5) According to the given images I selected the "region of interest" 
	 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by:

	1. Classifying the lines we found to three lines:
		a) non relevant lines. They don't have a steep slop (the slop threshold was set to 0.5) 
		b) Lines that classify as right lane: Those lines have a "steep" and negative slop
		c) Lines that classify as left lane: Those lines have a "steep" and positive slop
	2. Interpolate the lines we found to three lines, so we get more data points to longer lines therefore they will have higher weight in the linear regression 
	3. We used polyfit to get the linear extrapolation for each lane


Here is the image after each pipeline stage: 

![alt text][image1]
![alt text][image2]
![alt text][image3]
![alt text][image4]
![alt text][image5]


###2. Identify potential shortcomings with your current pipeline

	potential shortcoming: 
	1) Most of the thresholds were set according the the given images. In different visibility condition the threshold will not be good enough. Also in different image size the "region of interest" should be different (for example in the "Optional Challenge").
	2) We assume that the road is not too curvy so we can use linear fit. In a curvy road this would be a problem.
	3) we assume there is always (relevant) lane lines in each frame, and that there are no marks on the road that could be wrongly identified as lanes (or example in the "Optional Challenge").


###3. Suggest possible improvements to your pipeline

	1) Normalize the image - in size, in light level and so on.  
	2) We could use a 2ed order fit to help us in a curvy road.
	3) We should use the data we had from previous frames. The data from the history can help us smooth the result we get in the current frame

###4. In the "Optional Challenge":
	1) We normalized the picture size (according to the pictures of size 540x960)
	2) Due to different (and more noisy) conditions, we used different thresholds for the Canny algorithm and for Hough algorithm.
	3) In order to apply some kind of (low pass) filtering over time we set a boundaries for the point where the line crosses the X-axis at the maximal Y value  