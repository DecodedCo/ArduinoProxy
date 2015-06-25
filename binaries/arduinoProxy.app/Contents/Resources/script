#!/usr/bin/python
'''
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
'''

import re
import sys
import glob
import serial
import urllib2
import signal

# Find all serial ports
def serial_ports():

    if sys.platform.startswith('win'):
        ports = ['COM' + str(i + 1) for i in range(256)]

    elif sys.platform.startswith('linux') or sys.platform.startswith('cygwin'):
        # this is to exclude your current terminal "/dev/tty"
        ports = glob.glob('/dev/tty[A-Za-z]*')
    elif sys.platform.startswith('darwin'):
        ports = glob.glob('/dev/tty.*')
    else:
        raise EnvironmentError('Unsupported platform')
    #store the results in a list
    result = []
    for port in ports:
        try: #catch errors
            s = serial.Serial(port)
            s.close()
            result.append(port)
        except (OSError, serial.SerialException):
            pass
    return result

# Read serial data and process for URLs
def readSerialData(port):
    ser = serial.Serial(port, baudRate)
    print "Connected! Listening to serial output..."
    while True:
        input = ser.readline().strip()
        #find urls in returned data
        url = re.search('http[s]?://(?:[a-zA-Z]|[0-9]|[$-_@.&+]|[!*\(\),]|(?:%[0-9a-fA-F][0-9a-fA-F]))+', input)
        if url is not None:
            url = url.group(0)
            result = urllib2.urlopen(url).read()
            print "Requested " + url
        else:
            print input

#this is so that a CTRL C doesnt output a horrible error...
def exit_gracefully(signum, frame):
    # restore the original signal handler as otherwise evil things will happen
    # in raw_input when CTRL+C is pressed, and our signal handler is not re-entrant
    signal.signal(signal.SIGINT, original_sigint)
    print "Goodbye!"
    sys.exit(1)
    # restore the exit gracefully handler here
    signal.signal(signal.SIGINT, exit_gracefully)

if __name__ == '__main__':
    global baudRate
    if len(sys.argv) < 2:
        print "Baud rate set to 9600 by default"
        baudRate = 9600
    else:
        baudRate = sys.argv[1]
        print "Baud rate set to: " + str(baudRate)
    # store the original SIGINT handler
    original_sigint = signal.getsignal(signal.SIGINT)
    signal.signal(signal.SIGINT, exit_gracefully)

    print "Searching for an Arduino..."
    ports = serial_ports()
    for i in ports:
        if "tty.usbmodem" in i:
            print "Connecting to " + i
            readSerialData(i)
            break
    print "No Arduino available"
