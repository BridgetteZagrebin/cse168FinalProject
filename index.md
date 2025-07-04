---
title: .
layout: default
---

<h1 style="text-align: center;"><strong>CSE 168 Final Project</strong></h1>

Project Task: Extending my path tracer integrator with Vorba’s 2017 path guiding (a modern importance sampling strategy for lighting and BRDF).

--------------------------------------------------------------------------------------------------

## **Path Guiding**

Vorba's 2017 paper, "Path Guiding in Production", strives toward efficient and scalable sampling of indirect illumination in complex scenes by learning where light is likely to come from during rendering. The goal is to reduce noise and variance in path tracing by improving the way directions are sampled for indirect bounces, particularly in scenes with difficult indirect lighting, such as caustics, interiors, and occluded areas.

## **Scene Test Files**

For most scene files, we have the settings usually set like so. Here, we highlight how Vorba uses multiple importance sampling(MIS) and path guiding. In the case of comparing the path gudiing results, when we want to view the basline, we just change the importance sampling back to bidirectional reflectance distribution function(BRDF), keeping MIS as Vorba does.:

```
integrator pathtracer
spp 100
importancesampling pathGuiding
nexteventestimation mis
russianroulette on
```

The samples per pixel(SPP) may be increased for some scenes for less noisy images.

Gamma was ignored in these images since they were not within Vorba's path guiding and it seemed to cause some conflict when interacting with the path guiding changes. However, as seen below, difference were still made with path guiding.

To view more drastic differences in the images, I messed with the quad lights a bit. This is to show how Vorba's path guiding samples bright indirect bounces more accuratly, while the baseline BRDF tends to undersample.

## **Image Results**

<h3 style="text-align: center;">BRDF(left) vs. Path Guiding(right)</h3> 

<p>
  <img src="https://github.com/user-attachments/assets/e88a37f7-abab-48ad-851e-eae89b8ea920" alt="cornellPG5 2" width="350">
  <img src="https://github.com/user-attachments/assets/910fc61d-d9b3-497a-b581-4b2eecda918c" alt="cornellPG5" width="350">
</p>

- Here we have sphere images with(top) and without(without) gamma correction:
<p>
  <img src="https://github.com/user-attachments/assets/4c229f30-99f8-4599-b81c-5aa604aac4c4" alt="cornellPG2 3" width="350">
  <img src="https://github.com/user-attachments/assets/7d881d42-d359-4290-b4be-8efcb0288bf0" alt="cornellPG2 4" width="350">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/b0b224e7-b231-4065-ba2f-cf1fce934c28" alt="cornellPG2 6" width="350">
  <img src="https://github.com/user-attachments/assets/ea15e5c4-c0fe-4906-a191-65ea134cabf0" alt="cornellPG2 5" width="350">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/1a3e2e25-0b03-4cf4-8c60-c71350425656" alt="cornellPG1 9" width="350">
  <img src="https://github.com/user-attachments/assets/a7a6e24f-7862-4599-a7d0-bedd22733dd1" alt="cornellPG1 11" width="350">
</p>


## **What was done to achieve this?**

Vorba’s 2017 path guiding is meant to adaptively learn where to send rays based on previously gathered radiance information. This approach targets improvements in convergence, especially in scenes with complex indirect lighting.	

Thus, the goal here was for the images above to show the interactions of an integrated spatial-directional data structure that stores radiance information throughout the scene. This structure maps position and surface normal to discretized directional bins, which is then sampled during rendering to inform future ray directions. Sampling is implemented to blend BRDF-based directions with the learned guiding distribution. Multiple Importance Sampling (MIS) was also updated so that it uses the power heuristic (squared PDFs) to robustly balance between BRDF and guiding-based sampling. This is directly drawn from Vorba’s approach to reduce variance from mismatched PDFs. In line with Vorba's recommendations, the guiding structure is updated using scalar radiance values (e.g., luminance or energy) rather than full RGB vectors, to improve the stability of the learned distribution and reduce color noise. To improve stability and reduce color noise, only scalar radiance (luminance) is used for learning rather than RGB vectors, following Vorba's advice. Additionally, path guiding files were implemented, which include core logic for direction sampling, PDF evaluation, and distribution updates based on hit position and incoming directions. The cornell box scene did seem to have some issues; I'm unsure if this was from editing the scene test file incorrectly somehow or something having to do with the way I implemented path guiding. The black squares on the ceiling might be due to some convergence issues.

This guiding data is stored in a regular 3D grid across the scene, with each cell containing bins that correspond to hemispherical directions aligned to surface normals. These bins accumulate radiance information over time and are used to sample new directions with higher probability toward more light-contributing regions. When the renderer needs to sample a direction, a probabilistic mixture of BRDF-based and guided directions is chosen, with each path contributing according to its weighted MIS contribution.

After the milestone, besides working on the work explained above, I strived for visual differences and I was able to lessen the amount of noise. Something else that seemed to have made a difference in the rendering was waiting until after the first diffuse bounce to update the guiding data to avoid bias from direct lighting. Extremely high radiance contributions were clamped to prevent instability in the learning process. This clamping helped avoid sharp outliers and allowed the guide to converge more smoothly. After lots of trial and error, the guiding data finally seemed to have stabilized.

I ended up using multiple scenes from the previous assignments and messing with lighting, emission, and many other things to test what brought out more improvements, compared running with the basline brdf+mis scenes. There did seem to be some difference between just using BRDF vs. using path guiding. Path guiding seemed to have brought out a more realistic look to the scenes like the dragon. The lighting looked much more converged to its true self. When using the same amount of samples per pixel in the images above, we can see that path guiding gives smoother, less noisy images. This is due to the guiding structure focusing sampling efforts in directions that historically carried more energy, rather than relying on uniform BRDF models alone. This is also due to the advanced convergence that path guiding allows for. I also found it interesting how gamma correction interacted with path guiding since Vorba did not use it. With path guiding, gamma seemed to have shown more depth and look more realitic due to better convergence and better sampling compared to just brdf. 

Overall, at times, I let the project become 'inspired' by Vorba rather than being so strict. This was due to time limitations and me trying to tie in path guiding to what we have learned and implemented throughout the quarter. Vorba did speak of multiple state-of-the-art complex methods and was a bit vague on them at times but the work from this course did seem to fill in some blanks or misundertsandings from my end. In that sense, this implementation serves as a more practical, minimal version of Vorba's framework, omitting things like hierarchy or learning in favor of simplicity and stability. This just means that my path guiding might be a bit of a basic version of what Vorba was planning on doing next in terms of them trying out those state of the art methods that were new to them and path guiding at the time.

I unfortunatly did not have time to revisit Muller’s methods for path guiding as much as I'd hoped, however, the black and brown image below in the bloopers(middle top row) shows my partial attempt at it. I'm unsure if this meant that Vorba's method is supperior in terms of path guiding or if I was missing something to really show what Muller strived towards. I beleive that the brown in the Muller images I got were the product of the colors mixing together and just not converging correctly. 


--------------------------------------------------------------------------------------------------

## **Bloopers!**

Here are some happy accidents from previous progressions throughout the project.

These might have been the products of attempting more abstarct modern importance sampling strategy for lighting and BRDF, convergence did not go as planned, or reaching that point in the milestone where I seemed to have finally gotten somewhere with path guiding.

<p>
  <img src="https://github.com/user-attachments/assets/037979bc-fb02-4f44-a29d-ce766be54951" alt="cornellPG1 2" width="240">
  <img src="https://github.com/user-attachments/assets/fb4e57a9-684c-4c50-89f8-d007a7342a9a" alt="cornellPG1" width="240">
  <img src="https://github.com/user-attachments/assets/b852f76a-7ead-49b3-a9bd-8b0c4595b365" alt="cornellGuided" width="240">
</p>

<p>
  <img src="https://github.com/user-attachments/assets/35db7ffb-1180-41b8-bea8-794e468e5ef1" alt="cornellPG1 8" width="240">
  <img src="https://github.com/user-attachments/assets/2bd23f05-be67-4079-bdc1-03e8ca385cf4" alt="cornellPG1 6" width="240">
</p>

--------------------------------------------------------------------------------------------------


## **Resources:**
- Vorba (2017): [https://dl.acm.org/doi/abs/10.1145/2601097.2601203](https://dl.acm.org/doi/abs/10.1145/2601097.2601203)
- CSE 168 material: [https://cseweb.ucsd.edu/~viscomp/classes/cse168/sp25/schedule.html](https://cseweb.ucsd.edu/~viscomp/classes/cse168/sp25/schedule.html)
