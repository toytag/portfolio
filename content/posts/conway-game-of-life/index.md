---
title: "Conway's Game of Life"
date: 2024-01-02T10:30:30-05:00
tags: ["javascript", "wasm", "webgpu", "parallel computing"]
---

> JavaScript vs WebAssembly vs WebGPU

{{< alert icon="fire" cardColor="#e63946" iconColor="#1d3557" textColor="#f1faee" >}}
This is a performance demanding page.
{{< /alert >}}

## Introduction

![](featured.gif "A single [Gosper](https://en.wikipedia.org/wiki/Bill_Gosper)'s [glider gun](https://en.wikipedia.org/wiki/Gun_(cellular_automaton)) creating [gliders](https://en.wikipedia.org/wiki/Glider_(Conway%27s_Life)). <br> Source: [Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)")

The project conducted performance comparisons between vanilla JavaScript, WebAssembly, and WebGPU. The WebGPU version is the fastest. When tested on a desktop Intel 12600K with integrated GPU, the WebGPU compute shader achieved around 10x speed-up, with FPS capped at 60. While the other two implementations struggled to reach 20 FPS.

On the mobile end, unfortunately, WebGPU is not yet supported by mobile browsers. WebAssembly is the fastest here, surpassing its desktop counterpart. The WebAssembly version is around 2x faster than the vanilla JavaScript version. On a mainstream mobile device like iPhone, the WebAssembly version can achieve around 30 FPS.

## Demo

https://conway-game-of-life.toytag.net

The simulation resolution is 1024x1024. The embedded demo is default to WebGPU version as it more efficient and less CPU intensive.

<embed type="text/html" src="https://conway-game-of-life.toytag.net/webgpu" class="rounded-md aspect-[4/5]" width="100%" />

## References
- [Your first WebGPU app - Conway's Game of Life](https://codelabs.developers.google.com/your-first-webgpu-app)
- [Game of Life example | The AssemblyScript Book](https://www.assemblyscript.org/examples/game-of-life)
- [Conway's Game of Life - Wikipedia](https://en.wikipedia.org/wiki/Conway%27s_Game_of_Life)

{{< github repo="toytag/conway-game-of-life" >}}
