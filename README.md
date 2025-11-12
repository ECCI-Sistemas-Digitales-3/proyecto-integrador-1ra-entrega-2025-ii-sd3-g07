[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21605303&assignment_repo_type=AssignmentRepo)
# Proyecto integrador 1ra Entrega

## Integrantes
Daniel Cobos
Camilo Cruz
Paula Cañon
## Arquitectura propuesta
permite controlar remotamente cinco bombas de color (CYAN, MAGENTA, YELLOW, BLACK y WHITE) utilizando un microcontrolador ESP32 conectado a un broker MQTT.
El código recibe mensajes desde el broker para encender o apagar cada bomba de forma individual.

Cada bomba está asociada a un pin digital del ESP32, y la comunicación se realiza a través del protocolo MQTT, lo que permite la integración con paneles IoT, dashboards o aplicaciones de monitoreo remoto.


## Periférico a trabajar
Valvulas

## Avances
Hasta el momento, se ha desarrollado la primera versión funcional del sistema de control remoto de bombas mediante MQTT utilizando una tarjeta ESP32.

🔹 Avances técnicos

Se implementó la conexión WiFi mediante un módulo personalizado wify.py, que permite verificar la conexión antes de ejecutar el sistema.

Se estableció la comunicación con un broker MQTT remoto (ngrok) para la recepción de mensajes en tiempo real.

Se definieron cinco canales de control independientes (topics MQTT) para las bombas:
bombas/CYAN, bombas/MAGENTA, bombas/YELLOW, bombas/BLACK, bombas/WHITE.

Cada bomba se controla desde el ESP32 utilizando pines digitales configurados como salida.

Se programó un callback MQTT que interpreta los mensajes ON y OFF para activar o desactivar las bombas.

El sistema mantiene una escucha continua (check_msg()) para responder de forma inmediata a los comandos enviados.
<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->
