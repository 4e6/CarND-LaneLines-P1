# **Finding Lane Lines on the Road**

## Pipeline

Pipeline contains six following steps:

0. Original image

![original][original]

1. Convert image to grayscale and turn down brightness for better contrast
   between bright lane lines and darker road.

![grayscale][grayscale]

2. Apply Gaussian blur to smooth edges.

![gaussian blur][gaussian_blur]

3. Apply the Canny transformation to get the edges.

![canny][canny]

4. Apply region of interest mask to focus on the lane lines.

![region][region]

5. Apply Hough algorithm to identify lines in the image

![hough][hough]

6. Overlay with original image

![combined][combined]

The first video `test_videos_output/solidWhiteRight.mp4` shows lines produced by
Hough algorithm.

To draw straight lines, I modified `draw_lines()` function. I calculated the
slope of the lines and separated them on left and right by the sign of the
slope.

```
ind_right = np.apply_along_axis(lambda row: slope(row) > 0, 2, lines).flatten()
ind_left = np.apply_along_axis(lambda row: slope(row) < 0, 2, lines).flatten()
```

Then I implemented two functions `average_line_left()` and `average_line_right()`
that averages and extrapolates the lane lines on the image. I used
`numpy.polyfit()` method to get the polynomial of degree 1 that represents the
line `y = ax + b`. Then I solved it for `x` using `numpy.poly1d()` to get the
same length (`y` coordinate) for both lines.

![combined lines][combined_lines]

The second video `test_videos_output/solidYellowLeft.mp4` shows straight lines,
a result of modified `draw_lines()` function.

## Shortcomings

One shortcoming is that algorithm relies on the quality image and that the
bright lines are well visible on the darker surface of the road.

Another shortcoming is that the lines should be in the area of the
interest. What would happen when the car starts changing the lanes.

## Improvements

To improve the current algorithm, we could add some color filters to make lines
more distinguishable on the images of reduced quality.

Also, we could average final lines to a polynomial of higher degree to get
better fit.

## Optional Challenge

The original method did not work for this video. To solve this extra challenge,
I added two additional steps to the pipeline. Before the grayscaling, I added
yellow and white filters to make the lane lines more distinguishable from the
road surface. Surprisingly filters worked pretty well, and you can see the
result in the `test_videos_output/challenge.mp4` video.

[original]: ./writeup/original.png
[grayscale]: ./writeup/grayscale.png
[gaussian_blur]: ./writeup/gaussian_blur.png
[canny]: ./writeup/canny.png
[region]: ./writeup/region.png
[hough]: ./writeup/hough.png
[combined]: ./writeup/combined.png
[combined_lines]: ./writeup/combined_lines.png
