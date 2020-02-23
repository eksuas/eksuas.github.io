# 1. Basic Ray Tracer

Our very beginning ray tracer will support ray-triangle and ray-sphere intersections with a simple perspective camera model and simple shading models. These include diffuse shading, specular (Blinn-Phong) shading, and ambient shading.

## Input
I will employ custom XML files that define the camera and scene properties as an input of the Ray Tracer. Example XML file:
```markdown
<Scene>
    <BackgroundColor>0 0 0</BackgroundColor>

    <ShadowRayEpsilon>1e-3</ShadowRayEpsilon>

    <IntersectionTestEpsilon>1e-6</IntersectionTestEpsilon>

    <Cameras>
        <Camera id="1">
            <Position>0 0 0</Position>
            <Gaze>0 0 -1</Gaze>
            <Up>0 1 0</Up>
            <NearPlane>-1 1 -1 1</NearPlane>
            <NearDistance>1</NearDistance>
            <ImageResolution>800 800</ImageResolution>
            <ImageName>simple.png</ImageName>
        </Camera>
    </Cameras>

    <Lights>
        <AmbientLight>25 25 25</AmbientLight>
        <PointLight id="1">
            <Position>0 0 0 </Position>
            <Intensity>1000 1000 1000</Intensity>
        </PointLight>
    </Lights>

    <Materials>
        <Material id="1">
            <AmbientReflectance>1 1 1</AmbientReflectance>
            <DiffuseReflectance>1 1 1</DiffuseReflectance>
            <SpecularReflectance>1 1 1</SpecularReflectance>
            <PhongExponent>1</PhongExponent>
        </Material>
    </Materials>

    <VertexData>
        -0.5 0.5 -2
        -0.5 -0.5 -2
        0.5 -0.5 -2
        0.5 0.5 -2
        0.75 0.75 -2
        1 0.75 -2
        0.875 1 -2
        -0.875 1 -2
    </VertexData>

    <Objects>
        <Mesh id="1">
            <Material>1</Material>
            <Faces>
                3 1 2
                1 3 4
            </Faces>
        </Mesh>
        <Triangle id="1">
            <Material>1</Material>
            <Indices>
                5 6 7
            </Indices>
        </Triangle>
        <Sphere id="1">
            <Material>1</Material>
            <Center>8</Center>
            <Radius>0.3</Radius>
        </Sphere>
    </Objects>
</Scene>
```

The XML file is generally self-explanatory. But some points may not be clear: the "Mesh" element defines a collection of triangles using an index-based representation. Indices are 1-based (index 1 represents the first vertex in your vertex list). In order to parse XML file, I use TinyXML lib that parses XML file as a tree and you can reach attributes by preserving nested relationships.

TinyXML can be install with this command:
`sudo apt-get install libtinyxml-dev`

## Code Design
Most probably, I spent the most time on code design. It is very important to define your class and relationship well before coding the tracer. I follow the nested XML features and define classes for almost each attributes.

## Algorithm
```markdown
function main:
1. parse XML file and load object
2. create scene models
3. preprocess (computing "normal" vectors of triangles)
4. for each cameras:
5.     arrange camera perspective
6.     initialize image buffer
7.     for each pixels:
8.         create a ray from camera position to image plane
9.         buffer[pixel] <- rayTracer(ray)
10.    write buffer to PNG file
```

Ray tracer function traces a ray by checking any intersection. If there is a intersection, it computes the closest hit point and the closest distance and normal vector of the intersected object (if intersected object is a sphere) and it finally returns the color of pixel.
```markdown
function rayTracer(ray):
1. find the closest intersected object and distance to it
2. if there is intersection:
3.     compute hit_point
4.     compute normal if object is a sphere
5.     color <- shading(intersected_object, ray, hit_point, normal)
6. else:
6.     color <- zero
7. return color
```

Shading function calculates basic shadings.
```markdown
function shading(intersected_object, ray, hit_point, normal):
1. get the material information of the intersected_object
2. color <- ambient_shading
3. get unit ray vector
4. for each lights:
5.     compute the distance between light position and hit_point
6.     get unit light vector
7.     compute shadow ray origin
8.     check for all object for possible shadow situation
9.     if not isShadow(intersected_object, shadow_ray):
10.        compute intensity at hit_point
11.        color <- color + diffuse_shading
12.        color <- color + specular_shading
13. return color
```

The isShadow function checks if the shadow_ray intersects with any object. The important point here is the shadow_ray is not a unit vector. Its direction is hit_point to the light_position. By the way, we know that the larger intersection distance than one will not cast shadow on this intersected object. Until figure out this situation, I spent much time on this problem :D
Lets look at pseudo code:
```markdown
function isShadow(intersected_object, shadow_ray):
1. for each object:
2.     if the object is not intersected_object:
3.         get the closest distance
4.         if distance < 1 and distance > intersectionTestEpsilon:
5.             return true
6. return false
```
## Results
Lets look at the results of my implementation.
![Image](results/hw1/simple.png)
![Image](results/hw1/cornellbox.png)
![Image](results/hw1/spheres.png)
![Image](results/hw1/bunny.png)
![Image](results/hw1/scienceTree.png)
