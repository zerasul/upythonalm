# Interrupciones

Las interrupciones permiten que un microcontrolador responda a eventos externos o internos de manera inmediata, interrumpiendo la ejecución normal del programa para atender una tarea específica. Esto es especialmente útil en aplicaciones donde la respuesta rápida a eventos es crucial, como en sistemas de control, comunicación y manejo de sensores.

## Configuración de Interrupciones en MicroPython

En MicroPython, las interrupciones se pueden configurar utilizando la clase `Pin` para definir pines de entrada que pueden generar interrupciones. A continuación, se muestra un ejemplo básico de cómo configurar una interrupción en un pin digital:

```python
from machine import Pin
import time
import micropython
# Habilitar el manejo de interrupciones
micropython.alloc_emergency_exception_buf(100)
# Definir una función de manejo de interrupción
def pin_handler(pin):
    print("Interrupción detectada en el pin:", pin)    
# Configurar el pin como entrada con una interrupción en el flanco de subida
pin = Pin(14, Pin.IN, Pin.PULL_UP)
pin.irq(trigger=Pin.IRQ_RISING, handler=pin_handler)
# Bucle principal
while True:
    print("Esperando interrupción...")
    time.sleep(1)
```
En este ejemplo, se configura el pin 14 como una entrada con una resistencia pull-up interna. Se establece una interrupción que se activa en el flanco de subida (cuando el pin cambia de bajo a alto) y llama a la función `pin_handler` cuando ocurre la interrupción.

## Consideraciones Importantes

- **Manejo de Interrupciones**: Las funciones de manejo de interrupciones deben ser lo más rápidas posible, ya que bloquean la ejecución del programa principal. Evita operaciones que puedan tardar mucho tiempo, como esperas o llamadas a funciones complejas.

- **Variables Compartidas**: Si la función de interrupción modifica variables que también son utilizadas en el programa principal, es importante proteger el acceso a estas variables para evitar condiciones de carrera. Esto se puede hacer deshabilitando temporalmente las interrupciones o utilizando mecanismos de sincronización.

- **Prioridad de Interrupciones**: Algunos microcontroladores permiten configurar la prioridad de las interrupciones. Asegúrate de entender cómo funciona esto en el hardware que estás utilizando.

## Aplicaciones Comunes

- **Lectura de Sensores**: Responder rápidamente a cambios en sensores, como botones o sensores de movimiento.

- **Comunicación**: Manejar eventos de comunicación, como la recepción de datos a través de UART, SPI o I2C.

- **Control de Tiempo Real**: Implementar temporizadores y eventos periódicos para tareas de control en tiempo real.

Las interrupciones son una herramienta poderosa en la programación de microcontroladores y pueden mejorar significativamente la eficiencia y capacidad de respuesta de tus aplicaciones en MicroPython.