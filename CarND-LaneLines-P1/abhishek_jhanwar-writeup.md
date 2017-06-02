Abhishek Jhanwar       June 1, 2017
CarND-LineLanes-P1 WriteUp

Introduction
    This project is about finding the lane lines on the road. We use ImageProcessing techniques with OpenCV library and python to find the lane lines on a static image, and the result is then extended to work for a video which is essentially a series of frames. We describe the approach to tackle the problem and various weeknesses of the solution is described as well.
    
Pipeline
    We will describe the pipeline to detect lane lines for an image and then will show how that approach can be applied for a video.

1. Preprocessing
    To get best results from CannyEdgeDetection algorithm, we first pre process the image a bit. 
    a) GrayScale -> Convert the image to grayscale so as to convert it to 0,255 bytescale. Grayscale (i.e. intensity) is usually sufficient to distinguish such edges, it simplifies the complexity and speeds up the process.
    b) GaussianBlur -> This step is convolving an image with gaussian function, results in producing the bokeh affect (focus on center and blurred out edges). Acts as a low pass filter (reduces the image's high frequence components).
After this cannyEdgeDetection algorithm is applied and we obtain edges (set of points across the entire image)

2. CannyEdgeDetection
    Then CannyEdge function is applied and returns edges (points of interest) in the image. Result shown as below:
![alt text](result-edgeImage.jpg)

3. Region Masking to obtain Area of Interest
    Since Camera is mounted at the front of car, it can only capture a region, so we need to find lane lines only in that region. We define a polygon (quadilateral in this case) and only keep the region of the image defined by the polygon
formed from `vertices`. The rest of the image is set to black.
    
4. Hough Transform
    Once we get our set of edges (set of points) in our area of region, we need to connect these dots. But dots can be connected in many ways, so we need to find the model of the line such that these dots can fit there. This retuns set of lines based on the threshold and other parameters passed to it. Threshold params are set as
    threshold = 20     # minimum number of votes (intersections in Hough grid cell)
    minLineLength = 5 #minimum number of pixels making up a line
    maxLineGap = 15 
    So as to remove the lines outside the region of interest and noise.
![alt text](result-HoughImage.jpg)
    
5. Draw Lines
    From hough transformation we got a bunch of lines and when we draw them on image, we find that these are kind of broken. Here we try to average and/or extrapolate the line segments we've detected to map out the full extent of the lane lines.
    To extrapolate the lines, we will separate the left lines and right lines based on slope (y2-y1)/(x2-x1)
    a) If slope > 0 it is part of the right line and if slope < 0 it is part of left line, because 
    coordinate system y is positive downwards.
       i) two separate lists of these slopes are maintained, and is averaged out to retrieve average slope for left 
    line and average slope for right line.
    
    b) Based on the slope, we also maintain the center points ((x1+x2/2),(y1+y2)/2) of each line in left and right arrays.
        i) But we also separate the outliers to ignore the noise. If the mid points of these lines go beyond a certain threshold, we ignore those points. The threshold value is chosen to be half of the image width (imshape[1]).
        
    c) After getting left and right arrays, we average over two arrays (array containing mid points of left line and array containing mid points of right line) so as to get a Average Mid point of the left line and right line respectively. 
    np.divide(np.sum(leftLineMidPoints,axis=0),len(leftLineMidPoints));# axis =0 ensures it is done tuple wise
    
    d) So now we have the slopes of both left line and the right line and a point lying on each line. y=mx+c, thus we can get the intercept. Still we need to find the two extreme points such that we are able to extrapolate the lane line. 
        i) LeftLine : Left line intersects y axis at (x,heightImage) and we do have height of image (imshape[0]), hence the y coordinate. We get the leftLineLowerX Coordinate -> Y{imshape[0]} = AverageSlopeLeftLine*X + C which we obtained in previous step.
            - To get the other extreme of the left line, we use the fact that camera is mounted on the front of the car and the region it can access is limited (Pt 2). We marked the vertices (regionofinterest) to have Y coordinate as 320 for our case beyond which car wont be able to see (farthest the car can see). So we use this Y coordinate as the extreme point on the left line. Now with the slope and Y coordinate, we get the leftLineUpperX coordinate as well.
            - So left line extreme points become (leftLineLowerX, imshape[0]{height of image}) and (leftLineUpperX, 320{region of Interest - farthest the car can see})
        ii) Right Line: To get the extreme points of the right line, we again use the intution that right line will also pass through (x, imshape[0]). Now with the Y Coordinate and Slope we can get the rightLineLowerXCoordinate.
            - Other extreme is obtained in the similar manner where we use the fact that car's view is limited and the 
            extreme end it can see has to be regionOfInterest(320). With the Y coordinate and slope we get the other 
            X coordinate.
            - Points are (rightLineLowerX, imshape[0]), (rightLineUpperX, regionOfiInterest)
    e) Now we have extreme points for both the lines, this allows us to draw the lines on the image and this represents the lane lines.
![alt text](result-LaneLineImage)

6. Video: Now we need to draw the lane lines in a video stream. Video stream is just a series of frames, and we do have a way of marking lane lines on a frame. Thus we apply the algorithm on each image and thus obtain a video stream with lane lines marked.


Shortcomings 
1. Have divided the lines into left and right based on the slope, but if the video has curves (roads with curves) and direction of curve changes i.e. first road is curved inside (road coming inwards) and then it goes outside (road going outwards), this solution would fail and it wont be able to plot the lane lines properly.

2. To eliminate the noise, have used the threshold values for X Coordinates to be half of the image Width size. But this again fails when the road has curves and image is taken at a steep curve. Left line may go beyond width/2. 

3. Have taken the mean values for the slopes and the center points, but some outlier (even after eliminating noise) can skew the results a lot. This limitation is probably the reason for Shortcomings 1 and 2.

4. Have fixed the threshold values of region of interest based on the input images, and have hardcoded them. I believe this is not robust and might fail with different kind of frames/images.

5. Does not work well for the challenge, the reason being the extrapolation algorithm is not robust and the way I obtain the extreme points does not work well. As the orientation of image changes, the system does not work well enough.

6. Our solution does not differentiate between the white lanes and the yellow lines. It treats them as same but in the real world this can have major implications and system would fail miserably while changing the lanes. It might go on the wrong side of the road. 

Improvements
1. As far as threshold values are considered for the slopes and points for the line, mean is not necessarily the right tool. Better statistical techniques like median, mode can be used.
    - Also outlier which are decided based on hardThreshold, can be decided based on standard deviation. If a point goes beyond mean +- standard deviation, it should not be considered. 
    - Or may be clustering algorithms can be used to divide the data in left cluster and right cluster. Elimiate the noise using KMeans and then our algorithm can be used. It is little bit more robust then our current solution and eliminates noise and thus results wont be that skewed.
   
2. Region of interest threshold is hardcoded, this is not a good way. I dont essentially know the right way, but something like this might work:
    - This approach is still good as long as road is straight and the area the camera can capture is kind of fixed. But with the curves and all probably we should reduce this threshold value, but then the challenge is to determine the curves which probably can be determined based on the slope values. Just a rough idea, but I am pretty sure something can be done about this.
    
