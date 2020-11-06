# **Advanced Lane Finding** 

## Project goal

Identifying road lanes using a robust algorithm against different color intensities and lightness.

## Project steps

1. Camera calibration: computation of camera matrix, distortion coefficients, rotation and translation vectors using a set of chessboard images.
2. Distortion correction: applying calibration outputs to correct a distorted image.
<img src="./output_images/1 undistort_output.png" alt="Distortion correction" width="500" height="170">

3. Images thresholding: extracting of useful information by doing color/gradient thresholding.
<img src="./output_images/2 binary_combo.png" alt="Hough Transform" width="300" height="170">

4. Perspective transformation: transform image from normal camera 2D view to bird-eye view.
<img src="./output_images/3 warped_straight_lines.png" alt="Hough Transform" width="500" height="170">

5. Lanes detection: detect lanes and fitting suitable polynomial curves to them.
<img src="./output_images/line-4 color_fit_lines.png" alt="Hough Transform" width="300" height="170">

6. Curvature radius and car offset calculation: calculate lane curvature and car offset with respect to lanes.
7. Lanes region output: displaying lanes region by unwarping bird-eye view image and displaying lanes curvature and car offset.
<img src="./output_images/5 example_output.png" alt="Hough Transform" width="300" height="170">

## Pipeline description

Pipeline `pipeline(image)` returns `lanes_output` image of camera video frame, indicating lane lines area in green.
1 - Camera calibration is done using `cv2.calibrateCamera`
2 - Distortion correction is done using `undistort(img)` routine
3 - Color, Gradient Absolute, Gradient direction and Gradient magnitude is used to threshold images:
```comb_binary[((abs_binary == 1) & (mag_binary == 1)) | (col_binary == 1) | ((mag_binary == 1) & (dir_binary == 1))] = 1```
4 - Perspective transformation is done using `pers_tranform(image)` routine. Four source points are mapped into four destination points of bird-eye view, described in the following table:
|   | Source  |  Destination  |
|-----------|--------|--------|
|Top left | (588, 451)   |  (250, 0)  |
|Top right | (690, 451) |  (1030, 0)  |
| Bottom right | (1055, 678)   |  (1030, 720)  |
| Bottom left | (251, 678)  |  (250, 720)  |
5 - Two polynomials are fit to right and left lanes using `fit_polynomial`
6 - Lane curvature is measured using formula of: R_curve=  ( [1 + (2Ay+B) ^ 2 ]^(3⁄2) )/|2A| 


### Basic Build Instructions
The jupyter notebook _main.ipynb_ is containing necessary routines. The original camera video is located in the root path and algorithm output will be generated in _./output_video_ path.