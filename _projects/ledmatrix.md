---
layout: projects
name: 8X8 LED Matrix PCB
description: PCB design of an 8X8 LED Matrix
image: /assets/ledmatrix.jpg
permalink: /ledmatrix/
---

This project came about so that I could learn more about PCB design and manufacture. The design is featured around a 100mm X 100mm board size constraint from PCBWay. The design uses 16 PNP transistors, to control eacho row and column for full multiplexing. The ESP8266 is used to control two shift registers, which are connected to the transistors. The ESP8266 then hosts a webserver that is access from a phone to turn the display on or off. 

Check out the [Source on Github](https://github.com/camrbuss/8x8-led-matrix) for more information.

Schematic:

<img src="{{ "/assets/ledmatrix/ledmatrix4.jpg" | prepend: site.baseurl }}" />

Components:

<style>
table {
    width: 30%;
    border-collapse: collapse;
}
th {
    border: medium solid #999;
}
td {
    border: medium solid #999;
}
</style>

| Qty | Component |
| --- | --------- |
| 2 | Voltage Regulators |
| 2 | Shift Registers |
| 16 | FETs |
| 64 | LEDs |
| 1 | ESP8266 |

<br/>

Traces from Gerber Files:

<img src="{{ "/assets/ledmatrix/ledmatrix5.png" | prepend: site.baseurl }}" />

Final Product:

<img src="{{ "/assets/ledmatrix/ledmatrix1.jpg" | prepend: site.baseurl }}" />

CAD of PCB with printed frame:

<img src="{{ "/assets/ledmatrix/ledmatrix2.png" | prepend: site.baseurl }}" />

Final Product in frame:

<img src="{{ "/assets/ledmatrix.jpg" | prepend: site.baseurl }}" />

Video showing the multiplexing is at a greater rate than the camera frame rate:

<iframe src="{{ "/assets/ledmatrix/ledmatrix3.mp4" | prepend: site.baseurl }}"> </iframe> 

