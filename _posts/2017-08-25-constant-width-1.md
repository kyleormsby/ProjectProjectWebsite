---
layout: post
title: Constructing Bodies of Constant Width
author: Cameron Fish and Jalan Ziyad
category: Constant Width
resources:
  - name: Reuleaux triangle
    file: reuleaux.png
  - name: Crescent
    file: crescent.png
  - name: Support Lines Animation
    file: supportlines.gif
  - name: Analytic Reuleaux Triangle
    file: analyticreuleaux.png
  - name: xyz Body
    file: xyz.stl
  - name: Support Line Intersection
    file: suppinter.png
---
Consider what happens when you roll a sphere between your hands. Assuming your hands are perfectly flat and parallel, then no matter how you roll, the distance between your hands--the “width” of the sphere--should remain constant. Is a sphere the only solid for which this is true? Interestingly enough, there are in fact many solids possessing this constant width property which are not spheres! To see how, we need to be a little more formal with this idea of width.

Suppose you have some compact, convex body living in $\mathbb{R}^2$. For example:

{% include image name="reuleaux.png" caption="You might recognize this figure as the Reuleaux triangle."  %}

{% include image name="crescent.png" caption="This body is not convex--you can draw a line connecting two points inside it which pokes outside."%}

To capture this notion of width, we want to be able to draw a pair parallel lines just touching the body in two places and find the distance between them. Notice that for any choice of unit vector, we can find a line with that vector as normal which touches the body at exactly one point. We can actually find two, since a line has two choices of unit normal vector, but we can insist that, for a given unit vector, the line we pick is the one with outwardly oriented normal (i.e. pointing away from the body, which is unambiguous since the body is convex). Thus, given a support line given by a unit normal vector $u$, the support line parallel to it is given by $-u$.  These "support lines" are analogous to our hands rolling the body around.

{% include image name="supportlines.gif" caption="Pairs of parallel support lines given a varying unit vector angle--the distance between the lines is the width in that direction.. Note that this figure has constant width!" %}

We call the distance between two parallel support lines with normal direction $u$ the width of the body in the $u$ direction. If this width is the same for every choice of $u$, then the body has constant width. The usual notation is to define the "support function" $h$ which, given a unit vector, gives you the distance from the center of the body to the support line having that vector as normal. Then, constant width means that $h(u) + h(-u)$ is constant for any unit vector $u$. Note that we can similarly define bodies of constant width in any number of dimensions by defining the support function from $\mathbb{S}^{n-1}$ to $\mathbb{R}^n$ in general. The support lines become planes in three dimensions, and hyperplanes for $n \gt 3$. 

Given any convex body, there exists such a support function. It turns out that we can also choose specific support functions and guarantee they specify a convex body. First, consider the extension $H$ of $h$ where $H(su) = sh(u)$ for unit vectors $u$ in $\mathbb{R}^n$ and $s \geq 0$  (in particular, $H(x) = H(||x||\frac{x}{||x||}) = ||x||h(\frac{x}{||x||})$ since $\frac{x}{||x||}$ is in $\mathbb{S}^{n-1}$). $H$ satisfies a few key properties:
	
$$H(0) = 0$$$$H(su) = sH(u) \mbox{ for }u\mbox{ in } \mathbb{R}^n \mbox{ and }s \gt 0$$$$H(u + v) \leq H(u) + H(v) \mbox{ for }u, v \mbox{ in }\mathbb{R}^n$$

Additionally, $H$ has the constant width property $H(u) + H(-u) = c$ for $u$ nonzero if and only if h also does. So, if we can find such a function with this constant width property $H(u) + H(-u) = c$ and satisfying the above properties, then we have a convex body of constant width with support function $h = H|\mathbb{S}^{n-1}$ (H restricted to the unit sphere).

One such support function (given by Fillmore 1969) is $h(\theta) := a + Cos(3\theta)$ for  $a \gt 8$. Note this is not defined in terms of unit vectors u, so let's rewrite it as $h(x, y) = a - 3 x + 4 x^3$. (Note that $cos(3\theta) = 4(cos(\theta))^3 - 3cos (\theta)$ and for unit $u$, the angle it makes with the x axis is $\theta = arccos(x))$. This function clearly satisfies the first two conditions above, and the third may be checked with some algebra and the triangle inequality.

We can also see that this function has the constant width property! 

$$h(u) + h(-u) =  a - 3 x + 4 x^3 + a - 3 (-x) + 4(- x)^3 = 2a$$
	
Therefore, we can define a body of constant width using this support function! Here's what the body defined by this support function looks like (for a = 15 for example):

{% include image name="analyticreuleaux.png"  %}

Notice the similarity to the Reuleaux triangle depicted above—this body is an analytic version. (Choosing $a = 8$ gives the Reuleaux triangle, and the body approaches a circle as a approaches infinity. For $a \lt 8$ the body is no longer convex.)

Here's an example of a body of constant width in three dimensions:

{% include stl name="xyz.stl" caption="The support function for this body is $h(x,y,z) = 1 + xyz$"  %}

This body is piecewise smooth and has the same symmetries as a tetrahedron. It is an example from a particularly interesting class of bodies of constant width which exhibit the symmetries of familiar solids (tetrahedron, octahedron, icosahedron, etc), and other subgroups of $SO(3)$, i.e. rigid rotations of $\mathbb{R}^3$.

How exactly can we create these bodies, given one of these support functions? Recall that our definition of constant width involves parallel support hyperplanes which bound the body in every direction. These planes are exactly $u\cdot x = h(u)$ for all $u$ in $\mathbb{S}^{n-1}$ (i.e. the hyperplane $u_1x_1 + u_2x_2 + u_3x_3 + … + u_nx_n = h(u)$).

If our support function h satisfies the above conditions, and so defines a convex body, then this body is the set of points x such that:

$$u\cdot x \leq h(u) \mbox{ for all } u$$

In other words, each hyperplane divides $\mathbb{R}^{n}$ into two half-spaces, and the body is the intersection of each of the “interior” half spaces (the ones lying opposite the normal vectors of the hyperplanes). So, one way to create a body given a support function is to simply find this intersection. This method turns out to be difficult to implement in practice however--you would need to have your mathematical software intersect an infinite, or at least very large, number of half-spaces. 

{% include image name="suppinter.png" caption="An approximation of the body using support lines. The body is the limiting intersection of the interior half spaces given by these lines."  %}

It turns out there is a simpler approach: for unit vectors $u$ in $\mathbb{R}^n$, the gradient $\nabla H(u)$ parametrizes the boundary of the convex body having support function $h = H|\mathbb{S}^{n-1}$ (try checking this fact for yourself as an cool exercise).

For example, if we choose $f(x,y,z) = xyz$ as above then we have:

$$h = 1 + xyz$$$$H = \sqrt{x^2 + y^2 + z^2}\frac{1 + (x y z)}{(x^2 + y^2 + z^2)^{3/2}}$$$$H|S^{1} = 1 + xyz$$$$ \nabla H|S^{1}=\begin{pmatrix}y z - 3 x^2 y z + x (1 + x y z)  \\x z - 3 x y^2 z + y (1 + x y z)\\ x y - 3 x y z^2 + z (1 + x y z)\end{pmatrix}$$

Now, we can parametrize this gradient with spherical coordinates and plot it using Mathematica, giving the body seen above. Next time, we’ll discuss why the function $xyz$ gives a body with tetrahedral symmetry, as well as how to create even more solids with a number of different groups of symmetry. Until next time!

