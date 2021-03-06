[![GoDoc](https://godoc.org/github.com/linklayer/go-cantact?status.svg)](http://godoc.org/github.com/linklayer/go-cantact)
# go-cantact

A package for communicating on Controller Area Network buses using the
[CANtact](http://cantact.io) hardawre.

## Details

This package makes use of the [serial](https://github.com/tarm/serial) library
for providing access to CANtact's serial port. It provides functions for sending
and receiving frames in the correct format for the device.

## Usage
```go
package main

import (
	"log"
	"os"
	"github.com/linklayer/go-cantact"
)

func main() {
	d, err := cantact.NewDevice(os.Args[1])
	if err != nil {
		log.Fatal(err)
	}
	
	// set bitrate mode to 6, 500 kbps
	d.SetBitrate(6)

	// open connection to CAN bus
	d.Open()

	// send a frame
	data := []byte{0xDE, 0xAD, 0xBE, 0xEF}
	txFrame := cantact.Frame{ID: 0x123, Dlc: len(data), Data: data}
	d.WriteFrame(txFrame)

	// read a frame (blocking call)
	rxFrame, err := d.ReadFrame()
	if err != nil {
		log.Panic(err)
	}
	log.Println(rxFrame)
}
```

