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

Since Vorba's path guiding is meant to work better with emissive shapes, emissive white spheres were added in some of the scenes. This can be seen in the middle of the images like the cornell sphere scenes.

## **Image Results**

<h3 style="text-align: center;">BRDF(left) vs. Path Guiding(right)</h3> 

<p>
  <img src="https://github.com/user-attachments/assets/f696ee2c-dcd0-4821-9fcc-27f724be24fd" alt="cornellPG1 5" width="350">
  <img src="https://github.com/user-attachments/assets/2bd23f05-be67-4079-bdc1-03e8ca385cf4" alt="cornellPG1 6" width="350">
</p>



<p>
  <img src="https://github.com/user-attachments/assets/0aaa5f69-94f4-4df4-a7b5-003b2cffa3f6" alt="cornellPG2" width="350">
  <img src="https://github.com/user-attachments/assets/6fceacba-079f-4f33-8170-47bc4b032828" alt="cornellPG2 2" width="350">
</p>



## **Within the Code**

Vorba’s 2017 path guiding is meant to adaptively learn where to send rays based on previously gathered radiance information. This approach targets improvements in convergence, especially in scenes with complex indirect lighting.	

Thus, the goal here was for the images above to show the interactions of integrated spatial-directional data structure that store radiance information throughout the scene. This structure maps position and surface normal to discretized directional bins, which is then sampled during rendering to inform future ray directions. Sampling is implemented to blend BRDF-based directions with the learned guiding distribution. Multiple Importance Sampling (MIS) waas also updated so that it uses the power heuristic (squared PDFs) to robustly balance between BRDF and guiding-based sampling. This is directly drawn from Vorba’s approach to reduce variance from mismatched PDFs. In line with Vorba's recommendations, the guiding structure is updated using scalar radiance values (e.g., luminance or energy) rather than full RGB vectors, to improve the stability of the learned distribution and reduce color noise. Additionally, path guiding files were implemented, which include core logic for direction sampling, PDF evaluation, and distribution updates based on hit position and incoming directions.


## **Next Steps**

For my next steps, I plan on improving the integrator I currently have. I plan to lessen the amount of noise and really ensure I have a well working path guiding code to make an even more significant difference in the final images. To do this, I plan to improve the current integrator by reducing noise and ensuring that the guiding data is stable and used effectively. To do this, I will delay guiding updates until after the first diffuse bounce to avoid bias from direct lighting, and clamp extremely high radiance contributions to prevent instability in the learning process. I will also refine the discretization parameters (number of bins, resolution) and test for convergence quality across different scenes. If time permits, I can also experiment with adaptively allocating more resolution (spatial or directional) to regions with higher variance, as suggested in Vorba’s paper. I also plan on modifying my scene test files in order to accommodate for anything that path guiding works well with. If time permits, I might even try to create more interesting or try to use .obj files somehow in an attempt to highlight what path guiding can really do. Previously, I also tried out some of Muller’s methods for path guiding but that led me to dark black and bronze looking images. If I have extra time, I could revisit Muller’s paper(s) and add some ideas into my integrator. I could even look at Vorba’s older papers on path guiding and see if I can use those methods there to improve anything. Lastly, I will need to continue to work on updating my website so that it reflects the final result of this project.










--------------------------------------------------------------------------------------------------

<h2 style="text-align: center;"><strong>Bloopers!</strong></h2>

Here are some happy accidents from previous progressions throughout the project.

These might have been the products of attempting more abstarct modern importance sampling strategy for lighting and BRDF or reaching that point in the milestone where I seemed to have finally gotten somewhere with path guiding.

<p>
  <img src="https://github.com/user-attachments/assets/037979bc-fb02-4f44-a29d-ce766be54951" alt="cornellPG1 2" width="240">
  <img src="https://github.com/user-attachments/assets/fb4e57a9-684c-4c50-89f8-d007a7342a9a" alt="cornellPG1" width="240">
  <img src="https://github.com/user-attachments/assets/2bd23f05-be67-4079-bdc1-03e8ca385cf4" alt="cornellPG1 6" width="240">
</p>

--------------------------------------------------------------------------------------------------


## **Resources:**
- Vorba (2017): [https://dl.acm.org/doi/abs/10.1145/2601097.2601203](https://dl.acm.org/doi/abs/10.1145/2601097.2601203)
- CSE 168 material: [https://cseweb.ucsd.edu/~viscomp/classes/cse168/sp25/schedule.html](https://cseweb.ucsd.edu/~viscomp/classes/cse168/sp25/schedule.html)
