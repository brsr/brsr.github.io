---
layout: post
title: "Multilateration on the sphere with vectors"
tags: geometry

images2:
  - name: Two intersecting circles
    link: /assets/images/spherical/bilat_45.svg
  - name: Two touching circles
    link: /assets/images/spherical/bilat_37.97.svg
  - name: Two non-intersecting circles
    link: /assets/images/spherical/bilat_10.svg

images3:
  - name: No single point of intersection
    link: /assets/images/spherical/trilat_20.svg
  - name: Single point of intersection
    link: /assets/images/spherical/trilat_40.svg
  - name: No single point of intersection
    link: /assets/images/spherical/trilat_60.svg

images4:
  - name: Solution outside all circles
    link: /assets/images/spherical/multilat_20.svg
  - name: Solution inside some circles
    link: /assets/images/spherical/multilat_50.svg

imagewidth: 32%
---
[An earlier article]({% post_url 2021-05-01-vector-spherical-geometry %}) mentioned how to find the intersection of two great circles on the sphere using vectors. Another common problem is to find the intersection of small circles, or equivalently, the point at a certain distance from a set of other points. This is called multilateration in the general case, or bi- or trilateration in the case of 2 or 3 distances. Sometimes the circles do not meet exactly, and an approximate solution is required.

Circles on the unit sphere are the intersection of the plane $$\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} = \cos r$$ with the sphere, where $$\mathbf{\hat{c}}$$ is the center of the circle and $$r$$ is its radius. Temporarily ignoring the restriction to the sphere, a set of two or more planes forms a linear system.

In the following, $$\begin{bmatrix} \mathbf{c}_1 & \mathbf{c}_2 & \ldots \end{bmatrix}$$ denotes a matrix whose columns are $$\mathbf{c}_1$$, $$\mathbf{c}_2$$, etc.

# Two
Two planes intersect in a line, unless they are parallel.

$$
\begin{bmatrix}
\mathbf{\hat{c}}_1 & \mathbf{\hat{c}}_2
\end{bmatrix} \mathbf{v} = \begin{bmatrix}
\cos r_1 \\
\cos r_2
\end{bmatrix}
$$

This has general solution:

$$
\mathbf{v} = h_1 \mathbf{\hat{c}}_1 + h_2 \mathbf{\hat{c}}_2 + t (\mathbf{\hat{c}}_1 \times \mathbf{\hat{c}}_2)
$$

where

$$h_1 = \frac{\cos r_1 - \cos r_2 \left(\mathbf{\hat{c}}_1 \cdot \mathbf{\hat{c}}_2\right)}{1 -\left(\mathbf{\hat{c}}_1 \cdot \mathbf{\hat{c}}_2\right)^2},$$

$$h_2$$ is the same with $$r_1$$ and $$r_2$$ exchanged, and $$t$$ is a real number. For the point to be on the sphere, $$t$$ must be such that $$\|\mathbf{v}\| = 1$$. That is given by:

$$
t = \pm \frac{\sqrt{1 - h_1^2 - h_2^2 - 2 h_1 h_2 \mathbf{\hat{c}}_1 \cdot \mathbf{\hat{c}}_2}}
{\|\mathbf{\hat{c}}_1 \times \mathbf{\hat{c}}_2\|}.
$$

$$t$$ is zero if the two circles just touch in a single point. If the two circles do not intersect,
$$t$$ is not defined. The least-squares solution, whether or not the circles intersect, is given by letting $$t=0$$ and normalizing $$\mathbf{v}$$ so that it is a unit vector.

{% include image-gallery.html listing = page.images2 width = page.imagewidth %}

# Three
Three planes intersect at a point, unless any pair are parallel to each other. This point may or may not lie on the surface of the sphere. If it does not -- if the circles do not intersect exactly in a single point -- this gives an approximate solution that does lie on the sphere.

$$
\begin{bmatrix}
\mathbf{\hat{c}}_1 & \mathbf{\hat{c}}_2 & \mathbf{\hat{c}}_3
\end{bmatrix} \mathbf{\hat{v}} = \begin{bmatrix}
\cos r_1 \\
\cos r_2 \\
\cos r_3
\end{bmatrix}
$$

Let $$\mathbf{r}$$ be the column vector with entries $$\cos r_i$$, then the solution is

$$
\mathbf{\hat{v}} = \frac{\begin{bmatrix}
\mathbf{\hat{c}}_2 \times \mathbf{\hat{c}}_3 &
\mathbf{\hat{c}}_3 \times \mathbf{\hat{c}}_1 &
\mathbf{\hat{c}}_1 \times \mathbf{\hat{c}}_2
\end{bmatrix}
\mathbf{r}}{\|\ldots\|}
$$

Note that the solution may lie outside the small triangle formed by the circles. This makes sense if you think about the intersection of planes, but can feel wrong if you're thinking about "triangulation" and expect that triangle to be involved somehow. For an alternative, take each pair of circles and calculate the points of intersection, and then use the [spherical centroid]({% post_url 2021-05-02-spherical-triangle-centers %}) of that small triangle. (Assuming the circles all intersect, of course.)
{% include image-gallery.html listing = page.images3 width = page.imagewidth %}

# Four or more

Four or more planes do not in general meet at a single point, but the least squares method can be applied to find an approximate solution.

$$
\begin{bmatrix}
\mathbf{\hat{c}}_1 & \mathbf{\hat{c}}_2 & \ldots
\end{bmatrix} \mathbf{\hat{v}} = \begin{bmatrix}
\cos r_1 \\
\cos r_2 \\
\vdots
\end{bmatrix}
$$

Let $$\mathbf{\hat{c}}_i = [x_i, y_i, z_i]$$. Let $$\mathbf{x}$$ be the vector having $$x_i$$ as its $$i$$th component, and so on for $$\mathbf{y}$$ and $$\mathbf{z}$$. Also let $$\mathbf{r}$$ be the column vector with entries $$\cos r_i$$ as before. Then the least squares solution to the system above is

$$
\begin{bmatrix}
\mathbf{x} \cdot \mathbf{x} & \mathbf{y} \cdot \mathbf{x} & \mathbf{z} \cdot \mathbf{x} \\
\mathbf{x} \cdot \mathbf{y} & \mathbf{y} \cdot \mathbf{y} & \mathbf{z} \cdot \mathbf{y} \\
\mathbf{x} \cdot \mathbf{z} & \mathbf{y} \cdot \mathbf{z} & \mathbf{z} \cdot \mathbf{z}
\end{bmatrix} \mathbf{\hat{v}} = \begin{bmatrix}
\mathbf{r} \cdot \mathbf{x} \\
\mathbf{r} \cdot \mathbf{y} \\
\mathbf{r} \cdot \mathbf{z}
\end{bmatrix}
$$

Given this system, proceed as in the three-plane case. Although the least squares method is used here, the solution does not minimize the squared error in $$r$$, since the elements of $$\mathbf{r}$$ are $$\cos r_i$$, not $$r_i$$. The effect of bringing the vector back onto the sphere also causes deviations from the least-squares solution. However, it's a good approximation.
{% include image-gallery.html listing = page.images4 width = page.imagewidth %}

# Triangulation
Does your application have angles of bearing instead of distances, or a mix of bearings and distances? Convert the bearings to great circles using the method described in [the solving the "forward problem" section of this earlier post]({% post_url 2021-05-01-vector-spherical-geometry %}), and then apply the methods decribed above.
