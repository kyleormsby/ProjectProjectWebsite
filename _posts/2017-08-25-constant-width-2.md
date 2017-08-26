---
layout: post
title: Bodies of Constant Width With Particular Symmetries
author: Cameron Fish and Jalan Ziyad
category: Constant Width
resources:
  - name: x^3 Body
    file: x3.stl
  - name: Degree 9 Octahedral
    file: oct9.stl
  - name: Degree 7 Tetrahedral
    file: tet7.stl
  - name: generators
    file: gens.txt

---
Last time we introduced bodies of constant width and showed several examples, as well as hinted at constructing 3D bodies with the same symmetries as several familiar solids. This time we will discuss that method in detail and produce a number of interesting solids. All we will need is some linear algebra, group theory, and a little invariant theory.

We previously gave an example of a body of constant width with the support function $h(x,y,z) = 1 + xyz$. That was a particular example of a general method of constructing bodies of constant width using support functions of the form $h(u) = 1 + \epsilon f(u)$, where f is any odd function (from $\mathbb{R}^{n}$ to $\mathbb{R}$) and $|\epsilon| \gt 0$ is sufficiently small. Notice that for odd $f$, $$h(u) + h(-u) = 1 + \epsilon f(u) + 1 + \epsilon f(-u) = 2.$$ So any support function like this defines a body of constant width 2. The condition on $\epsilon$ just ensures the body is convex. (Given some odd function f, we can determine the upper bound on $|\epsilon|$ such that $h(u) = 1 + \epsilon f(u)$ defines a convex body by requiring that the triangle equality $H(x + y) \leq H(x) + H(y)$ holds).

For example, say our odd function is $f(x,y,z) = x^3$. This gives the following body of constant width:

{% include stl name="x3.stl"  %}

This is nice, as there are any number of simple odd functions to choose from. However, it would be cool to construct bodies which have some chosen symmetries. How about bodies which have any choice of symmetry group (subgroups of $SO(3)$, i.e. orientation preserving rigid rotations in $\mathbb{R}^{3}$)? A theorem of Fillmore (1969) tells us that every finite subgroup of $SO(3)$ is the group of symmetries of some body of constant width. In particular, if we choose a subgroup $G$ and an odd function $f(u)$ which is invariant under the action of $G$, then the body defined by the support function $h(u) = 1 + \epsilon f(u)$ has the symmetries of $G$. The set of polynomials invariant under the actions of a given group $G$ form a ring, any odd member of which we can use to create a $G$-symmetric body of constant width.

Fortunately, the theory of [G-invariant polynomials](https://en.wikipedia.org/wiki/Invariant_theory) is well-developed, and mathematical software such as Sage and Magma have efficient ways to compute the ring of polynomial invariants of a given matrix group. So all we need to do is determine three-dimensional matrix representations of our favorite symmetry group. Our options include the tetrahedral, octahedral, and icosahedral groups, among a few others--let’s focus on the octahedral group $O$.

The set of symmetries of an octahedron is the same as the symmetries of a cube, since they are dual polyhedra. Consider that there are only two actions needed to generate all the orientation preserving symmetries of the cube: rotation by 90 degrees around the $z$ axis and rotation by 90 degrees around the $y$ axis. These rotations act on the unit vectors (pointing to three of the faces) in the following ways:

$$S = \begin{pmatrix}0&1&0  \\-1&0&0\\ 0&0&1\end{pmatrix}$$$$T = \begin{pmatrix}0&0&1  \\0&1&0\\ -1&0&0\end{pmatrix}$$

We will use Magma to compute the generators of the ring of polynomials invariant under O:

> F := Rationals();
> S := [0,1,0,-1,0,0,0,0,1];
> T := [0,0,1,0,1,0,-1,0,0];
> G := MatrixGroup<3, F|S,T>;
> R := InvariantRing(G);
> PrimaryInvariants(R);
[
    x1^2 + x2^2 + x3^2,
    x1^4 + x2^4 + x3^4,
    x1^6 + x2^6 + x3^6
]
> SecondaryInvariants(R);
[
    1,
    x1^5*x2^3*x3 - x1^5*x2*x3^3 - x1^3*x2^5*x3 + x1^3*x2*x3^5 + x1*x2^5*x3^3 -
        x1*x2^3*x3^5
]

In general, the ring of invariants $R$ has primary invariants $f_1,...,f_m$ which are algebraically independent. Thus the algebra of primary invariants $A = F[f_1,...,f_n]$ is a polynomial subalgebra of $R$  If $g_1,...,g_n$ are the secondary invariants of $R$, then there is an A-module decomposition of $R$ as

  $$R \cong \bigoplus_{i=1}^n Ag_n$$

Thus, to determine an F-basis all homogeneous degree $d$ polynomials in $R$, we just need to enumerate all the products of primary and secondary invariants homogeneous of degree $d$, with the restriction that exactly one secondary invariant can appear in each product (note that the first secondary invariant is always 1, and so a product with 'only' primary invariants still has one secondary invariant). For example, in the above Magma computation, our invariants are:

$$f_1 = x_1^2 + x_2^2 + x_3^2$$$$f_2 = x_1^4 + x_2^4 + x_3^4$$$$f_3 = x_1^6 + x_2^6 + x_3^6$$$$g_1 = 1$$$$g_2 =x_1^5x_2^3x_3 - x_1^5x_2x_3^3 - x_1^3x_2^5x_3 + x_1^3x_2x_3^5 + x_1x_2^5x_3^3 - x_1x_2^3x_3^5$$

Every degree 4 polynomial in the ring can be written as a linear combination of $f_1^2$ and $f_2$. These happen to be linearly independent and so form a basis (writing $x_1, x_2, x_3$ as $x, y, z$):

$$\{(x^2 + y^2 + z^2)^2,  x^4 + y^4 + z^4\}$$

Note that since we only really care about the behavior of these polynomials on the unit sphere, we can simplify our basis with the substitution $x^2 + y^2 + z^2 = 1$, giving us:

$$\{1, x^4 + y^4 + z^4\}$$

However, this particular vector space contains no polynomials we can use to create bodies of constant width, since the degree of each of them is even. The smallest odd degree polynomial vector space in this ring has degree 9 and the vector space of such elements has basis:

$$\{x^5 y^3 z - x^3 y^5 z - x^5 y z^3 + x y^5 z^3 +x^3 y z^5 - x y^3 z^5\}$$

Now, we can choose any polynomial in this space to define a body of constant width with octahedral symmetry. For example, the body below has support function $$h = 1 + 3.2(x^5 y^3 z - x^3 y^5 z - x^5 y z^3 + x y^5 z^3 + x^3 y z^5 - x y^3 z^5)$${% include stl name="oct9.stl"  %}

This whole process can be repeated for any other finite subgroup of $SO(3)$. Below are some bodies with tetrahedral and icosahedral symmetries respectively:

{% include stl name="tet7.stl" caption="This body has tetrahedral symmetry and has support function $h = 1 + 0.0523014 \cdot 11/8 \sqrt{1365/\pi} (-(5/11) x y z + x y z (x^4 + y^4 + z^4))$" %}

{% include stl name="ico15.stl" caption="This body has icosahedral symmetry (the support function is much too long to present here)." %}

[Here's](**Link to gens.txt**) a list of the primary and secondary generators we found this way for the polynomials invariant under the groups T, O, and I.

With this theory laid down, it’s straightforward to compute any number of support functions of interesting bodies of constant width with these symmetries. Next time, we’ll walk through the process of realizing one of these bodies, from initial choice of symmetry to final 3D-printed product.