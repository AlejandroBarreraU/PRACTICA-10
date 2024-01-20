# PRACTICA LED CON NODE-RED 
## Introducción
En este repositorio aprenderemos a hacer la conexión NODE-RED con la ESP32 para mandar señales de 0 y 1 a un LED.
## Materiales a utilizar
+ WOKWI
+ NODE-RED
## Intrucciones
### En WOKWI
1. Escribimos el siguiente código:
```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "18.193.219.109";
const int mqttPort = 1883;
const char* mqttUser = "alejandrob";
const char* mqttPassword = "1234";
const char* mqttTopic = "alejandro123";

WiFiClient espClient;
PubSubClient client(espClient);

int ledPin = 13; // Pin del LED

void setup() {
  pinMode(ledPin, OUTPUT);
  Serial.begin(115200);
  setup_wifi();
  client.setServer(mqttServer, mqttPort);
  client.setCallback(callback);
}

void loop() {
  if (!client.connected()) {
    reconnect();
  }
  client.loop();
}

void setup_wifi() {
  delay(10);
  Serial.println();
  Serial.print("Conectando a: ");
  Serial.println(ssid);
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("Conectado a la red WiFi");
}

void reconnect() {
  while (!client.connected()) {
    Serial.print("Intentando conexión MQTT...");
    if (client.connect("ESP32Client", mqttUser, mqttPassword)) {
      Serial.println("Conectado");
      client.subscribe(mqttTopic);
    } else {
      Serial.print("Error de conexión, rc=");
      Serial.print(client.state());
      Serial.println(" Intentando de nuevo en 5 segundos");
      delay(5000);
    }
  }
}

void callback(char* topic, byte* payload, unsigned int length) {
  Serial.print("Mensaje recibido: [");
  Serial.print(topic);
  Serial.print("] ");
  for (int i = 0; i < length; i++) {
    Serial.print((char)payload[i]);
  }
  Serial.println();

  if (strcmp(topic, mqttTopic) == 0) {
    if ((char)payload[0] == '1') {
      digitalWrite(ledPin, HIGH);
    } else {
      digitalWrite(ledPin, LOW);
    }
  }
}
```
2. Agregamos la sig. libreria.
+ PubSubClient
   ### En NODE-RED
  1. Agregamos Switch, y lo configuramos
     On Payload     1
     Off Payload    0
  2. Agregamos mqtt y configuramos de la siguiente forma:
     Server: 18.193.219.109
     Topic: alejandro123

## Resultados
![]()
![]()
## Créditos
Elaborado por el Ing. Alejandro Barrera
