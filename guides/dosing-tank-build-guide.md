# Dosing Tank Water Level Monitor - Build Guide

A complete guide for building an ESP8266-based water level monitoring system for hydroponic dosing tanks with OLED display and Home Assistant integration.

## Overview

This project creates an affordable, WiFi-connected water level monitor for hydroponic dosing tanks using an ultrasonic sensor, OLED display, and ESP8266 microcontroller. The system provides local display readouts and integrates seamlessly with Home Assistant for remote monitoring and automation.

## Hardware Requirements

### Bill of Materials

| Item | Description | Quantity | Price | Link |
|------|-------------|----------|-------|------|
| Ultrasonic Sensor | PWM DC5V Waterproof Distance Sensor | 1 | ~$20 | Amazon |
| D1 Mini | WeMos D1 Mini V4.0.0 Type-C ESP8266 | 1 | ~$2.60 | Amazon (10-pack) |
| OLED Display | 0.96" 128x64 SSD1306 I2C Display | 1 | ~$3 | Amazon ASIN: B07WPCPM5H |
| Jumper Wires | Dupont Wire Kit (M-F, M-M, F-F) | 1 kit | ~$7 | Amazon (reusable) |

**Total Cost:** ~$26 per sensor unit (excluding reusable jumper wires)

### Tools Needed

- Computer with USB port
- Web browser (for ESPHome installation)
- Measuring tape or ruler (for calibration)

## Wiring Diagram

### Ultrasonic Sensor to D1 Mini

```
Ultrasonic Sensor          D1 Mini
─────────────────          ───────
VCC (Red)          ───→    5V
GND (Black)        ───→    GND
TRIG (Yellow)      ───→    D5 (GPIO14)
ECHO (Green)       ───→    D6 (GPIO12)
```

### OLED Display to D1 Mini (I2C)

```
OLED Display              D1 Mini
────────────              ───────
VCC                ───→   3.3V
GND                ───→   GND
SDA                ───→   D3 (GPIO0)
SCL                ───→   D4 (GPIO2)
```

### Complete Wiring Summary

```
D1 Mini Pin        Connected To
───────────        ────────────
5V                 Ultrasonic VCC
3.3V               OLED VCC
GND                Ultrasonic GND + OLED GND (common ground)
D3 (GPIO0)         OLED SDA
D4 (GPIO2)         OLED SCL
D5 (GPIO14)        Ultrasonic TRIG
D6 (GPIO12)        Ultrasonic ECHO
```

## Software Setup

### Prerequisites

1. **ESPHome** - For flashing and managing the ESP8266
2. **Home Assistant** (optional) - For remote monitoring and automation
3. **WiFi Network** - 2.4GHz network with internet access

### Step 1: Install ESPHome

You can install ESPHome using one of these methods:

#### Option A: ESPHome Dashboard (Recommended for beginners)
1. Install ESPHome Dashboard via Home Assistant Add-on, or
2. Install standalone using Docker or Python pip

#### Option B: ESPHome Web (Easiest for first-time flash)
1. Visit [ESPHome Web](https://web.esphome.io/)
2. Connect D1 Mini via USB
3. Flash initial firmware through browser

### Step 2: Create Configuration File

1. Create a new device in ESPHome
2. Copy the configuration from this repository:
   - [dosing-tank-ultrasonic.yaml](https://github.com/byoungster/Hydroponic/blob/main/configs/dosing-tank-ultrasonic.yaml)
3. Create a `secrets.yaml` file with your credentials:

```yaml
# secrets.yaml
api_encryption_key: "your-32-character-encryption-key"
ota_password: "your-ota-password"
wifi_ssid_iot: "Your_WiFi_SSID"
wifi_password_iot: "Your_WiFi_Password"
```

### Step 3: Flash the D1 Mini

1. Connect D1 Mini to computer via USB-C cable
2. In ESPHome, click "Install" on your device
3. Choose "Plug into this computer"
4. Select the COM port for your D1 Mini
5. Wait for compilation and upload (takes 3-5 minutes first time)

### Step 4: Verify Operation

1. Check ESPHome logs to confirm WiFi connection
2. Verify OLED display shows "DOSING TANK" and sensor readings
3. If using Home Assistant, device should auto-discover

## Calibration

### Understanding the Calibration Points

The ultrasonic sensor measures distance from the top of the tank. As water level rises, the distance decreases.

**Default calibration (in config):**
- Empty tank: 13.5 inches = 0 gallons
- Full tank: 5.2 inches = 10 gallons

**For a 27-gallon tank monitoring 0-20 gallons:**
- Physical capacity: 27 gallons
- Monitored range: 0-20 gallons (20 gal = 100%)

### Calibration Steps

1. **Empty Tank Measurement:**
   - Drain tank to empty (or desired minimum)
   - Measure distance from sensor to water surface
   - Record this value (e.g., 13.5 inches)

2. **Full Tank Measurement:**
   - Fill tank to desired "full" level (e.g., 20 gallons)
   - Measure distance from sensor to water surface
   - Record this value (e.g., 5.2 inches)

3. **Update Configuration:**

```yaml
# In dosing-tank-ultrasonic.yaml, find this section:
filters:
  - calibrate_linear:
      - 13.5 -> 0    # Empty: your measured distance -> 0 gallons
      - 5.2 -> 10    # Full: your measured distance -> max gallons
```

4. **Update Tank Capacity:**

If monitoring a different maximum (e.g., 20 gallons instead of 10):

```yaml
# Find water_level_percent sensor
lambda: |-
  float gallons = id(water_level_gallons).state;
  return (gallons / 20.0) * 100.0;  # Change 20.0 to your max gallons
```

5. **Re-flash the Device:**
   - Click "Install" in ESPHome
   - Choose "Wirelessly" (if on same network)
   - Device will update via OTA

### Calibration Tips

- Mount sensor securely before calibrating
- Ensure sensor is pointing straight down
- Account for foam or bubbles on water surface
- Test at multiple levels to verify accuracy
- Recalibrate if you move the sensor

## Home Assistant Integration

### Automatic Discovery

1. Ensure ESPHome device and Home Assistant are on same network
2. Go to **Settings → Devices & Services**
3. ESPHome integration should show "Discovered" notification
4. Click "Configure" and enter the API encryption key

### Available Sensors

- `sensor.dosing_tank_ultrasonic_distance_inches` - Raw distance in inches
- `sensor.dosing_tank_ultrasonic_water_level_gallons` - Water volume in gallons
- `sensor.dosing_tank_ultrasonic_water_level_percent` - Percentage full (0-100%)

### Creating Automations

Example: Low Water Alert

```yaml
automation:
  - alias: "Dosing Tank Low Water Alert"
    trigger:
      - platform: numeric_state
        entity_id: sensor.dosing_tank_ultrasonic_water_level_percent
        below: 20
    action:
      - service: notify.mobile_app
        data:
          message: "Dosing tank is low! Only {{ states('sensor.dosing_tank_ultrasonic_water_level_gallons') }} gallons remaining."
```

## Troubleshooting

### OLED Display Not Working

- **Blank screen:** Check I2C address (should be 0x3C). Try running I2C scan in ESPHome logs
- **Wrong address:** Update `address:` in display configuration
- **No power:** Verify 3.3V connection (NOT 5V - may damage OLED)

### Sensor Reading "Unknown" or "NaN"

- **Check wiring:** Verify TRIG on D5, ECHO on D6
- **Power issue:** Confirm 5V connected to ultrasonic sensor
- **Out of range:** Ensure water level is within sensor's detection range (0.12m to 0.37m)

### WiFi Connection Issues

- **2.4GHz only:** ESP8266 does not support 5GHz WiFi
- **Fast connect:** If connection fails, try removing `fast_connect: True`
- **Signal strength:** Check if router is too far from device

### Erratic Readings

- **Outlier filter:** Adjust lambda filter threshold (currently 20%)
- **Clamp values:** Verify physical range in clamp filter matches your tank
- **Mounting:** Ensure sensor is stable and pointing straight down
- **Interference:** Check for objects between sensor and water surface

## Advanced Configuration

### Adjusting Update Intervals

Change `update_interval: 1s` to update less frequently (e.g., `5s` or `10s`) to reduce processing load.

### Customizing OLED Display

Edit the `display:` section lambda to modify:
- Text positions
- Font sizes
- Additional information (WiFi status, IP address, etc.)

### Multiple Tanks

1. Clone the configuration for each tank
2. Change device name (e.g., `dosing-tank-ultrasonic-2`)
3. Flash to additional D1 Mini boards
4. Each appears as separate device in Home Assistant

## Maintenance

- **Clean sensor probe:** Monthly cleaning prevents buildup affecting accuracy
- **Check connections:** Ensure wires remain secure, especially in humid environments
- **Update firmware:** Periodically update ESPHome version for bug fixes and features
- **Backup configuration:** Keep copy of YAML file and secrets

## Safety Considerations

- **Waterproofing:** Ensure D1 Mini and OLED are protected from water splashes
- **Power:** Use proper 5V power supply rated for continuous operation
- **Mounting:** Secure sensor to prevent falling into tank
- **Electrical safety:** Keep all non-waterproof electronics away from water

## Credits and Resources

- **ESPHome Documentation:** https://esphome.io/
- **Home Assistant:** https://www.home-assistant.io/
- **GitHub Repository:** https://github.com/byoungster/Hydroponic

## License

This project documentation is provided for educational and informational purposes.
