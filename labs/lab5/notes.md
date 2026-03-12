# Convolution
Γιατί:
1. Smoothing
2. Sharpening
3. Enhancement

Ουσιαστικά κάνω την διαδικασία του `erode / dilate` αλλά κάνω την πράξη `average` (αν είναι εκτώς της εικόνας το παράθυρο -> 0 padding)

## Δημιουργία Kernel & Convolution
```python
kernel5 = np.ones((5,5), np.float32) / 25 # 5x5 matrix
kernel9 = np.ones((9,9), np.float32) / 81 # 9x9 matrix

# Convolution
dst5 = cv2.filter2D(img_gray, -1, kernel5) # Αντί -1 έχω CV_8U, CV_16U, CV_16S, CV_32F, CV_64F


#! OR using blur
dst5 = cv2.blur(img_gray, (5,5)) # Identical
```

> Μεγαλύτερο Kernel έχει πιο θωλό αποτέλεσμα (σαν low pass filter).

## Άλλα filters
```python
gaussian_5 = cv2.GaussianBlur(img_gray, (5,5), 0) # 0 is the sd i want in the Gaussian kernel. It has to do with my initial image.
median_5 = cv2.medianBlur(img_gray, 5)
```

# Image Gradients - High Pass Filters
```python
laplcian_5 = cv2.Laplacian(img_gray, cv2.CV_64F)

# Επιλογή και διάστασης (img, _, d1, d2, ksize), d1 is the x axis (horizontal) and d2 the y axis (vertical)
sobel_x5   = cv2.Sobel(img_gray, cv2.CV_64F, 1, 0, ksize=5) 
sobel_y5   = cv2.Sobel(img_gray, cv2.CV_64F, 0, 1, ksize=5)
```

> Αναγνωρίζουν τις απότομες εναλλαγές φωτεινοτήτων.

# Insert Noise

> Filters work better with noise.

- gaussian (white noise with constant mean and variance)
    - Προσθέτει
- localvar (zero-mean gaussian white noise with intensity-dependent variance)
- poisson (poisson noise)
- salt (replaces random pixels with 1)
    - Τυχαία
- pepper (replaces random pixels with 0)
    - Τυχαία
- s&p (salt and pepper)
- speckle (multiplicative noise [out = image = n*image, where n is gaussian noise with μ and σ])
    - Πολλαπλασιάζει

# Noise in OpenCV
```python
import skimage
salt_pepper = skimage.util.random_noise(img, mode='s&p', amount=0.2) # amount% of pixels will be salted and peppered
gauss = skimage.util.random_noise(img, mode='gaussian')
localvar = skimage.util.random_noise(img, mode='localvar')
# All random noise images are float valued
```