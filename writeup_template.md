**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


### Reflection

My pipeline consisted of 5 steps. First, I converted the images to grayscale. Second, I applied Gaussian Blur. Third, I applied the Canny edge detection to find the edges of shaps in the image. Fourth, to only consider the region of interest (which only contains my lane) I masked the image containg the edges by a polygon of size four. Fifth, since from the edges found, I am only interested in lines, I applied Hough transform to find the lines. Finally, I draw the lines on the original image.

In order to draw a single line on the left and right lanes, I modified the draw_lines() function as follows:

* I find the slope of all lines found by Hough transform.
* If slope is negative, I consider the line belonging to my big left line, otherwise belonging to my big right line.
* To find endpoints of my big left line, I find the bottom point as the point with largest y value and smallest x value among all left lines. I find the top point as the point with smallest y value and largest x value among all left lines.
* To find endpoints of my big right line, I find the bottom point as the point with largest y value and largest x value among all left lines. I find the top point as the point with smallest y value and smallest x value among all right lines.
* I extend the big two lines found above to the bottom of the image.
* To kill outliers, I ignore left lines that are not entirely on the left side of the image.
* Similarly, I ignore right lines that are not entirely on the right side of the image.


### Shortcoming


One potential shortcoming would be what would happen when the road is too curvy in the middle. Then one line from bottom to top may not be a good approximation.

Another shorcoming is that I am ignoring the width of the lane lines.


### Suggested Improvements

* For first shorcoming, we can approximate by multiple connected lines rather than jist one line.

* For second shortcoming, we can find two left lines and use fillPoly to fill red between two lines. Similar thing can be done for two right lines. To do this, for top points of left lines, we find the two largest x values among all left lines instead of just finding the largest x value. Similar thing can be done for other endpoints.

* A major improvement can be done as follows:

Here we are processign each image in isolation of other images. We can figure out the mean slope of the previous images, and then force our current image lines slope not to diverge from it too much.