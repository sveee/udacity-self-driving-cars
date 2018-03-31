
## Finding Lane Lines on the Road

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/solidYellowCurve.png

---

### Reflection

### 1. Pipeline description

My pipeline consists of couple of steps. They are:

 - convert the image to grayscale
 - use Gaussian Blur to remove the noise from the image
 - use Canny edge detection to find the most relevant edges in the image
 - restrict the edges by a triangle area siuated at the bottom in the middle where the lane lanes are expected to be situated. On the restricted edges Hough line trasform is run to find lines in the image
 - before the lines are drawn on the image filter the lines that not have specific angle range with the horizon (between 25 and 55 degrees). After that we classify lines into either being "left" or "right". For each class, we use the endpoints of the lines to train a Ridge regression in order to obtain robust averaging of the all lines
  - after all these steps are get two lane lines drawn

Here is the result of after applying the pipeline a test image: 

![Example][image1]


### 2. Potential shortcomings 

#### Issue 1
The Canny edge detection and Hough transform are tuned in a way that allows that we don't miss out any significant line but often times the number of identified is too much if there are too many artefacts in the image. The number of false positives might be too big. The average lines would be unreliable and it would wobble too much.

#### Issue 2
The pipeline is reliant on the position of the lines/angle of the lane line with respect to the center of the image. If the car tries to turn left or right and as a consequence crosses the lane line the algorithm would get confused where the lane lines actually are. This in turn could lead to confusing the vehicle.


### 3. Suggestions for possible improvements to the pipeline

A fix to **Issue 1** might be to add additioin filters (we might want the distance from bottom middle to a Hough line to be in specific boundaries)

A fix to **Issue 2** might be to switch to diferent method altogether. Identifing first the road, then from the road identifying the lane line would be more robust approach. This would use image segmentation.
