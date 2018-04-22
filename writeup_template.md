# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.
The pipleline is as following : 
1. covert image to grayscale
2. apply gaussian blur with kernel == 5
3. apply canny edge detection, low_threashold == 50 and high_threashold == 150
4. mask out unwanted area
5. fine hough lines
6. combine the found lines with the original image.

There is one thing I learned the most along the process : simplicity is the key. I started by using the length of the segments to give different segments different weights, which needed many extra efforts but the result was suboptimal. After several trials and errors I realiazed the length was computed by the points of the segment which is an intrinsic information the segments carrary, therefore by averaging the segments would naturally give the same (and better) result as the weighing approach. 

Another thing I noticed was the importance of historical data. If we process each frame individually, we are neglecting the valuable information from previous frames. Each frame is not independent, therefore by including information from previous frames we increased the stability of the system, in other words, we redueced the disturbance of the noise in any single frame.

To achieve what we discussed above, I added a new argument for historical data to the draw_line function. Inside the draw_line function:
1. we first mix the current lines with historical data.
2. Second, we put these lines into 2 groups : left and right. 
3. Compute the average line of each line groups.
4. Given the computed left and right line, extrapolate the lines.

### 2. Identify potential shortcomings with your current pipeline
There are several potential shortcomings of my current apporach :
1. Lines are seperated by a hardcoded value, which wouldn't be able to handle turns.
2. The noisy data close to the region of interest has noticeable effect to the detection.

### 3. Suggest possible improvements to your pipeline
1. The complete historical data could be further reduced into line averages.
2. Instead of a straight line, more degree of freedom might help identifying curves. 
