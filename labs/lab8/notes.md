# Template Matching
I have a template image T, and I want to find it in an image I
- The image I, might have a bigger, smaller, or rotated version of T
- Or even noisy or stretched
-> Will not be a match

## How
I want a metric to tell me how good a match is

- R table: R(x, y) = how good is the match at x, y
```python
cv2.matchTemplate(method= cv2.pass)
cv2.minMaxLoc() # Finds the min/max (optimal) location of table R
# Only works for single channel images (not RGB)
```

### Mathods
1. CV_TM_SQDIFF: R(x, y) = Σ x' y' (T(x', y') - I(x + x', y + y'))^2
2. CV_TM_SQDIFF_NORMED:
> Checks the differences between the template and the image in brightness
> I want the min value of R

3. CV_TM_CCORR: R(x, y) = Σ x' y' (T(x', y') * I(x + x', y + y'))
4. CV_TM_CCORR_NORMED:
> I want the max value of R

5. CV_TM_CCOEFF:
6. CV_TM_CCOEFF_NORMED:
> I want the max value of R

### Example
```python
methods = [cv2.TM_CCOEFF, cv2.TM_CCOEFF_NORMED, cv2.TM_CCORR, cv2.TM_CCORR_NORMED, cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED]

for m in methods:
    img = image.copy()
    result = cv2.matchTemplate(img, template, m)
    min_val, max_val, min_loc, max_loc = cv2.minMaxLoc(result)

    if m in [cv2.TM_SQDIFF, cv2.TM_SQDIFF_NORMED]:
        top_left = min_loc # I need min R value
    else:
        top_left = max_loc # I need max R value

    # h, w = template.shape
    bottom_right = (top_left[0] + w, top_left[1] + h)
    cv2.rectangle(img, top_left, bottom_right, 255, 2) # Draw a rectangle with a thickness of 2

    # The image of the result will have a bright or dark spot on the upper left point of the template.
    plt.subplot(121), plt.imshow(result, cmap='gray')
    plt.title('Matching Result'), plt.xticks([]), plt.yticks([])
    plt.subplot(122), plt.imshow(img, cmap='gray')
    plt.title('Detected Point'), plt.xticks([]), plt.yticks([])
    plt.suptitle(m)
    plt.show()
```

Template Matching is sensitive to all mentioned above. The template should be included exactly the same in the image.

# Edge Detection
From a previous lab we learned the Laplacian and Sobel filters which detect edges.

## Edge Detection with Canny Algorithm
1. Image smoothing with Gaussian filter
2. Gradient calculation with Sobel filter on both directions 
3. For each pixel in the image, check if its a local maximum in its neighborhood.
    - If it is not a local maximum set the value of the pixel to 0  (The corresponding (i,j) value)
4. If the pixel is a local maximum, compare with a threshold to see if it is an edge and is set to 1.
    - In this step we apply the Canny algorithm.

### Canny Algorithm
1. We need 2 threshold values (min and max)
    - Pixels with gradient value greater than the max threshold are an edge
    - Pixel with gradient value less than the min threshold are not an edge and are discarded immediately
2. For pixels between the min and max threshold are checked.
    - If the pixel is connected to a pixel already defined as an edge, it is considered an edge as well.
    - Otherwise it is not an edge.

```python
edges = cv2.Canny(img, 100, 200) # 100 is the min threshold, 200 is the max threshold

plt.subplot(121), plt.imshow(img, cmap='gray')
plt.title('Original Image'), plt.axis('off')
plt.subplot(122), plt.imshow(edges, cmap='gray')
plt.title('Edge Image'), plt.axis('off')
plt.show()
```