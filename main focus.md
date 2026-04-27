# main focus



### bring back bloom but make it like unreal engine 5.1





Unreal Engine handles bloom as a post-process effect that simulates light bleeding around bright objects, applied during the tonemapper pass. Bloom is turned on by default in the Post Process Volume. It is calculated by downsampling the HDR scene color buffer, applying Gaussian blurs, and upsampling them back to create a glow. 

Froyok

Froyok

&#x20;+4

Key Aspects of Bloom in Unreal Engine

Default State: Bloom is enabled by default in all new scenes via the default Post Process Volume.

Calculation Method (Standard): The engine uses a multi-pass approach—downsampling the scene multiple times, blurring, and then upsampling to combine the glow with the main scene color, usually based on intensity thresholds.

Convolution Bloom: For higher fidelity, UE supports "Convolution Bloom" (FFT), which uses images (textures) to generate complex light shapes or lens flares, rather than just a simple gaussian blur.

Performance vs. Quality: Wide blurs are performed at lower resolutions for better performance, ensuring high-quality, wide-reaching blooms without excessive performance costs.

Anamorphic Bloom: You can modify the default circular bloom shape to an anamorphic streak using console commands (e.g., r.Bloom.Cross).

Controls: Within the Post Process Volume, you can control the Intensity, Threshold (when bright pixels start to glow), and Tint of the bloom



Path Tracing: This provides realistic shadows and lighting, although it can look noisy or cause "ghosting" artifacts, requiring tools like DLSS-Ray Reconstruction to improve quality.

Hero Lighting: A technique used in dark indoor settings, similar to Horizon Forbidden West, which casts ambient light on characters to improve shadow and shading details.

Performance: Ray tracing significantly improves global illumination and reflections but can be demanding, causing "smearing" on certain settings.

HDR and Brightness: The game uses a dark, atmospheric style that can appear washed out or too dark on some displays, often requiring manual adjustments to the three-tier brightness system.

Lighting Artifacts: Some users reported bright, obstructive white light during puzzle interactions, especially when modifying the field of view (FOV). 





3D models use specialized texture maps and material properties to interact with lighting, enhancing realism and depth. Key elements include Emission maps (making surfaces glow), Normal/Bump maps (creating surface depth), Roughness/Gloss maps (defining reflection sharpness), and HDRI maps (using 360-degree environment lighting for realistic reflections). 

Reddit

Reddit

&#x20;+4

Emission Nodes/Textures: These make specific parts of a texture map emit light, creating a glowing effect (e.g., lights on a console, eyes).

Normal Maps: These fake high-resolution surface details on a low-poly model, causing light to bounce differently and creating depth without complex geometry.

Roughness/Metallic Maps: These textures tell the lighting engine which parts of the model are shiny, matte, or metal, affecting specular reflections.

Light Texture (Gobos): Textures can be applied directly to a light source, acting as a "stencil" to create complex patterns, shadows, or softer lighting, similar to a physical spotlight filter.

Bloom Effect: A post-processing technique applied to the model's emissive materials to make bright areas seem to bleed light into the surrounding scene. 

Creative Shrimp

Creative Shrimp

&#x20;+5

These methods ensure that textures react naturally to the light in the scene, rather than relying solely on default, flat lighting. 



#### Make leons transitions more smoother and human like (maybe make a system that ragdolls some of his limbs befor correcting it to the correct spot?)





