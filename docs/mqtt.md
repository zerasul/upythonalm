# MQTT

MQTT (Message Queuing Telemetry Transport) es un protocolo de mensajería ligero diseñado para dispositivos con recursos limitados y redes de baja ancho de banda. Es ampliamente utilizado en aplicaciones de Internet de las Cosas (IoT) debido a su eficiencia y simplicidad.

## Conceptos Básicos de MQTT

- **Broker**: Es el servidor que recibe todos los mensajes, los filtra y los envía a los clientes suscritos. Ejemplos populares de brokers MQTT incluyen Mosquitto y HiveMQ.
- **Cliente**: Es cualquier dispositivo o aplicación que se conecta al broker para enviar o recibir mensajes.
- **Tema (Topic)**: Es una cadena jerárquica que se utiliza para organizar y filtrar mensajes. Los clientes pueden publicar mensajes en un tema o suscribirse a un tema para recibir mensajes.
- **Publicar (Publish)**: Es el acto de enviar un mensaje a un tema específico.
- **Suscribirse (Subscribe)**: Es el acto de registrarse para recibir mensajes de un tema específico.

## Uso de MQTT en MicroPython

Para utilizar MQTT en MicroPython, se puede utilizar la biblioteca `umqtt.simple`, que proporciona una implementación básica del cliente MQTT. A continuación, se muestra un ejemplo de cómo conectar un dispositivo MicroPython a un broker MQTT, publicar mensajes y suscribirse a un tema.

Para instalar la biblioteca `umqtt.simple`, asegurate de tener conectividad a Internet y sigue las instrucciones de instalación específicas para tu entorno MicroPython.

```python
import upip
upip.install('micropython-umqtt.simple')
```

### Ejemplo de Código MQTT

```python
import network
from umqtt.simple import MQTTClient
import time

# configuracion broker MQTT
MQTT_BROKER = "test.mosquitto.org"
MQTT_CLIENT_ID = "micropython_client"
MQTT_TOPIC = b"test/topic"
# conectar MQTT
def connect_mqtt():
    client = MQTTClient(MQTT_CLIENT_ID, MQTT_BROKER)
    client.connect()
    return client
# publicar mensaje
def publish_message(client, message):
    client.publish(MQTT_TOPIC, message)
# main
def main():
    client = connect_mqtt()
    while True:
        publish_message(client, b"Hello from MicroPython!")
        time.sleep(5)

if __name__ == "__main__":
    main()
```

Para poder ver los mensajes publicados, puedes utilizar un cliente MQTT como MQTT.fx o cualquier otra herramienta que te permita suscribirte al tema `test/topic` en el broker `test.mosquitto.org`.

Este código conecta un dispositivo MicroPython a un broker MQTT público, publica un mensaje cada 5 segundos en el tema especificado y puede ser modificado para adaptarse a tus necesidades específicas.

#### Observar mensajes

Podemos suscribirnos a los mensajes enviados utilizando la web gratutia: 

https://testclient-cloud.mqtt.cool/

En esta página, podemos conectarnos al broker `test.mosquitto.org` y suscribirnos al tema `test/topic` para ver los mensajes que nuestro dispositivo MicroPython está publicando.

### Suscribirse a un Tema

Para suscribirse a un tema y recibir mensajes, podemos modificar el ejemplo anterior para incluir una función de callback que maneje los mensajes entrantes.

```python
def message_callback(topic, msg):
    print("Mensaje recibido en el tema {}: {}".format(topic, msg))
client.set_callback(message_callback)
client.subscribe(MQTT_TOPIC)
```