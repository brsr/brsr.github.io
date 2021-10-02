---
layout: post
title: "A variation on the Chamberlin trimetric map projection"
tags: cartography
---

[My paper "A variation on the Chamberlin trimetric map projection" was just published in Cartography and Geographic Information Science.](https://www.tandfonline.com/doi/full/10.1080/15230406.2021.1975571) If you don't have institutional access, [the preprint is on my github repo](https://github.com/brsr/trimetric/blob/main/tex/chamberlin_variation.pdf).

**The casual summary:** A compromise map projection is one that isn't [area-preserving](https://en.wikipedia.org/wiki/Equal-area_map) or [conformal](https://en.wikipedia.org/wiki/Conformal_map_projection) (equal-angle). Conformal map projections have a lot of area distortion, and equal-area map projections have a lot of angle distortion. Compromise projections can have less noticable distortion than conformal or equal-area map projections. The concept of the just-noticable difference from [psychophysics](https://en.wikipedia.org/wiki/Psychophysics) is relevant: for visual stimuli, the just-noticable difference is in the ballpark of 5%.

[The Chamberlin trimetric map projection](https://en.wikipedia.org/wiki/Chamberlin_trimetric_projection) is a compromise map projection, useful for maps of continents and large regions. Without going into detail, it uses triangulation. It has a nice geometric construction, but the corresponding algebraic formula is long and fussy. It also lacks an easy-to-calculate inverse.

By changing how the triangulation is done very slightly, a projection with a much nicer algebraic formula was found. This was called the Matrix trimetric projection, since the formula involves the product of a 3x3 matrix with a vector. It takes about half the time to calculate as the Chamberlin projection. The formula can be inverted easily using a simple one-parameter [Newton's method](https://en.wikipedia.org/wiki/Newton%27s_method) iteration. The Matrix trimetric causes slightly more area and angle distortion than the Chamberlin, but it's still in the ballpark of that just-noticable difference.

**The personal summary:** This was my COVID project. A good distraction from doomscrolling and compulsively reloading COVID dashboards, I suppose. I was intimidated by submitting an academic paper as someone who's not affiliated with an institution any more, but it turned out to be a non-issue.
