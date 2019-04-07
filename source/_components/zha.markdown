---
layout: page
title: "Zigbee Home Automation"
description: "Instructions on how to integrate your Zigbee Home Automation (ZHA) devices within Home Assistant."
date: 2017-02-22 19:59
sidebar: true
comments: false
sharing: true
footer: true
logo: zigbee.png
ha_category:
  - Binary Sensor
  - Fan
  - Light
  - Sensor
  - Switch
ha_release: 0.44
ha_iot_class: Local Polling
redirect_from:
  - /components/binary_sensor.zha/
  - /components/fan.zha/
  - /components/light.zha/
  - /components/sensor.zha/
  - /components/switch.zha/
---

[Zigbee Home Automation](http://www.zigbee.org/zigbee-for-developers/applicationstandards/zigbeehomeautomation/)
integration for Home Assistant allows you to connect many off-the-shelf Zigbee based devices to Home Assistant, using one of the available Zigbee radio modules compatible with [zigpy](https://github.com/zigpy/zigpy) (an open source Python library implementing a Zigbee stack, which in turn relies on separate libraries which can each interface a with Zigbee radio module a different manufacturer).

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Sensor
- Light
- Switch
- Fan

ZHA exception and deviation handling:

Zigbee devices that deviate from or do not fully conform to the standard specifications set by the [Zigbee Alliance](https://www.zigbee.org) may require the development of custom [ZHA Device Handlers](https://github.com/dmulcahey/zha-device-handlers) (ZHA custom quirks handler implementation) to for all their functions to work properly with the ZHA component in Home Assistant. These ZHA Device Handlers for Home Assistant can thus be used to parse custom messages to and from Zigbee devices.

The custom quirks implementations for zigpy implemented as ZHA Device Handlers for Home Assistant are a similar concept to that of [Hub-connected Device Handlers for the SmartThings Classics platform](https://docs.smartthings.com/en/latest/device-type-developers-guide/) as well as that of [Zigbee-Shepherd Converters as used by Zigbee2mqtt](https://www.zigbee2mqtt.io/how_tos/how_to_support_new_devices.html), meaning they are each virtual representations of a physical device that expose additional functionality that is not provided out-of-the-box by the existing integration between these platforms.

Known working Zigbee radio modules:

- EmberZNet based radios using the EZSP protocol (via the [bellows](https://github.com/zigpy/bellows) library for zigpy)
  - [Nortek GoControl QuickStick Combo Model HUSBZB-1 (Z-Wave & Zigbee USB Adapter)](https://www.nortekcontrol.com/products/2gig/husbzb-1-gocontrol-quickstick-combo/)
  - [Elelabs Zigbee USB Adapter](https://elelabs.com/products/elelabs_usb_adapter.html)
  - [Elelabs Zigbee Raspberry Pi Shield](https://elelabs.com/products/elelabs_zigbee_shield.html)
- XBee Zigbee based radios (via the [zigpy-xbee](https://github.com/zigpy/zigpy-xbee) library for zigpy)
  - Digi XBee Series 2C (S2C) modules
- Dresden-Elektronik deCONZ based Zigbee radios (via the [zigpy-deconz](https://github.com/zigpy/zigpy-deconz) library for zigpy)
  - [ConBee USB adapter from Dresden-Elektronik](https://www.dresden-elektronik.de/conbee/)
  - [RaspBee Raspberry Pi Shield from Dresden-Elektronik](https://www.dresden-elektronik.de/raspbee/)

## {% linkable_title Configuration %}

To configure the component, a `zha` section must be present in the `configuration.yaml`,
and the path to the serial device for the radio and path to the database which will persist your network data is required.

```yaml
# Example configuration.yaml entry
zha:
  usb_path: /dev/ttyUSB2
  database_path: /home/homeassistant/.homeassistant/zigbee.db
```

{% configuration %}
radio_type:
  description: One of `ezsp`, `xbee` or `deconz`.
  required: false
  type: string
  default: ezsp
usb_path:
  description: Path to the serial device for the radio.
  required: true
  type: string
baudrate:
  description: Baud rate of the serial device.
  required: false
  type: integer
  default: 57600
database_path:
  description: _Full_ path to the database which will keep persistent network data.
  required: true
  type: string
enable_quirks:
  description: Enable quirks mode for devices where manufacturers didn't follow specs.
  required: false
  type: boolean
  default: true
{% endconfiguration %}

To add new devices to the network proceed to the ZHA section under Confgiuration. Click the Add Devices button up top and you will see a scrolling log of the process on the bottom of the screen. Put your device into pairing mode acorrding to the manufacturer's instructions. The new device should appear at the top of the screen allowing you to rename them and/or assign them to an area. 
<iframe width="692" height="389" src="https://www.youtube.com/embed/ye-tf2RFfLQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe> 

Some battery powered devices will need to be kept alive while pairing, this can sometimes be acheived by periodically pressing the pairing button during the pairing process.

Additionally you may call the `permit` service on the `zha` domain. Do this by clicking the Service icon in Developer tools and typing `zha.permit` in the **Service** dropdown box. Next, follow the device instructions for adding, scanning or factory reset.

In case you want to add Philips Hue bulbs that have previously been added to another bridge, have a look at: [https://github.com/vanviegen/hue-thief/](https://github.com/vanviegen/hue-thief/)
