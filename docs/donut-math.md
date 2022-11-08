# Donut Math
This implementation follows the math from [Andy Sloane's implementation][donut-math]. I recommend reading his implementation as he goes more in depth.

## Projection
It uses a [raycasted projection][ray-casting] following this rendering equation: 
$( x’, y’ ) = ( \frac{K_1 x}{K_2 + z} , \frac{K_1 y}{K_2 + z} )$
where z is the constant $K_1$ representing the field of view and $K_2$ is the distance of the object from the viewer.
We maintain a z-buffer storing the z-coordinate of every drawn point and when plotting we check if the point is in front of already existing plotted points using the following equation when computing $z^{-1}=\frac{1}{z}$.

## Drawing the donut
Draw a circle of Radius $R_1$ centered at point $(R_2, 0, 0)$ drawn on the xy-plane:
$(x,y,z) = (R_2,0,0) + (R_1 \cos \theta, R_1 \sin \theta, 0)$
where we sweep the angle $\theta$ from $0$ to $2\pi$.

## Rotation Matrixes
These rotation matrixes have been adapted from the basic rotations as described on [wikipedia][rotation-matrix].
#### Rotate about the y-axis:
$\begin{bmatrix}
  \cos \phi & 0 & \sin \phi \\
  0 & 1 & 0 \\
  -\sin \phi & 0 & \cos \phi
\end{bmatrix}$
Here, $\phi$ represents the angle to rotate the y-axis by.

#### Rotate about the x-axis by A:
$\begin{bmatrix}
  1 & 0 & 0 \\
  0 & \cos A & \sin A \\
  0 & -\sin A & \cos A
\end{bmatrix}$

#### Rotate about the z-axis by B:
$\begin{bmatrix}
  \cos B &\sin B & 0 \\
  -\sin B & \cos B & 0 \\
  0 & 0 & 1
\end{bmatrix}$

## Luminance
To get the luminance of a point we start by getting the surface normal:
$\left( \begin{matrix} N_x, & N_y, & N_z \end{matrix} \right) = \left( \begin{matrix} \cos \theta, & \sin \theta, & 0 \end{matrix} \right) \cdot \left( \begin{matrix} \cos \phi & 0 & \sin \phi \\ 0 & 1 & 0 \\ -\sin \phi & 0 & \cos \phi \end{matrix} \right) \cdot \left( \begin{matrix} 1 & 0 & 0 \\ 0 & \cos A & \sin A \\ 0 & -\sin A & \cos A \end{matrix} \right) \cdot \left( \begin{matrix} \cos B & \sin B & 0 \\ -\sin B & \cos B & 0 \\ 0 & 0 & 1 \end{matrix} \right)$
and multiply it by the wanted light direction (in this case 0, 1, -1):
$L=(N_x, N_y, N_z)\cdot(0, 1, -1)$

Map the dot product of the equation above to some characters to tweak the lighting:
| Luminance | Character  |
|---|---|
| 1 | . |
| 2 | , |
| 3 | - |
| 4 | ~ |
| 5 | : |
| 6 | ; |
| 7 | = |
| 8 | ! |
| 9 | * |
| 10 | # |
| 11 | $ |
| 12 | @ |

[donut-math]: https://www.a1k0n.net/2011/07/20/donut-math.html
[ray-casting]: https://en.wikipedia.org/wiki/Ray_casting
[rotation-matrix]: https://en.wikipedia.org/wiki/Rotation_matrix#Basic_rotations