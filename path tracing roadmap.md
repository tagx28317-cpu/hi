# RE Fan Game Path Tracing Roadmap

## Goal
Make the path-traced visuals feel closer to a modern AAA horror game.

## Core Rule
- `triangle + BVH` fixes what the tracer can hit.
- `temporal reprojection + denoising` fixes the dots.
- If the goal is "AAA-looking," noise cleanup comes before full mesh tracing.

## Phase 1: Upgrade the Path Tracer Outputs
### Goal
Make the tracer produce enough data for denoising and temporal reuse.

### Steps
1. Output a `beauty` buffer from the path tracer.
2. Output G-buffer data alongside it:
   depth
   normal
   roughness
   material ID
3. Make sure these buffers are stable and match the same camera and sample layout every frame.
4. Store previous-frame versions of the needed buffers.

### Done When
- Every pixel has beauty plus depth, normal, roughness, and material info.
- Buffers can be reused by later denoise passes.

## Phase 2: Add Temporal Reprojection
### Goal
Reuse history instead of treating every frame like a fresh noisy render.

### Steps
1. Generate motion vectors for each pixel.
2. Reproject previous-frame lighting into the current frame.
3. Blend current noisy color with previous accumulated color.
4. Add history rejection rules:
   reject when depth changes too much
   reject when normals change too much
   reject when material ID changes
   reject on disocclusion or newly revealed pixels

### Done When
- Camera movement keeps some stability.
- The image does not fully restart every frame.
- Ghosting is limited by rejection tests.

## Phase 3: Add SVGF-Style Denoising
### Goal
Remove the dot and noise look while keeping edges sharp.

### Steps
1. Add a simple edge-aware bilateral filter first.
2. Upgrade that into an `a-trous` wavelet filter.
3. Guide the filter using:
   depth
   normals
   roughness
   luminance variance if available
4. Run multiple passes with widening step sizes.
5. Protect silhouettes and hard material transitions.

### Done When
- Noise is reduced heavily.
- Edges do not smear badly.
- Reflections and contact shadows stay readable.

## Phase 4: Improve Sampling
### Goal
Make the denoiser's job easier by feeding it better noise.

### Steps
1. Replace simple white-noise sampling.
2. Add blue-noise or a low-discrepancy sequence.
3. Stabilize per-frame sample patterns.
4. Reduce fireflies with clamping or robust sample weighting.

### Done When
- Noise looks finer and less clumpy.
- Temporal accumulation behaves better.
- Denoising produces cleaner results.

## Phase 5: Add Triangle + BVH for Static Level Meshes
### Goal
Let the path tracer hit real environment geometry instead of only simple primitives.

### Steps
1. Pack static level triangles into GPU buffers.
2. Build a BVH for those static meshes.
3. Add triangle intersection support in WGSL.
4. Validate:
   walls
   floors
   props
   occlusion
   reflections
5. Keep this static at first.

### Done When
- The tracer can hit real scene meshes.
- Reflections, shadows, and GI respond to actual level geometry.

## Phase 6: Keep Leon Hybrid for Now
### Goal
Avoid blowing up performance too early.

### Steps
1. Keep Leon on the existing hybrid WebGPU gameplay renderer.
2. Let the environment carry most of the traced GI and reflection mood.
3. Revisit animated mesh path tracing later only if needed.

### Why
- Skinned mesh path tracing is much harder.
- It needs triangle updates and BVH updates for animation.
- It is not the first thing needed to kill the dots.

## Phase 7: Add FSR2-Style Upscaling After Denoising
### Goal
Recover clarity and performance after the denoise pipeline is stable.

### Steps
1. Add temporal upscaling only after reprojection and denoising are working.
2. Feed it:
   color
   depth
   motion vectors
   optional reactive mask
3. Use it as the final upscale and sharpen stage.
4. Tune it for horror-game readability:
   face detail
   specular highlights
   thin geometry
   UI separation

### Done When
- Lower internal render resolution still looks sharp.
- The final image is cleaner and more stable.

## Phase 8: Consider ReSTIR Later
### Goal
Improve low-spp lighting quality once the base pipeline is already solid.

### Steps
1. First finish:
   G-buffer
   reprojection
   denoise
   better sampling
   triangle and BVH
2. Then test ReSTIR for direct lighting reuse.
3. Only go deeper into ReSTIR GI after the simpler pipeline is stable.

### Why
- ReSTIR is powerful, but it is not step one.
- It helps low-sample lighting reuse, not basic pipeline stability.

## Recommended Build Order
1. G-buffer outputs
2. Temporal reprojection plus history rejection
3. `a-trous` or SVGF-style denoise
4. Better sampling
5. Triangle plus BVH for static level meshes
6. FSR2-style upscale and sharpen
7. ReSTIR
8. Animated or skinned path tracing if still needed

## AAA Priority Summary
### Highest Priority
- temporal reprojection
- denoising
- better sampling

### Medium Priority
- triangle plus BVH for static world meshes
- FSR2-style upscale and sharpen

### Lowest Priority
- ReSTIR
- full skinned Leon path tracing

## Final Direction
For this RE fan game, the best near-term AAA path is:

- Path trace the environment better.
- Denoise aggressively but intelligently.
- Use temporal reuse.
- Keep Leon hybrid during gameplay.
- Add FSR2-style upscale after the denoiser.
- Only then push into ReSTIR or animated mesh path tracing.
