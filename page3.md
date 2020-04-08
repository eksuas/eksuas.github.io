# Transformations, Instancing, Multisampling and the Distribution Ray Tracing

This section includes the experiences of implementing object transformations, instancing, multisampling and distribution ray tracing. The depth of field, Glossy reflections (imperfect mirrors) and translation motion blur is added to our ray tracing model.

# 1. Transformations

## Input
So, for new features new elements come to the input XML file. Transformations are given under the attribute same name. It can be scaling, translation or rotation. The first two ones are only 3D vectors, but rotation includes angle in degree and rotation vector as 3D vector, respectively. Any kind of object can be transformed via <Transformations> list under this attribute. These transformations are applied in the given order.

```markdown  
<Scene>
    <Transformations>
        <Translation id="1">0 -6 0</Translation>
        <Scaling id="1">3 0.1 0.1</Scaling>
        <Rotation id="2">0 0 1 0</Rotation>
    </Transformations>

    <Objects>
        <Mesh id="1">
            <Transformations>r1</Transformations>
        </Mesh>
        <Triangle id="1">
            <Transformations>s1</Transformations>
        </Triangle>
    </Objects>
</Scene>
```

## Code Design
I have used GLM library for all transformation and vector-matrix multiplications. Until this section, I have used my own vec3 class and its methods. Thus, I renewed the code pretty much by replacing all vectors.

Transformations are added to the Scene class as a list of string where the first character indicates the type of transformation and the remaining number is the identification number of the transformation. In order to speed the process, all matrix multiplications are done in preprocess and saved as a localMatrix and inverseLocalMatrix element to the Object class.  

## Algorithm
When implementing transformation, I transformed the ray by the inverse transformation matrix of the current object instead of transforming the objects. In other words, coming rays are firstly inversely transformed and then processed in the previous intersection tests of objects.

I plan to use the same things for Bounding Boxes. But, in preprocessing, bounding boxes are combined to a new bounding box. Thus, they must be in the same space to be added properly. Because of this reason, unfortunately, I could not apply the same methods for bounding boxes. In preprocessing, bounding boxes are created from the points of transformed objects and then, in intersection tests, the world ray is directly tested with them.

```markdown
Class Object
function intersect(ray, origin):
1. ray <- inverseLocalMatrix * ray;
2. origin = inverseLocalMatrix * origin;
3. apply remaining (in the previous part) intersection tests
```

## Implementation Process

Firstly, I planned to generate the following scene.

<p align="left"><img width="410" src="results/hw3/spheres_dof.png"></p>

Of course, not everything was as easy as described above :D When I thought that I was doing everything perfectly as described above, I got the following result (left image).

<p float="left">
  <img src="results/hw3/process/v1_spheres_dof_ray_transformed.png" width="410" />
  <img src="results/hw3/process/v2_spheres_dof_after_transformed_ray_normalized.png" width="410" />
</p>

I realized that I forgot to normalize ray directions after these transformations. After that I got the result above (right). Now, the problem seemed to be related to shading because light directions don't look correct and the scene is too bright. I checked the shading function and realized that all shading operations are done via non-transformed ray. After coming from the intersection test, if the ray is intersected with an object, ray should be inversely transformed again for remaining shading operations. After fixing it, the results would be below (left).

<p float="left">
  <img src="results/hw3/process/v3_spheres_dof_shading_done_with_transformed_ray.png" width="410" />
  <img src="results/hw3/process/v4_spheres_dof_light_transformed.png" width="410" />
</p>

It is clearly seen that the spotlight is not in the correct direction because we need to transform the light direction to the inverse local space of the object, as well. After fixing related operations on shading function, I got the result in the above figure (right). But, there are some problems still. Shadows do not look like correctly. Yes, we have to re-transform intersection ray (with localMatrix of the object) to be sure that it is ready to intersect with other objects for shadow testing. The result can be seen in the following figure (left).

<p float="left">
  <img src="results/hw3/process/v5_spheres_dof_inversed_ray_send_to_shadow_test.png" width="410" />
  <img src="results/hw3/process/v6_spheres_dof_intensity_divided_d_world.png" width="410" />
</p>

The problematic intensity is divided by the correct distance between light source and hit point (in shading function). In addition, obviously, mesh normal vectors have some problems. After fixing the calculation of normal vectors, I got the correctly transformed result in the below.

<p align="left"><img width="410" src="results/hw3/process/v7_spheres_dof_mesh_normal_fixed.png.png"></p>

