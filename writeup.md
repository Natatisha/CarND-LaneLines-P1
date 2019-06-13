**Finding Lane Lines on the Road**

### Reflection

### Pipeline description.

My pipeline consists of 8 steps. 
#### 1. Gamma correction. 
I've noticed that sometimes some road parts could be too light compared to the other road, and it's hard to find a yellow line in such conditions. So I've added a gamma correction to an image (the source is [OpenCV doc](https://docs.opencv.org/3.4/d3/dc1/tutorial_basic_linear_transform.html)) and played around a bit with `gamma` parameter to fix this and increase the efficiency of the color selection step.
#### 2. Color selection.
Next step is to select the yellow and white colors from an image. It's something very similar to RGB selection as we did in lesson, but for the yellow color, it's HSV selection is more convenient. I've added `color_selection()` function to perform this step. 
#### 3. Grayscale. 
After color selection, I performed the grayscale conversion. It's quite a default step which transforms RGB image with three layers for red, green and blue to a one-layer image of 8-bit color.    
#### 4. Gaussian blur.
I've added Gaussian blur with `kernel_size=3` to soften the image a little bit. I've played around with kernel size parameter and decided that making it too big doesn't help to achieve the desired result. 
#### 5. Canny.
The fifth step is Canny edge detection. The hyperparameters for this step were the same as in class:
``
low_threshold=50
high_threshold=150
``
#### 6. Region of interest. 
The region of interest in our case is the lower half of an image, more narrow at the horizon. So my polygon is a triangle, but a bit lower than the half of the image height. This allows to capture the most important for line detection part of the road and avoid confusion caused by other objects.
#### 7. Hough lines. 
Probably the most important part of the algorithm. After many experiments I've ended up with these hyperparameters:

``
    rho = 1;
    theta = (np.pi/180.0);
    threshold = 20;
    min_line_length = 20;
    max_line_gap = 300;
``
#### 8. Weighted image.
This step returns an original image with hough lines drawn on it. 

The results of an algorithm are shown on test images in the notebook. 

### 2. Identify potential shortcomings with your current pipeline

This algorithm is still quite raw and unstable. The most obvious shortcomings are: 
- The algorithm could fail to identify lines in the dark.
- Another cause of bad performance could be bad weather (fog, rain, snow, etc.)

### 3. Suggest possible improvements to your pipeline
- We could try to add more pre-processing to an image for more efficient color selection.
- Another improvement could be to work with hough lines. Some additional transformations and cleanup could help to improve the algorithm performance. 


