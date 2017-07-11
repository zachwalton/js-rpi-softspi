# SoftSPI
This library contains a javascript class, which implements the SPI (Serial Peripheral Interface Bus)
for a Raspberry Pi. In contrast to other libraries it is a software implementation.
So it does not use the hardware SPI of the Raspberry Pi and you can choose any pin
of the GPIO for the needed logic signals of SPI.

##Motivation
I wanted to control some components with my Raspberry Pi, which are using SPI.
But a couple of clients (e.g. WS2801 led stripe) do not support a client select.
So I needed a software SPI and didn't find one for javascript. Now here is.

## Precondition
The library is implemented in ECMAScript 2015, so your project should support
at least this version.

## Installation
Install it via npm:
```shell
$ npm install rpi-softspi
```

## Usage
Start with importing the module via:
```js
var softspi = require('rpi-softspi')
```

Create an instance for your client device specifying the used pins and 
needed configurations for the communication in an options object:
```js
let client = new softspi({
   clock: 15,     // pin number of SCLK
   mosi: 11,      // pin number of MOSI
   miso: 13,      // pin number of MISO
   client: 16,    // pin number of CS
   clientSelect: rpio.LOW, // trigger signal for the client
   mode: 0,                // clock mode (0 - 3)
   bitOrder: softspi.MSB   // bit order in communication
})
```
> At least you have to specify the clock pin, all other can be left unspecified
> to use the default configuration

This is the default configuration:
```js
default = {
   clock: null,
   miso: null,
   mosi: null,
   client: null,
   clientSelect: rpio.LOW,
   mode: 0,
   bitOrder: SoftSPI.MSB
}
```

Clock modes are:
| Mode | Polarity | Phase |
|:----:|:--------:|:-----:|
| 0    | 0        | 0     |
| 1    | 0        | 1     |
| 2    | 1        | 0     |
| 3    | 1        | 1     |

Start the communication:
```js
client.open()
```
> The SoftSPI is in an invalid state till you open the communication. If it is
> invalid you cannot transfer data.

Close the communication:
```js
client.close()
```
> Once you close the communication the SoftSPI changes again to invalid state.

Read (only) a number of bytes from the client:
```js
let bytes = client.read(5)
// bytes[0] = first byte
```

Write data (only) to the client:
```js
client.write([0xff, 0x01, 0xab]) //supply any data
```

Transfer data to and from the client:
```js
let bytes = client.transfer([0xff, 0x01, 0xab])
```
> The number of returned bytes is the same number of bytes you have supplied

## API
Check out the [documentation](doc) for details.

## License
MIT

## Additional Information
Find a description on [Wikipedia - Serial Peripheral Interface Bus](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus)
or with the keywords SPI as well as Serial Peripheral Interface.