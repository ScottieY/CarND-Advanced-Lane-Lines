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

### Camera Calibration

#### 1. Briefly state how you computed the camera matrix and distortion coefficients. Provide an example of a distortion corrected calibration image.

The code for this step is contained in the first code cell of the IPython notebook located in "./examples/example.ipynb" (or in lines # through # of the file called `some_file.py`).  

I start by preparing "object points", which will be the (x, y, z) coordinates of the chessboard corners in the world. Here I am assuming the chessboard is fixed on the (x, y) plane at z=0, such that the object points are the same for each calibration image.  Thus, `objp` is just a replicated array of coordinates, and `objpoints` will be appended with a copy of it every time I successfully detect all chessboard corners in a test image.  `imgpoints` will be appended with the (x, y) pixel position of each of the corners in the image plane with each successful chessboard detection.  

![corner](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/corner.png)

I then used the output `objpoints` and `imgpoints` to compute the camera calibration and distortion coefficients using the `cv2.calibrateCamera()` function.  I applied this distortion correction to the test image using the `cv2.undistort()` function and obtained this result: 

![chess](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/chess.png)

### Pipeline

#### 1. Provide an example of a distortion-corrected image.

The result objpoints and image points were used to correct the distortion for the following images

![distort](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/distort.png)



#### 2. Describe how (and identify where in your code) you used color transforms, gradients or other methods to create a thresholded binary image.  Provide an example of a binary image result.

I used a combination of color and gradient thresholds to generate a binary image shown in `combined_thresh` function.  Here's an example of my output for this step.  

#### ![thresh](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/thresh.png)3. Describe how (and identify where in your code) you performed a perspective transform and provide an example of a transformed image.

The code for my perspective transform includes a function called `warper()`,The `warper()` function takes as inputs an image (`img`), as well as source (`src`) and destination (`dst`) points.  I chose the hardcode the source and destination points in the following manner:

```python
src = np.float32([(690,452),(1093,718),(272,716),(600,452)])
dst = np.float32([(900,0),(900,720),(270,720),(270,0)])
```

This resulted in the following source and destination points:

|   Source   | Destination |
| :--------: | :---------: |
| (690,452)  |   (900,0)   |
| (1093,718) |  (900,720)  |
| (272,716)  |  (270,720)  |
| (600,452)  |   (270,0)   |

I verified that my perspective transform was working as expected by drawing the `src` and `dst` points onto a test image and its warped counterpart to verify that the lines appear parallel in the warped image.

![unwarped](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/unwarped.png)

#### 4. Describe how (and identify where in your code) you identified lane-line pixels and fit their positions with a polynomial?

I used sliding window method to find where most white dots were, and discard the rest error dots.

Then I fit two second order polynomials (left and right) through those points `fit_poly` 

![draw_yellow_lines](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/draw_yellow_lines.png)

#### 5. Describe how (and identify where in your code) you calculated the radius of curvature of the lane and the position of the vehicle with respect to center.

I did this with formula presented in class

curvature = ((1+(2Ay + B)^2^)^3/2^ )/abs(2A)

shown in `find_curvature` function

#### 6. Provide an example image of your result plotted back down onto the road such that the lane area is identified clearly.

![text_on](/Users/ScottieYan/CarND/CarND-Advanced-Lane-Lines/output_images/text_on.png)

---



### Pipeline (video)

#### 1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (wobbly lines are ok but no catastrophic failures that would cause the car to drive off the road!).

Here's a [link to my video result](./output_images/project_video_out.mp4)
