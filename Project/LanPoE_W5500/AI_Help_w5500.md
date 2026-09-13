# AI Help W5500 Ethernet

## ESPHome how do ethernet get mac address?

* You can get the Ethernet MAC address in ESPHome by using the [Ethernet Info Text Sensor](https://esphome.io/components/text_sensor/ethernet_info/) component in your YAML configuration. 
* Configuration Example
  * Add the ethernet_info platform to your configuration to expose the MAC address as a text sensor: 

```yaml
text_sensor:
  - platform: ethernet_info
    mac_address:
      name: "Ethernet MAC Address"
```

* Alternative: Setting a Custom MAC Address
  * If you need to assign or override a fixed MAC address on your Ethernet interface instead of just reading it, use the mac_address option directly under your [Ethernet Component](https://esphome.io/components/ethernet/) configuration:

```yaml
ethernet:
  type: W5500
  # ... other pins ...
  mac_address: "DC:DA:0C:16:A7:01"
```

*
  * If you're trying to view a MAC address that isn't showing up or need help with a specific Ethernet chip like the LAN8720 or W5500, let me know your board model and hardware setup.

## Do i have to enable wifi to get esp32 mac address?

* No, you do not have to enable Wi-Fi to get an ESP32's MAC address.
  * Every ESP32 chip comes with a factory-burned, unique base MAC address stored in its eFuse memory. This address is hardcoded into the hardware and is available even if Wi-Fi is completely disabled or if you are using an Ethernet connection.

## Here is how ESPHome handles it and how you can find it:

1. How ESPHome Uses It
When you configure an Ethernet connection in ESPHome, the ESP32 automatically pulls this hardware base MAC address and assigns it to your Ethernet interface. You do not need to include a wifi: block in your YAML configuration.
2. How to Find Your MAC Address Without Wi-Fi
  * Check the ESPHome Logs: When you flash your ESP32 with an Ethernet-only configuration, the very first lines of the boot log will display the hardware MAC address. Look for a line like this:

```code
[I][app:029]: Running on ESP32; MAC Address: AA:BB:CC:DD:EE:FF
```

  * Check your Router/DHCP Server: Once the ESP32 is plugged into your network via Ethernet, it will request an IP address. You can log into your network router's admin panel and look at the connected client list to see its Ethernet MAC address.
  * If you are having trouble getting the device to connect or show up on your network, let me know your Ethernet board type (e.g., WT32-ETH01, Olympus ESP32 Gateway) so I can help check your configuration pins.

## How do i connect w5500 to esp32-s3

* To connect a W5500 Ethernet module to an ESP32-S3, you must use a SPI connection. Unlike the standard ESP32, the ESP32-S3 does not have an internal Ethernet MAC, so it relies entirely on the W5500's controller over SPI.

1. Wiring Diagram (Pin Mapping)
The ESP32-S3 has flexible GPIOs, but it is highly recommended to use the default FSPI (SPI2) pins for the best performance and stability.

|W5500 Pin|ESP32-S3 Pin|Description|
|:---|:---|:---|
|VCC|3V3|Power (Use a stable 3.3V source)|
|GND|GND|Ground|
|MISO|GPIO 13|SPI Master In Slave Out|
|MOSI|GPIO 11|SPI Master Out Slave In|
|SCLK|GPIO 12|SPI Clock|
|SCS / CS|GPIO 10|Chip Select (Can be changed)|
|RST / RESET|GPIO 9|Hardware Reset (Can be changed)|
|INT|Optional|Interrupt (Not strictly required by ESPHome)|

2.ESPHome YAML Configuration
  * In your ESPHome configuration file, you need to explicitly define the SPI bus first, and then map those pins to the Ethernet component.
  * Add the following blocks to your YAML file:

```yaml

# 1. Define the SPI Bus using the ESP32-S3 FSPI pins
spi:
  clk_pin: GPIO12
  mosi_pin: GPIO11
  miso_pin: GPIO13

# 2. Configure the W5500 Ethernet component
ethernet:
  type: W5500
  clk_pin: GPIO12   # Matches SPI clock
  mosi_pin: GPIO11  # Matches SPI mosi
  miso_pin: GPIO13  # Matches SPI miso
  cs_pin: GPIO10    # Connected to W5500 SCS/CS
  reset_pin: GPIO09 # Connected to W5500 RST
```

⚠️ Critical Troubleshooting Tips for ESP32-S3 & W5500

1. Power Supply: The W5500 chip is power-hungry, especially when transmitting data. If your ESP32-S3 is powered only via a flimsy USB cable from a computer, the W5500 might brown out, fail to initialize, or constantly drop packets. Use a reliable 3.3V regulator or a dedicated power supply.
2. Short Cables: Keep your SPI jumper wires as short as possible (ideally under 10–15 cm). SPI signals operate at high frequencies and are prone to cross-talk and noise on long, loose wires.
3. Reset Pin: Do not skip the reset_pin in your YAML configuration. ESPHome needs physical control over the reset pin to power-cycle and properly initialize the W5500 on boot.
   * Are you using a pre-made development board with an integrated W5500 (like a LilyGO product), or are you wiring a standalone blue W5500 module to your ESP32-S3 using jumper cables?

## How do i connect ip101 to esp32-s3

* You cannot connect an IP101 Ethernet chip to an ESP32-S3.
  * The ESP32-S3 does not have an internal Ethernet MAC controller, meaning it lacks the hardware required to communicate via RMII (Reduced Media-Independent Interface). The IP101 is a transceiver (PHY) that requires an internal RMII MAC to function. [1, 2]
  * According to the official [ESPHome Ethernet Component documentation](https://esphome.io/components/ethernet/), RMII PHY chips like the IP101, LAN8720, and RTL8201 are strictly "ESP32 only". They will not work with the S2, S3, or C3 variants. [1, 2]
  * Your Alternatives for ESP32-S3 Ethernet
  * If you must use the ESP32-S3 and need wired Ethernet, you have to use a controller that features its own MAC and communicates with the ESP32-S3 via SPI:
    1. W5500 (Highly Recommended): Fully supported by ESPHome and widely available as an external module.
    2. DM9051 / ENC28J60: Also supported via SPI, though the W5500 offers much better performance and reliability. [1]

## What to do next?

* If you already bought the IP101 chip/board, you will need to swap out your microcontroller for an original ESP32 (like the ESP32-WROOM-32E) to use it.
* If you must use the ESP32-S3, you should purchase a W5500 module instead. [1, 2]

Do you already have an original ESP32 development board lying around that you could swap in for this project?
