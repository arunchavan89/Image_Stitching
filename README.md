# Image Stitching with OpenCV and C++
This is a project to stitch images with help of OpenCV functions.

## Dependencies

This lab requires:
* OpenCV 3.3.1.
* OpenCV contrubution modules.
* Build OpenCV by doing following steps.
  * use cmake GUI
  * check                     : INSTALL_C_EXAMPLES
  * uncheck                   : BUILD_opencv_Saliency
  * uncheck                   : BUILD_SHARED_LIBS
  * OPENCV_EXTRA_MODULES_PATH : (Opencv_contrib_3.3.1)\modules

## Project: 

First the frames from the 3 video input devices are captured. Then the pairwaise H matrix (homography matrix) is calculated. 

   The H matrix is 3*3 matrix.Homography relates the pixel co-ordinates in the two images.

   When applied to every pixel the new image is a warped version of the original image


   Let us assmue the 3 input frames are I1 , I2 and I3 in the order from left to right.(This assmuption is possible in my case since the camera positions are fixed and I have written the code keeping in mind the which camera will be the leftmost.)

   Now, we calculate the H matrix for image pairs (I1, I2), let us call it H12, and (I2, I3), let's call it H23. Since I1 and I3 will not have a lot of areas in common there is no point in calculating H matrix for these image pairs.


Choose a image as reference image. Among the 3 images presented to us, the image I2 has the most in common with I1 and with I3 (since it is in the center). Hence we chose I2 as the reference image.This is also the reason why we do not compute H matrix for I1 and I3 image pairs, because we only compute the homography of images with the reference image, I2.

Now using our H matrices we wrap the image pairs (I1,I2), now called I12, and (I2,I3), now called I23.

Again calculate the H matrix for images I12 and I23 and wrap them up finally to get the ouptup I123. 


## Implementation:
I have chosen C++ language. 
Calculation of H matrix : 


1. Detecting the KeyPoints in an image using SURF.

    Speeded Up Robust Features.
    This detects the keypoints in the images.
    SURF is a local feature detector and descriptor. <a href = "http://docs.opencv.org/3.0-beta/doc/py_tutorials/py_feature2d/py_surf_intro/py_surf_intro.html"> Introduction to SURF </a>

    To improve the feature detection you can change the variable value *minHessian*, which is the **Hessian Threshold**. 
  
    To increase the number of keypoints reduce the Hessian Threshold.

2. Calculating Descriptors :
   This computes and extracts the decriptors from the images using the keypoints from the previous step.

3. FLANN Matching :
   Fast Library for Approximate Nearest Neighbors  finds the best matches for local image features [3].

4. Filter, allowing only the good matches and find the correspoding keypoints.

5. Finds a perspective transformation between two planes using RANSAC

   RANdom SAmple Consensus (RANSAC) is a general parameter estimation approach designed to cope with a large
   proportion of outliers in the input data. 

