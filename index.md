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

Gamma was ignored in these images since they were not within Vorba's path guiding and it seemed to cause some conflict when interacting with the path guiding changes. 

To view more drastic differences in the images, I messed with the quad lights a bit. This is to show how Vorba's path guiding samples bright indirect bounces more accuratly, while the baseline BRDF tends to undersample.

## **Image Results**

<h3 style="text-align: center;">BRDF(left) vs. Path Guiding(right)</h3> 

<p>
  <img src="https://github.com/user-attachments/assets/e88a37f7-abab-48ad-851e-eae89b8ea920" alt="cornellPG5 2" width="350">
  <img src="https://github.com/user-attachments/assets/910fc61d-d9b3-497a-b581-4b2eecda918c" alt="cornellPG5" width="350">
</p>



<p>
  <img src="https://github.com/user-attachments/assets/f696ee2c-dcd0-4821-9fcc-27f724be24fd" alt="cornellPG1 5" width="350">
  <img src="https://github.com/user-attachments/assets/2bd23f05-be67-4079-bdc1-03e8ca385cf4" alt="cornellPG1 6" width="350">
</p>



<p>
  <img src="https://github.com/user-attachments/assets/0aaa5f69-94f4-4df4-a7b5-003b2cffa3f6" alt="cornellPG2" width="350">
  <img src="https://github.com/user-attachments/assets/6fceacba-079f-4f33-8170-47bc4b032828" alt="cornellPG2 2" width="350">
</p>


<p>
  <img src="https://github.com/user-attachments/assets/0aaa5f69-94f4-4df4-a7b5-003b2cffa3f6" alt="cornellPG2" width="350">
  <img src="https://github.com/user-attachments/assets/4c229f30-99f8-4599-b81c-5aa604aac4c4" alt="cornellPG2 3" width="350">
</p>


## **What was done to achieve this?**

Vorba’s 2017 path guiding is meant to adaptively learn where to send rays based on previously gathered radiance information. This approach targets improvements in convergence, especially in scenes with complex indirect lighting.	

Thus, the goal here was for the images above to show the interactions of integrated spatial-directional data structure that store radiance information throughout the scene. This structure maps position and surface normal to discretized directional bins, which is then sampled during rendering to inform future ray directions. Sampling is implemented to blend BRDF-based directions with the learned guiding distribution. Multiple Importance Sampling (MIS) was also updated so that it uses the power heuristic (squared PDFs) to robustly balance between BRDF and guiding-based sampling. This is directly drawn from Vorba’s approach to reduce variance from mismatched PDFs. In line with Vorba's recommendations, the guiding structure is updated using scalar radiance values (e.g., luminance or energy) rather than full RGB vectors, to improve the stability of the learned distribution and reduce color noise. Additionally, path guiding files were implemented, which include core logic for direction sampling, PDF evaluation, and distribution updates based on hit position and incoming directions.

After the milestone, I was able to lessen the amount of noise. Something else that seemed to have made a difference in the rendering was delaying guiding updates until after the first diffuse bounce to avoid bias from direct lighting. Extremely high radiance contributions were clamped to prevent instability in the learning process. After lots of trial and error, the guiding data finally seemed to have stabilized.

I ended up using multiple scenes from the previous assignments and messing with lighting, emission, and many other things to test what brought out more improvements, compared running with the basline brdf+mis scenes. There did seem to be some difference between just using BRDF vs. using path guiding. Path guiding seemed to have brought out a more realistic look to the scenes like the dragon. The lighting looked much more converged to its true self. 

Overall, at times, I let the project become 'inspired' by Vorba rather than being so strict. This was due to time limitations and me trying to tie in path guiding to what we have learned and implemented throughout the quarter. Vorba did speak of multiple state-of-the-art complex methods and was a bit vague on them at times but the work from this course did seem to fill in some blanks or misundertsandings from my end. This just means that my path guiding might be a bit of a absic version of what Vorba was planning on doing next in terms of them trying out those state of the art methods that were new to them and path guiding at the time.

I unfortunatly did not have time to revisit Muller’s methods for path guiding too much, however, the black and brown image below in the bloopers(middle top row) shows my partial attempt at it. I'm unsure if this meant that Vorba's method is supperior in terms of path guiding or if I was missing something to really show what Muller strived towards. I beleive that the brown in the Muller images I got were the product of the colors mixing together and just not converging correctly. 


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
