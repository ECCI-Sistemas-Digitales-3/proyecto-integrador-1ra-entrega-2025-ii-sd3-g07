[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21605303&assignment_repo_type=AssignmentRepo)
# Proyecto integrador 1ra Entrega

## Integrantes
Camilo Cruz
Daniel Cobos
Paula Cañon
## Arquitectura propuesta



## Periférico a trabajar
Valvulas, estamos en equipo con el grupo 6

## Avances

<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->

Este codigo permite controlar remotamente cinco bombas de color (CYAN, MAGENTA, YELLOW, BLACK y WHITE) utilizando un microcontrolador ESP32 conectado a un broker MQTT.
El código recibe mensajes desde el broker para encender o apagar cada bomba de forma individual.

Cada bomba está asociada a un pin digital del ESP32, y la comunicación se realiza a través del protocolo MQTT, lo que permite la integración con paneles IoT, dashboards o aplicaciones de monitoreo remoto.

Antes de iniciar la comunicación MQTT, el programa verifica la conexión WiFi utilizando el módulo wify:

if not wify.conectar():
    print("❌ Error: no se puede continuar sin conexión WiFi")
    while True:
        time.sleep(1)


Si la conexión falla, el sistema se detiene hasta restablecer el enlace.

Configuración del Broker MQTT
BROKER = "6.tcp.ngrok.io"
PORT = 18263


El ESP32 se conecta a un broker MQTT remoto (en este caso a través de ngrok, lo que permite exponer el servicio a Internet).

Los topics definidos corresponden a cada color de bomba:

TOPICS = {
    "CYAN": b"bombas/CYAN",
    "MAGENTA": b"bombas/MAGENTA",
    "YELLOW": b"bombas/YELLOW",
    "BLACK": b"bombas/BLACK",
    "WHITE": b"bombas/WHITE"
}
Control de Bombas (Pines de salida)

Cada bomba está conectada a un pin digital del ESP32:

Color	Pin GPIO	Descripción
CYAN	14	Controla bomba CYAN
MAGENTA	12	Controla bomba MAGENTA
YELLOW	13	Controla bomba YELLOW
BLACK	27	Controla bomba BLACK
WHITE	26	Controla bomba WHITE

Ejemplo de configuración:

bombas = [
    Pin(14, Pin.OUT),
    Pin(12, Pin.OUT),
    Pin(13, Pin.OUT),
    Pin(27, Pin.OUT),
    Pin(26, Pin.OUT)
]

Recepción de Mensajes MQTT

La función mensaje(topic, msg) se ejecuta automáticamente cada vez que llega un mensaje desde el broker.

Dependiendo del topic (por ejemplo, bombas/CYAN) y del mensaje (ON u OFF), se activa o desactiva la bomba correspondiente:

if topic == "bombas/CYAN":
    if msg == "ON" and flag_CYAN_galga:
        bombas[0].value(1)
    elif msg == "OFF" or not flag_CYAN_galga:
        bombas[0].value(0)


Esto se repite para cada color.
Las flags (flag_CYAN_galga, etc.) permiten activar o desactivar la respuesta de cada bomba por software (sirven como “interruptores lógicos”).

Conexión y Suscripción MQTT

El ESP32 se conecta al broker MQTT y se suscribe a todos los topics definidos:

def conectar_mqtt():
    client = MQTTClient("ESP32_Bombas", BROKER, PORT)
    client.set_callback(mensaje)
    client.connect()
    for color, t in TOPICS.items():
        client.subscribe(t)
        print(f"✅ Suscrito al topic: {t.decode()}")
    return client

    Bucle Principal

El programa entra en un bucle infinito, escuchando continuamente mensajes entrantes:

while True:
    cliente.check_msg()
    time.sleep(0.1)


Si se interrumpe el programa (por ejemplo, con Ctrl + C), se desconecta del broker de forma segura.

Avances del Proyecto

Hasta el momento, se ha desarrollado la primera versión funcional del sistema de control remoto de bombas mediante MQTT utilizando una tarjeta ESP32.

🔹 Avances técnicos

Se implementó la conexión WiFi mediante un módulo personalizado wify.py, que permite verificar la conexión antes de ejecutar el sistema.

Se estableció la comunicación con un broker MQTT remoto (ngrok) para la recepción de mensajes en tiempo real.

Se definieron cinco canales de control independientes (topics MQTT) para las bombas:
bombas/CYAN, bombas/MAGENTA, bombas/YELLOW, bombas/BLACK, bombas/WHITE.

Cada bomba se controla desde el ESP32 utilizando pines digitales configurados como salida.

Se programó un callback MQTT que interpreta los mensajes ON y OFF para activar o desactivar las bombas.

El sistema mantiene una escucha continua (check_msg()) para responder de forma inmediata a los comandos enviados.

🔹 Pruebas realizadas

Se probaron las conexiones con el broker MQTT usando MQTT Explorer.

Se verificó la correcta respuesta de las bombas a los mensajes ON/OFF.

Se comprobó la estabilidad de la conexión WiFi y la comunicación MQTT durante sesiones prolongadas.
