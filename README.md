# Simple Motion Detection System

[![Python 3.x](https://img.shields.io/badge/python-3.x-blue.svg)](https://www.python.org/downloads/)
[![OpenCV](https://img.shields.io/badge/OpenCV-2A9960?style=flat&logo=opencv&logoColor=white)](https://opencv.org/)
[![imutils](https://img.shields.io/badge/imutils-orange.svg)](https://github.com/PyImageSearch/imutils)

This project provides a straightforward, real-time motion detection system using a webcam and OpenCV. It identifies moving objects in the camera's field of view by continuously comparing the current frame to a reference "first frame" and highlighting significant changes.

---

## How It Works

This motion detection system operates on the principle of **background subtraction**, specifically using a static background reference:

1.  **Video Stream Capture:** The system starts capturing video frames from your default webcam.
2.  **Initial Background Reference:** The very first frame captured from the webcam is stored as the `firstFrame`. This frame is assumed to be a static background against which all subsequent motion will be detected.
3.  **Preprocessing:** Each incoming video frame undergoes several preprocessing steps:
    * It's resized to a fixed width for consistent processing.
    * Converted to **grayscale** to simplify calculations (color information isn't needed for basic motion detection).
    * A **Gaussian blur** is applied to smooth out noise and reduce high-frequency variations, which helps in detecting larger, more significant movements.
4.  **Difference Calculation:** The preprocessed current frame is compared to the `firstFrame` using an **absolute difference** operation. This highlights areas where changes have occurred.
5.  **Thresholding & Dilation:**
    * The difference image is converted to a **binary image** using a threshold. Pixels with a difference value above a certain threshold (e.g., 25) are turned white (indicating motion), and others are turned black.
    * The binary image is then **dilated**. This operation expands the white areas, helping to connect fragmented regions of motion and make smaller movements more visible.
6.  **Contour Detection:** The system finds contours (outlines of connected white pixels) in the dilated binary image. Each contour potentially represents a moving object.
7.  **Motion Filtering & Highlighting:**
    * Contours smaller than a predefined `area` threshold (e.g., 500 pixels) are ignored to filter out minor disturbances or noise.
    * For any contour larger than the threshold, a **green bounding rectangle** is drawn around it on the original color frame.
    * A text overlay indicates "Moving Object detected" or "Normal" based on whether significant motion is found.
8.  **Real-time Display:** The processed video feed with overlays is displayed in a window titled "cameraFeed".
