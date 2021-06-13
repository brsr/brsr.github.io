---
layout: post
title: "Spherical geometry with vectors"
tags: geometry vectors cartography
---
Geometric constructions on the sphere are often simpler when expressed using 3-dimensional unit vectors. This method sees some use in geography, where it is called the [n-vector representation](https://en.wikipedia.org/wiki/N-vector): [other sites have more formulas for n-vector based geodesy](https://www.movable-type.co.uk/scripts/latlong-vectors.html). (Technically n-vectors account for the slightly flattened ellipsoidal shape of the earth, but the set of n-vectors forms a unit sphere.) It can also be faster, avoiding trig functions. Also, vectors are the natural representation for applications like 3D rendering. Furthermore, these formula allow you to plug-and-chug a lot of spherical geometry problems, rather than having to rely on geometric insight. (That might not be a plus for some people, which is fair.) This document is a collection of notes on general spherical geometry using vectors. It doesn't focus on its application to geodesy, but geodesy is mentioned where applicable.

A unit vector is any vector $$\mathbf{\hat{v}} = [v_x, v_y, v_z]$$ with Euclidean norm 1: $$v_x^2 + v_y^2 + v_z^2 = 1$$. Vectors are written in bold, and unit vectors are written with a hat. Often in these calculations the result is a vectors that must be normalized to unit length, like $$\mathbf{\hat{v}} = \frac{\mathbf v}{\|\mathbf v\|}$$. For longer equations this gets repetitive, so sometimes it is convenient to abbreviate the denominator as $$\mathbf{\hat{v}} = \frac{\mathbf v}{\|\ldots\|}$$, where $$\|\ldots\|$$ is the norm of the numerator. Note that if $$\mathbf v$$ is the zero vector, then $$\mathbf{\hat{v}}$$ is undefined.

These formula may fail for some specific configurations of points, often when two points are antipodal to each other. For instance, for most choices of two points on the sphere, there is a single point halfway between two points on the sphere. However, if one point is the North Pole and the other is the South Pole, every point on the equator is halfway between the poles. Often, if the formula fails, it's because there are multiple solutions or no solutions.

In general the notation used here is standard 3-dimensional vector algebra notation. The scalar triple product is written $$\begin{vmatrix} \mathbf{a}, \mathbf{b}, \mathbf{c} \end{vmatrix}$$ instead of $$\mathbf{a} \cdot \mathbf{b} \times \mathbf{c}$$. For a summary of relevant vector identities, see [Wikipedia's page](https://en.wikipedia.org/wiki/Vector_algebra_relations).

Because of their importance for geodesy, it's convenient to define a few static points. The north pole is denoted $$\mathbf{\hat{n}} = [0,0,1]$$ and the point at latitude 0 and longitude 0 is denoted $$\mathbf{\hat{o}} = [1,0,0]$$. The center of the great circle passing through those points, that is, the prime meridian and 180th meridian is $$\mathbf{\hat{p}} = [0,1,0]$$ (the significance of which will be explained later).

# Converting latitude and longitude to vector
Let latitude be $$\varphi$$ and longitude be $$\lambda$$, then the forward
conversion is:

$$
\mathbf{\hat{v}} =
\begin{bmatrix}
  v_x \\ v_y \\ v_z
\end{bmatrix}
=
\begin{bmatrix}
 \cos(\varphi) \cos(\lambda) \\
 \cos(\varphi) \sin(\lambda) \\
 \sin(\varphi)
\end{bmatrix}.
$$

The reverse conversion is:

$$
\begin{split}
  \varphi =& \arcsin(v_z) = \arctan\left(\frac{v_z}{\sqrt{v_y^2 + v_x^2}}\right), \\
  \lambda =& \arctan\left(v_y, v_x\right),
\end{split}
$$

where [the 2-variable form of $$\arctan$$, commonly called `arctan2` or `atan2` in numeric software libraries](https://en.wikipedia.org/wiki/Atan2), is used for $$\lambda$$.

# Random vector on the unit sphere
A random vector on the unit sphere can be produced by creating a 3D vector, each of whose components is drawn from a normal distribution with mean 0 and constant standard deviation, and then normalizing. The distribution of vectors drawn this way is uniform across the sphere.

# Distance
Distance on the unit sphere is simply the central angle between the two unit vectors. The distance function on the sphere has a few equivalent forms:

$$
d\left(\mathbf{\hat{a}}, \mathbf{\hat{b}}\right)
= \arccos\left(\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right)
= \arcsin\left(\| \mathbf{\hat{a}} \times \mathbf{\hat{b}} \|\right)
= \arctan\left(\frac{\| \mathbf{a} \times \mathbf{b} \|}{\mathbf{a} \cdot \mathbf{b}}\right).
$$

The central angle takes values from 0 to $$\pi$$ in radians. Radians are convenient here because they make angles and distances equivalent. Note that the $$\arcsin$$ form is only valid for distances less than $$\pi/2$$. The form using $$\arctan$$ is valid for all non-zero vectors: it is also numerically accurate over the entire range.

The distance function is bounded from below by the Euclidean distance between the unit vectors:

$$
d\left(\mathbf{\hat{a}}, \mathbf{\hat{b}}\right) \ge \|\mathbf{\hat{a}} - \mathbf{\hat{b}}\|.
$$

For small distances, the Euclidian distance is a good approximation of the spherical distance.

Calculating distances is also called the reverse geodetic problem, although the problem on an ellipsoid instead of a sphere is much more complicated.

# Circles and lines
<img src="/assets/images/spherical/circles.svg" alt="Concentric circles on the sphere" height="300" style="float:right">
Given a center vector $$\mathbf{\hat{c}}$$ and a constant radius $$r$$ in $$(0, \pi)$$, the solutions for $$\mathbf{\hat{v}}$$ of equation $$d\left(\mathbf{\hat{c}}, \mathbf{\hat{v}}\right) = r$$ make a circle on the sphere. If $$r = 0$$, the solution is just the original point $$\mathbf{\hat{c}}$$. If $$r= \pi$$, the solution is the antipodal point $$-\mathbf{\hat{c}}$$. If $$r= \pi / 2$$, the circle is called a great circle: otherwise, it is called a small circle. Some concentric circles on the sphere are pictured to the right.

Rearranging the distance function form that uses cosine gives $$\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} = \cos r$$, which is the [Hesse normal form](https://en.wikipedia.org/wiki/Hesse_normal_form) of a plane. $$\mathbf{\hat{c}}$$ is a unit normal vector to the plane. These circles are just the intersection of a plane with the sphere: furthermore, circles on the surface of the sphere are also circles in 3D Euclidean space.

Great circles are the lines of shortest distance between two points, or geodesics, on the sphere. For a great circle, $$k=\pi/2$$, and $$\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} = 0$$. That is, a great circle is determined by its center point, also called the pole of the great circle, or the normal of the plane of the great circle. This text uses "center of the great circle", mostly because I think it's the more intuitive choice.

Given any two points on a great circle, the center of that great circle is

$$\mathbf{\hat{c}} = \frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}}{\|\ldots\|}.$$

Of course, $$-\mathbf{\hat{c}}$$ is also a center of that circle, but the two centers correspond to opposite orientations of the circle. Following the [right-hand rule](https://en.wikipedia.org/wiki/Right-hand_rule), the circle with center $$\mathbf{\hat{c}}$$ is oriented counterclockwise, turning from $$\mathbf{\hat{a}}$$ towards $$\mathbf{\hat{b}}$$. Later when spherical polygons show up, those will also be oriented counterclockwise.

To find if a point $$\mathbf{\hat{v}}$$ lies on the "counterclockwise side" of a great circle,
take the dot product $$\mathbf{\hat{v}} \cdot \mathbf{\hat{c}}$$ or the triple product $$\begin{vmatrix} \mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{v}} \end{vmatrix}$$. If it's positive, then $$\mathbf{\hat{v}}$$ is within that oriented great circle.

# Angles of Intersection
<img src="/assets/images/spherical/intersection.svg" alt="Intersecting great circles on the sphere" height="300" style="float:right">
The angle of intersection between two lines on the surface of the sphere is the central angle between the centers of their great circles. In the figure to the right, each colored point is the center of that colored great circle. Note that which angle depends on the orientation of the great circle: the other is given by negating one of the great circle centers, or equivalently as $$\pi - \theta$$. Central angle is the same as distance for the unit sphere, so one can use whichever form of the distance function is most convenient. Which angle depends on the order and orientation of the circles. This does reflect an important concept in spherical geometry: distances are angles, and angles are distances.

The particular case of the interior vertex angle $$\theta_a$$ at $$\mathbf{\hat{a}}$$ in a triangle with vertices $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and $$\mathbf{\hat{c}}$$ is as follows, although if you already have the great circle centers for the edges it may be more convenient to use those as above.

$$\begin{split}
\cos \theta_a &=
\frac{\mathbf{\hat{b}} \cdot \mathbf{\hat{c}} - \left(\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right) \left(\mathbf{\hat{c}} \cdot \mathbf{\hat{a}}\right)}{\|\mathbf{\hat{a}} \times \mathbf{\hat{b}}\|\|\mathbf{\hat{a}} \times \mathbf{\hat{c}}\|} \\

\sin \theta_a &=
\frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right|}{\|\mathbf{\hat{a}} \times \mathbf{\hat{b}}\|\|\mathbf{\hat{a}} \times \mathbf{\hat{c}}\|} \\

\tan \theta_a &= \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right|}
{\mathbf{\hat{b}} \cdot \mathbf{\hat{c}} - \left(\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right) \left(\mathbf{\hat{c}} \cdot \mathbf{\hat{a}}\right)}.
\end{split}
$$

# Cosine and Sine rules
The statement for $$\sin \theta$$ above can be used to derive the [spherical law of sines](https://en.wikipedia.org/wiki/Spherical_law_of_sines):

$$
\frac{\sin \theta_a}{\|\mathbf{\hat{b}} \times \mathbf{\hat{c}}\|} =
\frac{\sin \theta_b}{\|\mathbf{\hat{c}} \times \mathbf{\hat{a}}\|} =
\frac{\sin \theta_c}{\|\mathbf{\hat{a}} \times \mathbf{\hat{b}}\|} =
\frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right|}
{\|\mathbf{\hat{a}} \times \mathbf{\hat{b}}\|
\|\mathbf{\hat{b}} \times \mathbf{\hat{c}}\|
\|\mathbf{\hat{c}} \times \mathbf{\hat{a}}\|}.
$$

The statement above for $$\cos \theta_a$$ is equivalent to [the spherical law of cosines](https://en.wikipedia.org/wiki/Spherical_law_of_cosines):

$$
\cos \theta_a \|\mathbf{\hat{a}} \times \mathbf{\hat{b}}\|\|\mathbf{\hat{c}} \times \mathbf{\hat{a}}\| + \left(\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right) \left(\mathbf{\hat{c}} \cdot \mathbf{\hat{a}}\right) = \mathbf{\hat{b}} \cdot \mathbf{\hat{c}}.
$$

A second triangle, called the polar triangle, can be formed by the centers of each edge's great circle: $$\mathbf{\hat{a}}' = \frac{\mathbf{\hat{b}} \times \mathbf{\hat{c}}}{\|\ldots\|}$$, $$\mathbf{\hat{b}}' = \frac{\mathbf{\hat{c}} \times \mathbf{\hat{a}}}{\|\ldots\|}$$, and $$\mathbf{\hat{c}}' = \frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}}{\|\ldots\|}$$. Applying the law of cosines to this new triangle gives the spherical law of cosines for angles, although it doesn't change much in vector form:

$$
\left(\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right) \sin \theta_a \sin \theta_b - \cos \theta_a \cos \theta_b = \cos \theta_c.
$$

# Point of intersection of great circles
The points of intersection between two great circles are given by the cross product of their centers:

$$\mathbf{\hat{v}} = \frac{\mathbf{\hat{c}}_1 \times \mathbf{\hat{c}}_2}
{\|\ldots\|}.$$

There are two points of intersection, the other given by swapping the two great circle vectors or negating.

A common problem: given the great circle formed by $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{b}}$$, and the great circle formed by $$\mathbf{\hat{c}}$$ and $$\mathbf{\hat{d}}$$, where do the two great circles intersect? This is $$\mathbf{\hat{v}} = \frac{\left(\mathbf{\hat{a}} \times \mathbf{\hat{b}}\right) \times \left(\mathbf{\hat{c}} \times \mathbf{\hat{d}}\right)}{\|\ldots\|}$$. Since scalar factors can be removed from the cross product, all the normalization factors are lumped together into the bottom. This may be simplified using the [vector quadruple product identity](https://en.wikipedia.org/wiki/Quadruple_product#Vector_quadruple_product) into

$$
\begin{split}
\mathbf{\hat{v}}
& = \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{d}}\right| \mathbf{\hat{c}} - \left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right| \mathbf{\hat{d}}}{\|\ldots\|} \\
& = \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{c}}, \mathbf{\hat{d}}\right| \mathbf{\hat{b}} - \left|\mathbf{\hat{b}}, \mathbf{\hat{c}}, \mathbf{\hat{d}}\right| \mathbf{\hat{a}}}{\|\ldots\|},
\end{split}
$$

which makes it clear that the result lies on both great circles.

# Rotation
Rotation around an axis $$\mathbf{\hat{c}}$$ can be expressed using [Rodrigues' rotation formula](https://en.wikipedia.org/wiki/Rodrigues%27_rotation_formula). Let $$\theta$$ be the angle of rotation in the counterclockwise direction, then:

$$
\mathbf{v}_\mathrm{\theta} = \mathbf{v} \cos\theta + (\mathbf{\hat{c}} \times \mathbf{v}) \sin\theta + \mathbf{\hat{c}} ~(\mathbf{\hat{c}} \cdot \mathbf{v}) (1 - \cos\theta).
$$

This formula can also be expressed as a rotation matrix $$\mathbf{R}$$ such that $$\mathbf{v}_\mathrm{\theta} = \mathbf{R} \mathbf{v}$$:

$$
\mathbf{R} = \mathbf{I} + (\sin\theta) \mathbf{C} + (1-\cos\theta)\mathbf{C}^2,
$$

where $$\mathbf{C}$$ is a matrix representation of the cross product $$\mathbf{\hat{c}} \times$$:

$$
\mathbf{C}=\begin{bmatrix}
0 & -c_z & c_y \\
c_z & 0 & -c_x \\
-c_y & c_x & 0
\end{bmatrix}.
$$

Of course, certain rotation axes result in simpler rotation matrices. Rotation around the north pole, for instance, can be represented as this matrix:

$$
\mathbf{R}=\begin{bmatrix}
\cos \theta & -\sin \theta & 0 \\
\sin \theta & \cos \theta  & 0 \\
0 & 0 & 1
\end{bmatrix}.
$$

If $$\mathbf{v}$$ lies on the great circle with center $$\mathbf{\hat{c}}$$, the last term of the Rodrigues rotation is zero, and

$$
\mathbf{v}_\mathrm{\theta} = \mathbf{v} \cos\theta + (\mathbf{\hat{c}} \times \mathbf{v}) \sin\theta.
$$

This formula can be used to solve problems where we want a point a certain distance and direction from another point: rotate it along a great circle with the right orientation. The next section gives a specific case.

# The forward geodetic problem
<img src="/assets/images/spherical/forward.svg" alt="The forward geodetic problem on the sphere" height="300" style="float:right">
Here is a common geodesic calculation, called the direct or forward geodetic problem. Given a point $$\mathbf{\hat{a}}$$ on the globe, a bearing $$\theta$$, and a distance $$\ell$$, what is the destination point $$\mathbf{\hat{b}}$$? Bearing is the clockwise angle with respect to the north pole. Take these steps:

1. Calculate the center $$\mathbf{\hat{c}}_1$$ of the great circle through the North Pole $$\mathbf{\hat{n}}$$ and $$\mathbf{\hat{a}}$$.
2. Using the Rodrigues formula for points on a great circle, rotate that center $$\mathbf{\hat{c}}_1$$ by $$\theta$$ around $$\mathbf{\hat{a}}$$, giving a new great circle center $$\mathbf{\hat{c}}_2$$. (Keep orientation in mind: bearing is usually clockwise from north.)
3. Using the Rodrigues formula for points on a great circle again, rotate $$\mathbf{\hat{a}}$$ by $$\ell$$ around $$\mathbf{\hat{c}}_2$$, giving $$\mathbf{\hat{b}}$$. Remember, spherical distances are angles and vice versa.

# Interpolation

Let $$\ell < \pi$$ be the spherical distance between unit vectors $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{b}}$$. Then the point that is a fraction $$t$$ of the distance between the two is given by the "slerp" formula:

$$
\operatorname{Slerp}(\mathbf{\hat{a}},\mathbf{\hat{b}}; t) =
\mathbf{\hat{a}} \frac{\sin {\left( (1-t)\ell\right)}}{\sin \ell} +
\mathbf{\hat{b}}\frac{\sin \left(t\ell\right)}{\sin \ell}.
$$

If $$\ell = \frac{\pi}{2}$$, the slerp formula simplifies to

$$
\operatorname{Slerp}(\mathbf{\hat{a}},\mathbf{\hat{b}}; t) = \mathbf{\hat{a}} \cos {(t\ell)}  + \mathbf{\hat{b}} \sin{(t\ell)},
$$

which is the Rodrigues rotation formula when the rotated point lies on the great circle, with an axis of rotation given by $$\mathbf{\hat{c}} = \mathbf{\hat{a}} \times \mathbf{\hat{b}}$$. (Since $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{b}}$$ are at right angles to each other, no normalization is needed.)

This formula can be applied to $$t$$ outside of the interval $$[0, 1]$$, in which case it may wrap around the sphere.

One particular special case: the point midway between $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{b}}$$ is

$$
\mathbf{\hat{m}} =
\frac{\mathbf{\hat{a}}+\mathbf{\hat{b}}}
{\sqrt{2 + 2~\mathbf{\hat{a}}\cdot \mathbf{\hat{b}}}}.
$$

# Perpendicular through a point
<img src="/assets/images/spherical/perpendicular.svg" alt="Constructing a perpendicular through a point" height="300" style="float:right">
Let $$\mathbf{\hat{c}}$$ be the center of some circle on the sphere (not necessarily great): the red point in the figure to the right. Any other great circle that passes through $$\mathbf{\hat{c}}$$ is perpendicular to the original circle. If $$\mathbf{\hat{a}}$$ (the green point) is an arbitrary point, then the great circle that passes through $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{c}}$$, and is thus perpendicular to the other great circle centered at $$\mathbf{\hat{c}}$$, is

$$
\mathbf{\hat{g}} = \frac{\mathbf{\hat{a}} \times \mathbf{\hat{c}}}{\|\ldots\|}.
$$

The point of intersection (in black) is (with some simplification):

$$
\mathbf{\hat{x}} = \frac{\mathbf{\hat{c}} (\mathbf{\hat{a}} \cdot \mathbf{\hat{c}}) - \mathbf{\hat{a}}}{\|\ldots\|}.
$$

# Triangles
<img src="/assets/images/spherical/triangle.svg" alt="A spherical triangle" height="300" style="float:right">
Let the triangle have vertices $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and $$\mathbf{\hat{c}}$$. If the triple product $$\begin{vmatrix} \mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}} \end{vmatrix}$$ is positive, then the triangle is oriented counterclockwise, which is generally what we will assume. If it's zero, the triangle's vertices lie on a great circle. That can either mean that the triangle is degenerate and has area zero, or that the triangle takes up an entire hemisphere and has area $$2\pi$$. Hemispherical triangles often result in an undefined result, because of the ambiguity of orientation.

To find if a point $$\mathbf{\hat{v}}$$ lies within a triangle, calculate $$\begin{vmatrix} \mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{v}} \end{vmatrix}$$, $$\begin{vmatrix} \mathbf{\hat{b}}, \mathbf{\hat{c}}, \mathbf{\hat{v}} \end{vmatrix}$$, and $$\begin{vmatrix} \mathbf{\hat{c}}, \mathbf{\hat{a}}, \mathbf{\hat{v}} \end{vmatrix}$$. If each one is positive, then $$\mathbf{\hat{v}}$$ lies within the triangle. In fact, the signs of each of these quantities divides the sphere into eight regions.

# Area of a triangle
The sum of angles in a triangle on the unit sphere is larger than that of a Euclidean triangle ($$\pi$$). The amount by which it is larger (the "spherical excess") is in fact equal to the area of the triangle, expressed in square radians (steradians). Where $$\theta_i$$ is the angle at a vertex of a spherical triangle, the area $$\Omega$$ is:

$$
\Omega = \theta_a + \theta_b + \theta_c - \pi.
$$

This extends to general polygons on the sphere as

$$
\Omega = \left( \sum^N_{n=1} \theta_i \right)- (N-2)\pi.
$$

There are other formulas for the area of a triangle that do not require calculating angles first.

$$
\begin{split}
\tan \left( \frac{\Omega}{2} \right) &= \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right|}
  {1 + \mathbf{\hat{a}} \cdot \mathbf{\hat{b}} + \mathbf{\hat{b}} \cdot \mathbf{\hat{c}} + \mathbf{\hat{c}} \cdot \mathbf{\hat{a}}}, \\
\sin \left( \frac{\Omega}{2} \right) &= \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}\right|}
  {\sqrt{2
  \left(1 + \mathbf{\hat{a}} \cdot \mathbf{\hat{b}}\right)
  \left(1 + \mathbf{\hat{b}} \cdot \mathbf{\hat{c}}\right)
  \left(1 + \mathbf{\hat{c}} \cdot \mathbf{\hat{a}}\right)
}}.
\end{split}
$$

[The first formula was known to Euler and Lagrange](https://www.tandfonline.com/doi/abs/10.1080/0025570X.1990.11977515). The second, from [an arXiv paper](https://arxiv.org/abs/1307.2567), has an equivalent form in terms of the midpoint of each edge. Let $$\mathbf{\hat{m}}_a$$ be the midpoint opposite vertex $$\mathbf{\hat{a}}$$, and so on for the other vertices. Then

$$
\sin \left( \frac{\Omega}{2} \right) =\left|\mathbf{\hat{m}}_a, \mathbf{\hat{m}}_b, \mathbf{\hat{m}}_c \right|.
$$

These formulas can be applied to higher polygons by splitting that polygon into triangles.

Like distance, small spherical triangle areas are approximated by the Euclidean area:

$$
\Omega \approx A = \frac{1}{2}\| \mathbf{\hat{a}} \times \mathbf{\hat{b}} + \mathbf{\hat{b}} \times \mathbf{\hat{c}} + \mathbf{\hat{c}} \times \mathbf{\hat{a}}\|.
$$

# Area and perimeter of a circle
The surface area of the entire sphere is $$4\pi$$, so the area enclosed by a great circle is $$2\pi$$. Its perimeter is also $$2\pi$$ (on a unit sphere).

The area of a spherical circle with radius $$r$$ was known in antiquity, and is

$$
\Omega = 2\pi \left(1-\cos r\right) = 4\pi \sin^2 \left(\frac{r}{2}\right).
$$

This is approximately $$\pi r^2$$ for small $$r$$. The perimeter is $$2\pi \sin r$$, which is approximately $$2\pi r$$ for small $$r$$.

*A Python script to create the figures in this post is located [here](https://github.com/brsr/mapproj/blob/master/bin/vector-spherical-geometry.py).*
