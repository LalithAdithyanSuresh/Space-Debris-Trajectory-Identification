# Space Debris Trajectory Identification using Video Analysis


This project demonstrates a Python-based approach using OpenCV and Scipy to detect, track, and estimate the trajectory of moving objects (simulating space debris) within a video recording. It utilizes frame differencing to isolate motion, blob detection to identify objects, and tracking algorithms to follow them across frames and calculate their velocity.

## Visual Overview

*(Replace the placeholder links below with links to your actual GIFs)*

**1. Original Input Footage:**
![Original Video Sample](placeholder_link_to_original_video.gif)
*(Briefly describe what the original video shows)*

**2. Frame Differencing Output:**
![Frame Difference Sample](placeholder_link_to_difference_video.gif)
*(This shows the result of subtracting consecutive frames, highlighting areas of motion)*

**3. Final Object Tracking Output:**
![Object Tracking Sample](placeholder_link_to_tracking_output.gif)
*(This shows the original video with tracked objects marked by ellipses and IDs)*

---

## Features

*   **Video Loading:** Reads video files using OpenCV (`cv2`).
*   **Frame Extraction:** Converts video into NumPy arrays and optionally saves individual frames.
*   **Grayscale Conversion:** Simplifies processing by converting frames to grayscale.
*   **Frame Differencing:** Isolates moving regions by calculating the absolute difference between consecutive frames.
*   **Blob Detection:** Uses `scipy.ndimage` to find connected components (blobs) in difference frames.
*   **Object Tracking:** Implements methods to track blobs across frames based on proximity, area similarity, and persistence.
*   **Visualization:** Generates an output video showing original footage with tracked objects highlighted.
*   **Velocity Calculation:** Estimates the average velocity (pixels per frame) for each tracked object.

## Workflow

1.  **Load Video:** The input video (`MyRecording_001.mp4`) is loaded, and its properties (frame count, dimensions, FPS) are extracted. Frames are stored in a NumPy array (`frames_array`) and optionally saved to the `frames/` directory.
2.  **Grayscale Conversion:** Color frames are converted to grayscale (`gray_frames`).
3.  **Frame Differencing:** Absolute differences between consecutive grayscale frames are computed (`Diff`) to highlight motion.
4.  **Save Difference Frames (Optional):** Difference frames are saved to the `diffFrames/` directory and compiled into `diffFrames/output.mp4`. *(See GIF above)*
5.  **Persistent Blob Tracking & Visualization:**
    *   Blobs (moving regions) are detected in the difference frames.
    *   Blobs are tracked using proximity (`max_dist`) and area similarity (`max_area_diff`).
    *   Short-lived tracks (less than `min_duration`) are filtered out.
    *   Persistent blobs are drawn onto the *original* video frames with IDs.
    *   Annotated frames are saved as `persistent_objects.mp4`. *(See GIF above)*
6.  **Object Tracking & Velocity Calculation:**
    *   A separate tracking function (`track_objects_across_frames`) identifies and tracks objects using proximity (`max_dist`), relative size change (`size_tol`), and persistence (`min_frames`).
    *   The average velocity for each valid track is calculated and printed.

## Prerequisites

*   Python 3.x
*   Jupyter Notebook or JupyterLab
*   Required Python libraries:
    *   `opencv-python`
    *   `numpy`
    *   `scipy`
    *   `Pillow`

## Installation

1.  Clone the repository:
    ```bash
    git clone https://github.com/LalithAdithyanSuresh/Space-Debris-Trajectory-Identification.git
    cd Space-Debris-Trajectory-Identification
    ```
2.  Install the required libraries (preferably in a virtual environment):
    ```bash
    pip install opencv-python numpy scipy Pillow jupyterlab
    ```
    (Replace `jupyterlab` with `notebook` if you prefer the classic Notebook interface).

## Usage

1.  Place your input video file named `MyRecording_001.mp4` in the root directory of the repository (alongside `finalCode.ipynb`).
2.  Launch JupyterLab or Jupyter Notebook:
    ```bash
    jupyter lab
    # or
    jupyter notebook
    ```
3.  Open the `finalCode.ipynb` notebook.
4.  Run the cells sequentially from top to bottom. Ensure each cell completes execution before proceeding to the next.

## Input

*   `MyRecording_001.mp4`: The input video file containing the moving objects (e.g., simulated space debris).

## Outputs

*   `frames/` directory: Contains individual frames extracted from the input video (if `output_folder` is set in the first cell).
*   `diffFrames/` directory: Contains individual frame difference images.
*   `diffFrames/output.mp4`: A video compiled from the frame difference images.
*   `persistent_objects.mp4`: The main output video, showing the original footage with persistent tracked objects highlighted by red ellipses and unique IDs.
*   **Console Output:** Progress indicators, array shapes, and the calculated average velocities for tracked objects.

## Creating and Adding GIFs

To add the demonstration GIFs:

1.  **Create GIFs:** Use screen recording software (like Kap, ScreenToGif, LICEcap) or Python libraries (like `imageio`) to capture short clips of:
    *   A segment of your `MyRecording_001.mp4`.
    *   A segment of the generated `diffFrames/output.mp4`.
    *   A segment of the generated `persistent_objects.mp4`.
2.  **Upload GIFs:** Upload these GIF files directly to your GitHub repository or use an image hosting service.
3.  **Update Links:** Replace the `placeholder_link_to_...gif` URLs in this README file with the actual URLs/paths to your uploaded GIFs.

## Notes

*   **Parameter Tuning:** The tracking parameters (`min_area`, `min_duration`, `max_dist`, `max_area_diff`, `size_tol`, `min_frames`) in cells 8 and 10 are crucial. You may need to adjust them based on your specific video's resolution, frame rate, object size, speed, and noise levels for optimal results.
*   **Velocity Calculation Warnings:** The console output for velocity calculation (cell 12) shows many `NaN` and `inf` values. This often occurs when:
    *   A track contains only one detected point (no difference to calculate velocity).
    *   An object is detected in exactly the same position in consecutive tracked frames (`dt=0`), leading to division by zero. This might indicate stationary noise or very slow movement between frames where the centroid doesn't shift measurably. Review the `RuntimeWarning` messages.
*   **Directory Permissions:** Ensure the script has permission to create the `frames/` and `diffFrames/` directories if they don't exist.

