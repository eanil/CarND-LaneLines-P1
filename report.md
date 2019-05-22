# **Finding Lane Lines on the Road** 

---

**Finding Lane Lines on the Road**

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I implemented the following approach for this project:

First I used the techniques described in the class videos. The basic pipeline has three steps:

   1. Apply Canny edge detector and Hough transform to find lane lines. I operated on grayscale images and limited the detector to the region of interest I hardcoded into the functions based on the image size. Before applying the detector/transform I applied Gaussian noise to the grayscale image. By trial and error I picked the parameters I included in the jupyter notebook.
    
   2. Group the lane lines. Since I know beforehand that we are looking for two lane lines (one on the left and one on the right) I calculated the slope of each Hough line and separated the lines into two groups according to their slopes (i.e., left lane and right lane)
       
   3. Get rid of noisy lines and extrapolate: Naturally the detector and the transform are also finding line segments that really do not belong to the lane lines. To correct for noisy lane lines, I calculated the median slope and intercept for each lane line. Finally I extrapolated to the edges of the region of interest I picked.

Here is what the test image looks like:

[image1]: ./examples/test_image.png "Grayscale"

![alt text][image1]

The video results are in:

[vid1]: ./test_videos_output/solidWhiteRight_normal.mp4
[vid2]: ./test_videos_output/solidYellowLeft_normal.mp4

./test_videos_output/solidWhiteRight_normal.mp4

![alt text][vid1]

 ./test_videos_output/solidYellowLeft_normal.mp4

![alt text][vid2]

### 2. Identify potential shortcomings with your current pipeline

As soon as I applied the pipeline to videos I noticed that the lane lines are jumpy and not smooth. Tuning the Canny and Hough parameters helped with fixing the jumpiness but I was not satisfied with the results. To smooth the detections I put in a very simple linear Kalman filter as implemented in the notebook. I assumed a constant unknown process noise (which I hoped would handle the car motion and stabilize the detections). The state vector only considers the slope and intercept of the lane lines. I tried to balance the process and the measurement noise to make the detections smooth enough but also follow the measurements such that the detections do not get “stuck” and not follow the actual lane.

The kalman filtered results are shown in:
[vid3]: ./test_videos_output/solidWhiteRight_normal.mp4
[vid4]: ./test_videos_output/solidYellowLeft_normal.mp4

./test_video_output/solidYellowLeft_kalman.mp4

![alt text][vid3]

./test_video_output/solidWhiteRight_kalman.mp4

![alt text][vid3]

Another shortcoming is the technique is very sensitive to image quality and for every lighting and road situation it would need to be tuned (i.e., not robust in general). I would not trust a car that is using this filter as is.
Finally, turns and especially sudden maneuvers may not be handled very well. The Kalman filter may lag too behind. 


### 3. Suggest possible improvements to your pipeline

I would research more robust techniques that are not too sensitive to image quality, colors, etc.

I would also want to test the method on drives that include sudden turns and maneuvers. There are ways to make the kalman filter more adaptive. Improving the filter may help.
