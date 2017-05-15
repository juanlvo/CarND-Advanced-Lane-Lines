## Writeup

---

**Advanced Lane Finding Project**

The goals / steps of this project are the following:

* Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
* Apply a distortion correction to raw images.
* Use color transforms, gradients, etc., to create a thresholded binary image.
* Apply a perspective transform to rectify binary image ("birds-eye view").
* Detect lane pixels and fit to find the lane boundary.
* Determine the curvature of the lane and vehicle position with respect to center.
* Warp the detected lane boundaries back onto the original image.
* Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: example_image_undistorted.png "Undistorted"
[image2]: combination_of_color_spaces_and_perspective.png "Color spaces and perspective"
[image3]: lane_lines_detection_1.png "Lane lines detection 1"
[image4]: lane_lines_detection_2.png "Lane lines detection 2"
[image5]: plotted_image.png "Final Image"
[image6]: calibration.png "Calibration"
[image7]: warp.png "Warped image"
[image8]: points_1.png "Detection of points 1"
[image9]: points_2.png "Detection of points 2"
[video1]: ./project_video.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

<b>Writeup / README</b>
<table>
	<tr>
		<th>Criteria</th>
		<th>Meets Specifications</th>
	</tr>
	<tr>
		<td>Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf.</td>
		<td>The writeup / README should include a statement and supporting figures / images that explain how each rubric item was addressed, and specifically where in the code each step was handled.</td>
	</tr>
</table>

<b>Camera Calibration</b>
<table>
	<tr>
		<th>Criteria</th>
		<th>Meets Specifications</th>
	</tr>
	<tr>
		<td>Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.</td>
		<td>OpenCV functions or other methods were used to calculate the correct camera matrix and distortion coefficients using the calibration chessboard images provided in the repository (note these are 9x6 chessboard images, unlike the 8x6 images used in the lesson). The distortion matrix should be used to un-distort one of the calibration images provided as a demonstration that the calibration is correct. Example of undistorted calibration image is Included in the writeup (or saved to a folder).</td>
	</tr>
</table>

<b>Pipeline (test images)</b>
<table>
	<tr>
		<th>Criteria</th>
		<th>Meets Specifications</th>		
	</tr>
	<tr>
		<td>Provide an example of a distortion-corrected image.</td>
		<td>Distortion correction that was calculated via camera calibration has been correctly applied to each image. An example of a distortion corrected image is include in this writeup.</td>
	</tr>
	<tr>
		<td>
			Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image. Provide an example of a binary image result.
		</td>
		<td>
			In the project the function convertColorSpace which is in charge of make the transformation of several colors (Color space B from LAB, Color Space R, all colors spaces from HLS) spaces, Sobel x, Threshold x and y. 
		</td>
	</tr>
	<tr>
		<td>
			Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.
		</td>
		<td>
			In the code you can find the function def warp where is finded the perspective of the image.
		</td>
	</tr>
	<tr>
		<td>Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?</td>
		<td>In the method process_image can be found all the process related with the detection of the line lines, the method used to identify was polynomial. Example images with line pixels identified and a fit overplotted are included in this writeup.</td>
	</tr>
	<tr>
		<td>
			Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.
		</td>
		<td>
			The equation use it was R = (1+(2Ay+B)**2)**1.5)/abs(2A) and was used a convertion pixel/meters ym_per_pix = 30/720 and xm_per_pix = 3.7/700 all of this was implemented at the end of the method process_image
		</td>
	</tr>
	<tr>
		<td>Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.</td>
		<td>The fit from the rectified image has been warped back onto the original image and plotted to identify the lane boundaries. This could be found it in image example in this writeup.</td>
	</tr>
</table>

<b>Pipeline (video)</b>
<table>
	<tr>
		<th>Criteria</th>
		<th>Meets Specifications</th>	
	</tr>
	<tr>
		<td>
			Provide a link to your final video output. Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!)
		</td>
		<td>
			<a href="https://github.com/juanlvo/CarND-Advanced-Lane-Lines/blob/master/project_video.mp4">project_video.mp4</a> 
		</td>
	</tr>
</table>

<b>Discussion</b>
<table>
	<tr>
		<th>Criteria</th>
		<th>Meets Specifications</th>
	</tr>
	<tr>
		<td>
			Briefly discuss any problems / issues you faced in your implementation of this project. Where will your pipeline likely fail? What could you do to make it more robust?
		</td>
		<td>
			The biggest problem of this project was the detection of the lane line correctly under constants changes of the lights and colors of the pavements, that is the reason why it was need it to combinate so many space colors at the same time
		</td>
	</tr>
</table>

<b>Suggestions to Make Your Project Stand Out!</b>
<table>
	<td>
		For the case of the video for this project was simple to detect lane lines, but if the conditions of lights change probably is not going to detect the lines on the road, could be implemented an algorithm dynamic for the changes of light conditions.
	</td>
</table>
---

### Writeup / README

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

Inside of the file project.ipynb in the first section you could find all the information related with the calibration, for execute this process was need it a series of images provided by udacity in total were identify by the algoritm, in total 17 images were identify by the algorithm

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![alt text][image6]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

To demonstrate this step, I will describe how I apply the distortion correction to one of the test images like this one:
![alt text][image1]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

In the project the function convertColorSpace which is in charge of make the transformation of several colors (Color space B from LAB, Color Space R, all colors spaces from HLS) spaces, Sobel x, Threshold x and y. 

![alt text][image2]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`, which appears in lines 1 through 8 in the file `project.py`.  The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32(
        [[305, 674],
         [1080, 674],
         [348, 639],
         [1028, 639]])
    
dst = np.float32(
        [[310, 670],
         [1050, 670],
         [310, 640],
         [1050, 640]])
```

This resulted in the following source and destination points:

| Source        | Destination   | 
|:-------------:|:-------------:| 
| 305, 674      | 310, 670      | 
| 1080, 674     | 1050, 670      |
| 348, 639      | 310, 640      |
| 1028, 639     | 1050, 640      |

![alt text][image7]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

n the method process_image can be found all the process related with the detection of the line lines, the method used to identify was polynomial.

![alt text][image5]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this in lines # through # in my code in `my_other_file.py`

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

I implemented this step in lines # through # in my code in `yet_another_file.py` in the function `map_lane()`.  Here is an example of my result on a test image:

![alt text][image8]

![alt text][image9]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a link: <a href="https://github.com/juanlvo/CarND-Advanced-Lane-Lines/blob/master/project_video.mp4">project_video.mp4</a> 

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The biggest problem of this project was the detection of the lane line correctly under constants changes of the lights and colors of the pavements, that is the reason why it was need it to combinate so many space colors at the same time 
