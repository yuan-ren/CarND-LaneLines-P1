#**Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./example.png "Example"

---

### Reflection

###1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps: 

1. Convert the images to grayscale.

2. Gaussian blur to remove unwanted noise.

3. Call Canny edge detection to extract all edges.

4. Use image mask to extract edges in the regions of interest.

5. Call Hough transformation to get lines along edges.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as below:

1. After Hough transformation, resulting lines (vertices) are divided into 2 groups. The left group contains all vertices located on the left half side of the image, and the right group contains all vertices on the right half side of the image. 

2. Perform linear fitting on vertices in the left and right groups respectively. Linear fitting results are coefficients of left and right lines of the lane.

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![example_image][image1]


###2. Identify potential shortcomings with your current pipeline

1. Current pipeline is very sensitive to parameters of Hough transformation. For example, sometimes the line segments are short so small *min_line_length* parameter is needed. But small *min_length_length* may lead to unwanted lines from objects other than lane markers

2. Current pipeline is sensitive to parameters of Canny transformation. This is especially true when some lane markers are in shadow.

3. Current pipeline is sensitive to camera mounting, zoom and other settings. Change of camera settings will change position of lane on the image, so region of interest is changed. Current pipeline uses a predefined region of interest. In some cases unwanted lines are included, therefore the calculated left and right lines of lane may deviate from what they should be.


###3. Suggest possible improvements to your pipeline

1. Initial images need to be sharpened (modifying image contrast) before further processing. This way lane markers in the shadow will be more obvious, and the pipeline is less dependent on threshold setting of Canny edge detection.

2. In Hough transformation, we can only extract line pairs that are in parallel to each other. Because these line pairs are opposite edges of a lane marker. This way we can exclude some random objects (not lane markers) in the region of interest.

3. Don't use predefined region of interest as image masks. Instead, detect the rough region of interest by color of lane (this assumes that lane color is uniform but it's not ture when the lane is in shadow).
