# chess-recorder

## Goal

Record moves made on a physical chessboard in a digital format.

## Similar projects

The Raspberry Pi is a popular [SBC](https://en.wikipedia.org/wiki/Single-board_computer) and chess is a popular pastime so it's no surprise there's an abundance of projects combining the two. These projects often focus either on :

- robotics:
  - [robot arm](https://www.instructables.com/id/Chess-Robot-Raspberry-Pi-Lynxmotion-AL5D-Arm/)
  - [stepper motors](https://magpi.raspberrypi.org/articles/wizard-chess)
- piece detection

The robotics part seems more complex and not really that interesting. I don't mind moving the pieces myself at all, it's one of the main attractions of playing with a physical board after all. So I'll focus on piece detection and recording the moves.

## Board

The main idea is to put a sensor underneath each square. Multiple sensors are possible but the most economical and efficient one is the reed switch. [Reed switches](https://www.first4magnets.com/blog/what-is-a-reed-switch-and-which-magnets-operate-them/) are switches opening and closing by magnetism.

For example [this project]((https://circuitdigest.com/project/raspberry-pi-chess-board)) is a good example of using switches on each square. An issue immediately which immediately arises is how to connect all these sensors. There are 64 squares on a chessboard and the Raspberry Pi itself hasn't as many GPIO pins. A possible solution is to use I/O expanders.

### I/O expander

[This](http://chess.fortherapy.co.uk/home/a-wooden-chess-computer/design-ideas-for-easy-to-build-beaglebone-black-chess-computer/) is an example project. The library I used before to connect to peripherals is `gpiozero`. However, this library doesn't seem to mainly support SPI over I2C. There's a `gpiozero` PR open to add a [gpiozero implementation for the  MCP23017](https://github.com/gpiozero/gpiozero/issues/199) but it hasn't been merged yet.
Most example projects use [smbus](https://raspberrypi.stackexchange.com/questions/79070/reading-and-writing-with-smbus-package) for I2C communication.

I do have positive experience using a [MCP3008 ADC](https://www.adafruit.com/product/856). For this project I'd like to stick to using Microchip products (`MCP...`) with the only difference I use I2C instead of SPI as protocol. 

The [MCP23017](https://www.microchip.com/wwwproducts/en/MCP23017) seems to be the most popular Microchip I/O expander and is cheap (about 1 euro a piece). 

SPI involves adding additional wires for each peripheral added. With all these sensors per square that would be too much. These are nice explanations of what I2C is:
- [written](https://learn.sparkfun.com/tutorials/i2c)
- [youtube](https://www.youtube.com/watch?v=jFtr0Ha5f-c)


TO DO:
- cleanup
- write TLDR reddit to verify basic assumptions
- clear outline





[How to use multiplexing](https://www.raspberrypi.org/forums/viewtopic.php?t=69430).

> Wire one lead of each sensor in a column together, and the other lead to the row, so that you can apply a voltage to a column, and then read whether you have continuity in any given row. This way for any NxN board, you only need 2n GPIO pins, not N^2 pins.

> Conveniently, a chess board is 8x8, which means that using this method, you need 16 GPIO pins, one less than the Pi's standard header has.

https://www.raspberrypi-spy.co.uk/2013/07/how-to-use-a-mcp23017-i2c-port-expander-with-the-raspberry-pi-part-1/#prettyPhoto

This tutorial shows how to add 16 pins but you might need more. 

Or not: analog vs digital multiplexing (above seems digital)

### camera

Check if square is no longer black or white.