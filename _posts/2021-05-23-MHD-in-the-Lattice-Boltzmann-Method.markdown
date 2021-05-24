---
layout: post
title:  "MHD in the Lattice Boltzmann Method"
date:   2021-05-23 10:33:54 -0300
categories: jekyll update
author: "Bruno Magacho"
---

* #### **Changing the equilibrium distribution**

Following [Dellar (2002)](https://doi.org/10.1006/jcph.2002.7044), the correct equilibrium part of the stress tensor $$\Pi^{(0)}$$ may be incorpored if the equilibrium distributions are corrected by the term 

$$
	f^{(0)}_{i,mag} = \dfrac{\omega_i}{2 c^4_s}( \mathbf{c_i} \otimes \mathbf{c_i} - c^2_s\mathbf{I} ) : [M - c^2_s \mathbf{I} (Tr\hspace{0.1 cm}M) ] ,
$$

where $$\otimes$$ represent the tensorial product, $$:$$ is the Frobenius inner product, $$\mathbf{I}$$ is the identity matrix and $$M$$ is the Maxwell stress tensor, given by

$$
	M_{\alpha\beta} = \dfrac{1}{2}\delta_{\alpha\beta}\vert\mathbf{B}\vert ^2 - B_{\alpha}B_{\beta} .
$$

Opening the expression for the $$f^{(0)}_{i,mag}$$ in $$D$$ dimensions (where $$D = \{2,3\}$$), we have

$$
	f^{(0)}_{i,mag} = \dfrac{\omega_i}{2 c^4_s} [\mathbf{c_i} \otimes \mathbf{c_i} : M - c^2_s \mathbf{c_i} \otimes \mathbf{c_i}:\mathbf{I} (Tr\hspace{0.1 cm}M) -c^2_s \mathbf{I}:M + c^4_s \mathbf{I}:\mathbf{I}(Tr\hspace{0.1 cm}M)] \\
	= \dfrac{\omega_i}{2 c^4_s} [c_{i\alpha}c_{i\beta}M_{\alpha\beta} - c^2_s \vert \mathbf{c_i} \vert^2 (Tr\hspace{0.1 cm}M) -c^2_s (Tr\hspace{0.1 cm}M) + D c^4_s (Tr\hspace{0.1 cm}M) ] \\
	= \dfrac{\omega_i}{2 c^4_s} \Bigg [ \dfrac{1}{2}\vert \mathbf{c_i} \vert ^2 \vert \mathbf{B} \vert ^2 - (\mathbf{c_i} \cdot \mathbf{B})^2 - c^2_s \vert \mathbf{c_i} \vert^2 (Tr\hspace{0.1 cm}M) -c^2_s (Tr\hspace{0.1 cm}M) + D c^4_s (Tr\hspace{0.1 cm}M) \Bigg ] .
$$

But in $$D$$ dimensions we have

$$
	Tr\hspace{0.1 cm}M = \vert \mathbf{B} \vert ^2 \Bigg (\dfrac{D}{2} -1 \Bigg )
$$

So, after some straightfoward algebra $$f^{(0)}_{i,mag}$$ can be written as

$$
	f^{(0)}_{i,mag} = \dfrac{\omega_i}{2 c^4_s} \Bigg [ \dfrac{\vert \mathbf{c_i} \vert ^2 \vert \mathbf{B} \vert ^2}{D} - (\mathbf{c_i} \cdot \mathbf{B})^2  \Bigg ].
$$

* #### **Applying the Lorentz Force as an external force**

Following [Pattison et al. (2008)](https://doi.org/10.1016/j.fusengdes.2007.10.005) and [Lévêque et al. (2020)](https://hal.archives-ouvertes.fr/hal-02970050), the Lorentz force might be written as

$$
	F_k = (\mathbf{J} \times \mathbf{B})_k = -\dfrac{\omega_{m}}{c^2_{m}}\sum_{i}(\xi_{ij}B_j g_{ik} - B_j g_{ij}\xi_{ik})-\dfrac{2\omega_{m}}{c^2_{m}}[\vert \mathbf{B} \vert^2 u_{k} - (\mathbf{u} \cdot \mathbf{B}) B_k],
$$ 

where $$\omega_m$$ and $$c_m$$ are the magnetic relaxation frequency and the magnetic disturbance velocity respectively. For more details on the magnetic distribution functions $$g$$ the reader is recommended to the above articles. 

Note: Both ways should be equivalent, changing the equilibrium distribution and imposing a Lorentz Force as an external force, although at least for me, the implementation with the force term converged faster (it might be for a single realization, more tests should be made to certify this).




