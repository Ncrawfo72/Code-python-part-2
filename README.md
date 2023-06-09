# Codepython part 2
The mastery of code python in a series of three different projects
This repo is a template VS code project for CircuitPython projects that automatically uploads your code to the board when you press F5. Requires F5Anything extension.

### Description
Use a tempature sensor to take in the nearby tempature in Farhenhiet and display in onto and LCD with it saying wether it too cold or hot with the bas being 74 and allowing the tempature to be withinn a .5 range od degrees manually. 


### refelction
This assignment was rather straight forward, but I still struggled with getting the tempature sensor to accurately read the tempature on to the LCD. I do have to thank Chatgpt for the code as he was a heavy part in helping me with my code. With help from kaz I was able to get my LCD to accruately display my readings from the tempature sesnor.

### Wiring

![tmp36_circuit](https://user-images.githubusercontent.com/112981453/228340407-b3ad295e-59c6-4e54-b15b-df0bc159f9ae.png)



### Proof
![name](https://github.com/Ncrawfo72/Code-python-part-2/blob/master/this%20one%202.gif)

### Code
```
import board   #[Lines 1-8] Importing all Neccessary libraries to communicate with LCD
import time
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
from digitalio import DigitalInOut, Direction, Pull 
import board
import analogio


# get and i2c object
i2c = board.I2C()
tmp36 = analogio.AnalogIn(board.A0)
# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)
def tmp36_temperature_C(analogin):              #Convert millivolts to temperature
    millivolts = analogin.value * (analogin.reference_voltage * 1000 / 65535)
    return (millivolts - 500) / 10


while True:
    # Read the temperature in Celsius.
    temp_C = tmp36_temperature_C(tmp36)  
    # Convert to Fahrenheit.
    temp_F = (temp_C * 9/5) + 32
    # Print out the value and delay a second before looping again.
    lcd.set_cursor_pos(0, 0)           #[Lines 26-36] Print different messages based on the temperature
    if temp_F > 75:
        lcd.print("it's too hot!")
    elif temp_F < 70:
        lcd.print("it's too cold")
    else:
        lcd.print("It's just right")
    lcd.set_cursor_pos(1, 0)
    lcd.print("Temp: {}F".format(temp_F))
    time.sleep(.5) 
```




### Description
For this assignment we had to take a Rotary endcoder and make it so when it was turned it would change the display on an LCD screen from "go" to "stop" depending on which way you spun the Ratary encoder and if you pressed down on it, it would change which led was lit up between a red, a yellow, and a green light.

### Reflection
This assignment was the hardest of the three assignments and probably the trickesiest assignment I have had in Engineering in a while. I got a lot of help from Kaz and his code along with his assitance allowed me to actually finish the project. I was ble to get the wioring done on my own, but the code was very complicated and far beyond what I was gonna be able to do so again I have to thank Kaz for his help and code. Another thing worth noting is the picture of the wiring is for if the light on the Aurdino was to be used which I did not.



### Wiring
![trafficlight_circuit](https://user-images.githubusercontent.com/112981453/228340528-ce69e90e-a2c9-4738-b83b-3f310dc698a7.png)



### Proof
![name](https://github.com/Ncrawfo72/Code-python-part-2/blob/master/this%20one.gif)


### Code
```
# Rotary Encodare light thingksf;ja
import time
import rotaryio
import board
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface
from digitalio import DigitalInOut, Direction, Pull


encoder = rotaryio.IncrementalEncoder(board.D3, board.D2)
last_position = 0
btn = DigitalInOut(board.D1)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
state = 0
Buttonyep = 1


i2c = board.I2C()
lcd = LCD(I2CPCF8574Interface(i2c, 0x3f), num_rows=2, num_cols=16)


ledGreen = DigitalInOut(board.D8)
ledYellow = DigitalInOut(board.D9)
ledRed = DigitalInOut(board.D10)
ledGreen.direction = Direction.OUTPUT
ledYellow.direction = Direction.OUTPUT
ledRed.direction = Direction.OUTPUT


while True:
    position = encoder.position
    if position != last_position:
        if position > last_position:
            state = state + 1
        elif position < last_position:
            state = state - 1
        if state > 2:
            state = 2
        if state < 0:
            state = 0
        print(state)
        if state == 0: 
            lcd.set_cursor_pos(0, 0)
            lcd.print("GOOOOO")
        elif state == 1:
            lcd.set_cursor_pos(0, 0)
            lcd.print("yellow")
        elif state == 2:
            lcd.set_cursor_pos(0, 0)
            lcd.print("STOPPP")
    if btn.value == 0 and Buttonyep == 1:
        print("buttion")
        if state == 0: 
                ledGreen.value = True
                ledRed.value = False
                ledYellow.value = False
        elif state == 1:
                ledYellow.value = True
                ledRed.value = False
                ledGreen.value = False
        elif state == 2:
                ledRed.value = True
                ledGreen.value = False
                ledYellow.value = False
        Buttonyep = 0
    if btn.value == 1:
        time.sleep(.1)
        Buttonyep = 1
    last_position = position
```






### Description
One of the easiest assignments ever all you had to do was plug in a photointerupter into an arudino and run some pretty simple code. All the photointerrupter had to was do its simple basic job and detect when it was obstructed or not.


### Reflection
This assignment quite litterally probably took me a total of ten minutes to complete. I thought it was pretty easy to do all thopugh I did take the code from River and used it as my code. As for the wiring I just had to search up what wire did what on a Photointerupter and was able to get it done with no real problem. This was a really easy project and a good enjd to these three assignments.


### Wiring
![photointcircuit](https://user-images.githubusercontent.com/112981453/228340575-71563045-9ad7-4722-bcbf-26fe0f55e0b5.png)



### Proof
![name](https://github.com/Ncrawfo72/Code-python-part-2/blob/master/New%20video.mp4)

### Code 
```
#thing with photointerrupter
import time
import digitalio
import board

photoI = digitalio.DigitalInOut(board.D7)
photoI.direction = digitalio.Direction.INPUT
photoI.pull = digitalio.Pull.UP

last_photoI = True
last_update = -4

photoICrosses = 0

while True:
    if time.monotonic()-last_update > 4:
        print(f"The number of crosses is {photoICrosses}")
        last_update = time.monotonic()
    
    if last_photoI != photoI.value and not photoI.value:
        photoICrosses += 1
    last_photoI = photoI.value
```

doodoofart.com - chelsea
