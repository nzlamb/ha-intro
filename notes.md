# Intro

This workshop is not a complete "do it this way" instruction manual.

It's a series of guides and discussion points, primarily intended to inspire and spark ideas on what is possible.

Do you have any itches to scratch?

---

## Why Home Assistant?

* Free and open source.
* Huge support community.
* Based on standards.
* Single platform to do the things.
* Much of the hard work is already done.
* Expert mode: Hack your own things!


* You have control.
* No subscription fees (now or later).
* No expiry or vendor pulling the plug.
* No data snooping (unless you want it to).
* Community has reverse engineered lots of things already.
* Many parts are cheap!


## Why not HA?

* Want it to Just Work™.
* Don't care about privacy.
* You have deep pockets.
* HA can sometimes be complex:
  * Retail components (e.g. light switches) in NZ usually need hacking.
  * Equipment vendors often don't make it easy.

---

# Planning

## You'll definitely need

* Computer
* WiFi network that doesn't totally suck
* Money
* Time


## You'll *probably* need

* USB to serial adapter (3.3v)
* Soldering gear
* Multimeter
* Jumper leads and probes
* House
* Understanding partner


## Connectivity

* WiFi
* Bluetooth Low Energy (BLE)
* Zigbee


## WiFi

* You probably already have it!
* But...
  * Most complex option.
  * Tends to be power hungry.


## Bluetooth Low Energy

* Low cost.
* Easy to set up.
* Can use for device-to-device comms.
* Simple presence detection.
* Geat for battery-powered devices.
* Easy to expand - simply add more proxies.
* But... many devices use proprietary messaging.


## Zigbee

* Standards-based.
* Low cost.
* Purpose built.
* Geat for battery-powered devices.
* Easy to extend range with repeaters (proxies).
* But... dependent on a single master controller.


## Security tips

* Enable passwords on everything that allows it!
* Put IoT devices on a separate network to your home stuff.
* Separate inside and outside things.

---

# Install HAOS

If you already have a Raspberry Pi: https://www.home-assistant.io/installation/raspberrypi/

If you dont have a Raspberry Pi and just want a thing to run HA: https://www.home-assistant.io/green/

If you have a NAS with Docker, fire up a Home Assistant container.

If you already have a Linux server then here's a Docker compose file to get you started:


# Choosing an HA platform

If you're handy with Linux and have hardware or a NAS, consider Docker containers.

To keep it simple you can use a Raspberry Pi but note the lack of real time clock might result in unexpected behaviour if rebooted with no time source is available.

If you need to buy hardware consider a Home Assistant Green Smart Hub (approx NZ$150). This is a similar price to a Pi 5 (approx. NZ$130) but includes a case, power supply and has an RTC. There's also the Yellow which takes a Pi compute module.


## Completing installation

Once it's all started up, visit your new Home Assistant site at http://homeassistant:8123

Follow the guide at https://www.home-assistant.io/getting-started/onboarding/ and set up an intial login.

Often HA will detect devices on your network for you, such as smart TVs, printers, NAS units, etc.

---

# Basics


## Locations

# ESPhome

1. Head to **Settings** -> **Add-ons** -> **Add-on Store**
2. Search for **ESPhome Device Builder** and hit **Install**.
3. Once installed, make sure it's set to auto start on boot and hit **Start**.
4. Optionally add it to the side-bar to easy access.

If you run your own Linux box, here's another Docker compose file to get you started:


# Adding a device

This is crazy easy!

Building the image will take a few minutes depending on the speed of the server running Home Assistant. The first run takes much longer as it will need to compile all the things.

1. Create a new ESPhome device.
2. Build it.
3. Disconnect the ESP module from the circuit.
4. Plug in USB connector.
5. Use Chrome (or Edge) to flash the image via webserial. Visit https://web.esphome.io/
6. Once done the device should show as online in ESPhome. From here you can update it OTA.


## 1-Wire Temp Sensor (DS18B20)

Connect:

1. Ground
2. GPIO 4 or 5 (ESP8266)
3. 3.3v

Attach a 4k7 resistor between pins 2-3.

Enable 1-wire config to ESP home and install it to search for the sensor address:

```yaml
one_wire:
  - platform: gpio
    pin: GPIO4
```

Review log output:

```
[01:02:49][C][gpio.one_wire:022]: GPIO 1-wire bus:
[01:02:49][C][gpio.one_wire:023]:   Pin: GPIO4
[01:02:49][C][gpio.one_wire:084]:   Found devices:
[01:02:49][C][gpio.one_wire:086]:     0xbc000003b72eda28 (DS18B20)
```

Use the address of the device to configure it as a temperature sensor:

```yaml
sensor:
  - platform: dallas_temp
    address: 0xbc000003b72eda28
    name: "Hall Temperature"
    update_interval: 30s
```

If all went well we should now see it detected in the device logs:

```
[01:18:46][C][gpio.one_wire:022]: GPIO 1-wire bus:
[01:18:46][C][gpio.one_wire:023]:   Pin: GPIO4
[01:18:46][C][gpio.one_wire:084]:   Found devices:
[01:18:46][C][gpio.one_wire:086]:     0xbc000003b72eda28 (DS18B20)
[01:18:46][C][dallas.temp.sensor:029]: Dallas Temperature Sensor:
[01:18:46][C][dallas.temp.sensor:034]:   Address: 0xbc000003b72eda28 (DS18B20)
[01:18:46][C][dallas.temp.sensor:035]:   Resolution: 12 bits
[01:18:46][C][dallas.temp.sensor:036]:   Update Interval: 30.0s
```


# Shelly switch config

1. Double-check the wiring!
2. Power up the Shelly.
3. Connect to the WiFi network called **Shelly*Device-nnnnnn***
4. Visit http://192.168.33.1/ in a browser.
5. Visit WiFI settings and locate your WiFi network. Enter the password.
6. (Recommended) disable Open Network for WiFi 1 and 2.
7. Save the config.
8. Return to your main WiFI network and go to Home Assistant.
9. Navigate to **Settings** -> **Devices and Services**.
10. If all went well the Shelly should appear - click **Add**.
11. Give it a name and location and finish.

At this point the deice page should appear and you should be able to toggle the switches.



# Scenarios to provide

* Temp/humidity
  * Wall thermometer
  * DIY with ESP
* Heat pump remote control
  ESP with IR LED.
* Garage door opener/sensor
  ESP board with relay and wide DC input voltage.
  e.g. https://www.aliexpress.com/item/1005007383267856.html or https://www.aliexpress.com/item/1005005898666081.html or https://www.aliexpress.com/item/1005005301505486.html
  Make sure it's DC, if it has a transformer it's probably AC input.
  Also checkout out ratgdo https://paulwieland.github.io/ratgdo/
* Light control with Shelly
* PIR control
* Soil moisture
* Sprinkler control (relay boards and valves)
* Timers for fans, towel rails, underfloor heating, etc
* Mood lighting with ESPhome or WLED and addressable LED strips
* Scenes and automations

---

# Choosing an ESP module

ESP8266 is the original and now deprecated. Still widely supported and fine to use but has a number of limitations. Fine to buy when integrated into other products or boards, but if you have the choice buy ESP32 series.


## ESP8266/NodeMCU

For the most part only GPIO 4 and 5 (D1 and D2) are reliable for general use.

GPIO 12, 13, and 14 (D5, D6, and D7) are usually fine but pull low during boot.

https://randomnerdtutorials.com/esp8266-pinout-reference-gpios/


# Heat Pumps

Many heat pumps have an internal serial port which allows for bi-directional communication.

Refer https://github.com/asund/esphome-daikin-s21 for example.

Failing that you can use simple IR comms instead to emulate the remote control:

https://esphome.io/components/climate/climate_ir.html


Example (for ESP32-C3 mini):

```yaml
remote_transmitter:
  pin: GPIO1
  carrier_duty_percent: 50%

climate:
  - platform: daikin
    name: "Dining Room A/C"
```

Connect an IR LED via a 100 Ohm resistor to the GPIO pin and ground.

Go test it!

The range will be much shorter than the regular remote, but that's OK!

If you want the existing remote to send status to HA that's also fine:

```yaml
remote_receiver:
  id: rcvr
  pin:
    number: GPIO1
#    inverted: true
#    mode:
#      input: true
#      pullup: true
  tolerance: 55%

remote_transmitter:
  pin: GPIO3
  carrier_duty_percent: 50%

climate:
  - platform: daikin
    name: "Dining Room A/C"
    receiver_id: rcvr
```

Connect an IR receiver module with pins:

1. Signal to ESP GPIO pin
2. Ground
3. 3.3V

Put the receiver and LED as close to the heat pump receiver as possible.

Alternative inverted config:

```yaml
remote_transmitter:
  pin:
    number: GPIO3
    inverted: true
    mode:
      input: true
      pullup: true
  carrier_duty_percent: 50%
```


# Energy Monitoring

Two methods:

1. Buy something that exists.
2. DIY

DIY means:

* Playing with mains.
* Calibration.

Existing kit can be expensive though (Shelly).

Recommendation: Buy Shelly and get a sparky to install it for you. Can put current clamps on phase wires in your meter box.

Could also set up fake sensors based on guesstimations.


# Garage Door

Most garage door openers have a power output and door trigger input. Some have additional I/O features (e.g. light).

For most cases Uue a simple relay board like the HW-622. This has a relay to control the door (connect in parallel with the garage door buttoon) and can draw power (7-26V) from the opener unit. Most opener units I've checked output 12-16V. Connect a door open sensor between the ext input and 5v.

Tech specs and STL files for case: https://ayatec.eu/introducing-relay-module-hw-622/

Get GPIO pinout from https://templates.blakadder.com/HW-622.html

At 24V with the relay on the power inductor is the hottest component on the board, sitting at about 60°C.
At 8V the relay transistor is the hottest component at 50°C.

The code below will add a trigger button and door state sensor to HA.

```yaml
# Config for HW-622 board

# Makes the LED on the ESP module show status (optional)
status_led:
  pin:
    number: GPIO2

# switch defines the relay output and ensures it is off by default. Leaving the name unset hides this control
# When turned on it will turn off again after a 1s delay to emulate a momentary action. Most garage door
# openers expect the button to be pressed for about half a second or so to trigger the door.
switch:
  - platform: gpio
    pin: GPIO4
    restore_mode: ALWAYS_OFF
    id: relay1
    on_turn_on:
      - delay: 1s
      - switch.turn_off: relay1

# button template makes the relay emulate a 1-second button press
button:
  - platform: template
    name: "Garage Door Control"
    on_press:
      - switch.turn_on: relay1

# sensor input for door state switch
binary_sensor:
  - platform: gpio
    pin:
      number: GPIO5
      mode:
        input: true
        pullup: true
    name: "Garage Door State"
    device_class: garage_door
```


# Sprinklers


# Smoke Alarms

Latest building code requires wireless smoke alarms. Why not go all-in?

Only buy EN 14604/AS 3786:2014 certified devices!

Some brands offer Zigbee compatible alarms.

Aqara claims Home Assistant support. Currently NZ$90 each.


# Failure Domains

What happens if:

* Internet disconnects?
* Home Assistant server fails?
* Time sync stops working?
* Network gets broken into?

Sometimes having devices able to operate normally without automation is preferable.


# Mood Lighting

* Addressable LED strips.
* ESPhome or WLED.

Generic ESP modules usually work fine, but dedicated LED controllers can be found also that include level shifters. No soldering, just plug and play!

e.g. https://www.aliexpress.com/item/1005008422983344.html or https://www.aliexpress.com/item/1005009315012504.html


## WLED

1. Connect your ESP.
2. Visit https://install.wled.me/
3. Install it.
4. Enter WiFi details.
5. Add device to Home Assistant.

Refer: https://lastminuteengineers.com/esp32-wled-tutorial/


### Recommended config for WLED

Visit your WLED device in a browser.

1. GPIO
2. Time and location

Then go forth and make presets!

---

# BLE

## Bluetooth Proxy

Refer https://blakadder.com/gl-s10/

Flash with precompiled image above or copy config from https://raw.githubusercontent.com/blakadder/bluetooth-proxies/refs/heads/main/gl-s10_v2.yaml directly into a new ESPhome device.


## Flashing a thermometer

Use Chrome or Edge
Turn on Bluetooth
Visit https://pvvx.github.io/ATC_MiThermometer/TelinkMiFlasher.html
Hit **Connect**

---

# Zigbee

COnnect USB adapter.

In HA **Settings** -> **Add Integration**.

Select **Zigbee Home Automation**.

Select the Zigbee adapter.

Select **Create a network**.

Done!

To add a device, go to **Settings** -> **Devices and services** -> **Zigbee Home Automation**.

Click the Zigbee adapter.

Click **Add devices via this device**.

Put the device in pairing mode.

e.g. smart button hold for 10s.


# BLE vs Zigbee vs WiFi

All use 2.4GHz spectrum for IoT devices.

WiFi - you already have it. Setup and security is harder. Convenient for permanently powered devices.

Zigbee is repeatable but needs a central controller. If that is down then Zigbee comms cease.

BLE can have multiple receivers/proxies and comms can function even if HA is down.
BLE works outof the box if your Home Assistant server has Bluetooth (e.g. Raspberry Pi)

BLE and Zigbee are one-touch setup.

BLE - multiple comms standards - lots of proprietary devices. Zigbee is somewhat more standard.

---

# Mains


## Wiring

In New Zealand, an owner/occupier can perform a limited range of household electrical maintenance themselves,
without qualified inspection. Work must be performed to the current electrical code.

Replacing or relocating:
  * Sockets
  * Switches
  * Permanently connected appliances
  * Light fittings


You can **not**:
* Install, alter or extend circuits.
* Perform work in a live switchboard.

Refer:
- https://www.worksafe.govt.nz/laws-and-regulations/electrical-and-gas-codes-of-practice/electricity-codes-of-practice/
- https://www.worksafe.govt.nz/dmsdocument/1580-new-zealand-electrical-code-of-practice-for-homeowneroccupiers-electrical-wiring-work-in-domestic-installations-nzecp-51-2004


## Safety

* Be very, very careful with imported mains equipment!
* Most Shelly devices have AS/NZS certification.
* Big box retailers sell certified kit, but it not HA-friendly.


# Sprinklers

Use AC valves! They draw less power and generate less heat - much more efficient and durable.


# Presence

Easy methods:

* Home network integration e.g. UniFi, Mikrotik, etc
* BLE tracking: https://esphome.io/components/binary_sensor/ble_presence.html


# Addressable LED strips

* Use use WLED or ESPhome
* Most rely on 5v signalling.
  * An ESP can usually drive directly if the wires are kept short.
  * Consider a 'sacrificial pixel' or logic level shifter.
* Popular options:
  * WS2812B for RGB
  * SK6812 for RGBW

---

# Displays

Put your things on show with a cheap yellow display!

Examples:
https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display/







Be careful when importing mains-powered devices! Don't even buy from smaller bargain stores.

Most aren't certified and safety can be questionable.

Just because it works doesn't make it safe.

You can replace mains fittings in your own home like-for-like, as long as you follow the standards.

Shelly makes mains powered WiFi switches:

* Many products are AS/NZS certified.
* Reasonably open source friendly.
* Native support in Home Assistant.
* ESP-based so can be re-flashed.
* Relatively low cost, especially compared to hardware stores!
