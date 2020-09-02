你好！
很冒昧用这样的方式来和你沟通，如有打扰请忽略我的提交哈。我是光年实验室（gnlab.com）的HR，在招Golang开发工程师，我们是一个技术型团队，技术氛围非常好。全职和兼职都可以，不过最好是全职，工作地点杭州。
我们公司是做流量增长的，Golang负责开发SAAS平台的应用，我们做的很多应用是全新的，工作非常有挑战也很有意思，是国内很多大厂的顾问。
如果有兴趣的话加我微信：13515810775  ，也可以访问 https://gnlab.com/，联系客服转发给HR。
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
