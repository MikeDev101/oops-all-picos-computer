![oops-all-picos](https://user-images.githubusercontent.com/12957745/160698351-63428087-1c0b-4902-a6b5-21127f7be20e.png)

# Oops! All Raspberry Pi Picos!

![shitty_computer](https://user-images.githubusercontent.com/12957745/160719292-b28d94f1-2e64-4b92-b13e-9e1c8c8e18dd.png)

This is a **CONCEPT** computer that uses Raspberry Pi Picos, Glue Logic, Parallel Static RAM, and Parallel Flash/EEPROM. The design comes with a custom virtual CPU for running code, an I/O controller coprocessor for managing sound/DMA/keyboard input, and a graphics chip for generating VGA tile-and-sprite graphics.

## Main CPU
This Pi Pico runs a custom vCPU with it's own isolated vCPU RAM. The vCPU talks to the MIOC Coprocessor over UART Serial to send/handle requests. The vCPU can also perform DMA transfers with external RAM/ROM,the MIOC's PSG, and the VGU.

## MIOC (Multipurpose I/O Controller)
This Pi Pico runs a PS/2 driver to handle PS/2 Keyboard events.

The MIOC's built-in PSG (Programmable Sound Generator) utilizes the Pico's PWM pins to generate arbitrary waveforms, and can load BGM/SFX data from vCPU RAM or external RAM/ROM using the MIOC's DMA functionality.

Another function of the MIOC is to serve as a DMA controller. The vCPU, PSG, and VGU all need to talk to the MIOC before performing a DMA transfer over the Parallel Bus.

DMA transfer priority
1 (highest) VGU
2 (middle) vCPU
3 (lowest) PSG

## VGU (Video Generator Unit)
This Pi Pico generates a 640x480@60Hz VGA signal and performs tile-and-sprite-based graphics. Utilizing the MIOC as a bridge between the vCPU and External RAM/ROM, the VGU can perform DMA transfers to quickly load bitmap data into Video RAM.
