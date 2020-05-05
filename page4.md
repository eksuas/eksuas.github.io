# Texture, Normal and Bump Mapping

This section includes the experiences of implementing the Texture Mapping, Normal and Bump Mapping. Smooth shading, Perlin Noise texture and Perlin Bump mapping is also implemented in this section.

In order to achieve these mapping, I firstly should have been able to read PNG and JPEG image files. We already have libPNG library to be write a scene as PNG file. In this section, I added JPEG library given in [this code archive](https://code.google.com/archive/p/jpgd/). I will not give details about the reading image files here to keep the blog short, but I have spent some hours to achieve it. Firstly, I used the libjpeg, but I could not manage it to get the pixel successfully. Then, I switched the JPGD given above.

# 1. Texture Mapping

## Input
All kind of textures are given under the <Textures> in the XML files. It has a list of images with their corresponding ID. Then, each texture map, <TextureMap>, will be given with map ID and type. Image type indicates the texture will be taken from the particular image file which is specified with its image ID at the list of images.

Texture can be added for several purposes (decalMode) such as blend_kd, replace_kd, replace_all, replace_background, replace_normal, bump_normal. The first four ones related to color, directly while the last two ones affects the object normal vectors. They will be explain the following sections. <Normalizer> field indicates the normalizing coefficient of the pixel colors. Generally, it will be 1 or 255 means use the RGB color directly or normalize it in the interval of 0-1, respectively. Its default value is 255 but it is expected to be 1 if the <decalMode> is replace_all or bump_normal.

Image pixels can be interpolated to the object in two ways which are bilinear or nearest interpolations. They will be explain the following part, as well.

If object covered by a texture is based on triangles, the corresponding texture coordinates of the vertices should be known. Thus, the XML file includes the corresponding texture coordinates under the <TexCoordData> field.

Once <Textures> list is given, object can have at most two textures (one for diffusion and one for normal mapping) from this list. These texture IDs are given in the <Textures> field of objects.

```markdown  
<Scene>
    <Textures>
        <Images>
            <Image id="1">textures/wall.png</Image>
        </Images>

        <TextureMap id="1" type="image">
            <ImageId>1</ImageId>
            <DecalMode>replace_kd</DecalMode>
            <Normalizer>255</Normalizer>
            <Interpolation>bilinear</Interpolation>
        </TextureMap>
    </Textures>

    <TexCoordData>
        1 1
        0 1
        ...
        1 0
    </TexCoordData>

    <Objects>
        <Mesh id="3">
            <Material>1</Material>
            <Textures>1</Textures>
            <Faces>
                13 9 14
                14 9 10
            </Faces>
        </Mesh>
    </Objects>
</Scene>
```

## Code Design
In this of improvement, I added many classes and new features. Thus, I have changed the design pretty much.
New class "TextureMap" and "Image" are implemented. These include the all new features given in the previous part. Object class will new features, as well. Each object can have a textureDiffuse and/or textureNormal. These are determined according to the decalMode of the corresponding texture of the object. It decalMode belongs to the first set of decalMode, it is set as a textureDiffuse, otherwise, it will be considered as a textureNormal.

```markdown
Class Image
    width
    height
    component
    data

Class TextureMap
    Image image
    mode  type
    mode  interpolation
    mode  decalMode
    float normalizer

    function init()
    function getColor(texCoord)

Class Object
    TextureMap* textureDiffuse
    TextureMap* textureNormal
    int         textureOffset
	int         vertexOffset

    function getTextureColor(pHit)
```

## Algorithm
Texture mapping is applied for triangles and spheres. To active it, we should firstly construct texture coordinates of the objects. The texture coordinates of the triangle can be computed by using the barycentric coefficients (alpha and beta) obtained from the object-ray intersection. For sphere, the UV coordinates of the hit point is employed.

Note that I assumed that all sphere is a unit sphere such that their radius is one to simplified the most of the calculations. For this assumption, I used the scaling transformation we add in the previous section. Be careful when applying this scaling transformation. Firstly, the sphere should be translate to the center, then we should apply scaling, finally, it should be re-translate to the its original location.

```markdown
Class Object
function init():
1. if object type is sphere:
2.     localMatrix = localMatrix * translate(center.position)
3.     localMatrix = localMatrix * scale(radius)
4.     localMatrix = localMatrix * translate(-center.position)

function getTextureColor(pHit):
1. if object type is triangle:
2.     texCoord = (1-alpha-beta) * v1.texCoord \
                + alpha * v2.texCoord \
                + beta * v3.texCoord
3. if object type is sphere:
4.     phi = atan2(normal.z, normal.x)
5.     theta = acos(normal.y, normal.x)
6.     texCoord = ((PI-phi)/(2*PI), theta/PI)
3.     return textureDiffuse->getColor(texCoord)
```

Once we get the corresponding texture coordinates, we can interpolate the image color to this coordinates as given below.

```markdown
Class TextureMap
function getColor(texCoord):
1. if interpolation is bilinear:
2.     return getColorBilinear(texCoord)
3. else:
4.     return getColorNearest(texCoord)

function getColorNearest(texCoord):
1. find the nearest pixel to the given texCoord
2. normalize the color acccoring to the Normalizer
3. return color

function getColorBilinear(texCoord):
1. find the nearest 4 pixels to the given texCoord
2. interpolate them linearly by using their distance to the given texCoord
3. normalize the color acccoring to the Normalizer
4. return color
```

## Implementation Process
I had some difficulties on the implementation, especially for spheres. Because my assumption on unit spheres, I had to transform them carefully as mentioned in the previous part.

Updated localMatrix (or object space matrix) is used to compute inverseLocalMatrix and the normalMatrix (as a inverse transformation of the local matrix).

I employed the normal vectors in the world space (multiplied with the Normal Matrix) to compute the texture coordinates (see in the left). However, this approach was wrong. We need to the use normals in object local space so that we can transform the object once we get the correct texture value (see in the right). 

<p float="left">
  <img src="results/hw4/process/ellipsoids_texture_v2_v3.png" width="410" />
  <img src="results/hw4/ellipsoids_texture.png" width="410" />
</p>

# 2. Perlin Noise

## Input

## Code Design

## Algorithm

## Implementation Process

# 3. Normal Mapping

## Input

## Code Design

## Algorithm

## Implementation Process

# 4. Bump Mapping

## Input

## Code Design

## Algorithm

## Implementation Process

## Other Improvements (Smooth Shading)

### Bugs on


## Final Results
Let's look at the final results of my implementation after all improving.

### bump_mapping_transformed.xml
<p align="left"><img src="results/hw4/bump_mapping_transformed.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### cube_cushion.xml
<p align="left"><img src="results/hw4/cube_cushion.png"></p>
```markdown
XML file is parsed in 1 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### cube_perlin_bump.xml
<p align="left"><img src="results/hw4/cube_perlin_bump.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### cube_perlin.xml
<p align="left"><img src="results/hw4/cube_perlin.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### cube_wall_normal.xml
<p align="left"><img src="results/hw4/cube_wall_normal.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### cube_wall.xml
<p align="left"><img src="results/hw4/cube_wall.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### cube_waves.xml
<p align="left"><img src="results/hw4/cube_waves.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### ellipsoids_texture.xml
<p align="left"><img src="results/hw4/ellipsoids_texture.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### galactica_dynamic.xml
<p align="left"><img src="results/hw4/galactica_dynamic.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 10
Preprocessing is finished in 0 sec
Scene is created in 204 sec
```

### galactica_static.xml
<p align="left"><img src="results/hw4/galactica_static.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 10
Preprocessing is finished in 0 sec
Scene is created in 3 sec
```

### killeroo_bump_walls.xml
<p align="left"><img src="results/hw4/killeroo_bump_walls.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 15
Preprocessing is finished in 1 sec
Scene is created in 33 sec
```

### sphere_nearest_bilinear.xml
<p align="left"><img src="results/hw4/sphere_nearest_bilinear.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### sphere_nobump_bump.xml
<p align="left"><img src="results/hw4/sphere_nobump_bump.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### sphere_nobump_justbump.xml
<p align="left"><img src="results/hw4/sphere_nobump_justbump.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### sphere_normal.xml
<p align="left"><img src="results/hw4/sphere_normal.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 30 sec
```

### sphere_perlin_bump.xml
<p align="left"><img src="results/hw4/sphere_perlin_bump.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### sphere_perlin_scale.xml
<p align="left"><img src="results/hw4/sphere_perlin_scale.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 1 sec
```

### sphere_perlin.xml
<p align="left"><img src="results/hw4/sphere_perlin.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 1
Preprocessing is finished in 0 sec
Scene is created in 0 sec
```

### tap_0200.xml
<p align="left"><img src="results/hw4/tap_0200.png"></p>
```markdown
XML file is parsed in 0 sec
Maximum BVH depth is 17
Preprocessing is finished in 0 sec
Scene is created in 360 sec
```
