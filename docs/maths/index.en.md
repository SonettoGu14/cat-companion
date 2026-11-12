# Maths for games

The maths that game code actually uses, written down: vectors, matrices, coordinate spaces,
interpolation, and a little probability for balance work. These things turn up in every
shader, every script and every balance table, and nobody teaches them on the way in.

It is not a maths course. There is no proof, no notation for its own sake, and nothing that
will not appear in the engine this year. Each page takes one idea, shows where it appears,
and stops.

## Pages

| Page | Status | Why it is here |
| --- | --- | --- |
| [Vectors and the dot product](vectors-and-dot-product.md) | written | Lighting, facing, projection, rim light — the dot product is behind all four |
| Matrices and coordinate spaces | planned | Why moving an object is a multiplication, and why world space and object space are not the same thing |
| UV coordinates and textures | planned | Why a texture repeats, and what a texel is |
| Interpolation: lerp, smoothstep and curves | planned | Why easing looks the way it does, and how to choose between lerp and smoothstep |
| Probability and expected value | planned | Balance numbers, drop rates, and why "50% chance" does not mean "half your players" |

Planned pages have no date. They get written when the gap shows up often enough to be worth
writing down.

## How to read a page

Read the first section. If the example already makes sense, skip to *check yourself* and
try the questions. If a question fails, go back — the failure tells you which section to
reread, which is faster than rereading all of it.
