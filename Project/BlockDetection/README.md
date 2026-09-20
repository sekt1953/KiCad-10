# Sporbesat

## AI Help

* [AI Help ESPHome filters: - delayed_off](./AI-Help_Input_min_Time.md)
  * [ESPHome Dokumentation](./AI-Help_Input_min_Time.md#esphome-dokumentation)
  * [ESPHome how to set min time for input to be low before accepted](./AI-Help_Input_min_Time.md#esphome-how-to-set-min-time-for-input-to-be-low-before-accepted)
  * [can delayed_off bee set in milisec](./AI-Help_Input_min_Time.md#can-delayed_off-bee-set-in-milisec)

## SporBesat a la´ Moppe

* [moppe.dk/besat.html](https://moppe.dk/besat.html) her et forslag til sporbesat tilbage melding, jeg har brugt det som inspiration og her er min version:
* ![Moppe.svg](./Moppe/Images/Moppe.svg)
* For a ting skal virke best muligt, er det en god ide at placerer dette modul tæt på sporisulstionen som muligt, og så forbinde kabel mellem U1 udgange til MCUen input.
* Lidt om diagrammet
  * D1 & D2 skal begrænse spændingen over Basis Emiter på Q1
    * Dioderne skal kunne børe en kortslutnings strøm, når der kommer kortslutning på skinderne, her 5A.
  * R2 på 10R skal begrænse strømen i Q1's Basis til under 5mA
  * U1 ved at bruge en Optokobler her, opnår vi adskillese mellem DCCs 12VAC og MCUs 3,3VDC.
    * U1's Collector forbindes til indgang på MCU eller MCU Interface
    * U1's Emiter forbindes til MCU GND
* MCU's Binary Sensor:
  * For at undgå falske sporbesat & sporfrit meldinger,  
  skal have forsinkelse for sporbesat på 1/4 bølgelængde af DCC,  
  og en forsinkelse for sporfrit på 3 bølgelængder af DCC.
  * Dette opnås med :
  * YAML

```yaml
  filters:
    - delayed_off: 1/4 bølgelængde af DCC
```

og for spor frit:

```yaml
  filters:
    - delayed_on: 3 bølgelængder af DCC
```

Se mere her [AI-Help_Input_min_Time.md](./AI-Help_Input_min_Time.md)

## KiCad files

* Project files:
  * [Moppe.kicad_pro](./Moppe/Moppe.kicad_pro)
* Schematic files:
  * [Moppe.kicad_sch](./Moppe/Moppe.kicad_sch)

## Modbus med RJ45 Cat5e Kabel

### 1. The Two Main Pinout Standards

* Because RS-485 is a differential signal, **the A (+) and B (-) signals must always be on the exact same twisted pair** to ensure proper noise cancellation.

#### **Standard A: The Modbus Organization Standard (TIA-856)**

* This is the official recommended standard by the Modbus Organization for serial Modbus over an RJ45 connector.

|RJ45 Pin|T568B Wire Color|RS-485 Function|Notes|
|:---:|:---|:---|:---|
|Pin 4|🔵 Blue|D1 / B / +|Non-inverting data signal (Twisted Pair 1)|
|Pin 5|🔵⚪ Blue/White|D0 / A / -|Inverting data signal (Twisted Pair 1)|
|Pin 8|🟤 Brown|Common / GND|Reference ground line (Crucial for isolation)|

#### Standard B: Alternative Industrial Pinout (e.g., Delta VFDs / Security Systems)

* Many automated hardware manufacturers swap the pairs to utilize the outermost positions or alternate twisted pairs.

RJ45 PinT568B Wire ColorRS-485 FunctionNotes

|RJ45 Pin|T568B Wire Color|RS-485 Function|Notes|
|:---:|:---|:---|:---|
|Pin 1|🟠⚪ Orange/White|D1 / B / +|Non-inverting data signal (Twisted Pair 2)|
|Pin 2|🟠 Orange|D0 / A / -|Inverting data signal (Twisted Pair 2)|
|Pin 7|🟤⚪ Brown/White|Common / GND|Reference ground line

![connector.png](./Moppe/Images/rj45-pass-through-connector.png)

## 2. Critical Wiring Rules

* **Never Plug RS-485 into an Ethernet Port:** While the connector is physically identical, injecting RS-485 voltages (up to 12V) into a standard 2.5V network switch or computer NIC will instantly burn out the magnetic transformers.
* **Always Connect the Ground (GND):** A common misconception is that RS-485 only requires two wires. Without a common ground reference between transceivers, standard-mode voltage spikes can cause data corruption or blow out communication chips.
* **Daisy-Chaining:** Standard Ethernet splitters cannot be used to break out the signal because they create "stubs" (T-junctions) that reflect signals. You must use a true daisy-chain block where the bus enters the adapter and immediately continues out to the next node
* **Termination:** Just like standard terminal-block RS-485, if your RJ45 cable run is long or operating at high speeds (above 9600 baud), you must bridge a 120-ohm resistor across the data pair (Pins 4 and 5, or 1 and 2) at the final physical device on the line.