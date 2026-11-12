# Vectors and the dot product

!!! abstract "What this page answers"
    Why lighting is written `dot(N, L)`, what that number actually means, and the four
    places in a game where the dot product does the work. Ten minutes.

## A vector is two things at once

The same three numbers can describe a **position** or a **direction**, and the engine
treats them almost the same way. Both are `Vector3`:

```csharp
Vector3 position = transform.position;    // where something is
Vector3 forward  = transform.forward;     // which way something faces
```

The difference is what you are allowed to do with them:

- Two positions can be **subtracted** to get a direction: `target - self` points from you
  to the target.
- A position plus a direction gives a new position.
- Two directions can be **compared** — are they the same way, opposite, or at a right
  angle? That is the dot product, and it is the reason this page exists.
- Two positions cannot be compared that way. "How similar are Suzhou and Shanghai" is not
  a question.

## Length, and normalising

A vector's length (magnitude) is the distance from the origin to its tip:

```text
|v| = sqrt(vx*vx + vy*vy + vz*vz)
```

A **normalised** (unit) vector has length exactly 1. It keeps the direction and throws
away the length. In Unity that is `v.normalized`; in a shader it is `normalize(v)`.

Everything that needs an angle or a "facing" answer needs unit vectors. If you feed the
dot product a vector of length 3, the answer comes back three times too large — and it
will still look plausible, which is why this bug survives.

## Adding and scaling

```text
new position   = position + direction * speed * deltaTime
```

Multiplying a direction by a positive number lengthens it without turning it; by a
negative number it turns around. This is how a velocity is applied to a position, and
it is why *speed* and *direction* are stored separately in most movement code.

Adding two directions has no obvious meaning. Adding two **velocities** does: the player's
motion plus a wind push is a new velocity.

## The dot product

The dot product takes two vectors and returns one number:

```text
a · b = |a| * |b| * cos(theta)
```

where `theta` is the angle between them. If both vectors are unit length, the lengths
disappear and the dot product **is** the cosine of the angle:

| Angle between the vectors | Dot product (unit vectors) | Meaning |
| --- | --- | --- |
| 0° | 1 | same direction |
| 60° | 0.5 | mostly the same way |
| 90° | 0 | perpendicular |
| 120° | -0.5 | mostly the other way |
| 180° | -1 | exactly opposite |

So the dot product is a **similarity score** between two directions, clamped by
construction to the range −1 to 1. That is all it is. The reason it is everywhere is that
"how much does direction A agree with direction B" is a question games ask constantly.

## What the sign tells you

For two unit directions:

- `dot > 0` — they point the same way. In gameplay terms: *in front of me*.
- `dot == 0` — perpendicular. *Directly to my side*, or *on the horizon*.
- `dot < 0` — opposite. *Behind me*.

The sign alone answers a huge number of gameplay questions: is the enemy in front, is the
player facing the door, is this surface pointing at the sun.

## Where it appears, four times

### 1. Lighting — `dot(N, L)`

`N` is the surface normal (the direction the surface faces), `L` is the direction to the
light. When the surface faces the light, `dot(N, L)` is 1 and the surface is fully lit;
when the surface faces away it is negative, and a surface cannot be lit from behind:

```hlsl
float ndl = saturate(dot(N, L));          // clamp to 0..1
float3 lit = albedo * ndl * lightColor;   // Lambert
```

`saturate` is the whole reason lighting does not go black-and-then-bright again on the
back side. This one line is the core of almost every lit shader.

### 2. Facing tests — `dot(forward, toTarget) > 0`

Is the target in front of me, or behind me? No `Atan2`, no angles, one dot product. The
same trick answers "is the player looking at the door" and "should this AI notice the
player".

### 3. Projection — how much of this movement is forward

Moving at `velocity`, how much of that motion goes along `forward`? `dot(velocity, forward)`
gives the answer as a number you can compare to a speed limit. This is also how "walk
slower while strafing" and "no forward speed while against a wall" are written.

### 4. Rim light and fresnel — `1 - dot(N, V)`

`V` is the direction to the camera. When the surface is edge-on to the camera, `dot(N, V)`
approaches 0 and `1 - dot(N, V)` approaches 1 — which is exactly the glowing edge in a rim
light or a fresnel highlight:

```hlsl
float rim = pow(1.0 - saturate(dot(N, V)), 4.0);
```

## In code

=== "C# (Unity)"

    ```csharp
    // Normalise first: Dot works with any length, but only unit vectors give a cosine.
    Vector3 toTarget = (target.position - transform.position).normalized;
    float facing = Vector3.Dot(transform.forward, toTarget);   // 1 front, 0 side, -1 behind

    if (facing > 0f)
    {
        // target is in the forward half of the world
    }
    ```

=== "Shader (HLSL)"

    ```hlsl
    float3 N = normalize(i.normalWS);
    float3 L = normalize(_MainLightPosition.xyz);
    float  ndl = saturate(dot(N, L));
    ```

!!! note "Dot product vs `Vector3.Angle`"
    `Vector3.Angle(a, b)` returns degrees (0–180) and internally uses the dot product.
    When you only need a comparison — *is it in front*, *is it within 45 degrees* — use the
    dot product directly; comparing `dot > 0.707` is the same as "within 45 degrees" and
    costs no trig. Reach for `Angle` when you need to display or interpolate the number.

## Three traps

!!! danger "Trap 1 — forgetting to normalise"
    `dot(forward, toTarget)` where `toTarget` has length 4 returns 4 when facing it
    directly. The comparison `> 0` still works, so the bug hides — until the code starts
    comparing against a threshold like `0.5`.

!!! danger "Trap 2 — reading the dot product as an angle"
    A dot product of 0.5 is **60 degrees**, not 0.5 degrees and not 30. If you need the
    angle, take `acos` of the dot product — but if you only need a decision, do not.

!!! danger "Trap 3 — mixing coordinate spaces"
    A normal in **object space** dotted with a light direction in **world space** gives a
    wrong number that still varies smoothly across the surface, so it looks like lighting.
    It is the classic "the shading rotates with the model" bug. This is also the reason a
    normal map is blue — see
    [what happens in a shader](../shaders/what-happens-in-a-shader.md).

## Check yourself

??? question "1. Two unit vectors have a dot product of 0. What is the angle between them?"
    90 degrees. The dot product of unit vectors is `cos(theta)`, and `cos(90°) = 0`.

??? question "2. `dot(N, L)` is negative on half of a sphere. Why does the lighting not go negative there?"
    Because of `saturate` (clamp to 0–1). The negative values are the back side, where no
    light arrives; clamping makes it black instead of subtracting light.

??? question "3. Your code says `if (Vector3.Dot(transform.forward, toTarget) > 0.7f)`. What is the actual condition?"
    0.7 is the cosine of about 45°, so "the target is within roughly 45 degrees of straight
    ahead". If `toTarget` is not normalised, the threshold means something else entirely.

??? question "4. Why is `N` normalised in the shader, when the mesh normals are already unit length?"
    Because interpolation across a triangle shortens them: the average of two unit vectors
    is almost never unit length. Normalising per fragment is what keeps `dot(N, L)` a
    genuine cosine.
