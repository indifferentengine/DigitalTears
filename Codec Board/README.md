# Codec Board

This is an implementation of the reference design for the Cirrus Logic CS4272 codec chip.

We have implemented a mono input (differential input A) and a stereo output (both differential output A and B).

The board is configured for:

* Control port mode. Registers are set by the MCU over I2C.
* I2S Master. Clock signal is provided by the external 24.576MHz crystal, as per the CS4272 datasheet. This provides for best audio performance.
* 48Khz sampling rate, set by the external clock.

Signal conditioning for ADC and DAC is provided, as per the CS4272 reference design. Further conditioning is required offboard depending on what your input signal it, and what your sending your output signal to (for example, to condition the inputs for guitar signals vs. line level signals).

Pin diagram [TODO]
