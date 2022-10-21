# Playing arcade games on original hardware in 2022

Ever get the urge to relive old classic arcade games? Some games have been ported to other platforms, reproduction systems, or run on emulators. But there's a different kind of experience playing the games on the original hardware for an authentic experience. I recently tried to do that.

Of course, arcade cabinets take up a lot of room, so it would be nice to play the games and have a decent way to store them. I'm here to tell you that there is a way.

## What you need

- [Game board(s)](#Game_boards)
- [Supergun](#Supergun)
- [Video](#Video)
- [Audio](#Audio)

### Game boards

![image](https://user-images.githubusercontent.com/4001640/197288883-fb6dea9f-42b1-44bd-831c-0caef5922003.png)

The actual computer for arcade games are typically just big motherboard PCBs. Most PCBs connect to power, input, video, and sound through a wiring harness standard called JAMMA. JAMMA makes it really easy to repurpose an existing arcade cabinet for use with a new game, saving cost, space, and so on. Games that are older than JAMMA can be used on JAMMA by making a converter. 

On this image, you can see the JAMMA pinout on the left side.
![image](https://user-images.githubusercontent.com/4001640/197288883-fb6dea9f-42b1-44bd-831c-0caef5922003.png)

I got extremely lucky by finding a craigslist ad and was able to purchase the following for $250
1. Street Fighter II Rainbow Edition [a very famous bootleg](https://en.wikipedia.org/wiki/Street_Fighter_II:_Rainbow_Edition)
1. Street Fighter II Hyper Fighting
1. Killer Instinct
1. Street fighter III New Generation
1. Gauntlet Legends
1. Golden Tee '98

Here's a picture of all of the boards together.
![image](https://user-images.githubusercontent.com/4001640/197286800-b3f0bad1-0858-4afc-a7f2-3b209833d67c.png)

I don't own a cabinet, so I need to buy something new instead. So, just find something that connects from JAMMA to a TV, a controller, and plug it into the wall, right? Well... not as simple as I originally thought...

### Supergun

![image](https://user-images.githubusercontent.com/4001640/197289828-f79dea49-69bf-4390-835f-41c938ac36f9.png)

A Supergun connects from JAMMA and lets you connect your input, power, video, and sound. You can find them on ebay, buy from makers, or even make your own using [open source plans called the Minigun](https://www.arcade-projects.com/threads/minigun-supergun-an-open-source-supergun.9408/). Your Supergun will affect the rest of your purchases, so you should pick that out first.

I wasn't particularly interested in soldering an entire PCB myself. [So, I purchased a Supergun Minigun from here.](https://trp-retromods.ca/index.php?route=product/product&path=72&product_id=80)

### Video

It's not as simple as plugging a video cable from your Supergun to your TV, at least in the United States. It's slightly complicated, but for good reasons. If you don't care about the reasons, I suggest getting the 8pin/9pin to SCART cable (depending on your supergun), a transcoder to the display you are going to use, and then connect it to that display.

Arcade games (and your Supergun) output a video signal in a format called RGB. That signal is different from the Yellow/White/Red Composite cables found in US TVs, so you'll need cables and some kind of device to convert the signal. It's actually closer to the format used in computer monitors with VGA. However, most VGA computer monitors don't handle the refresh rate the arcade game requires, so that will also require an extra converter.

On the Supergun Minigun, you can pick either a miniDIN 8pin or 9pin output connection. 8pin is probably more purest, but 9pin is a good option if you already happen to have a cable using 9pin. This is more common in Europe... more on that later.

There's a lot of options here for cables and displays. Here's a summary of all the relevant analog connections that you could run into...
1. miniDIN 8pin/9pin -> the output of the Supergun Minigun
1. SCART -> RGB connection standard, probably the highest quality you can get for analog video. Common on European CRTs, not so much in the US.
1. BNC -> another RGB connection standard, used on PVMs. Simple cable conversion from SCART to BNC. More on that later.\
1. Composite -> Yellow/Red/White Connection on most US TVs. Not RGB signal. Probably used as it was cheaper, sacrificing video quality
1. SVideo -> miniDIN but a different pin setup. Not RGB signal. One step up than composite.
1. Component -> Red/Blue/Green/... connection on some US TVs. Not RGB signal. One step up from SVideo.
1. VGA -> 15 pin connector. Common on CRT Computer monitors.

Here are your display options

1. CRT TV -> Reasonably common option. In the US, can have Composite, Component, or SVideo, and would require a converter. In Europe, you might have SCART.
1. CRT Computer monitor -> Also common, typically with VGA. Requires a converter if your display doesn't handle the arcade refresh rate.
1. PVM -> Probably the best option, but the hardest to find. These displays were used as TV Studio monitors with high quality colors and low latency. They typically have BNC (no converter needed!)

Of course, there are digital options, with HDMI, and LCD displays. The games weren't designed with these displays in mind, and I'm a purest, so I would avoid these.

I reccomend going from your Supergun to a SCART cable, and then get transcoders/cables necessary depending on the display that you have.

Right now, I have a basic CRT TV with a Composite connection that I found on the side of the road. So, I got a SCART cable and a composite transcover.
![image](https://user-images.githubusercontent.com/4001640/197292829-43b056bf-1708-4267-b179-53d30865b09a.png)

[Transcoder](https://www.ebay.com/usr/wakabavideo?_trksid=p2047675.m3561.l2559)

[SCART Cable](https://retro-access.com/)

I hope to upgrade to PVM one day. Moving on!

### Audio
