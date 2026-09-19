# AI Help Wifi_On_Off

## AI-oversigt

* You can enable and disable the ESPHome Wi-Fi interface dynamically using the built-in wifi.enable and wifi.disable actions triggered by a Modbus register or switch.

### Example Configuration

* Add a template switch or monitor a Modbus register/coil that executes wifi.enable or wifi.disable upon state changes:

```yaml
yaml

wifi:
  ssid: "MyHomeNetwork"
  password: "VerySafePassword"

# Optional: prevent reboots if Wi-Fi is intentionally kept off
api:
  reboot_timeout: 0s

# Example using a Modbus register/coil to control state
modbus_controller:
  - id: my_modbus_device
    # ... your modbus configuration ...

# Create a template switch or tie actions to a Modbus sensor/trigger
switch:
  - platform: template
    name: "WiFi Control via Modbus"
    id: wifi_control_switch
    turn_on_action:
      - wifi.enable
    turn_off_action:
      - wifi.disable
```

* If you are reading the state directly from a Modbus register via a modbus_controller sensor or binary sensor, use on_state automation triggers:

```yaml
yaml

binary_sensor:
  - platform: modbus_controller
    modbus_controller_id: my_modbus_device
    name: "Modbus WiFi Toggle Request"
    register_type: coil
    address: 0x0001
    on_state:
      then:
        - if:
            condition:
              lambda: 'return x;' # If Modbus coil is ON
            then:
              - wifi.enable
            else:
              - wifi.disable
```

### Warning: If you leave the Wi-Fi disabled for longer than your configured API or MQTT reboot_timeout, ESPHome's internal watchdog will assume the network stack is broken and reboot the device. Set reboot_timeout: 0s on your api: or mqtt: components if you intend to keep Wi-Fi disabled indefinitely.

* If you can share your current Modbus register configuration or let me know whether you are using a holding register or a coil, I can tailor the exact trigger for your setup.