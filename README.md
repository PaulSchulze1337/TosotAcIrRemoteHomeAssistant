# Wifi IR Remote for Tosot AC for Home Assistant (ESPHome DIY Project)

## Description

In this project, I built a Wifi IR remote control for my Tosot Air Conditioner (Tosot Twin AG12) using ESPHome and Home Assistant. The IR remote control is based on an ESP8266 board (Wemos LoLin D1 R2 & Mini). It's a DIY project, so you need to solder the parts together.


## Pictures

### Interface in Home Assistant
![home_assistant_interface.png](img/home_assistant_interface.png)

This interface comes from the ESPHome device builder and is used to control the Tosot Air Conditioner.


### Case for the IR Remote Control (3D model can be found in thingyverse)
![ir_remote_control_case.webp](img/ir_remote_control_case.webp)

![ir_remote_control_case_open.webp](img/ir_remote_control_case_open.webp)
Note: The green LED is just a power indicator and NOT an essential part of the ir remote control. In the diagrams and item list below, it is not mentioned.


### My soldering solution for the IR remote control (can be improved, but it works)
![grid_hole_board_with_soldered_parts.webp](img/grid_hole_board_with_soldered_parts.webp)
Notes:
- There is one electrolytic capacitor too much soldered, it was a mistake, but I was to lazy to remove it. See Fritzing diagram below for the correct wiring. The correct electrolytic capacitor needs to be connected to the 5 V of the ESP8266 board.
- The green LED is just a power indicator and NOT an essential part of the ir remote control. In the diagrams and item list below, it is not mentioned.


## Requirements
- [Home Assistant 2025.5.0](https://www.home-assistant.io/) or later (earlier versions may work but are not tested)
- Installed [ESPHome](https://esphome.io/) add-on in Home Assistant
- Tosot Air Conditioner with IR remote control (needs to be adapted in [`esphome-ir-remote.yaml`](source/esphome-ir-remote.yaml) config)
- ESP8266-based board (like Wemos LoLin D1 R2 & Mini in this example)
- see **parts list** below for soldering

Note: ESPHome config need to be adpated to your specific AC Model und ESP8266 board.


## ESPHome Config / Source Code for ESPHome Device Builder 

[`esphome-ir-remote.yaml`](source/esphome-ir-remote.yaml)

For Installation of a generic ESPHome device, reed the dokumentation here:
https://esphome.io/guides/getting_started_hassio

- First install ESPHome on your Microcontroller (e.g., Wemos LoLin D1 R2 & Mini) and connect it to your Home Assistant instance.
  (It must be listed in the ESPHome Device Builder on your Home Assistant instance.)
- Then copy the code below into the ESPHome Device Builder and upload it to your ESP8266 board.
 

``` yaml
esphome:
  name: tosot-twinag12-ir-remote
  friendly_name: ESPHome IR AC Control
  min_version: 2025.5.0
  name_add_mac_suffix: false


esp8266:
  board: d1_mini

# Enable logging
logger:

# Enable Home Assistant API
api:

# Allow Over-The-Air updates
ota:
- platform: esphome

wifi:
  ssid: 'add_your_wifi_ssid_here'
  password: 'add_your_wifi_password_here'

remote_transmitter:
  id: retx
  pin:
    number: D7
        
  carrier_duty_percent: 50%

climate:
  - platform: gree
    name: "Living Room AC"
    model: yac
    icon: "mdi:air-conditioner"
    supports_heat: false
```

## Fritzing Diagram
You need to adapt this Fritzing diagram to your specific grid hole board. This is just a demonstration of how the parts should be connected. 

![IR Remote Control Wirering - Fritzing.png](img/IR%20Remote%20Control%20Wirering%20-%20Fritzing.png)


## Parts for soldering
- 1x ESP8266 board (e.g., Wemos LoLin D1 R2 & Mini)
- 1x Grid hole board
- 2x IC Sockets (optional, but recommended for easy replacement of the ESP8266 board)
- 1x PCB Screw Terminal Block (optional, but recommended for easy connection of the IR LED)
- 1x IR LED (e.g., IR 7373C EVL)
- 1x Electrolytic capacitor (10 µF)
- 1x Resistor 10 Ω (for IR LED)
- 1x Resistor 1 kΩ (as pull-down)
- 1x Resistor 20 Ω (as pre-resistor after the D7 pin)
- 1x Mosfet (e.g., IRF 540N)

The mosfet is used to switch the IR diode. The mosfet is connected to 5 V with a ≈ 20 Ohm resistor.