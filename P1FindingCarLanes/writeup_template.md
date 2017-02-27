#**Finding Lane Lines on the Road** 


**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./examples/grayscale.jpg "Grayscale"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

My pipeline consisted of the following steps.
 1. First I convert the image into grayscale.
 2. Then I smooth the image using gaussian blurring kernel of size 5.
 3. Use canny edge detection with lower threshold and higher threshold to get the edges.
 4. Now mask the edges using region of interest code that uses cv2.fillPoly
 5. Use Hough transform with parameters to run hough transform on the masked edges/
 6. Now using the hough lines , extract possible lanes.
 7. Now extrapolated the lines using  extrapolated_draw_lines() to get the full extent of lanes.
 8. Combined the extrapolated lanes with image to see results.



### Explanation of Modified draw_lines() i.e. extrapolated draw lines
 1. First I get all the possible lines from hough_lines method.
 2. Now for each line i calculate slope and then group lines into left and right depending upon magnitude of slope.
 3. Now if I have less than 1 lines then I replace the lines by previous frame's hough lines.
 4. Now with all given lines,I fit the lines of left and right to the best fit using numpy's poly1d(Least square method)
 5. Now that we have the slopes, I find the top y - co-ordinate using min of all y's in that line and also set max y co-ordinate to image height. Now set the y co-ordinates and slope from the least square fit we get corresponding x co-ordinates.
 6. Now using average of previous 30 frames , we extrapolate the lines using avg of current line and previous frame information.




### Identify potential shortcomings with your current pipeline

Shortcomings
1. Whenever the scene changes drastically from the previous frames my pipelines fails badly.
2. Also there are some jitters in my pipeline sometimes, where the direction of lanes change quickly.
3. In case of quickly alternating or curvy lanes, my pipeline will fail. What i mean is it's not very generic.
4. The yellow lanes are not marked very nicely . They are a bit off. ROI was hard-coded.
    

    

###  Suggest possible improvements to your pipeline

1. More time was to be spent on tuning hough parameters.
2. I could improve my averaging method to obtain a smoother transition of lanes.
3. COuld have used some other techniques to allow yellow lanes to be detected.