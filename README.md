# Advanced Ray Tracer

In computer graphics, ray tracing is a rendering technique for generating an image by tracing the path of light as pixels in an image plane and simulating the effects of its encounters with virtual objects. (from Wikipedia)

In this serial, I'll share my experiences while developing an Advanced Ray Tracer.

Sections:

1. [Basic Ray Tracer](page1.md) includes diffuse shading, specular (Blinn-Phong) shading, and ambient shading

2. [Reflection and Refraction with Ray Tracer Acceleration](page2.md) includes reflection and refraction shadings with Fresnel coefficient, Snell's and Beer's laws. In addition, ray tracer is accelerated via Bounding Volume Hierarchy (BVH) algorithm.

3. [Transformations, Instancing, Multisampling and Distribution Ray Tracing](page3.md) includes the experiences of implementing object transformations, instancing, multisampling and distribution ray tracing. The depth of field, Glossy reflections (imperfect mirrors) and translation motion blur is added to our ray tracing model.

4. [Texture Mapping, Perlin Noise, Normal and Bump Mapping](page4.md) includes the experiences of implementing the Texture Mapping, Normal and Bump Mapping. Smooth shading, Perlin Noise texture and Perlin Bump mapping is also implemented in this section.

My development environment is Ubuntu18.4

Required Libraries are libpng-dev, libtinyxml-dev and libglm-dev.
```markdown  
sudo apt-get update -y
sudo apt-get install libpng-dev
sudo apt-get install libtinyxml-dev
sudo apt-get install libglm-dev
```


