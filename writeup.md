#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./test_images/out/solidWhiteCurve.jpg "solidWhiteCurve"
[image2]: ./test_images/out/solidWhiteRight.jpg "solidWhiteRight"
[image3]: ./test_images/out/solidYellowCurve.jpg "solidYellowCurve"
[image4]: ./test_images/out/solidYellowCurve2.jpg "solidYellowCurve2"
[image5]: ./test_images/out/solidYellowLeft.jpg "solidYellowLeft"
[image6]: ./test_images/out/whiteCarLaneSwitch.jpg "whiteCarLaneSwitch"
[image7]: ./01_two_left_lines.jpg "01_two_left_lines"
[image8]: ./02_wrong_slope.jpg "02_wrong_slope"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 6 steps.
Step 1 - Convert the image to grayscale (to remove difference between white lines, yellow lines, etc).
Step 2 - Apply blur filter (gaussian blur filter)
Step 3 - Apply canny filter (leave only edges)
Step 4 - Apply region mask (leave only objects in the front of the car)
Step 5 - Apply Hough transform (detect lines)
Step 6 - Combine original image and black image with lines (after Hough transform)
Function process_image_implementation() contains the implementation of the pipeline.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by steps:
Step 1 - Place lines (edges) in several groups named contours (in code). Create list of contours, group "similar" lines to one contour. "Similar" means one contour contains list of lines that can form one long line. In other words if y=mx+b, "similar" means with close slope or "m" koefficient.
Step 2 - Sort list of contours by the size of contours (number of points in a contour) from greater to smaller. The first two contours should be the left and right lane lines.
Step 3 - Take two first contours from this list. Approximate each contour with line and draw it.
Function draw_lines() contains modificated code.

If you'd like to include images to show how the pipeline works, here is how to include an image:


If you'd like to include images to show the results of the lane lines detection:
![solidWhiteCurve][image1]
![solidWhiteRight][image2]
![solidYellowCurve][image3]
![solidYellowCurve2][image4]
![solidYellowLeft][image5]
![whiteCarLaneSwitch][image6]


###2. Identify potential shortcomings with your current pipeline

I've seen two major shortcomings (that I've seen in the challenge video)

1. Not always the first two biggest contours from my solution are left and right lines. Sometimes I've seen on the challenge video two left lines.
![01_two_left_lines][image7]

2. Not always each contour contains lines (edges) from ONE lane line of the road. Sometimes one contour contains lines (edges) from several lane line and when I calculate average line it doesn't coinside with the lane line from the image.
![02_wrong_slope][image8]

3. Potential weak side is that we take two biggest contours. Lane line can contain small amount of points (if it's a segmented line), so after sorting this lane line could be not in the two first contours.

###3. Suggest possible improvements to your pipeline

A possible improvements would be to
1. Before combine lines (edges) in groups (contours) separate them to two lists - with positive slope (potential left lines) and negative slope (potential right lines). Process each list separately - group in contours, sort by size, take first contour and approximate with line. This could solve the first shortcoming.
2. Use both koefficients: "m" (slope) and "b" (y=mx+b). We should group lines (edge) using both koefficients. This could solve the second shortcoming.
3. Use length of the line - it' s a distance between farthest points in the contour. This could solve the third shortcoming.
