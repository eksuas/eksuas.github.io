# 1. Reflection and Refraction (recursive ray tracing)

In this section, I shared my experiences during implementing reflection and refraction shadings. This section includes Fresnel coefficient, Snell's and Beer's laws, as well.  

## Input
In order to see the reflection and refraction effects, we need new material types such as mirror, conductor and dielectrics and attributes such as mirror reflectance, reflection and refraction indices.
Reflection and refraction are recursive ray tracing features. To stop and yield a result, we need a recursion depth limit (MaxRecursionDepth).
For these purposes, the input XML files include new attributes. Some of new coming attributes are given below XML file:

```markdown  
<Scene>
    <MaxRecursionDepth>6</MaxRecursionDepth>
    <Material id="1" type="conductor">
        <AmbientReflectance>0 0 0</AmbientReflectance>
        <DiffuseReflectance>0 0 0</DiffuseReflectance>
        <SpecularReflectance>0 0 0</SpecularReflectance>
        <MirrorReflectance>1 0.86 0.57</MirrorReflectance>
        <RefractionIndex>0.370</RefractionIndex>
        <AbsorptionIndex>2.820</AbsorptionIndex>
    </Material>
    <Material id="2" type="dielectric">
        <AmbientReflectance>0 0 0</AmbientReflectance>
        <DiffuseReflectance>0 0 0</DiffuseReflectance>
        <SpecularReflectance>0 0 0</SpecularReflectance>
        <AbsorptionCoefficient>0.01 0.01 0.01</AbsorptionCoefficient>
        <RefractionIndex>1.55</RefractionIndex>
    </Material>
    <Material id="3" type="mirror">
        <AmbientReflectance> 1 1 1 </AmbientReflectance>
        <DiffuseReflectance> 0.2 0.2 0.2 </DiffuseReflectance>
        <SpecularReflectance> 0 0 0 </SpecularReflectance>
        <PhongExponent>1</PhongExponent>
        <MirrorReflectance> 1 1 1 </MirrorReflectance>
    </Material>
    <Objects>
        <Mesh id="1">
            <Material>1</Material>
            <Faces plyFile="ply/dragon_remeshed.ply" />
        </Mesh>
    </Objects>
</Scene>
```

In addition, this section include an implementation of ray tracing acceleration method. By this way, huge meshes can be traced. So, we need an huge meshes. But, as known, more complex meshes can be found in ply file format on web. So, our XML file includes ply filename as a mesh faces, as well. This file includes mesh vertices and faces with vertex indices. The normal vectors can be found in ply files, as well.

## Reading meshes from the PLY file
In order to read PLY files, I used hapPLY library. It is very easy and useful library. You just need to include header which can be found in [Github repo](https://travis-ci.com/nmwsharp/happly.svg?branch=master)

## Code Design
My code design is changed a little bit with new attributes. Material class gets new attributes such as material type, mirror reflectance, reflection and refraction indices etc.

## Algorithm
In reflection and refraction, light can pass through the object, a phenomenon we call transmission and they can reflect light at the same time.
<p align="left"><img src="results/hw2/example.png"></p>

```markdown
function reflection(ray, hit_point, normal):

```

```markdown
function refraction(ray, hit_point, normal, refractionInd):

```

One may ask how to know how much light is transmitted vs how much light is reflected? In order to decide it, I used the Fresnel effect. It is a coefficient for reflection and refraction calculations.

```markdown
function fresnel(ray, normal):

```

Therefore, the final color of hit point can be calculated according to the material type.

```markdown
function rayTracer(ray):

```

Note that the final refraction color is multiplied with transparency factor. It comes from the Beer's Law.

# 2. Acceleration Structure

I implemented Bounding Volume Hierarchy (BVH) algorithm to accelerate the tracing computation. It is a tree using to check an intersection of objects with the incident ray. In my implementation, I select the median object according to the split axis at each step. Then, I divided the object list from the median into new two nodes (or object list). Each node includes an object list and the bounding box of these objects.

## Code Design
I added new class, BoundingBox having its own intersection and union methods and Node class for nodes of the BVH tree. Each node has an object list, two child nodes (low and high), the bounding box and split axis id.

## Algorithm
```markdown
function constructBVH(object_list, split_axis):

```

## Results
Lets look at the results of my implementation.

### Spheres.xml
<p align="left"><img height="500" src="results/hw2/spheres.png"></p>

```markdown
Before acceleration: Real running time is 0m0,122s
After acceleration:
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### cornellbox_recursive.xml
<p align="left"><img height="500" src="results/hw2/cornellbox_recursive.png"></p>

```markdown
Before acceleration: Real running time is 0m0,232s
After acceleration:
XML file is parsed in 0 sec
Maximum BVH depth is 2
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### ScienceTree.xml
<p align="left"><img height="500" src="results/hw2/scienceTree.png"></p>

```markdown
Before acceleration: Real running time is 1m26,100s
After acceleration:
XML file is parsed in 0 sec
Maximum BVH depth is 2
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### Bunny.xml
<p align="left"><img height="500" src="results/hw2/bunny.png"></p>

```markdown
Before acceleration: Real running time is
```

### chinese_dragon.xml
<p align="left"><img height="500" src="results/hw2/chinese_dragon.png"></p>

```markdown
After acceleration:
XML file is parsed in 375 sec
Maximum BVH depth is 19
Preprocessing is finished in 1 sec
Scene is created in 2 sec
```

### other_dragon.xml
<p align="left"><img height="500" src="results/hw2/other_dragon.png"></p>

```markdown
After acceleration:
XML file is parsed in 1 sec
Maximum BVH depth is 19
Preprocessing is finished in 2 sec
Scene is created in 9 sec
```

The last two examples are more challenging examples that include many triangles and takes much more time. I could not get a result without BVH acceleration.
