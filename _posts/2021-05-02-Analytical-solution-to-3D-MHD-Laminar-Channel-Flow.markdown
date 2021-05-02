---
layout: post
title:  "Analytical solution to 3D MHD Laminar Channel Flow"
date:   2021-05-02 10:33:54 -0300
categories: jekyll update
author: "Bruno Magacho"
---

Following the analytical solution given by [Shercliff (1953)](https://doi.org/10.1017/S0305004100028139)

In a 3D channel flow with a rectangular cross section bounded by walls in $$ x = \pm a$$ and $$ y = \pm b $$ with a pressure gradient $$dp/dz$$, a conducting fluid with conductivity $$\sigma$$ and dynamical viscosity $$\mu$$, if we have an uniform and static external magnetic field $$\mathbf{H} = H_0\hat{x}$$, the analytical solution to the velocity in the streamwise direction $$v_z$$ and the induced magnetic field $$H_z$$ may be obtained by


$$

	v = v_z + H_z/4\pi(\sigma \mu)^{1/2} \ , \\
	w = v_z - H_z/4\pi(\sigma \mu)^{1/2} \ ,

$$

where 

$$
	v = \dfrac{16b^2}{\mu\pi^3}\dfrac{dp}{dz}\sum^{\infty}_{n=0} \dfrac{(-1)^n}{(2n+1)^3}
	\Bigg \{ 1 + \dfrac{e^{m_+ x}sinh(m_- a)-e^{m_- x}sinh(m_+ a)}{sinh[(m_+-m_-)a]} \Bigg \} 
	cos \Bigg [\dfrac{(2n+1)\pi y}{2b} \Bigg ] \ ,
$$

and 

$$
	w(x,y) = v(-x,y) \ .
$$

Where $$m_+$$ and $$m_-$$ are given by

$$
	m_{\pm} = -\dfrac{M}{2a} \pm \dfrac{1}{2} \Bigg [ \dfrac{M^2}{a^2} + \dfrac{(2n+1)^2\pi^2}{b^2} \Bigg ]^{1/2} \ ,
$$

and $$ M = \mu_{mag}H_0 a(\sigma/\mu)^{1/2} $$. Here $$\mu_{mag}$$ stands for the magnetic permeability of the fluid.

Note: Do NOT confuse $$\mu$$ with $$\mu_{mag}$$, it should be clear the $$\mu$$ is the dynamical viscosity and $$\mu_{mag}$$ is the magnetic permeability.




