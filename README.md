# Digital Tears
A modular system for building digital effects for guitar, vocals and synthesisers.

# Features
* Separate, modular parts that can be reused to create different projects with a minimal effort on the hardware side.
* Based around "easy" to source parts.
* Powerful; the intial design uses the Teensy 4.0 platform (Arm Cortex-M7 600Mhz with hardware FPU).
* High fidelity. Codec board provided, based around the Cirrus Logic CS4272 stereo codec chip. Supports 48KHz, 24bit. Mono in, stereo out.
* Codec and extension boards can be re-used in your own projects.
* Extension boards (WIP) for Stereo out, MIDI and UI.
* Basic software environment provided (WIP).

# Long term goals
The intention is to develop a "spin-killer" that is entirely open source and, to a reasonable extent, silicon agnostic.

There are three planned stages to development, with the current revisions inteded to meet stage 1:

* Stage 1 - Development of open source "platform" that uses readily available parts and allows us to build and ship our own commercial pedals, whilst also allowing other people to jump in and do the same. This phase is intended for short-runs rather than large scale manufacture.
  
* Stage 2 - Moving away from off the shelf parts (Teensy) into integrating our own MCU/DSP part(s) into the modular system. To allow for more processing power and flexibility, whilst also keeping portability in mind (the potential to offer multiple versions with different silicon).
  
* Stage 3 - Development of a visual programming language that allows users to create a range of DSP effects without having to roll code by hand, in a similar vein to SpinCAD. The intention at this point is to use some sort of lightweight virtualised environment to aide portability and also make for a cleaner developer experience. The success of this would largely depend on how close to native we could get performance, whilst also leaving the door open to more bespoke implementations that access architectural features on individual silicons.

# Why?

We want to develop a way to easily design and manufacture digital effects units for guitars, vocals and synthesisers that can meet the goals we have for generating money to fund musical projects, whilst also presenting a way for others to also do the same.

Our system is largely inspired by the success of the Spin FV-1. If you're not familiar with the Spin, it's a dedicated DSP silicon designed for low cost and good performance for a range of effects tasks. One of the features we really love is the SpinCAD visual editor, that allows people with minimal programming experience to design bespoke effects, which they can then manufacture and sell. It's a great platform.

However, the big drawback is a lack of portability - the fact that it's a dedicated piece of silicon means that it is hard to source and that makes mid and large scale manufacturing difficult. What we propose is to develop an open platform with a focus on visual programming that is, to a large extent, silicon agnostic. That is to say, that the platform would eventually either support multiple MCUs, or that porting to a new silicon is something that would be reasonably straightforward, should the chosen chip become scarce or no longer be supported.
