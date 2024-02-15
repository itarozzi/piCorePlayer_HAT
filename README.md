# piCorePlayer RaspberryPi 3 HAT

A Raspberry Pi 3 HAT for my piCorePlayer device.


By Ivan Tarozzi
- itarozzi@gmail.com
- ivan.tarozzi@fablabromagna.org



## Description
When I decided to use a RaspberryPi and the [piCorePlayer project](https://www.picoreplayer.org/) to stream my music library from NAS to HiFi I encountered the problem to have a proper PSU for the RaspberryPi, having the ability to shutdown the system in safe mode and using a HW button for a real ON/OFF.

This becasuse the RaspberryPi 3 lacks of a native way to halt and restart by buttons.

I found a solution in a forum (see credits) and I decided to create a prototype using handwired assembly.

But in the same days FablabRomagna (the FabLab where I play as maker here in Rimini-IT) organized a course about KiCad so I decided to give it a try. And here the result of my attempt to use KiCad for creating my HAT.

Instead of use dupont wires on the RaspberryPi 40-pin connector I created a HAT to connect the power module and at the same time use it for the DAC, the TFT display, the encoder and buttons for the front panel (piCorePlayer commands).

I used a lot of components I already owned, so I adapted the circuit to that things.

I creatd the PCB and using STEP file generated  by KiCad as reference, I created a case with FreeCAD.



## How it works

The power section use a latched relay to power the RaspberryPi throught the 40-pin GPIO connector, when a button is pressed. I changed the original circuit to use the same button to also poweroff the device.

So, in its initial state, the START/STOP button (replicated in PCB and front panel) drive the transistor Q1 to switch-on the relay and power on the RaspberryPi.

When the RaspberryPi is on and the START/STOP button  is pressed again, a high level is sent to GPIO_16 pin. A little daemon intercept this event and execute the safe power-off using linux commands.

When the RaspberryPi is halted, the GPIO_26 rise (using poweroff overlay) and this reset the relay via the Q2 transistor.
And additional safe button is used to activate Q2 if the software poweroff fails (hope ever used).

> there is a hypothetical race condition when the START/STOP button is pressed the first time, because when the relay switch its state, it rise the level on GPIO_16. But in real case, the Raspberry is booting so the shutdown daemon is not running until 10/12 seconds. I think it will works in this first version



## TODO

- may be a smd version
- DAC integration in the HAT


## License

- Hardware design is released under CERN-OHL-S v2 license
- Software is released under GPL3 License
- Documentation is released under CC BY-SA 3.0


## Credits

- [piCorePlayer project](https://www.picoreplayer.org/)
- [Seamus](https://raspberrypi.stackexchange.com/questions/142837/questions-on-dtoverlay-gpio-poweroff) for the power circuit
- [Maurizio Conti @ FablabRomagna](https://github.com/mconti) for forced me to try KiCad :)
