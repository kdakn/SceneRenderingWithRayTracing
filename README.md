## Table of Contents
1. [Part 1: Introduction](#part1-introduction)
2. [Part 2: Shadows](#part2-shadows)
3. [Part 3](#part3)
   1. [Diffuse Component – Lambertian Shading](#part31-diffuse-component--lambertian-shading)
   2. [Specular Component – Blinn Phong Shading](#part32-specular-component--blinn-phong-shading)
4. [Part 4: Ambient Light](#part4-ambient-light)
5. [Part 5: Reflections](#part5-reflections)
6. [Part 6: Fresnel Equation](#part6-fresnel-equation)
7. [Part 7: Transmission](#part7-transmission)

## Part 1: Introduction
This project was developed by using advanced rendering techniques, such as shadow generation using the Ray Tracing Algorithm, Lambertian and Blinn-Phong Shadings, and the incorporation of ambient light to improve realism of the scene.

## Part 2: Shadows
In the initial phase of the project, implemented the "The Ray Tracing Algorithm" to generate shadows for objects in the scene after the rendering process. The algorithm involves shooting rays from all pixels of the object to the light source. If these rays intersect with any object in the scene, the corresponding pixels are considered as "black."

The key steps in coding this process included:
- Calculating the light vector direction from the intersected object to the light source. With this formula from the code: "𝑙𝑖𝑔ℎ𝑡_𝑣𝑒𝑐 = 𝑙𝑖𝑔ℎ𝑡.𝑙𝑜𝑐𝑎𝑡𝑖𝑜𝑛 − ℎ𝑖𝑡_𝑙𝑜𝑐"
- After finding the light vector it was crucial to prevent “Spurious Self-Occlusion” because in some rare cases, the ray can be re-intersected with the same object. To do this the light vector position must be shifted with respect to the “epsilon” value. So,  shifted the light vector from the hitting location. Obtained this functionality with the code: "𝑛𝑒𝑤_𝑜𝑟𝑖𝑔 = ℎ𝑖𝑡_𝑙𝑜𝑐 + 𝑒𝑝𝑠 ∗ 𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟" where eps is e-3.
- Finally, to test if the ray shot to the light source intersected with any of the objects in the scene the “ray_cast()” function has been used. The code can be observed with: "ℎ𝑎𝑠_𝑙𝑖𝑔ℎ𝑡_ℎ𝑖𝑡,_,_,_,_,_ = 𝑟𝑎𝑦_𝑐𝑎𝑠𝑡(𝑠𝑐𝑒𝑛𝑒,𝑛𝑒𝑤_𝑜𝑟𝑖𝑔,𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟)"

The resulting rendered image is shown in [Figure 1](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint1.png).

## Part 3
The second part of the project consisted of finding both the "Diffuse Component" (Lambertian Shading) and the "Specular Component" (Blinn-Phong Shading). These components were calculated when there was no intersection with any object in the scene along the light vector.

### Part 3.1: Diffuse Component – Lambertian Shading
The Diffuse Component, obtained from Lambertian Shading, produces matte images without light reflections. This component is calculated based on the object's color (diffuse coefficient), light intensity, and the dot product of the normal and light vectors. Mentioned formula to obtatin this image is: 
- "L = 𝑘𝑑 ∗ 𝐼 ∗ max(0,n · l)" and the code for this was: "𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑚𝑝𝑜𝑛𝑒𝑛𝑡 = 𝑛𝑝.𝑎𝑟𝑟𝑎𝑦(𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑙𝑜𝑟) ∗ 𝐼_𝑙𝑖𝑔ℎ𝑡 ∗( 𝑛𝑝.𝑑𝑜𝑡(𝑙𝑖𝑔ℎ𝑡_𝑑𝑖𝑟,ℎ𝑖𝑡_𝑛𝑜𝑟𝑚))". 
- After applying new color values of the pixels of objects are added to to the final pixel color: "𝑐𝑜𝑙𝑜𝑟 += 𝑛𝑝.𝑎𝑟𝑟𝑎𝑦(𝑑𝑖𝑓𝑓𝑢𝑠𝑒_𝑐𝑜𝑚𝑝𝑜𝑛𝑒𝑛𝑡)"
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
The rendered output of this complete model is Figure 6. As can be observed some of the shadows are less dense (for example shadows that monkey created).

The rendered output of the complete shading model is presented in [Figure 6](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/bf554251b051e0741c840ab07af992f44065000f/renders_for_readme/checkpoint3_ambientshading_power200.png).

## Part 5: Reflections
I applied the “Recursive Ray Tracing Algorithm” to
obtain “Reflections”. It is the technique that simulates the reflection of light on surfaces to create more realistic
images. To explain in detail, recursive ray tracing is a rendering technique used for computing
specular reflections, for example, those seen in mirrors or glossy surfaces. Also, this technique
is crucial for simulating dielectric materials (diamonds, glass, etc.).
When light travels from one medium to another with a different refractive index, a portion of it is transmitted. The
hemisphere (see on figure 7) reflects the entire scene, it seems more realistic and visually appealing element to
the rendering. My code, starts with a depth check, ensuring that the recursive process doesn't
continue indefinitely. If the depth is greater than zero, the reflection calculations proceed. A
reflected ray which showed 𝐷reflect is generated from the current intersection point. The
direction of reflection is calculated using the formula like in the book where D is the is the
incident ray direction and N is the surface normal.
D_reflect = ray_dir - Vector(2 * np.dot(ray_dir, hit_norm) * hit_norm)
Calculation is;
𝐷reflect = d-2(d.n)n where:
- r: Reflected vector.
- d: The incident vector, representing the direction of incoming light or any other vector.
- n: Normal vector to the surface.

The reflected ray is then recursively traced using the function RT_trace_ray. This
involves extending the ray in the reflection direction and calculating its color
contribution 𝐿reflect based on its interactions with the scene.
reflect_color = RT_trace_ray(scene, hit_loc + eps * D_reflect, D_reflect, lights, depth - 1)

The color contribution from the reflection is adjusted by multiplying it with the
reflectivity factor 𝑲𝒓 is to determine the strength of the reflection.

The adjusted color contribution is added to the original pixel color, contributing to the
final color of the pixel in the rendered image. Similar to the treatment of shadow ray
casting, self-occlusion is considered to prevent reflections from being inaccurately
occluded by the same object, ensuring a more accurate representation.

Finally, after rendered the scene I reached this image in shown in [Figure 7](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/checkpoint4_reflection.png)

## Part 6: Fresnel Equation
To enhance the quality of the renders I implemented the Fresnel
reflections. Fresnel reflection is a phenomenon that accounts for the varying reflectivity of
surfaces based on the viewing angle which is called the "Grazing
Angle". If viewing angle is same as the grazing angle then, the reflection should be stronger.

In this section for my code introduce a conditional check for the use of Fresnel reflection. If
Fresnel is not enabled, the reflectivity is obtained directly from the material properties.
However, when Fresnel is active (if mat.use_fresnel), I proceed to calculate the reflectivity
using Schlick’s approximation.

To ensure accurate calculations, both the view vector and the surface normal vector are
normalized. The incident angle (θ) is calculated using the dot product between the normalized
surface normal and view vectors.

The code then computes the Fresnel reflectivity (R0) and and utilizes it in the Schlick’s
approximation formula to determine the overall reflectivity (𝐾r).
reflectivity = R_0 + (1 - R_0) * (1 - np.cos(incident_angle))**5

The Fresnel effect is modeled through the term (1−R0) (1 − cos(𝜃))^5, where θ is the incident
angle. Finally, after rendered I reached this image in [Figure 8](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/checkpoint5_fresnel.png).

For example you can see some additional reflections on the black trapezoid also in [Figure 9](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/Screenshot%202024-02-20%20162936.png).

## Part 7: Transmission
To make pictures look more real, I applied refractions to some objects in the scene.
I achieved them using a technique called transmission. In simple terms,
transmission in ray tracing is how light behaves when it goes through see-through stuff like
glass or water.
When I added transmission to our ray tracing process, it let me to simulate how light moves
through transparent materials.

My code:

Firstly checks if the material has transparency (if mat.transmission > 0) and whether the depth
of recursion is greater than zero before performing transmission calculations.

Assigns refractive indices n_1 and n_2 based on the media through which the ray is passing.
n_1 is the refractive index of air (set to 1), and n_2 is the refractive index of the material
obtained from mat.ior. Calculates an intermediate vector here by scaling the ray direction.
This vector is used in the subsequent calculations. Here is the sample code lines:
- n_1 = 1
- n_2 = mat.ior
- here= ray_dir*(n_1/n_2).

The code computes the transmitted ray direction (D_transmit) using Snell's Law, which describes how
light bends when passing through different objects. The formula involves the incident ray
direction (ray_dir), surface normal (hit_norm), and refractive indices. I calculated
D_transmit with the formula which is: [Figure 10] (https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/Screenshot%202024-02-20%20163528.png).

This equation models situations of [Figure 11](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/Screenshot%202024-02-20%20164118.png) and [Figure 12](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/Screenshot%202024-02-20%20164127.png)
- The code I wrote for transmission:
- if mat.transmission > 0:
- n_1 = 1
- n_2 = mat.ior
- here= ray_dir*(n_1/n_2)
- D_transmit = ray_dir*(n_1/n_2)-hit_norm*((n_1/n_2)*np.dot(ray_dir,hit_norm)+sqrt(1-((n_1/n_2)**2)*(1-(np.dot(ray_dir,hit_norm))**2)))
- L_transmit = RT_trace_ray(scene, hit_loc + eps * D_transmit, D_transmit, lights, depth - 1)
- color += L_transmit * (1 - reflectivity)

Then, it recursively traces the transmitted ray using the RT_trace_ray function. This simulates the
continuation of the ray through the transparent material, and the return value is stored as
L_transmit. Also, calculation for L_transmit is:
- L_transmit = RT_trace_ray(scene, hit_loc + eps * D_transmit, D_transmit, lights, depth - 1).

Finally, after rendered we reached this image in [Figure 13](https://github.com/kdakn/SceneRenderingWithRayTracing/blob/main/renders_for_readme/checkpoint6.png).


