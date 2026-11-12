# Shaders

A shader is the smallest program in the project and the one that decides how everything
looks. The lab teaches you to build effects; these pages cover the part underneath, so the
effect can be changed instead of copied.

## Pages

| Page | Status | Why it is here |
| --- | --- | --- |
| [What happens in a shader](what-happens-in-a-shader.md) | written | The pipeline in one screen, and why three effects work the way they do |
| Lighting, beyond Lambert | planned | Half-Lambert, specular and the difference between an *art* choice and a *cost* choice |
| A dissolve, step by step | planned | Noise, threshold, alpha clip — built once, explained node by node |
| UVs, tiling and scrolling | planned | Why a texture repeats and what a scrolling UV actually moves |
| Shader Graph and HLSL side by side | planned | The same shader written twice, so the nodes stop being magic |
| What a shader costs | planned | Fill rate, overdraw, transparency, and how to read the frame |

## The one thing to take away

Fragment work scales with **pixels**, not with objects. A small object with a heavy shader
is cheap; a full-screen transparent effect with a trivial shader is expensive. Almost every
shader optimisation conversation starts from that sentence.
