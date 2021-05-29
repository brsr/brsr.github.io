---
layout: post
title: "Polyhedra may be beautiful, but these ones aren't"
tags: geometry antiprism cursed
---
Often, beauty in mathematics is described as relating to simplicity or symmetry. The procedure about to be described is interesting because despite being simple and symmetric, the end result looks a little like it suffers from an untreated skin condition.

Take some polyhedron with triangular faces. Add a point at the center of each face (it doesn't have to be exact), and draw new edges from the vertices of each original face to this new point, and from each of the new points to the new points on adjacent faces. Create a new polyhedron based on these new edges. Don't project the new points onto a sphere or otherwise move them. Then, repeat the process a few times. This can be done with the program `conway` in [Antiprism](http://www.antiprism.com/). Below, `I` indicates the icosahedron, `n` is the "needle" operation (basically what I described above: also the same as the Goldberg-Coxter triangular 1,1 operation), and `-p x` disables the usual step of [canonicalizing the polyhedron](https://www.georgehart.com/virtual-polyhedra/canonical.html).

    conway -p x nI
    conway -p x nnI
    conway -p x n^3I
        etc...

Here's an animated gif of the series from the original icosahedron to `n^6`, applying the operation 6 times. It seems to bend inwards a little, turning the original vertices into barbs and adding broader welts at points of symmetry. The result is something very organic-looking, bringing to mind [microscope images of pollen](https://commons.wikimedia.org/wiki/File:Misc_pollen.jpg) and [electron microscope images of viruses](https://commons.wikimedia.org/wiki/File:Icosahedral_Adenoviruses.jpg). It's also... somehow threatening? I realize the monochrome grey isn't doing it any favors, but I tried other colors and they were just as bad. Red looks like a [Cacodemon from the game Doom](https://doomwiki.org/wiki/Cacodemon). Green? Ew. Polyhedra with sharper vertex angles, like tetrahedra, end up with sharper barbs but look similar.
<p align="center">
<img alt="An icosahedron modified in the way described in this post" src="/assets/images/ugly_ico.gif" />
</p>

A torus made of triangular faces can be created with this Antiprism command:

    unitile2d -s t 2 -w 5

Applying the same series of operations produces a similarly threatening object. On the inside of the torus, where the curvature is negative, the barbs and welts become pock marks instead.
<p align="center">
<img alt="A torus modified in the way described in this post" src="/assets/images/ugly_torus.gif" />
</p>
