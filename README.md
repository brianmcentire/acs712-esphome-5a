# ACS712 sensor component for esphome

This is based on https://github.com/RobTillaart/ACS712

The code is working on ESP32 and ACS712-5A.

Example yaml fragment:

```
output:
  - platform: ledc
    pin: GPIO25
    id: test_dac_output
    frequency: 1000 Hz

number:
  - platform: template
    name: "DAC Test Output"
    id: dac_test_number
    min_value: 0
    max_value: 1
    step: 0.01
    set_action:
      then:
        - output.set_level:
            id: test_dac_output
            level: !lambda 'return x;'

external_components:
  - source:
      type: git
      url: https://github.com/brianmcentire/acs712-esphome-5a
    components: [acs712]

sensor:
  - platform: acs712
    pin: 34
    voltage: 3.3
    adc_bits: 4095
    mv_per_amp: 185
    line_voltage: 24
    noisemV: 21
    current_sensor:
      name: "Amps"
      accuracy_decimals: 1
    power_sensor:
      name: "Watts"
      accuracy_decimals: 1
```
Expects ACS712 module Out pin to connect to GPIO pin 34. Additionally connect a 100 uF capacitor between pin 34 and GND for filtering.

The YAML above also includes a DAC output on pin 25. This can be connected to pin 34 for testing/verifying the code works without an ACS712 module connected. Vary the slider in Homeassistant from 0 to .9 to see output values from 0 to 6 Amps. Very low output values like .01 will produce readings of .5 A while .9 will produce 6 A reading.

