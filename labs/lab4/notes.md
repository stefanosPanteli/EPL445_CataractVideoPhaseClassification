# 3 (συνέχεια)

## Δημιουργία συνημιτονιακής εικόνας
```python
import math
import numpy as np

Icos = np.zeros((N,N)) # N x N matrix

freq = 2 # Frequency. 2 means 2 cycles -> 2 black horizontal lines
         # If freq = 4, 4 cycles -> 4 black horizontal lines (greater freq -> faster wave -> more cycles)

# Rows
for i in range(N):
    # Columns
    for j in range(N):
        Icos[i, j] = 127 * (1 + math.cos((freq * 2 * math.pi * i) / N)) # If we change to j, then the changes will happen in the x axis. 
        # We generate a cosine wave with 127 as amplitude.
        # The generated image will have a wave form downwards. (i)
```

## Μετασχηματισμός Fourier
```python
f = np.fft.fft2(Icos)
fshift = np.fft.fftshift(f)
magnitude_spectrum = np.log(1 + np.abs(fshift))
```

## Αντίστροφος Μετασχηματισμός Fourier
```python
f_back = np.fft.ifftshift(fshift)
img_back = np.fft.ifft2(f_back)
img_back = np.real(img_back)
```

# Μάσκες - Φίλτρα
Οι μάσκες είναι δυαδικές εικόνες (0 και 1).
Πρέπει να έχει τις ίδιες διαστάσεις με την αρχική εικόνα.
Εφαρμόζεται **ΜΟΝΟ** στις FFT -> Shifted εικόνες.

- Είδη μασκών
    - Low pass: Επιτρέπουν να περάσουν **ΜΟΝΟ** οι συχνότητες χαμηλής συχνότητας.
    - Mid pass: Επιτρέπουν να περάσουν **ΜΟΝΟ** οι συχνότητες μέτριας συχνότητας.
    - High pass: Επιτρέπουν να περάσουν **ΜΟΝΟ** οι συχνότητες ψηλής συχνότητας.

```python
image_gr = cv2.im_read('image.jpg', 0) # Grayscale image
rows, cols = image_gr.shape     # Dimensions
crow, ccol = rows / 2, cols / 2 # Center
crow = np.int(crow) # To take integer, not floating point.
ccol = np.int(ccol) # To take integer, not floating point.

# Apply FFT
f = np.fft.fft2(image_gr)
fshift = np.fft.fftshift(f)
magnitude_spectrum = np.log(1 + np.abs(fshift))

# Set mask boundary
boundary = 50

mask = np.zeros(img.shape, np.uint8)                                       # low pass
mask[crow - boundary:crow + boundary, ccol - boundary:ccol + boundary] = 1 # Make all elements 1 within the mask.

# Apply mask
fshift = fshift * mask # If mask[i,j] = 0, then the corresponding fshift[i,j] = 0.

# Apply inverse FFT
f_back = np.fft.ifftshift(fshift)
img_back = np.fft.ifft2(f_back)
img_back = np.real(img_back)
```

* High pass
```python
# Set mask boundary
boundary = 50

mask = np.ones(img.shape, np.uint8)
mask[crow - boundary:crow + boundary, ccol - boundary:ccol + boundary] = 0 # Make all elements 1 within the mask.
```

* Mid pass
```python
# Set mask boundary
boundary1 = 50
boundary2 = 100

mask = np.zeros((rows, cols), np.uint8)

mask[crow - boundary1:crow + boundary1, ccol - boundary2:ccol + boundary2] = 1 # Make all elements 1 within the mask.
mask[crow - boundary2:crow + boundary2, ccol - boundary1:ccol + boundary1] = 1 # Make all elements 0 within the smaller mask.
```

## Cyclic Mask
* m(x - a)^2 + (y - b)^2 = r^2

- For Low Pass:
if sqrt((x - a)^2 + (y - b)^2) <= r: mask(x,y) = 1