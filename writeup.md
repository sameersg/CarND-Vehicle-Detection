**Vehicle Detection Project**

The goals / steps of this project are the following:

* Perform a Histogram of Oriented Gradients (HOG) feature extraction on a labeled training set of images and train a classifier Linear SVM classifier
* Optionally, you can also apply a color transform and append binned color features, as well as histograms of color, to your HOG feature vector. 
* Note: for those first two steps don't forget to normalize your features and randomize a selection for training and testing.
* Implement a sliding-window technique and use your trained classifier to search for vehicles in images.
* Run your pipeline on a video stream (start with the test_video.mp4 and later implement on full project_video.mp4) and create a heat map of recurring detections frame by frame to reject outliers and follow detected vehicles.
* Estimate a bounding box for vehicles detected.

[//]: # (Image References)
[image2]: ./output_images/hog_images.png
[image3]: ./output_images/sliding_window.png
[image4]: ./output_images/test_images.png


## [Rubric](https://review.udacity.com/#!/rubrics/513/view) Points
###Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---

###Histogram of Oriented Gradients (HOG)

####1. Explain how (and identify where in your code) you extracted HOG features from the training images.

The code for this step is contained in the fourth code cell of the IPython notebook.

I started by reading in all the `vehicle` and `non-vehicle` images.
I used the function `single_img_features` shown in the class to get car_features and car_hog_img as a return images. 

I then explored different color spaces and different `skimage.hog()` parameters (`orientations`, `pixels_per_cell`, and `cells_per_block`).  I grabbed random images from each of the two classes and displayed them to get a feel for what the `skimage.hog()` output looks like. 


####2. Explain how you settled on your final choice of HOG parameters.

After trying out differnt options i found out that YCrCb was workring well for HOG features and orient 9, pix_per_cell = 6, cell_per_block = 2, hog_channel = All
spatial_size = (32, 32) and hist_bins = 32 were good option to get a distinctive Hog image. So i stick with it. -> Later in trainig it showed me it was a good choice it gave me a accuracy of 0.9918. 

Here is an example of the images :
![alt text][image2]


####3. Describe how (and identify where in your code) you trained a classifier using your selected HOG features (and color features if you used them).
Code for this is in cell 7. 
I trained a linear SVM using: 9 orientations, 6 pixels per cell, 2 cells per block, ALL channels, 32 histogram bins, (32, 32) spatial size, and YCrCb colorspace. Since all 3 Hog channels seams signifikant i choose All of them.
Feature vector length was 11916 and Test Accuracy of SVC = 0.9918.
   

###Sliding Window Search

####1. Describe how (and identify where in your code) you implemented a sliding window search.  How did you decide what scales to search and how much to overlap windows?

I decided to search from y 400 to 656 beacaus that was the main are where Vehicle were, i don't need to search the sky. ;) I used two Scales 1.3 and 1.5

![alt text][image3]

####2. Show some examples of test images to demonstrate how your pipeline is working.  What did you do to optimize the performance of your classifier?

Ultimately I searched on two scales 1.3 and 1.5 using YCrCb 3-channel HOG features, which provided a nice result.  Here are some example images:

![alt text][image4]
---

### Video Implementation

####1. Provide a link to your final video output.  Your pipeline should perform reasonably well on the entire project video (somewhat wobbly or unstable bounding boxes are ok as long as you are identifying the vehicles most of the time with minimal false positives.)

Here's a [link to my video result](./output.mp4)


####2. Describe how (and identify where in your code) you implemented some kind of filter for false positives and some method for combining overlapping bounding boxes.

The cell 29 contains the final pipeline with a buffer for the heatmaps, so they can be summed up over several frames. The summed heatmap is used as threshold for false postitiv. After triying out buffer 20 and thresold 30 give me good result for false postives. 


---

###Discussion

####1. Briefly discuss any problems / issues you faced in your implementation of this project.  Where will your pipeline likely fail?  What could you do to make it more robust?

Choosig also the x coordinates would decrease the numer of windows a could also reduce false postives, like detectiong veheicles on the other side of the lane. Also a better windows function to search only for Cars where cars are found in the last frames would make the system better. Not sure but Deep-learning could make it much better? 

