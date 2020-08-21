---
layout: post
title: "Balloon Polyhedra"
tags: geometry antiprism
images1:
  - name: Triangular balloon polyhedron...
    link: /assets/images/balloon/balloon3.png
  - name: ...and the opposite side
    link: /assets/images/balloon/balloon3a.png
images2:
  - name: Quadrilateral balloon polyhedron...
    link: /assets/images/balloon/balloon4.png
  - name: ...and the opposite side
    link: /assets/images/balloon/balloon4a.png
imagewidth: 45%
---

Aside from the Platonic solids, there are no regular tilings of the sphere. You can't, for instance, have a spherical polyhedron where the vertices are all connected to 6 edges. You need to introduce a non-triangular face, or a vertex with a different connectivity. This is why the [geodesic polyhedra](https://en.wikipedia.org/wiki/Geodesic_polyhedron) and [Goldberg polyhedra](https://en.wikipedia.org/wiki/Goldberg_polyhedron) have the structure they do: most of the faces and vertices are regular, but a small number are not.

You can, however, cheat a little. Suppose you want most of your polyhedron to be regular, but it's OK if there's a small region where anything goes. For example, maybe you want a regular grid over most of the Earth, but you're only really interested in the land areas. All the irregularity can go near [the South Pacific pole of inaccessibility](https://en.wikipedia.org/wiki/Pole_of_inaccessibility#Oceanic_pole_of_inaccessibility). (R'lyeh already has non-Euclidean geometry, let Cthulhu deal with it...)

Such polyhedra can be constructed by drawing a larger polygon over a planar grid, then contracting all the points outside the polygon or on its borders into a single point. Any duplicate edges between the same two vertices are treated as a single edge. Then, "inflate" the resulting graph into a spherical polyhedron. I call the result a balloon polyhedra, but I'm probably not the first to define this. These polyhedra can be created using the `balloon.py` script in my [Antitile]({% post_url 2017-12-07-antitile %}) package.

Here's such a polyhedron, created by drawing an equilateral triangle with sides of length 15 over a regular triangular grid with edges of length 1. The coloring is a minimal coloring calculated by `off_color` in [Antiprism](http://www.antiprism.com/). The front side looks like a nice regular grid. The back side is more complicated. Most of the vertices of the polyhedron are of order 6, but some have order 5, three have order 3, and the one at the "pole" on the back side (the point that all those other points were contracted to) has a whopping order 36.
{% include image-gallery.html listing = page.images1 width = page.imagewidth %}

The same thing can be done with a quarilateral grid. Here's the result of a square with sides of length 15 over a regular square grid. Most of the faces are quadrilaterals, and most of the vertices have order 4. 52 faces are triangles, though; 4 vertices have order 3, and the pole has order 52. (Note the few red faces near the pole. Also, not all the quad faces are flat; if your definition of "polyhedron" strictly requires planar faces, tough.)
{% include image-gallery.html listing = page.images2 width = page.imagewidth %}

With some shifting of the vertices of such polyhedron, the regular part of the polyhedron could be enlarged and the irregular part shrinked. Of course, there are significant differences in aspect ratio and size between the faces of these polyhedra.
