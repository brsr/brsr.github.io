---
layout: post
title: "Snyder's equal-area projection"
tags: vectors cartography
---

John P Snyder, author of many useful articles and books on map projections, created an equal-area projection called (in an aversion of [Stigler's Law](https://en.wikipedia.org/wiki/Stigler%27s_law_of_eponymy)) Snyder's equal-area projection.[^Snyder] Snyder's projection was designed for polyhedral globes and maps. The best-known example of a polyhedral map might be Buckminster Fuller's [Dymaxion map](https://en.wikipedia.org/wiki/Dymaxion_map), based on an [icosahedron](https://en.wikipedia.org/wiki/Icosahedron).

Snyder looked at the platonic solids and the truncated icosahedron. Each face of the polyhedron was subdivided into isosceles triangles using the center point and the vertices of the face. Even triangular faces were subdivided this way, to maintain symmetry. These triangles on the sphere were transformed into triangles in the plane and then reassembled. In a broad sense, this projection includes the method of subdividing the sphere as well as the function between the spherical triangle and the planar triangle. In a narrow sense, the projection is just the function between a triangle on the sphere and a triangle on the plane. That is what this post focuses on: that projection, considered on its own.

# Vector form
Snyder's projection is restated here in vector form. Other have noticed that projections related to Snyder's projection are simpler to express in terms of unit vectors rather than latitude and longitude,[^Patt] and this turns out to be true for Snyder's projection as well. As usual, reference [the introductory article on spherical geometry with vectors]({% post_url 2021-05-01-vector-spherical-geometry %}) if needed.

Time for a whole bunch of definitions. Let $$\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2$$ be points on the sphere, in counterclockwise order. and let $$\mathbf V = [\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2]$$ be a matrix having those vectors as columns. Let $$\mathbf{\hat{v}} = [v_x, v_y, v_z]$$ be another point on the sphere. Let $$\beta_i$$ be barycentric coordinates such that $$\beta_0 + \beta_1 + \beta_2 = 1$$. Let $$A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2)$$ denote the signed area of the triangle made up of those vertices, positive for counterclockwise order.

## Forward
Leaving out some of the nitty-gritty of the derivation, first find the point $$\mathbf{\hat{p}}$$ where the great circle passing through $$\mathbf{\hat{v}}_0$$ and $$\mathbf{\hat{v}}$$ and the great circle passing through $$\mathbf{\hat{v}}_1$$ and $$\mathbf{\hat{v}}_2$$.

$$
\mathbf{\hat{p}} = \frac{(\mathbf{\hat{v}}_0 \times \mathbf{\hat{v}}) \times (\mathbf{\hat{v}}_1 \times \mathbf{\hat{v}}_2)}{\|\ldots\|}
= \frac{
|\mathbf{V}| \mathbf{\hat{v}} -
|\mathbf{\hat{v}}, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2| \mathbf{\hat{v}}_0
}{\|\ldots\|}
$$

Then find the distance from $$\mathbf{\hat{v}}_0$$ to $$\mathbf{\hat{p}}$$, and relate it to the distance from $$\mathbf{\hat{v}}_0$$ to $$\mathbf{\hat{v}}$$ in a way that preserves area.

$$
h = \sqrt{\frac{1 - \mathbf{\hat{v}}_0 \cdot \mathbf{\hat{v}}}
               {1 - \mathbf{\hat{v}}_0 \cdot \mathbf{\hat{p}}}}
$$

Then one of the coordinates can be found in terms of $$b$$ and the fraction of the area contained to one side of the line from $$\mathbf{\hat{v}}_0$$ to $$\mathbf{\hat{p}}$$:

$$
\beta_2 = h \frac{A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}})}
                 {A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2)}
$$

Finally, $$\beta_0 = 1 - h$$, and $$\beta_1 = h - \beta_2$$.

## Inverse
The inverse formula is lengthy, but follows straightforwardly from the formward formula. Contrary to Snyder's original paper[^Snyder] and a paper that followed[^Harrison], it does have a closed form solution.

Let $$c_{ij} = \mathbf{\hat{v}}_i \cdot \mathbf{\hat{v}}_j$$, and $$s_{ij} = \sqrt{1-c_{ij}^2} = \| \mathbf{\hat{v}}_i \times \mathbf{\hat{v}}_j \|$$. $$\arctan(x,y)$$ is [the two-parameter form of arctan](https://en.wikipedia.org/wiki/Atan2).

$$\begin{split}
h &= 1 - \beta_0 \\
a &= \frac{\beta_2}{h} A\left(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2\right)\\
S &= \sin(a) \\
C &= 1 - \cos(a) \\
F &= \left(S |\mathbf{V}| + C \left(c_{01} c_{12} - c_{20}\right)\right)^2 - \left(s_{12} C \left(1 + c_{01}\right)\right)^2\\
G &= 2 s_{12} C (1 + c_{01}) (S |\mathbf{V}| + C (c_{01} c_{12} - c_{20}))\\
q &= \frac{\arctan\left( G, F\right)}{\arccos \left(c_{12} \right)} \\
\mathbf{\hat{p}} &= \operatorname{Slerp}\left(\mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2; q\right) \\
t &= \frac{\arccos \left( 1 - h^2 (1 - \mathbf{\hat{v}}_0 \cdot \mathbf{\hat{p}})\right)}
{\arccos \left(\mathbf{\hat{v}}_0 \cdot \mathbf{\hat{p}} \right)} \\
\mathbf{\hat{v}} &= \operatorname{Slerp}\left(\mathbf{\hat{v}}_0, \mathbf{\hat{p}}; t\right)
\end{split}$$

# Special cases

Some special cases simplify significantly. For brevity I'll just do the forward formulas.
For both of these, let $$\mathbf{z}_0 = [0,0]$$, $$\mathbf{z}_1 = [1,-1]$$, and $$\mathbf{z}_2 = [1, 1]$$, and define $$\mathbf{z}=[x, y]$$ in terms of barycentric coordinates: $$\mathbf{z} = \sum \mathbf{z}_i \beta_i$$, or in terms of components directly,

$$\begin{split}
x &=  &\beta_1 + \beta_2 = h,\\
y &= -&\beta_1 + \beta_2 = 2\beta_2 - h
= h \left(2\frac{A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}})}
                 {A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2)} - 1\right)
\end{split}$$

Also recall [the formula for a vector in terms of latitude and longitude]({% post_url 2021-05-01-vector-spherical-geometry %}):

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

## Hemisphere to square
<img src="/assets/images/snyder/hemisphere.svg" alt="Diagram of projection from hemisphere to plane" height="150" align="right"/>
Consider a hemisphere with four evenly spaced vertices along its boundary, a hemispherical square if you will. Using the center point, this can be divided into four triangles, effectively forming a spherical octahedron. The following derivation is for the triangle in green.

Let $$\mathbf{\hat{v}}_0 = [0, 0, 1]$$, $$\mathbf{\hat{v}}_1 = \frac{1}{\sqrt{2}}[1, -1, 0]$$, and $$\mathbf{\hat{v}}_2 = \frac{1}{\sqrt{2}}[1, 1, 0]$$. Then $$\mathbf{\hat{v}}_i \cdot \mathbf{\hat{v}}_j = 0$$ for all $$i \ne j$$, and $$A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2) = \frac{\pi}{2}$$.

First, note that $$\mathbf{\hat{p}}$$ lies in the same plane as $$\mathbf{\hat{v}}_1$$ and  $$\mathbf{\hat{v}}_2$$, so $$\mathbf{\hat{p}} \cdot \mathbf{\hat{v}}_0$$ is also 0. Also recognize that $$\varphi \in [-\frac{\pi}{2}, \frac{\pi}{2}]$$. Thus, $$h$$ simplifies as so:

$$
h = \sqrt{1 - \hat{v}_z} = \sqrt{1 - \sin(\varphi)} = \sqrt{2} \sin\left(\frac{\pi}{4} - \frac{\varphi}{2}\right).
$$

Second, note that the interior angle at vertex $$\mathbf{\hat{v}}_1$$ is a right angle, and so is the interior angle at $$\mathbf{\hat{p}}$$. From the fact that the area of a spherical triangle is the sum of its interior angles minus $$\pi$$, it follows that $$A\left(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}}\right) = \frac{\pi}{4} + \lambda$$, so:

$$
\beta_2 = h \left( \frac{2}{\pi}\lambda + \frac{1}{2}\right).
$$

Combining everything gives this formula:

$$\begin{split}
x &= \sqrt{2} \sin\left(\frac{\pi}{4} - \frac{\varphi}{2}\right),\\
y &= \frac{4}{\pi} x \lambda.
\end{split}$$

This is equivalent to the formula for the [Collignon projection](https://en.wikipedia.org/wiki/Collignon_projection) with an affine transformation. Snyder didn't mention it in his paper, despite having included the Collignon projection in his 1989 [Album of Map Projections](https://doi.org/10.3133%2Fpp1453) with Voxland.[^Voxland] Snyder may not have been interested in the hemisphere case – he didn't include any hemisphere examples in his paper – so he might not have bother including the fact in the paper.

Lambers (not Lambert) in a paper "Mappings between sphere, disc, and square"[^Lambers] used a mapping from a hemisphere to a square that is a composition of the [Lambert azimuthal equal-area projection](https://en.wikipedia.org/wiki/Lambert_azimuthal_equal-area_projection) and the Shirley-Chiu equal-area mapping between the disk and the square.[^Chiu] This, when restricted to a single quadrant, is also equivalent to the Collignon projection with an affine transformation. This is easiest to see in polar form:

$$\begin{split}
\left(r, \Theta \right) &= \left( \sqrt{2} \sin \left(\frac{\pi}{4} - \frac{\varphi}{2}\right), \theta\right) \\
\left(x, y \right) &= \left(r, \frac{4}{\pi}r\Theta\right),\, \Theta \in [-\frac{\pi}{4}, \frac{\pi}{4}]
\end{split}$$

The map can be applied to other triangles by negating $$\varphi$$, $$x$$, and $$y$$, and/or replacing $$\lambda$$ with $$(\lambda - \lambda_0)$$, where $$\lambda_0$$ is the meridian passing through the center of the face. It may be necessary to add or subtract $$2\pi$$ from $$(\lambda - \lambda_0)$$ to move it into the appropriate range.
The projection for a triangle from a hemisphere divided into 3 parts, or generally a hemisphere divided into $$n$$ parts, is the same up to affine transformation.

## Cube face to square
<img src="/assets/images/snyder/cube.svg" alt="Diagram of projection from cube to plane" height="150" align="right"/>
Now consider a cube on the sphere. Dividing each face into triangles creates a spherical [tetrakis cube](https://en.wikipedia.org/wiki/Tetrakis_hexahedron).

Let $$\mathbf{\hat{v}}_0 = [0, 0, 1]$$ as before, but $$\mathbf{\hat{v}}_1 = \frac{1}{\sqrt{3}}[1, -1, 1]$$, and $$\mathbf{\hat{v}}_2 = \frac{1}{\sqrt{3}}[1, 1, 1]$$. Then $$\mathbf{\hat{v}}_0 \cdot \mathbf{\hat{v}}_1 = \mathbf{\hat{v}}_0 \cdot \mathbf{\hat{v}}_2 = \frac{1}{\sqrt{3}}$$, $$\mathbf{\hat{v}}_1 \cdot \mathbf{\hat{v}}_2 = \frac{1}{3}$$, $$\lvert \mathbf{V}\rvert = \frac{2}{3}$$, and $$A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_2) = \frac{\pi}{6}$$.

First, here are some expressions involving $$\mathbf{\hat{p}}$$. Note that $$\mathbf{\hat{v}}_0 \times \mathbf{\hat{v}}_1 = \frac{1}{\sqrt{3}}[1,1,0]$$ and $$\mathbf{\hat{v}}_1 \times \mathbf{\hat{v}}_2 = \frac{2}{3}[-1, 0, 1]$$. To avoid some repetition, let $$b = \sqrt{2v_x^2 + v_y^2}$$.

$$\begin{split}
\mathbf{\hat{p}} &= \frac{\mathbf{\hat{v}} - (v_z - v_x) \mathbf{\hat{v}}_0}
{b} \\
\mathbf{\hat{v}}_0 \cdot \mathbf{\hat{p}} &= \frac{v_x}
{b} \\
\mathbf{\hat{v}}_1 \cdot \mathbf{\hat{p}} &= \frac{2v_x - v_y}
{\sqrt{3}\, b} \\
|\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}}| &= \frac{v_x + v_y}
{\sqrt{3}\, b} \\
A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}}) &= 2\arctan \frac{v_x + v_y}{
\left(2+\sqrt{3}\right)v_x - v_y + \left(1+\sqrt{3}\right)b}
\end{split}$$

Then, with a bunch of tedious algebra and a couple $$\arctan$$ identities, we find that

$$\begin{split}
x &= \sqrt{b \frac{b+v_x}{1+v_z}}\\
y &= x \frac{12}{\pi}\arctan \left(\frac{v_y}{b + 2 v_x}\right)
\end{split}$$

Projections for the other triangles can be found by permuting the vector and negating components of the vector and the result. This projection is the one used in the [COBE sky cube](https://en.wikipedia.org/wiki/COBE_sky_cube) scheme. Well, one of them. The projection used in the earlier paper[^Chan] is different than the one in the next paper.[^ONeill][^Patt] The later one is the exact form, which is more common and is the one used here. (Note that the original derivation is for a different face.)

# Extension outside the triangle

Snyder's equal-area projection can be extended outside the triangle. This isn't particularly useful in its usual application, but can be nice for analysis.

Only one change to the forward formula is needed, and even that's optional. As one vertex of a triangle is moved around the sphere, there is a place on the far side where its signed area changes sign with a discontinuity. The placement of this discontinuity can be changed by splitting the triangle $$\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}}$$ to offset the area.  Formula-wise, say we want the discontinuity to lie opposite the midpoint $$\mathbf{\hat{m}}$$  between $$\mathbf{\hat{v}}_1$$ and $$\mathbf{\hat{v}}_2$$. Just replace $$A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{p}})$$ in the forward formula with $$A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_m, \mathbf{\hat{p}}) + A(\mathbf{\hat{v}}_0, \mathbf{\hat{v}}_1, \mathbf{\hat{v}}_m)$$.

# Irregular triangles

Snyder's paper only looked at regular polygons, which can be divided symmetrically into isosceles triangles. It is possible to divide an irregular spherical triangle into three equal-area triangles, using the [spherical area centroid]({% post_url 2021-05-02-spherical-triangle-centers %}). These sub-triangles are not isosceles, but the formulas stated above work perfectly fine for irregular triangles.

# References
[^Snyder]: John P Snyder (1992). An equal-area map projection for polyhedral globes. Cartographica: The International Journal for Geographic Information and Geovisualization, 29(1):10–21. [doi://10.3138/27H7-8K88-4882-1752](https://doi.org/10.3138/27H7-8K88-4882-1752)
[^Voxland]: Snyder, J.P.; Voxland, P.M. (1989). Album of Map Projections, United States Geological Survey Professional Paper. United States Government Printing Office. [doi://10.3133/pp1453](https://doi.org/10.3133%2Fpp1453)
[^Lambers]: Lambers, M. (2016) [Mappings between sphere, disc, and square.](http://jcgt.org/published/0005/02/01/) Journal of Computer Graphics Techniques (JCGT), vol. 5, no. 2, 1-21
[^Chiu]: Shirley, P.; Chiu, K. (1997). A low distortion map between disk and square. Journal of graphics tools 2, 3, 45–52. [doi://10.1080/10867651.1997.10487479](http://dx.doi.org/10.1080/10867651.1997.10487479)
[^Chan]: Chan, F.K.; O'Neill, E. M. (1975). [Feasibility Study of a Quadrilateralized Spherical Cube Earth Data Base](https://ntrl.ntis.gov/NTRL/dashboard/searchResults/titleDetail/ADA010232.xhtml) (CSC - Computer Sciences Corporation, EPRF Technical Report 2-75) (Technical report). Monterey, California: Environmental Prediction Research Facility.
[^ONeill]: O'Neill, E. M.; Laubscher, R. E. (1976). [Extended Studies of a Quadrilateralized Spherical Cube Earth Data Base](https://apps.dtic.mil/dtic/tr/fulltext/u2/a026294.pdf) (Technical report). Monterey, California: Environmental Prediction Research Facility.
[^Patt]: Fred Patt (1993). Comments on draft WCS standard, in Usenet group sci.astro.fits. [archive 1](http://www.sai.msu.su/~megera/wiki/relation_featur%E5s/SAI_RVO/SkyPixelization/SphereCube/SphereCube), [archive 2](https://groups.google.com/g/sci.astro.fits/c/KonX3W7m-xY/m/aAPO_dR4NXMJ)
[^Harrison]: Harrison E., Mahdavi-Amiri A., Samavati F. (2012) Analysis of Inverse Snyder Optimizations. In: Gavrilova M.L., Tan C.J.K. (eds) Transactions on Computational Science XVI. Lecture Notes in Computer Science, vol 7380. Springer. [doi://10.1007/978-3-642-32663-9_8](https://doi.org/10.1007/978-3-642-32663-9_8)
