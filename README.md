# Multi-Image Panorama Stitching – Computer Vision

## Project Overview

This project implements a multi-image panorama stitching pipeline using the PASSTA LunchRoom dataset.

The system combines consecutive images into a single panoramic image through feature detection, feature description, feature matching, geometric alignment, perspective warping, and image blending.

The main implementation uses Harris Corner Detection and SIFT descriptors, followed by BFMatcher, Lowe's Ratio Test, and RANSAC-based Homography Estimation.

## Dataset

- **Dataset:** PASSTA LunchRoom
- **Selected Images:** 12 consecutive images
- **Image Sequence:** `img01.jpg` to `img12.jpg`
- **Reference Image:** `img07.jpg`
- **Maximum Image Dimension:** 1000 pixels

Dataset structure:

```text
dataset/
└── LunchRoom/
    ├── images/
    │   ├── img01.jpg
    │   ├── img02.jpg
    │   └── ...
    │   └── img12.jpg
    └── Calibration.txt
```

## Project Pipeline

The panorama stitching process consists of the following stages:

1. **Image Loading and Preprocessing**
   - Load consecutive images from the dataset.
   - Resize images to a maximum dimension of 1000 pixels.
   - Prepare images for feature detection and matching.

2. **Harris Corner Detection**
   - Detect candidate feature locations using Harris Corner Detection through `cv2.goodFeaturesToTrack`.

3. **SIFT Feature Description**
   - Compute SIFT descriptors at the detected Harris corner locations.

4. **Feature Matching**
   - Match descriptors using Brute-Force Matcher (`BFMatcher`).
   - Use Euclidean distance (`NORM_L2`).
   - Apply K-Nearest Neighbors with K=2.
   - Filter matches using Lowe's Ratio Test with a threshold of 0.75.

5. **RANSAC and Homography Estimation**
   - Estimate pairwise homographies between adjacent images.
   - Apply RANSAC with a reprojection threshold of 5.0 pixels.
   - Separate geometrically consistent matches (Inliers) from Outliers.

6. **Multi-Image Alignment**
   - Transform all selected images into a common reference coordinate system.
   - Use the middle image as the reference image.

7. **Panorama Canvas Calculation**
   - Transform image corners.
   - Calculate the required panorama canvas dimensions.
   - Account for negative coordinates using a translation matrix.

8. **Perspective Warping**
   - Warp all images into the common panorama canvas.
   - Generate validity masks for image coverage.

9. **Image Blending**
   - Implement Simple Average Blending.
   - Implement Feather Blending as an improved blending method.

10. **Cropping**
    - Detect the largest fully valid rectangular region.
    - Remove black borders from the final panorama.

## Main Algorithms and Techniques

| Technique | Purpose |
|---|---|
| Harris Corner Detection | Detect candidate feature locations |
| SIFT | Extract local image descriptors |
| BFMatcher | Match image descriptors |
| Lowe's Ratio Test | Filter ambiguous matches |
| RANSAC | Reject geometrically inconsistent matches |
| Homography | Estimate perspective transformations |
| Perspective Warping | Align images into a common coordinate system |
| Simple Averaging | Blend overlapping image regions |
| Feather Blending | Improve transitions between overlapping images |
| Image Cropping | Remove invalid black borders |

## Results

The implementation successfully stitched 12 consecutive images from the PASSTA LunchRoom dataset.

### Pairwise Matching Results

- **Adjacent Image Pairs:** 11
- **Average Inlier Ratio:** Approximately 98%
- **Reference Image:** `img07.jpg`

RANSAC visualization demonstrates the detected inliers and outliers:
- Green lines represent inliers.
- Red lines represent outliers.

### Final Panorama

- **Final Dimensions:** 1734 × 1000 pixels
- **Output Formats:** PNG and JPG
- **JPEG Quality:** 95%

## Output Files

All generated results are stored in the `outputs/` directory.

| File | Description |
|---|---|
| `final_panorama.png` | Final cropped panorama using Feather Blending |
| `final_panorama.jpg` | Final panorama in JPG format |
| `panorama_simple_average.png` | Panorama generated using Simple Averaging |
| `panorama_feathered.png` | Panorama generated using Feather Blending |
| `opencv_stitcher_comparison.png` | Optional comparison using OpenCV's built-in Stitcher |
| `stitching_summary.json` | Processing configuration, image information, and pairwise matching statistics |

## Project Structure

```text
panorama-stitching-computer-vision/
│
├── dataset/
│   └── LunchRoom/
│       ├── images/
│       └── Calibration.txt
│
├── docs/
│
├── notebooks/
│   └── Panorama_Stitching_Project.ipynb
│
├── outputs/
│   ├── final_panorama.png
│   ├── final_panorama.jpg
│   ├── panorama_simple_average.png
│   ├── panorama_feathered.png
│   ├── opencv_stitcher_comparison.png
│   └── stitching_summary.json
│
├── src/
│
├── .gitignore
├── README.md
└── requirements.txt
```

## Installation and Execution

### 1. Clone the Repository

```bash
git clone <repository-url>
cd panorama-stitching-computer-vision
```

### 2. Create a Virtual Environment

```bash
python -m venv .venv
```

### 3. Activate the Environment

For Windows PowerShell:

```powershell
.venv\Scripts\Activate.ps1
```

### 4. Install Dependencies

```bash
pip install -r requirements.txt
```

### 5. Run the Project

Open the notebook:

```text
notebooks/Panorama_Stitching_Project.ipynb
```

Run all cells sequentially to execute the complete panorama stitching pipeline and generate the output files.

## Technologies Used

- Python
- OpenCV
- NumPy
- Matplotlib
- Jupyter Notebook

## Conclusion

The project demonstrates a complete multi-image panorama stitching pipeline using classical computer vision techniques.

By combining Harris Corner Detection, SIFT descriptors, feature matching, RANSAC-based Homography Estimation, perspective warping, and image blending, the system successfully generates a panoramic representation from 12 consecutive images.

The implementation also provides a comparison between two blending methods and an optional comparison with OpenCV's built-in Stitcher.
