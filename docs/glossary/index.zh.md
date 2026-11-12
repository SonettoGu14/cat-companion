# 术语表

一个英文词、一个中文词、一句话解释。页面和这张表冲突时，是页面错了，表要改。

第四列写这个词在哪里干活 —— 看清楚一个词为什么重要，通常这是最快的路子。

## 数学与空间

| 中文 | English | 含义 | 出现在 |
| --- | --- | --- | --- |
| 向量 | vector | 三个数，读成位置或方向 | 到处都是；`Vector3` |
| 标量 | scalar | 单个数字，不是方向 | 速度、强度、阈值 |
| 模长 | magnitude / length | 向量的长度 | 两点之间的距离 |
| 归一化 | normalise | 把向量缩到长度 1，方向不变 | 需要角度的点乘之前 |
| 点乘 | dot product | `a·b = \|a\|\|b\|cosθ`；两个方向有多一致 | 光照、朝向判断、边缘光 |
| 叉乘 | cross product | 垂直于两个输入向量的向量；用来搭直角坐标系 | 切线、从几何求法线 |
| 矩阵 | matrix | 数字排成的网格；一次变换就是一个矩阵 | 移动、旋转、缩放物体 |
| 世界空间 | world space | 整个场景共用的那个坐标系 | 光照方向、玩家位置 |
| 物体空间 | object / local space | 网格自己的坐标系，在变换之前 | 网格法线、预制体设计 |
| 裁剪空间 | clip space | 顶点最后所在的坐标系，紧挨着屏幕之前 | 顶点着色器输出 |
| UV 坐标 | UV coordinates | 二维贴图坐标，每边 0 到 1 | 贴图、溶解、指示器 |
| 纹素 | texel | 贴图里的一个像素 | 过滤、mipmap |

## 渲染与着色器

| 中文 | English | 含义 | 出现在 |
| --- | --- | --- | --- |
| 顶点 | vertex | 网格的角点 | 顶点阶段每顶点跑一次 |
| 片元 | fragment | 像素候选；逐像素着色器按片元跑 | 每次估算效果成本 |
| 光栅化 | rasterise | 把三角形变成像素候选 | 顶点阶段和片元阶段之间 |
| 着色器 | shader | 计算表面或效果的那个程序 | 材质、效果、后处理 |
| 着色器图 | Shader Graph | Unity 的视觉编辑器，把图编译成着色器 | CAT105TC 第 14–17 周；CAT204TC 实验 |
| 高级着色语言 | HLSL | 写着色器用的文本语言 | `Show Generated Code` |
| 主堆栈 | Master Stack | Shader Graph 的输出契约 | Base Color、Normal、Emission… |
| 黑板 | Blackboard | Shader Graph 暴露出来的值列表 | 能被 C# 驱动的属性 |
| 基础色 | albedo / Base Color | 表面在受光之前的漫反射颜色 | `lit = albedo * ndl * light` |
| 自发光 | emission | 表面自己发出的光，不受场景光照影响 | 受击闪白、发光部件 |
| 无光照 | unlit | 忽略场景光照的着色器 | UI、指示器、VFX |
| 透明裁剪 | alpha clip | 直接丢弃片元，而不是混合 | 溶解 |
| 法线贴图 | normal map | 用切线空间存逐像素表面方向的贴图 | 那张蓝紫色的贴图 |
| 切线空间 | tangent space | 贴合表面的坐标系；`z` 朝表面外 | 法线贴图为什么是蓝色 |
| 菲涅尔 | fresnel | 掠射角处反射更强的现象 | 边缘光、玻璃 |
| 绘制调用 | draw call | CPU 告诉 GPU「画这个东西」的一条命令 | 性能、批处理 |

## XR

| 中文 | English | 含义 | 出现在 |
| --- | --- | --- | --- |
| 扩展现实 | XR | VR、AR、MR 的总称 | CAT207TC |
| 虚拟现实 | VR | 全合成的视野；真实房间被替换掉 | CAT207TC 主体 |
| 头戴显示器 | HMD | 头显本身 | Pico 4 Ultra |
| 六自由度 | 6DoF | 位置（3 个）+ 旋转（3 个）都被追踪 | 头、手柄 |
| 三自由度 | 3DoF | 只有旋转；位置固定 | 便宜头显、部分视频播放器 |
| 瞳距 | IPD | 两眼之间的距离，决定立体分离度 | 头显设置 |
| 视场角 | FOV | 视野有多宽，按度数算 | 舒适度、尺度 |
| 临场感 | presence | 「我确实在那里」的感觉 | 多数 VR 设计的目标 |
| 沉浸感 | immersion | 被体验吸进去（技术层面或叙事层面） | 和临场感不是一回事 |
| 具身感 | embodiment | 觉得虚拟身体就是自己身体的感觉 | 虚拟化身、手 |
| 本体感觉 | proprioception | 不用看也知道自己肢体在哪的感觉 | 手的位置为什么必须对上 |
| 视动 | vection | 世界在动，于是觉得自己在动 | 舒适度；平滑移动 |
| 模拟器不适 | simulator sickness | 视觉与内耳不一致导致的恶心 | 舒适度设置 |
| 瞬转 | snap turn | 视角一格一格地转，而不是平滑转 | 最主要的舒适工具 |
| 运动到光子延迟 | motion-to-photon latency | 从头动到画面变的时间 | 一般希望 20 ms 以内 |
| 触觉反馈 | haptics | 手柄传来的震动反馈 | 抓取确认 |
| 透视 | passthrough | 用摄像头把真实房间显示到头显里 | 混合现实功能 |
| 安全区 | guardian / boundary | 系统画出来的游玩边界 | 房间设置 |
| 空间音频 | spatial audio | 声音在 3D 里有位置，随头部朝向变化 | CAT207TC 第三次实验 |

## C# 与 Unity

| 中文 | English | 含义 | 出现在 |
| --- | --- | --- | --- |
| 字段 | field | 存在对象上的变量 | `public float speed;` |
| 属性 | property | 读写时会跑代码的成员 | `public int Health { get; set; }` |
| 方法 | method | 一段有名字的代码 | `void Jump()` |
| 类 | class | 对象的蓝图 | 每个脚本 |
| 实例化 | instantiate | 运行时创建一个对象的副本 | 子弹、敌人 |
| 序列化 | serialise | 把字段的值存进 Inspector 或磁盘 | 存档、Inspector 字段 |
| 预制体 | prefab | 存好、可复用的对象模板 | 每次实验都会用 |
| 可脚本化对象 | ScriptableObject | 不挂在场景对象上的数据资产 | 配置、技能数据 |
| 协程 | coroutine | 可以跨帧暂停和恢复的方法 | 定时效果、等待 |
| 帧间隔 | deltaTime | 上一帧花了多少秒 | 所有逐帧移动 |
| 事件 | event | 一个对象发出、其他对象监听的通告 | UI、伤害、得分 |
| 单例 | singleton | 全局可达、只允许一个实例的类 | 各种 Manager |
