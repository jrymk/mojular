# mojular
Modular ESPHome configuration templates

## Base config

```yaml
esp32:
  board: esp32-s3-devkitc-1
  framework:
    type: arduino
    version: latest

i2c:
  - id: i2c0
    sda: GPIO5
    scl: GPIO6
  - id: i2c1
    sda: GPIO15
    scl: GPIO16

uart:
  - id: mmwave_uart
    tx_pin: GPIO17
    rx_pin: GPIO18
    baud_rate: 256000
    parity: NONE
    stop_bits: 1
  - id: co2_uart
    tx_pin: GPIO47
    rx_pin: GPIO48
    baud_rate: 9600

packages:
  base: !include
    file: mojular/base.yaml
    vars:
      name: "mojular_bedroom"
      friendly_name: "Mojular-Bedroom"
  # add extra devices here  

# Enable Home Assistant API
api:
  encryption:
    key: "**********"

ota:
  password: "**********"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "${name}"

captive_portal:
```


## Supported devices

#### SHT3X-D Temperature+Humidity Sensor
[ESPHome](https://esphome.io/components/sensor/sht3xd.html)

```yaml
  sht31: !include
    file: mojular/sht3x.yaml
    vars:
      id: "sht31"
      name: "SHT31"
      i2c_id: i2c0
      address: 0x44
      update_interval: 4s
```

#### SenseAir CO_2 Sensor
[ESPHome](https://esphome.io/components/sensor/senseair.html)

```yaml
  senseair_s8: !include
    file: mojular/senseair_s8.yaml
    vars:
      id: "senseair_s8"
      name: "Senseair S8"
      uart_id: co2_uart
      update_interval: 4s
```

#### LD2410 Sensor
[ESPHome](https://esphome.io/components/sensor/ld2410.html)

```yaml
  mmwave: !include
    file: mojular/ld2410.yaml
    vars:
      id: "mmwave"
      name: "mmWave"
      uart_id: "mmwave_uart"
```
