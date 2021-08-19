Short story:

I tried to reverse-engineer the library but found endless amount of reference.
And when I tried to trace them I end up in places like "usbCore.cpp" and things that are too deep to touch on.
The library is otherwise structured quite badly. He could make functions called "setAxis" with a parameter indicating which axis. same with setbutton.
I will try to make improvements and make clean-ups while reducing footprint & reduce the number of files.

# Arduino Joystick Library

#### Version 2.0.7

This library can be used with Arduino IDE 1.6.6 (or above) to have an
Arduino Leonardo, Arduino Micro (or any other board with ATmega32u4) appear to the host computer as a joystick (or gamepad).
This library will also work with the Arduino Due, thanks to @Palakis (https://github.com/Palakis). A complete list of supported boards can be found in the . This will not work with Arduino IDE 1.6.5 (or below) or with boards without native USB support (uno, mega, nano, mkr).

## Features

The joystick or gamepad can have the following features:
- up to 32 Buttons
- Up to 2 Hat Switches
- X, Y, and Z Axis & Rotation (up to 16-bit precision)
- Rudder (up to 16-bit precision)
- Throttle (up to 16-bit precision)
- Accelerator (up to 16-bit precision)
- Brake (up to 16-bit precision)
- Steering (up to 16-bit precision)

## Installation Instructions

The following instructions can be used to install the latest version of the library in the Arduino IDE (thanks to [@per1234](https://github.com/per1234) for this update):

1. Download https://github.com/MHeironimus/ArduinoJoystickLibrary/archive/master.zip
2. In the Arduino IDE, select `Sketch` > `Include Library` > `Add .ZIP Library...`.  Browse to where the downloaded ZIP file is located and click `Open`. The Joystick library's examples will now appear under `File` > `Examples` > `Joystick`.

## Examples

The following example Arduino sketch files are included in this library:

- `JoystickTest` - Simple test of the Joystick library. It exercises many of the Joystick library’s functions when pin A0 is grounded.
This will be changed, where the arduino will act as a controller, but the input is randomized every second for every channel.

- `MultipleJoystickTest` - Creates 4 Joysticks using the library and exercises the first 16 buttons, the X axis, and the Y axis of each joystick when pin A0 is grounded.
this will be removed. there will be absolutely no need to create multiple joystick objects.

- `JoystickButton` - Creates a Joystick and maps pin 9 to button 0 of the joystick, pin 10 to button 1, pin 11 to button 2, and pin 12 to button 3.
This will be changed. analog pin 0123 is used for direction buttons. 7 used for button 2, 8 for button 3.

- `JoystickKeyboard` - Creates a Joystick and a Keyboard. Maps pin 9 to Joystick Button 0, pin 10 to Joystick Button 1, pin 11 to Keyboard key 1, and pin 12 to Keyboard key 2.
this will be removed.

- `GamepadExample` - Creates a simple Gamepad with an Up, Down, Left, Right, and Fire button.
this will be removed.

- `DrivingControllerTest` - Creates a Driving Controller and tests 4 buttons, the Steering, Brake, and Accelerator when pin A0 is grounded. 
this will be changed. a potentiometer wired to A0 is needed, and two buttons across pin 7 and 8.

- `FlightControllerTest` - Creates a Flight Controller and tests 32 buttons, the X and Y axis, the Throttle, and the Rudder when pin A0 is grounded.
this will be removed.

- `HatSwitchTest` - Creates a joystick with two hat switches. Grounding pins 4 - 11 cause the hat switches to change position.  
this ... this will stay.

## Joystick Library API

The following API is available if the Joystick library in included in a sketch file.

### Joystick\_(...)

Constructor used to initialize and setup the Joystick. The following optional parameters are available:

- `uint8_t hidReportId` - Default: `0x03` - Indicates the joystick's HID report ID. This value must be unique if you are creating multiple instances of Joystick. Do not use `0x01` or `0x02` as they are used by the built-in Arduino Keyboard and Mouse libraries.
- `uint8_t joystickType` - Default: `JOYSTICK_TYPE_JOYSTICK` or `0x04` - Indicates the HID input device type. Supported values:
  - `JOYSTICK_TYPE_JOYSTICK` or `0x04` - Joystick 
  - `JOYSTICK_TYPE_GAMEPAD` or `0x05` - Gamepad
  - `JOYSTICK_TYPE_MULTI_AXIS` or `0x08` - Multi-axis Controller 
- `uint8_t buttonCount` - Default: `32` - Indicates how many buttons will be available on the joystick.
- `uint8_t hatSwitchCount` - Default: `2` - Indicates how many hat switches will be available on the joystick. Range: `0` - `2`
- `bool includeXAxis` - Default: `true` - Indicates if the X Axis is available on the joystick.
- `bool includeYAxis` - Default: `true` - Indicates if the Y Axis is available on the joystick.
- `bool includeZAxis` - Default: `true` - Indicates if the Z Axis (in some situations this is the right X Axis) is available on the joystick.
- `bool includeRxAxis` - Default: `true` - Indicates if the X Axis Rotation (in some situations this is the right Y Axis) is available on the joystick.

- `bool includeRyAxis` - Default: `true` - Indicates if the Y Axis Rotation is available on the joystick.
- `bool includeRzAxis` - Default: `true` - Indicates if the Z Axis Rotation is available on the joystick.
- `bool includeRudder` - Default: `true` - Indicates if the Rudder is available on the joystick.
- `bool includeThrottle` - Default: `true` - Indicates if the Throttle is available on the joystick.

- `bool includeAccelerator` - Default: `true` - Indicates if the Accelerator is available on the joystick.
- `bool includeBrake` - Default: `true` - Indicates if the Brake is available on the joystick.
- `bool includeSteering` - Default: `true` - Indicates if the Steering is available on the joystick.

this function will be updated. instead of passing a endless list of "includeXaxis" arguments, the user will pass a single "short", in the form of 0xIDTP 0XYZ RyzU TCBS, then two uint8_ts indicating the buttoncount and hatswitchcount.

Internal cleanup:
I will make sure that the amount of sticks configured here actually affects how the arduino will appear to the host computer.

The following constants define the default values for the constructor parameters listed above:

- `JOYSTICK_DEFAULT_REPORT_ID` is set to `0x03`
- `JOYSTICK_DEFAULT_BUTTON_COUNT` is set to `32`
- `JOYSTICK_DEFAULT_HATSWITCH_COUNT` is set to `2`

### Joystick.begin(bool initAutoSendState)

Starts emulating a game controller connected to a computer. By default, all methods update the game controller state immediately. If `initAutoSendState` is set to `false`, the `Joystick.sendState` method must be called to update the game controller state.

The entire AutoSendState function will be removed. The user can decide whether to update the state (via sendState) after each keypress or on a number-

### Joystick.end()

Stops the game controller emulation to a connected computer.

### Joystick.setXAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the X axis. Default: `0` to `1023`

### Joystick.setXAxis(int16_t value)

Sets the X axis value. See `setXAxisRange` for the range.

### Joystick.setYAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Y axis. Default: `0` to `1023`

### Joystick.setYAxis(int16_t value)

Sets the Y axis value. See `setYAxisRange` for the range.

### Joystick.setZAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Z axis. Default: `0` to `1023`

### Joystick.setZAxis(int16_t value)

Sets the Z axis value. See `setZAxisRange` for the range.

### Joystick.setRxAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the X axis rotation. Default: `0` to `1023`

### Joystick.setRxAxis(int16_t value)

Sets the X axis rotation value. See `setRxAxisRange` for the range.

### Joystick.setRyAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Y axis rotation. Default: `0` to `1023`

### Joystick.setRyAxis(int16_t value)

Sets the Y axis rotation value. See `setRyAxisRange` for the range.

### Joystick.setRzAxisRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Z axis rotation. Default: `0` to `1023`

### Joystick.setRzAxis(int16_t value)

Sets the Z axis rotation value. See `setRzAxisRange` for the range.

### Joystick.setRudderRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Rudder. Default: `0` to `1023`

### Joystick.setRudder(int16_t value)

Sets the Rudder value. See `setRudderRange` for the range.

### Joystick.setThrottleRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Throttle. Default: `0` to `1023`

### Joystick.setThrottle(int16_t value)

Sets the Throttle value. See `setThrottleRange` for the range.

### Joystick.setAcceleratorRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Accelerator. Default: `0` to `1023`

### Joystick.setAccelerator(int16_t value)

Sets the Accelerator value. See `setAcceleratorRange` for the range.

### Joystick.setBrakeRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Brake. Default: `0` to `1023`

### Joystick.setBrake(int16_t value)

Sets the Brake value. See `setBrakeRange` for the range.

### Joystick.setSteeringRange(int16_t minimum, int16_t maximum)

Sets the range of values that will be used for the Steering. Default: `0` to `1023`

### Joystick.setSteering(int16_t value)

Sets the Steering value. See `setSteeringRange` for the range.

All of the function to set range of a certain axis will be combined into one function. The function will be called "setRange" and take three parameters: the name of the axis (in the form of macros), the min and the max.

All of the function to set the value of a specific axis will be combined into one function. The function will be called "setAxis" and take two parameters: the name of the axis (in the form of macros), and the value.

### Joystick.setButton(uint8_t button, uint8_t value)

Sets the state (`0` or `1`) of the specified button (range: `0` - (`buttonCount - 1`)). Button number start counting from 0. The value is `1` if the button is pressed and `0` if the button is released.

This function will be changed. Rather than using 0s and 1s, macros indicating state (e.g. PRESSED, RELEASED) will be used.

### Joystick.pressButton(uint8_t button)

Press the indicated button (range: `0` - (`buttonCount - 1`)). Button number start counting from 0. This function will be removed.

### Joystick.releaseButton(uint8_t button)

Release the indicated button (range: `0` - (`buttonCount - 1`)). Button number start counting from 0. This function will be removed.

### Joystick.setHatSwitch(int8_t hatSwitch, int16_t value)

Sets the value of the specified hat switch.
The value is from 0° to 360°, but in 45° increments. Any value less than 45° will be rounded down (i.e. 44° is rounded down to 0°, 89° is rounded down to 45°, etc.). Set the value to `JOYSTICK_HATSWITCH_RELEASE` or `-1` to release the hat switch.

This function will be changed. Rather than using degrees, macros indicating direction (e.g. U,D,L,R,UL,LR,DL,DR) will be used.

### Joystick.sendState()

Sends the updated joystick state to the host computer. Only needs to be called if `AutoSendState` is `false` (see `Joystick.begin` for more details).

See the [Wiki](https://github.com/MHeironimus/ArduinoJoystickLibrary/wiki) for more details on things like FAQ, supported boards, testing, etc.
