---
date:   2018-04-24 15:01:35 +0300
title: "Valorant Aimbot [Proof Of Concept]"
image: '/assets/images/valorant_bot/valorant-aimbot-header.png'
tags:   [coding, hardware]
---
Is it perfect? No.

Does it work? Sometimes.

Do I trust this more than I trust my own aiming skills? Absolutely Not

## General Concept:
The program is broken up into roughly 3 steps:

1. Mss takes a 250 x 250 screenshot of the area around the crosshair
2. Opencv uses thresholding, masking and contouring to draw bounding boxes and estimate the location of the head
3. The relative position of the head is send to the raspberrypi that is acting as a HID mouse

## Can you get banned?
No, since it does not affect Valorant in any way, and interfaces with it in a way that looks legitimate (the rpi0 is indistinguishable from a normal mouse) 
it is undetectable and therefore, you can’t get banned. 
It uses the same information that is available to a player, however the program is able to process it faster and more precisely

## How it Works:

- This is the raw input that the player sees, mss is able to screenshot at ~30fps.

<figure style="width: 248px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/valorant_bot/VA-raw.png" alt="">
</figure> 

- This is the first and most important masking pass, it cuts out all other colours except for a very particular shade of purple, after the inital mask, some slight thresholding is done to 
improve the contouring, and allow the contouring to accurately identify enemy players.

<figure style="width: 248px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/valorant_bot/VA-masked.png" alt="">
</figure> 

- While this is just basic contouring to find all connected edges, it is helped by thresholding and masking.

<figure style="width: 248px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/valorant_bot/VA-contoured.png" alt="">
</figure> 

- A bounding box is drawn around all the contours, and the largest one (which is *usually* an enemy player) is identified. The head is found by taking the width of the box // 2
and the height of the box // 8. It's not exact, but it does the job in most scenarios.

<figure style="width: 248px" class="align-center">
  <img src="{{ site.url }}{{ site.baseurl }}/assets/images/valorant_bot/VA-head.png" alt="">
</figure> 

All 4 components in action: 

![1. Raw Input](/assets/images/valorant_bot/VA-raw.gif){: .w-15 .normal}
![2. Masked](/assets/images/valorant_bot/VA-masked.gif){: .w-15 .normal}
![3. Contours](/assets/images/valorant_bot/VA-contoured.gif){: .w-15 .normal}
![4. Estimated head location](/assets/images/valorant_bot/VA-head.gif){: .w-15 .normal} 

The regular head location (the red dot) is then sent to the raspberrypi along with a "left-click" command.
