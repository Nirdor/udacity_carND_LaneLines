# **Finding Lane Lines on the Road** 

## Writeup

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/gradient.jpg "Gradient"
[image2]: ./examples/masked.jpg "Masked"
[image3]: ./examples/line_img.jpg "Lines"

---

### Reflection

### 1. Description of the Pipeline

My pipeline consists of 5 steps: 
1. Convert image to gray scale and calculate the gradient with canny. ![Gradient image][image1]
2. Apply a Mask to crop the image. As vertices I use the four relative coordinates: {*1/14width*, *height*}, {*4/9width*, *3/5height*}, {*5/9width*, *3/5height*} and {*13/14width*, *height*}.
   By using relative coordinates the pipeline still works with resized images of the same kind. ![Masked image][image2]
3. Use the HoughTransform to find line segments with following parameters for the HoughLinesP function: `ϱ=1`, `θ=π/180`, `threshold=20`, `min_line_len=20` and `max_line_gap=20`
4. Filter the line segments using their slope: For each line segment calculate the form y=mx+b. Then remove all segments that do not cross the bottom border of the masked image.
   Also divide the remaining line segments into right and left segments using their slope. ![Line image][image3]
5. Draw the line over the input image using the `draw_lines()` function.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging the slope for all given lines and then drawing a single line through the topmost point using the averaged slope.


### 2. Identify potential shortcomings with your current pipeline

Because i filter my found line segments using their slope, one potential shortcoming would be that if the lane makes a strong turn, any segments which would not cross the bottom border of the image, would be filtered too.

Another shortcoming is that by simply averaging the slopes and drawing a single line through the topmost point the result gets instable near the bottom of the image resulting in jumping around in the video.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to draw a line by selecting two stable points on found line segments instead of averaging the slopes.

Another potential improvement could be to reduce unwanted line segments by choosing better parameters for the houghlines function instead of filtering them by slope.


### 4. Challenge

For the challenge I adjusted the mask, because at the bottom of the image is the engine hood.

Also i tried a segmental approach for selecting,interpolating and drawing the lines.

