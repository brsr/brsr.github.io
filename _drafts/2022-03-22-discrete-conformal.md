---
layout: post
title: Discrete conformal polyhedral projection
tags: cartography cursed python
---

[The last post]({% post_url 2021-03-03-conformal-polyhedral %}) ended with a promise that I'd write about another dubious way to calculate conformal polyhedral projections. This version relies on the theory of discrete analytic functions.

# The what now?

Let's take a step back. Conformal functions -- the usual kind of complex analytic function -- can be described as transforming infinitesimal circles into infinitesimal circles, rather than to ellipses or polygons or something even worse. So the motivation here is, maybe we can find a function that transforms some circles of a small positive radius into other small circles.

# Finding circle packings

The "meta-code" given in page 29 of Stephenson's book[^Stephenson2005] is as so:

1. Initialize radii $$r_i$$.
2. For each $$i$$:
  a. Compute the interior angle sum $$\theta_i$$ using $$r_i$$, $$r_j$$ for neighboring $$j$$, and the Law of Cosines for whatever geometry.
  b. If $$\theta_i < 2\pi$$, decrease $$r_i$$. If $$\theta_i > 2\pi$$, increase $$r_i$$
3. Repeat step 2 until $$\theta_i$$ is within a given tolerance of $$2\pi$$ for all $$i$$.

Once the above has converged, the vertices are placed one-by-one based on $$r_i$$: the exact details of that placement vary depending on the application.

The "meta-code" is a little vague: increase or decrease $$r_i$$ by how much? Collins and Stephenson give full fledged pseudocode in their 2003 paper[^Stephenson2003], although it has technical issues with spherical geometry.


[Ken Stephenson](http://www.circlepack.com/)

[^Stephenson2003]: Collins, Charles R.; Stephenson, Kenneth. (2003) A circle packing algorithm. Computational Geometry 25, pp. 233--256.
[^Stephenson2005]: Stephenson, Kenneth. (2005) Introduction to Circle Packing: The Theory of Discrete Analytic Functions. Cambridge University Press, NY. ISBN 9780521823562
