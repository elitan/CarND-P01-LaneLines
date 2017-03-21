# **Finding Lane Lines on the Road**

## The goals / steps of this project are the following:

* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[raw_image]: ./writeup_images/raw_image.png "Raw image"
[grayscale]: ./writeup_images/grayscaled.png "Grayscale"
[gaussian]: ./writeup_images/gaussian.png "Gaussian"
[canny]: ./writeup_images/canny.png "Canny"
[region_of_interest]: ./writeup_images/region_of_interest.png "Region of Interest"
[hough_lines]: ./writeup_images/hough_lines.png "Hough lines"
[extrapolate_lanes]: ./writeup_images/extrapolate_lanes.png "Extrapolate lanes"
[final_image]: ./writeup_images/final_image.png "Final image"

---

## 1. My pipeline to detect lane lines


The pipeline that I was using to detect and draw lines with was using 7 steps.

0\. Our raw image.

![alt text][raw_image]

1\. Convert image to gausian
- This was a preproccessing step for step 3.

![alt text][grayscale]

2\. Applying a gaussian blur on the image
- Used to smooth out and remove noise from the grayscaled image.

![alt text][gaussian]

3\. Apply canny edge detection on the image
- Find and return edges in the image.

![alt text][canny]

4\. Applying a region_of_interest function
- For this lane line finding problem, only a certain region is of interest for us.

![alt text][region_of_interest]

5\. Applying a hough_lines function
- This is to specify actual lines ((x1,y1), (x2,y2)) out of the canny edge detection.

![alt text][hough_lines]

6\. Differentiate all left and right lines and find one average left and right line for each lane respectively.
- The two lines (the left and right one) are drawn on the raw image and is returned

![alt text][extrapolate_lanes]

7\. Place the two lines above on the raw image

![alt text][final_image]

---

Where I did most of the work was in the `draw_lines` function. In that function, I first find all left and right lines, then I take the respective lines and calculate an average left and right line. The specific average I am looking for is `k` and `m` from the line formula `y = kx + m`. That way I get the gradient `k` and orientation `m`. After that I am using the `y` variable to place my line. I am using the `y` to be placed in the bottom of the image and in the top area of my area of interest. Once I have `y`, `k` and `m` I can calculate `x` and there by getting my two coordinates of my line.

## 2. Reflection

First of all, as a reflection, this was a really fun project! This looks promising for the rest of this term.

### Potential shortcomings and improvements of my pipeline

The left and right lines in the video is a bit jumpy. Since the video is really just images in a time series, it make sense to have a inertial filter on the lines that is drawn. Right now, every image is treated individually, without prior knowledge to where the line was one frame ago. This does not accurately replicate how we humans are perceiving the world.

Also, some of the images found in the hough lines function is simply noise. Lines that stand out too much should be removed.

I am only drawing two straight lines. This works OK for the two example videos of this project, since the video is from a car who is driving on a straight road. However, it get obvious that my technique does not work once the road is turning, as seen on the challenge video. What I should do here, I think, is to divide the right and left lines in to chunks of lines, and try to look at a smaller portion of the line to make the lines turn. Another option, that I am thinking about, is to try to mimic a function (instead of one large `y = kx + m`) to the different chunk of lines that is brought out by the hough lines function.
