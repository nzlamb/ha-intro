# Automate all the things
## with Home Assistant


This workshop is not a complete "do it this way" instruction manual.

It's a series of examples and discussion points, primarily intended to inspire and spark ideas on what is possible.

Do you have any itches to scratch?


## Why Home Assistant?

* Free and open source.
* Huge support community.
* Based on standards.
* Single platform to do the things.
* No Internet connection necessary.
* Expert mode: Hack your own things!


* You have control.
* No subscription fees (now or later).
* No expiry or vendor pulling the plug.
* No data snooping (unless you want it to).
* Community has reverse engineered lots of things already.
* Many parts are cheap!


## Why not?

* Want it to Just Work.
* You don't care about privacy.
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

---

# Connectivity Options


## WiFi

* You probably already have it!
* But...
  * Most complex option.
  * Tends to be power hungry.
  * Can allow access to other things on the network.


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
* Secure.
* Low cost.
* Purpose built.
* Great for battery-powered devices.
* Easy to extend range with repeaters (proxies).
* But... dependent on a single master controller.

---

# Security tips


Some good practice:

* Enable passwords on everything that allows it!
* Put IoT devices on a separate network to your home stuff.
* Separate indoor and outdoor things.

---

# Failure Domains

What happens if:

* Internet disconnects?
* Home Assistant server fails?
* Time sync stops working?
* Network gets broken into?

Sometimes having devices able to operate normally without automation is preferable.

---

# Choosing an HA platform


If you're handy with Linux and have hardware or a NAS, consider Docker containers.


To keep it simple you can use a Raspberry Pi



If you need to buy hardware consider a Home Assistant Green Smart Hub (approx NZ$150). This is a similar price to a Pi 5 (approx. NZ$130) but includes a case, power supply and has an RTC. There's also the Yellow which takes a Pi compute module.


# Install HAOS

If you already have a Raspberry Pi:
https://www.home-assistant.io/installation/raspberrypi/


If you dont have a Raspberry Pi and just want a thing to run HA:
https://www.home-assistant.io/green/


If you have a NAS with Docker, fire up a Home Assistant container.

If you have a Linux box, ask me for a compose file to get you started. :)


## Completing installation

Once it's all started up, visit your new Home Assistant site at http://homeassistant:8123

Follow the guide at https://www.home-assistant.io/getting-started/onboarding/ and set up an intial login.

Often HA will detect devices on your network for you, such as smart TVs, printers, NAS units, etc.

---



---

## ESPhome

* Low-code platform for deploying and managing devices.
* Highly integrated with Home Assistant.
* Huge community support.
* Simple options for flashing images (web serial and over-the-air (OTA)).


Primarily targeted at ESP8266/ESP32 which are widely available as standalone modules. Also supports Beken and Realtek chips often used by Tuya and other big IoT brands.

Most 'smart' WiFi devices I've seen for retail sale in NZ use Beken or Realtek chips.

I strongly recommend ESPhome over Tasmota/OpenBeken for Home Assistant.

Images can take a while to compile depending on the speed of the Home Assistant server.


### General ESPhome process

1. Create a device in ESPhome.
2. Flash it to get initial WiFi comms (may need to use USB serial).
3. Add remaining code and flash OTA.


## Modding tips

If modding an existing device, you may need to isolate the serial lines or even temporarily desolder the module to initially flash it.

Find the pinout for the module online - almost all of them have extensive documentation.


## Modding tips

Don't power mains devices while flashing with serial.

You'll need to trace PCB tracks to find what each pin does.

Search online first though, you'll be surprised how at how often this has already been done for your device!


## Modding tips

Tuya devices often use Tuya serial for communication as opposed to direct GPIO.


## ESPhome things

ESPhome is extensive! Generally you'll create some of:

* binary_sensors
* sensors
* lights
* switches
* buttons

And optionally use actions or lambda functions to glue it together.

If you get it wrong, just update the device OTA.

---

# Zigbee


## Initial setup

* Connect [USB adapter](https://www.aliexpress.com/item/1005009460438197.html).
* In HA **Settings** -> **Add Integration**.
* Select **Zigbee Home Automation**.
* Select the Zigbee adapter.
* Select **Create a network**.


## Add a device

* **Settings** -> **Devices and services** -> **Zigbee Home Automation**.
* Click the Zigbee adapter.
* Click **Add devices via this device**.
* Put the device in pairing mode (RTFM).

---

# BLE


## Bluetooth Proxy

* Refer https://blakadder.com/gl-s10/

* Flash with precompiled image above or copy config from https://raw.githubusercontent.com/blakadder/bluetooth-proxies/refs/heads/main/gl-s10_v2.yaml directly into a new ESPhome device.


## Buttons

Will fetch an example later?


## Flashing a thermometer

* Use Chrome or Edge
* Turn on Bluetooth
* Visit https://pvvx.github.io/ATC_MiThermometer/TelinkMiFlasher.html
* Hit **Connect**

Though you might need a serial cable...

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
- [WorkSafe codes](https://www.worksafe.govt.nz/laws-and-regulations/electrical-and-gas-codes-of-practice/electricity-codes-of-practice/)
- [Guide booklet](https://www.worksafe.govt.nz/dmsdocument/1580-new-zealand-electrical-code-of-practice-for-homeowneroccupiers-electrical-wiring-work-in-domestic-installations-nzecp-51-2004)


## Safety

* Be very, very careful with imported mains equipment!
* If in doubt stick to local or certified suppliers.
* Big box retailers sell certified kit, but it not HA-friendly.


## Shelly

[Shelly](http://shelly.cloud/) makes mains powered WiFi switches:

* Many products are AS/NZS certified.
* Reasonably open source friendly.
* Native support in Home Assistant.
* ESP-based so can be re-flashed.
* Relatively low cost, especially compared to hardware stores!


--

# Displays

Put your things on show with a cheap yellow display!

Examples:
https://github.com/witnessmenow/ESP32-Cheap-Yellow-Display/


---

# Mood Lighting

* Addressable LED strips.
* ESPhome or WLED.

Generic ESP modules usually work fine, but [dedicated LED controllers](https://www.aliexpress.com/item/1005008422983344.html) can be found also that include level shifters. No soldering, just plug and play!


## Addressable strips

* Popular options:
  * WS2812B for RGB
  * SK6812 for RGBW (white channel)
* Most rely on 5v signalling.
  * An ESP can usually drive directly if the wires are kept short.
  * Consider a 'sacrificial pixel' or logic level shifter.


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

# Smoke Alarms

Latest building code requires wireless smoke alarms. Why not go all-in?

Only buy EN 14604/AS 3786:2014 certified devices!

Some brands offer Zigbee compatible alarms.

Aqara claims Home Assistant support. Currently NZ$90 each.


---
