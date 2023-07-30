---
layout: post
title: "Homogeneous coordinates on the sphere with vectors"
tags: geometry vectors
---
[Trilinear coordinates](https://en.wikipedia.org/wiki/Trilinear_coordinates), a type of [homogeneous coordinate](https://en.wikipedia.org/wiki/Homogeneous_coordinates), are very useful for the study of Euclidean triangles, especially that of triangle centers. They can be specified as a triplet of coordinates, $$(x: y: z)$$. Colons are used instead of commas to emphasize that these quantities are ratios: $$(x: y: z)$$ is the same point as $$(kx: ky: kz)$$ for any positive constant $$k$$. The values x, y, and z are the perpendicular directed distance between the specified point and each side of a given triangle.

homogeneous coordinates can also be defined for the sphere (also hyperbolic space, but we're not interested in that right now). The coordinates are in proportion to the sines of the distances instead: $$(\sin x: \sin y: \sin z)$$. [Spherical triangle centers can be defined like in the Euclidean case.](https://arxiv.org/abs/1608.08190)

In this post, I derive how to convert these coordinates to and from vectors on the unit sphere. Let the vertices of this triangle be $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and $$\mathbf{\hat{c}}$$, arranged in counterclockwise order. As is standard in the study of triangle centers, let $$A$$, $$B$$, $$C$$ be the interior angles at each vertex, and $$a$$, $$b$$, $$c$$ be the length of the edge opposite each vertex. Note that $$\| \mathbf{\hat{a}} \times \mathbf{\hat{b}} \| = \sin c$$ and $$\mathbf{\hat{a}} \cdot \mathbf{\hat{b}} = \cos c$$, and so on for the other edges. Also let $$\mathbf{\hat{p}}$$ be some other point on the sphere, and $$(f, g, h) = \mathbf{h}$$ be the homogeneous coordinates of that point.

# Trilinear

The great circle center corresponding to the edge between $$\mathbf{\hat{a}}$$ and $$\mathbf{\hat{b}}$$, also called (at least in the context of spherical triangles) the pole of $$\mathbf{\hat{c}}$$, is 

$$
\mathbf{\hat{c}}' =
\frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}
}{\| \mathbf{\hat{a}} \times \mathbf{\hat{b}} \|}
=
\frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}
}{\sin c}
$$

The distance between $$\mathbf{\hat{p}}$$ and $$\mathbf{\hat{c}}'$$ is therefore $$\cos z' = \mathbf{\hat{p}} \cdot \mathbf{\hat{c}}'$$. Being a great circle, $$\mathbf{\hat{c}}'$$ has a distance of $$\pi/2$$ to $$\mathbf{\hat{a}}$$, $$\mathbf{\hat{b}}$$, and any other point on that great circle. Therefore the perpendicular distance between $$\mathbf{\hat{p}}$$ and that great circle is $$ z = \pi/2 - z'$$, and it also follows that 

$$
\sin z = \mathbf{\hat{p}} \cdot \mathbf{\hat{c}}'
$$
 
where $$\sin z$$ is one of the coordinates. The other two can be found by cyclic permutation of the vertices. All these may be combined in matrix form as 

$$
\begin{bmatrix}
\mathbf{\hat{a}}' \\ \mathbf{\hat{b}}' \\ \mathbf{\hat{c}}'
\end{bmatrix} \mathbf{\hat{p}} = \begin{bmatrix}
\sin x \\
\sin y \\
\sin z
\end{bmatrix} 
= \mathbf{h}
$$

which can be easily inverted:

$$
\begin{bmatrix}
\mathbf{\hat{a}} \sin a
& \mathbf{\hat{b}} \sin b  
& \mathbf{\hat{c}} \sin c 
\end{bmatrix} \mathbf{h} = \mathbf{p}
$$

Note that we don't really need to calculate $$x, y, z$$, just their sines. Also note that some factors are dropped from the inverted formula, or I should rather say they are absorbed into $$\mathbf{h}$$ or $$\mathbf{p}$$. The last formula determines $$\mathbf{p}$$, not $$\mathbf{\hat{p}}$$, which can be calculated as $$\mathbf{\hat{p}} = \frac{\mathbf{p}}{\|\mathbf{p}\|}$$

# Barycentric coordinates
If $$(f, g, h)$$ are trilinear coordinates, the corresponding barycentric coordinates are $$\mathbf{\beta} = (f a, g b, h c)$$. If $$(f, g, h)$$ are homogeneous coordinates on the sphere, the corresponding spherical barycentric coordinates are $$\mathbf{\beta} = (f \sin a, g \sin b, h \sin c)$$. Notably, the sines of the edges cancel out the factors in the matrix equations above. The spherical barycentric coordinates $$\mathbf{\beta}$$ relate to the point $$\mathbf{p}$$ as:

$$
\begin{bmatrix}
\mathbf{\hat{b}} \times \mathbf{\hat{c}} \\
\mathbf{\hat{c}} \times \mathbf{\hat{a}} \\
\mathbf{\hat{a}} \times \mathbf{\hat{b}} 
\end{bmatrix} \mathbf{\hat{p}}
= \mathbf{\beta}
$$

and 

$$
\begin{bmatrix}
  \mathbf{\hat{a}}
& \mathbf{\hat{b}}  
& \mathbf{\hat{c}} 
\end{bmatrix} \mathbf{\beta} = \mathbf{p}
$$

These are the same equations to relate a point in Euclidean space with its barycentric coordinates, although the spherical barycentric coordinates don't share all the geometric properties of Euclidean barycentric coordinates.

[An earlier post]({% post_url 2021-06-12-perspective-vector %}) discussed how to express the gnomonic projection using barycentric coordinates. The gnomonic projection can be thought of as calculating barycentric coordinates on the sphere, then using them as barycentric coordinates in the plane, or vice versa.

# Polar triangle 
A bit of a detour before we get to triangle centers. Let 
$$\mathbf{\hat{a}} = \frac{\mathbf{\hat{e}} \times \mathbf{\hat{f}}}{\ldots}$$, 
$$\mathbf{\hat{b}} = \frac{\mathbf{\hat{f}} \times \mathbf{\hat{d}}}{\ldots}$$, and 
$$\mathbf{\hat{c}} = \frac{\mathbf{\hat{d}} \times \mathbf{\hat{e}}}{\ldots}$$, so that abc is the polar triangle of def. def is also the polar triangle of abc:

$$\frac{\mathbf{\hat{a}} \times \mathbf{\hat{b}}}{\ldots} 
= \frac{(\mathbf{\hat{e}} \times \mathbf{\hat{f}}) \times (\mathbf{\hat{f}} \times \mathbf{\hat{d}})}{\ldots} 
= \frac{\begin{bmatrix}\mathbf{\hat{e}} & \mathbf{\hat{f}} & \mathbf{\hat{d}}\end{bmatrix} \mathbf{\hat{f}}}{\ldots}
= \frac{\mathbf{\hat{f}}}{\ldots}
= \mathbf{\hat{f}} $$

By the properties of the polar triangle, 

$$
A = \pi - d,
B = \pi - e,
C = \pi - f,
a = \pi - D,
b = \pi - E,
c = \pi - F,
$$

and note that $$\sin (\pi - x) = \sin x$$ and $$\cos (\pi - x) = -\cos x$$.

Note that by the spherical law of sines, 

$$\frac{\sin A}{\sin a} = \frac{\sin B}{\sin b} = \frac{\sin C}{\sin c} 
= \frac{\begin{bmatrix}\mathbf{\hat{e}} & \mathbf{\hat{f}} & \mathbf{\hat{d}}\end{bmatrix}}{\sin a\sin b\sin c}.$$

If triangle abc is the polar triangle of def, the transformations can be expressed as:

$$
\begin{bmatrix}
\mathbf{\hat{d}} \\ \mathbf{\hat{e}} \\ \mathbf{\hat{f}}
\end{bmatrix} \mathbf{\hat{p}} 
= \mathbf{h}
$$

and

$$
\begin{bmatrix}
\mathbf{\hat{e}} \times \mathbf{\hat{f}} &
\mathbf{\hat{f}} \times \mathbf{\hat{d}} &
\mathbf{\hat{d}} \times \mathbf{\hat{e}} 
\end{bmatrix} \mathbf{h} = \mathbf{p}.
$$

We can convert trilinear coordinates on a triangle abc to trilinear coordinates on its polar triangle through a composition of transformations:
$$
\begin{bmatrix}
\mathbf{\hat{a}} \\ \mathbf{\hat{b}} \\ \mathbf{\hat{c}}
\end{bmatrix}
\begin{bmatrix}
\mathbf{\hat{a}} \sin a
& \mathbf{\hat{b}} \sin b  
& \mathbf{\hat{c}} \sin c 
\end{bmatrix} 
\mathbf{h}
= \mathbf{h}'
$$

Writing out the inner matrix explicitly,
$$
\begin{bmatrix}
\mathbf{\hat{a}} \\ \mathbf{\hat{b}} \\ \mathbf{\hat{c}}
\end{bmatrix}
\begin{bmatrix}
\mathbf{\hat{a}} \sin a
& \mathbf{\hat{b}} \sin b  
& \mathbf{\hat{c}} \sin c 
\end{bmatrix} 
=
\begin{bmatrix}
\mathbf{\hat{a}} \cdot \mathbf{\hat{a}} \sin a & \mathbf{\hat{a}} \cdot \mathbf{\hat{b}} \sin b & \mathbf{\hat{a}} \cdot \mathbf{\hat{c}} \sin c \\
\mathbf{\hat{b}} \cdot \mathbf{\hat{a}} \sin a & \mathbf{\hat{b}} \cdot \mathbf{\hat{b}} \sin b & \mathbf{\hat{b}} \cdot \mathbf{\hat{c}} \sin c \\ 
\mathbf{\hat{c}} \cdot \mathbf{\hat{a}} \sin a & \mathbf{\hat{c}} \cdot \mathbf{\hat{b}} \sin b & \mathbf{\hat{c}} \cdot \mathbf{\hat{c}} \sin c 
\end{bmatrix} 
=
\begin{bmatrix}
\sin a & \cos c \sin b & \cos b \sin c \\
\cos c \sin a & \sin b & \cos a \sin c \\ 
\cos b \sin a & \cos a \sin b & \sin c 
\end{bmatrix} 
$$

The inverse matrix is the same with the distances replaced by the corresponding angles ($$A$$ for $$a$$, etc.)

# Triangle centers 

[An earlier post described some triangle centers in terms of vectors, derived in terms of their geometric properties.]({% post_url 2021-05-02-spherical-triangle-centers %}) Here we illustrate that the homogenous coordinates in the article i need to reference here correspond to the same vector formulas. For simplicity, the denominators in these formulas are omitted.

## Incenter

$$\mathbf{h} = (1: 1: 1)$$

$$
\mathbf{p} = \mathbf{\hat{a}} \sin a + \mathbf{\hat{b}} \sin b + \mathbf{\hat{c}} \sin c 
= \mathbf{\hat{a}} \| \mathbf{\hat{b}} \times \mathbf{\hat{c}} \|
+ \mathbf{\hat{b}} \| \mathbf{\hat{c}} \times \mathbf{\hat{a}} \|
+ \mathbf{\hat{c}} \| \mathbf{\hat{a}} \times \mathbf{\hat{b}} \| 
$$

as given in the earlier post.

## Vertex-Median point (Centroid)

$$\mathbf{h} = (\csc A: \csc B: \csc C)$$

$$
\mathbf{p} = \mathbf{\hat{a}} \frac{\sin a}{\sin A} 
+ \mathbf{\hat{b}} \frac{\sin b}{\sin B} 
+ \mathbf{\hat{c}} \frac{\sin c}{\sin C}
$$

By the spherical law of sines, $$\frac{\sin a}{\sin A} = \frac{\sin b}{\sin B} = \frac{\sin c}{\sin C}$$, so we can factor out those terms. Somewhat abusing notation by absorbing the constant into $$\mathbf{p}$$, we find that

$$
\mathbf{p} = \mathbf{\hat{a}} + \mathbf{\hat{b}} + \mathbf{\hat{c}} 
$$

as given in the earlier post.

## Circumcenter
$$\mathbf{h} = (\sin(S - A) : \sin(S - B) : \sin(S - C))$$

where $$S = (A + B + C)/2$$

$$
\mathbf{p} = \mathbf{\hat{a}} \sin a \sin(S - A) + \mathbf{\hat{b}} \sin b \sin(S - B)+ \mathbf{\hat{c}} \sin c \sin(S - C)
$$

If we want to stop there that's fine, but the form of the circumcenter given in the earlier post used the cross product of the vertices, suggesting a relationship with the polar triangle. So convert $$\mathbf{h}$$ using the matrix given earlier:

$$
\mathbf{h}'
=
\begin{bmatrix}
\sin a & \cos c \sin b & \cos b \sin c \\
\cos c \sin a & \sin b & \cos a \sin c \\ 
\cos b \sin a & \cos a \sin b & \sin c 
\end{bmatrix} 
\mathbf{h}
=
\begin{bmatrix}
\sin(a) \sin(S-A) + \sin(b) \cos(c) \sin(S-B) + \sin(c) \cos(b) \sin(S-C),\\
\sin(b) \sin(S-B) + \sin(c) \cos(a) \sin(S-C) + \sin(a) \cos(c) \sin(S-A),\\
\sin(c) \sin(S-C) + \sin(a) \cos(b) \sin(S-A) + \sin(b) \cos(a) \sin(S-B),
\end{bmatrix}
$$

We'd like to have all angles (capital letters) so we can simplify. By the law of sines (and again, absorbing a constant term), we can replace $$\sin a$$ and $$\sin A$$, and the same for $$b$$ and $$c$$. We can replace $$\cos c$$ using the second spherical law of cosines: $$\cos C = -\cos A \cos B + \sin A \sin B \cos c$$, and correspondingly for $$a$$ and $$b$$. Let's concentrate on the first term.

$$\begin{split}
& \sin(A) \sin(S-A) + \sin(B) \cos(c) \sin(S-B) + \sin(C) \cos(b) \sin(S-C) \\
= 
& \sin(A) \sin(S-A) + \frac{\sin(S-B)(\cos(C) + \cos(A) \cos(B))}{\sin(A)} + \frac{\sin(S-C)(\cos(B) + \cos(C) \cos(A))}{\sin(A)} \\
= 
& 2 \cos(S - A) \cos(S - B) \cos(S - C)
\end{split}$$

where we're omitting a bunch of tedious trigonometric identities that I made Wolfram Alpha do anyways. Note that this is invariant under cyclic permutation of $$A$$, $$B$$, and $$C$$, so the second and third elements of $$\mathbf{h}$$ are the same quantity, and the polar transformation of the trimetric coordinates is $$(1:1:1)$$. To put it another way: The circumcenter of a spherical triangle is the incenter of its polar triangle. This fact is probably easier to prove in a directly geometric manner.

## Orthocenter

$$\mathbf{h} = (\sec A : \sec B : \sec C)$$

$$
\mathbf{p} = \mathbf{\hat{a}} \frac{\sin a}{\cos A} + \mathbf{\hat{b}} \frac{\sin b}{\cos B} + \mathbf{\hat{c}} \frac{\sin c}{\cos C}
$$

Again, the first time I derived this the expression it was in terms of the cross products of the vertices, so let's transform it to the polar triangle.

$$
\mathbf{h}'
=
\begin{bmatrix}
\sin a & \cos c \sin b & \cos b \sin c \\
\cos c \sin a & \sin b & \cos a \sin c \\ 
\cos b \sin a & \cos a \sin b & \sin c 
\end{bmatrix} 
\mathbf{h}
=
\begin{bmatrix}
\frac{\sin a }{\cos A} + \frac{\sin b \cos c }{\cos B} + \frac{\sin c \cos b}{\cos C},\\
\frac{\sin b }{\cos B} + \frac{\sin c \cos a }{\cos C} + \frac{\sin a \cos c}{\cos A},\\
\frac{\sin c }{\cos C} + \frac{\sin a \cos b }{\cos A} + \frac{\sin b \cos a}{\cos B}
\end{bmatrix}
$$

Like before, replace the side lengths with distances using the spherical laws of sines and cosines, and just focus on the first one.

$$\begin{split}
& \frac{\sin A }{\cos A} + \frac{\sin B \cos c }{\cos B} + \frac{\sin C \cos b}{\cos C} \\
=
& \frac{\sin A }{\cos A } +
\frac{\cos C  + \cos A  \cos B }{\sin A  \cos B } +
\frac{\cos B  + \cos C  \cos A }{\sin A  \cos C } \\
=
& \frac{ (\cos A  \cos B  + \cos C )  (\cos A  \cos C  + \cos B )}{\sin A  \cos A  \cos B  \cos C } \\
=
& \cos c  \cos b  \tan A  \tan B  \tan C  \\
\end{split}$$

Between the second and third forula a number of trig identities are omitted. The third step is transformed into the fourth step by applying the spherical law of cosines again. The tangent factors, taken together, are invariant under cyclic permutation. Eliminating the common factors, $$\mathbf{h}' = (\cos c \cos b: \cos c \cos b : \cos c \cos b)$$, which is the form we used for the orthocenter in the earlier post. Dividing through by the product of the cosines of each edge gives $$\mathbf{h}' = (\sec a : \sec b : \sec c)$$, which suggests that the orthocenter of a spherical triangle is also the orthocenter of its polar triangle.
