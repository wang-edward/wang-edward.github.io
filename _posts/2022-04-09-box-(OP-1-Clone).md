---
title:  "box (OP-1 Clone)"
layout: post
categories: media
---

# Latest:
{% include embed.html url="https://www.youtube.com/embed/7L5JRVEC_is" %}

## August 9th: PCB almost done
- Pretty close on the PCB, gonna order this week I think
- Here's some pics :)
- ![fullboard](https://user-images.githubusercontent.com/53409587/183802078-edd3fed3-85ce-42bb-8090-d994eae4b353.png)
- Also here's an interesting pic of the USB-C port
- ![E pic](https://user-images.githubusercontent.com/53409587/183802199-8f8bcf2d-81b8-4b5e-8264-6ef077503669.png)


## July 7th: timeline + new terminal setup
- Kinda busy lately but implemented recording/timeline
- Idk how optimized it is (I feel like I'm checking positions super redundantly + wasting alot of memory)
- Fun Anecdote: got stuck for like a week on the difference between deque & vector and push_back & emplace_back

### ALSO I got a new terminal setup (copied from jdh)
<img width="1440" alt="Screen Shot 2022-07-07 at 5 21 47 PM" src="https://user-images.githubusercontent.com/53409587/177873498-b48e6c27-701e-46dc-96a3-c8e8f12b742b.png">

## June 26th: Built box on the radxa zero
{% include embed.html url="https://www.youtube.com/embed/7L5JRVEC_is" %}
- Basically the same process as allolib/allotemplate
- Made me realise that I did a ton of unnecessary setup (linking my own gamma/sndfile libraries for example)
- Pretty cool that it works
- Also got GPIO working
- Once I have an i2c demo done I'm gonna order the PCBs!!!

## June 20th: Built allolib on the radxa zero
{% include embed.html url="https://www.youtube.com/embed/eoLglYR7Tn8" %}

I decided to build natively instead of cross compiling
- Native compiles literally take >20 minutes 
- I think overall the compile time doesn't matter that much since I only need to rebuild when I add a new library

This actually took me a really long time (~3 weeks)

Ran into a bunch of issues

- No OpenGL support :(
  - I actually started with a Raspberry pi 4 (thanks to the Gaming Warlord Titan Pharaoh Jacob Lin)
  - Raspbian only has support for OpenGL 3.1 for some reason, even though the hardware is capable of 3.2
  - GLFW (windowing system) needs OpenGL 3.2 as a minimum, so it could never create the window
  - I didn't know about this (or that the radxa zero actually supported 3.2) so buying the radxa was complete luck 
- No wifi support for AP6212 chip (wifi/bluetooth module was replaced on the radxa zero due to supply issues, many distributions didn't have firmware
- Had to flash ~5 times? Manjaro, 3 versions of Armbian, and I also tried starting with ubuntu server and manually downloading a desktop environment
  - eventually succeeded with debian desktop (which actually seems like the most straightforward approach, not sure why I didn't do it first)

## June 6th: HDMI LCD display came!!

{% include embed.html url="https://www.youtube.com/embed/J8syuFlKLwY" %}

There's still a lot of problems with this design:
- HDMI audio works, but I can't get pulseAudio (with Gamma) to recognize it as a device
- I bought the wrong display 💀 (so there's no screws to attach it)
- Lastly, I still haven't figured out cross compiling 

## May 16th: Hardware - I2C GPIO Expander

Got my prototype GPIO expander working.

{% include embed.html url="https://www.youtube.com/embed/GrQHEn_yZ8Y" %}

I'm using the CAT9555, an I2C chip with 16 input/output pins.

It communicates over I2C, which is really nice because it only uses 2 pins, SDA and SCL

Additionally, you can chain them together, with up to 8 on the same 2 lines by modifying the I2C address pins, which lets you have up to 128 IO lines with only 2 pins!!!! (8 IC's should still be within the [3mA limit](https://www.nxp.com/docs/en/user-guide/UM10204.pdf)
![i2c_diagram](https://user-images.githubusercontent.com/53409587/168877401-1ec0f126-641b-45dc-bb24-0303db834b39.png)
credit: [bluedot](https://www.bluedot.space/tutorials/how-many-devices-can-you-connect-on-i2c-bus/)


## May 4th: Got pixel image rendering working

<img width="400" alt="doom_render" src="https://user-images.githubusercontent.com/53409587/166711911-4c0426ed-5d08-4172-9c36-9cc70f3c1aba.png"> <img width="400" alt="rectangle_test_render" src="https://user-images.githubusercontent.com/53409587/166711983-8da3eee8-0c06-4d64-9063-c0d45b3e50f6.png">

I'm using allolib's Image class, which stores images as a vector of uint8_t's

Every 4 indices represents a pixel's Red, Green, Blue, and transparency values (stride = 4)

I'm then just iterating through the pixels to plot each one on the screen. You have to iterate in reverse because stb has a different coordinate system than openGL (see below). This means if I draw it regularly, it will actually be flipped upside down. I think the "official" way to fix this is to run:
```cpp
stbi_set_flip_vertically_on_load(true);
```

<img width="942" alt="opengl_vs_stb_coords" src="https://user-images.githubusercontent.com/53409587/166716700-215ef188-0ce1-4457-9ee8-ca3f2eb63a14.png">

The only thing was, I was worried that it would mess up other texture rendering services (in allolib), so even though it's more annoying this way, it will probably end up better overall.

I feel like in general the double for loop is a super slow approach, but I'm also not sure how to make it better. The one thing I did think of is to not run plot_pixel() if the pixel has a transparency of 0 (visibility of 0?) because it won't be visible anyway.
```cpp
int position = x_position * t_width + y_position;

for (int y=image.height()-1;y>=0;y--) { 
    for (int x=image.width()-1;x>=0;x--) {
    
        int red = get_image_index(x, y, image);
        int green = red + 1;
        int blue = red + 2;
        int a = red + 3;

        if (a!=0) {
            al::Color c (image.array()[red]/255., image.array()[green]/255., image.array()[blue]/255., image.array()[a]/255.);

            plot_pixel(c, x_position - x, y_position - y);
        }
        
    }
}
```

Here's how I got the image to be centered, one kind of annoying thing is that I made the MF DOOM head asymmetrical (odd pixel width) when I drew it, so I can't actually center it. Kinda nice how the integer rounding is actually useful here though!
<img width="777" alt="Screen Shot 2022-05-04 at 11 15 11 AM" src="https://user-images.githubusercontent.com/53409587/166713401-b8dad759-1e91-43a8-8588-c9763507a1ef.png">

ALSO note to self: use square images that are multiples of 2^n i.e. 2x2, 4x4, 16x16, 2048x2048 because they are better optimized or something

## April 29th: Got pixel drawing working

{% include embed.html url="https://www.youtube.com/embed/k90510pb15o" %}

I'm using the [Midpoint circle algorithm](https://groups.csail.mit.edu/graphics/classes/6.837/F98/Lecture6/circle.html) to draw circles

![Midpoint_circle_algorithm_animation_(radius_23)](https://user-images.githubusercontent.com/53409587/165981810-d5f8e860-e29e-44f0-a1db-a31bd49ea962.gif)

Circles are very symmetrical, so you can save a lot of time by just making 1 computation and repeating it 8 times
![image](https://user-images.githubusercontent.com/53409587/165980601-e8f2ef77-3555-4e8a-b03e-036479ac3df4.png)



## April 28th: Got wav visuals working
{% include embed.html url="https://www.youtube.com/embed/_pIKUf-R2VU" %}
* Also started working on 2d pixel art filter
* I think the basic idea is to render to a frame buffer of the desired size (240 x 160), and make that into a texture.
* Then, you bind the texture to a quad that covers up the entire screen (-1,-1) -> (1,1)
* The upscaling is done with [Nearest-Neighbour interpolation](https://www.youtube.com/watch?v=AqscP7rc8_M), which maintains the sharp lines of the pixel art
![image](https://user-images.githubusercontent.com/53409587/165774296-47e8ad70-f5c1-41f2-9648-a11cb91a887a.png)

* Now I just need to figure out how to do that using the ALlolib system

#### Art style & Creative direction
* One thing that I was thinking about was that I didn't want the box to be a carbon copy of the OP-1, so I think I'm going to go with a 2d pixel art style
* I really like the look of this game, Celeste
![image](https://user-images.githubusercontent.com/53409587/165767231-64fb86c6-ddad-4dd9-b673-52ea5df1e7ec.png)
* It runs at a native resolution of (320 x 180), which is quite comparable to classic retro games

* Here's a size comparison (from Brandon James Greer)
<img width="1440" alt="pixel_art_sizes" src="https://user-images.githubusercontent.com/53409587/165768788-c050768d-96f0-4634-bfe3-8a27e7f109be.png">
* Right now my plan is to run a (480 x 320) at 3" or 3.5" display, and then downsample it to (240 x 160). This means every "pixel" will be made up of 4 pixels.
  * I think this gives it enough of a retro look (equivalent to Gameboy Advance), but also gives the freedom to upsample if needed (i.e. wav visuals should be higher resolution)


## April 15th: Got the subtractive synthesizer working. 
* For now it's just the basic tutorial one (16 sines and 16 envelopes), but I think it'll be pretty easy to change the architecture later. 
* Also organized the sampler into classes. Side note: Allolib + Gamma is really cool and handles all the complex code so it's actually really easy to make cool stuff.

I also got started ordering proof of concept parts, and GOD DAMN are SMD breakout boards expensive (Adafruit ones are ~$2.5 each but almost all others are >$7)
{% include embed.html url="https://www.youtube.com/embed/9Ugzczsdl9c" %}

## April 9th: Got the MPC3000 clone working
* Had some trouble making it OOP, but it works I guess ¯\_(ツ)_/¯
{% include embed.html url="https://www.youtube.com/embed/0GinlhEjZQA" %}

## April something: starting point

I started working on my new synth, which is a clone of the OP-1 this time.

To start, I was heavily inspired by Prajwal Mahesh's [Portable Synth](https://github.com/prajwal1121/Portable-Synth "Portable Synth"), as well as the [OTTO Project](https://github.com/bitfieldaudio/OTTO "OTTO Project")
I'm going to try and make updates to this post as I go, as last time it was really annoying to go back and reupdate things.
