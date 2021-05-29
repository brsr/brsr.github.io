---
layout: post
title: "A (sort of) Euclidean triangle on a non-Euclidean surface"
tags: geometry cursed

images1:
  - name: Rectangle on a ring torus, cross-section
    link: /assets/images/torus/ringtorus1.png
  - name: Alternate angle
    link: /assets/images/torus/ringtorus2.png

images2:
  - name: Triangle on a horn torus, cross-section
    link: /assets/images/torus/horntorus1.png
  - name: Alternate angle
    link: /assets/images/torus/horntorus2.png

imagewidth: 45%
---

A question that came up in a math chatroom (yes, I'm the kind of nerd who spends time in math chatrooms): find a "Euclidean" triangle on a non-Euclidean surface. More exactly, find a geodesic triangle on a surface with non-constant [Gaussian curvature](https://en.wikipedia.org/wiki/Gaussian_curvature) having the same sum of interior angles as an Euclidean triangle. In [spherical geometry](https://en.wikipedia.org/wiki/Spherical_geometry), where curvature is a positive constant, that sum is larger than 180 degrees, while in [hyperbolic geometry](https://en.wikipedia.org/wiki/Hyperbolic_geometry) (negative constant curvature) it is smaller. Intuitively, on a surface with regions of positive and negative curvature, maybe one can find a triangle where it balances out. (The [Gauss–Bonnet theorem](https://en.wikipedia.org/wiki/Gauss%E2%80%93Bonnet_theorem) formalizes this notion, although here I won't use it directly.)

The torus is such a surface: it has positive curvature on the outer region, and negative curvature on the inner region. [In general the geodesics of the torus are complicated](http://www.rdrop.com/~half/math/torus/geodesics.xhtml), but there are some "trivial" geodesics: the inner and outer equators, and the meridians that cut through the torus at right angles to the equators. Let's see what can be built out of the trivial geodesics.

Consider a geodesic quadrilateral on a ring torus (the usual kind with a hole in the middle), formed by the inner equator, a meridian, the outer equator, and another meridian. Meridians meet equators at right angles, so this quadrilateral has an interior angle of 90 degrees at each vertex. This is a polygon with the angle sum we would expect from the equivalent Euclidean polygon. It's a rectangle! Sort of. Its edges along the meridians have the same length, but its edges along the equators have different lengths.

{% include image-gallery.html listing = page.images1 width = page.imagewidth %}

Now consider the same polygon, but on a horn torus instead of a ring torus. The inner equator shrinks into a single point at the origin, and two of the vertices of the rectangle become a single point.

{% include image-gallery.html listing = page.images2 width = page.imagewidth %}

Each of the meridians is tangent to each other at the origin. Thus, the interior angle at that vertex is zero. The other two vertices have right angles at their vertices, as before. Although a triangle with angles of 90, 90, and 0 degrees could not exist in Euclidean geometry, the angle sum is 180 degrees, so it meets the original criteria.

Reactions in the group chat to this "Euclidean" triangle were... let's say, mixed.

*[The Jupyter Notebook used to create the images is available on Github.](https://github.com/brsr/math/blob/master/Curved%20polygon%20on%20a%20torus.ipynb) If downloaded and brought into a local Jupyter session, the 3d plot can be manipulated in interactive mode.*
