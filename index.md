---
title: .
layout: default
---

<h1 style="text-align: center;"><strong>CSE Final Project</strong></h1>


Modern Importance Sampling for Lighting and BRDFs

This is the product of extending my path tracer integrator in order to acheive more interesting image results!

I decidied to go on the route of enabling rendering of environment maps and BRDFs more efficiently. 

Here were some strategies: 
- One could implement the wavelet importance sampling or similar paper for sampling the product of lighting and BRDFs.
- A more modern approach would be to also implement path guiding (for example Vorba or Muller papers); this can best be showcased by modeling a scene where most of the indirect lighting corresponds to a small number of paths.
- You may also want to consider some form of Metropolis light transport (originally from Veach 97, but there are many newer and simpler implmentations).
- Note that importance sampling techniques can also be applied to volumetric phenomena, on which there have been several recent papers. 

- images
- documentation

<p>
  <img src="https://github.com/user-attachments/assets/b5519dd7-de41-43b3-bb87-e03fdb158498" alt="sphere" width="350">
  <img src="https://github.com/user-attachments/assets/b5519dd7-de41-43b3-bb87-e03fdb158498" alt="sphere" width="350">
</p>

<h2 style="text-align: center;"><strong>Documentation</strong></h2>



<h2 style="text-align: center;"><strong>Resources:</strong></h2>
- “Practical Path Guiding” by Müller et al. (2017)
- 2D/3D grid or BVH-like tree
- [title](https://www.example.com)
- Müller et al., Neural Importance Sampling, SIGGRAPH 2019.
- Vorba et al. (2014)
