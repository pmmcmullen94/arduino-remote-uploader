# arduino-remote-uploader

The goal of this project is provide over-the-air (OTA) upload of Arduino sketches to a remote Arduino. There is support for XBee (Series 1 and 2), Wifi (ESP8266), and Nordic nRF24L01+. This is accomplished by programming one Arduino with another. The circuit is conceptually very simple:

<img src="https://raw.githubusercontent.com/andrewrapp/arduino-remote-uploader/master/resources/prototype-wiring-xbee_bb.png" alt="xbee prototype" border="0" />

One Arduino receives firmware packets from a wireless device (XBee in this case) and writes to EEPROM. Then, once it receives the entire sketch it flashes the firmware on the other Arduino. There are other tricks for remote firmware programming but nearly all involve unfortunate tradeoffs. This solution is fault tolerant, in that it checksums every firmware page (packet), acks and retries dropped packets and only flashes when a complete firmware image has been written to EEPROM. You can unplug the remote during programming, then plug back in and it will resume and flash successfully.

It's also quite cost-effective: the Microchip EEPROM costs $1. Arduino Pros can be found online for about $3 each. Nordic radios can be found for about $1 online.

Here's a prototype I built to control my garage door. A Modern Device RBBB performs the programming while the app runs on a Arduino Pro. I'm using an Arduino XBee shield just for the XBee socket and 3.3V conversion. 

<img src="https://github.com/andrewrapp/arduino-remote-uploader/blob/master/resources/garage-door-prototype.jpeg?raw=true"/>

The client is a simple command-line app:

<img src="https://github.com/andrewrapp/arduino-remote-uploader/blob/master/resources/remote-upload-client.png?raw=true"/>

Here's a <a href="https://youtu.be/Vqg-zZniQU0" target="_blank">screenshare</a> of an upload in action.

Version [0.1](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22arduino-remote-uploader%22) is in Maven Central

<!--<video src="https://github.com/andrewrapp/arduino-remote-uploader/blob/master/resources/remote-flash-screen-recording.mov?raw=true" controls autoplay></video>-->

I've written about the project in detail on Medium https://medium.com/@nowir3s/arduino-remote-uploader-aa61e4d620b1

Demo

<a href="https://vine.co/v/O0jLBw7aO1m" target="_blank"><img src="https://raw.githubusercontent.com/andrewrapp/arduino-remote-uploader/master/resources/vine-xbee.png" alt="vine" width="300" height="299" border="0" /></a>

The remote Arduino starts with a sketch that blinks fast. Then, I change the sketch to blink slow, compile and flash the sketch on the remote Arduino, via XBee. The command completes and indicates that the flash was successful. And lastly we see the Arduino is running the new sketch and blinking slowly. So there it is: over-the-air Arduino programming via XBee in six seconds!

<a href="https://vine.co/v/empxvZgpuqV" target="_blank"><img src="https://raw.githubusercontent.com/andrewrapp/arduino-remote-uploader/master/resources/vine-wifi.png" alt="vine" width="300" height="299" border="0" /></a>

Here's the same thing but over wifi with a esp8266 and 3.3V Arduino Pros at 8Mhz.
