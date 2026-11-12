# Foundations

The three modules assume a small amount of vector maths and engine knowledge that none of
them teaches. This section is that assumption, written down.

It is not a maths course. There is no proof, no notation for its own sake, and nothing
that will not appear in the engine this year. Each page takes one idea, shows where it
appears in a real shader or a real script, and stops.

## Pages

| Page | Status | Why it is here |
| --- | --- | --- |
| [Vectors and the dot product](vectors-and-dot-product.md) | written | Lighting, facing, projection, rim light — the dot product is behind all four |
| Matrices and coordinate spaces | planned | Why moving an object is a multiplication, and why world space and object space are not the same thing |
| UV coordinates and textures | planned | Why a texture repeats, and what a texel is |
| The engine loop, in C# terms | planned | `Update` vs `FixedUpdate`, and where a frame actually goes |

Planned pages have no date. They get written when the gap shows up in class often enough
to be worth writing down.

## How to read a page

Read the first section. If the example already makes sense, skip to *check yourself* and
try the questions. If a question fails, go back — the failure tells you which section to
reread, which is faster than rereading all of it.
