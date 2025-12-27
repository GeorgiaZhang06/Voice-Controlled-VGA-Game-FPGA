# Voice-Controlled "Flappy" Game (FPGA)

A hardware-based side-scrolling game implemented in **Verilog** on the **Terasic DE1-SoC (Cyclone V)** platform. This project utilizes real-time audio processing for character control and VGA output for display.

*Note: In compliance with university academic integrity policies, the source code for this project is not public.*

## 1. Project Overview
This project replaces traditional button inputs with **voice-amplitude control**. A player "jumps" by making noise into a microphone, which is processed via the FPGA's onboard Audio Codec.

### Key Features:
* **Real-time Audio Processing:** Digital signal processing of microphone input to determine jump strength.
* **VGA Graphics:** 640x480 resolution graphics driven by a custom-built VGA controller and arbiter.
* **Hardware Collision Detection:** Pixel-accurate collision logic between the player sprite and dynamic pillar obstacles.
* **Dynamic Difficulty:** Obstacle gaps change position based on the player's score.

## 2. System Architecture
The system is composed of several high-level modules:

1.  **Audio Subsystem (`volume.v`):** Interfaces with the Wolfson WM8731 codec via I2C to capture 16-bit audio samples. It calculates the signal magnitude to trigger the "jump" logic.
2.  **VGA Arbiter (`vga_demo.v`):** A finite state machine (FSM) that manages bus access between the player, pillars, and background reset modules to prevent screen tearing.
3.  **Physics Module (`player.v`):** Implements gravity and velocity-based movement.
4.  **Collision Module (`collision.v`):** Purely combinational logic that monitors coordinate overlaps to trigger a game-over state.


## 3. Hardware Requirements
* **FPGA:** Terasic DE1-SoC
* **Display:** VGA Monitor (640x480 @ 60Hz)
* **Audio:** 3.5mm Microphone (Line-in)

## 4. Implementation Details
The game uses a complex FSM structure to handle the drawing of objects. To ensure smooth performance, the background is only redrawn upon a reset/collision, while moving objects (the bird and pillars) use an "erase-and-redraw" cycle within the VGA arbiter.

### Score System
The score is tracked using a binary-to-BCD converter and displayed in real-time on the **HEX0-HEX5** 7-segment displays.

## Project Documentation
* [Technical Presentation (PDF)](./Project_Technical_Presentation.pdf) - A deep dive into the FSM logic, game logic, and the process of creating this project
