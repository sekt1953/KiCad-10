# Sporbesat

## AI Help

* [AI Help ESPHome filters: - delayed_off](./AI-Help_Input_min_Time.md)
  * [ESPHome Dokumentation](./AI-Help_Input_min_Time.md#esphome-dokumentation)
  * [ESPHome how to set min time for input to be low before accepted](./AI-Help_Input_min_Time.md#esphome-how-to-set-min-time-for-input-to-be-low-before-accepted)
  * [can delayed_off bee set in milisec](./AI-Help_Input_min_Time.md#can-delayed_off-bee-set-in-milisec)

## SporBesat a la´ Moppe

* [moppe.dk/besat.html](https://moppe.dk/besat.html) her et forslag til sporbesat telbage melding, jeg har brugt det som inspiration og her er min version:
* ![Moppe.svg](./Moppe/Images/Skærmbillede%20fra%202026-09-17%2009-59-23.png)
* Lidt om diagrammet
  * D1 & D2 skal begrænse spændingen over Basis Emiter på Q1
    * Dioderne skal kunne børe en kortslutnings strøm, når der kommer kortslutning på skinderne, her 5A.
  * R2 på 10R skal begrænse strømen i Q1's Basis til under 5mA
  * U1 ved at bruge en Optokobler her, opnår vi adskillese mellem DCC`s 12VAC og MCU`s 3,3VDC.
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

og 

```yaml
  filters:
    - delayed_on: 3 bølgelængder af DCC
```

Se mere her [AI-Help_Input_min_Time.md](./AI-Help_Input_min_Time.md)