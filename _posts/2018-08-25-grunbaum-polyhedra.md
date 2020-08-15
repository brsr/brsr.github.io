---
layout: post
title: "A Grünbaum Polyhedron in Antiprism"
tags: geometry antiprism
---
A paper from 2012, [The Regular Grünbaum Polyhedron of Genus 5](https://arxiv.org/abs/1212.6588), includes a polyhedron of genus 5, earlier discovered by Grünbaum, that is an embedding of the {3,8} Fricke-Klein [regular map](https://en.wikipedia.org/wiki/Regular_map_(graph_theory)). The polyhedron (Figure 2 and 3) can be recreated using the `wythoff` program in [Antiprism](http://www.antiprism.com/), although the exact proportions are a little different in the paper.

    wythoff -p [3VE2F]0EV,0fe0EV0fevfV,0F,0V0F0evfe oct

The polyhedron in Figure 4 can be created with a small change:

    wythoff -p [3VE2F]0EV,0fe0EV0fevfV,0F,0V0E0F oct

These can also be used to generate the genus 11 polyhedra in Figure 5: just replace `oct` with `ico`.

Here's the titular Grünbaum polyhedron of genus 5:
<p align="center">
<img alt="The Grünbaum polyhedron of genus 5" src="/assets/images/grunbaum5.gif" />
</p>
