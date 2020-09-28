---
layout: post
title: "Trends among geodesic polyhedra"
tags: geometry antiprism
images:
  - name: (7,0), Method 1
    link: /assets/images/geodesic/tet70s.png
  - name: (7,0), unprojected
    link: /assets/images/geodesic/tet70p.png
  - name: (7,0), equal edges
    link: /assets/images/geodesic/tet70p.png
  - name: (6,2), Method 1
    link: /assets/images/geodesic/tet62s.png
  - name: (6,2), unprojected
    link: /assets/images/geodesic/tet62p.png
  - name: (6,2), equal edges
    link: /assets/images/geodesic/tet62u.png
  - name: (5,3), Method 1
    link: /assets/images/geodesic/tet53s.png
  - name: (5,3), unprojected
    link: /assets/images/geodesic/tet53p.png
  - name: (5,3), equal edges
    link: /assets/images/geodesic/tet53u.png
  - name: (4,4), Method 1
    link: /assets/images/geodesic/tet44s.png
  - name: (4,4), unprojected
    link: /assets/images/geodesic/tet44p.png
  - name: (4,4), equal edges
    link: /assets/images/geodesic/tet44u.png
imagewidth: 30%
---
[Geodesic polyhedra](https://en.wikipedia.org/wiki/Geodesic_polyhedron) are the abstract geometric version of Buckminster Fuller's geodesic domes. Take a polyhedron with triangular faces (usually the icosahedron) and subdivide each face using a regular triangular grid. Geodesic domes have fallen out of favor as structures, but geodesic polyhedra are still applied in 3d modeling and GIS. Geodesic polyhedra are a specific application of [the Goldberg-Coxeter construction](https://en.wikipedia.org/wiki/Goldberg%E2%80%93Coxeter_construction). Anything I don't explain in this post is in one of the linked Wikipedia pages.

Class is a qualitative description of how skewed the dividing grid (or PPT in geodesic dome lingo, or master triangle in Goldber-Coxeter jargon) is with respect to the original faces. If $$a > 0$$ and $$b ≥ 0$$ are the parameters defining the dividing grid of the geodesic polyhedron, then Class I are the polyhedra where $$b=0$$, Class II have $$a=b$$, and Class III are everything else. $$(b,a)$$ is the mirror image of $$(a,b)$$. Let's define a quantity to represent class with finer granularity: let $$c = min(a,b)/max(a,b)$$. It follows that for Class I, $$c=0$$, for Class II, $$c=1$$, and for Class III, $$c$$ is between 0 and 1.

Vaguely defining "regularity" as the consistency in edge length, face area, etc., the trend seems to be, from more regular to less regular, I to III to II. Geodesic dome enthusiasts have noticed that Class I domes tend to have edges with a more consistent length than Class II domes.  [Altschuler et al. also conjectured](https://www.mcs.anl.gov/~zippy/publications/thomson/thomsonPRL.html) that, given two icosahedral geodesic polyhedra with equal numbers of vertices, if a unit charge is placed at each vertex like in the [Thomson problem](https://en.wikipedia.org/wiki/Thomson_problem), the one with the lower $$c$$ value has the lower Thomson energy. These measurements have consequences: structurally, triangles with closer to equal edge lengths are stronger than other triangles, although for geodesic dome design the difference may not be significant.

I'll use geodesic polyhedra based on the tetrahedron, to make the regularities (or absence of) more noticable. There are two such geodesic polyhedra with 100 vertices, having parameters $$(a,b) = (7,0)$$ and $$= (5,3)$$. To illustrate the trend over $$c$$, let's throw in polyhedra withalmost the same number of vertices: $$(4,4)$$, with 98 vertices and $$(6,2)$$, with 106 vertices. Here they are illustrated in three forms: geodesic polyhedra projected with Method 1, with the new vertices left on the planar surfaces of the original polyhedron ("unprojected"), and scaled using a minimax procedure so that all edges are equal. As usual, everything is built with [Antiprism](http://www.antiprism.com/).

{% include image-gallery.html listing = page.images width = page.imagewidth %}

Let's define some variables to quantify "regularity".

* The ratio of the longest edge length to the shortest edge length.
* The ratio of the largest face area to the smallest face area.
* The Thomson energy, as mentioned above. Represents how regularly spaced the vertices are from one another.
* A variable depending on the total area of the equal-edge polyhedron, comparing it to the original polyhedron. Define this as $$ \frac{A}{L^2} \frac{L'^2}{A'}$$, where $$A$$ is the total surface area of the equal-edge polyhedron, and $$L$$ is the distance between any two of the vertices corresponding to the original vertices. $$A'$$ here is the area of the original polyhedron, and $$L'$$ is the length of its edges. For a regular tetrahedron, $$\frac{A'}{L'^2} = \sqrt{3}$$. (It's $$2\sqrt{3}$$ for a regular octahedron and $$5\sqrt{3}$$ for a regular icosahedron.) So basically it's comparing the area of the equal-edge polyhedron to the area of the original polyhedron, with appropriate scaling.

Below is a table of these quantities. For the $$(7,0)$$ polyhedron (and, in fact, all Class I or $$c=0$$ polyhedra), the unprojected polyhedron's faces lie within the original faces, so it already has equal edge lengths. The last three columns increase monotonically with $$c$$. The Thomson energy does not in general, but does for the two polyhedra with the same number of vertices. The Thomson problem is notoriously finicky, so that's not too surprising. (The lowest Thomson energy configuration known for 100 vertices is 4448, which is lower than the $$(7,0)$$ polyhedron, but we're just concerned with comparisons between different geodesic polyhedra.)

| a | b | c |Vertices|Edges|Faces|Thomson energy (projected)|Edge length ratio (unprojected)|Edge length ratio (projected)|Face area ratio (projected)|Total area ratio (equal-edge)|
|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| 7 | 0 | 0.00 |100 |294|196| 4497 | 1.000 | 1.837 | 3.055 | 1.000 |
| 6 | 2 | 0.33 |106 |312|208| 5089 | 1.612 | 2.398 | 5.148 | 1.028 |
| 5 | 3 | 0.60 |100 |294|196| 4521 | 1.698 | 2.645 | 6.770 | 1.049 |
| 4 | 4 | 1.00 | 98 |288|192| 4342 | 1.732 | 2.839 | 7.024 | 1.110 |

So let's make some more conjectures. Given two or more geodesic polyhedra with vertices lying on a sphere (and no intersecting faces or any other funny business), using the same regular polyhedron as the base, and the same projection method:

* If the polyhedra have the same number of vertices, then the one with lowest $$c$$ has the lowest edge length ratio, the lowest face area ratio, the lowest total area ratio, and the lowest Thomson energy. This may also apply to other measures of regularity not included here: spherical length and solid angle, for instance.
* If the polyhedra each have numbers of vertices lying in an interval $$[n, n+\epsilon]$$, the above also holds for some $$\epsilon > 0$$ that may depend on $n$ and the measure being considered.
* Given a certain measure, the above also holds for the realization of that polyhedra that is optimal with respect to that measure (under the same conditions we started with, and preserving topology, of course). (Altschuler et al. explore this in their paper, with the measure being the Thomson energy: it seems to hold.)

I suspect that the unprojected and equal-edge polyhedra would be useful to prove these conjectures. I haven't had a chance to try proving it yet, because it's 2020 and I barely had time to make this blog post.

Final notes: I personally dislike the name "geodesic polyhedra", since it overloads the already overloaded term "geodesic", but there's not really a better name in common use. Fuller polyhedra, maybe? Buckyhedra? And since somewhat disturbing-looking polyhedra seem to be my thing now, here's an animation transitioning between the equal-edge, planar, and spherical versions of tetrahedral (4,4).
<p align="center">
<img alt="Animation of the tetrahedral (4,4) geodesic polyhedron" src="/assets/images/geodesic/tet44.gif" />
</p>
