# Glossary

One English term, one Chinese term, one line of meaning. When a page and this table
disagree, the page is wrong and the table should be fixed.

The fourth column says where the term does work — that is usually the fastest way to see
why a term matters.

## Maths and spaces

| English | 中文 | Meaning | Where it shows up |
| --- | --- | --- | --- |
| vector | 向量 | Three numbers read as a position or a direction | Everywhere; `Vector3` |
| scalar | 标量 | A single number, not a direction | Speed, intensity, threshold |
| magnitude / length | 模长 | The length of a vector | Distance between two points |
| normalise | 归一化 | Scale a vector to length 1, keeping its direction | Before any dot product that needs an angle |
| dot product | 点乘 | `a·b = |a||b|cosθ`; how much two directions agree | Lighting, facing tests, rim light |
| cross product | 叉乘 | A vector perpendicular to both inputs; builds a right-angled frame | Tangents, normals from geometry |
| matrix | 矩阵 | A grid of numbers; a transform is one | Moving, rotating, scaling an object |
| world space | 世界空间 | The scene's single shared frame | Light directions, player position |
| object (local) space | 物体空间 | A mesh's own frame, before its transform | Mesh normals, prefab design |
| clip space | 裁剪空间 | The frame a vertex ends in, just before the screen | Vertex shader output |
| UV coordinates | UV 坐标 | 2D texture coordinates, 0 to 1 per side | Textures, dissolve, indicators |
| texel | 纹素 | One pixel of a texture | Filtering, mipmaps |

## Rendering and shaders

| English | 中文 | Meaning | Where it shows up |
| --- | --- | --- | --- |
| vertex | 顶点 | A corner point of a mesh | Vertex stage runs once per vertex |
| fragment | 片元 | A pixel candidate; the per-pixel shader runs per fragment | Every effect cost estimate |
| rasterise | 光栅化 | Turn triangles into pixel candidates | Between vertex and fragment stage |
| shader | 着色器 | The program that computes a surface or an effect | Materials, effects, post-processing |
| Shader Graph | 着色器图 | Unity's visual editor that compiles a graph into a shader | CAT105TC weeks 14–17; CAT204TC lab |
| HLSL | 高级着色语言 | The text language shaders are written in | `Show Generated Code` |
| Master Stack | 主堆栈 | The output contract of a Shader Graph | Base Color, Normal, Emission… |
| Blackboard | 黑板 | The list of values a Shader Graph exposes | Properties you can drive from C# |
| albedo / Base Color | 基础色 | The surface's diffuse colour, before lighting | `lit = albedo * ndl * light` |
| emission | 自发光 | Light the surface emits itself; not affected by scene light | Hit-flash, glowing parts |
| unlit | 无光照 | A shader that ignores scene lighting | UI, indicators, VFX |
| alpha clip | 透明裁剪 | Discard a fragment entirely instead of blending it | Dissolve |
| normal map | 法线贴图 | A texture storing per-pixel surface direction in tangent space | The blue-purple texture |
| tangent space | 切线空间 | A frame aligned to the surface; `z` points out of it | Why normal maps are blue |
| fresnel | 菲涅尔 | The effect where grazing angles show more reflection | Rim light, glass |
| draw call | 绘制调用 | One command from the CPU telling the GPU to draw something | Performance, batching |

## XR

| English | 中文 | Meaning | Where it shows up |
| --- | --- | --- | --- |
| XR | 扩展现实 | The umbrella term for VR, AR and MR | CAT207TC |
| VR | 虚拟现实 | Fully synthetic view; the real room is replaced | CAT207TC core |
| HMD | 头戴显示器 | The headset itself | Pico 4 Ultra |
| 6DoF | 六自由度 | Position (3) plus rotation (3) tracked | Head, controllers |
| 3DoF | 三自由度 | Rotation only; position is fixed | Cheap headsets, some video players |
| IPD | 瞳距 | Distance between the eyes; sets the stereo separation | Headset setup |
| FOV | 视场角 | How wide the view is, in degrees | Comfort, scale |
| presence | 临场感 | The felt sense of *being there* | The goal of most VR design |
| immersion | 沉浸感 | Being absorbed by the experience (technical or narrative) | Not the same as presence |
| embodiment | 具身感 | The felt sense that a virtual body is your body | Avatars, hands |
| proprioception | 本体感觉 | Your sense of where your body parts are without looking | Why hands must match |
| vection | 视动（自我运动错觉）| Feeling you are moving because the world moved | Comfort; smooth locomotion |
| simulator sickness | 模拟器不适 | Nausea from a mismatch between vision and the inner ear | Comfort settings |
| snap turn | 瞬转 | Rotate the view in discrete jumps instead of smoothly | The main comfort tool |
| motion-to-photon latency | 运动到光子延迟 | Time from moving the head to the image changing | Usually want under ~20 ms |
| haptics | 触觉反馈 | Vibration feedback through the controllers | Grab confirmation |
| passthrough | 透视 | Cameras show the real room through the headset | Mixed-reality features |
| guardian / boundary | 安全区 | The play-space border the system draws | Room setup |
| spatial audio | 空间音频 | Sound positioned in 3D, changing with head direction | CAT207TC lab 3 |

## C# and Unity

| English | 中文 | Meaning | Where it shows up |
| --- | --- | --- | --- |
| field | 字段 | A variable stored on an object | `public float speed;` |
| property | 属性 | A member that runs code when read or written | `public int Health { get; set; }` |
| method | 方法 | A named block of code | `void Jump()` |
| class | 类 | A blueprint for objects | Every script |
| instantiate | 实例化 | Create a copy of an object at runtime | Bullets, enemies |
| serialise | 序列化 | Store a field's value in the Inspector or on disk | Save/load, Inspector fields |
| prefab | 预制体 | A saved, reusable object template | Reusable objects in every lab |
| ScriptableObject | 可脚本化对象 | A data asset that is not attached to a scene object | Config, skill data |
| coroutine | 协程 | A method that can pause and resume across frames | Timed effects, waits |
| deltaTime | 帧间隔 | The time the last frame took, in seconds | Any per-frame movement |
| event | 事件 | A notification one object raises and others listen to | UI, damage, score |
| singleton | 单例 | A class with exactly one instance, reachable globally | Managers |
