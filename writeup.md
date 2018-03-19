# **Finding Lane Lines on the Road**

## Intro

This file describes ideas and reflection for finding lane lines project for Udacity Nanodegree program "Self-Driving Car Engineer".

The purpose of this project was:

* Create function in jupyter notebook. This function should track exactly two lane lines on the image from camera located on the moving car. As the result, this function should return input image with two red lines, highlighting lane lines.
* Write reflection report with algorithm description, potential shortcomings and possible improvements.

---

## Results

Here is the results of the algorithm:

* Algorithm tracks lane lines on 100% of input images from video files `solidWhiteRight.mp4` and `solidYellowLeft.mp4`. Exactly two lines was tracked on each frame of the video input and visual control showed that this lines are match lane lines.
* For `challenge.mp4` file results looks pretty well. Most of the time lines tracks correctly, some persistence errors can be fixed with ideas from "possible improvements" section later in this document.

---

## Reflection

### Pipeline description.

I'll demonstrate algorithm steps based on this input picture:

![Input image][image_base]

Please, consider this input is challengeable. Intermediate outputs looks mach nicer on simpler inputs.


My pipeline consists of next steps:

1. Image is gray-scaled

    ![Gray-scaled image][image_gs]

1. Image is blurred. It happened that algorithm works pretty well with blur values from 5 to 13

    ![Blurred image][image_bl]

1. Canny algorithm is applied to the blurred image to get gradient picture. To deal with sunny area on the road on video `chalenge.mp4` canny parameters have to be set low and some additional filtering is applied later.

    ![Image gradient][image_grad]

1. Area of interest is chosen on the image

    ![Image region of interest][image_roi]

1. Hough lines procedure was applied to the image. Max line gap parameter was set so to track dash lane line as solid line. Other parameters was set experimentally.

    ![Image with tracked lines][image_lines]

1. Lines are combined to groups with similar slope angle.

1. For each group average line is calculated, extended to expected edges and removed if it's slope angle is too high

1. If result lines quantity is higher that 2, the distance between each pair of lines is calculated on the low edge of the image. The best pair is expected to be pair with closest distance value to some fixed parameter.

    ![Output image][image_final]


### Potential shortcomings

Here is the list of potential shortcomings for current algorithm realization:

1. It works only when car is more or less parallel to the lane lines.
2. It traces only two lines. It may be an issue if car would decide to change line.
3. Small region of line searching. It may become an issue if car would go uphill or downhill.

### Possible improvements

And here is some ideas how this algorithm can be improved:

1. The obvious one is to use data from previous frame. Car can't jump, so the lines on this frame have to be close to there position on the previous frame.
1. All parameters may be adaptive. We ran algorithm on the image, tighten or loosen thresholds and other parameters depending on number of lines we traced and than run algorithm again for same image.
1. May be it would be a good idea to try RANSAC method.

### Thank you for reading!

[//]: # (Image References)
[image_base]: ./writeup_imgs/report_base_sm.jpg "Input image"
[image_gs]: ./writeup_imgs/report_gs_sm.jpg "Gray-scaled image"
[image_bl]: ./writeup_imgs/report_bl_sm.jpg "Blurred image"
[image_grad]: ./writeup_imgs/report_grad_sm.jpg "Image gradient"
[image_roi]: ./writeup_imgs/report_roi_sm.jpg "Image region of interest"
[image_lines]: ./writeup_imgs/report_lines_sm.jpg "Image with tracked lines"
[image_final]: ./writeup_imgs/report_final_sm.jpg "Output image"
