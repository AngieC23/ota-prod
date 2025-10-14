# ESP32 OTA Update via HTTPS and MQTT with TLS

Este proyecto implementa actualizaciones OTA (Over-The-Air) para dispositivos ESP32 utilizando HTTPS y notificaciones MQTT con TLS. El dispositivo se conecta a un broker MQTT seguro y escucha mensajes de actualización. Cuando recibe una notificación de actualización, descarga e instala el nuevo firmware mediante HTTPS.

## Características

- **Actualización OTA**: Actualización de firmware a través de HTTPS.
- **Notificación MQTT**: Recepción de notificaciones de actualización a través de MQTT.
- **Seguridad TLS**: Conexiones seguras tanto para MQTT como para HTTPS.
- **Reconexión automática**: Reconexión automática a MQTT en caso de pérdida de conexión.
- **Publicación de estado**: Publicación periódica del estado del dispositivo.

## Requisitos

- **ESP32**: Placa compatible con ESP32.
- **Broker MQTT**: Broker MQTT con soporte para TLS (por ejemplo, Mosquitto).
- **Servidor HTTPS**: Servidor web con soporte para HTTPS para alojar el firmware.
- **Certificado TLS**: Certificado TLS válido para el servidor (por ejemplo, Let's Encrypt).

## Configuración

### Configuración WiFi

```cpp
const char* ssid = "TU_SSID";
const char* password = "TU_PASSWORD";
```

### Configuración MQTT

```cpp
const char* mqtt_broker = "uceva-iot-core.freeddns.org";  // Broker MQTT con TLS
const int mqtt_port = 8883;                                // Puerto para MQTT con TLS
const char* mqtt_topic = "dispositivo/device1/ota";
const char* mqtt_client_id = "ESP32Client";
const char* mqtt_username = "device1";  // Opcional: usuario MQTT
const char* mqtt_password = "a1b2c3d4"; // Opcional: contraseña MQTT
```

### URL para OTA

```cpp
const char* ota_url = "https://uceva-iot-core.freeddns.org/firmware.bin";
```

### Certificado TLS

```cpp
const char* root_ca = R"EOF(
-----BEGIN CERTIFICATE-----
... tu certificado aquí ...
-----END CERTIFICATE-----
)EOF";
```

## Estructura del proyecto

- **src/main.cpp**: Código principal del proyecto.
- **platformio.ini**: Configuración de PlatformIO.
- **partitions_ota.csv**: Configuración de particiones para OTA.

## Compilación y carga

### Usando PlatformIO

1. Instala PlatformIO en tu IDE (por ejemplo, VSCode).
2. Clona este repositorio.
3. Abre el proyecto en PlatformIO.
4. Compila y carga el firmware en tu ESP32.

### Usando Arduino IDE

1. Instala las bibliotecas necesarias:
   - PubSubClient
   - ArduinoJson
   - esp32-https-ota
2. Abre el archivo `src/main.cpp` en Arduino IDE.
3. Compila y carga el firmware en tu ESP32.

## Flujo de trabajo

1. El dispositivo se conecta a WiFi y al broker MQTT.
2. El dispositivo se suscribe al tópico MQTT configurado.
3. El dispositivo publica su estado inicial.
4. El dispositivo publica su estado cada 5 minutos.
5. Cuando el dispositivo recibe un mensaje MQTT con una nueva versión, descarga e instala el nuevo firmware.

## Formato del mensaje MQTT

El mensaje MQTT debe tener el siguiente formato JSON:

```json
{
  "version": "1.1.0",
  "url": "https://uceva-iot-core.freeddns.org/firmware_v1.1.0.bin"
}
```

## Licencia

Este proyecto está licenciado bajo la Licencia MIT - ver el archivo LICENSE para más detalles.

## Referencias

- [PubSubClient](https://github.com/knolleary/PubSubClient)
- [ArduinoJson](https://github.com/bblanchon/ArduinoJson)
- [esp32-https-ota](https://github.com/espressif/esp-idf/tree/master/examples/system/ota)
- [iot-mqtt-tls](https://github.com/alvaro-salazar/iot-mqtt-tls.git) 