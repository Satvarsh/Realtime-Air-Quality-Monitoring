# Realtime-Air-Quality-Monitoring

Made this project for my university.

- Consists of TWO sets of projects accounting forboth Arduino and Raspberry Pi. <br>
- Made an Application toolkit which is highly modular and can be integrated into existing IOT Applications seamlessly(UsesMosqsuitto MQTT Broker) <br>
- Includes anall digital temperature sensorintegration along with data to account for 5 Hazardous gases + Humidity <br>
- One of the very few projects that useEMU8086 as an IOT application <br>

## Contents

- Report <br>
- Raspberry Pi Code <br>
- Arduino Code <br>

See Report for: <br>

- Initial Setup + Setup Code <br>
- EMU8086 Code <br>
- Thermometer module code (Open Source Code NOT mine) <br>

## Working: <br>

### Audrino <br>

1. Using PubSubClient to determine Publisher, Subscribers <br>
2. Get the port value of your server (1883 by default).<br>
3. Create a user, password in "Mosquitto.config" file<br>
4. Import the basic libraries like ESPWifi, MQTT<br>
```
#Import <ESP826.Wifi.h>
#Import<PubSubClient.h>
```
5. Define the constants needed for the MQTT and Wi-Fi configurations<br>
6. Import MQ135 sensor's library<br>
```
#include <dht.h>
#define dht_apin D5
dht DHT;
WiFiClient espClient;
PubSubClient client(espClient);
long lastMsg = 0;
char msg[50];
int value = 0;
```
7. Define functions to set up Wi-Fi, MQTT, and callback method for MQTT Message <br>
8. Use the functions in set up to configure settings and connect to server<br>

void setup(){
pinMode(BUILTIN_LED, OUTPUT);
Serial.begin(9600);
Setup_wifi();
Client.setServer(mqtt_server, 17389);
Client.setCallback(callback);
reconnect();
}

### Raspberry Pi <br>

1. Testing/Establishing Broker <br>

```
mosquitto_pub -t "testing_phrase" -m "Testing Word"
mosquitto_pub -t "testing_phrase"
```

2. Set SWITCH as the publish client <br>
3. Set LIGHT as subscriber client <br>
4. Connect to the broker <br>
5. Establish a frequency <br>
6. Connect to port "1883" in mosquito <br>
7. Get outputs from "ny-power.org." <br>

Setup Code: <br>
```
#set GPIO pin numbering method to BCM
import RPi.GPIO as GPIO
GPIO.setmode(GPIO.BCM)
#define pins
#define MCP3008_PIN_CS 8
#define MQ135_3V3_PIN_AOUT 0
```
Note : <br>
Topics/Phrases are arranged in a directory like structure. A topic might be "MQ135", or
"MQ135/C02" if you have multiple clients within that parent topic. The subscriber client
will listen for incoming messages from the subscribed topic and react to what was
published to that topic, such as "on" or "off." Clients can subscribe to one topic and
publish it to another as well. If the client subscribes to "MQ135/NOx", it might also want
to publish to another topic like "MQ135/NH3/Result" so that other clients can monitor
the state of that pollutant
