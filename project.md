# Term Project

The converting [Glass of Water by aXel](https://benedikt-bitterli.me/resources/) scene to our XML format is my term project.

<p float="left">
  <img src="results/project/TungstenRender.png" />
</p>

As seen, it includes glass types objects. Unfortunately, I could not find the gaze vector or gaze point and near distance by using the world matrix given in the original work. I completed all fields except camera properties in the XML file and assumed the camera properties and tried to produce a scene. But, I could not achieve it.

```xml  
<Scene>
	<BackgroundColor>0 0 0</BackgroundColor>

	<MaxRecursionDepth>6</MaxRecursionDepth>

	<Cameras>
		<Camera id="1" type="lookAt">
			<Position>-0.0893585 2.69412 25.6726</Position>
			<Gaze>0 0 1</Gaze>
			<Up>0 1 0</Up>
			<FovY>20.114292</FovY>
			<NearDistance>1.7321</NearDistance>
			<ImageResolution>1280 720</ImageResolution>
			<ImageName>glass-of-water.png</ImageName>
			<NumSamples>16</NumSamples>
			<Tonemap>
				<TMO>Photographic</TMO>
				<TMOOptions>0.18 0</TMOOptions>
				<Saturation>1.0</Saturation>
				<Gamma>2.2</Gamma>
			</Tonemap>
			<Renderer>PathTracing</Renderer>
			<RendererParams>NextEventEstimation ImportanceSampling</RendererParams>
		</Camera>
	</Cameras>

	<BRDFs>
		<ModifiedPhong id="1" normalized="true">
			<Exponent>90</Exponent>
		</ModifiedPhong>
		<ModifiedPhong id="2" normalized="true">
			<Exponent>30</Exponent>
		</ModifiedPhong>
		<ModifiedPhong id="3" normalized="true">
			<Exponent>10</Exponent>
		</ModifiedPhong>
	</BRDFs>

	<Materials>
		<Material id="1" type="conductor" BRDF="3" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>4.277513 3.513154 2.761126</DiffuseReflectance>
			<SpecularReflectance>0.578596, 0.578596, 0.578596</SpecularReflectance>
			<PhongExponent>1</PhongExponent>
			<MirrorReflectance>3.491184 2.889358 3.111696</MirrorReflectance>
			<RefractionIndex>0.135</RefractionIndex>
			<AbsorptionIndex>3.985</AbsorptionIndex>
			<Roughness>0.1</Roughness>
		</Material>
		<Material id="2" type="conductor" BRDF="3" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>1.657460 0.880369 0.521229</DiffuseReflectance>
			<SpecularReflectance>0.578596, 0.578596, 0.578596</SpecularReflectance>
			<PhongExponent>1</PhongExponent>
			<MirrorReflectance>9.223869 6.269523 4.837001 </MirrorReflectance>
			<RefractionIndex>0.135</RefractionIndex>
			<AbsorptionIndex>3.985</AbsorptionIndex>
			<Roughness>0.1</Roughness>
		</Material>

		<Material id="3" type="dielectric" BRDF="2" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>0 0 0</DiffuseReflectance>
			<SpecularReflectance>0 0 0</SpecularReflectance>
			<AbsorptionCoefficient>0.01 0.01 0.01</AbsorptionCoefficient>
			<RefractionIndex>1.330000</RefractionIndex>
		</Material>
		<Material id="4" type="dielectric" BRDF="2" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>0 0 0</DiffuseReflectance>
			<SpecularReflectance>0 0 0</SpecularReflectance>
			<AbsorptionCoefficient>0.01 0.01 0.01</AbsorptionCoefficient>
			<RefractionIndex>1.310000</RefractionIndex>
		</Material>
		<Material id="5" type="dielectric" BRDF="2" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>0 0 0</DiffuseReflectance>
			<SpecularReflectance>0 0 0</SpecularReflectance>
			<AbsorptionCoefficient>0.01 0.01 0.01</AbsorptionCoefficient>
			<RefractionIndex>1.500000</RefractionIndex>
		</Material>
		<Material id="6" type="dielectric" BRDF="2" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>0 0 0</DiffuseReflectance>
			<SpecularReflectance>0 0 0</SpecularReflectance>
			<AbsorptionCoefficient>0.01 0.01 0.01</AbsorptionCoefficient>
			<RefractionIndex>0.763000</RefractionIndex>
		</Material>

		<Material id="7" BRDF="1" degamma="true">
			<AmbientReflectance>1 1 1</AmbientReflectance>
			<DiffuseReflectance>0 0 0</DiffuseReflectance>
			<SpecularReflectance>0 0 0</SpecularReflectance>
			<PhongExponent>1</PhongExponent>
		</Material>
	</Materials>

	<VertexData>
		-5.03848 14.3022 22.968
		4.96152 14.3022 22.968
		4.96152 6.65957 29.417
		-5.03848 6.65957 29.417
	</VertexData>

	<Transformations>
		<Composite id="1">5 4.86887e-007 1.77975e-007 -0.0384822 4.86887e-007 -3.82133 -3.22451 10.4809 -1.77975e-007 3.22451 -3.82133 26.1925 0 0 0 1</Composite>
	</Transformations>

	<TexCoordData>
		0 0
		1 0
		1 1
		0 1
	</TexCoordData>

	<Objects>
		<Mesh id="1">
			<Material>3</Material>
			<Faces plyFile="models/Mesh008.ply" />
		</Mesh>
		<Mesh id="2">
			<Material>3</Material>
			<Faces plyFile="models/Mesh005.ply" />
		</Mesh>
		<Mesh id="3">
			<Material>4</Material>
			<Faces plyFile="models/Mesh004.ply" />
		</Mesh>
		<Mesh id="4">
			<Material>6</Material>
			<Faces plyFile="models/Mesh003.ply" />
		</Mesh>
		<Mesh id="5">
			<Material>4</Material>
			<Faces plyFile="models/Mesh010.ply" />
		</Mesh>
		<Mesh id="6">
			<Material>6</Material>
			<Faces plyFile="models/Mesh009.ply" />
		</Mesh>
		<Mesh id="7">
			<Material>6</Material>
			<Faces plyFile="models/Mesh012.ply" />
		</Mesh>
		<Mesh id="8">
			<Material>4</Material>
			<Faces plyFile="models/Mesh014.ply" />
		</Mesh>
		<Mesh id="9">
			<Material>6</Material>
			<Faces plyFile="models/Mesh015.ply" />
		</Mesh>
		<Mesh id="10">
			<Material>4</Material>
			<Faces plyFile="models/Mesh006.ply" />
		</Mesh>
		<Mesh id="11">
			<Material>6</Material>
			<Faces plyFile="models/Mesh002.ply" />
		</Mesh>
		<Mesh id="12">
			<Material>4</Material>
			<Faces plyFile="models/Mesh001.ply" />
		</Mesh>
		<Mesh id="13">
			<Material>1</Material>
			<Faces plyFile="models/Mesh007.ply" />
		</Mesh>
		<Mesh id="14">
			<Material>2</Material>
			<Faces plyFile="models/Mesh011.ply" />
		</Mesh>
		<Mesh id="15">
			<Material>5</Material>
			<Faces plyFile="models/Mesh013.ply" />
		</Mesh>
		<Mesh id="16">
			<Material>3</Material>
			<Faces plyFile="models/Mesh000.ply" />
		</Mesh>

		<LightMesh id="17">
			<Material>7</Material>
			<Transformations>c1</Transformations>
			<Faces>
				 0 1 2
				 0 2 3
			</Faces>
			<Radiance>15.9155, 27.0563, 31.831</Radiance>
		</LightMesh>

	</Objects>

</Scene>
```
