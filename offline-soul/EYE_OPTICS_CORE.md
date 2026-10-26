# Eye Optics Core

Pure mathematical models. No camera access is claimed or required.

## 1. Eye Aspect Ratio (EAR)

Six periocular landmarks p1…p6:

- p1, p4 = horizontal eye corners
- p2, p6 and p3, p5 = vertical eyelid pairs

```
EAR = (‖p2 − p6‖ + ‖p3 − p5‖) / (2 · ‖p1 − p4‖)
```

- Open eye: EAR typically stays in a relatively stable range (often ~0.25–0.35)
- Blink / closed: EAR falls rapidly toward 0
- Scale-invariant under ordinary viewing conditions

Python reference:

```python
import math

def eye_aspect_ratio(landmarks):
    """
    landmarks: list of 6 (x, y) points in order p1..p6
    """
    p1, p2, p3, p4, p5, p6 = landmarks

    def dist(a, b):
        return math.hypot(a[0] - b[0], a[1] - b[1])

    v1 = dist(p2, p6)
    v2 = dist(p3, p5)
    h  = dist(p1, p4)

    if h == 0:
        return 0.0
    return (v1 + v2) / (2.0 * h)
```

## 2. Spherical Eye Model (Gaze)

- Average adult eye diameter ≈ 24 mm
- Eye treated as a sphere with rotation center e
- Optical axis = vector from e through the 3D pupil center
- Visual axis is offset from the optical axis by the individual kappa angle (not assumed here)

```python
import numpy as np

def gaze_ray(eye_center, pupil_center_3d):
    direction = np.asarray(pupil_center_3d) - np.asarray(eye_center)
    norm = np.linalg.norm(direction)
    if norm == 0:
        return np.array([0.0, 0.0, 1.0])
    return direction / norm
```

## 3. Pupil Foreshortening

When the pupil is viewed off-axis it projects as an ellipse.  
Apparent size is reduced roughly by cos(θ), where θ is the angle between the optical axis and the camera line of sight.  
Any pupillometry that ignores this angle is systematically biased.

## 4. Basic Optical Constants (schematic)

- Cornea radius of curvature ≈ 7.8 mm
- Entrance pupil = virtual image of the real iris aperture as seen through the cornea

These values are population averages used in schematic eye models. Individual eyes vary.

---

All formulas above are geometry and optics only.  
They invent nothing beyond the recorded models.  
They do not claim to read minds, detect deception, or replace judgment.
