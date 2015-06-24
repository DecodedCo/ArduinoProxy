# Arduino Proxy

*Prototyping IoT without direct internet connectivity*

## Description

Have you ever needed to test an Internet of Things prototype while programming your Arduino over USB, and you don't have a WiFi or Ethernet shield setup?

This simple python script will listen to the serial output from your Arduino, and perform a GET request on any URLs output. It will pipe any non-URL serial output to the terminal.

## Installation

* You will need the `pySerial` library installed e.g. `pip install pySerial`

## Usage

### In your Arduino code

```
void setup() {
	Serial.begin(9600);
}

void loop() {
	if () { // Logic to trigger an API call
		Serial.println("http://api.to.hit/"); // URL of API
		Serial.println("Debugging message");
	} else {
		Serial.println("Debugging message");
	}
}
```

### When you're ready to test

```
python arduinoProxy.py [9600]
```

* Optional argument for baud rate defined in your Arduino code (default: 9600)
* To quit do a CTRL-C and it will gracefully exit

The python script will parse all serial input, and perform a GET request on any URLs detected.

## Binaries

In the `binaries/` folder are ready-to-use binaries for the classroom, that can be launched from a GUI.

## License

The MIT License (MIT)

Copyright (c) 2015 Decoded

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN
THE SOFTWARE.
