# Smart-Parking-Detection-System-with-OpenCV
This Python project detects and monitors parking spaces in a video feed using OpenCV. It highlights available spots and dynamically updates the count of free spaces.

# Features
Processes video frame-by-frame for parking status.
Uses adaptive thresholding and pixel count to determine space availability.
Annotates video feed with free (green) and occupied (red) spots.

# How It Works
Input: A video of a parking lot and parking spot coordinates.
Output: Real-time parking availability displayed on the video.Explanation of the Code

**This code is a Python program that monitors parking spaces in a video feed and detects available spots using OpenCV.**

# Key Components:
**Setting Up:**
cv2.VideoCapture: Loads a parking lot video (CarPark1.mp4).
CarParkPos: A pickle file containing coordinates of parking spots (list of top-left corner positions for each parking space).
Parking Space Dimensions: Defined as rectW = 107 (width) and rectH = 48 (height).

**Processing the Video:**
Converts each frame to grayscale and applies Gaussian blur for noise reduction.
Uses adaptive thresholding to highlight significant features (e.g., empty parking spaces) and binary inversion for contrasting areas.
Further cleans up the binary image using median blur and dilation.

**Detecting Parking Spaces:**
check Function:
Iterates through parking space coordinates in posList.
Extracts and analyzes a cropped portion of the processed frame corresponding to each parking space.
If the number of non-zero pixels (cv2.countNonZero) is below a threshold (900), the space is marked as free (green rectangle). Otherwise, it’s occupied (red rectangle).
Displays the count of free spaces on the video frame.

**Video Playback:**
Reads and processes each frame.
Loops the video when it ends by resetting the frame position (cap.set(cv2.CAP_PROP_POS_FRAMES, 0)).
Exits when the user presses q.

**Display Results:**
Annotates each frame with colored rectangles for parking spaces and the count of available spots.
Shows the video with live annotations in a window titled "Image".

