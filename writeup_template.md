# **Finding Lane Lines on the Road** 

[image1]: ./grayscale.png "Grayscale"
[image2]: ./edges.png "Edges"
[image3]: ./roi_adaptive.PNG "Adaptive Region of Interest"
[image4]: ./challenge_color.PNG "Color Difference"

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
Then i divided all the lines into lines with slope < mean_slope and lines with slope >= mean_slope. This worked out well for the first two videos, but failed miserably on the challenge video.
I quickly realized, that the bonnet and condition of the road caused horizontal lines to appear. By restricting the slope to abs(slope) > 0.3 it was possible to filter out horizontal or near horizontal lines.
Using the two separated clusters of lines and slopes i could calculate a mean slope for the right hand side, as well as a mean slope for the left hand side of the car. Using the average left points and average right points it was simple to find the full function

mx + b

which described the whole line and to draw it on the image.

#### Problems with the extrapolation

My first approach was to use a line matching tool from math or numpy, however, these tools repeatedly failed when applied to the points. So I had to implement the slope calculation manually using the approach described above. This showed good results on all the images and videos.

### Improving the Canny Edge Algorithm

The only shortcoming to the algorithm at this point was the bad condition of the road in the challenge video. In order to improve that, i analyzed that part thoroughly and came up with the idea to analyze each color channel seperately instead of just focussing on the grayscale image.
The yellow line does not differ much from the ground in a grayscale image at this point.

![alt_text][image4]

In the top left corner of the image is the lane marking on the grey highway. As you can see it doesnt differ much. However by seperating the three color channels and then combining all the edges together before running the hough algorithm improved the detection of the lane on this difficult part of the road.

## Shortcomings of the current algorithm

Because of all the added on weight for the challenge video, my code is rather inefficient.

Also the road matching for the challenge video is not ideal yet. I think having a memory in the code wouldn't hurt. This means that adjusting the slope of the lane line with a Auto Regression Moving Average, as ther should not be sudden changes to the road line. This would reduce chitter and probably also unreliable lines.
