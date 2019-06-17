# Finding Lane Lines on the Road
## https://medium.com/@patrickhk/self-driving-car-basic-car-lane-line-detection-a784be871157

Perception is one of the most important modules of self driving car. Currently I have enrolled the Udacity Self Driving Car Engineer Nanodegree and the first project is to build a basic car lane detection with the help of OpenCV, Canny detection and Hough Transform. Let’s see the result<br/>

## My Result
![p1](https://cdn-images-1.medium.com/max/800/1*F269ksHcBaJht-u-0x23vw.png)<br/>
![p2](https://cdn-images-1.medium.com/max/800/1*A8jeg0K8G0YwmmXLY3lTJA.png)<br/>
https://www.youtube.com/watch?v=TlrkfVtzdeg<br/>

### Read the image into np array
We have to read and turn the image file into numpy array with the help of cv2.imread or mpimg.imread . Note the color channel of cv2 and matplotlib are different<br/>
![p3](https://cdn-images-1.medium.com/max/800/1*KTyo8dP9Wmz_XVmnseWt9g.png)<br/>


### Turn the image into gray scale for Canny detection
We use cv2.cvtColor function to turn the img array into gray scale as Canny detection read gray scale img. Since I read image by using matplotlib therefore I will use cv2.COLOR_RGB2GRAY<br/>

### Apply Canny detection
It is optional to apply Gaussian smoothing before canny detection as it is already embedded in the cv2.Canny function. I use the parameter low_threshold=50, high_threshold=150, the low:high ratio is suggested to be 1:3 as the rule of thumb to get the edge image.<br/>

![p4](https://cdn-images-1.medium.com/max/800/1*9w_gk0kmkn3ThRMbaQcC2g.png)

### Create region of interest
As you can see there are many edges detected by Canny detection but this is actually not we want as it contains many noises, such as the shape of the tree and mountain. We use cv2.fillPoly to create a region like this.<br/>
![p6](https://cdn-images-1.medium.com/max/800/1*yppTW0xweOIYxi5gsOaBtA.png)
Then we apply masking, which is to select the pixel where got “activated” by canny detection and within the above region. As a result we got this<br/>
![p7](https://cdn-images-1.medium.com/max/800/1*JdefwSgu68vFYUTKcKelwg.png)

### Hough transform and draw lane line
The edge we got from Canny detection actually are many dots. We apply cv2.HoughLinesP function to get “lines”. I use the following parameters, rho = 2,threshold = 50, min_line_length = 100 and max_line_gap = 160.<br/>

Lines are actually a list of many x1,y1,x2,y2 coordinates. With all these line coordinate, we can iterate and find the best first order polynomial line by using np.polyfit. Then we use cv2.draw to draw the two lines(left lane and right lane). We can apply some filtering criteria such as slope and angle to help removing noise.<br/>

![p8](https://cdn-images-1.medium.com/max/800/1*urZy5hH-ZqzQhNUZdp9AYQ.png)
### Stack the draw lane with the original img
Then what we do is to stack the lane line in the original image with the help of cv2.addWeighted function.<br/>
![p10](https://cdn-images-1.medium.com/max/800/1*AvIvQ9_cAQOql6Mq35FYBA.png)
### Apply to video input
Same principle but different source input. We can use the VideoFileClip function from moviepy. Then we can save the output video and visualize it within jupyter notebook.<br/>
![p11](https://cdn-images-1.medium.com/max/800/1*6_yvgKvXNXx_sh37YVakYA.png)

-------------------------------------------------------------------------------------------------------------------------------------
### More about me
[[:pencil:My Medium]](https://medium.com/@patrickhk)<br/>
[[:house_with_garden:My Website]](https://www.fiyeroleung.com/)<br/>
[[:space_invader:	My Github]](https://github.com/fiyero)<br/>
