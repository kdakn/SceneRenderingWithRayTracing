### Table of Contents
1. [Part 1: Shadows](#part1-shadows)
2. [Part 2](#part2)
   1. [Diffuse Component – Lambertian Shading](#part21-diffuse-component--lambertian-shading)
   2. [Specular Component – Blinn Phong Shading](#part22-specular-component--blinn-phong-shading)
3. [Part 3: Ambient Light](#part3-ambient-light)
4. [References](#references)

### Part 1: Shadows
In the initial phase of the project, we implemented "The Ray Tracing Algorithm" to generate shadows for objects in the scene after the rendering process. The algorithm involves shooting rays from all pixels of the object to the light source. If these rays intersect with any object in the scene, the corresponding pixels are considered as "black."

The key steps in coding this process included:
- Calculating the light vector direction from the intersected object to the light source.
- Normalizing the vector to prevent spurious self-occlusion.
- Shifting the light vector position with respect to an epsilon value to avoid re-intersection with the same object.
- Using the "ray_cast()" function to test if the ray to the light source intersects with any objects in the scene.

### Part 2
The second part of the project focused on finding both the "Diffuse Component" (Lambertian Shading) and the "Specular Component" (Blinn-Phong Shading). These components were calculated when there was no intersection with any object in the scene along the light vector.

#### Part 2.1: Diffuse Component – Lambertian Shading
The Diffuse Component, obtained from Lambertian Shading, produces matte images without light reflections. This component is calculated based on the object's color (diffuse coefficient), light intensity, and the dot product of the normal and light vectors. The rendered image is shown in Figure 3.

#### Part 2.2: Specular Component – Blinn Phong Shading
To achieve more realistic images with reflections, we implemented the Blinn-Phong Shading model. This complex algorithm produces reflections based on the light source and viewing location. The specular component is calculated using the formula: 𝑘𝑠∗ 𝐼∗ 𝑚𝑎𝑥(0,𝑛 · ℎ)𝑝.


### Part 3: Ambient Light
To simulate indirect lighting and reduce the intensity of shadows, an "Ambient Lighting" term was introduced. This constant is added to the shading model to make shadows less black. The complete shading model is given by:
𝐿𝑐𝑜𝑚𝑝𝑙𝑒𝑡𝑒 = 𝑘𝑎 ∗ 𝐼𝑎 + 𝑘𝑑 ∗ 𝐼 ∗ max(0,n · l)+ 𝑘𝑠 ∗ 𝐼 ∗ 𝑚𝑎𝑥(0,𝑛 · ℎ)^𝑝

