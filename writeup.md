# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

The goals of this project , according to me , involved not just a task of building a pipeline but to provide a chance to play out the concepts imparted in term 1. 

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

The pipeline I have implemented follows the following scheme:

#### PIPELINE SCHEME:

1 - Convert the image to grayscale.
2 - Apply Canny.
3 - Apply Gaussian smoothing.
4 - Create a trapezoidal region-of-interest mask.
5 - Apply Hough transform.
6 - Manipulate Hough lines.
7 - Draw extrapolated lines.

I applied Canny function before Gaussian smoothing with the intention of preserving very minute lane lines which might otherwise be lost while smoothing and subsequent transform. Step 1 to Step 4 is actually pretty straightforward and I implemeted it using almost the same values as used in term quizzes.

The really interesting part is the task of actually extrapolating lines obtained after applying the Hough transform in the draw_lines() function. I tried two approaches for the same.

1 - The first approach that I tried basically turned out to be a one which wasn't very flexible and hardly suitable for a production environment. It involved storing the x1,y1 values for left and right lanes in two+two arrays and the x2,y2 values again in the same way. Then I tried a cv2 where i provided explicit x1,y1 and x2,y2 values for left and right lane based on min and max values from these four arrays that i created. While, I could individually tune the values to obtain a valid result for a specific test image, this is not the point of the pipeline.

2 - The second approach and the one that i used (and the one which i have finally implemented), is using 'np.polyfit' function to determine the best fit line for left lane and right lane (x,y) values. The values were separated based on the simple concept of slope being greater or less than 0. Here an important point that i observed is , the slopes with form - dy/0 needed t be cleaned from the (x,y) values as they often caused the result to be completely out of range. 

The result obtained was as follows:

![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-solidWhiteCurve.jpg)
![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-solidWhiteRight.jpg)
![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-solidYellowCurve.jpg)
![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-solidYellowCurve2.jpg)
![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-solidYellowLeft.jpg)
![alt text](https://github.com/arpitsri3/carNDarpit/blob/master/test_images_output/output-whiteCarLaneSwitch.jpg)



### 2. Identify potential shortcomings with your current pipeline


The major shortcomings that I see in the code are as follows:

- There is no consideration for changing lighting conditions on road which will effect resolution.
- There is no consideration for changing elevation. Say, a road bump can cause the lane line detection to be wrong (as I noticed          occasionally in the video output)
- There is much room for improvement for non-linear lane detection. The 'np.polyfit' method used is for a simple y=mx+b in this project which is definitely not going to be the case ina real life self-driving car scenario.


### 3. Suggest possible improvements to your pipeline

As discussed in the previous section there is major room for improvement that I feel especially for the 3rd point. I have been parallely exploring the points mentioned in the last section and hope to work on them in the upcoming projects.
