
# Surface Area of a Plane in the First Octant

## Problem Statement

Determine the surface area of the portion of the plane $2x + 3y + 6z = 9$ that lies in the first octant.

## Reasoning Process

Okay, let's see. I need to find the surface area of the part of the plane $2x + 3y + 6z = 9$ that's in the first octant. The first octant means all $x$, $y$, $z$ are non-negative. So I should figure out the region on the plane where $x \ge 0$, $y \ge 0$, $z \ge 0$, then find the area of that triangular surface.

Planes in 3D can form triangles when intersected with the first octant, depending on where they cut the axes. I should find the intercepts first. To find intercepts, set the other variables to zero.

For the x-intercept, set $y = 0$ and $z = 0$. Then $2x = 9$, so $x = 9/2 = 4.5$.

For the y-intercept, set $x = 0$ and $z = 0$. Then $3y = 9$, so $y = 3$.

For the z-intercept, set $x = 0$ and $y = 0$. Then $6z = 9$, so $z = 1.5$.

So the plane intersects the x-axis at $(4.5, 0, 0)$, y-axis at $(0, 3, 0)$, and z-axis at $(0, 0, 1.5)$. In the first octant, the portion of the plane is a triangle connecting these three points. Therefore, the surface area we need is the area of this triangle.

For a triangle in 3D space, I can use the formula involving vectors: half the magnitude of the cross product of two sides.

First, find the vectors that form the triangle. Take intercept points $A(4.5, 0, 0)$, $B(0, 3, 0)$, and $C(0, 0, 1.5)$. Take vectors $\overrightarrow{AB}$ and $\overrightarrow{AC}$.

$\overrightarrow{AB} = B - A = (-4.5, 3, 0)$

$\overrightarrow{AC} = C - A = (-4.5, 0, 1.5)$

Then the cross product of $\overrightarrow{AB}$ and $\overrightarrow{AC}$ is:

$$
\begin{vmatrix}
\mathbf{i} & \mathbf{j} & \mathbf{k} \\
-4.5 & 3 & 0 \\
-4.5 & 0 & 1.5
\end{vmatrix}
$$

Calculating the determinant:

$$\mathbf{i}(3 \cdot 1.5 - 0 \cdot 0) - \mathbf{j}(-4.5 \cdot 1.5 - 0 \cdot (-4.5)) + \mathbf{k}(-4.5 \cdot 0 - 3 \cdot (-4.5))$$

$$= \mathbf{i}(4.5) - \mathbf{j}(-6.75) + \mathbf{k}(13.5)$$

So the cross product vector is $(4.5, 6.75, 13.5)$.

The magnitude of this vector is $\sqrt{4.5^2 + 6.75^2 + 13.5^2}$.

Compute each term:

- $4.5^2 = 20.25$
- $6.75^2 = 45.5625$ (since $6.75 = 27/4$, $(27/4)^2 = 729/16 = 45.5625$)
- $13.5^2 = 182.25$ (since $13.5 = 27/2$, squared is $729/4 = 182.25$)

Sum: $20.25 + 45.5625 + 182.25 = 248.0625$

The square root of $248.0625$ is $15.75$, because $15.75^2 = (15 + 0.75)^2 = 225 + 22.5 + 0.5625 = 248.0625$.

So the magnitude of the cross product is $15.75$.

The area is half of that: $15.75 / 2 = 7.875$, which is $63/8$.

### Verification Using a Surface Integral

I can also verify this using a surface integral. Parameterize the surface using $x$ and $y$. From $2x + 3y + 6z = 9$, solve for $z$:

$$z = \frac{9 - 2x - 3y}{6}$$

The domain $D$ in the $xy$-plane satisfies $x \ge 0$, $y \ge 0$, and $z \ge 0$, so $9 - 2x - 3y \ge 0$, meaning $2x + 3y \le 9$. The domain is the triangle with vertices $(0, 0)$, $(4.5, 0)$, and $(0, 3)$.

The surface area formula is:

$$\iint_D \sqrt{\left(\frac{\partial z}{\partial x}\right)^2 + \left(\frac{\partial z}{\partial y}\right)^2 + 1} \, dA$$

Compute the partial derivatives:

$$\frac{\partial z}{\partial x} = -\frac{2}{6} = -\frac{1}{3}, \qquad \frac{\partial z}{\partial y} = -\frac{3}{6} = -\frac{1}{2}$$

So:

$$\left(\frac{\partial z}{\partial x}\right)^2 + \left(\frac{\partial z}{\partial y}\right)^2 + 1 = \frac{1}{9} + \frac{1}{4} + 1 = \frac{4}{36} + \frac{9}{36} + 1 = \frac{13}{36} + 1 = \frac{49}{36}$$

Therefore:

$$\sqrt{\frac{49}{36}} = \frac{7}{6}$$

The surface area is the integral over $D$ of $7/6 \, dA$, which equals $7/6$ times the area of $D$.

The area of $D$ is $\frac{1}{2} \times 4.5 \times 3 = 6.75$.

Surface area:

$$\frac{7}{6} \times 6.75 = 7 \times 1.125 = 7.875 = \frac{63}{8}$$

Both methods give the same result, confirming the surface area is $63/8$.

## Formal Solution

### Step 1: Find the Intercepts

Set two variables to zero at a time to find where the plane intersects the axes:

- **x-intercept**: Set $y = 0$, $z = 0$:
  $$2x = 9 \implies x = \frac{9}{2} = 4.5$$

- **y-intercept**: Set $x = 0$, $z = 0$:
  $$3y = 9 \implies y = 3$$

- **z-intercept**: Set $x = 0$, $y = 0$:
  $$6z = 9 \implies z = \frac{9}{6} = 1.5$$

The intercept points are $A(4.5, 0, 0)$, $B(0, 3, 0)$, and $C(0, 0, 1.5)$.

### Step 2: Determine Vectors

From point $A$:

$$\overrightarrow{AB} = B - A = (-4.5, 3, 0)$$

$$\overrightarrow{AC} = C - A = (-4.5, 0, 1.5)$$

### Step 3: Calculate the Cross Product

$$\overrightarrow{AB} \times \overrightarrow{AC} = \begin{vmatrix} \mathbf{i} & \mathbf{j} & \mathbf{k} \\ -4.5 & 3 & 0 \\ -4.5 & 0 & 1.5 \end{vmatrix}$$

$$= \mathbf{i}(3 \cdot 1.5 - 0 \cdot 0) - \mathbf{j}(-4.5 \cdot 1.5 - 0 \cdot (-4.5)) + \mathbf{k}(-4.5 \cdot 0 - 3 \cdot (-4.5))$$

$$= (4.5, 6.75, 13.5)$$

### Step 4: Compute the Magnitude

$$\|\overrightarrow{AB} \times \overrightarrow{AC}\| = \sqrt{4.5^2 + 6.75^2 + 13.5^2} = \sqrt{20.25 + 45.5625 + 182.25} = \sqrt{248.0625} = 15.75$$

### Step 5: Calculate the Triangle Area

$$\text{Area} = \frac{1}{2} \|\overrightarrow{AB} \times \overrightarrow{AC}\| = \frac{1}{2} \times 15.75 = 7.875 = \frac{63}{8}$$

## Final Answer

The surface area of the portion of the plane in the first octant is

$$\boxed{\dfrac{63}{8}}$$
