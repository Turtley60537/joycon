# joycon

Nintendo Switch's Joycon Device access library(via bluetooth only)

## Reverse engineering info

https://github.com/dekuNukem/Nintendo_Switch_Reverse_Engineering

## Feature

- supported deveces: Joycon(L/R), Pro-Controller
- get: Digial Buttons state
- get: Analog Sticks state
- set: Raw Vibration data
- calibration support for analog stick.

## Dependencies

- go get -u github.com/flynn/hid
- go get -u github.com/shibukawa/gotomation `optional`

## Usage

In advance, you perform Bluetooth pairing for Joycon.
(Joycon must be connected before execute below code.)

Note: When Joycon is fitted to the main body, BT sessions are overwritten, so when you connect to PC later, you need to redo pairing.

```go
package main

import "github.com/nobonobo/joycon"

func main() {
    devices, err := joycon.Search(joycon.JoyConL)
    if err != nil {
        log.Fatalln(err)
    }
    jc, err := joycon.NewJoycon(devices[0].Path, false)
    if err != nil {
        log.Fatalln(err)
    }
    s := <-jc.State()
    fmt.Println(s.Buttons)  // Button bits
    fmt.Println(s.LeftAdj)  // Left Analog Stick State
    fmt.Println(s.RightAdj) // Right Analog Stick State
    a := <-jc.Sensor()
    fmt.Println(a.Accel) // Acceleration Sensor State
    fmt.Println(a.Gyro)  // Gyro Sensor State

    jc.Close()
}
```
## TODO

- [ ] Deadzone parameter read from SPI memory. 
- [x] Rich Vibration support.
- [ ] Set Player LED.
- [ ] Set HomeButton LED.
- [ ] Low power mode support.
- [ ] IR sensor capture.(wip)
