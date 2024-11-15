# LED-Serial

Sending data between Arduinos with an LED and photoresistor

## Introduction

Arduinos and other microcontrollers often use Serial communication to transmit and receive data. Serial sends data in the form of individual bits, one after the other at a fixed rate. I wanted to replicate that system in a wireless way. This project uses an LED to transmit data and a photoresistor to receive it.

For an excellent explanation of Serial communication, check out [Nick Gammon's post](https://arduino.stackexchange.com/questions/19756/how-does-serial-communications-work-on-the-arduino).

## Supplies

1. 2 Arduinos and their respective USB cables
2. LED
3. 220 ohm resistor
4. Photoresistor (I used the [KY-018 Module](https://arduinomodules.info/ky-018-photoresistor-module/))
5. Breadboard and jumper wires

## Instructions

1. Wire the photoresistor module to one Arduino, with the signal pin going to Analog Pin 0.
2. Place the LED directly beside the photoresistor on the breadboard. Connect the negative side to the other Arduino's ground and the positive side to digital pin 8 through the resistor.

   <img src="WiringDiagram.png" width=50%>

3. Connect both Arduinos to your computer.
4. Run `CalibratePhotoresistor.ino` on the Receiver Arduino (the one with the photoresistor) and follow the instructions in that file.
5. Upload `Transmit.ino` to the Transmitter Arduino and `Receive.ino` to the Receiver Arduino
6. Open the Serial monitor for the Receiver and watch the message be printed.

## Results

Ambient light level in my room was ~350 and I started with a threshold of 20, so any reading below 350 - 20 = 330 would be considered a 1. This allowed reliable data transmission up to baud 60, or a bit period of 16ms. I found that increasing the threshold allowed for much higher speeds. With an threshold of 150, I was able to get 100% reliable transmission at baud 333, or one bit every 3 ms.

I did not expect this performance from a cheap LED. I believe that the speed is mostly limited by the responsiveness of the LED and photoresistor. Perhaps some additional circuitry could improve their performance.

## Going Further

If the Transmitter and Receiver Arduinos are connected to 2 different computers, then the Transmit.ino code could be modified to take Serial input for the message, rather than repeating the same message over and over again. This not possible with just one computer, as the Arduino IDE only allows one Serial monitor to be open at a time (you can either have the transmitter or receiver monitor open, but not both).

## Troubleshooting

- Ensure that you are correctly switching between COM ports when uploading code and viewing the Serial monitor. Since this project uses two Arduinos, you will be switching frequently.
- Recalibrate the backgroundLightLevel and threshold in Receive.ino.
- Move the LED very close to the photoresistor. I had a separation of ~3 mm. This is NOT a long range communication method.
- Decrease the baud to something slow like 5. Watch the Transmitter and ensure that the LED is blinking in a clear pattern.
- Make sure that the baud is the same on both the Transmitter and Receiver Arduinos, otherwise the signal will not be interpreted correctly.
