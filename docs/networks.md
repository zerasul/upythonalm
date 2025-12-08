# Redes (Networking)

A continuación, vamos a ver una de las funcionalidades más interesantes de los microcontroladores modernos: la capacidad de conectarse a redes, ya sea a través de Wi-Fi, Ethernet o Bluetooth. Esto permite que nuestros dispositivos puedan comunicarse entre sí y con otros dispositivos en la red, así como acceder a servicios en la nube.

Es importante mencionar que el ESP32 si tiene soporte nativo para Wi-Fi y Bluetooth, mientras que la Raspberry Pi Pico sólo los modelos "W" soportan Wi-fi, pero no Bluetooth. Para utilizar estas funcionalidades, es necesario contar con las librerías adecuadas que faciliten la conexión y comunicación en red.

## Conexión a una red Wi-Fi

Para conectar nuestro dispositivo a una red Wi-Fi, utilizamos la librería `network`. A continuación, se muestra un ejemplo de cómo conectarse a una red Wi-Fi utilizando el ESP32 o Raspberry Pi Pico W:

```python
import network
import time 
ssid = 'TU_SSID'
password = 'TU_CONTRASEÑA'
# Crear una instancia de la interfaz Wi-Fi
wifi = network.WLAN(network.STA_IF)
# Activar la interfaz Wi-Fi
wifi.active(True)
# Conectarse a la red Wi-Fi
wifi.connect(ssid, password)
while not wifi.isconnected():
    time.sleep(1)
print("Conectado a la red Wi-Fi:", wifi.ifconfig())
```

En este ejemplo, reemplaza `'TU_SSID'` y `'TU_CONTRASEÑA'` con el nombre y la contraseña de tu red Wi-Fi. El código activa la interfaz Wi-Fi, intenta conectarse a la red especificada y espera hasta que la conexión se establezca correctamente. Una vez conectado, imprime la configuración de red obtenida (dirección IP, máscara de subred, puerta de enlace y servidores DNS).

## Creación de Redes Ad-Hoc

Además de conectarse a redes Wi-Fi existentes, algunos dispositivos permiten crear redes ad-hoc o puntos de acceso (AP) para que otros dispositivos puedan conectarse directamente a ellos. A continuación, se muestra un ejemplo de cómo configurar un punto de acceso Wi-Fi utilizando el ESP32 o Raspberry Pi Pico W:

```python
import network
# Crear una instancia de la interfaz Wi-Fi en modo AP
ap = network.WLAN(network.AP_IF)
# Activar la interfaz AP
ap.active(True)
# Configurar el SSID y la contraseña del punto de acceso
ap.config(essid='Mi_Punto_de_Acceso', password='mi_contraseña')
print("Punto de acceso creado. SSID: Mi_Punto_de_Acceso")
```
En este ejemplo, se crea un punto de acceso con el SSID `'Mi_Punto_de_Acceso'` y la contraseña `'mi_contraseña'`. Otros dispositivos podrán conectarse a este punto de acceso utilizando las credenciales proporcionadas.

## Uso de Contraseñas y/o Seguridad

Es recomendable utilizar ficheros externos con la configuración de nuestro SSID y contraseñas para evitar exponer esta información sensible en el código fuente. De esta manera, podemos mantener nuestras credenciales seguras y facilitar la gestión de múltiples configuraciones de red.

Veamos un ejemplo:

```python
# archivo config.py
SSID = 'TU_SSID'
PASSWORD = 'TU_CONTRASEÑA'
```

```python
# archivo main.py
import network
from config import SSID, PASSWORD
import time
# Crear una instancia de la interfaz Wi-Fi
wifi = network.WLAN(network.STA_IF)
# Activar la interfaz Wi-Fi
wifi.active(True)
# Conectarse a la red Wi-Fi
wifi.connect(SSID, PASSWORD)
while not wifi.isconnected():
    time.sleep(1)
print("Conectado a la red Wi-Fi:", wifi.ifconfig())
```
En este ejemplo, las credenciales de la red Wi-Fi se almacenan en un archivo separado llamado `config.py`, lo que ayuda a mantener el código más limpio y seguro.