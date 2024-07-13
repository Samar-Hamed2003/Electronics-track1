# Electronics-track1
#### Task: Design and program an electronic circuit that contains an ESP along with any sensor or electronic piece, along with an explanation of the circuit’s function.

##### Using the wokwi application, I ran the circuit and the code:
###### 1-ESP32 microcontroller.
###### 2-DHT22 temperature and humidity sensor
###### 3-Breadboard and jumper wires Circuit function:
###### The ESP microcontroller will be responsible for connecting to the Wi-Fi network and retrieving weather data from an online source. It will also collect data from the temperature and humidity sensor.

#### code:

```
#include <WiFi.h>
#include "DHTesp.h"
#include "ThingSpeak.h"

const int DHT_PIN = 15;
const int LED_PIN = 13;
const char* WIFI_NAME = "Wokwi-GUEST";
const char* WIFI_PASSWORD = "";
const int myChannelNumber =2226105 ;
const char* myApiKey = "5ZZNFKTB60NEJIV9";
const char* server = "api.thingspeak.com";

DHTesp dhtSensor;
WiFiClient client;

void setup() {
  Serial.begin(115200);
  dhtSensor.setup(DHT_PIN, DHTesp::DHT22);
  pinMode(LED_PIN, OUTPUT);
  WiFi.begin(WIFI_NAME, WIFI_PASSWORD);
  while (WiFi.status() != WL_CONNECTED){
    delay(1000);
    Serial.println("Wifi not connected");
  }
  Serial.println("Wifi connected !");
  Serial.println("Local IP: " + String(WiFi.localIP()));
  WiFi.mode(WIFI_STA);
  ThingSpeak.begin(client);
}

void loop() {
  TempAndHumidity  data = dhtSensor.getTempAndHumidity();
  ThingSpeak.setField(1,data.temperature);
  ThingSpeak.setField(2,data.humidity);
  if (data.temperature > 35  data.temperature < 12  data.humidity > 70 || data.humidity < 40) {
    digitalWrite(LED_PIN, HIGH);
  }else{
    digitalWrite(LED_PIN, LOW);
  }
  
  int x = ThingSpeak.writeFields(myChannelNumber,myApiKey);
  
  Serial.println("Temp: " + String(data.temperature, 2) + "°C");
  Serial.println("Humidity: " + String(data.humidity, 1) + "%");
  
  if(x == 200){
    Serial.println("Data pushed successfull");
  }else{
    Serial.println("Push error" + String(x));
  }
  Serial.println("---");

  delay(10000);
}
```
##### This code is an Arduino sketch that uses the DHT22 sensor to measure temperature and humidity.
###### 1. Include Libraries: The code includes the necessary libraries for WiFi connectivity, DHT22 sensor, and ThingSpeak.

###### 2. Define Constants: The code defines the following constants:
   ###### - DHT_PIN: The pin connected to the DHT22 sensor.
   ###### - LED_PIN: The pin connected to an LED.
   ###### - WIFI_NAME and WIFI_PASSWORD: The name and password of the WiFi network.
   ###### - myChannelNumber and myApiKey: The channel number and API key for the ThingSpeak account.
   ###### - server: The ThingSpeak server address.

###### 3. Setup Function:
   ###### - Initializes the Serial communication.
   ###### - Sets up the DHT22 sensor.
   ###### - Configures the LED pin as an output.
   ###### - Connects to the WiFi network.
   ###### - Initializes the ThingSpeak client.

###### 4. Loop Function:
   ###### - Retrieves the temperature and humidity data from the DHT22 sensor.
   ###### - Sends the temperature and humidity data to the ThingSpeak channel using the ThingSpeak.setField() function.
   ###### - Checks if the temperature is outside the range of 12°C to 35°C or the humidity is outside the range of 40% to 70%. If so, it turns the LED on, otherwise, it turns the LED off.
   ###### - Writes the data to the ThingSpeak channel using the ThingSpeak.writeFields() function.
   ###### - Prints the temperature and humidity data to the Serial monitor.
   ###### - Waits for 10 seconds before repeating the loop.

###### The code demonstrates the following functionality:

###### 1. Temperature and Humidity Monitoring: The DHT22 sensor is used to measure the temperature and humidity, which are then displayed in the Serial monitor.

###### 2. Temperature and Humidity Thresholds: The code checks if the temperature and humidity values are outside the specified ranges and controls the LED accordingly.

###### 3. Data Transmission to ThingSpeak: The measured temperature and humidity data are sent to the ThingSpeak IoT platform using the ThingSpeak library.
