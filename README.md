# tic-tac-toe webgame
## For both PC and Pico W
The contents of this folder enable a simple web page offering the game, tic-tac-toe. The page is served by a small server called [microdot](), which was written for [MicroPython](), however may also be used with Python. It is intended to serve as a simple example as to how to develop a user-facing webpage running on the Pico.

## Additional Files Required
### secrets.py
This file contains the SSID and password of your desired wireless LAN connection:
```python
ssid = 'Demo'
password = 'mpc1234!'
```
A simple text file called *secrets.py* with the above format and the correct SSID and password is required. It sits at the root folder along with the other files such as wlan.py.
### config.py
This file exists to identify the board, if you don't need to do so you may comment out the lines related to *config.py* as in lines 6 and 72 in *wlan.py*. Or you may create one, which looks like this:
```python
name = 'Pico W A'
```
Provide a name which matters to you. At present the *title* of the webpage isn't using this variable.
## Installation
1. Copy the folder to your PC
2. If running on the PC:
```bash
# in your terminal
python3 tictactoe.py
# browse to 0.0.0.0:5001
# play the game!
Ctrl-C in the terminal
```
If you want to run on a Pico W:
```bash
# copy all files to Pico W except tictactoe.py
mpremote fs cp filename :filename
# copy tictactoe to main.py
mpremote fs cp tictactoe.py :main.py
# ensure secrets.py has the desired SSID and password for your WiFi
# start a serial program (see Note below)
# press Reset or cycle power on your Pico W
# Use the IP address provided via the serial port
# Browse to IP address:5001 to play game (not 0.0.0.0 as stated)
```

## Resolving Connection Errors with Pico W
Sometimes it is difficult to connect to the Pico W, here is some background and hints how to resolve issues.

When the Pico W resets, it will attempt to connect to the SSID using the information in *secrets.py*. The built-in LED will blink slowly when this is in progress. If a wireless connection is made, the LED will turn off and the program will print the following information:
```
Name: Pico W B
IP Address: 10.0.1.12
MAC Address: 28:cd:c1:08:a9:7d
Channel: 1
SSID: Demo
TX Power: 31
Starting sync server on 0.0.0.0:5001...
```
It is also critical the device (phone or PC) which are using to connect with the Pico is on the same network as well. **Be sure you are connected to the same SSID.**

The address you enter in your browser is a combination of the addresses supplied above. Use the **IP Address** combined with the *port number* following the *0.0.0.0* as in *:5001*. Using the above data, you would need to go to this address:
```
http://10.0.1.12:5001
```

If the wireless connection can not be made, the LED will blink at a faster rate and a error will be printed via the serial port as in:
```
Connection failed: LINK_BADAUTH
Starting sync server on 0.0.0.0:5001...
```
In this case, the password was in-correct, resulting in a *BADAUTH* or *bad authorization* error. The following line *Starting sync...* is irelevant as there isn't an IP address to connect.

## Serial Programs
I develop on a Mac and use a paid program called [Serial](https://www.decisivetactics.com/products/serial/). It is quite robust and is able to connect to everything I've attempted. That said, you might not want to pay for Serial (or have a Mac).

My second favorite "connect to everything" serial program is the serial monitor in the Arduino [Legacy IDE (1.8.X)](https://www.arduino.cc/en/software). I've found it is easy to configure AND it seems robust enough to connect to everything I'v attempted as well.

I've found several programs on the Mac which won't work with a Pico (*when it is re-booted...*):
* *cu* - a common, simple program which crashes when the Pico is rebooted
* *screen* - a ubiquitous, powerful screen program which doesn't seem to connect to the Pico after re-boot
* *minicom* - crashes when the Pico is rebooted
* *Thonny IDE* - loses serial connection when the Pico is rebooted

## Resetting the Pico
The *Pico W* doesn't have a reset button, which means there are two alternatives. First, power cycle the Pico by removing the USB cable or second, add a reset button. I find the second method preferable and have described the process [here](https://wellys.com/posts/rp2040_micropython_1/#reset).

## Program Size
This example use [bulma](https://bulma.io) as its framework. I chose it as I wanted a CSS-only framework and *bulma* was the first one, that I found, which made sense. That said, it is huge for an embedded microcontroller at 206kB. That is almost 25% of the entire file storage capacity of the Pico W. Going forward, I won't use it. I also won't invest any time changing it for this project.

## Automation to Copy Project to Board
The program list_walk will copy all required program files to the board.

## Tool to Erase Pico LittleFS filesystem