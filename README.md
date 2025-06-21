# Twigstrument
Code for an adaptation of Michael Ridge's Earth Block Instrument for use (initially) in Bastard Assignments' piece PIGSPIGSPIGS. I call it the Twigstrument because it's a bowed twig instrument (as with Ridge's original concept, it could be used with other long thin things, but twigs are the plan, at least for now).

This project was begun on 2024-08-04 and has Michael Ridge's permission to adapt his concept.

It is being coded in PureData and the aim is that the Bela Mini hidden inside the box of the instrument will act like a guitar pedal, adding effects to the sounds created by bowing sticks inserted into an earth block attached on top of the box, and picked up by a contact microphone. I have added (August 2024) a Bela Trill Bar touch controller to one side of the instrument to allow for volume control and muting from the instrument - this may eventually be adapted to control effect parameters. In March 2025, I added a small circuit to incorporate 2 LED indicators which give visual feedback of when the instrument is or is not muted and when the reset section of the touch control is triggered.

The Twigstrument's development is supported by PIGSPIGSPIGS' commissioners:
- Borealis - a festival for experimental music (NO)
- SPOR Festival (DK)
- Wigmore Hall (UK)

with additional support from Cyborg Soloists, a UKRI Future Leaders Fellowship based at Royal Holloway, University of London, led by Dr Zubin Kanga (the Bela Mini is on loan from this project) and from Arts Council England.

# Technical Overview 
(summary produced by Claude.ai based on the most recent version of the main patch)

`_main.pd` is the main controller for the Twigstrument. Here's what happens in the patch:

## Audio Signal Flow
- Takes input from `adc~` (the contact microphone picking up bowed twig sounds)
- Amplifies the signal by a factor of 7 using `*~ 7`
- Adds the amplified signal to itself with `+~` (creating a doubling effect)
- Passes through a `reverb_twig` effect module
- Controls output volume and muting via `*~` before sending to `dac~` (audio output)

## Touch Control Integration
- The `readtouchbar` subpatch processes input from the Bela Trill Bar touch controller
- This provides three control zones on the touch bar:
  - **Mute zone** (0-0.2): toggles audio muting with each touch
  - **Volume zone** (0.2-0.75): provides continuous volume control
  - **Reset zone** (0.75-1.0): resets the system and unmutes audio

## LED Feedback System
- Controls two LEDs via Bela digital outputs (pins 12 and 17)
- Pin 17 LED indicates mute status (off when muted, on when audible)
- Pin 12 LED briefly flashes when the reset function is triggered

## Initialization
- Sets up the Trill Bar sensor and digital output pins on startup via `loadbang`
- Audio starts muted by default and can be unmuted via the reset touch zone

The patch essentially creates a smart effects pedal that responds to touch control while processing the acoustic input from your bowed twig instrument.

## Summary
The main Pure Data patch (`_main.pd`) implements the core functionality of the Twigstrument:

- **Audio Processing**: Amplifies and processes the contact microphone signal, applying reverb effects before output
- **Touch Control**: Integrates with the Bela Trill Bar to provide three touch zones for mute/unmute, volume control, and system reset
- **Visual Feedback**: Controls LED indicators to show mute status and reset activation
- **Hardware Integration**: Manages Bela Mini digital I/O for sensor reading and LED control

The instrument starts muted by default and can be controlled entirely from the touch bar, allowing for hands-free operation while playing.
