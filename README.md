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



# Importing Libraries
**import cv2
import numpy as np
import pickle**
cv2: OpenCV library for image and video processing.
numpy: Library for numerical operations, used here for matrix manipulations.
pickle: Python module to load pre-saved parking space positions.

# Defining Parking Space Dimensions
**rectW, rectH = 107, 48**
rectW and rectH: Width and height of each parking space rectangle, matching real-world measurements.

# Loading the Video
**cap = cv2.VideoCapture('CarPark1.mp4')**
cv2.VideoCapture: Opens the video file for processing. Each frame will represent a parking lot view.

# Loading Parking Space Positions
**with open('CarParkPos', 'rb') as f:
    posList = pickle.load(f)**
CarParkPos: Predefined parking space coordinates (stored as a list of tuples).
pickle.load: Reads the saved data into posList.

# Initializing Frame Counter
**frame_counter = 0**
Tracks the current frame in the video. Used for looping the video playback.

# Defining the Function to Check Parking Spaces
**def check(imgPro, img):**
check: Function to identify free or occupied parking spaces.
Parameters:
imgPro: Processed (binary) image to analyze parking spots.
img: Original frame for drawing annotations.

# Checking Each Parking Spot
**spaceCount = 0
for pos in posList:
    x, y = pos
    crop = imgPro[y:y + rectH, x:x + rectW]
    count = cv2.countNonZero(crop)**
spaceCount: Counter for free spaces.
for pos in posList: Loops through parking space coordinates.
crop: Extracts the region of the processed image corresponding to a parking space.
cv2.countNonZero(crop): Counts white pixels (non-zero) in the cropped region.
Threshold: A high count indicates the space is occupied.

# Determining Free or Occupied Spaces
**if count < 900:
    color = (0, 255, 0)
    thickness = 5
    spaceCount += 1
else:
    color = (0, 0, 255)
    thickness = 2
count < 900:**
If fewer than 900 white pixels are detected, the space is free.
Free Space: Green rectangle ((0, 255, 0)), thick border.
Occupied Space: Red rectangle ((0, 0, 255)), thin border.

# Drawing Rectangles and Displaying Count
**cv2.rectangle(img, pos, (pos[0] + rectW, pos[1] + rectH), color, thickness)
cv2.rectangle(img, (45,30),(250, 75), (100, 0, 100), 1)
cv2.putText(img, f'Free: {spaceCount}/{len(posList)}', (50, 50),
            cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 0), 2)
cv2.rectangle: Draws colored rectangles around each parking space.
cv2.putText: Displays the free spaces count (e.g., "Free: 5/10") on the frame.**

# Main Loop to Process Video Frames
**while True:
  ret, img = cap.read()
  if not ret:
    break**
cap.read: Captures the next frame in the video.
ret: Boolean indicating if the frame was successfully captured. Breaks the loop at the end of the video.

# Resetting Video to Loop
**frame_counter += 1
  if frame_counter == cap.get(cv2.CAP_PROP_FRAME_COUNT):
    frame_counter = 0
    cap.set(cv2.CAP_PROP_POS_FRAMES, 0)**
    
cv2.CAP_PROP_FRAME_COUNT: Total number of frames in the video.
When the last frame is reached, the counter is reset, and playback starts from the beginning.

# Image Preprocessing
  **gray = cv2.cvtColor(img, cv2.COLOR_BGR2GRAY)
  blur = cv2.GaussianBlur(gray, (3, 3), 1)
  Thre = cv2.adaptiveThreshold(blur, 255, cv2.ADAPTIVE_THRESH_GAUSSIAN_C,cv2.THRESH_BINARY_INV, 25, 16)
  blur = cv2.medianBlur(Thre, 5)
  kernel = np.ones((3, 3), np.uint8)
  dilate = cv2.dilate(blur, kernel, iterations=1)**
cv2.cvtColor: Converts the frame to grayscale.
cv2.GaussianBlur: Smoothens the image to reduce noise.
cv2.adaptiveThreshold: Highlights parking spaces by creating a binary image.
THRESH_BINARY_INV: Inverts the colors (parking spaces become white).
cv2.medianBlur: Further smoothens the binary image.
cv2.dilate: Expands white regions to fill small gaps.

# Check and Display Results
  **check(dilate, img)
  cv2.imshow('Image', img)
  if cv2.waitKey(1) & 0xFF == ord('q'):
    break**
check: Calls the parking space detection function with the processed image.
cv2.imshow: Displays the video frame with annotated results.
cv2.waitKey(1): Waits for a key press. Exits if 'q' is pressed.

# Releasing Resources
**cap.release()
cv2.destroyAllWindows()**
cap.release: Frees video resources.
cv2.destroyAllWindows: Closes all OpenCV windows.

