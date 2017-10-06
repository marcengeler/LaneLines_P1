# **Finding Lane Lines on the Road** 

[image1]: ./grayscale.png "Grayscale"
[image2]: ./edges.png "Edges"
[image3]: ./roi_adaptive.png "Adaptive Region of Interest"

## Writeup

### Grayscale Canny-Edge

To start off, i used a grayscale image and a canny edge algorithm based on fixed thresholds to identify and edges.

![alt text][image1]

Using fixed thresholds for the canny edge detection and filtering all the lines outside of a trapeziodal region of interest, the resulting image looked promising

![alt_text][image2]

### Hough Algorithm on Videos

Applying a basic hough algorithm on videos showed promisinig results, but failed repeatedly on the last video. Testing on all three videos allowed me to get more insight in what are of code is prone to errors.
First of all, I changed the region of interest to a relative region of interest, based on the image size. This way i could easily change the cropped image based on the video size (especially for the last one).
This worked out quite well and was an easy addition to the code.

![alt_text][image3]

### Extrapolation of Hough Lines

It was clear that we have to find two distinct lines with two different slopes. So i calculated all the slopes for all the lines and then the mean slope of all points.
Then i divided all the lines into lines with slope < slope_{mean} and lines with slope >= slope_{mean}

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of 5 steps. First, I converted the images to grayscale, then I .... 

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by ...

If you'd like to include images to show how the pipeline works, here is how to include an image: 

![alt text][image1]


### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be what would happen when ... 

Another shortcoming could be ...


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to ...

Another potential improvement could be to ...
