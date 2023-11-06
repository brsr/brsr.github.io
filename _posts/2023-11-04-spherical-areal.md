---
layout: post
title: "Spherical area coordinates, and a derived triangle center"
tags: geometry vectors

images:
  - name: .
    link: /assets/images/areacenter/area_center_10-90-90.svg 
  - name: .
    link: /assets/images/areacenter/area_center_40-34-129.svg 
  - name: .
    link: /assets/images/areacenter/area_center_56-133-18.svg    
  - name: .
    link: /assets/images/areacenter/area_center_158-60-44.svg 
  - name: .
    link: /assets/images/areacenter/area_center_162-49-52.svg 
  - name: .
    link: /assets/images/areacenter/area_center_171-167-174.svg 
    
imagewidth: 32%
---

[An earlier post]({% post_url 2023-07-29-spherical-trilinear-barycentric %}) explained homogenous coordinates on the sphere, and derived some relationships between spherical triangle centers. A certain geometric property of Euclidean barycentric coordinates fails to carry over into spherical geometry. Specifically, the Euclidean barycentric coordinate $$\beta_i$$ equals the ratio of the area of the small triangle opposite vertex $$i$$ and the area of the large triangle: this remains true for points outside the triangle if signed areas are used. The previous is not true for spherical homogenous coordinates, but an analogous set of coordinates will be defined below.

Russell[^Russell] notes that in spherical geometry, the point where the medians of the triangle intersect is in general not the same as the point that divides a triangle into three smaller triangles of equal area as shown. He gives a formula for the vertex median point: $$ (\csc A : \csc B : \csc C) $$, but not the equal-area center. Later, we use spherical area coordinates to derive that center and some related points.

We'll switch between notation using angles labeled $$A$$, $$B$$, and $$C$$ and vertices labeled with subscripts $$i$$, depending on which is more convenient. The length of the edge opposite is $$a$$, $$b$$, or $$c$$; or $\ell_i$$, correspondingly.

# Spherical area coordinates

Let $$\hat{\mathbf v}_i$$ be the vertices of the triangle and $$\hat{\mathbf v}_P$$ be the projected point. Let $$\Omega_i$$ designate the signed geodesic area of the small triangle opposite of vertex $$i$$, and $$\Omega$$ designate the area of the large triangle. Area coordinates on the sphere can be defined as:

$$
    \alpha_i = \frac{\Omega_i}{\Omega}
$$

These coordinates can be applied to any surface where area is well-defined. On the sphere, the area of a triangle can calculated from the total of the interior angles minus $$\pi$$, although that is an unsigned quantity, and often calculating each angle is numerically unstable. A formula such as this may be used instead:

$$
\tan \left( \frac{\Omega}{2} \right) = \frac{|\mathbf{\hat{v}_1} \cdot
       \mathbf{\hat{v}}_2 \times \mathbf{\hat{v}}_3|}
       {1+\mathbf{\hat{v}}_1\cdot \mathbf{\hat{v}}_2+\mathbf{\hat{v}}_2
       \cdot \mathbf{\hat{v}}_3+\mathbf{\hat{v}}_3\cdot \mathbf{\hat{v}}_1}
$$

For points within the triangle, $$\alpha_i$$ are all positive. Outside, some may be negative. In particular, in the triangle formed by the antipodes of $$\hat{\mathbf v}_i$$, all of $$\alpha_i$$ are negative. Except for in that antipodal triangle, $$\sum \alpha_i = 1$$.

# Inverse
The idea of spherical area coordinates presented above is not new: for example, Praun et al.,[^Praun] Carfora et al.,[^Carfora] and Lei et al.[^Lei] have written about it before. The latter article includes an formula to convert $$\alpha_i$$ back to $$\hat{\mathbf v}_P$$, given in terms of a matrix that must be inverted. However, in a math.StackExchange post,[^Hui] Achille Hui gives a close-formula for this inverse. I repeat it here in preparation for whenever StackExchange finally bites the dust. There are some quantities that depend only on the larger triangle, not the point to be projected:

$$\begin{split}        
        k_i & = \mathbf v_{i-1} \cdot \mathbf v_{i+1} = \cos \ell_i\\
        h_i & = \frac{k_{i-1} + k_{i+1}}{1 + k_i}\\
        \tau &= \tan\left(\frac{\Omega}{2}\right) \\
\end{split}$$

With that, consider $$\alpha_i$$ as givens, and proceed as follows. While lengthy, this set of formulas consists only of scalar and vector arithmetical operations and the $$\tan$$ function.

$$\begin{split}        
\Omega_i &= \alpha_i \Omega \\    
\tau_i &= \tan\left(\frac{\Omega_i}{2}\right) \\
t_i &= \frac{\tau_i}{\tau} \\
g_i &= \frac{t_i}{(1+h_i) + (1-h_i) t_i} = \frac{\tau_i}{(1+h_i) \tau + (1-h_i) \tau_i} \\
f_i &= \frac{g_i}{1 - g_1 - g_2 - g_3} \\
\hat{\mathbf v}_P &= \sum^3_{i=1} f_i \hat{\mathbf v}_i = \frac{\sum^3_{i=1} g_i \hat{\mathbf v}_i }{ 1 - \sum^3_{i=1} g_i }
\end{split}$$

Note that the final formula is a linear combination of vectors, as appears in Euclidean barycentric coordinates.

# Small triangles

It is easy to demonstrate that for small triangles, and for points within or near the triangle, spherical areal coordinates are approximately barycentric coordinates. Remember that $$\sum \alpha_i = 1$$ except within the antipodal triangle, which is far enough away that we're not concerned with it.

$$\begin{split}        
k_i &= \cos \ell_i \approx 1 \\
h_i &= \frac{k_{i-1} + k_{i+1}}{1 + k_i} \approx 1 \\
\tau &= \tan\left(\frac{\Omega}{2}\right) \approx \frac{\Omega}{2}\\
\tau_i &= \tan\left(\frac{\Omega_i}{2}\right) \approx \frac{\Omega_i}{2} = \alpha_i \frac{\Omega}{2}\\
t_i &= \frac{\tau_i}{\tau} \approx \alpha_i\\
g_i &= \frac{t_i}{(1+h_i) + (1-h_i) t_i} \approx \frac{t_i}{2} = \frac{\alpha_i}{2}\\
f_i &= \frac{g_i}{1 - g_1 - g_2 - g_3} \approx \frac{\alpha_i/2}{1 - \alpha_1/2 - \alpha_2/2 - \alpha_3/2} = \frac{\alpha_i}{2 - \alpha_1 - \alpha_2 - \alpha_3} = \alpha_i \\
\mathbf v_P &= \sum^3_{i=1} f_i \mathbf v_i \approx \sum^3_{i=1} \alpha_i \mathbf v_i
\end{split}$$

# Specific points

## Center

{% include image-gallery.html listing = page.images width = page.imagewidth %}

Several examples of the equal-area center of a triangle are shown above: these (and more random triangles that I'm not showing because they basically look like these or they're close to being equilateral) have been numerically verified to divide the triangle into three triangles of equal area.  Define $$S = \frac{A+B+C}{2}$$, and $$\Omega = 2S - \pi$$, which is the spherical area of the triangle. In homogenous coordinates, the equal-area triangle center is 

$$
\csc \left( A - \frac{\Omega}{3}\right) : \csc \left( B - \frac{\Omega}{3}\right) : \csc \left( C - \frac{\Omega}{3}\right) 
$$

This is probably also the equal-area center for hyperbolic geometry, but I haven't verified that and there might be a sign change somewhere.

This can be derived from the inverse formula using $$\alpha_1 = \alpha_2 = \alpha_3 = 1/3$$ and converting all the lengths that appear to angles. Or in long-form:

### Derivation

We start with a derivation of the barycentric coordiantes for this center. The derivation is simplified in two ways. One is that, since barycentric coordinates are homogenous, we can ignore the denominator of $$f_i$$, which is invariant under permutation of indices, and concentrate on $$g_i$$ instead. The other is that we can just focus on $$g_1$$ and get $$g_2$$ and $$g_3$$ by symmetry once we're finished.

Note that $$\tau = \tan\left(\frac{\Omega}{2}\right)$$ and, given the values for $$\alpha_i$$ above, $$\tau_i = \tan\left(\frac{\Omega}{6}\right)$$ regardless of $$i$$. We'll actually need $$t_1^{-1}$$, which, with the appropriate <strike>computer algebra system</strike>trig identities, simplifies as:

$$
t_1^{-1} = \frac{\tau}{\tau_i} = \frac{\tan\left(\frac{\Omega}{2}\right)}{\tan\left(\frac{\Omega}{6}\right)} 
= \frac{2}{2 \cos\left(\frac{\Omega}{3}\right) - 1} + 1
$$

It winds up being more convenient for the derivation to rearrange $$g_1$$ slightly, and then substitute $$t_1$$.

$$\begin{split}  
g_1 &= \frac{t_1}{(1+h_1) + (1-h_1) t_1} \\
& =  \left(\frac{1+h_1}{t_1} + 1-h_1 \right)^{-1} \\
& =  \left(\left(1+h_1\right)\left(\frac{2}{2 \cos\left(\frac{\Omega}{3}\right) - 1} + 1\right) + 1-h_1 \right)^{-1} \\
& =  \left(2\frac{1+h_1}{2 \cos\left(\frac{\Omega}{3}\right) - 1} + 2 \right)^{-1}
\end{split}$$

At this point we examine the quantity $$1+h_1$$:

$$
1 + h_1 = \frac{k_3 + k_2}{1 + k_1} + 1 = \frac{1 + k_1 + k_2 + k_3}{1 + k_1} 
= \frac{1 + \cos a + \cos b + \cos c}{1 + \cos a} 
$$

For reference, the [spherical law of cosines for angles](https://en.wikipedia.org/wiki/Spherical_law_of_cosines) can be rearranged to isolate the length:

$$
\cos c = \frac{\cos A \cos B + \cos C}{\sin A \sin B}
$$

and correspondingly for lengths $$a$$ and $$b$$. Substitute the denominator first to get a feel for the problem:

$$\begin{split} 
1 + \cos a &= 1 + \frac{\cos B \cos C + \cos A}{\sin B \sin C} \\
&= \frac{\sin B \sin C + \cos B \cos C + \cos A}{\sin B \sin C} \\
&= 2 \frac{\cos \frac{A-B+C}{2} \cos \frac{A+B-C}{2}}{\sin B \sin C} \\
&= 2 \frac{\cos \left(S - B \right) \cos \left(S - C \right)}{\sin B \sin C}
\end{split}$$

where $$S$$ was defined earlier. Now for the numerator. Ideally, we'll want some factors in common so we can simplify the rational expression. Also note that the original expression was invariant under cyclic permutation of the triangle, so whatever we come up with at the end should be too.

$$\begin{split}
&1 + \cos a + \cos b + \cos c \\
&= 1 + \frac{\cos B \cos C + \cos A}{\sin B \sin C} + \frac{\cos C \cos A + \cos B}{\sin C \sin A} + \frac{\cos A \cos B + \cos C}{\sin A \sin B} \\
&= \frac{\sin A \sin B \sin C + \sin A \cos B \cos C + \cos A \sin A + \cos A \sin B \cos C + \cos B \sin B + \cos A \cos B \sin C + \cos C \sin C}{\sin A \sin B \sin C} \\
&= \frac{4 \sin(A/2 + B/2 + C/2) \cos(A/2 - B/2 - C/2) \cos(A/2 + B/2 - C/2) \cos(A/2 - B/2 + C/2)}{\sin A \sin B \sin C} \\
&= 4 \frac{\sin(S) \cos(S-A) \cos(S-B) \cos(S-C)}{\sin A \sin B \sin C}
\end{split}$$

Therefore,

$$\begin{split}
1 + h_1 &= \frac{1 + \cos a + \cos b + \cos c}{1 + \cos a} \\
&= 4 \frac{\sin(S) \cos(S-A) \cos(S-B) \cos(S-C)}{\sin A \sin B \sin C} \cdot \frac{\sin B \sin C}{2 \cos \left(S - B \right) \cos \left(S - C \right)} \\
&= 2 \frac{\sin(S) \cos\left(S-A\right)}{\sin A} \\
&= 2 \frac{\cos\frac{\Omega}{2} \sin\left(A-\frac{\Omega}{2}\right)}{\sin A}
\end{split}$$

In preparation for the next step, the last equality substitutes $$S = \frac{\Omega+\pi}{2}$$. Now plug $$1 + h_1$$ into the expression of $$g_1$$ from earlier.

$$\begin{split}
g_1 &= \left(2\frac{1+h_1}{2 \cos\left(\frac{\Omega}{3}\right) - 1} + 2 \right)^{-1} \\
&= \left(4\frac{\cos\frac{\Omega}{2} \sin\left(A-\frac{\Omega}{2}\right)}{\sin A \left(2 \cos\left(\frac{\Omega}{3}\right) - 1 \right)} + 2 \right)^{-1} \\
&= \frac{\sin A}{4\frac{\cos\frac{\Omega}{2} \sin\left(A-\frac{\Omega}{2}\right)}{\left(2 \cos\left(\frac{\Omega}{3}\right) - 1 \right)} + 2\sin A} \\
&= \frac{\sin A}{4 \cos\frac{\Omega}{6} \sin\left(A-\frac{\Omega}{2}\right) + 2\sin A} \\
&= \frac{\sin A}{2 \left(2 \cos\left(\frac{\Omega}{3}\right) + 1\right) \sin\left(A - \frac{\Omega}{3}\right)}
\end{split}$$

Finally, note that the term $$2 \left(2 \cos\left(\frac{\Omega}{3}\right) + 1\right)$$ is invariant under cyclic permutation of the triangle. Since we are working with homogenous coordinates, that term can be ignored. The barycentric coordinates of the area center are:

$$
\frac{\sin A}{\sin\left(A - \frac{\Omega}{3}\right)} : \frac{\sin B}{\sin\left(B - \frac{\Omega}{3}\right)} : \frac{\sin C}{\sin\left(C - \frac{\Omega}{3}\right)}
$$

To convert to trimetric coordinates, divide the first term by $$\sin a$$, the second by $$\sin b$$, and the third by $$\sin c$$. By the spherical law of sines, 

$$
\frac{\sin A}{\sin a} = \frac{\sin B}{\sin b} = \frac{\sin C}{\sin c}
$$

so that's another constant multiplier we can drop. The trimetric coordinates are therefore

$$
\frac{1}{\sin\left(A - \frac{\Omega}{3}\right)} : \frac{1}{\sin\left(B - \frac{\Omega}{3}\right)} : \frac{1}{\sin\left(C - \frac{\Omega}{3}\right)}
$$

which can be more succinctly expressed using cosecant, as written earlier.

### Sanity check
This derivation is somewhat opaque, and doesn't really give any geometric insight. Let's make a sanity check: we would expect the equal-area center to always be inside the triangle. Thus, all the components of the coordinates should be positive, possibly zero for degenerate triangles. Here's a proof of that.  

First, assume that the triangle is proper, such that $$0 \le A \le \pi$$ and similarly for $$B$$ and $$C$$. 

Note that the triangle is contained within the spherical lune created by extending the edges opposite $$B$$ and $$C$$ to their other point of intersection. The angle of intersection at either point is $$A$$, and the area of the lune is $$2A$$. Therefore, $$\Omega \le 2A$$. Substitute the inequality into the first coordinate: $$A - \frac{\Omega}{3} \ge A - \frac{2}{3}A = \frac{1}{3}A \ge 0$$, where the rightmost inequality is the lower bound of the initial assumption.

On the other end, 

$$
A - \frac{\Omega}{3} = A - \frac{A + B + C - \pi}{3} = \frac{2}{3}A - \frac{B + C - \pi}{3} \le \frac{2}{3}\pi - \frac{B + C - \pi}{3} 
= \pi - \frac{B + C}{3} \le \pi
$$

where the final inequality applies the lower bound of the initial assumption, i.e. that B and C are nonnegative.

Therefore, $$0 \le A - \frac{\Omega}{3} \le \pi$$. $$\csc$$ is positive over that range of inputs. The same argument applies to the second and third coordinates. QED.

## Edge midpoints

The point on the edge opposite angle $$C$$ such that the edge from that point to $$c$$ bisects the triangle into two equal-area parts is:

$$
\csc \left( A - \frac{\Omega}{4}\right) : \csc \left( B - \frac{\Omega}{4}\right) : 0 
$$

Expressions for the other edges are analogous. This can be derived using area coordinates too, although i'm sure there's a clever geometric proof. Let $$\alpha_1 = \alpha_2 = 1/2$$ and $$\alpha_3 = 0$$. $$1+h_1$$ is the same as before:

$$\begin{split}
1 + h_1 = 2 \frac{\cos\frac{\Omega}{2} \sin\left(A-\frac{\Omega}{2}\right)}{\sin A}
\end{split}$$

$$
t_1^{-1} = \frac{\tau}{\tau_1} = \frac{\tan\left(\frac{\Omega}{2}\right)}{\tan\left(\frac{\Omega}{4}\right)} 
= \sec\frac{\Omega}{2} + 1
$$

$$\begin{split}  
g_1 &= \left(\frac{1+h_1}{t_1} + 1-h_1 \right)^{-1} \\
&= \left(\left(1+h_1\right)\left( \sec\frac{\Omega}{2} + 1\right) + 1-h_1 \right)^{-1} \\
&= \left(\frac{1+h_1}{\cos \frac{\Omega}{2} } + 2 \right)^{-1} \\
&= \left(2\frac{\cos\frac{\Omega}{2} \sin\left(A-\frac{\Omega}{2}\right)}{\sin A \cos \frac{\Omega}{2} } + 2 \right)^{-1} \\
&= \left(2\frac{\sin\left(A-\frac{\Omega}{2}\right)}{\sin A} + 2 \right)^{-1} \\
&= \frac{\sin A}{2 \sin\left(A-\frac{\Omega}{2}\right) + 2\sin A} \\
&= \frac{\sin A}{4 \cos \frac{\Omega}{2} \sin\left(A-\frac{\Omega}{4}\right)}
\end{split}$$

As before, we can ignore the factor $$4 \cos \frac{\Omega}{2}$$. The second coordinate can be found by analogy, and the third can be easily shown to be 0, leading to the expression given above.

Note that these are not the points that can be used to divide a spherical triangle into six smaller triangles, but that can be accomplished by dividing a spherical triangle into three triangles using the equal-area center, and then dividing those triangles in two using the midpoint opposite the equal-area center.

*A Python script to create the figures in this post is located [here](https://github.com/brsr/mapproj/blob/master/bin/equalareapoint.py).*

[^Praun]: Praun, Emil; Hoppe, Hugues (2003), "Spherical parametrization and remeshing", ACM Transactions on Graphics, 22 (3): 340–349, doi:10.1145/882262.882274, <http://hhoppe.com/sphereparam.pdf>
[^Carfora]: Carfora, Maria Francesca (2007), "Interpolation on spherical geodesic grids: A comparative study", Journal of Computational and Applied Mathematics, 210 (1–2): 99–105, doi:10.1016/j.cam.2006.10.068, <https://www.sciencedirect.com/science/article/pii/S0377042706006522>
[^Lei]: Lei, Kin; Qi, Dongxu; Tian, Xiaolin (2020), "A new coordinate system for constructing spherical grid systems", Applied Sciences, 10 (2): 655, doi:10.3390/app10020655, <https://www.mdpi.com/2076-3417/10/2/655/htm>
[^Russell]: Russell, Robert A. (2016), "Non-Euclidean Triangle Centers", arXiv preprint, arXiv:1608.08190,  <https://arxiv.org/abs/1608.08190>
[^Hui]: Hui, Achille (2019), <https://math.stackexchange.com/questions/1151428/point-within-a-spherical-triangle-given-areas/1244218>
