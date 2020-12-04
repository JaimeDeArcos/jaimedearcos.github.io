---
date: 2020-11-22T11:00:00.000Z
layout: post
title: Playing with OCR libraries
subtitle: How to recognize text from images with pytesseract library
description: How to recognize text from images with pytesseract library. From
  image to python string
image: /assets/img/uploads/ideas.jpg
optimized_image: /assets/img/uploads/ideas.jpg
category: code
tags:
  - ocr
  - python
author: jaimedearcos
paginate: false
---

A Few days ago I found a cool library for text recognition in images, <a href="https://pypi.org/project/pytesseract/">pytesseract</a>.
With this library and just a few lines of code you can implement OCR features.

## Get started

Install `teseract` in MacOs ([for other platforms](https://tesseract-ocr.github.io/tessdoc/Home.html))

```sh
brew install tesseract
```

Create a new python environment & install requirements

```bash
python -m venv en
source env/bin/activate
pip install -r requirements.txt
```

requirements.txt file
```txt
numpy==1.19.4
opencv-python==4.4.0.46
Pillow==8.0.1
pytesseract==0.3.6
```

## Create your first ocr script

```python
from PIL import Image
import pytesseract
import argparse
import sys

original_image = "image.jpg"
image = cv2.imread(original_image)
gray = cv2.cvtColor(image, cv2.COLOR_BGR2GRAY)

# Convert to grayscale for better performance
filename = "grayscale_image.png"
cv2.imwrite(filename, gray)

# Load the image as a PIL/Pillow image and apply OCR
text = pytesseract.image_to_string(Image.open(filename))
os.remove(filename)
print(text)
```

You can find the complete code of this example in [this GitHub repository](https://github.com/JaimeDeArcos/ocr-python)

## References

- [PyTesseract pip documentation](https://pypi.org/project/pytesseract/)