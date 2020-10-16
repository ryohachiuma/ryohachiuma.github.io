---
title: 'OpenCV-Python tips'
date: 2020-01-02
permalink: /posts/python/cv2/
excerpt: "OpenCV-Python Tips"
tags:
  - python

---

# OpenCV-Python Tips

いつも調べることを備忘録としてまとめる

## Numpy

### Numpy array to OpenCV array

- [Converting Numpy Array to OpenCV Array](https://stackoverflow.com/questions/7587490/converting-numpy-array-to-opencv-array)

```python
import numpy as np, cv2
np_img = np.zeros((400, 300), np.float32)
cv2_img = cv2.cvtColor(np_img, cv2.COLOR_GRAY2BGR)
cv2.imshow("cv2_img", cv2_img)  # Verify output
```

