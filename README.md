# **Lane Line Detection Pipeline** 

### Completed for Udacity Self Driving Car Engineer - 2017/01/03

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:
* Make a pipeline that finds lane lines on the road
* Reflect on your work in a written report


[//]: # (Image References)

[image1]: ./ReportImages/grayscale.png "Grayscale"
[image2]: ./ReportImages/canny.png "Canny"
[image3]: ./ReportImages/hough.png "Hough Transform"
[image4]: ./ReportImages/Cropped.png "Cropped and Overlay"

---

### Reflection

### 1. Describe your pipeline. As part of the description, explain how you modified the draw_lines() function.

I conqured the assignment in two steps, first by constructing a pipeline able to detect lane markings on road succesfully and second by combining the markings into solid line segments. 

#### Pipeline
The pipeline is constructed in a similar order to the lessons. First the image is converted to grayscale, blurred using a gaussion filter and then converted to edges with Canny Edge Detection. Parameteres were tuned to extract crisp edges exposing the lane markings.  kernel = 5, low threshold = 75, high threshold = 180.

![alt text][image1]
![alt text][image2]
 
Next, the Hough Transform is applied to the edge detected image. Again these parameters were finely tuned across all sample images to accuarely draw lines along the lane markings: rho = 1, theta = math.pi/180, threshold = 15, min_line_len = 30, max_line_gap = 20.

![alt text][image3]

Lastly the Hough Transform image is cropped and overlayed on the original image using the supplied function addWeighted(). The cropping area was tuned using the sample images and initially hardcoded.  However in order for the pipeline to perform on the *optional challenge*, I used some basic ratios to scale the area based on larger image dimensions. Final result looked something like :

![alt text][image4]

#### Modifying draw_lines():

In order to draw a single line on the left and right lanes, I modified the draw_lines() function by averaging the lines that make up the lane markings and drawing this new line on top of the image. First I filtered out the line segments that were not the lane marking using a series of filters. For each line, the slope, intercept, and location of the line had to meet specific criteria in order to qualify as a lane marking and pass the filter. These criteria were tuned using the sample images, and were hardcoded into draw_lines() because of the fixed position of the camera ( this would need to be adjusted if the footage was from a different vehicle ). Then I took the average slope and intercept and generated a single line that is overlayed on the image.



### 2. Identify potential shortcomings with your current pipeline


One potential shortcoming would be if the vehicle was driving at night as it was not tuned to such low contrast between lane marking and road surface. Improved tuning of the algorithm would be required. In the case of snow, this pipeline would stand no chance.

Another shortcoming is that the inside lane marking appears choppy because when the pipeline cannot find a lane, as no line is drawn. This sometimes occurs on the hatched lane markings on the inside of the highway. An improvement here would be to spend more time tuning the parameters and to add smoothing to the line segments between consequtive frames.


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to increase the number of lines or change the model used to extrapolate the lane marking. For instance, a solid line does not track the lane on a curve that well, whereas a quadratic would be more effective.

Another improvement would be to add the ability to detect two solid double lines which are found on a divided highway here in Canada.


In conclusion, I am satisfied with the performance of my algorithm given how well it manages the sample images and videos.  Moving forward, certainly more effort would be required to tune to parameters in the pipeline and the draw_lines() function to be more robust.
