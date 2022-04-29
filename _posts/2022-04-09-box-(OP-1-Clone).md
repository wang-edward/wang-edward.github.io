---
title:  "box (OP-1 Clone)"
layout: post
categories: media
---

I started working on my new synth, which is a clone of the OP-1 this time.

To start, I was heavily inspired by Prajwal Mahesh's [Portable Synth](https://github.com/prajwal1121/Portable-Synth "Portable Synth"), as well as the [OTTO Project](https://github.com/bitfieldaudio/OTTO "OTTO Project")

I'm going to try and make updates to this post as I go, as last time it was really annoying to go back and reupdate things.

### April 29th:
Got pixel drawing working

{% include embed.html url="https://www.youtube.com/embed/o6XfxsDwJVM" %}

I'm using the [Midpoint circle algorithm](https://groups.csail.mit.edu/graphics/classes/6.837/F98/Lecture6/circle.html) to draw circles

![Midpoint_circle_algorithm_animation_(radius_23)](https://user-images.githubusercontent.com/53409587/165981810-d5f8e860-e29e-44f0-a1db-a31bd49ea962.gif)

Circles are very symmetrical, so you can save a lot of time by just making 1 computation and repeating it 8 times
![image](https://user-images.githubusercontent.com/53409587/165980601-e8f2ef77-3555-4e8a-b03e-036479ac3df4.png)



### April 28th:
Got wav visuals working
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


### April 15th:
Got the subtractive synthesizer working. 
* For now it's just the basic tutorial one (16 sines and 16 envelopes), but I think it'll be pretty easy to change the architecture later. 
* Also organized the sampler into classes. Side note: Allolib + Gamma is really cool and handles all the complex code so it's actually really easy to make cool stuff.

I also got started ordering proof of concept parts, and GOD DAMN are SMD breakout boards expensive (Adafruit ones are ~$2.5 each but almost all others are >$7)
{% include embed.html url="https://www.youtube.com/embed/9Ugzczsdl9c" %}

### April 9th:
Got the MPC3000 clone working
* Had some trouble making it OOP, but it works I guess ¯\_(ツ)_/¯
{% include embed.html url="https://www.youtube.com/embed/0GinlhEjZQA" %}
