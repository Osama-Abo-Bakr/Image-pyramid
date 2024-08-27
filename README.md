
# Image Pyramids

Image pyramids are a type of multi-scale signal representation in which an image is repeatedly smoothed and subsampled. They are used in various image processing tasks, such as image compression, object detection, and image blending.

## Types of Image Pyramids

### Gaussian Pyramid

A Gaussian pyramid is created by repeatedly applying a Gaussian blur to an image and then downsampling it. Each level of the pyramid represents the image at a different resolution.

- **Construction:**
    - Apply a Gaussian filter to the image.
    - Downsample the image by removing every second row and column.
    - Repeat the process for the desired number of levels.

- **Applications:**
    - Used for multi-scale analysis.
    - Helps in detecting objects at different scales.

### Laplacian Pyramid

A Laplacian pyramid is formed from the Gaussian pyramid. It captures the difference between the Gaussian-blurred image and the original image at each level, effectively highlighting the edges and details.

- **Construction:**
    - Create a Gaussian pyramid.
    - For each level, subtract the Gaussian-blurred image from the original image to get the Laplacian image.
    - The Laplacian pyramid is the collection of these difference images.

- **Applications:**
    - Used for image compression.
    - Useful in image blending and reconstruction.

## Sample Code

Here's a Python example using OpenCV to create Gaussian and Laplacian pyramids:

```python
import cv2
import numpy as np

# Load the image
image = cv2.imread('input.jpg')

# Generate Gaussian Pyramid
gaussian_pyramid = [image]
for i in range(6):
    image = cv2.pyrDown(image)
    gaussian_pyramid.append(image)

# Generate Laplacian Pyramid
laplacian_pyramid = []
for i in range(5, 0, -1):
    gaussian_expanded = cv2.pyrUp(gaussian_pyramid[i])
    laplacian = cv2.subtract(gaussian_pyramid[i-1], gaussian_expanded)
    laplacian_pyramid.append(laplacian)

# Save the results
for i, img in enumerate(gaussian_pyramid):
    cv2.imwrite(f'gaussian_level_{i}.jpg', img)

for i, img in enumerate(laplacian_pyramid):
    cv2.imwrite(f'laplacian_level_{i}.jpg', img)

# Display the results
for i, img in enumerate(gaussian_pyramid):
    cv2.imshow(f'Gaussian Level {i}', img)

for i, img in enumerate(laplacian_pyramid):
    cv2.imshow(f'Laplacian Level {i}', img)

cv2.waitKey(0)
cv2.destroyAllWindows()
