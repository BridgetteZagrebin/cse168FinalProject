---
title: Welcome to My Rendering Final
layout: default
---

This is the product of  extending my path tracer integrator in order to acheive more interesting image results!

# Modern Importance Sampling Strategies for Lighting and BRDFs
- enabling rendering of environment maps and BRDFs more efficiently. One could implement the wavelet importance sampling or similar paper for sampling the product of lighting and BRDFs. A more modern approach would be to also implement path guiding (for example Vorba or Muller papers); this can best be showcased by modeling a scene where most of the indirect lighting corresponds to a small number of paths. You may also want to consider some form of Metropolis light transport (originally from Veach 97, but there are many newer and simpler implmentations). Note that importance sampling techniques can also be applied to volumetric phenomena, on which there have been several recent papers. 

- images
- documentation
