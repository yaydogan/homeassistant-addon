# Smart Meter Monitor Home Assistant Addon
Home Assistant addon to receive AMR readings from utility meters using RTL2832U based DVB-T USB tuners via MQTT. 

This addon is mainly built on jdeath's repository (https://github.com/jdeath/RTLAMR2MQQT). I customized based on my needs:
   - Configuration has templates for power, gas, and water meter
   - Docker image is pre-build and pushed to Docker hub for faster installation
   - Implemented multistage docker build to reduce image size
   - Powering the radio until reading from all meters (my meters transmit very often so no concerns for CPU hug)

## Todo List

   - Currently it will read only "scm" meters. Will add support for scanning other protocols. 
   - Automatically create sensors upon startup based on meters configured
   
## Usage

 1. Install the addon:
    - adding this respository to the Add-on Store

 2. Use addon configuration to configure:
    - mqtt_host (defaulted to mosquitto address, can be modified with IP address)
    - mqtt_port
    - mqtt_user
    - mqtt_password
    - Device IDs 

 3. Configure the sensors in /config/configuration.yaml
```
sensor:
  - platform: mqtt
    state_topic: "readings/4411782/meter_reading"
    name: "Power Meter"
    unit_of_measurement: kWh

  - platform: mqtt
    state_topic: "readings/11032620/meter_reading"
    name: "Gas Meter"
    unit_of_measurement: M3
```
 
 4. Start the addon
