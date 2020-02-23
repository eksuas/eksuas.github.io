## 1. Basic Ray Tracer

Our very beginning ray tracer will support ray-triangle and ray-sphere intersections with a simple perspective camera model and simple shading models. These include diffuse shading, specular (Blinn-Phong) shading, and ambient shading.

### Input
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

### Code Design
Most probably, I spent the most time on code design. It is very important to define your class and relationship well before coding the tracer. I follow the nested XML features and define classes for almost each attributes.

### Main algorithm

```markdown

```
