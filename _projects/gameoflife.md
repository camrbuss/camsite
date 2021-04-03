---
layout: projects
name: Conway's Game of Life
description: Conway's Game of Life on a FPGA, 32-bit MCU, and 64-bit CPU
image: /assets/gameoflife.gif
permalink: /gameoflife/
---

2020 was a hectic year with many notable losses. John Conway the incredible English mathematician past away, and thus started my journey of
implementing Conway's game of life in several ways.

### [x86_64 with Python3 and WX GUI](https://github.com/camrbuss/gol-wx-py)

My first goal was to improve my Python knowledge and implement *The Game of Life* on my desktop computer. I started with this so that I could
implement all the features below to aid in understanding the game.

- Play
- Pause
- Reset
- Single Step
- Stagnant animation detection
- Auto Restart upon stagnant animation
- Image saving (in current directory)
- Speed Adjustment
- Grid Size Adjustment

<center><img src="https://raw.githubusercontent.com/camrbuss/gol-wx-py/main/demo.gif" /></center><br/>

### [STM32F103 with 12864 Display](https://github.com/camrbuss/gol-bluepill-12864)
After understanding the basics of implementing *The Game of Life*, I moved on to implementing it in C on a STM32 using libopencm3.
The implementation was easier than I expected as I just had to write a 12864 driver, implement Marsaglia's Xorshift for randomization,
and finally use C arrays/pointers to calculate cell states. This step was a lot of fun because the hardware is easily to procure and libopencm3 makes
using all the STM32 peripherals a breeze.
<center><img src="https://raw.githubusercontent.com/camrbuss/gol-bluepill-12864/main/demo_img.png" /></center><br/>


### [FPGA with Hub75 LED Matrix](https://github.com/camrbuss/gol-ice40-hub75)
Finally moving onto the most difficult implementation for me... using the iCE40UP5K FPGA to drive a Hub75 LED Panel while playing *The Game of Life*.
This was my first project using an FPGA and Yosys, so I spent a good time in Icarus Verilog simulating how everything worked. Because simulation
doesn't have any LUT limitations, I initially implemented *The Game of Life* using `wires` and `assign` statements so the next state of the game could
be determined in one clock cycle. This worked, but ate through the iCE40UP5K LUTs really fast so I could only display a 16x16 game.
I then realized that I needed to use RAM in some form to store the current and future state of the game. This unveiled a whole new world of
FPGAs to me and I went down the path of learning implicit and explicit BRAM/SPRAM. I really needed to use BRAM, but for some reason I could not
get Yosys to infer BRAM correctly and the explicit BRAM definition also did not seem to be working. For this reason, I used explicit SPRAM and a lot of it.
See the GitHub repository for details on the implementation, but in short it takes about 2000 clock cycles to determine th next state which seems like a lot,
but at 24MHz you can play the game faster than the eye can see.
<center><img src="{{ '/assets/gameoflife/gameoflife1.png' | prepend: site.baseurl }}" /></center><br/>
