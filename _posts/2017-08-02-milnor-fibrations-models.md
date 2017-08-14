---
layout: post
title: Fibration Fabrication
author: Henry Blanchette
published: false
category: Milnor Fibrations
---

feature:
  file: fibration-model-large.jpg
  file_type: image

## Introduction

This article will cover my methods and results of modelling and printing [Milnor Fibrations]({{ site.baseurl }}/posts/milnor-fibrations). Make sure to check out that article for explanations of the theory behind these intricate structures! As for visualizations, I had a few target aspects I wanted to focus on:

* **Single Fiber**

This is the basic $$\Phi(m,n,\theta)$$. A fiber is a 2D surface however, so some sort of thickening process is necessary for a 3D model. Below is an example of an unaltered fiber:

$$ \Phi(3,5,\pi/2) $$
{% include stl name="fiber_3-5-pidiv2.stl" %}

* **Double Fibers**

This is the union of two, "opposite" fibers at $$\theta$$ and $$\theta^\prime$$, where opposite mathematically means $$ \theta = \theta^\prime + \pi $$. This translates to the fibers being on opposites sides of $$\mathbb{S}^1$$, and accordingly facing each other in their $$\mathbb{R}^3$$ projections, as in below (though it is difficult to distinguish between the two fibers):

$$ \Phi(2,3,\pi/2) \cap \Phi(2,3,3\pi/2) $$
{% include stl name="fiber_2-3-pidiv2-double.stl" %}

It is also apprent that they fit very aesthetically together.

* **_K_**

This is the boundary that all the fibers with fixed $$(m,n)$$ share. For a more detailed explanation, consult my [previous article]({{ site.baseurl }}/posts/milnor-fibrations). $$K$$ projects down to a knot or link in $$\mathbb{R}^3$$, so needs to be tubular in order to show up in a model. Below is an example of just $$K$$ without any fibers attached:

$$K \textnormal{ where } m,n=4,2$$
{% include stl name="fiber_4-2-pidiv2-justK.stl" %}

## Program

So how am I creating all these great visualizations anyway? To answer this, I will demonstrate my code in sections, explaining it briefly. A download link for the notebook that contains everything I'll talk about is also avaliable at the top of this article. Now lets get into it.

The first step is ```InitMilnor[m,n]```. This function defines two polynomial functions ```Ref```,```Imf``` whom deconstuct $$M$$'s codomain $$\mathbb{R}^2$$ into two seperate coordinates; ```Ref``` takes the real ```Imf``` the imaginary.

```
(* takes as input the powers in z^n - w^m. generates Ref and Imf *)
InitFiber[m_, n_] := Block[{f, g, u, v, x, y, z, w},
    f = u^m - v^n /. {u -> x + (I*y), v -> z + (w*I)};
    (* the real and imaginary parts of f, inversely projected *)
    {Ref, Imf} = Factor[(ComplexExpand[{Re[f], Im[f]}] /. {
        x -> (Sqrt[2]*(-2 + x^2 + y^2 + z^2))/(2 + x^2 + y^2 + z^2), 
        y -> (4 x)/(2 + x^2 + y^2 + z^2), 
        z -> (4 y)/(2 + x^2 + y^2 + z^2), 
        w -> (4 z)/(2 + x^2 + y^2 + z^2)
    })] * (2 + x^2 + y^2 + z^2)^Max[m, n];
];
```

Now, to get a single fiber, we need to consider some trigonometry. The goal is the get $$f^{-1}(\theta)$$ for $$\theta \in \mathbb{S}^1$$. What does this equivelate to in terms of ```Ref``` and ```Imf```? Looking at $$S^{1}$$ we see this correspondence between $$\theta$$ and the ```milnor```'s:

![](../assets/posts/milnor-fibrations-models/mfg_graphic.jpg)

From this, the following follows

$$
\tan(\theta) = \frac{ \sin(\theta) }{ \cos{\theta} } = \frac{\textnormal{ Ref }}{\textnormal{ Imf }}
$$

$$
\Longrightarrow \sin(\theta) \textnormal{Ref} - \cos(\theta) \textnormal{Imf} = 0
$$

I will refer to the left side of the above equation as _f_. By solving for $$
f = 0$$, we can extract the fiber we want! Well, actually, as is evident in the graphic, $$\tan(\theta)$$ is going to give us two, opposite, fibers. In order to eliminate one, we can make use of a utility fiber _g_, where _g_ is at $$\theta' = \theta + \frac{\pi}{2}$$. In the previous graphic, _g_ would be perpendicular to the green line of _f_. Now, in order to get a single fiber and meet goal 1, we can solve for $$f = 0 \land g > 0$$. In the code for drawing the fiber,

```
DrawFiberSingle[t_, index_] := Block[{f,g},
    f = Cos[t]*Ref + Sin[t]*Imf;
    g = Sin[t]*Ref - Cos[t]*Imf;
    ContourPlot3D[f == 0 && g < 0, {x, -range, range}, {y, -range, range}, {z, -range, range}]
];
```

Great! This actually gets two birds with one stone, because we can remove the specification about _g_ in order to meet goal 2 - the double fiber.

For the last goal, the following code generates _K_'s parametric path in $$\mathbb{R}^3$$:

```
InitKnot[m_, n_] := Block[{u, uu, v, x, y, z, w, curves, tubes},
    (* paramtaterize K (there may be multiple curves) *)
    curves = Table[{-((Sqrt[2] y)/(-Sqrt[2] + x)), -((Sqrt[2] z)/(-Sqrt[2] + x)), -((Sqrt[2] w)/(-Sqrt[2] + x))} /.
        {x -> Cos[n u + 2 Pi k/m], y -> Sin[n u + 2 Pi k/m], z -> Cos[m u], w -> Sin[m u]}, {k, 0, GCD[m, n] - 1}];
    (* create tubes along K *)
    tubes = Table[curves[[i]], {i, 1, Length[curves]}];
    milnorlink = Show[ParametricPlot3D[tubes, {u, -Pi, Pi}], PlotStyle -> Tube[tubeRadius]]
];
```

## Prints

I have developed five methods of physically modelling combinations of the goals listed in the previous section. Each method has its strengths and weaknesses. In this section, I will explain each one and the results I've produced with it. Many of the Mathematica techniques that I use here are covered in my [3D printing article]({{ site.baseurl }}/posts/3d-modelling).

(pics)

* **Thickened Fiber**

This method is the simplest of them all. It uses the exact code from the **Program** section, with the addition of ```Extrusion->thickness``` to the ```ContourPlot3D```. The result is a clean-cut single fiber, if you have the ```PlotPoints``` high enough, that is. With lower ```PlotPoints```, Mathematica lets the boundary at _K_ get jagged. 

(pics)

* **Infilled Fiber**

This method yields an interesting 3D volume. To create it, you fill in the region $$f \leq 0$$ rather than just $$f = 0$$. This effectively "fills in" the space between $$\Phi(\theta)$$ and $$\Phi(0)$$ by way of the region including all the fibers of $$\theta \in [0,\theta]$$. This is good for low $$m,n$$ values, as it creates a more solid version of the Thickened Fiber, while still allowing for detail near the center part of the model. However, with higher $$m,n$$ values, the center part becomes complicated with folded layers and results in an ugly model.

(pics)

* **Infilled Pair**

This method is an application of the above method, but for two opposite fibers. The unconventional infill effect of the above method ends up being invisible to a human viewer of the 3D model, for the infilled section is inside. In this way, I can print solid double fibers.

(pics)

* **Asymmetrical Fiber Pair**

Rather than printing two opposite fibers, this method displays two nearby fibers, ideally where $$\theta - \theta^\prime \leq \pi/3$$. To make this method work, some of the outer-fiber has to be cut away to allow for the viewer to see inside, hence the point of having an inner fiber at all. This style of model demonstrates better than the others the fashion in which the fibers wrap around _K_.

(pics)

* **Thickened Fiber Pair with a _K_ removed** or "Cut the Knot"

This method is very unique. It starts as what would be a Thickened Fiber Pair with _K_ embedded. Then, I used Blender to apply a boolean mesh difference between the fibers and _K_, leaving the fibers intact a distance away from their boundary at _K_.

(pics)

* **"Cut the Knot" with thinner _K_ Embedded**

This method is an addition on the previous. Along with cutting away _K_ (modeled as a tube with radius $$r$$), I further embedded a copy of _K_ tubed with radius $$r/2$$, allowing for all three parts - the two fibers and the _K_ tube - to be free moving!

(pics)

## Conclusion