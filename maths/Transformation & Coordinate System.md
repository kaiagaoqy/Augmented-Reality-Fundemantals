## 1D Transformations


## 1D->2D tramsformations
![[Pasted image 20230206150858.png]]
### 1.  Euclidean transformation 
**Euclidean Transformation**: represent a point with 2D or 3D vectors

### 2. Homogeneous representation
**Homogeneous Representation**: is a uniform representation of rotation, translation, scaling and other transformations, which eases the optimization

#### Why Homogeneous representation
##### 1. Uniform representations & enable pre multiplication

Imagine you want to represent transformations using matrices. Points could be stored as
$$
\begin{bmatrix}
x\\
y\\
\end{bmatrix}$$
and you could represent a **rotation** as
$$
\begin{bmatrix}
u\\
v\\
\end{bmatrix}=\begin{bmatrix}
\cos(\theta) & \sin(\theta)\\
-\sin(\theta) & \cos(\theta)\\
\end{bmatrix}\begin{bmatrix}
x\\
y\\
\end{bmatrix}$$
and **scaling** as
$$
\begin{bmatrix}
u\\
v\\
\end{bmatrix}=\begin{bmatrix}
k_1 & 0\\
0 & k_2\\
\end{bmatrix}\begin{bmatrix}
x\\
y\\
\end{bmatrix}=\begin{bmatrix}
k_1 x\\
k_2 y\\
\end{bmatrix}$$

These are known as linear transformations and they allow us to do transformations as **matrix multiplication**s. 

But notice that you cannot do **translations** as a matrix multiplication. Instead you have to do
$$
\begin{bmatrix}
u\\
v\\
\end{bmatrix}=\begin{bmatrix}
s\\
t\\
\end{bmatrix}+\begin{bmatrix}
x\\
y\\
\end{bmatrix}=\begin{bmatrix}
s + x\\
t + y\\
\end{bmatrix}$$

This is known as an **affine transformation**. However this is undesirable (computationally).

> Let R and S be rotation and scaling matrices and T be a translation vector.

In computer graphics, you may need to do a series of translations to a point. You could imagine how tricky this could get.

Scale, translate, then rotate and scale, then translate again:
$$𝑝′=𝑆𝑅(𝑆𝑝+𝑇)+𝑇$$
Not too bad but imagine you had do this computation on a million points. What we would like is to represent rotation, scaling, and translation all as matrix multiplications. Then those matrices can be **pre multiplied** together for a single transformation matrix which is easy to do computations with.

Scale, translate, then rotate and scale, then translate again:
$$𝑀=𝑇𝑆𝑅𝑇𝑆$$
$$𝑝′=𝑀𝑝$$
We can achieve this by adding another coordinate to our points -> homogeneous coordinates

Let
$$p=\begin{bmatrix}
x\\
y\\
1\\
\end{bmatrix}$$

- Rotation matrix:
$$R = \begin{bmatrix}
\cos(\theta) & \sin(\theta) & 0\\
-\sin(\theta) & \cos(\theta) & 0\\
0 & 0 & 1\\
\end{bmatrix}$$
- Scale matrix:
$$S = \begin{bmatrix}
k_1 & 0 & 0\\
0 & k_2 & 0\\
0 & 0 & 1\\
\end{bmatrix}$$
- Translation matrix:
$$T = \begin{bmatrix}
1 & 0 & s\\
0 & 1& t\\
0 & 0 & 1\\
\end{bmatrix}$$

##### 2. Represent Infinity Points & enable division
- Go further and allow the extra coordinate to take on any value.
$$p = \begin{bmatrix}
x\\
y\\
w\\
\end{bmatrix}\rightarrow point\ \begin{bmatrix}
\frac{x}{w}\\
\frac{y}{w}\\
\end{bmatrix}$$
and say *this this homogeneous (x, y, w) coordinate represents the euclidean (x, y) coordinate at (x/w, y/w).* 

- homogeneous coordinates also allows to represent infinity: 
$$(𝑥,𝑦,0)=\frac{(𝑥,y)}{0} \rightarrow point\; at \ Infinity$$
in 3D, i.e., the point at infinity in direction 𝑥,𝑦,𝑧.
Typically, light sources at finite or infinite position can be represented the same way.

About perspective transform, it even allows to interpolate correctly with no perspective distortion (contrary to early graphics hardware on PC ).


- Normally you cannot do division using matrix transformations, however by allowing w to be a divisor, you can set w to some value (through a matrix multiplication) and allow it to represent division. 

This is useful for doing projection because (in 3D) you will need to divide the x and y coordinates by -z (in a right handed coordinate system). 

You can achieve this by simply setting w to -z using the following projection matrix:


