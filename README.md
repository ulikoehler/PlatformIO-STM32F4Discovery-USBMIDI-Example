# README

This example implements a USB MIDI device to demonstrate the use of the
USB device stack. It implements the device configuration found in Appendix
B of the Universal Serial Bus Device Class Definition for MIDI Devices.

This example (including this README) is heavily based on the [`libopencm3`](https://github.com/libopencm3) [USB-MIDI example](https://github.com/libopencm3/libopencm3-examples/tree/master/examples/stm32/f4/stm32f4-discovery/usb_midi) and contains only minor modification by [Uli Köhler](https://techoverflow.net) to make it work with the STM32F4 discovery and the current version of PlatformIO as of 12/2019.

You can program this example using the built-in STLinkV2 on the [STM32F4Discovery](https://www.st.com/en/evaluation-tools/stm32f4discovery.html) board. Just click `PlatformIO: Upload` (the right arrow) on the bottom panel of your Visual Studio Code instance with PlatformIO installed. Running for the first time might take some time since PlatformIO will download all the libraries, compilers etc.

The 'USER' button sends note on/note off messages.

The board will also react to identity request (or any other data sent to
the board) by transmitting an identity message in reply.

## Board connections

| Port  | Function       | Description                               |
| ----- | -------------- | ----------------------------------------- |
| `CN5` | `(USB_OTG_FS)` | USB acting as device, connect to computer |

## Testing

To list midi devices, which should include this demo device

    $ amidi -l
    Dir Device    Name
    IO  hw:2,0,0  MIDI demo MIDI 1
    $

To record events, while pushing the user button

    $ amidi -d -p hw:2,0,0
    
    90 3C 40   -- key down
    80 3C 40   -- key up
    90 3C 40
    80 3C 40^C
    12 bytes read
    $

To query the system identity, note this dump matches sysex\_identity[] in the
source.

    $ amidi -d -p hw:2,0,0 -s Sysexdump.syx
    
    F0 7E 00 7D 66 66 51 19 00 00 01 00 F7
    ^C
    13 bytes read
    $

