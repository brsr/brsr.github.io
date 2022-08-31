---
layout: post
title: "Scaling the Schwarz triangle function"
tags: complex_functions geometry

---
Triangles in spherical and hyperbolic geometry have a property that Euclidean geometry does not: their angles determine their size. The area of a spherical triangle is equal to the angular excess, the sum of its interior angles minus $$\pi$$. The area of a hyperbolic triangle is equal to the angular deficit, $$\pi$$ minus the sum of its interior angles. The spherical or hyperbolic laws of sines and cosines behave similarly: they determine specific edge lengths.

The Schwarz triangle function, mentioned in a couple prior posts -- [1]({% post_url 2022-03-03-conformal-polyhedral %}), [2]({% post_url 2022-08-27-schwarz-triangle-function-edge %}) -- is useful for conformal mapping of triangles in various geometries. However, it does not necessarily map to a triangle of the right size. In particular, triangles with an angle sum very close to $$\pi$$ should be very small in spherical or hyperbolic geometry. The result of the Schwarz triangle function often must be scaled to obtain a properly-sized triangle for the geometry.

# Metrics
Since we're talking about size, we need to define what we mean by size, and we do that by the metric of whatever geometric space we're operating in. The standard model for spherical geometry is the [Riemann sphere](https://en.wikipedia.org/wiki/Riemann_sphere), which has line element:

$$ ds^2 = \frac{4}{(1+x^2+y^2)^2} (dx^2 + dy^2) $$

There are a few different ways to model hyperbolic geometry in the complex plane. Here we choose the [Poincaré disk](https://en.wikipedia.org/wiki/Poincar%C3%A9_disk_model), rather than the upper half-plane or some other model, since its line element is very similar to that of the Riemann sphere:

$$ ds^2 = \frac{4}{(1-(x^2+y^2))^2} (dx^2 + dy^2) $$

The factor of 4 is so that the curvature of the surface is $$1$$ for the sphere and $$-1$$ for hyperbolic space.

We'll look at the distance from $$s(0)=0$$ to $$s(1)$$, where $$s(z)$$ is the Schwarz triangle function (not the differential in the line element). $$s(1)$$ lies on the real line, so we can just ignore $$y$$. Integrating the line element gives the distance function:

$$ \int_0^u ds = \int_0^u \frac{2}{1 \pm x^2} dx $$

which is $$2 \arctan u $$ for the Riemann sphere and $$2 \operatorname{artanh} u $$ for the Poincaré disk.

For later, we note these equations:

$$\begin{split}
\cos(2 \arctan u) &= \frac{1 - u^2}{1+u^2} \\
\cosh(2 \operatorname{artanh} u) &= \frac{1 + u^2}{1-u^2}
\end{split}$$

# Law of cosines
Consider the triangle with interior angles (in clockwise order) $$\pi \alpha$$, $$\pi \beta$$, and $$\pi \gamma$$, and edge length $$\ell$$ opposite of $$\beta$$.
The [spherical law of cosines](https://en.wikipedia.org/wiki/Spherical_law_of_cosines) gives

$$
\cos \beta = - \cos \alpha \cos \gamma + \sin \alpha \sin \gamma \cos \ell
$$

while the [hyperbolic law of cosines](https://en.wikipedia.org/wiki/Hyperbolic_law_of_cosines) is the same thing with $$\cosh \ell$$ instead of $$\cos \ell$$:

$$
\cos \beta = - \cos \alpha \cos \gamma + \sin \alpha \sin \gamma \cosh \ell
$$

Solving for $$\cos \ell$$ (or $$\cosh \ell$$) gives 

$$
\frac{\cos \beta + \cos \alpha \cos \gamma}{\sin \alpha \sin \gamma}
$$

$$\ell$$ is the distance in the geometry, rather than the untransformed distance in the plane: these are related by the distance functions we calculated earlier. The forms for the composition of $$\cos$$ (or $$\cosh$$) and the distance function were given earlier. Combining everything and solving for $$u^2$$ gives a remarkable result: the formulas for spherical and hyperbolic geometry are the same, except for the sign of the formula. The changeover point is where the angles indicate a Euclidean triangle, in which case $$u^2$$ is zero. So combine the formulas using the absolute value, and take the root:

$$
u = \sqrt{\left| \frac{\cos(\pi (\alpha + \gamma)) + \cos(\pi \beta)}{\cos(\pi (\alpha - \gamma)) + \cos(\pi \beta) } \right|}
$$

Or, in terms of the Schwarz parameters mentioned in the first two articles:

$$
u = \sqrt{\left| \frac{\sin(\pi a) \sin(\pi b)}{\sin(\pi a') \sin(\pi b')} \right|}
$$

# Scaling 

So, an appropriately scaled Schwarz triangle function is:

$$
z' = s(z) \frac{u}{s(1)}
$$

The quantity $$u/s(1)$$ can be expressed in terms of gamma functions (and a square root) by using the reflection formula to eliminate the $$\sin$$s, but unless you really need it in terms of gamma functions for some reason there's no advantage to it.

For illustration, let's look at the case of an equilateral triangle, where $$\alpha = \beta = \gamma$$. $$u$$ simplifies as so:

$$u = \sqrt{\left| 2 \cos(\pi \alpha) - 1 \right|}$$

Let's plot $$u$$, $$s(1)$$, and $$u/s(1)$$:

<center><img src="/assets/images/Schwarz_triangle_function_scale.svg" alt="Graph of u, s(1), and u/s(1)" /></center>

<details>
  <summary><i>Click here for the Python code to generate the plot above.</i></summary>
<pre>
import numpy as np
from scipy.special import gamma
import matplotlib.pyplot as plt

alpha = np.linspace(0, 1, 241)
beta = alpha
gam = alpha
a = (1 - alpha - beta - gam)/2
b = (1 - alpha + beta - gam)/2
c = 1 - alpha
ap = (1 + alpha - beta - gam)/2  # a - c + 1
bp = (1 + alpha + beta - gam)/2  # b - c + 1
cp = 1 + alpha  # 2-c

s1 = gamma(1 - ap) * gamma(cp) * gamma(1 - bp)/(gamma(1 - a) * gamma(c) * gamma(1 - b))
s1[-1] = 2
u = np.sqrt(abs(np.sin(np.pi*a) * np.sin(np.pi*b) /
            (np.sin(np.pi*ap) * np.sin(np.pi*bp))))
u[-1] = np.sqrt(3)

fig, ax = plt.subplots()
ax.plot(alpha, s1, label='s(1)')
ax.plot(alpha, u, label='u')
ax.plot(alpha, u/s1, label='u / s(1)')
ax.legend()

fig.savefig('Schwarz_triangle_function_scale.svg', bbox_inches='tight')
</pre>
</details>
