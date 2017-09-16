if you want to see mathjax enabled html, please see [here](./README.html)

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on work in a written report


[//]: # (Image References)

[image1]: ./test_images_output/line-segments-input.jpg "Input"
[image2]: ./test_images_output/line-segments-result.jpg "Output"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps*

1. Conversion to grayscale
2. Applying Gaussian filter
3. Edgedetection using Canny algorithm
4. Hough transform to find straight line segment
5. Region division at the center line

In order to draw a single line on the left and right lanes, I added some calculation to find regression line for left/right region.
Parameter of straight line $y = a x + b$ can be estimated by covariance and average:

###### slope $a$
$a = \frac{s_{xy}}{s_x^{2}} = \frac{\sum_{i=1}^{n} (x_i - \bar{x})(y_i - \bar{y})}{\sum_{i=1}^{n} (x_i - \bar{x})^2}$

###### intercept $b$
$b = \bar{y} - a\bar{x}$


#### Example
###### Input
![alt text][image1]
###### Output
![alt text][image2]


### 2. Identify potential shortcomings with your current pipeline

There are potential shortcomings when ...

* the vehicle driving on curve
* there is few sunlight (e.g. night)
* the weather is bad
* white lanes are concealed by leading vehicle
* the edge detection fails by shades

### 3. Suggest possible improvements to your pipeline

A possible improvement would be to 

* Use randomized algorithm like RANSAC, which reduce potential risk for false positive
* Use weighted average when we calculate regression line, for example, shorter line should be less impact
* Use pre-filtering like local contrast correction, to reduce bad effect at cast shadows
* Use some specialized kernel when applying sobel filter. For example, horizontal line can be ignored which shouldn't be lane markings. Of course, such a filtering can be applied after the line detection is applied.