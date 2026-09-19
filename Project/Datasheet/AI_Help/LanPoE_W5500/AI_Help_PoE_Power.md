# AI Help on PoE Power

## Is a PoE switch power output dependency on lan connection ?

* A PoE (Power over Ethernet) switch's power output is independent of the LAN data connection, meaning data transmission does not affect power delivery, though both share the same physical cable. [1] (https://www.hpe.com/emea_europe/en/what-is/poe-switch.html), [2] (https://www.fs.com/blog/6-things-you-should-know-about-poe-switch-faq-127.html)

### How PoE and LAN Coexist

* ***Separate paths:*** Ethernet cables contain distinct wire pairs for data and electrical current. They use common-mode voltage so electricity and data signals do not interfere with each other. [1] (https://www.geeksforgeeks.org/computer-networks/what-is-power-over-ethernet-poe/), [2] (https://www.fs.com/blog/6-things-you-should-know-about-poe-switch-faq-127.html)
* ***Independent operation:*** A port can function as a standard data LAN port without sending power, or supply power simultaneously while passing high-speed network traffic. [1] (https://www.come-star.com/blog/lan-port-vs-poe-port/), [2] (https://www.hpe.com/emea_europe/en/what-is/poe-switch.html)
* ***No bandwidth penalty:*** Drawing maximum electrical wattage (such as 30W or 90W) does not reduce network speed or packet throughput. [1] (https://network-switch.com/blogs/switches/do-all-ethernet-switches-need-power), [2] (https://www.fs.com/blog/6-things-you-should-know-about-poe-switch-faq-127.html)

### Factors That Actually Affect PoE Power Output

* ***Switch power budget:*** The total electrical wattage the switch's internal power supply can distribute to all ports combined.
* ***PoE standard:*** The classification limits per port, such as IEEE 802.3af (15.4W), 802.3at (30W), or 802.3bt (up to 90W at the source). [1] (https://www.avixa.org/explore/articles/power-over-ethernet-switch), [2] (https://www.omadanetworks.com/us/blog/2595/poe-vs-poe+-vs-poe++-power-over-ethernet-standards/), [3] (https://www.fs.com/blog/6-things-you-should-know-about-poe-switch-faq-127.html)
* ***Cable length and quality:*** Resistance over long Ethernet runs (approaching 100 meters) causes minor power loss as heat, reducing the usable wattage reaching the end device. [1] (https://www.purple.ai/en-gb/guides/power-over-ethernet-poe-for-access-points-an-implementation-guide)
* ***Device negotiation:*** Active PoE switches perform a handshake to detect and supply only the precise power requested by the connected device. [1] (https://www.ewindnet.com/what-is-poe-and-poe-switch-working-principle-power-supply-standard-and-application-scenario-of-industrial-poe-switch/), [2] (https://things-embedded.com/us/white-paper/a-guide-to-industrial-power-over-ethernet/)
* If you are planning a setup, tell me:
  * What devices you want to power (cameras, access points, etc.)
  * The model or total power budget of your switch

## Can I draw power without datalink ?

* Yes, you can draw power from a PoE port without establishing an active LAN data link. However, whether it works automatically depends entirely on whether your switch uses Active PoE or Passive PoE.
* Here is how the two types handle power without a data connection:

### 1. Active PoE (Standard: 802.3af/at/bt)

* Most modern commercial switches use Active PoE. These switches require a hardware handshake, but they do not require a data link.
  * ***How it works:*** When you plug a device in, the switch sends a tiny, low-voltage electrical signal to detect a specific resistance (a 25k-ohm resistor) inside the powered device.
  * ***The result:*** If the switch detects this resistance, it turns on the full power immediately. The data link lights do not need to flash, and no network traffic needs to pass.
  * ***Limitation:*** You cannot simply strip an Ethernet cable and wire it to a raw battery, motor, or non-PoE device. Without that internal detection circuit, the switch will keep the power turned off for safety.

### 2. Passive PoE (Always On)

* Some budget switches, older equipment (like older Ubiquiti gear), and standalone PoE injectors use Passive PoE.
  * ***How it works:*** These devices do not perform any handshake or detection. They blindly send raw voltage (usually 24V or 48V) down the power wires of the Ethernet cable the moment they are turned on.
  * ***The result:*** You can draw power instantly without a data link, and you can even use simple splitter cables to power non-networked devices.
  * ***Warning:*** Because it is "always on," plugging a non-PoE device (like a standard laptop) into a passive PoE port can instantly fry the device's network card.
