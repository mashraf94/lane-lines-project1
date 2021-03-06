# **Finding Lane Lines on the Road** 
---

### Reflection

The proposed pipeline consists of 8 steps:

##### *The whiteCarLaneSwitch image is used throughout this brief description of the pipeline to resemble each step.*

  1. Took a copy of the image, to avoid affecting the input image.
  
  <p align="center"><img src="./my_examples/whiteCarLaneSwitch.jpg" width="500"></p>
  
  2. Applied the helper function grayscale on the image.
  
  <p align="center"><img src="./my_examples/grayscale.jpg" width="500"></p>
  
  3. Smoothed the image using the Gaussian Blur function on the image using a Kernel Size of 5x5 
  4. Detected the image's edges using the Canny Edge Detection function, with a ratio of 1:2 through a low_threshold @ 100 and a
      high threshold @ 200.
      
      <p align="center"><img src="./my_examples/edges.jpg" width="500"></p>
      
  5. Constructed a polygon mask for every image by determining 4 vertices across the image. Using approximate ratios with respect to
      the image's specific dimensions calculated by variables xsize & ysize.
      
      <p align="center"><img src="./my_examples/mask.jpg" width="500"></p>
      
  6. Transformed the masked part of the image to a Hough Space, using the Hough Lines function, determining the lines connecting the            Edge points previously collected through the Canny function. These lines were filtered according to several parameters which              took a lot of trials and logic to determine, including: the vote threshold of line intersections in a single grid space, the              minimum line length and maximum line gap.
  
  7. These lines are passed to the draw_lines function to be drawn on a blank image with the same dimensions as the input, which was completely rewritten.
      * The points were filtered to two seperate arrays representing the left and the right lanes by dividing the lines on the left               of the image and on the right.
      * Furthermore, I had to use a slope filter to avoid anypoints that might be included within the mask perimeter but aren't lane               lines.
          - For Example: In the challenge video, the car hood disturbed the lines drastically due to lines detected with slopes = 0.
      * The functions polyfit, poly1d and polylines were used to get a fitting set of points for the lines set as the left and right lane, create a function of the first degree for the fitting points, and draw a line across the points specifying the left and right lanes. 
  
  8. Merged the lines drawn and input image together using the weighted image function, to display the drawn lanes marked by *the red lines* on the input image.
  
  <p align="center"><img src="./my_examples/lanes.jpg" width="500"></p>
  
### Potential Shortcomings

The pipeline proposed might have several shortcomings, which would cause indecisiveness in determining the lanes.  
These shortcomings might occur under certain conditions:  
  1. Extremely high or low lighting.
  2. Shadows
  3. Different cameras, and captured portions of the car (eg:. the car's front hood)
  4. Urban Traffic with really close pedestrians or other vehichles.
  
  * All these conditions would might cause a distortion in the pipeline's algorithm since it detects the lines available in a specified region infront of the car, and it might be inaccurate.

### Possible Improvements

Ofcourse, this algorithm needs a lot of improvement, one of the ways is using machine learning to know where the lanes are, instead of hard coding the ratios determining the region of interest. Moreover, we could use convolutional neural networks to detect more precisely the lane location.
