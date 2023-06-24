# Practica 9 ESP32 con NODE-RED ENCENDER LED

En esta práctica podemos programar una ESP32 con un ultrasonido para obtener la distancia y un sensor de temperatura y humedad, y asi poder visualizar los datos en tiempo real.


## Contenido 

- Introducción 
- Objetivo
- Material Requerido
- Procedimiento 
- Instrucciones de operación 
- Resultados 



## 1. Introducción

La ```Esp32``` la utilizamos en un entorno de adquision de datos, lo cual en esta practica ocuparemos un ultrasonido (```HC-SR04 Ultrasonic distance sensor```) un sensor (```DTH22```).
### 1.1 Descripción
  
 
 ### 1.2 Especificación 
 Es importante considerar que esta practica se estara realiando en un simulador llamado [WOKWI](https://https://wokwi.com/).

## 2. Objetivo 

Poder diseñar un diagrama con HC-SR04 Ultrasonic distance sesor, y con sensor DHR22, mediante la utilización de una ESP32 y poder 


## 3. Material Requerido

Tomar en cuenta el material necesario para realizar esta práctica:

- [WOKWI](https://https://wokwi.com/)
- Tarjeta ESP 32
- Sensor de ultrasonido de distancia 
  (HC-SR04 Ultrasonic distance sensor)
- Programa Node - Red 


## 4. Procedimiento a realizar 

 - Requisitos previos

Para poder usar este repositorio necesitas entrar a la plataforma [WOKWI](https://https://wokwi.com/).


## Paso 1 

1. Abrir la terminal de programación y colocar la siguente programación:

```
#include <WiFi.h>
#include <PubSubClient.h>

const char* ssid = "Wokwi-GUEST";
const char* password = "";
const char* mqttServer = "52.29.234.128";
const int mqttPort = 1883;
const char* mqttUser = "dulcem";
const char* mqttPassword = "1241";
const char* mqttTopic = "dulcemrz";

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

## Paso 2 

2. Instalar las librerias 
   **ArduinoJson** y
   **PubSubClient**

Como se muestra en la siguente imagen.

![](https://github.com/DulceMRZ/PRACTICA7_ESP32_ULTRASONIC/blob/main/LIBRERIAS.PNG?raw=true)

## Paso 3

3. Hacer la conexion de **HC-SR04 Ultrasonic distance sensor** con la **ESP32** como se muestra en la siguentes imagenes:

3.1 Es importante considerar que para **ESP32** se maneja un ```Voltaje de trabajo 3.3 VDC```. 

a) Observar conexión de  **ESP32**. 

![](

b) Corrobora que el simulador compile bien el programa 

![]()


### 5. Conexión Diagrama en Node - Red

En Node Red, nos estaremos apoyando para poder ver los datos en tiempo real, al momento de estar simularlos. 

###### Nota: es importante ya contantar con Node Red previamente descargado, y la IP, con la que estaras trabajando. 

![]()

Al inciar tu diagrama de bloques debes ajustar información importante en cada Topico: 


a) Para el **Bloque - mqtt in** es importante Seleccionar la opción de "dulce/tokai2" y la IP que se tiene en el programa (44.195.202.69), en tu caso anexaras tus datos. 

![]()


b) Para el **Bloque - JSON** es importante Seleccionar la opción de "Always convert to JavaScript Object"

![]()









### 6. Instrucciónes de operación

1. Iniciar simulador.
2. Visualizar los datos en el monitor serial.
3. Colocar la temperatura y humedad dando *doble click* al sensor **DHT11** 

## 7. Resultados

Cuando haya funcionado, verás los valores dentro del monitor serial como se muestra en la siguente imagen.

![]()


# Créditos

Desarrollado por Dulce M Rodriguez Zarate 

- [GitHub](https://github.com/DulceMRZ)