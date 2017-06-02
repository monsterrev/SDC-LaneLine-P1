#**Finding Lane Lines on the Road** 
[![Udacity - Self-Driving Car NanoDegree](https://s3.amazonaws.com/udacity-sdc/github/shield-carnd.svg)](http://www.udacity.com/drive)

<img src="examples/laneLines_thirdPass.jpg" width="480" alt="Combined Image" />

Overview
---

When we drive, we use our eyes to decide where to go.  The lines on the road that show us where the lanes are act as our constant reference for where to steer the vehicle.  Naturally, one of the first things we would like to do in developing a self-driving car is to automatically detect lane lines using an algorithm.

In this project you will detect lane lines in images using Python and OpenCV.  OpenCV means "Open-Source Computer Vision", which is a package that has many useful tools for analyzing images.  

To complete the project, two files will be submitted: a file containing project code and a file containing a brief write up explaining your solution. We have included template files to be used both for the [code](https://github.com/udacity/CarND-LaneLines-P1/blob/master/P1.ipynb) and the [writeup](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md).The code file is called P1.ipynb and the writeup template is writeup_template.md 

To meet specifications in the project, take a look at the requirements in the [project rubric](https://review.udacity.com/#!/rubrics/322/view)


Creating a Great Writeup
---
For this project, a great writeup should provide a detailed response to the "Reflection" section of the [project rubric](https://review.udacity.com/#!/rubrics/322/view). There are three parts to the reflection:

1. Describe the pipeline

2. Identify any shortcomings

3. Suggest possible improvements

We encourage using images in your writeup to demonstrate how your pipeline works.  

All that said, please be concise!  We're not looking for you to write a book here: just a brief description.

You're not required to use markdown for your writeup.  If you use another method please just submit a pdf of your writeup. Here is a link to a [writeup template file](https://github.com/udacity/CarND-LaneLines-P1/blob/master/writeup_template.md). 


The Project
---

## If you have already installed the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) you should be good to go!   If not, you should install the starter kit to get started on this project. ##

**Step 1:** Set up the [CarND Term1 Starter Kit](https://classroom.udacity.com/nanodegrees/nd013/parts/fbf77062-5703-404e-b60c-95b78b2f3f9e/modules/83ec35ee-1e02-48a5-bdb7-d244bd47c2dc/lessons/8c82408b-a217-4d09-b81d-1bda4c6380ef/concepts/4f1870e0-3849-43e4-b670-12e6f2d4b7a7) if you haven't already.

**Step 2:** Open the code in a Jupyter Notebook

You will complete the project code in a Jupyter notebook.  If you are unfamiliar with Jupyter Notebooks, check out <A HREF="https://www.packtpub.com/books/content/basics-jupyter-notebook-and-python" target="_blank">Cyrille Rossant's Basics of Jupyter Notebook and Python</A> to get started.

Jupyter is an Ipython notebook where you can run blocks of code and see results interactively.  All the code for this project is contained in a Jupyter notebook. To start Jupyter in your browser, use terminal to navigate to your project directory and then run the following command at the terminal prompt (be sure you've activated your Python 3 carnd-term1 environment as described in the [CarND Term1 Starter Kit](https://github.com/udacity/CarND-Term1-Starter-Kit/blob/master/README.md) installation instructions!):

`> jupyter notebook`

A browser window will appear showing the contents of the current directory.  Click on the file called "P1.ipynb".  Another browser window will appear displaying the notebook.  Follow the instructions in the notebook to complete the project.  

**Step 3:** Complete the project and submit both the Ipython notebook and the project writeup


Review : https://review.udacity.com/?utm_medium=email&utm_campaign=reviewsapp-submission-reviewed&utm_source=blueshift&utm_content=reviewsapp-submission-reviewed&bsft_clkid=703b155c-2e38-4391-bfb8-206d48b95f4d&bsft_uid=1a37e52b-3d02-4a4b-9cf7-fb20ecdcd96e&bsft_mid=326246b8-e986-4881-a4bd-b7b07044c082&bsft_eid=6f154690-7543-4582-9be7-e397af208dbd&bsft_txnid=90f59c85-bffd-4d00-9178-d562513d3cf3#!/reviews/530964

Meets Specifications

Really nice job with this submission. Your project meets all the specifications. Congratulations on passing the first project in the Self Driving Car Nanodegree on your very first attempt and best of lucks in the ones ahead.
Cheers and Keep up the good work !!!

Here are some links for further improvements:
https://www.youtube.com/watch?v=hnXkCiM2RSg&feature=youtu.be
https://www.python.org/dev/peps/pep-0008/
http://stackoverflow.com/questions/36598897/python-and-opencv-improving-my-lane-detection-algorithm
Required Files

The project submission includes all required files
Lane Finding Pipeline

The output video is an annotated version of the input video.
Your code outputs video with annotations. Nice job !
In a rough sense, the left and right lane lines are accurately annotated throughout almost all of the video. Annotations can be segmented or solid lines
The left and right lanes are accurately annotated on the videos.
Good job !
Suggestions & Comments

To center even more your annotations on the actual lane lines, I recommend you try tuning the values of your parameters especially the threshold , min_line_length and max_line_gap.
Examine the parameters by modifying them separately. That will allow you to identify how each parameter affects the lane line. For example:
max_line_gap defines the maximum distance between segments that will be connected to a single line.
min_line_len defines the minimum length of a line that will be created.
Increasing these parameters will create smoother and longer lines
threshold defines the minimum number of intersections in a given grid cell that are required to choose a line.
Increasing this parameter, the filter will choose longer lines and ignore short lines.
These resources ( 1 & 2 & 3 ) might provide further intuition on the subject here
Visually, the left and right lane lines are accurately annotated by solid lines throughout most of the video.
Both left and right lines are each a single solid line (i.e. not segmented) generally centered on the actual lane lines
Here's an algorithm of the process
2cce0b10-bab0-11e6-8ec1-0242ac110120.jpg
Reflection

Reflection describes the current pipeline, identifies its potential shortcomings and suggests possible improvements. There is no minimum length. Writing in English is preferred but you may use any language.
Current pipeline is well described
Possible shortcomings and potential solutions have been provided.
Good job !
Suggestion

As far as curves are concerned on the challenge video, I will encourage you to read this documentation:
http://airccj.org/CSCP/vol5/csit53211.pdf

