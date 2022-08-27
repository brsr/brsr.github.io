---
layout: post
title: "Edges in the image of the Schwarz triangle function"
tags: complex_functions

---
<a href="https://commons.wikimedia.org/wiki/File:Schwarz_triangle_function.svg"><img src="/assets/images/Schwarz_triangle_function.svg" alt="Image of the upper half-plane transformed by the Schwarz triangle function with various parameters" height="300" align="right"/></a>

[A few posts ago, the Schwarz triangle function came up in the discussion of conformal map projections.]({% post_url 2022-03-03-conformal-polyhedral %}) The Schwarz triangle function maps the upper half-plane to a triangle with circular arcs for edges. The real line (including infinity) becomes the boundary of the triangle. "Circular arc" should be understood to include straight lines, thought of as an arc of a circle with its center at infinity. In the image, you can see that two of those edges are straight lines. All are straight lines for the graph in the bottom left; its parameters define a mapping to a Euclidean triangle. For the other cases, can we determine the circle that the rightmost edge is part of?

# Schwarz triangle function
Let's not repeat everything from the earlier post, just what's relevant. The [Schwarz triangle function](https://en.wikipedia.org/wiki/Schwarz_triangle_function) $$s(z)$$ is parameterized by the three interior angles of the triangle, $$πα$$, $$πβ$$, and $$πγ$$. If $$α +β +γ$$ is greater than 1, the triangle is spherical; if equal to 1, Euclidean, and if less than 1, hyperbolic. $$πα$$ is the interior angle at $$s(0)$$, but the interior angle at $$s(1)$$ is $$πγ$$, and the angle at $$s(\infty)$$ is $$πβ$$. For some reason the angles are labeled in clockwise order even though counterclockwise is conventional in complex analysis. If it really annoys you, replace $$β$$ with $$γ$$ and vice versa.

$$α$$, $$β$$, $$γ$$ can take values from 0 to 1. For an ideal triangle, all are 0: for a hemispherical triangle, all are 1. We define six parameters, and determine their range for later use:

<table><tr>
<td>$$\begin{split}
&a &= (1−α−β−γ)/2 \in [-1, 1/2],\\
&a' &= (1+α−β−γ)/2 \in [-1/2, 1],
\end{split}$$</td>
<td>$$\begin{split}
&b &= (1−α+β−γ)/2 \in [-1/2, 1],\\
&b' &= (1+α+β−γ)/2 \in [0, 3/2],
\end{split}$$</td>
<td>$$\begin{split}
&c &= 1 − α \in [0, 1], \\
&c' &= 1 + α \in [1, 2].
\end{split}$$</td>
</tr></table>

The transformed vertices can be expressed using [gamma functions](https://en.wikipedia.org/wiki/Gamma_function):

$$\begin{aligned}
s(0) &= 0, \\
s(1) &= \frac
  {\Gamma(1-a')\Gamma(1-b')\Gamma(c')}
  {\Gamma(1-a)\Gamma(1-b)\Gamma(c)}, \\
s(\infty) &= \exp\left(i \pi \alpha \right)\frac
  {\Gamma(1-a')\Gamma(b)\Gamma(c')}
  {\Gamma(1-a)\Gamma(b')\Gamma(c)}.
\end{aligned}$$

Define $$x_\infty$$ as the real part of $$s(\infty)$$ and $$y_\infty$$ as the imaginary part, such that $$s(\infty) = x_\infty + i y_\infty$$. Also, for consistency, define $$x_1 = s(1)$$. $$s(1)$$ is real, so $$y_1 = 0$$.


# Circle 

A circle in the plane has equation 

$$
(x - x_c)^2 + (y - y_c)^2 = r^2,
$$

where $$r$$ is the radius and $$x_c, y_c$$ is the center of the circle. We're expressing things as $$\mathbf{R}^2$$ instead of $$\mathbf{C}$$ for simplicity, but we can define $$z_c = x_c + i y_c$$.

The equation of the circle can be rearranged as 

$$
x^2 + y^2 = 2xx_c + 2yy_c + r^2 - x_c^2 - y_c^2
$$

Let $$u=2x_c$$, $$v=2y_c$$, and $$w = r^2 - x_c^2 - y_c^2$$.

$$
x^2 + y^2 = u x + v y + w
$$

This can be used to create a linear system, but we only have two points on the circle, and three unknowns. However, we know the interior angles at the vertices $$s(1)$$ and $$s(\infty)$$, and we can use one of those to complete the system.

Taking the implicit derivative of the last equation gives:

$$
2x + 2y \frac{dy}{dx} = u + v \frac{dy}{dx}
$$

We know the slope of the tangent line at $$s(1)$$, because we know the interior angle there. (We also know that for $$s(\infty)$$, but it's more complicated and we only need one to complete the system.) Therefore, at $$s(1)$$,

$$\frac{dy}{dx} = -\tan \left(\pi \gamma \right),$$

and plugging that and the values for $$x$$ and $$y$$ in gives

$$
2x_1 = u - v \tan \left(\pi \gamma \right).
$$

# Solving the system
Bringing everything together into matrix form gives:

$$
\begin{bmatrix}
 x_1 & 0 & 1\\
 x_\infty & y_\infty & 1\\
 1 & -\tan \left(\pi \gamma \right) & 0\\
\end{bmatrix}

\begin{bmatrix} u \\v \\ w
\end{bmatrix}
=
\begin{bmatrix} x_1^2 \\ x_\infty^2 + y_\infty^2\\ 2x_1 
\end{bmatrix}.
$$

This system can be solved with [Gaussian elimination](https://en.wikipedia.org/wiki/Gaussian_elimination) in a fairly straightforward if a bit tedious manner, resulting in:

$$\begin{split}
u &= \frac{ 2 x_1 y_\infty - \tan \left(\pi \gamma \right) (x_1^2 - x_\infty^2 - y_\infty^2) }{y_\infty -\tan \left(\pi \gamma \right) (x_1 - x_\infty)} \\
v &= \frac{ (x_1 - x_\infty)^2 + y_\infty^2 }{y_\infty -\tan \left(\pi \gamma \right) (x_1 - x_\infty)} \\
w &= x_1 \frac{ (\tan \left(\pi \gamma \right) (x_1 x_\infty - x_\infty^2 - y_\infty^2) - y_\infty}{y_\infty -\tan \left(\pi \gamma \right) (x_1 - x_\infty)}
\end{split}$$

Then, solving for $$x_c$$, $$y_c$$, and $$r$$, splitting up the tangent function to avoid some unneccesary singularities, and simplifying a ton:

$$\begin{split}
x_c &= \frac{ 2 x_1 y_\infty \cos \left(\pi \gamma \right) - \sin \left(\pi \gamma \right) (x_1^2 - x_\infty^2 - y_\infty^2) }{2 \left( y_\infty \cos \left(\pi \gamma \right) -(x_1 - x_\infty) \sin \left(\pi \gamma \right) \right)}\\
y_c &= \cos \left(\pi \gamma \right) \frac{ (x_1 - x_\infty)^2 + y_\infty^2 }{2 \left( y_\infty \cos \left(\pi \gamma \right) -(x_1 - x_\infty)\sin \left(\pi \gamma \right) \right)}\\
r &= \frac{x_1^2-2 x_1 x_\infty +x_\infty^2+y_\infty^2}{ 2 \left( y_\infty \cos \left(\pi \gamma \right) - (x_1 - x_\infty)\sin \left(\pi \gamma \right)  \right)}
\end{split}$$

This is useful if we already have $$x_1$$, $$x_\infty$$, and $$y_\infty$$ calculated, which is probably true for most cases where you'd be interested in this formula. But it's a little opaque: it doesn't really give much insight. So...

# We need to go deeper!
Let's see if anything interesting comes out if we try to express it in terms of gamma functions. The derivations here are, as usual, too long to show, but they make use of the gamma function reflection formula, $$\Gamma(z) \Gamma(1-z) \sin(\pi z) = \pi$$, and various identities for products and sums of sines and cosines.

These formulas are surprisingly simple, and share many factors:

$$\begin{split}
x_c &= \left(\cos(\pi\beta) + \cos(\pi\alpha)\cos(\pi\gamma)\right)\frac
  {\Gamma(1-a')\Gamma(1-b')\Gamma(c')\Gamma(a)\Gamma(b)}
  {2\pi^2\Gamma(c)}, \\
y_c &= \sin(\pi\alpha)\cos(\pi\gamma)\frac
  {\Gamma(1-a')\Gamma(1-b')\Gamma(c')\Gamma(a)\Gamma(b)}
  {2\pi^2\Gamma(c)}\\
r &= \cos(\pi\alpha)\frac
  {\Gamma(1-a')\Gamma(1-b')\Gamma(c')\Gamma(a)\Gamma(b)}
  {2\pi^2\Gamma(c)}
\end{split}$$

The gamma function has no zeros, and is infinite at 0 and the negative integers. Given the ranges for the parameters we determined earlier, this circle will be infinite when:

* $$a = 0$$. This implies $$\alpha + \beta + \gamma = 1$$, the Euclidean case mentioned earlier.
* $$a = -1$$. This implies $$\alpha = \beta = \gamma = 1$$, which is the case when the triangle is a hemisphere. 
* $$b = 0$$. This implies $$s(\infty) = \infty$$, per the formula for $$s(\infty)$$ given above.
* $$a' = 1$$. This implies $$s(\infty) = s(1) = \infty$$, per their formulas.
* $$b' = 1$$. This implies $$s(1) = \infty$$, per its formula.
