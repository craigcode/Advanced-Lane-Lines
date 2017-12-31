## Udacity SDC - Advanced Lane Lines - Project 4
## Writeup 

The goals / steps of this project are the following:

- Compute the camera calibration matrix and distortion coefficients given a set of chessboard images.
- Apply a distortion correction to raw images.
- Use color transforms, gradients, etc., to create a thresholded binary image.
- Apply a perspective transform to rectify binary image ("birds-eye view").
- Detect lane pixels and fit to find the lane boundary.
- Determine the curvature of the lane and vehicle position with respect to center.
- Warp the detected lane boundaries back onto the original image.
- Output visual display of the lane boundaries and numerical estimation of lane curvature and vehicle position.

[//]: # (Image References)

[image1]: ./output_images/calibration1_orig_distortion_corrected_pair.png "Camera Calibration"
[image2]: ./output_images/test2_distortion_corrected.jpg "Distortion Corrected"
[image3]: ./output_images/test2_color_transform.jpg "Color Transform"
[image4]: ./output_images/test2_gradient_transform.jpg "Gradient Transform"
[image5]: ./output_images/test2_combined_transform.jpg "Combined Transform"
[image6]: ./output_images/test2_birds_eye_view.jpg "Bird's Eye View"
[image7]: ./output_images/test2_lane_boundaries.jpg "Lane Boundaries"
[image8]: ./output_images/test2_lane_overlay.jpg "Lane Overlay"
[image9]: ./output_images/test6_lane_line_final.jpg "Final Pipeline Image"
[video1]: ./project_video_output.mp4 "Video"

## [Rubric](https://review.udacity.com/#!/rubrics/571/view) Points

### Here I will consider the rubric points individually and describe how I addressed each point in my implementation. 

### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  [Here](https://github.com/udacity/CarND-Advanced-Lane-Lines/blob/master/writeup_template.md) is a template writeup for this project you can use as a guide and a starting point.  

You're reading it!

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

In cell 3, `computeCalibrationMatrix()` I prepare "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

In cell 5, `distortionCorrection()` I use the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![Camera Calibration][image1]

### Pipeline (single images)

#### 1. Provide an example of a distortion-corrected image.

![Distortion Correction][image2]

#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

In cell 8, `thresholdImage()` I use color and gradient thresholds to generate a combined transform image.  

![Color Transform][image3]
![Gradient Transform][image4]
![Combined Transforms][image5]

#### 3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

In cell 10, `perspectiveTransform()` uses `cv2.getPerspectiveTransform()` and `cv2.warpPerspective()`  functions to warp the image into a Bird's Eye View. 

![alt text][image6]

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

In cell 12, `findLaneBoundary()` is a near-copy of the materials of **Udacity Lesson 33 - Finding the Lanes **using a histogram to find the peak left and right halves, concatenate the arrays of indices, build left and right line pixel positions while `np.polyfit()` fit a second order polynomial to each.

![alt text][image7]

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I calculate radius of curvature and vehicle deviation values in cell 14, `calcRadiusCurvatureDeviation()` using  somewhat arbitrary values for x and y meters per pixel based on experimentation.

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

In cell 16, `drawLane()` overlays the lane area on the original image

![Lane Overlay][image8]

Cell 18 defines `overlayText()`  which draws the text describing the calculated radius of curvature values while cell 19 defines pipeline() - the entire pipeline process for each image.

![Lane Line Final Image for Test6][image9]

---

### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](https://github.com/craigcode/Advanced-Lane-Lines/blob/master/project_video_output.mp4)

---

### Discussion

#### 1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

The current pipeline has problems charting the lane in the other challenge videos where line breaks and shadows cause erratic lane detection errors; to fix this I would need to introduce greater consistency and projection between frames. Fun challenge.
