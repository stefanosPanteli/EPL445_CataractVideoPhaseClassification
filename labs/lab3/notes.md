# 2a

Το img.ravel() κάνει την εικόνα από 2D σε 1D array.


## How to make an image a little darker or lighter
1. We just have to add an offset to each pixel.
`I guess it doesnt work`

```python
offset = 50 # Whatever

# If pixel = 210
# pixel + offset = 260
# However pixel values can be up to 256 (for 8 bit values)
# It will wrap around -> pixel = 260 - 256 = 4
# We place if else statements to cap it

# For each pixel
for i in range(img.shape[0]):
    for j in range(img.shape[1]):
        # To cap it
        if img[i,j] + offset > 255:
            img[i,j] = 255
        elif img[i,j] + offset < 0:
            img[i,j] = 0
        # Go normally
        else:
            img[i,j] += offset
```

2. Or we have a scaling value
`Works a little better but not very good`

```python
p = 2.5 # Whatever

# If pixel = 210
# pixel * p = 530
# However pixel values can be up to 255 (for 8 bit values)
# It will wrap around -> pixel = 530 - 256 = ...
# We place if else statements to cap it

# For each pixel
for i in range(img.shape[0]):
    for j in range(img.shape[1]):
        # To cap it
        if math.floor(img[i,j] * p + 0.5) > 255:
            img[i,j] = 255
        elif math.floor(img[i,j] * p + 0.5) < 0:
            img[i,j] = 0
        # Go normally
        else:
            img[i,j] += offset
```

3. Or logarithmic transformation

```python
# Should enhance dark areas but compress bright
img_new = np.log2(1 + img.astype(np.uint8))

# Will be just black

# Get max and min intensities
A = int(img_new.max())
B = int(img_new.min())

# Full-Scale Contrast Stretch
# Min to 0
# Max to 255 
# All inbetween will be scaled
img_new = ((255 * (img.astype(float) - A)) / (B - A)).astype(np.uint8)
```

4. Histogram Equalization

```python
# Ανακατανομή του histogram για να βελτιστοποθήσει την εικόνα (το contrast)
# Κανονικοποίηση των φωτεινοτήτων σε όλο το φάσμα.
equalised_img = cv2.equalizeHist(img)
```


# 3

Προς το παρόν είδαμε μόνο το χωρικό πεδίο.
Θα δούμε το πεδίο συχνότητας. Η εικόνα πλέον είναι ένα άθροισμα περιοδικών σημάτων.
Μέσω *μετασχηματισμού*. Για να καταλήξουμε στην αρχική εικόνα κάνω τον *αντίστροφο μετασχηματισμό*

## Συχνότητα
Αναπαριστούν τον ρυθμό μεταβολής της φωτεινότητας ή του χρώματος.
- Χαμηλή συχνότητα = Δεν υπάρχουν απότομες αλλαγές. Επίσης έχουν μεγαλύτερο A.
- Ψηλή συχνότητα = Απότομες αλλαγές. (π.χ., ακμές)

## Γιατί
Είναι πιο εύκολη η εφάρμοση φίλτρων.

## Σήματα και Εικόνες
Η κάθε εικόνα μπορεί να αναπαρασταθεί ως άθροισμα εικόνων ημιτόνου ή συνημιτόνου.

```math
I(x,y) = 255 * sin(2π * (u/M * x + v/N * y))
# or
I(x,y) = 255 * cos(2π * (u/M * x + v/N * y))
```
- u: Η κάθετη συχνότητα
- v: Η οριζόντια συχνότητα

Μια εικόνα έχει μεγάλες τιμές συχνοτήτων αν αλλάζει συχνότητες συνεχός (π.χ., σελίδα με γράμματα)
Αντίθετα μια εικόνα μπορεί να έχει και χαμηλές και ψηλές συχνότητες.

- Μπορώ να τα αφαιρέσω με φίλτρα.

### Μετασχηματισμός Fourier
Ένα σύνολο ημιτονοείδων κυμάτων. Αναπαριστούν τον ρυθμό μεταβολής της φωτεινότητας σε μια εικόνα.
- Ένα σήμα με χωρική συχνότητα f θα έχει μια κορυφή στο f.
- Η συχνότητα 0 αντιπροσωπεύει τη μέση φωτεινότητα. Αν η εικόνα είναι σκοτεινή, τότε η DC συχνότητα ειναι χαμηλή. Αν αλλάξουμε την φωτεινότητα, τότε η DC συχνότητα είναι η μόνη που αλλάζει.

#### Μήκος Κύματος
- Μεγάλες εναλλαγές = Μεγάλες συχνότητες = Μακριά από το 0.
       Α
       ^
       |
       |
_______0_______
Πάντα συμμετρικές δεξιά και αριστερά του 0.


Οι εικόνες του μετασχηματισμού Fourier, έχουν σημεία και στους δύο άξονες και η ένταση της κουκκίδας είναι το A.

#### Κώδικας
```python
import numpy as np
import cv2
from matplotlib import pyplot as plt

# Read image in grayscale
img = cv2.imread('image.jpg', 0)

# Apply FFT from numpy
f = np.fft.fft2(img)
# Shift to center of image (rather than top left)
fshift = np.fft.fftshift(f) # Shifts by N/2 in both directions
# Get magnitude (A), scaled with log
magnitude_spectrum = np.log(1 + np.abs(fshift))

# Plot
plt.subplot(121), plt.imshow(img, cmap= 'gray')
plt.title('Input Image')
#                                                               # fraction for colorbar size, pad for colorbar position
plt.subplot(122), plt.imshow(magnitude_spectrum, cmap= 'gray'), plt.colorbar(cmap= 'gray', fraction= 0.03, pad= 0.04)
plt.title('Magnitude Spectrum')

plt.show()
```

Αν έχω περισσότερες συχνότητες στο κέντρο, σημαίνει ότι η εικόνα έχει να κάνει με μεγάλα αντικείμενα

```python
# To inverse the fourier transform
f_back = np.fft.ifftshift(fshift)
img_back = np.fft.ifft2(f_back)
img_back = np.real(img_back)
```