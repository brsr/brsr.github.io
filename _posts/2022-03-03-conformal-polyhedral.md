---
layout: post
title: "Some dubious ways to calculate conformal polyhedral projections"
tags: cartography cursed python
---
(The title of this post is in tribute to Moler and Van Loan's classic paper "[Nineteen Dubious Ways to Compute the Exponential of a Matrix](https://doi.org/10.1137/S00361445024180).")

Conformal map projections are deeply connected to the theory of complex analysis of a single variable, which is at this point in time is very mature. One case of interest is transforming the faces of a spherical polyhedron into Euclidean polygons in the plane, and Adams basically had a mature description of that by 1925.[^Adams] The best description of these projections is contained in Lee's 1976 monograph, which updates and fixes some errors in Adams.[^Lee] While Lee's work is a little dated, it remains the most complete description. Except for simple cases like the mapping of a hemisphere to a square,[^Stark] few have improved on Lee's work. However, these projections aren't used very often. I believe part of this is that, despite the well-developed theory of conformal polyhedral projections, they're kind of a pain in the neck to actually calculate or implement.

This does not intend to be a complete description: Lee's monograph is freely available on archive.org, and also a hundred pages long.[^Lee] Instead, this is a high-level summary. The transformation of a face of a spherical polyhedron can be described as a composition of three functions. Let's not be too fussy about the domains and codomains of the functions, but we do need to include the point at infinity of the extended complex plane (the [Riemann sphere](https://en.wikipedia.org/wiki/Riemann_sphere)). Let $$z$$ be a complex number and $$\lambda, \phi$$ be latitude and longitude.

* $$A(\lambda, \phi)$$: Use the stereographic projection to transform a geodesic polygon on the sphere to a polygon in the complex plane having circular arcs for edges.
* $$B(z)$$: Conformally transform that arc-edged polygon to some convenient region in the complex plane, usually the upper half-plane or unit disk.
* $$C(z)$$: Conformally transform that region to the interior of a Euclidean polygon.

Thus, the forward projection is $$C(B(A(\lambda, \phi)))$$, and the inverse is $$A^{-1}(B^{-1}(C^{-1}(z)))$$. For example, to transform a face of a spherical octahedron to an equilateral triangle, the results at each step look like this:
<table><tr>
<th><img src="/assets/images/conformal/conformaltriangle1.svg" alt="Triangle on the sphere in orthographic projection"/></th>
<th><center>A<br/>↔</center></th>
<th><img src="/assets/images/conformal/conformaltriangle2.svg" alt="Arc-edged triangle in the plane"/></th>
<th><center>B<br/>↔</center></th>
<th><img src="/assets/images/conformal/conformaltriangle3.svg" alt="Upper half plane"/></th>
<th><center>C<br/>↔</center></th>
<th><img src="/assets/images/conformal/conformaltriangle4.svg" alt="Euclidean triangle in the plane"/></th>
</tr></table>

The stereographic projection $$A$$ is well documented and does not need to be elaborated on here. Large-scape projections like these are usually used in spherical form, but if you really want you can use the ellipsoidal form of the stereographic projection: see pages 160 -- 163 of Snyder.[^Snyder]

# Preliminaries

The [Schwarz triangle function](https://en.wikipedia.org/wiki/Schwarz_triangle_function) transforms the upper half-plane to a triangle: spherical, Euclidean, or hyperbolic depending on the angles given. For a triangle with interior angles $$πα$$, $$πβ$$, and $$πγ$$, the formula for the Schwarz triangle function $$s(z)$$ is:[^Nehari]

$$
s(z) = z^{\alpha} \frac{_2 F_1 \left(a', b'; c'; z\right)}{_2 F_1 \left(a, b; c; z\right)}
$$

where $$_2 F_1 \left(a, b; c; z\right)$$ is a [hypergeometric function](https://en.wikipedia.org/wiki/Hypergeometric_function) and
<table><tr>
<td>$$\begin{split}
&a &= (1−α−β−γ)/2,\\
&a' &= (1+α−β−γ)/2,
\end{split}$$</td>
<td>$$\begin{split}
&b &= (1−α+β−γ)/2,\\
&b' &= (1+α+β−γ)/2,
\end{split}$$</td>
<td>$$\begin{split}
&c &= 1 − α, \\
&c' &= 1 + α.
\end{split}$$</td>
</tr></table>

The function $$s(z)$$ maps $$z = 0$$ to the vertex having angle $$πα$$, $$z = 1$$ to the vertex having angle $$πβ$$, and $$z = \infty$$ to the vertex having angle $$πγ$$. The transformed vertices can be expressed using [gamma functions](https://en.wikipedia.org/wiki/Gamma_function):

$$\begin{aligned}
s(0) &= 0, \\
s(1) &= \frac
  {\Gamma(1-a')\Gamma(1-b')\Gamma(c')}
  {\Gamma(1-a)\Gamma(1-b)\Gamma(c)}, \\
s(\infty) &= \exp\left(i \pi \alpha \right)\frac
  {\Gamma(1-a')\Gamma(b)\Gamma(c')}
  {\Gamma(1-a)\Gamma(b')\Gamma(c)}.
\end{aligned}$$

The boundary of the triangle is the image of the real line.

Also useful are [Möbius transformations](https://en.wikipedia.org/wiki/M%C3%B6bius_transformation), which conformally map the Riemann sphere to itself. Möbius transformations map circles to circles, if you consider a line to be a circle passing through the point at infinity. These can be used to map the upper half-plane to the unit disk, choosing which points on the real line are mapped to which points on the unit circle as you require.

# Step B: Nice in very common but very restricted scenarios

Any polygon (spherical or Euclidean) can be divided into triangles, so it's enough to transform each triangle and then stitch them together later. So all we have to do is invert that Schwarz triangle function. Easy, right? Well, not for arbitrary triangles, but for the usual symmetric "polyhedral globe" setups it is.

More specifically, for a spherical [Schwarz triangle](https://en.wikipedia.org/wiki/Schwarz_triangle), then the inverse of $$s(z)$$ can be written as a rational function. A [Schwarz triangle](https://en.wikipedia.org/wiki/Schwarz_triangle) is (loosely) a triangle that can tile a sphere (or the plane, or hyperbolic space) by reflections. If the original polygon isn't a Schwarz triangle it may be subdividable into Schwarz triangles: other spherical polyhedra, squares, pentagons, etc. can also be subdivided this way. So, let's take $$B(z) = s^{-1}(\frac{z}{k})$$, where $$k > 0$$ is some real number to scale the input to the Schwarz function appropriately. Lee gave formulas for $$B(z)$$ for the subdivided faces of platonic solids and other shapes.[^Lee]

Let's see a simple example. The faces of the octahedron are already Schwarz triangles, so do not need to be subdivided, and its rational function is particularly simple. Specifically, orient the octahedron is oriented so two vertices lie at the "poles" and the rest lie along the equator, and apply the stereographic projection centered on a pole so that that pole is mapped to 0, the other is mapped to $$\infty$$, and the equator vertices are mapped to $$1, i, -1, -i$$. Then the function is

$$
B(z) = \frac{4z^2}{(1+z^2)^2}
$$

The North and South Poles, at 0 and $$\infty$$ on the Riemann sphere, are mapped to 0. The equator vertices at $$1$$ and $$-1$$ go to 1, and those at $$i$$ and $$-i$$ go to $$\infty$$. This function maps faces of the octahedron alternately to the upper and lower half plane.

In this particular case, the inverse of $$B(z)$$ has a nice analytic form:

$$
B^{-1}(z) = \pm \sqrt{\pm \frac{2\sqrt{1-z}}{z} + \frac{2}{z}-1 }
$$

where the two $$\pm$$ signs are independent of each other. In general, however, $$B^{-1}(z)$$ can't be expressed simply: the Schwarz triangle formula using hypergeometric functions is often as good as you can get.

# Step C: Time for elliptic functions
In step C, since the target triangle is a Euclidean triangle, $$\alpha + \beta + \gamma = 1$$. The hypergeometric function in the denominator reduces to 1, and the whole expression becomes:

$$
C(z) = s(z) = z^{\alpha} \,_2 F_1 \left(\alpha, \alpha+\beta; 1 + \alpha; z\right)
$$

$$C^{-1}(z)$$ can be expressed in terms of elliptic functions. In general the construction of the inverse is somewhat involved, [as shown in this math.stackexchange page](https://math.stackexchange.com/questions/246309/conformal-mapping-from-triangle-to-upper-half-plane-in-terms-of-weierstrass-wp). However, there is one well-known special case, and one less-well-known special case, that are easier to handle.

## Squares
The [Jacobi elliptic functions](https://en.wikipedia.org/wiki/Jacobi_elliptic_functions) can be used to map between a square and the unit disk. The [incomplete elliptic integral of the first kind](https://en.wikipedia.org/wiki/Elliptic_integral#Incomplete_elliptic_integral_of_the_first_kind) is its inverse, so in this case we can replace the formula for $$C(z)$$ with a different, somewhat simpler special function. Let $$\operatorname{cn}(z, k)$$ be the elliptic cosine Jacobi function in terms of modulus, and let $$F(\varphi, k)$$ be the incomplete elliptic integral of the first kind in terms of the amplitude and modulus.

Transforming a hemisphere to a square is the simplest of these transformations, and a useful example. $$A$$ remains the stereographic projection, mapping a hemisphere to the unit disk, but $$B$$ is the identity function, keeping the center of the hemisphere at the origin, and then $$C$$ is as so:[^Fong]

$$\begin{split}
C(z) &= \frac{\sqrt{1-i}}{-K} F\left(\arccos \left( \frac{1+i}{\sqrt{2}} z \right),  \frac{1}{\sqrt{2}}\right) + 1 - i\\
C^{-1}(z) &= \frac{\sqrt{1-i}}{\sqrt{2}}  \operatorname{cn} \left( K \frac{1+i}{2} z - K, \frac{1}{\sqrt{2}}\right)
\end{split}$$

where $$K = F\left(\frac{\pi}{2}, \frac{1}{\sqrt{2}}\right)$$.

## Equilateral triangles
The [Dixon elliptic function](https://en.wikipedia.org/wiki/Dixon_elliptic_functions) can be used to map an equilateral triangle to the unit disk. Chapter 4 of Lee covers this transformation, giving formulas for the projection of the whole earth, a hemisphere, triangle-faced platonic solids, and so on into one or more equilateral triangles. All involve the inversion of the functions $$\operatorname{sm}$$ and/or $$\operatorname{cm}$$ for the forward transformation. The Dixon elliptic functions do not have a simple inverse that I know of, and even Lee resorts to the form using hypergeometric functions.

The Dixon elliptic functions are obscure, and nearly only come up in discussions about conformal map projections. However, Dixon elliptic functions can be expressed in terms of Jacobi elliptic functions, as mentioned on the Wikipedia article.

# So what's the catch?

How do we calculate all these special functions?

Calculating the hypergeometric function over the entire upper half-plane is not straightforward. See the (lengthy) [NIST Digital Library of Mathematical Functions entry](https://dlmf.nist.gov/15). A numerically well-behaved implementation must make use of a number of different transformations and formulas. Full-fledged software implementations are uncommon. SciPy does implement hyperbolic function of complex arguments, [but only recently did they fix a long-standing bug and make it correct in the entire complex plane](https://github.com/scipy/scipy/pull/14706), which demonstrates how difficult it is to implement well. Even if you do have a usable implementation of the hypergeometric function available, it's expensive to calculate.

The elliptic functions and integrals are simpler but still complicated. The most commonly used implementations are those in [Cephes](https://netlib.org/cephes/), which are only implemented for real arguments: formulas to separate the real and imaginary parts are necessary. There are more considerations after that. Stark's 2009 paper[^Stark] on the disk-to-square mapping dedicates sections 3 and 4, a decent chunk of the paper by length, to a stable and fast evaluation of $$F$$. He splits the unit disk into three regions, each one using a different numerical technique.

Lee's monograph is the length that it is because of all the numerical methods included. And honestly, there has not been a radical improvement in those methods since he was writing in 1976. Some calculations are still long and complicated.

# Conclusion

On one hand, the theory here has been settled for some time, and the steps are documented. But on the other, those steps are more complicated to calculate than they look on the surface. For Adams to have calculated all this in 1925 must have been very time-consuming. You're also up a creek if you want to use an asymmetric polyhedron. Given that polyhedral projections are more often seen as novelties instead of practically useful, it's understandable that most GIS software doesn't bother.

*A Python script to create the figures in this post is located [here](https://github.com/brsr/mapproj/blob/master/bin/conformal_polyhedral.py).*

# References
[^Adams]: Adams, Oscar S. (1925). Elliptic Functions Applied to Conformal World Maps. Issue 297 of United States Coast and Geodetic Survey Serial. U.S. Government Printing Office.
[^Fong]: Fong, Chamberlain (2015). [Schwarz-Christoffel mapping](https://squircular.blogspot.com/2015/09/schwarz-christoffel-mapping.html).
[^Lee]: Lee, Laurence (1976). [Conformal Projections based on Elliptic Functions.](https://archive.org/details/conformalproject0000leel) Cartographica Monographs. 16. University of Toronto Press. Also in [The Canadian Cartographer. 13 (1). 1976](https://www.utpjournals.press/toc/cart/13/1).
[^Nehari]: Nehari, Ze'ev (1975). Mapping Properties of Special Functions: The Schwarzian $$s$$-functions. Chapter 6, section 5 of Conformal Mapping. Dover Pubs, NY pp 308--316.
[^Snyder]: Snyder, John P. (1987). [Map Projections---A Working Manual](https://archive.org/details/Snyder1987MapProjectionsAWorkingManual). [United States Geological Survey Professional Paper 1395](https://pubs.er.usgs.gov/publication/pp1395).
[^Stark]: Stark, Michael M. (2009) [Fast and Stable Conformal Mapping Between a Disc and a Square](https://doi.org/10.1080/2151237X.2009.10129277). Journal of Graphics, GPU, and Game Tools, 14:2, 1--23.
