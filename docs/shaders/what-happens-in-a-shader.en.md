# What happens in a shader

!!! abstract "What this page answers"
    Where the colour of a pixel actually comes from, why a normal map is blue, and why
    hit-flash, dissolve and a ground indicator are three different kinds of shader work.

## The pipeline, on one screen

Drawing one object goes through four stages:

```text
vertices ──▶ VERTEX ──▶ rasteriser ──▶ FRAGMENT ──▶ output merge ──▶ screen
             (per vertex)   (triangles      (per pixel)   (depth, blend)
                            → pixels)
```

1. **Vertex stage** — runs once per vertex. It moves each vertex from object space into
   the space the camera can see, and passes data forward: UV, normal, world position.
2. **Rasteriser** — a fixed-function step. It works out which pixels each triangle covers
   and interpolates the forwarded data across the triangle.
3. **Fragment stage** — runs once per *candidate* pixel. It receives the interpolated
   values and returns a colour (and optionally an alpha). This is the part you write in a
   Shader Graph, and the part that costs the most.
4. **Output merge** — depth test, blending, write.

The asymmetry at the top is the thing to remember: **the vertex stage runs per vertex, the
fragment stage runs per pixel.** A model with 5,000 vertices covering most of the screen
spends almost all of its time in the fragment stage.

## Material, shader, Shader Graph — three different things

| Word | What it is | What it stores |
| --- | --- | --- |
| **Material** | an asset: a saved bag of values | colours, textures, numbers |
| **Shader** | a program | the instructions |
| **Shader Graph** | an editor for shaders | a graph that compiles into a shader |

A material does not "have a look". It holds values; the shader decides what those values
mean. That is why two materials using the same shader look different, and why editing a
shared material changes every object using it.

## A fragment shader is just a function

Stripped of ceremony, the fragment stage is:

```hlsl
float4 frag(Varyings i) : SV_Target
{
    float3 colour = ...;      // everything you compute
    return float4(colour, 1); // what the pixel becomes
}
```

Everything else — the Master Stack, the nodes, the blackboard — is a way of writing that
function. The question "what does this shader do?" is always "given these inputs, what
colour comes out?"

## Where lighting comes from

A lit shader does not receive a finished colour. It receives the surface's **base colour**
(albedo, the material's diffuse tint) and multiplies it by the light:

```hlsl
float3 N   = normalize(i.normalWS);
float3 L   = normalize(lightDir);
float  ndl = saturate(dot(N, L));

float3 lit = baseColour * ndl * lightColour;
```

`dot(N, L)` is the term from
[vectors and the dot product](../foundations/vectors-and-dot-product.md): 1 when the
surface faces the light, 0 at the terminator, clamped below that. Everything else in a
more advanced lighting model — specular, ambient, shadows — is added on top of this line,
not instead of it.

## Why the normal map is blue

This is the question the lecture cannot stop for, so here it is.

A normal is a direction, with components in −1…1. A texture stores colour in 0…1. To put a
direction in a texture, the components are remapped:

```text
stored = direction * 0.5 + 0.5
```

A **tangent-space** normal map stores the normal relative to the surface, in a frame where

- `x` runs along the surface (tangent),
- `y` runs across it (bitangent),
- `z` points **straight out of the surface**.

A perfectly flat surface has the normal `(0, 0, 1)` — pointing out. Stored as colour that
becomes `(0.5, 0.5, 1.0)`, which is the familiar **light blue-purple**. A flat normal map is
almost entirely that colour; the details are small deviations around it. When you see a
normal map that looks like a lavender photo of stone, you are looking at a picture of
"which way does each point of this surface face, relative to flat".

This is also why the components must be read back with `* 2 - 1` in the shader — the texture
gives you 0…1, the maths needs −1…1.

## Three effects, three kinds of shader work

### Hit-flash — change a value, keep the shader

A hit-flash does not need a new shader. The shader reads a number — say `_Flash` — and the
script animates it from 0 to 1 and back:

```hlsl
float3 col = lerp(baseColour, float3(1,1,1), _Flash);
// or, cheaper for a whole-surface flash:
col += emissionColour * _Flash;
```

The blending of values is `lerp`; the timing belongs to C#, not the shader. This is the
general pattern: **the shader exposes a control, the game code drives it**.

### Dissolve — noise, threshold, and a hard cut

A dissolve is a noise texture compared against a moving threshold:

```hlsl
float noise = SAMPLE_TEXTURE2D(_NoiseTex, sampler_NoiseTex, uv).r;
float threshold = _DissolveAmount;          // 0 → 1 over the effect
clip(noise - threshold);                    // discard fragments, do not blend them
```

`clip` (alpha clip) throws the fragment away entirely, so the holes are hard-edged and the
object can be cut away progressively. Note what it is *not*: it is not fading alpha. A fade
makes the whole object ghostly; a clip makes it disappear in pieces.

### Skill indicator — a transparent decal on the ground

A ground indicator is usually an **unlit, transparent** quad or decal, with its fill driven
by UV distance from the centre:

```hlsl
float d = distance(uv, float2(0.5, 0.5));
float fill = step(d, _Radius);              // or smoothstep for a soft edge
float4 col = _Colour * fill;                // unlit: no dot(N, L) at all
```

Three consequences worth knowing: unlit (it ignores the scene lighting, deliberately), no
depth write (it lies on the ground and must not occlude it), and it covers a lot of pixels
at once — which is the next section.

## What a shader costs

Cost is roughly:

```text
cost ≈ pixels_covered × instructions_per_pixel
```

Which gives three rules:

- **Small object, heavy shader** — fine. Few pixels.
- **Full-screen effect, trivial shader** — expensive, because "trivial" is still multiplied
  by two million pixels.
- **Transparency** — more expensive than it looks: transparent fragments cannot be
  resolved early, and several overlapping transparent surfaces (a ground indicator under a
  dissolve under a flash) all get drawn. That is **overdraw**.

## Check yourself

??? question "1. A model has 100 triangles and fills the screen. Where does its cost come from?"
    Almost entirely the fragment stage: the whole screen's worth of pixels run the per-pixel
    shader. Triangle count is irrelevant at that size.

??? question "2. Why does `clip(noise - threshold)` give harder edges than multiplying alpha by `noise`?"
    `clip` discards the fragment, so it is either drawn fully or not at all — the boundary is
    the noise threshold itself. Multiplying alpha makes the object translucent and the edge
    soft, and it also cannot use depth-only techniques.

??? question "3. A normal map looks lavender. What does the colour actually encode, and which space?"
    A direction per pixel, remapped from −1…1 to 0…1, in tangent space — `z` points out of
    the surface, so a flat normal is `(0.5, 0.5, 1)`, i.e. lavender.

??? question "4. Why is the ground indicator unlit?"
    Because it is a gameplay signal, not a surface. It must read the same in a dark room and
    in sunlight; making it react to scene lighting would make the warning unreliable.
