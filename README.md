# Digital Tears
A modular system for building digital effects for guitar, vocals and synthesisers.

# Features
* Separate, modular parts that can be reused to create different projects with a minimal effort on the hardware side.
* Based around "easy" to source parts.
* Powerful; the intial design uses the Teensy 4.0 platform (Arm Cortex-M7 600Mhz with hardware FPU).
* High fidelity. Codec board provided, based around the Cirrus Logic CS4272 stereo codec chip. Supports 48KHz, 24bit. Mono in, stereo out.
* Codec and extension boards can be re-used in your own projects.
* Extension boards [WIP] for Stereo out, MIDI and UI.
* Basic software environment provided [WIP].
* Reference design for digital control of analog circuits [WIP]. To show how to integrate analog effects that are controlled digitally.

# Why?

We want to develop a way to easily design and manufacture digital effects units for guitars, vocals and synthesisers that can meet the goals we have for generating money to fund musical projects, whilst also presenting a way for others to also do the same.

Our proposed system is largely inspired by the success of the Spin FV-1. If you're not familiar with the Spin, it's a dedicated DSP silicon designed for low cost and good performance for a range of effects tasks. One of the features we really love is the SpinCAD visual editor, that allows people with minimal programming experience to design bespoke effects, which they can then manufacture and sell. It's a great platform.

However, the big drawback is a lack of portability - the fact that it's a dedicated piece of silicon means that it is hard to source and that makes mid and large scale manufacturing difficult. What we propose is to develop an open platform with a focus on visual programming that is, to a large extent, silicon agnostic. That is to say, that the platform would eventually either support multiple MCUs, or that porting to a new silicon is something that would be reasonably straightforward, should the chosen chip become scarce or no longer be supported. We propose to achieve this by using a simple virtualised environment that abstracts away the architecture. 

This has some strengths; 
* Portability - the ability to maintain a consistent feature set across a range of architectures
* Clean development experience - the ability to offer a very clean development experience specific to audio effects, with no terrible IDEs and toolchains that often dog embedded development.

But also some risks
* Lower performance compared to getting closer to the metal.
* Some architecture-specific operations and features will not be available without extra work to expose them, which would not be forward compatible or portable to other systems. This would also not be officially supported but would be left up to advanced users to figure out.
* Added difficulty in profiling and debugging. Predicting system resource usage at design time might prove a problem.

Some of these risks can be mitigated in the sense that users can still, if they choose, develop effects for the hardware in the traditional way.

Another advantage to using modular parts would be reliability - if many devices ship using the same components, flaws in those components and the system as a whole can be iterated out more effectively. When reliability of a part increases, everybody benefits from a reduction in after sales support. Once modules have shipped many times, confidence in the reliability of those parts can be assumed as a strength of the system.

# Long term goals

The intention is to develop a "spin-killer" that is entirely open source and, to a reasonable extent, silicon agnostic.

There are three planned stages to development, with the current revisions inteded to meet stage 1:

* Stage 1 - "Proof of Concept" - Development of a proof of concept for a platform that uses readily available parts and allows us to build and ship our own commercial pedals, whilst also allowing other people to jump in and do the same. This phase is intended for short-runs rather than large scale manufacture.
  
* Stage 2 - "Platform Development" - Moving away from off the shelf parts (no more Teensy dev boards) into integrating our own MCU/DSP part(s) into the modular system. To allow for more processing power and flexibility, whilst also keeping portability in mind (the potential to offer multiple versions with different silicon).
  
* Stage 3 - "Visual Software" - Development of a visual programming language that allows users to create a range of DSP effects without having to roll code by hand, in a similar vein to SpinCAD. The intention at this point is to use some sort of lightweight virtualised environment to aide portability and also make for a cleaner developer experience. The success of this would largely depend on how close to native we could get performance, whilst also leaving the door open to more bespoke implementations that access architectural features on individual silicons for more advanced users. Maintaining software forward compatibility so ensure that effects developed on first iteration devices continue to work into the future.

# Current issues

We're currently in Stage 1 development - building a simple proof of concept that can also ship in a commercial pedal in limited numbers without serious manufacturing or support issues.

* The system is considerably more "bulky" than dedicated silicon - it's unclear whether we could fit the system into a small format pedal enclosure whilst still maintaining a certain degree of modularity.

* Off-chip memory performance is quite poor (SPI SRAMs). When using DMA read/write appears unreliable -  lowering the speed of the SPI clock helps, this points to a potential hardware issue.

* In order to achieve high fidelity, the codec needs to be the I2S master. However, this causes clocking problems with the Teensy. Because of this, the Teensy's built in ADC functionality (which would ordinarily be used for reading potentiometers etc.) cannot be used whilst the codec is also installed. We're attempting to circumvent this for the prototype by including some 8bit SPI ADC chips to provide the user interface.

* The codec is communicating in 24 bit audio, however the Teensy audio library components that we are using in our prototype truncate this to 16bit. This is fine for the prototype, 16bit audio is adequate, but we would replace this with 24bit processing as we move away from the Teensy in some future iteration. This is an important feature for very high gain applications, such as guitar effects, where the increase in noise floor from going to 16bit is a drawback.

* The CS4272 codec is supported by the Teensy audio library, but not "out of the box". There are some issues that had to be solved in software in order to bring the CS4272 online. The first is the DAC and ADC control registers on the CS4272 are configured incorrectly in the master branch of the audio library - these need correcting in order to work with our codec board. Because of this, we will provide our own software environment rather than using the audio library in full. We also neglected to spot the developer note that, in order to work with the audio library, the codec reset pin needs to be on pin 2. We arbitrarily connected the reset pin to pin 22 of the Teensy. Rather than change this in hardware, we simply change the software define in the audio library.

* The codec board has yet to be tested with any other MCU, though we assume if it works with the Teensy there's no reason it can't work elsewhere.

* Bad drill size on the codec header on the mainboard needs addressing.

* Some clipping occuring - we're unsure if this is on the way in or the way out. The signal conditioning and overload protection could use a review.

# Manufacture

Due to the heavy use of surface mount components, we would support the pedal building community by aiming to provide pre-soldered modules that are ready to be dropped into a pedal design. A user can design a simple UI board - for mounting potentiometers, switches and other UI elements - and then plug that into a pre-built digital mainboard that provides the necessary interaction with the digital components.

At stage 1 we would provide a barebones software environment and some simple tutorials on how to achieve basic digital audio processing in software [WIP].

# Monetization

The project is intended to be primarily open source. All hardware developed will be under MIT license (a very permissive license) that allows for commercial use. 

Software, when we get to that stage, will likely be under a "free for non commercial use" type license, with commercial users paying a license fee to use digital models created in our software in their hardware products.

We would also sell pre-built modules and development kits.
