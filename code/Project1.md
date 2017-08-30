# Search and Sample Return Project


This project is modeled after the [NASA sample return challenge](https://www.nasa.gov/directorates/spacetech/centennial_challenges/sample_return_robot/index.html) 
The project used the Udacity robot simulation environment to serve as an environment in which to create an autonomous rover
that explores and identifies rocks on mars. There were two parts to this project. 
The first part was to test out functions in "Rover_Project_Test_Notebook.ipynb."
The goal was to add obstacle and rock identification functions and then to modify the `process_image()` function to create 
a world map.
The second part was to deploy the above functions in autonomous mode in the simulated environment.

## Notebook Tests

Describe in your writeup (and identify where in your code) how you modified or added functions to add obstacle and rock sample identification.

    Obstacle and rock detection was added in the Color Thresholding section of the notebook.  There I added two selection
    arrays `obstacle_select` and `rock_select`.  These two arrays were created by thresholding the images RGB dimensions
    according to values that were derived through experimentation.  For instance, the `obstacle_select` array is essentially
    the inverse of the `navigation_select` array so I inverted the comparison logic while using the same threshold values.  
    For the `rock_select` array, I experimented with an interactive image that displayed a rock in the notebook.  I gathered 
    3 separate RGB values across the image of the rock and used threshold values with some margin on top of those values. 
    Rather than keeping the inequalities the same, I flipped the logic on two of the color parameters so that I didn't simply
    capture larger swaths of navigation/obstacle state space.


Describe in your writeup how you modified the `process_image()` to demonstrate your analysis and how you created a worldmap.
Include your video output with your submission.

    In order to turn the robots camera images into a top down view I defined source points from the robots camera and
    destination points on the top down map to create a perspective transform. I used an interactive image from the robot's
    camera with the grid turned on to record four points as the source.  Then I derived a formula that maps those points to a
    location on the top down map as the destination points.
    I then used the function `perspect_transform(img, src, dst)` to transform the camera image into a warped, top-down image.  
    I then applied the function `color_thresh(warped)` to create arrays that represent navigable portions of the top-down image
    as well as obstacles and rocks.  I then converted those pixel arrays into rover-centric coordinates and then converted those
    rover-centric pixel values to world coordinates.  Lastly, I updated the world map.


## Autonomous Navigation and Mapping

Fill in the perception_step() (at the bottom of the perception.py script) and decision_step() (in decision.py) functions in the
autonomous mapping scripts and an explanation is provided in the writeup of how and why these functions were modified as they
were.

    Just as in the Notebook tests, I updated the perception.py script to transform rover camera images into top-down world-
    centric maps.  The main difference is that I updated Rover.vision_image with the color thresholded binary image for
    navigable, obstacle and rock pixel arrays.  I played around with the navigable and obstacle thresholds.  I noticed that
    sometimes the map fidelity was low and that seemed to coorelate with shadows being classified as obstacles.  In the end 160
    seemed to be a good balance for each of the colors.  I used my prior thresholds that I analyzed in the Notebook Tests and
    they identified rocks quite well.  For each of the navigable, obstacle, and rock arrays I then converted them to 
    rover-centric and then world-centric coordinates.  I then updated the worldmap with each of the navigable, obstacle and 
    rock pixel world-centric coordinates.  Lastly, I converted rover-centric pixels to polar coordinates.
    I played around with the steering angle when the rover had stopped by making it turn in the direction of the average
    angles.  However, I noticed that this had unintended consequences and could make the rover unstable at times.  Therefore, I
    left the decision.py script mostly unchanged.

Launching in autonomous mode your rover can navigate and map autonomously. Explain your results and how you might improve
them in your writeup.

    The rover was able to navigate through the simulation environment and identify rocks as well as map > 40% at >60%
    fidelity.  I noticed that the rover tended to oscillate while moving near its top speed.  I believe this has something to
    do with the fact that the average navigable angle is changing at a rate that's on the order of magnitude of the rover's
    speed.  I believe this affects the fidelity of the map, so one way to fix that would be to either slow down the rover or
    slow down the rate of steering change.
    
