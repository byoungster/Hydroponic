# YouTube Video Guide - Dosing Tank Water Level Monitor

## Video Title Suggestions

1. "Build a $26 WiFi Water Level Monitor for Hydroponics with OLED Display!"
2. "ESP8266 Hydroponic Tank Monitor - ESPHome + Home Assistant Tutorial"
3. "DIY Smart Water Level Sensor for Hydroponic Dosing Tanks - Full Build Guide"

## Video Description Template

```
In this video, I show you how to build an affordable WiFi-connected water level monitor for hydroponic dosing tanks using an ESP8266 (D1 Mini), ultrasonic sensor, and OLED display. Total cost: only $26!

⏱️ TIMESTAMPS
0:00 - Introduction & Overview
1:00 - Parts & Cost Breakdown
2:30 - Wiring Diagram
4:00 - ESPHome Setup
7:00 - Flashing the D1 Mini
10:00 - Calibration Process
13:00 - Home Assistant Integration
16:00 - Demo & Final Thoughts

🛒 PARTS LIST (Amazon links)
• Ultrasonic Sensor (~$20): [Your affiliate link]
• D1 Mini ESP8266 10-pack (~$26): [Your affiliate link]
• OLED Display 6-pack (~$18): https://amazon.com/dp/B07WPCPM5H
• Jumper Wire Kit (~$7): [Your affiliate link]

📁 GITHUB REPOSITORY
All code, wiring diagrams, and documentation:
https://github.com/byoungster/Hydroponic

📝 DIRECT LINKS
• Configuration File: https://github.com/byoungster/Hydroponic/blob/main/configs/dosing-tank-ultrasonic.yaml
• Build Guide: https://github.com/byoungster/Hydroponic/blob/main/guides/dosing-tank-build-guide.md

🔧 FEATURES
✅ Real-time water level monitoring
✅ Local OLED display (gallons + percentage)
✅ WiFi connectivity
✅ Home Assistant integration
✅ Low water alerts & automation
✅ OTA updates (no need to unplug!)
✅ Costs only $26 per sensor

💡 WHAT YOU'LL LEARN
• How to wire an ultrasonic sensor to ESP8266
• ESPHome configuration and flashing
• I2C OLED display setup
• Sensor calibration for your specific tank
• Home Assistant automation examples

🎯 PERFECT FOR
• Hydroponic growers
• Aquarium hobbyists
• Water tank monitoring
• Smart home enthusiasts
• DIY automation projects

#hydroponics #esp8266 #esphome #homeassistant #diy #smarthome #arduino #iot #watermonitoring #aquaponics

📧 Questions? Drop them in the comments!
👍 Don't forget to like and subscribe for more DIY smart home and hydroponic projects!
```

## Video Outline & Script

### INTRO (0:00 - 1:00)

**[Show completed project working]**

"Hey everyone! Today I'm going to show you how to build this WiFi-connected water level monitor for your hydroponic dosing tank. It costs only $26, shows real-time water levels on this OLED display, and integrates seamlessly with Home Assistant for remote monitoring and automation.

By the end of this video, you'll have a fully functional smart sensor that can alert you when your tank is running low, track usage over time, and even trigger automated refills. Let's get started!"

**[Show title card]**

### PARTS & COST BREAKDOWN (1:00 - 2:30)

**[Show all parts laid out on table]**

"Here's everything you need:

1. **Ultrasonic Waterproof Sensor** - This is the heart of the system. It's a waterproof ultrasonic distance sensor that costs about $20 on Amazon.

2. **D1 Mini ESP8266** - This is a tiny WiFi-enabled microcontroller. I bought a 10-pack for $26, so about $2.60 each. You'll only need one, but having extras is great for future projects.

3. **OLED Display** - This 0.96-inch I2C OLED display shows the water level locally. It comes in a 6-pack for around $18 on Amazon, so about $3 per display.

4. **Jumper Wires** - A basic dupont wire kit for about $7. These are reusable across all your projects.

**Total cost: $26 per sensor** (not counting the reusable jumper wires)

All the purchase links are in the description below, and the complete bill of materials is on my GitHub."

### WIRING DIAGRAM (2:30 - 4:00)

**[Show wiring diagram on screen + physical connections]**

"The wiring is straightforward. We're connecting two devices to the D1 Mini:

**First, the ultrasonic sensor:**
- VCC (red wire) goes to 5V
- GND (black) goes to ground
- TRIG (yellow) goes to pin D5
- ECHO (green) goes to pin D6

**Second, the OLED display uses I2C:**
- VCC goes to 3.3V - important! Not 5V, or you might damage the display
- GND goes to ground
- SDA goes to pin D3
- SCL goes to pin D4

I'll flash the wiring diagram on screen right now, and it's also available in the GitHub repository.

[Show close-ups of each connection being made]

Take your time with the wiring. Double-check each connection before powering on."

### ESPHOME SETUP (4:00 - 7:00)

**[Screen recording of ESPHome setup]**

"Now let's talk software. We're using ESPHome, which makes programming ESP devices incredibly easy - no coding required, just configuration files.

**If you have Home Assistant:**
1. Go to Settings → Add-ons
2. Install the ESPHome add-on
3. Open the ESPHome dashboard

**If you don't have Home Assistant:**
You can use ESPHome Web at web.esphome.io for your initial flash.

**Configuration File:**
Instead of creating the config from scratch, I've done the hard work for you. Go to my GitHub repository - link in the description - and grab the dosing-tank-ultrasonic.yaml file.

This configuration includes:
- WiFi setup
- Ultrasonic sensor configuration
- OLED display code with custom graphics
- All the sensors for Home Assistant
- Smart filtering to reduce sensor noise

**You'll need to create a secrets.yaml file** with four items:
1. Your WiFi SSID
2. Your WiFi password
3. An API encryption key (ESPHome can generate this)
4. An OTA password for wireless updates

[Show example secrets.yaml on screen]

Once you've got your configuration and secrets file ready, we can flash the device."

### FLASHING THE D1 MINI (7:00 - 10:00)

**[Screen recording of flashing process]**

"Connect your D1 Mini to your computer with a USB-C cable.

In the ESPHome dashboard:
1. Click 'New Device' or '+ New Configuration'
2. Give it a name - I'm calling mine 'dosing-tank-ultrasonic'
3. Paste in the configuration we downloaded
4. Click the three dots → Install → Plug into this computer
5. Select your COM port
6. Click Install

The first flash takes about 3-5 minutes. ESPHome is compiling the code and uploading it to the ESP8266.

[Show compilation progress]

Once it's done, you'll see the logs streaming. Look for:
- WiFi connected message
- IP address assigned
- Sensor readings starting to appear

[Show log output]

Great! The device is online. Now unplug it from your computer and power it with a standard USB power adapter."

### CALIBRATION PROCESS (10:00 - 13:00)

**[Show physical calibration process]**

"Out of the box, the sensor won't be accurate for YOUR specific tank. We need to calibrate it.

The ultrasonic sensor measures the distance from the sensor to the water surface. The closer the water, the shorter the distance.

**Here's how to calibrate:**

**Step 1: Mount your sensor**
Position it at the top of your tank, pointing straight down at the water. Make sure it's secure.

**Step 2: Empty tank measurement**
With your tank empty (or at the minimum level you want to track), use a measuring tape to measure the distance from the sensor to the water surface.

[Show measurement]

I measured 13.5 inches. This will be our '0 gallons' point.

**Step 3: Full tank measurement**
Fill your tank to your desired 'full' level. For my 27-gallon tank, I'm considering 20 gallons as '100% full'.

Measure the distance again.

[Show measurement]

I got 5.2 inches. This will be our '20 gallons' point.

**Step 4: Update the configuration**

In your YAML file, find the calibrate_linear section:

```yaml
filters:
  - calibrate_linear:
      - 13.5 -> 0    # Your empty measurement
      - 5.2 -> 20    # Your full measurement
```

Replace 13.5 with your empty reading, and 5.2 with your full reading. Change the 20 to match your maximum gallons.

**Also update the percentage calculation:**

```yaml
lambda: |-
  float gallons = id(water_level_gallons).state;
  return (gallons / 20.0) * 100.0;  # Change 20.0 to your max gallons
```

**Step 5: Re-flash wirelessly**

Back in ESPHome, click Install again, but this time choose 'Wirelessly'. The device will update over WiFi - no need to unplug it from your tank!

[Show OTA update process]

After the update, check the OLED display. It should now show accurate gallon readings!"

### HOME ASSISTANT INTEGRATION (13:00 - 16:00)

**[Screen recording of Home Assistant]**

"If you're using Home Assistant, integration is automatic.

Go to Settings → Devices & Services. You should see 'ESPHome' with a discovered device notification.

[Show discovery notification]

Click Configure, enter your API encryption key from the secrets file, and submit.

Your device now shows up with three sensors:
1. Distance in inches (raw sensor reading)
2. Water level in gallons
3. Water level percentage

[Show sensor entities]

**Let's create a dashboard card:**

[Show creating a gauge card with water level percentage]

**And a useful automation - low water alert:**

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

Now whenever my tank drops below 20%, I get a notification on my phone.

[Show notification demo]

You can also track water usage over time using Home Assistant's history graphs, create alerts for rapid water level drops (indicating a leak), or even trigger automated refill pumps!"

### DEMO & FINAL THOUGHTS (16:00 - end)

**[Show system working in real environment]**

"Let's see it in action!

[Demo pouring water into tank]

As I add water, watch the OLED display update in real-time. The gallon reading increases, the percentage goes up, and the visual bar fills.

[Show Home Assistant updating simultaneously]

And over in Home Assistant, the sensors update instantly.

**Why I love this project:**

1. **Affordable** - $26 for a complete WiFi sensor is hard to beat
2. **Local display** - You can check water levels without opening your phone
3. **Reliable** - The smart filtering eliminates false readings from sensor noise
4. **Expandable** - Build multiple units for different tanks using the same code
5. **OTA updates** - Make changes without physically accessing the device

**Possible improvements you could make:**

- Add a temperature sensor to track nutrient solution temperature
- Include a flow sensor to measure actual usage
- Connect a relay to trigger automated refills
- Add RGB LEDs for visual alerts

**Everything you need is on GitHub:**
- Complete configuration file
- Detailed build guide
- Wiring diagrams
- Troubleshooting tips

Link is in the description.

If you build this project, I'd love to see it! Share photos in the comments or tag me on social media.

Questions? Drop them below and I'll do my best to help.

If you found this helpful, please give it a thumbs up and subscribe for more DIY hydroponic and smart home projects.

Thanks for watching, and happy growing!"

**[End screen with subscribe button and related videos]**

## B-Roll Suggestions

- Close-up shots of each component
- Wiring connections being made
- Soldering (if applicable)
- ESPHome dashboard interface
- Log output scrolling
- OLED display updating
- Water being added to tank
- Home Assistant dashboard
- Mobile notification arriving
- Finished sensor mounted on tank
- Multiple camera angles of working system

## Graphics to Create

1. **Wiring Diagram** - Clean, colorful schematic
2. **Pin Mapping Table** - Visual reference for connections
3. **Parts List Overlay** - With prices and links
4. **Calibration Formula** - Visual explanation
5. **Code Snippets** - Well-formatted YAML examples
6. **Lower Third Graphics** - For your name/channel
7. **Thumbnail** - Eye-catching with "$26" prominently displayed

## SEO Keywords

Primary: ESP8266 water level sensor, hydroponic tank monitor, ESPHome tutorial
Secondary: D1 Mini project, Home Assistant automation, DIY aquarium sensor, ultrasonic water sensor, OLED display project

## Social Media Posts

### YouTube Community Post
"🌱 New video dropping soon! Building a $26 WiFi water level monitor for hydroponic tanks. Features:
✅ OLED display
✅ Home Assistant integration
✅ OTA updates
Want early access? Let me know! 👇"

### Twitter/X
"Built a $26 WiFi water level sensor for my hydroponic setup using ESP8266 + ESPHome. Full tutorial coming to YouTube! #hydroponics #esp8266 #diy"

### Reddit (r/hydro, r/homeassistant, r/esp8266)
Title: "Built a $26 WiFi water level monitor with OLED display for my dosing tank"
Body: [Link to video] "Full tutorial showing how to build this using ESP8266, ultrasonic sensor, and ESPHome. Integrates with Home Assistant for alerts and automation. All code and documentation on GitHub."

## Engagement Ideas

- Pin a comment asking viewers what sensors they'd like to see next
- Create a poll about preferred display types (OLED vs. LCD vs. E-ink)
- Offer to review submitted builds in a follow-up video
- Host a Q&A in the comments for first 24 hours

## Video File Naming

`hydroponic-dosing-tank-water-level-monitor-esp8266-esphome-tutorial.mp4`

## Closed Captions

Make sure to add accurate closed captions for:
- Better accessibility
- Improved SEO
- International audience (auto-translation)
- Watch time retention

Good luck with your video! 🎬
