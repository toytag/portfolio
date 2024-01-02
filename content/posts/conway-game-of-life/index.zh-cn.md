---
title: "Conway's Game of Life"
date: 2024-01-02T10:30:30-05:00
tags: ["javascript", "wasm", "webgpu", "parallel computing"]
---

> JavaScript vs WebAssembly vs WebGPU

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
此页面对性能要求较高。
{{< /alert >}}

## 引言

![](featured.gif "由[Gosper](https://en.wikipedia.org/wiki/Bill_Gosper)设计的[滑翔机枪](https://en.wikipedia.org/wiki/Gun_(cellular_automaton))，和其制造的[滑翔机](https://en.wikipedia.org/wiki/Glider_(Conway%27s_Life))。 <br> 来源：[维基百科](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)")

该项目对原生 JavaScript、WebAssembly 和 WebGPU 的性能进行了比较。WebGPU 版本是最快的。在搭载集成 GPU 的英特尔 12600K 桌面环境下测试，WebGPU 计算着色器的速度大约快了 10 倍，FPS 达到上限为 60。而其他两种方法则难以达到 20 FPS。

在移动端，不幸的是，移动浏览器尚未支持 WebGPU。WebAssembly 是这里最快的，超过了其桌面版本。WebAssembly 实现大约比原生 JavaScript 实现快 2 倍。在像 iPhone 这样的主流移动设备上，WebAssembly 可以达到大约 30 FPS。

## 演示

https://conway-game-of-life.toytag.net

模拟分辨率为 1024x1024。嵌入式演示默认为 WebGPU 版本，因为它更高效，CPU 占用更低。

<embed type="text/html" src="https://conway-game-of-life.toytag.net/webgpu" class="rounded-md aspect-[4/5]" width="100%" />

## 参考资料
- [您的第一个 WebGPU 应用 - 康威生命游戏](https://codelabs.developers.google.com/your-first-webgpu-app)
- [生命游戏示例 | AssemblyScript 书籍](https://www.assemblyscript.org/examples/game-of-life)
- [康威的生命游戏 - 维基百科](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)


{{< github repo="toytag/conway-game-of-life" >}}
