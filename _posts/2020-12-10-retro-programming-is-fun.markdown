---
layout: post
title:  "Retro Programming is Fun!"
date:   2020-12-10 09:12:17 +0330
categories: retro
---
As few months ago I decided that I wanna learn 6502 Assembly. Prior to that, I had only studied some non-standard version of Assembly on a virtual assembler, which our teacher told us is close to old ARM assembly, and even then, I hadn't learned much as I dropped out that same semester. I knew that 6502 doesn't have that many instructions in its set. The assembly we learned had C-like functions. 6502 was much simpler than that. I started learning using a few old and new sources and a JavaScript emulator. I found it fascinating that a machine-adjascent language which, 40 years ago, required a microprocessor to run, is now interpreted by a browser language. Amazing stuff. Moore's law predicted hardware progress, but its byproduct, software progress, is advancing even at a faster pace.


![6502](/assets/img/6502.jpeg)

After I graduated from "abstract emulator" college, I decided to tinker with emulated interfaces which were once connected to 6502 bus. Atari 2600's TIA and PIA, Nintendo's PPU, and Commodore 64's SID chip come to mind. Back then, each console and personal computer had different interfaces connected to the CPU's bus. And 6502 is still the king of variety in interfaces. A lot of small devices used, and still use, 6502. Did you know that Tamagotchi had a 6502? Also Bender from Futurama had a 6502. 

![Bender](/assets/img/bender.jpeg)

He was 40 lead, and 100% awesome.

Now, I didn't have time to produce actual games or music for these machines. All I could do was to tinker with them. And throughout the process of tinkering with them, I understood a lot of things.

First thing I did was trying to abuse the stack. The 6502's stack is 256 bytes. So I set the stack pointer at some address, and sta'd the number 16 at it until it overflew. Mission accomplished! I did it on C64 only. I saw a post online where someone did it with BASIC and BASIC actually checked for overflow, but I'm close to hardware baby, I'm randy and I wanna screw with stack. Can't stop me.

I then tried to make a "sprite" with Atari 2600. You see, Atari 2600 doesn't work like modern consoles. It has an "engine". It has two players, a field, a ball, and a bunch of missiles. This is because when Atari VCS came out, vidya like Pacman and Space Invaders weren't relased back then. The frame of mind for Atari was arcades like *Tank* and *Pong*. You can find the code for Atari 2600 "sprites" [here](https://8bitworkshop.com/v3.7.0/?platform=vcs&file=examples%2Fsprite.a). It is achieved by stacking up player character pixels. 

Speaking fo 8-bit workshop, their books are an awesome source for learning 6502. They also have this IDE which is slick and useful.

One source for 6502 which is often overlooked is [this](https://archive.org/details/6502GamesRodnayZaks/6502%20Games-Rodnay%20Zaks-OCR-Print/mode/2up) book. It's so old, so archaic, that seems like it's translated from Cuneiform. It has two sections, 6502 game dev with personal microcomputers, and with devices called "6502 game boards". It's a small 6502-based PCB with switches and LEDs connected to the bus. The amount of memory is very small, but it's not like you're gonna load memory address after memory address with color values. 

I could not find an emulator for these PCBs. I know that Mame emulates PCBs but I'm just an idiot with a computer who knows how to program, I ain't know how to do them cool stuff. If anyone has one of these Game Boards, and is not me, please make an emulator kthanx xoxo.

But what I can do is just use NES or C64 to write the smae code and use pixels or ASCII chars instead of LEDs and APU or SID for the sound interface, and well, toggling keys instead of switches. Toggling keys is difficult, and what I'm currently working on. These PCBs have a memory address for toggling the switch, but I have to toggle, and untoggle. "untoggle" is a weird verb. Toggle means two states for one action. But "untoggling" maans two actions for two states. I'm screwing with your brain huh? Don't worry, I can't work it out either.

Another book I found was about AI with C64. Hey, not that AI that we use today, but small state machines that back in the day were referred to as "expert systems". This is possible with 6502 ASM, it's hard, but possible. Only if your state machine does not have complex states. The book is in BASIC but I'm transating it to ASM. We've all seen the unfunnaly "AI means a lot of if statements" meme repaeted by people who think they're funny. Well, in 6502, we don't have `if` statements, we have `branch` statements. Like:

```
loop:
ldx #0
lda (source mem addr), x,
sta (destination mem addr), x
inx
cpx #10
bne loop
```

So here we first load 0 (decimal numbers, and any numbers that aren't memory addresses, are annotated with #) into the X regiser. Then we load the source memory address, let's say an sprite, then we store that sprite into the destination memory address, let's say the PPU. Then we increase X, compare X with the number we wish to be the maximum and if it's not that number, we "bne" recursivly. bne means "branch if not equal".

This is simple, but a bit verbose. 

Let's get philosophical for a minute. All the games written in most peoples' childhoods (well I was born in 1993, games of my childhood were written in C and used PSGL) used this simple subroutine A LOT. So you may ask, how does it work?

Well, as I said, it all depends on the interface which is connected to the CPU. We just load binary numbers into memory addresses after doing operations on them. These operations are done in the A register. The X and Y register are used for many things, including iteration. 

This is not different than a modern computer, but modern computers are less hard-electronics and more soft-electronics. A modern CPU only has a few off-the-shelf interfaces connected to its bus. The operating system is in charge of conveying the messages from these interfaces to the CPU and then back to output devices (I know I've made an error here, tell me please). But in 6502 Assembly, we have a direct line to these interfaces. In exchange, we can't modify them much. We get what the interface is designed to do. We can use a modern GPU to do floats operations, we can do float operations for anything we want. Tensor processing, 3D rendering, or playing games. But a NES PPU is designed for one thing, processing the sprites.

You came all the way here, let me explain how Atari 2600 works.

### Atari VCS

Two main interfaces are present in Atari 2600m, TIA and PIA. The former is a chip designed by Atari and the latter is an off-the-shelf chip. The following is extracted from [Stella Atari 2600 guide](http://www.classic-games.com/atari2600/stella.html).

TIA is the main player here. It works in tandem with the TV's electron beams to draw on the screen. It divies the screen into five sections:

![VCS Screen](/assets/img/vcs_scanline.png)

Each of the 192 lines is called a "scanline". CPU cycle is equal to three TIA clocks. We only have 22 CPU cycles (68/3) to set the registers on each game loop. This period is called as HBLANK. We have to incite the `WSYNC` register on the memory map to "wait for the horizontal sync". This is the time from when a TV starts to draw a scanline until it reaches the actual edge of TV.

PIA is in charge of making it work on all TVs. Since some TVs are different from the rest. It's basically a timer.



***


Ok, but you may ask, Chubak, why? Just why? These devices are old and stupid. But let me tell you the difference between "old" and "retro".

Retro is old when it's cool.

For example, an old IBM PC from the early 80s is old. But Apple II GS is retro. A Fairchild Channel F is old, but Atari VCS is retro. They're retro because they were cool when they came out. IBM PC was not cool. But Commodore 64 was. Like, have you ever seen someone collect Dell laptops from the early 2000s? No, because they're old. But people do collect old Macbooks, because they're cool (I'm not a Mac user, but I do admit that they're cool).


People my age, who were born in the early 90s, have a romanticized vision of the 70s and the 80s. So much that people have made a 'fantasy console' which uses its own dialect of BASIC to write old games. You can find it [here](https://www.lexaloffle.com/pico-8.php). This is literally retro programming with new technology.

At the end, it's just FUN. F-U-N fun. 

Thanks for reading my post. If you have any questiosn contact me on Discord (Chubak#7400).





