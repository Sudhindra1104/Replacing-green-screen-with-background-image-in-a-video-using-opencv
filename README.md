# Replacing green screen with background image in a video using opencv

Hi, I made this project during the 7 day long bootcamp on OpenCV conducted by **DevTown** and we were instructed by **Mr.Shaurya Sinha** (Ex-Software Engineer and Data Analyst at Fastenal and Jio).

During the course of the week long bootcamp, I managed to refresh on my Python fundamentals and NumPy concepts along with a nice dive into computer vision as we developed this project using OpenCV.

**OpenCV** is a library of programming functions mainly aimed at real-time computer vision. It is a great tool for image processing and computer vision tasks. It is an open-source library that can be used to perform tasks like face detection, object tracking, landmark detection and much more.

This simple project aims at removing the green screen in a video file and replacing the green screen area with a custom background image of our choice. 

To begin with, we will need a video file that contains the green screen video. Second, we will be needing an image to insert into the background replacing our green screen.

In this case, I have chosen the following video and image:

https://user-images.githubusercontent.com/100610040/181185172-979b4230-06f0-422e-824a-d3d698c97ee0.mp4

![road](https://user-images.githubusercontent.com/100610040/181185230-8eff645c-aa27-4a63-97d6-81378da7b0cc.jpg)

Initially, we start off with loading in our image and video file into separate variables. We can use `cv2.VideoCapture()` and `cv2.imread()`. We open up a while loop next. Now we proceed to create our first frame. Using `ret, frame = vid.read()`, we check whether there is valid video content being received to the frame. Else, the loop terminates. Now we can output the frame using `cv2.imshow()`. We use `cv2.waitKey()` to show the image for a fixed amount of time (takes in milliseconds as argument) and then automatically closes. Now we also have an `if` condition where we check that `if k == ord('q'):`. Here `ord()` converts the value of 'q' to unicode. If the condition is satisfied then we will break the loop. This allows the windows to get closed upon pressing the 'q' key. We use `vid.release()` and `cv2.destroyAllWindows()` to close and destroy all windows once 'q' is pressed.

![image](https://user-images.githubusercontent.com/100610040/181189611-9efa338d-5726-4609-a92a-d597888988d1.png)

This is what your inital frame is going to look like.

We want to convert the colors in our frame from BGR (Blue Green Red) to HSV (Hue Saturation Value) as HSV is more robust towards lighting changes and can eliminate image luminance from color information. This is mainly done to eliminate bright or dark areas from out original green screen area. We use `cv2.COLOR_BGR2HSV)` which is part of `cv2.cvtColor()`.

Now we need to separate the green color from the video. For this we create a mask. This mask should be able to mask the green color from the video and take it away from the video. For this we specify the color range using HSV values two NumPy arrays `l_g` and `u_g` which hold the lower and upper values of the green color (incl. blacks and whites). 

Our mask will look somewhat like this:

![image](https://user-images.githubusercontent.com/100610040/181193553-6f965359-d270-4a14-a629-09e03c023198.png)

Right now we want to place this mask onto our image as well. So the mask is black and white. Black indicates the value is 1 and white indicates that the value is 0. There is no other value in the mask.

To get the result of the mask being placed over our image, we need to perform `cv2.bitwise_and()` between the original frame and our mask. The gives the resultant frame which consists of our mask being interpolated onto our image.

So, this is how our resultant is going to look like:

![image](https://user-images.githubusercontent.com/100610040/181194538-d21d9d5d-d698-4067-a84a-3ccf71bdef21.png)

So we have identified the entire green screen and removed that Camaro from the frame.

Now our aim is to delete the green screen and keep the Camaro. How do we do that?

We just need to subtract the original the frame and the resultant. Remember that all of these images are just a set of matrices that contain pixels in the form of columns and rows.

So we create a new variable where we store the new frame that is the result of subtracting the original frame and the resultant.

![image](https://user-images.githubusercontent.com/100610040/181195527-0bee1e47-50c7-4e15-8d90-c9dd95cb0d40.png)

So now we can see that the entire green screen has been removed. So in the black area that remains, we need to insert our background image of choice. The black areas are represented by zero matrices. Wherever the value of the matrices are not null, we have to retain the original frame.

We use `np.where()`. This basically says wherever there are black areas in the frame, we replace it with the image content. Otherwise, we retain the original frame. We assign this to a separate variable and then display it as our final output which will look like this:

https://user-images.githubusercontent.com/100610040/181200936-dd77b39e-6b21-425b-bb76-3321f075fbca.mp4

So we have successfully interpolated our preferred background image replacing our green screen in the video!

You can also try the same with any green screen video and image file and view similar results.
