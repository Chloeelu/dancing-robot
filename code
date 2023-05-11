# SPDX-FileCopyrightText: 2021 ladyada for Adafruit Industries
# SPDX-License-Identifier: MIT

import time
import board
import digitalio
import pwmio
import busio
import terminalio
import displayio
import adafruit_imageload

from adafruit_motor import servo
from adafruit_display_text import label
from adafruit_st7789 import ST7789

# First set some parameters used for shapes and text
BORDER = 60
FONTSCALE = 2
BACKGROUND_COLOR = 0x000000 # Bright Green
FOREGROUND_COLOR = 0x000000 # Purple
TEXT_COLOR = 0xFFFFFF

#buzzer
buzzer = pwmio.PWMOut(board.GP6, duty_cycle=0, frequency=440, variable_frequency=True)

# Release any resources currently in use for the displays
displayio.release_displays()

tft_cs = board.GP9
tft_dc = board.GP8
spi_mosi = board.GP11
spi_clk = board.GP10
spi_reset = board.GP12
backlight = board.GP13
spi = busio.SPI(spi_clk, spi_mosi)

display_bus = displayio.FourWire(spi, command=tft_dc, chip_select=tft_cs, reset=spi_reset)
display = ST7789(
    display_bus, rotation=270, width=240, height=135, rowstart=40, colstart=53, backlight_pin=backlight
)

top_button = digitalio.DigitalInOut(board.GP15)
top_button.switch_to_input(pull=digitalio.Pull.UP)
bottom_button = digitalio.DigitalInOut(board.GP14)
bottom_button.switch_to_input(pull=digitalio.Pull.UP)

splash = displayio.Group()
display.show(splash)

# Setup eye grid
eyes = displayio.Bitmap(display.width, display.height, 2)
eye_palette = displayio.Palette(2)
eye_palette[0] = 0x000000
eye_palette[1] = 0xFFFFFF
eye_grid = displayio.TileGrid(eyes, pixel_shader=eye_palette)
eye_group = displayio.Group()
eye_group.append(eye_grid)
splash.append(eye_group)

def neutral_eyes():
    eye_size = 20
    ipd = 60
    delta = 20

    for x in range(display.width // 2 - ipd - eye_size, display.width // 2 - ipd + eye_size):
        for y in range(display.height // 2 - eye_size, display.height // 2 + eye_size):
            eyes[x, y] = 1

    for x in range(display.width // 2 + ipd - eye_size, display.width // 2 + ipd + eye_size):
        for y in range(display.height // 2 - eye_size, display.height // 2 + eye_size):
            eyes[x, y] = 1


def blink_eyes():
    eye_size = 10
    ipd = 60
    delta = 20

    for x in range(display.width // 2 - ipd - 2 * eye_size - delta, display.width // 2 - ipd + 2 * eye_size + delta):
        for y in range(display.height // 2 - 2 * eye_size - delta, display.height // 2 + 2 * eye_size + delta):
            eyes[x, y] = 0

    for x in range(display.width // 2 + ipd - 2 * eye_size - delta, display.width // 2 + ipd + 2 * eye_size + delta):
        for y in range(display.height // 2 - 2 * eye_size - delta, display.height // 2 + 2 * eye_size + delta):
            eyes[x, y] = 0

    for x in range(display.width // 2 - ipd - 2 * eye_size, display.width // 2 - ipd + 2 * eye_size):
        for y in range(display.height // 2 - eye_size, display.height // 2 + eye_size):
            eyes[x, y] = 1

    for x in range(display.width // 2 + ipd - 2 * eye_size, display.width // 2 + ipd + 2 * eye_size):
        for y in range(display.height // 2 - eye_size, display.height // 2 + eye_size):
            eyes[x, y] = 1

def clear_screen():
    for x in range(display.width):
        for y in range(display.height):
            eyes[x,y] = 0



led = digitalio.DigitalInOut(board.LED)
led.direction = digitalio.Direction.OUTPUT

pwm_left_leg = pwmio.PWMOut(board.GP21, duty_cycle=2 ** 15, frequency=50)
pwm_right_leg = pwmio.PWMOut(board.GP20, duty_cycle=2 ** 15, frequency=50)
pwm_left_foot = pwmio.PWMOut(board.GP19, duty_cycle=2 ** 15, frequency=50)
pwm_right_foot = pwmio.PWMOut(board.GP18, duty_cycle=2 ** 15, frequency=50)
pwm_left_arm = pwmio.PWMOut(board.GP17, duty_cycle=2 ** 15, frequency=50)
pwm_right_arm = pwmio.PWMOut(board.GP16, duty_cycle=2 ** 15, frequency=50)

# Create a servo object, my_servo.
left_leg = servo.Servo(pwm_left_leg)
right_leg = servo.Servo(pwm_right_leg)
left_foot = servo.Servo(pwm_left_foot)
right_foot = servo.Servo(pwm_right_foot)
left_arm = servo.Servo(pwm_left_arm)
right_arm = servo.Servo(pwm_right_arm)

def original_pos():
    left_leg.angle = 90
    right_leg.angle = 90
    left_foot.angle = 90
    right_foot.angle = 90
    left_arm.angle = 180
    right_arm.angle = 0

def wave_arms(steps):
    for i in range(steps):
        left_arm.angle = 0
        right_arm.angle = 150
        time.sleep(0.2)
        left_arm.angle = 30
        right_arm.angle = 180
        time.sleep(0.2)

def wave_arms_1(steps):
        left_arm.angle = 0
        time.sleep(0.2)
        right_arm.angle = 180

def wave_arms_2(steps):
        left_arm.angle = 180
        time.sleep(0.2)
        right_arm.angle = 180

def wave_arms_3(steps):
        right_arm.angle = 0
        time.sleep(0.2)
        left_arm.angle = 0

def wave_arms_4(steps):
        left_arm.angle = 0
        right_arm.angle = 150
        time.sleep(1)
        left_arm.angle = 30
        right_arm.angle = 180
        time.sleep(1)

def wave_arms_5(steps):
        left_arm.angle = 0
        right_arm.angle = 150
        time.sleep(1)
        left_arm.angle = 30
        right_arm.angle = 180
        time.sleep(1)

def wave_arms_6(steps):
        left_arm.angle = 0
        right_arm.angle = 150
        time.sleep(1)
        left_arm.angle = 30
        right_arm.angle = 180
        time.sleep(1)

def wave_arms_7(steps):
        left_arm.angle = 0
        right_arm.angle = 150
        time.sleep(1)
        left_arm.angle = 30
        right_arm.angle = 180
        time.sleep(1)


def shuffle(steps, backward=False):
    for j in range(steps):
        if backward:
            left_foot.angle = 100
            right_foot.angle = 70
            left_arm.angle = 90
            right_arm.angle = 90
        else:
            left_foot.angle = 70
            right_foot.angle = 100
            left_arm.angle = 90
            right_arm.angle = 90
        for angle in range(65, 115, 5):  # 0 - 180 degrees, 5 degrees at a time.
             right_leg.angle = angle
             left_leg.angle = 180 - angle
             left_arm.angle = left_leg.angle
             right_arm.angle = right_leg.angle
             time.sleep(0.01)
        time.sleep(0.1)
        if backward:
            left_foot.angle = 70
            right_foot.angle = 100
            left_arm.angle = left_foot.angle
            right_arm.angle = right_foot.angle
        else:
            left_foot.angle = 100
            right_foot.angle = 70
            left_arm.angle = left_foot.angle
            right_arm.angle = right_foot.angle
        for angle in range(65, 115, 5): # 180 - 0 degrees, 5 degrees at a time.
             right_leg.angle = 180-angle
             left_leg.angle = angle
             left_arm.angle = left_leg.angle
             right_arm.angle = right_leg.angle
             time.sleep(0.01)
        time.sleep(0.1)
        original_pos()

def step_number1(steps, Right=True):
    for j in range(steps):
        if Right:
            left_foot.angle = 70
            right_foot.angle = 100
            left_arm.angle = right_foot.angle
            right_arm.angle = left_foot.angle
        else:
            left_foot.angle = 100
            right_foot.angle = 70
        for angle in range(45, 115, 10):  # 0 - 180 degrees, 5 degrees at a time.
             right_leg.angle = angle
             left_leg.angle = 180-angle
             left_arm.angle = right_foot.angle
             right_arm.angle = left_foot.angle
             left_foot.angle = 30+angle
             right_foot.angle = 120-angle
             left_arm.angle = right_foot.angle
             right_arm.angle = left_foot.angle
             time.sleep(0.02)
        time.sleep(0.5)
        if Right:
            left_foot.angle = 100
            right_foot.angle = 70
            left_arm.angle = right_foot.angle
            right_arm.angle = left_foot.angle
        else:
            left_foot.angle = 100
            right_foot.angle = 70
            left_arm.angle = right_foot.angle
            right_arm.angle = left_foot.angle
        for angle in range(45, 115, 10): # 180 - 0 degrees, 5 degrees at a time.
             left_leg.angle = angle
             right_leg.angle = 180-angle
             left_arm.angle = right_foot.angle
             right_arm.angle = left_foot.angle
             right_foot.angle = 30+angle
             left_foot.angle = 120-angle
             left_arm.angle = right_foot.angle
             right_arm.angle = left_foot.angle
             time.sleep(0.01)
        time.sleep(0.5)
    original_pos()

def step_number2(steps):
    for _ in range(steps):

        right_foot.angle=70
        left_foot.angle = 110
        left_arm.angle = 90
        right_arm.angle = 90
        time.sleep(0.3)

        for angle in range(90, 135, 5):  # 0 - 180 degrees, 5 degrees at a time.
             right_leg.angle = angle
             left_leg.angle = angle
             left_arm.angle = angle
             right_arm.angle = angle
        time.sleep(0.1)

        right_foot.angle=130
        left_foot.angle=50
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
        time.sleep(0.3)

        for angle in range(90, 135, 5):  # 0 - 180 degrees, 5 degrees at a time.
             right_leg.angle = 180-angle
             left_leg.angle = 180-angle
             left_arm.angle = right_leg.angle
             right_arm.angle = left_leg.angle
        time.sleep(0.1)
        original_pos()

def step_number3():

    for angle in range(0, 80, 15):  # 0 - 180 degrees, 5 degrees at a time.
        right_foot.angle = angle
        left_foot.angle = angle
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
    time.sleep(0.1)


    for angle in range(0, 90, 5):
        right_foot.angle = angle
        left_foot.angle = 180-angle
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
    time.sleep(0.1)

    for angle in range(90, 130, 15):  # 0 - 180 degrees, 5 degrees at a time.
        right_foot.angle = angle
        left_foot.angle = angle
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
    time.sleep(0.1)


    for angle in range(180, 90, -15):
        right_foot.angle = angle
        left_foot.angle = angle
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
    time.sleep(0.1)

    left_leg.angle = 90
    right_leg.angle = 90
    left_foot.angle = 90
    right_foot.angle = 90
    left_arm.angle = 90
    right_arm.angle = 90

def step_number4():
    shuffle(2)
    shuffle(2, backward=True)
    original_pos()

def step_number5():

    for angle in range(90, 0, -15):  # 0 - 180 degrees, 5 degrees at a time.
        right_foot.angle = angle
        left_foot.angle = 180-angle
        left_arm.angle = right_foot.angle
        right_arm.angle = left_foot.angle
    time.sleep(0.1)


    for angle in range(0, 90, 15):
        right_leg.angle = angle
        left_leg.angle = 180-angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)

    for angle in range(90, 180, 15):  # 0 - 180 degrees, 5 degrees at a time.
        right_leg.angle = angle
        left_leg.angle = angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)


    for angle in range(180, 90, -15):
        right_leg.angle = angle
        left_leg.angle = angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)
    original_pos()

def step_number6():

    for angle in range(90, 0, -15):  # 0 - 180 degrees, 5 degrees at a time.
        right_leg.angle = angle
        left_leg.angle = 180-angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)


    for angle in range(0, 90, 15):
        right_leg.angle = angle
        left_leg.angle = 180-angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)

    for angle in range(90, 180, 15):  # 0 - 180 degrees, 5 degrees at a time.
        right_leg.angle = angle
        left_leg.angle = angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)


    for angle in range(180, 90, -15):
        right_leg.angle = angle
        left_leg.angle = angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)

    for angle in range(90, 0, -15):
        right_leg.angle = angle
        left_leg.angle = angle
        left_arm.angle = right_leg.angle
        right_arm.angle = left_leg.angle
    time.sleep(0.1)
    original_pos()

def update_mode(current_mode):
    if current_mode < 7:
        current_mode += 1
    else:
        current_mode = 0
    return current_mode

text_area = label.Label(terminalio.FONT, text="Start", color=TEXT_COLOR)
text_width = text_area.bounding_box[2] * FONTSCALE
text_group = displayio.Group(
        scale=FONTSCALE,
        x=display.width // 2 - text_width // 2,
        y=display.height // 2,
    )
text_group.append(text_area)  # Subgroup for text scaling
splash.append(text_group)

mode = 0
count = 0
original_pos()
wave_arms(5)

for f in (262, 294, 330, 349, 392, 440, 494, 523):
    buzzer.frequency = f
    buzzer.duty_cycle = 2**15
    time.sleep(0.25)  # On for 0.25s
    wave_arms(1)

original_pos()
buzzer.duty_cycle = 0
while True:
    count += 1
    text_group.x = display.width // 2 - text_area.bounding_box[2] * FONTSCALE // 2
    if not top_button.value:
        mode = update_mode(mode)
        if mode == 1:
            clear_screen()
        while not top_button.value:
            pass
    if mode == 0:
        text_area.text = ""
        neutral_eyes()
        blink_eyes()

    if mode == 1:
        text_area.text = "The Slide!"
        if not bottom_button.value:
            step_number1(2)

    if mode == 2:
        text_area.text = "The Shuffle!"
        if not bottom_button.value:
            step_number2(2)

    if mode == 3:
        text_area.text = "The Twist!"
        if not bottom_button.value:
            step_number3()

    if mode == 4:
        text_area.text = "The Step!"
        if not bottom_button.value:
            step_number4()

    if mode == 5:
        text_area.text = "The Hop!"
        if not bottom_button.value:
            step_number5()
    if mode == 6:
        text_area.text = "The Shake!"
        if not bottom_button.value:
            step_number6()
    if mode == 7:
        text_area.text = "DANCE PARTY"
        if not bottom_button.value:
            step_number1(2)
            step_number2(2)
            step_number3()
            step_number4()
            step_number5()
            time.sleep(0.5)
            step_number6()
            time.sleep(1)
