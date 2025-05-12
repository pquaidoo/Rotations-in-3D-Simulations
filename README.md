# Rotations in 3D Simulations

**Author:** Patrick Kristopher Quaidoo
**Date:** December 2024

---

## Abstract

In this paper, we explore the mathematical and computational methods used to represent and perform rotations in 3D simulations, focusing on Euler rotations and quaternions. We introduce Euler rotations, a commonly used and intuitive system for representing 3D orientations based on pitch, yaw, and bank. We also address the inherent limitations of Euler rotations, such as gimbal lock.

To overcome these limitations, we delve into quaternions, a powerful mathematical tool for representing 3D rotations, offering advantages such as gimbal-lock-free interpolation, computational efficiency, and robustness. We detail quaternion properties, conversion methods, and interpolation techniques like SLERP, providing practical examples to illustrate their effectiveness in 3D simulations.

---

## Introduction

Rotations are fundamental in 3D simulations, enabling realistic movement and interaction. We first present Euler rotations, their pros and cons, then introduce quaternions, and finally demonstrate a rotation example using quaternions.

---

## Euler Rotations

Euler rotations operate using three axes: pitch, yaw, and bank (representing rotations around x, y, z respectively).

You can visualize this like a gimbal, where 3 nested circles represent the axes.

### Rotation Matrices

```
B = Rz(b) = [
  cos(b)  sin(b)  0
 -sin(b)  cos(b)  0
  0       0       1
]

P = Rx(p) = [
  1       0        0
  0   cos(p)  sin(p)
  0  -sin(p)  cos(p)
]

H = Ry(h) = [
  cos(h)  0  sin(h)
  0       1      0
 -sin(h)  0  cos(h)
]
```

Multiplying them together produces the final rotation matrix.

---

## Gimbal Lock

Gimbal lock occurs when two axes align, causing the loss of one degree of freedom. This makes interpolation (smooth transition) between orientations problematic.

![Gimbal Lock Example](Gimbal%20lockex.jpg)

In 3D animations, this is a critical issue since interpolations between frames must be smooth and predictable.

---

## Quaternions

Quaternions extend rotations into 4D space and avoid gimbal lock.

They are defined as:

```
q = (q0, q1, q2, q3)

q0 = cos(theta / 2)
q1 = x * sin(theta / 2)
q2 = y * sin(theta / 2)
q3 = z * sin(theta / 2)
```

### Key Properties

* **Negation:** `-q` describes the same rotation as `q`.
* **Conjugate:** `q* = [w, -v]`
* **Inverse:** For unit quaternions: `q^-1 = q*`
* **Multiplication:** Quaternion multiplication is non-commutative but associative.

---

## Example Rotation

If we want to rotate a unit sphere by 90° around the x-axis:

![Initial Sphere](qzero.jpg)

![After Rotation](qprime.jpg)

---

### Euler to Quaternion Conversion

Given Euler rotations:

```
(b = 0°, p = 90°, h = 0°)
```

You can calculate the corresponding quaternion using the formulas previously shown.

### Applying the Rotation

The rotation quaternion for 90° about the x-axis:

```
qr = (sqrt(2)/2, sqrt(2)/2, 0, 0)
```

Final quaternion after rotation:

```
q' = qr * q0
```

---

### SLERP (Spherical Linear Interpolation)

```
SLERP(q0, q', t) = (sin((1 - t) * theta) / sin(theta)) * q1 + (sin(t * theta) / sin(theta)) * q2
```

Allows smooth animation between `q0` and `q'`.

---

## Conclusion

Euler rotations are intuitive but prone to gimbal lock, making them less ideal for interpolation tasks in 3D simulations.

Quaternions, although less intuitive, provide robust, efficient, and gimbal-lock-free rotation capabilities, especially when using SLERP for interpolation.

---

## References

1. John Horton Conway, Derek A Smith, *On Quaternions and Octonions*, CRC Press, 2013.
2. Rose, D. "Rotation Quaternions, and How to Use Them." [danceswithcode.net](https://danceswithcode.net/engineeringnotes/quaternions/quaternions.html), 2015.
3. Sanderson, Grant, and Ben Eater. "Visualizing Quaternions." [eater.net](https://eater.net/quaternions), 2018.
4. Wikipedia Contributors. "Rotation Matrix." [Wikipedia](https://en.wikipedia.org/wiki/Rotation_matrix), 2019.
5. Dunn, Fletcher, and Ian Parberry. *3D Math Primer for Graphics and Game Development*, CRC Press, 2012.
