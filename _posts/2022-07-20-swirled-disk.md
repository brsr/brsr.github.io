---
layout: post
title: "Area-preserving swirling of the disk"
tags: cartography

images_linear:
  - name: k = -π/4
    link: /assets/images/wiggles/linear_4.svg
  - name: k = -π/2
    link: /assets/images/wiggles/linear_2.svg
  - name: k = -π
    link: /assets/images/wiggles/linear_1.svg

images_wiechel:
  - name: k = -½
    link: /assets/images/wiggles/wiechel_2.svg
  - name: k = -1
    link: /assets/images/wiggles/wiechel_1.svg
  - name: k = -2
    link: /assets/images/wiggles/wiechel_0.svg

images_wiggles:
  - name: a = π/8, b=π
    link: /assets/images/wiggles/wiggles_8.svg
  - name: a = π/16, b=3π
    link: /assets/images/wiggles/wiggles_16.svg
  - name: a = π/32, b=8π
    link: /assets/images/wiggles/wiggles_32.svg

imagewidth: 32%
---
<img src="/assets/images/wiggles/identity.svg" alt="Lambert azimuthal equal-area projection of the Earth" height="300" style="float:right" />
A consequence of the [Riemann mapping theorem](https://en.wikipedia.org/wiki/Riemann_mapping_theorem) is that the conformal map from a given region of the sphere to a given region of the plane is unique. Area-preserving maps, however, are not. For example, the
The [Lambert azimuthal equal-area projection](https://en.wikipedia.org/wiki/Lambert_azimuthal_equal-area_projection) (left) is an azimuthal equal-area map projection, which takes the sphere to a disk. The [Wiechel projection](https://en.wikipedia.org/wiki/Wiechel_projection) (depicted later) is also an equal-area map projection, which resembles a swirled version of the Lambert projection. [As demonstrated on Daan Strebe's forums](https://www.mapthematics.com/forums/viewtopic.php?f=8&t=716&sid=d27a60a94f4adecffdc1485668e8e486#p1714), the Wiechel projection has worse angular distortion than the Lambert projection, so its only real use is to demonstrate that equal-area map projectons aren't unique.

The Wiechel projection isn't unusual. There is an infinite family of such equal-area map projectons to the disk. Those may be easier to understand as a transformation of the disk applied after the Lambert projection. Here we'll look at some such transformations that are easy to describe.

# Transformation of the disk
Consider a transformation of the disk in [polar coordinates](https://en.wikipedia.org/wiki/Polar_coordinate_system) given by

$$\begin{split}
r_o &= r_i \\
\theta_o &= \theta_i + f\left(r_i\right)
\end{split}$$

where $$f$$ is an arbitrary differentiable function. If $$f(0) = 0$$ this transformation preserves the orientation of the origin: if $$f(1) = 0$$ it preserves the orientation of the boundary.

All such transformations are area-preserving. This can be proven using [Jacobians](https://en.wikipedia.org/wiki/Jacobian_matrix_and_determinant). This transformation is given polar coordinates, so we need to include the Jacobians of the transformation into and from polar coordinates. This is given as an example on the linked Wikipedia page: the Jacobian determinant of the transformation from polar coordinates to Cartesian coordinates is $$r$$. The Jacobian determinant of the inverse transformation is just $$\frac{1}{r}$$. Since the transformation described earlier has $$r_o = r_i$$, the two cancel out, and we just need to examine the Jacobian determinant of the transformation itself:

$$
\begin{vmatrix}
 1 & 0 \\
 \frac{df}{dr_i} & 1
\end{vmatrix}
$$

which is clearly equal to 1 regardless of $$f$$.

Note that the inverse transformation is given by $$-f$$, and the identity is given by $$f = 0$$. It's also easy to show that these transformations are associative, so this is a group in the mathematical sense. The functions such that $$f(0) = 0$$ or $$f(1) = 0$$ form subgroups.

# Linear swirl
For these, $$f(r) = k r$$ for some constant k. This is what's usually implemented as "twirl" or "swirl" in graphics software like Photoshop. Somewhat notoriously, [an international criminal attempted to disguise an image of himself he posted online by swirling his face, but Interpol recovered the original image by applying a swirl in the opposite direction.](https://www.cbc.ca/news/canada/british-columbia/christopher-paul-neil-convicted-sex-offender-faces-10-new-charges-in-b-c-1.2590785) (Content warning for mentions of sexual abuse.)

{% include image-gallery.html listing = page.images_linear width = page.imagewidth %}

# Wiechel swirl
For these, $$f(r) = k \arcsin r$$ for some constant k. $$k = -\frac{1}{2}$$ gives the transformation from the Lambert azimuthal equal-area projection to the Wiechel projection, and $$k = \frac{1}{2}$$ gives the transformation from the Wiechel projection to the Lambert azimuthal equal-area projection.

{% include image-gallery.html listing = page.images_wiechel width = page.imagewidth %}

# Wiggles
We're not limited to monotonic functions for $$f$$. These ones use functions of the form $$f(r) = a \sin\left(br\right)$$. [Look at all of those wiggles!](https://youtu.be/V_OVxxIvqVw?t=45)

{% include image-gallery.html listing = page.images_wiggles width = page.imagewidth %}

# Angle distortion
None of these are improvements on the Lambert projection, at least in terms of angle distortion. Without getting into a lengthy calculation, notice that other transformations alter the angle at which meridians meet circles of latitude, particularly at the equator where, on the sphere, meridians intersect at right angles. However, it is interesting that we can make the angle distortion as bad as we like, for example by choosing an arbitrary large $$k$$.
