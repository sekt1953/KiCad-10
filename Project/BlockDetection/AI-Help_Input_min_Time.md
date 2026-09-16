# AI Help ESPHome     filters: - delayed_off

## ESPHome Dokumentation

* [Binary Sensor Filters](https://esphome.io/components/binary_sensor/#binary-sensor-filters)

## ESPHome how to set min time for input to be low before accepted

* In ESPHome kan du opsætte en minimumstid, som et input skal være low (lavt/slukket), før det bliver godkendt og udløser en handling, ved at bruge et delayed_off filter under din binary_sensor.
* Dette sikrer, at kortvarige dyk eller støj ikke registreres, og at signalet skal være stabilt lavt i en given periode, før ESPHome reagerer på det.

### Eksempel på kode

* Her er, hvordan du integrerer det i din YAML-konfiguration:

```yaml
binary_sensor:
  - platform: gpio
    pin: GPIO21
    name: "Min Sensor"
    filters:
      - delayed_off: 5s
    on_release:
      then:
        - logger.log: "Input har været LOW i 5 sekunder og er nu godkendt!"
```

### Sådan fungerer det

* ***delayed_off:*** Holder sensorens tilstand som ON (high), indtil det fysiske input har været uafbrudt LOW i den tid, du har defineret (f.eks. 5 sekunder).
* Hvis inputtet bliver HIGH igen inden de 5 sekunder er gået, nulstilles timeren, og ændringen ignoreres.
* Først når timeren udløber, skifter sensoren status til OFF i ESPHome, og handlinger under f.eks. on_release bliver kørt.

## can delayed_off bee set in milisec

* Yes, you can set delayed_off in milliseconds.
  * In ESPHome, you just need to use the ms suffix (for example, 500ms).
  * Here is how the configuration looks:

```yaml
binary_sensor:
  - platform: gpio
    pin: GPIO21
    name: "Min Sensor"
    filters:
      - delayed_off: 500ms
```

### Alternative formatting

* If you prefer not to use the suffix, you can also write it out using the time format key:

```yaml
    filters:
      - delayed_off:
          milliseconds: 500
```

* Both methods do exactly the same thing and will filter out any bouncing or noise shorter than your specified millisecond threshold.
