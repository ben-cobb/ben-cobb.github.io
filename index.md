---
layout: about
title: About
permalink: /
description: <a href="https://cse.gatech.edu/people/benjamin-myers-cobb" target="_blank"> Graduate PhD Student </a>, <a href="https://cse.gatech.edu/" target="_blank"> School of Computational Science and Engineering </a>, <a href="https://www.gatech.edu/" target="_blank"> Georgia Institute of Technology</a>
# . Address. Contacts. Moto. Etc.

profile:
  align: right
  image: prof_pic.jpg
#   address: >
    # <p>555 your office number</p>
    # <p>address...</p>
    # <p>address...</p>

news: false #true  # includes a list of news items
selected_papers: true # includes a list of papers marked as "selected={true}" false #
social: true  # includes social icons at the bottom of the page
---

Welcome
-----------

Hi! I am currently a PhD student at the Georgia Institute of Technology under the supervision of 
[Haesun Park](https://faculty.cc.gatech.edu/~hpark/experiences.html){:target="\_blank"} and [Richard
Vuduc](https://vuduc.org/v2/){:target="\_blank"}. Previous to
attending Georgia Tech, I attended Wake Forest University, where I received my Bachelor of
Sciences’ in Computer Science and Mathematical Business. To get in contact with me, feel
free to send me an email using the link below.


Research
-----------
My current research focuses on developing distributed Non-negative Matrix Factorization
([NMF](https://en.wikipedia.org/wiki/Non-negative_matrix_factorization){:target="\_blank"})
algorithms within the Parallel Low-rank Approximations with Non-negativity Constraints
([PLANC](https://github.com/ramkikannan/planc){:target="\_blank"}) framework for use in
large-scale datasets. This work is in close collaboration with Oak Ridge National Lab
([ORNL](https://www.ornl.gov/publication/planc-parallel-low-rank-approximation-nonnegativity-constraints){:target="\_blank"}).

Previous to this, I worked under the supervision of [Ümit V.
Çatalyürek](https://faculty.cc.gatech.edu/~umit/){:target="\_blank"} on the
[GenTen](https://gitlab.com/tensors/genten){:target="\_blank"} project in collaboration with Sandia
National Laboratories ([SNL](https://www.sandia.gov/){:target="\_blank"}) as part of the Department of
Energy's ([DOE](https://www.energy.gov/){:target="\_blank"}) larger [Exascale Computing
project](https://www.exascaleproject.org/){:target="\_blank"}. On this project, I had the pleasure of
collaborating with [Eric
Phipps](https://cfwebprod.sandia.gov/cfdocs/CompResearch/templates/insert/profile.cfm?etphipp){:target="\_blank"}
and [Hemanth Kolla](https://scholar.google.com/citations?user=_9VQ8rUAAAAJ&hl=en){:target="\_blank"}
as we worked to optimize portable dense tensor kernels based upon the
[Kokkos](https://github.com/kokkos){:target="\_blank"} programming model for use in
compressing large combustion simulation datasets. As part of this, we developed a novel in-place
variant of the Sequentially Truncated Singular Value Decomposition
([ST-HOSVD](https://people.cs.kuleuven.be/~nick.vannieuwenhoven/papers/01-STHOSVD.pdf){:target="\_blank"})
for computing the [Tucker
Decomposition](https://en.wikipedia.org/wiki/Higher-order_singular_value_decomposition){:target="\_blank"}.
The resulting algorithm, referred to as the Fused In-place Sequentially Truncated Singular Value
Decomposition (FIST-HOSVD), was shown to reduce the memory consumption of computing the Tucker
Decomposition by ~135x with comparable runtime performance compared to the current state-of-the-art
ST-HOSVD implementation within the
[TuckerMPI](https://gitlab.com/tensors/TuckerMPI){:target="\_blank"} framework.

My undergraduate research primarily focused on applying wedge-sampling heuristics to speeding up the
coarsening phase of hypergraph partitioners and applying bipartite matching algorithms to computing
least-squares cosine differences of ktensors. This work was performed under the supervision of my
undergraduate advisor and mentor, [Grey Ballard](http://users.wfu.edu/ballard/index.html){:target="\_blank"}.