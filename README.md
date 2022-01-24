# P1 Decrypter

Python script to decrypt Smart Meter output and send it over udp or to a serial port. 

Works with: Sagemcom T210-D-r (Austria).
Please create an issue if you found out that another Smart Meter will work too.

## Precondition

- Packages needed: pyserial pycryptodomex 
    - `sudo apt install python3 python3-serial python3-pycryptodome`
    - or `pip install pyserial pycryptodomex`
- Smart Meter that has a P1 interface (Tested with Smart Meter: Sagemcom T210-D-r in Austria)
- FTDI USB cable to connect to the Smart Meter
    - One possibly option: [https://www.aliexpress.com/item/32945225256.html](https://www.aliexpress.com/item/32945225256.html)
- Your energy provider has to activate your customer interface and provide the encryption key _"Global Unicast Encryption Key (GUEK)"_

## Usage

Usage example to send over UDP: `p1decrypter.py -u -ui UDP_SERVER_IP key`

All parameters:
```bash
usage: p1decrypter.py [-h] [-iport SERIAL_INPUT_PORT] [-ibaudrate SERIAL_INPUT_BAUDRATE] [-iparity SERIAL_INPUT_PARITY] [-istopbits SERIAL_INPUT_STOPBITS] [-m MAPPING] [-a AAD] [-u] [-ui UDP_HOST] [-up UDP_PORT] [-s]
                      [-oport SERIAL_OUTPUT_PORT] [-obaudrate SERIAL_OUTPUT_BAUDRATE] [-oparity SERIAL_OUTPUT_PARITY] [-ostopbits SERIAL_OUTPUT_STOPBITS] [-r] [-v] [-l LOGFILE] [-c CONFIGFILE]
                      key

positional arguments:
  key                   Global Unicast Encryption Key (GUEK)

optional arguments:
  -h, --help            show this help message and exit
  -iport SERIAL_INPUT_PORT, --serial-input-port SERIAL_INPUT_PORT
                        Serial input port. Default: /dev/ttyUSB0
  -ibaudrate SERIAL_INPUT_BAUDRATE, --serial-input-baudrate SERIAL_INPUT_BAUDRATE
                        Serial input baudrate. Default: 115200
  -iparity SERIAL_INPUT_PARITY, --serial-input-parity SERIAL_INPUT_PARITY
                        Serial input parity. Default: None
  -istopbits SERIAL_INPUT_STOPBITS, --serial-input-stopbits SERIAL_INPUT_STOPBITS
                        Serial input stopbits. Default: 1
  -m MAPPING, --mapping MAPPING
                        Value mapping. Default: '1-0:1.8.0','(?<=1-0:1.8.0\().*?(?=\*Wh)',\n'1-0:1.7.0','(?<=1-0:1.7.0\().*?(?=\*W)'\n'1-0:2.8.0','(?<=1-0:2.8.0\().*?(?=\*Wh)'\n'1-0:2.7.0','(?<=1-0:2.7.0\().*?(?=\*W)'
  -a AAD, --aad AAD     Additional authenticated data. Default: 3000112233445566778899AABBCCDDEEFF
  -u, --send-to-udp     Send data to UDP. Default: false
  -ui UDP_HOST, --udp-host UDP_HOST
                        UDP IP / Host
  -up UDP_PORT, --udp-port UDP_PORT
                        UDP port
  -s, --send-to-serial-port
                        Send data to output serial port. Use socat to generate virtual port e.g.: socat -d -d pty,raw,echo=0,link=/dev/p1decrypterI pty,raw,echo=0,link=/dev/p1decrypterO
  -oport SERIAL_OUTPUT_PORT, --serial-output-port SERIAL_OUTPUT_PORT
                        Serial output port. Default: /dev/p1decrypter
  -obaudrate SERIAL_OUTPUT_BAUDRATE, --serial-output-baudrate SERIAL_OUTPUT_BAUDRATE
                        Serial output baudrate. Default: 115200
  -oparity SERIAL_OUTPUT_PARITY, --serial-output-parity SERIAL_OUTPUT_PARITY
                        Serial output parity. Default: None
  -ostopbits SERIAL_OUTPUT_STOPBITS, --serial-output-stopbits SERIAL_OUTPUT_STOPBITS
                        Serial output stopbits. Default: 1
  -r, --raw             Output raw, without mapping
  -v, --verbose         Verbose mode
  -l LOGFILE, --logfile LOGFILE
                        Logfile path
  -c CONFIGFILE, --configfile CONFIGFILE
                        Configfile path
```

## Value mapping

If you don't need all informations of your smart meter you can use the value mapping. 
Format is 'label','regex'\n'label','regex'\n'label','regex'...

- Disable value mapping by using `raw` property.
- Mapping over configuration file must be base64 encoded.

### Example

To get `1-0:1.8.0:001234567` from raw output: `1-0:1.8.0(001234567*Wh)` 
use value mapping like this: `'1-0:1.8.0','(?<=1-0:1.8.0\().*?(?=\*Wh)'`

OBIS-Codes of Sagemcom T210-D-r (Austria): [https://github.com/metrophos/LoxBerry-Plugin-P1-Decrypter](https://github.com/metrophos/LoxBerry-Plugin-P1-Decrypter)

## Thanks to:

- tknaller - For his modified fork: https://github.com/tknaller/smarty_dsmr_proxy