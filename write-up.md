# **Finding Lane Lines on the Road** 

## Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consists of 6 steps, as described below:

1. Converting the images (video has multiple frames each containing a single image) to grayscale using the cv2.cvtColor() function.
2. Applying Gaussian smoothing to the grayscale images using the cv2.GaussianBlur() function.
3. Applying Canny edge detection to the smoothed images using the cv2.Canny() function.
4. Masking the region of interest in the Canny edge images using the cv2.fillPoly() function, whose parameters were fine tuned from random values.
5. Applying Hough transform to the masked images to detect lines using the cv2.HoughLinesP() function, whose parameters were fine tuned from random values.
6. Drawing the lines on the original images, which is done by modifying the draw_lines() function, as described below.
    - The draw_lines() function takes the output of the Hough transform and draws a single line on the left and right lanes.
    - The function first classifies the lines into left and right lanes based on the slope of the lines (positive slope -> left lane; negative slope -> right lane; because lanes seem to converge around x_center at a higher value of y) and the position of the lines in the image, which is cruicial for the challenge video, because the right curve in the road makes the right lane have a positive slope, making it falsly classified as a left lane.
    - It then uses the RANSAC algorithm to fit a line to the points detected by the Hough transform.
    - The function then extrapolates the line to the top and bottom of the region of interest.
    - Finally, the function draws the lines on the original images using the cv2.line() function.


### 2. Identify potential shortcomings with your current pipeline

One potential shortcoming would be what would happen when the lane lines are not straight. The current pipeline would not be able to detect curved lane lines, which can been seen in the challenge video.

Another shortcoming could be having missing lane lines in the region of interest, which would cause the pipeline to fail to detect the lanes.

Also, presence of other objects in the region of interest, such as cars (as seen in the challenge view where the cars in the opposite side of the road where being treated as lanes as well), could cause the pipeline to detect false lane lines, which was not evident in the test images and videos, but could be a possible shortcoming in real world scenarios.


### 3. Suggest possible improvements to your pipeline

A possible improvement could be to fine tune the region of interest based on the camera position and angle, as the current region of interest is a fixed polygon, which may not work in all scenarios, as in the curved road in the challenge video.

Another potential improvement would be to use a more robust algorithm to detect lane lines instead of Hough, like an RNN model, which could be more accurate in a variety of scenaries, such as curved lane lines and presence of other objects in the region of interest.

Otherwise, the Hough transform parameter can be fine tuned further for a more accurate lane lines detection, as the current parameters were fine tuned from a small set of random values.
