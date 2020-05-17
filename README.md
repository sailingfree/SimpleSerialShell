# SimpleSerialShell
An extensible command shell for Arduino.

This library provides a basic "command line interface" for your Arduino through its serial interface.  
This can connect to the Serial Monitor, for example.

Commands are of the old (1970s) Unix-style
" **int aCommand(int argc, char ** argv)** " described in Kernighan and Ritchie.

### Example:
Include the library:
```cpp
#include <SimpleSerialShell.h>
'''

Create the command to call:
```cpp
int helloWorld(int argc, char **argv) {...};

'''

Start the serial connection and attach it to the shell:

```cpp
setup()
{
  Serial.begin(9600);

  shell.attach(Serial);  // attach to any stream...
  shell.addCommand(F("sayHello"),helloWorld);
}
 ```

Check for input periodically:
```cpp
loop()
{
  shell.executeIfInput();
}
 ```

### Example Sketches
* AdjustableBlink -- lets you read and vary the blink rate of a Blink sketch.
* ArduinoTextInterface
lets you read and write digital and analog values to Arduino pins.  Basically wrappers for analogRead(), analogWrite(), tone(), noTone() etc.
It's enough to set and clear bits one at a time and see if you have an LED connected to the right pin.
* EchoCommand -- "echo" example.  
Sending "echo Hello World!" gives "Hello World!" on the serial monitor.
* IdentifyTheSketch -- Example provides an "id?" query which reports the filename and build date of the sketch running.  
Useful if you forgot what was loaded on this board.

### Tips

* To save limited RAM, make sure you use the F() macro to keep strings in flash
rather than copy them to RAM.  ("shell.addCommand(F("helloWorld"), hello);

* Since the shell delegates the actual communication to what it attaches to
(with shell.attach()), it can work with Serial, Serial2, SoftwareSerial or
a custom stream.

* To make it easy to switch the shell to a different connection, I recommend
sending command output to the shell
(rather than straight to Serial for example).

### Notes

It's just a simple shell... enough that the Serial Monitor can send text
commands and receive text responses.  OK for humans with keyboards.

The intent is for the Serial Monitor output to be _human-readable_ rather than _fast_.  
If you want fast serial communication between an Arduino and a host, 
 you may want to look into Processing/Wiring or MIDI protocols
(which I don't really know about).

Each added command uses up a small amount of limited RAM.  This may run out
if you need MANY commands.  (Less of a problem for newer processors.)


(I haven't tested this but) you should be able to switch among multiple
streams on the fly.  Not sure if that's useful with limited code space.

