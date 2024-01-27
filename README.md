## Table of Contents
1. [Introduction](#part1-introduction)
2. [Part 2: Shadows](#part2-shadows)
3. [Part 3](#part3)
   1. [Diffuse Component – Lambertian Shading](#part31-diffuse-component--lambertian-shading)
   2. [Specular Component – Blinn Phong Shading](#part32-specular-component--blinn-phong-shading)
4. [Part 4: Ambient Light](#part4-ambient-light)

## Part 1: Introduction
This project was developed by using advanced rendering techniques, such as shadow generation using the Ray Tracing Algorithm, Lambertian and Blinn-Phong Shadings, and the incorporation of ambient light to improve realism of the scene.

## Part 2: Shadows
In the initial phase of the project, implemented the "The Ray Tracing Algorithm" to generate shadows for objects in the scene after the rendering process. The algorithm involves shooting rays from all pixels of the object to the light source. If these rays intersect with any object in the scene, the corresponding pixels are considered as "black."

The key steps in coding this process included:
- Calculating the light vector direction from the intersected object to the light source. With this formula from the code: "𝑙𝑖𝑔ℎ𝑡_𝑣𝑒𝑐 = 𝑙𝑖𝑔ℎ𝑡.𝑙𝑜𝑐𝑎𝑡𝑖𝑜𝑛 − ℎ𝑖𝑡_𝑙𝑜𝑐"
- After finding the light vector it was crucial to prevent “Spurious Self-Occlusion” because in some rare cases, the ray can be re-intersected with the same object. To do this the light vector position must be shifted with respect to the “epsilon” value. So,  shifted the light vector from the hitting location. Obtained this functionality with the code:
- "𝑛𝑒𝑤_𝑜𝑟𝑖𝑔 = ℎ𝑖𝑡_𝑙𝑜𝑐 + 𝑒𝑝𝑠 ∗ 𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟" where eps is e-3.
- Finally, to test if the ray shot to the light source intersected with any of the objects in the scene the “ray_cast()” function has been used. The code can be observed with:
- "ℎ𝑎𝑠_𝑙𝑖𝑔ℎ𝑡_ℎ𝑖𝑡,_,_,_,_,_ = 𝑟𝑎𝑦_𝑐𝑎𝑠𝑡(𝑠𝑐𝑒𝑛𝑒,𝑛𝑒𝑤_𝑜𝑟𝑖𝑔,𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟)"

The resulting rendered image is shown in [Figure 1](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint1.png).

## Part 3
The second part of the project consisted of finding both the "Diffuse Component" (Lambertian Shading) and the "Specular Component" (Blinn-Phong Shading). These components were calculated when there was no intersection with any object in the scene along the light vector.

### Part 3.1: Diffuse Component – Lambertian Shading
The Diffuse Component, obtained from Lambertian Shading, produces matte images without light reflections. This component is calculated based on the object's color (diffuse coefficient), light intensity, and the dot product of the normal and light vectors. Mentioned formula to obtatin this image is: 
- "L = 𝑘𝑑 ∗ 𝐼 ∗ max(0,n · l)" and the code for this was: 
- "𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑚𝑝𝑜𝑛𝑒𝑛𝑡 = 𝑛𝑝.𝑎𝑟𝑟𝑎𝑦(𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑙𝑜𝑟) ∗ 𝐼_𝑙𝑖𝑔ℎ𝑡 ∗( 𝑛𝑝.𝑑𝑜𝑡(𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟,ℎ𝑖𝑡_𝑛𝑜𝑟𝑚))". 
After applying new color values of the pixels of objects are added to to the final pixel color: 
- "𝑐𝑜𝑙𝑜𝑟 += 𝑛𝑝.𝑎𝑟𝑟𝑎𝑦(𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑚𝑝𝑜𝑛𝑒𝑛𝑡)"
The rendered image is shown in [Figure 2](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint2.1_diffuse.png).

### Part 3.2: Specular Component – Blinn Phong Shading
To obtain more realistic images, reflections are crucial. These reflections were obtained by using the “Blinn-Phong Shading” model. The Blinn-Phong Shading model is a more complex algorithm compared with the Lambertian Shading. It requires more computational power but produces more realistic images. It creates reflections with the help of the light source depending on the viewing location. So, it also uses the viewing vector. With the viewing vector and the normal vector, the “h” vector was founded and with the normal and h vectors, an angle was found. This angle is important to obtain different values of shininess. If the normal vector and h vector are the same, the most brightness is obtained from the object. In this part, it was tried to find the “Specular Component” which is a component that adds image shininess or reflections.
Blinn-Phong model creates reflections in terms of this formula: 
- "𝑘𝑠∗ 𝐼 ∗ 𝑚𝑎𝑥(0,𝑛 · ℎ)^𝑝" 
where ks: specular coefficient (or specular color),
I: intensity of the light, max(0, n.h): angle between n and h vectors and
p: Phong exponent (controls the shininess of the surface) or power value.
As code is too long to mention here, it can be accessed from the repo.

Different power values were tried to demonstrate the impact on the output image, as shown in [Figures 3](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint2.2_specular_power10.png), [4](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint2.2_specular_power100.png), and [5](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint2.2_specular_power1000.png). The viewing direction was considered, and the power value (p) was adjusted to see different results.

## Part 4: Ambient Light
With respect to models combined Lambertian and Blinn-Phong Shadings it was expected to see black pixels if there was intersection between the shadow rays and any of the objects that stand middle of the ray between the object and the light source (meaningly shadow). But in real life there are indirect lighting that illuminates these shadows not completely but in recognizable extend. So, indirect lightings were not be used, but some constant were added to models to make these shadows less black. This is called as “Ambient Lightning”.
This term is: 𝑘𝑎 𝐼𝑎 where ka: surface ambient coefficient and Ia is ambient light intensity. So, formula with a code to calculate Ambient lightning:
- "𝐼_𝑎𝑚𝑏𝑖𝑒𝑛𝑡 = 𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑙𝑜𝑟 ∗ 𝑉𝑒𝑐𝑡𝑜𝑟(𝑎𝑚𝑏𝑖𝑒𝑛𝑡_𝑐𝑜𝑙𝑜𝑟)"
- Add this calculated term to the global color variable: "𝑐𝑜𝑙𝑜𝑟 += 𝐼_𝑎𝑚𝑏𝑖𝑒𝑛𝑡"
So, a complete shading model was obtained with Part 2, Part 3 and Part 4: 
- "𝐿𝑐𝑜𝑚𝑝𝑙𝑒𝑡𝑒 = 𝑘𝑎 𝐼𝑎 + 𝑘𝑑∗I∗ max(0,n · l)+ 𝑘𝑠∗ 𝐼∗ 𝑚𝑎𝑥(0,𝑛 · ℎ)^𝑝"
The rendered output of this complete model is Figure 9. As can be observed some of the shadows are less dense (for example shadows that monkey created).

The rendered output of the complete shading model is presented in [Figure 6](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint3_ambientshading_power200.png).
