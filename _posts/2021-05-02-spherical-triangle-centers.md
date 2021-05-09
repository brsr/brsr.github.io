---
layout: post
title: "Spherical triangle centers with vectors"
tags: geometry gis
---
There are many triangle centers defined for Euclidean triangles. These triangle centers can also be defined in spherical geometry, but some Euclidean centers correspond to two or more spherical centers. See [this paper](https://arxiv.org/abs/1608.08190) for details. That said, this post lists some spherical triangle centers with nice constructions in terms of vectors. Here's an image of them together. Unlike in Euclidean geometry, none of these fall on a geodesic together, although there may be an equivalent to the Euler line using centers not included here.

<center><img src="/assets/images/spherical/centers.svg" alt="Some centers of a spherical triangle" height="300" /></center>

Let the triangle have vertices $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and $$\mathbf{\hat{c}}$$, oriented counterclockwise. See [this earlier post]({% post_url 2021-05-01-vector-spherical-geometry %}) for more details on spherical geometry using vectors.

# Centroid (Vertex-Median point)
The simplest spherical triangle center is to take the Euclidean centroid of the points and then normalize onto the sphere. This is the point of intersection of medians.

$$
\mathbf{\hat{z}} = \frac{\mathbf{\hat{a}} + \mathbf{\hat{b}} + \mathbf{\hat{c}}}
{\sqrt{3 + 2~\mathbf{\hat{a}} \cdot \mathbf{\hat{b}} + 2~\mathbf{\hat{b}} \cdot \mathbf{\hat{c}} + 2~\mathbf{\hat{c}} \cdot \mathbf{\hat{a}}}}
$$

The linked paper above calls this the vertex-median point, because not all the qualities of the Euclidean centroid carry over. For example, in spherical geometry, unlike Euclidean, the centroid is not (in general) the point that divides the triangle into three equal-area triangles.
<center><img src="/assets/images/spherical/vertex-median.svg" alt="The vertex-median point of a spherical triangle" height="300" /></center>

# Centroid (Area)
The point that divides the triangle into three equal-area triangles, as pictured, is more complicated to construct. Use the formula in [this Stack Exchange answer](https://math.stackexchange.com/questions/1151428/point-within-a-spherical-triangle-given-areas) with $$t_1=t_2=t_3=1/3$$.

<center><img src="/assets/images/spherical/area_centroid.svg" alt="The area centroid of a spherical triangle" height="300" /></center>

# Incenter
The inradius is the intersection of the angle bisectors, and is given by:

$$
\mathbf{\hat{z}} = \frac{\| \mathbf{\hat{b}} \times \mathbf{\hat{c}}\| \mathbf{\hat{a}}
+ \| \mathbf{\hat{c}} \times \mathbf{\hat{a}}\| \mathbf{\hat{b}}
+ \| \mathbf{\hat{a}} \times \mathbf{\hat{b}}\| \mathbf{\hat{c}}}
{\|\ldots\|}
$$

The inradius $$r$$ is given by:

$$
\sin r = \frac{|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}}|}
{\begin{Vmatrix} \|\mathbf{\hat{b}} \times \mathbf{\hat{c}}\| \mathbf{\hat{a}}
+ \| \mathbf{\hat{c}} \times \mathbf{\hat{a}}\| \mathbf{\hat{b}}
+ \| \mathbf{\hat{a}} \times \mathbf{\hat{b}}\| \mathbf{\hat{c}}\end{Vmatrix}}
$$

<center><img src="/assets/images/spherical/incenter.svg" alt="The incenter of a spherical triangle" height="300" /></center>

# Circumcenter
The circumcenter of a Euclidean triangle is the point with equal distance to each vertex.

$$
\mathbf{\hat{z}} = \frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}+ \mathbf{\hat{b}} \times \mathbf{\hat{c}} + \mathbf{\hat{c}} \times \mathbf{\hat{a}}}
{\|\ldots\|}
$$

The numerator is similar to the formula for the area of a Euclidean triangle as mentioned above, but more importantly it is the formula for the normal to a plane that passes through the three points.

The circumradius $$r$$ is just the distance from $$\mathbf{\hat{z}}$$ to any vertex, and is given by

$$
\cos r = \frac{\left|\mathbf{\hat{a}}, \mathbf{\hat{b}}, \mathbf{\hat{c}} \right|}
{\|\mathbf{\hat{a}} \times \mathbf{\hat{b}} + \mathbf{\hat{b}} \times \mathbf{\hat{c}} + \mathbf{\hat{c}} \times \mathbf{\hat{a}}\|}.
$$

<center><img src="/assets/images/spherical/circumcenter.svg" alt="The circumcenter of a spherical triangle" height="300" /></center>

# Orthocenter
The orthocenter is the intersection of altitudes, extending the edges outside the triangle if needed. This seems the most likely of these centers to lie outside the triangle.

$$
\mathbf{\hat{z}} = \frac{(\mathbf{\hat{c}} \cdot \mathbf{\hat{a}})(\mathbf{\hat{b}} \cdot \mathbf{\hat{c}}) \mathbf{\hat{a}} \times \mathbf{\hat{b}}
+ (\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}) (\mathbf{\hat{c}} \cdot \mathbf{\hat{a}}) \mathbf{\hat{b}} \times \mathbf{\hat{c}}
+ (\mathbf{\hat{a}} \cdot \mathbf{\hat{b}}) (\mathbf{\hat{b}} \cdot \mathbf{\hat{c}}) \mathbf{\hat{c}} \times \mathbf{\hat{a}}}{\|\ldots\|}
$$

Note that if any two edges have length $$\pi/2$$, both these expressions are undefined. Geometrically, the altitude from the point where those two edges meet to the opposite edge is underspecified: every point on the opposite edge defines an altitude. Here are some sensible special definitions for that case:

* If the opposite edge has length in $$(0, \pi/2)$$, the orthocenter is the midpoint of that edge.
* If the opposite edge has length in $$(\pi/2, \pi)$$, the orthocenter is the antipode of the midpoint of that edge.
* If all three edges have length $$\pi/2$$, then the orthocenter is the same point as the centroid (and the other triangle centers listed).

<center><img src="/assets/images/spherical/orthocenter.svg" alt="The orthocenter of a spherical triangle" height="300" /></center>

*A Python script to create the figures in this post is located [here](https://github.com/brsr/mapproj/blob/master/bin/vector-spherical-geometry.py).*
