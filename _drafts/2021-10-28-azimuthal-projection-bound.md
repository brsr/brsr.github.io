---
layout: post
title: "A bound on distortion in azimuthal projections"
tags: cartography
---

[general perspective projections]({% post_url 2021-06-12-perspective-vector %})

$$\begin{split}
s &= hk \\
\sin \frac{\omega}{2} &= \frac{|h - k|}{h+k}
= \begin{cases}
\frac{h/k - 1}{h/k+1}\, \text{ where } h > k,\\
\frac{1 - k/h}{1+k/h}\, \text{ otherwise }
\end{cases}
\end{split}$$

For simplicitly, focus on $$\frac{h}{k} = m$$ first and return to $$\omega$$ later.

where $$m = \frac{1+\sin \frac{\omega_{\max}}{2}}{1-\sin \frac{\omega_{\max}}{2}} \ge 1$$.
When $$\omega_{\max} = 0$$, $$m=1$$: increases monotonically to $$\omega_{\max} = \pi$$, where $$m$$ is infinite

[Grönwall's lemma](https://en.wikipedia.org/wiki/Gr%C3%B6nwall%27s_inequality): does this
apply the way I think it does? do i need to make $$s$$ and $$m$$ functions of $$r$$?

# Azimuthal #
$$0 \le r \le r_{\max} \le \pi$$. ($$\le \frac{\pi}{2}$$?) $$\rho(r) = 0 $$ iff $$r = 0$$. $$\frac{\partial \rho}{\partial r} \ge 0$$ and $$\frac{\partial \rho}{\partial r} > 0$$ at $$r=0$$.

$$\begin{split}
h &= \frac{\partial \rho}{\partial r} \\
k &= \frac{\rho}{\sin r}
\end{split}$$

$$\begin{split}
s &= \frac{\rho}{\sin r} \frac{\partial \rho}{\partial r} \\
m &= \frac{\sin r}{\rho} \frac{\partial \rho}{\partial r}
\end{split}$$

Treat those as differential equations for constant $$s$$ and $$m$$: their solutions are

$$\begin{split}
s &= \frac{\rho^2}{4 \sin^2 \frac{r}{2}}\\
\exp(m) &= k\frac{\rho}{2 \tan \frac{r}{2}}
\end{split}$$

where $$k = \frac{\partial \rho}{\partial r}$$ at $$r=0$$.


Note that $$\rho = 2 \sin \frac{r}{2}$$ for the Lambert azimuthal equal-area projection,
and $$\rho = 2 \tan \frac{r}{2}$$ for the stereographic projection.

# Rectangular #

wlog, assume $$\phi, \lambda = 0, 0$$ iff $$x, y = 0, 0$$
$$\begin{split}
h &= \frac{\partial y}{\partial \phi} \\
k &= \frac{1}{\cos \phi} \frac{\partial x}{\partial \lambda}
\end{split}$$

then

$$\begin{split}
s &= \frac{1}{\cos \phi}\frac{\partial y}{\partial \phi} \\
m = \frac{h}{k} &= \cos \phi \frac{\partial y}{\partial \phi}
\end{split}$$

same logic as before:

$$\begin{split}
s &= \frac{y}{\sin \phi} \\
m &= \frac{y}{\operatorname{artanh} \sin \phi}
\end{split}$$

all these have $$x = \lambda$$ and $$k = \frac{1}{\cos \phi}$$
* mercator: $$y = \operatorname{artanh} \sin \phi$$, $$h = k$$
* equal-area: $$y=\sin \phi$$, $$h = \cos \phi = \frac{1}{k}$$
* miller: $$y = 1.25 \operatorname{artanh} \sin \left(0.8 \phi \right)$$, $$h=\frac{1}{\cos (0.8\phi)}$$
* equidistant: $$y = \phi$$, $$h = 1$$
