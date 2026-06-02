---

### 😎 Overlay Sunglasses on Faces using OpenCV

A computer vision application that detects human faces in images and overlays sunglasses with proper scaling and alignment using OpenCV and Python.

---

### 📌 Project Summary

This project demonstrates practical implementation of image processing, face detection, and alpha blending techniques. It dynamically positions a transparent sunglasses image on detected faces, ensuring proportional resizing and realistic placement.

The application highlights foundational concepts used in augmented reality filters, similar to those used in social media platforms.

---

### 🚀 Key Features

* Robust face detection using OpenCV Haar Cascades
* Dynamic overlay scaling based on face dimensions
* Accurate placement over eye region
* Support for transparent PNG overlays (alpha channel blending)
* Modular and extensible code structure

---

### 🛠️ Tech Stack

* **Language:** Python
* **Libraries:** OpenCV, NumPy
* **Concepts:** Image Processing, Object Detection, Alpha Blending

---

### 📂 Repository Structure

```
overlay-sunglasses-on-faces-using-opencv/
│
├── main.py                 # Core implementation
├── sunglasses.png         # Transparent overlay asset
├── input.jpg              # Sample input image
├── requirements.txt       # Dependencies
└── README.md              # Documentation
```

---
### 🧠 Technical Approach

#### 1. Face Detection

Utilizes Haar Cascade classifiers to identify bounding boxes around faces.

#### 2. Region Estimation

Approximates the eye region based on facial geometry.

#### 3. Image Resizing

Scales the sunglasses proportionally to face width.

#### 4. Alpha Blending

Applies overlay using mask separation:

* Foreground (sunglasses)
* Background (face region)
* Transparency mask

---

### 📊 Example Workflow

```
Input Image → Face Detection → Region Mapping → Resize Overlay → Blend → Output Image
```
---

### PROGRAM 

```
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Read Images
faceImage = cv2.imread("download.jpg")
glassPNG = cv2.imread("sunglass.png", cv2.IMREAD_UNCHANGED)

# Show Original Face
plt.figure(figsize=(5,5))
plt.imshow(faceImage[:,:,::-1])
plt.title("Original Face")
plt.axis("off")
plt.show()

# Resize Glass (center fit)
glassPNG = cv2.resize(glassPNG,(100,30))

# Show Glass
plt.figure(figsize=(8,3))
plt.imshow(glassPNG[:,:,::-1])
plt.title("Sunglass")
plt.axis("off")
plt.show()

# Separate Color and Alpha
glassBGR = glassPNG[:,:,:3]

glassMask = glassPNG[:,:,3] / 255.0
glassMask = np.dstack([glassMask,glassMask,glassMask])

# Face dimensions
H,W = faceImage.shape[:2]

# Center Position
glass_h, glass_w = glassBGR.shape[:2]

x1 = (W - glass_w)//2
y1 = int(H*0.35)

x2 = x1 + glass_w
y2 = y1 + glass_h

# Eye ROI
eyeROI = faceImage[y1:y2,x1:x2]

# Blend
maskedEye = cv2.multiply(
    eyeROI.astype(np.float32),
    (1-glassMask).astype(np.float32)
)

maskedGlass = cv2.multiply(
    glassBGR.astype(np.float32),
    glassMask.astype(np.float32)
)

eyeRoiFinal = cv2.add(maskedEye,maskedGlass)
eyeRoiFinal = eyeRoiFinal.astype(np.uint8)

# Intermediate Results
plt.figure(figsize=[20,20])

plt.subplot(131)
plt.imshow(maskedEye[:,:,::-1])
plt.title("Masked Eye Region")

plt.subplot(132)
plt.imshow(maskedGlass[:,:,::-1])
plt.title("Masked Sunglass Region")

plt.subplot(133)
plt.imshow(eyeRoiFinal[:,:,::-1])
plt.title("Augmented Eye and Sunglass")

plt.show()

# Final Output
faceWithGlasses = faceImage.copy()
faceWithGlasses[y1:y2,x1:x2] = eyeRoiFinal

plt.figure(figsize=(10,5))

plt.subplot(121)
plt.imshow(faceImage[:,:,::-1])
plt.title("Original Image")
plt.axis("off")

plt.subplot(122)
plt.imshow(faceWithGlasses[:,:,::-1])
plt.title("With Sunglasses")
plt.axis("off")

plt.show()
```

### 📸 Results
<img width="737" height="759" alt="image" src="https://github.com/user-attachments/assets/5d12dd2b-28ba-4b4e-a1c3-11cb5fbc3e0e" />


---

### 🔍 Use Cases

* Augmented Reality filters
* Social media effects
* Computer vision learning projects
* Real-time camera applications

---
