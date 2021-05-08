---
layout: post
title:  "Adding non equilibrium distributions to boundaries in LBM for a WET NODE approach"
date:   2021-05-02 10:33:54 -0300
categories: jekyll update
author: "Bruno Magacho"
---

According to the Chapman-Enskog analysis, the velocity distribution functions may be expanded as

$$
	f = f_{0} + \epsilon f_{1} + O(\epsilon^2) \ , 
$$

where $$f_0$$ is related to the equilibrium Maxwell Boltzmann distribution and alone it gives the contributions to the bulk of the flow as a solution to the Euler's equations. The second term $$f_{1}$$, which is the non equilibrium part $$f_{neq}$$, brings the information about the viscous contributions to the flow.

For the simplest models in Lattice Boltzmann, as the BGK approach for example, it already include the informations about the off equilibrium part in the bulk of the flow, since the collision in the BGK approach reads

$$
	f_{out} = f_{in} - \omega (f_{in} - f_{eq}) = f_{in} - \omega (f_{eq} + f_{neq} - f_{eq}) \ , \\
	\hspace{2.3 cm} = f_{in} - \omega f_{neq}.
$$

In the boundaries there are several ways in how to implement the boundary conditions, like imposing the equilibrium on boundaries with the density $$\rho$$ and velocity $$\mathbf{u}$$ specified $$f_{boundary} = f_{eq}(\rho,u)$$. Here we are going to show two straightfoward ways that leads to an accurate result for the velocity field close to the boundaries that include the informations about the non equilibrium part of the velocity distributions $$f_{neq}$$.

First approach

The simplest way should be adding this information from the first closest node. An example should be if the boundary is located in a fixed $$x$$ position say $$x_i$$ and it varies in the $$y$$ direction, on the boundaries it should be set

$$
	f_{in}(x_i,y) = f_{eq}(x_i,y,\rho,u) + ( f_{in}(x_{i+1},y) - f_{eq}(x_{i+1},y,\rho,u)) \ , \\
	= f_{eq}(x_i,y,\rho,u) + f_{neq}(x_{i+1},y) \ .
$$

This strategy applies also to edges and corners in 3D.

Second approach

In the second approach, following [J. Latt et al (2008)](https://doi.org/10.1103/PhysRevE.77.056703), the idea is that the information about the non equilibrium distribution can be calculated from the stress tensor 

$$
	\Pi = \sum^{q}_{i=0} (c_i \otimes c_i) f_i \ , 
$$

where $$q$$ is the number of links from the lattice and $$\otimes$$ is the tensorial product. In this approach the velocity gradients is approximated by the non equilibrium part of the stress tensor calculated using the non eqquilibrium part of the velocity distribution as in the first approach. Then the non equilibrium part may be calculated by the projection of the non equilibrium part of the stress tensor in the tensor

$$
	Q_i = (c_i \otimes c_i) - c_s^2 \ ,
$$

where $$c_s$$ is the sound velocity. The non equilibrium part then is 

$$
	f^{(1)}_i = \dfrac{w_i}{2c_s^4}Q_i : \Pi^{(1)} \ , 
$$

where $$w_i$$ is the weight of each direction.

The main advantage is that this method is more accurated than the first one but more costly, since the calculation of the stress tensor in each wall is associated with a tensorial product for each time step.

The evaluation of the unknown $$f$$'s fo the calculation of $$f_{neq}$$ on the walls is made by the bounce-back of off equilibrium parts.


