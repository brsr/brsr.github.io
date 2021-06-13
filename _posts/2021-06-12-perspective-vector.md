---
layout: post
title: "Perspective projections with vectors"
tags: geometry vectors cartography
---
This is the first post in a series on map projections described using unit vectors. Reference [the introductory article on spherical geometry with vectors]({% post_url 2021-05-01-vector-spherical-geometry %}) if needed.

<img src="/assets/images/perspective_projections.svg" alt="Some perspective projections" height="300" align="right"/>
This post describes perspective projections &ndash; such as [orthographic](https://en.wikipedia.org/wiki/Orthographic_map_projection), [stereographic](https://en.wikipedia.org/wiki/Stereographic_map_projection), and [gnomonic](https://en.wikipedia.org/wiki/Gnomonic_projection) &ndash; in terms of vectors. By perspective projections, I mean projections that can be described as projecting an object onto a plane using a certain point of perspective. The point of perspective may be outside, inside, or on the surface of the object. A line is drawn from a point on the object through the point of perspective, and where it meets the plane is the projected point. That said, this article may not be the best introduction to the topic: if not familiar with these projections already, I recommend reading the linked pages.

In the following, let $$\mathbf{\hat{v}}$$ be a point on the sphere, and $$\mathbf{z}$$ be the projected point on the 2d plane. Although some of these projections work with other objects, we'll limit ourselves to the sphere. Let $$\mathbf{\hat{n}} = [0,0,1]$$ be a unit vector pointing towards the north pole. Also let $$\mathbf{Q}$$ be a matrix that drops the third component of a vector:

$$
\mathbf{Q} = \begin{bmatrix}
1 & 0 & 0 \\
0 & 1 & 0
\end{bmatrix}
$$

Projections where the point of projection is vertical to the plane and projected object, rather than tilted with respect to them, are more common, and this post is limited to those. We'll use a plane tangent to the unit sphere at the point $$\mathbf{\hat{c}}$$: $$\mathbf{\hat{c}} \cdot \mathbf{p} = 1$$, where $$\mathbf{p}$$ is a point on the plane. Points with $$\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} > 0$$ are considered to be on the near side of the object. Note that other derivations may place the sphere and plane elsewhere in space: the difference in the projection is usually just a scaling factor or a sign change.

Derivations are saved for the last section, where they're performed for the general case.

# More rotations
First, let's go over a rotation that wasn't discussed in that introductory article. Often one wants to rotate a frame of reference so that a certain point is in a certain position. Let's say we want the unit vector $$\mathbf{\hat{c}}$$ to align with the z axis. We also need to pick an orientation: typically in map projections one wants north to be up, although if $$\mathbf{\hat{c}}$$ is at the north or south pole another choice must be made.

Let $$\mathbf{\hat{a}}$$ be the center of the great circle passing through $$\mathbf{\hat{c}}$$ and the north pole, such that $$\mathbf{\hat{a}} = \frac{\mathbf{\hat{n}} \times \mathbf{\hat{c}}}{\|\mathbf{\hat{n}} \times \mathbf{\hat{c}}\|}$$. Also let $$\mathbf{\hat{b}} = \mathbf{\hat{c}} \times \mathbf{\hat{a}}$$. The matrix $$\mathbf{R}$$ with rows $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and $$\mathbf{\hat{c}}$$ transforms the vector $$\mathbf{\hat{a}}$$ to $$[1,0,0],$$ $$\mathbf{\hat{b}}$$ to $$[0,1,0],$$ and $$\mathbf{\hat{c}}$$ to $$[0,0,1]$$. This is a rotation matrix by construction, so its inverse is equal to its transpose.

We can give $$\mathbf{z}$$ and $$\mathbf{p}$$ a more formal definition now:

$$\begin{split}
\mathbf{z} &= \mathbf{Q} \mathbf{R} \mathbf{p}, \\
\mathbf{p} &= \mathbf{R}^\top [z_1, z_2, 1].
\end{split}$$

# Orthographic
The orthographic projection is what an object in space looks like viewed a perspective infinitely far from the object. This can be described as rotating the object as described above, leaving out the component in the direction of $$\mathbf{\hat{c}}$$. Let $$\mathbf{R}$$ be that rotation matrix, and then

$$
\mathbf{z} = \mathbf{Q} \mathbf{R} \mathbf{\hat{v}},
$$

This representation of the projection does not depend on the shape of the surface being projected. Anything with a representation as a vector in space can be handled this way: a sphere, an ellipsoid, an irregular lumpy asteroid.

Normally one wishes to only project the near side of the object. With that restriction, the projection transforms a hemisphere centered on $$\mathbf{\hat{c}}$$ to a disk in the plane. Without that restriction, the other hemisphere is transformed to the same disk, in reversed orientation.

The inverse projection is fairly straightforward, except that the component discarded by $$\mathbf{Q}$$ needs to be determined. For a unit sphere, $$z_3 = \sqrt{1-z_1^2-z_2^2}$$ for points the near side. (Note that this is different from what was given in the previous section: the reason will be explained in the derivation.) Then:

$$
\mathbf{\hat{v}} = \mathbf{R}^\top [z_1, z_2, z_3].
$$

# Stereographic
For the stereographic projection, the point of perspective is antipodal to $$\mathbf{\hat{c}}$$. The stereographic projection maps the entire sphere to the plane, minus one point, $$\mathbf{\hat{c}}$$, which is mapped to infinity. This projection is conformal: it preserves angles locally.

The forward projection formula is as follows. The factor of 2 makes the scale true at the center of the projection.

$$
\mathbf{z} = \mathbf{Q} \mathbf{R} \mathbf{\hat{v}} \frac{2}{\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} + 1}
$$

The inverse projection is as so:

$$
\mathbf{\hat{v}} = \frac{4\mathbf{p} + \mathbf{\hat{c}}\left(1 - \|\mathbf{p}\|^2\right)}{\|\mathbf{p}\|^2 + 3}
$$

The stereographic projection can be extended to the ellipsoid by using the [conformal latitude](https://en.wikipedia.org/wiki/Latitude#Conformal_latitude).

# Gnomonic
The gnomonic projection can be thought of as placing a light at the center of a transparent sphere and shining it through onto a plane. It has the special quality that all geodesics on the sphere are transformed in to straight lines in the plane. This is easy to show: great circles are the intersection of a plane with the sphere, while the intersection of a plane with another plane is a line. (This doesn't hold with an ellipsoid, but can be a useful approximation.)

The forward projection is:

$$
\mathbf{z} = \mathbf{Q} \mathbf{R} \frac{\mathbf{\hat{v}}}{\mathbf{\hat{c}} \cdot \mathbf{\hat{v}}}
$$

Like the orthographic projection, one generally only wants the near side of the object. If included, the far side of the object appears reversed.

For the inverse, the formula for $$\mathbf{\hat{v}}$$ is simple:

$$
\mathbf{\hat{v}} = \frac{\mathbf{p}}{\|\mathbf{p}\|}
= \frac{\mathbf{R}^\top [z_1, z_2, 1]}{\|z_1, z_2, 1\|}
$$

## Gnomonic with barycentric coordinates
A bit of detour. [Barycentric coordinates](https://en.wikipedia.org/wiki/Barycentric_coordinate_system) can be applied for further useful results. Let $$\mathbf{\hat{v}}_1$$, $$\mathbf{\hat{v}}_2$$, and $$\mathbf{\hat{v}}_3$$ be points on the sphere not sharing a great circle, and let $$\mathbf{\hat{c}}$$ be the spherical circumcenter of the triangle formed by those points. (See [this post on spherical triangle centers]({% post_url 2021-05-02-spherical-triangle-centers%}) if needed.) Also let $$\mathbf V$$ be the matrix whose i-th columns is $$\mathbf{\hat{v}}_i$$. Let $$\mathbf \beta$$ be the vector of barycentric coordinates, let $$\beta_i$$ be its $$i$$th component, and let $$\mathbf \alpha$$ and $$\alpha_i$$ be defined similarly. Finally, the barycentric coordinates of the point projected in gnomonic projection centered at $$\mathbf{\hat{c}}$$ can be found in a two-step process:

$$
\begin{split}
\mathbf{\alpha} &= \mathbf{V}^{-1} \mathbf{\hat{v}}, \\
\beta_i &= \frac{\alpha_i}{\sum_i \alpha_i}.
\end{split}
$$

Note that the determinant of $$\mathbf V$$ can be left out of the matrix inverse: it'll be cancelled out in the second formula.

The inverse projection is particularly simple:

$$
\mathbf{\hat{v}} = \frac{\mathbf{V} \mathbf{\beta}}{\|\ldots\|},
$$

and is useful for creating triangular grids on the sphere. In the geodesic dome community, gnomonic projection of a triangular grid is called "method 1".

# General perspective projection

All three of these projections are specific cases of the general perspective projection. One could project from a point at a similar distance to the globe as a satellite camera is to the Earth. (Or the moon, but the moon is far enough away that you might as well use orthographic projection.)

Restating the assumptions so they're in one place: let $$\mathbf{\hat{v}}$$ be a point on the object being projected $$\mathbf{\hat{c}}$$ specifies a plane tangent to the unit sphere: where $$\mathbf{p}$$ is a point on the plane, $$\mathbf{\hat{c}} \cdot \mathbf{p} = 1$$. $$\mathbf{R}$$ is a rotation matrix as defined earlier, and $$\mathbf{Q}$$ is the 2x3 matrix defined earlier, so that $$\mathbf{z} = \mathbf{Q} \mathbf{R} \mathbf{p}$$ and $$\mathbf{p} = \mathbf{R}^\top [z_1, z_2, 1]$$. Finally, let $$k\mathbf{\hat{c}}$$ be the point of projection, where $$k$$ be a real number indicating how far the point of projection is from the origin. For stereographic, $$k=-1$$; for gnomonic, $$k=0$$; for orthographic, take the limit as $$k$$ approaches positive infinity. (At $$k=1$$, the projection is not well-defined.)

For the forward projection, start as so. $$\mathbf{p}$$ must lie on the line between $$\mathbf{\hat{v}}$$ and $$k\mathbf{\hat{c}}$$, so start with linear interpolation:

$$
\mathbf{p} = \mathbf{\hat{v}} t + \mathbf{\hat{c}} (1-t).
$$

The $$t$$ such that $$\mathbf{p}$$ satisfies $$\mathbf{\hat{c}} \cdot \mathbf{p} = 1$$ is

$$
t = \frac{1 - k}{\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} - k}.
$$

By definition, $$\mathbf{Q} \mathbf{R} \mathbf{\hat{c}} = [0, 0]$$, so the term involving $$\mathbf{\hat{c}}$$ can be dropped. Finally, the projected point is

$$
\mathbf{z} = \mathbf{Q} \mathbf{R} \mathbf{\hat{v}} \frac{1-k}
{\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} + k}.
$$

This can be easily verified to be equal to the specific projections above for their given value of $$k$$. (Hint for orthographic: divide the numerator and denominator by $$k$$.)

The inverse projection starts similarly: find $$\mathbf{\hat{v}}$$ on the line between $$\mathbf{p}$$ and $$k\mathbf{\hat{c}}$$. Applying linear interpolation, for some real number $$t$$,

$$
\mathbf{\hat{v}} = \mathbf{p} s + k\mathbf{\hat{c}} (1-s).
$$

An assumption about the shape of the object being projected is needed: assume the unit sphere, such that $$\|\mathbf{\hat{v}}\| = 1$$. Applying that and solving gives a quadratic in $$s$$, with solution:

$$
s = \frac{k(k-1)\pm \sqrt{(k-1)(2k-\|p\|^2(k+1))}}{\|p\|^2 + k(k-2)}
$$

Use the condition $$\mathbf{\hat{c}} \cdot \mathbf{\hat{v}} > 0$$ to choose the sign in the formula. The specific inverse projections for stereographic and gnomonic can be derived from this. When $$k=-1$$, $$s=\frac{4}{\|\mathbf{p}\|^2 + 3}$$ (or $$s=0$$, which is a spurious solution). When $$k=0$$, $$s=\pm\frac{1}{\|\mathbf{p}\|}$$: the positive solution is the one on the near side of the sphere. For the orthographic projection, however, this quadratic equation does not result in a useful answer in the limit: use the solution given earlier instead.
