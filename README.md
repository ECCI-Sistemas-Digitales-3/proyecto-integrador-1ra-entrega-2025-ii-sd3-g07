[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=21605303&assignment_repo_type=AssignmentRepo)
# Proyecto integrador 1ra Entrega

## Integrante

Paula Cañon

## Arquitectura propuesta

permite controlar remotamente cinco bombas de color (CYAN, MAGENTA, YELLOW, BLACK y WHITE) mas una bomba para el mezclador utilizando un microcontrolador ESP32 conectado a un broker MQTT.
El código recibe mensajes desde el broker para encender o apagar cada bomba de forma individual.

Cada bomba está asociada a un pin digital del ESP32, y la comunicación se realiza a través del protocolo MQTT, lo que permite la integración con paneles IoT, dashboards o aplicaciones de monitoreo remoto.


## Periférico a trabajar
Electrovalvulas y bombas

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


Con respecto al montaje fisico Durante las pruebas se evidenció que la electroválvula actual no proporciona el caudal suficiente. Al intentar usar una bomba de paso, el fluido se estancó en la salida, generando pérdidas de pintura. Por ello, se propone emplear una electroválvula de mayor capacidad y ubicar la bomba después de esta. Así se garantiza un flujo adecuado: la electroválvula controla la cantidad de pintura y, tras su cierre, la bomba continúa unos segundos para impulsar el remanente y evitar residuos en los conductos.

![Imagen de WhatsApp 2025-11-21 a las 11 34 07_f73c49da](https://github.com/user-attachments/assets/4298ce3a-367a-4344-8eb3-4d29d5fd97fc)

Introducción

El proyecto de control y dosificación de pintura mediante una mezcladora automatizada busca garantizar precisión, repetibilidad y eficiencia en los procesos de llenado. Para ello, se integran elementos electrome­cánicos como electroválvulas, bombas y sensores, controlados mediante un sistema digital basado en ESP32 y comunicación MQTT. Durante la fase experimental se realizaron diferentes pruebas con los actuadores encargados del transporte del fluido, lo que permitió identificar limitaciones en el flujo, problemas de estancamiento y pérdidas de material, aspectos críticos para la calidad del proceso de mezclado. Como resultado, se desarrolló una propuesta mejorada que optimiza la extracción y conducción de la pintura, asegurando un funcionamiento más estable y confiable dentro del sistema automatizado.

 funcionamiento y pruebas

En las primeras pruebas realizadas con la electroválvula encargada de liberar la pintura desde el tanque principal se evidenció que el caudal obtenido era significativamente bajo, lo que afectaba la capacidad de dosificación y el tiempo total del proceso. Ante esta situación se evaluó el uso de una bomba de paso para incrementar el flujo. No obstante, durante su operación se presentó estancamiento del fluido justo en la salida de la bomba, generando una acumulación que derivó en pérdidas de pintura y en una reducción de la exactitud al momento de medir el volumen solicitado para cada mezcla.

![Imagen de WhatsApp 2025-11-29 a las 08 57 37_4f219746](https://github.com/user-attachments/assets/6a8d10b1-fbb6-41de-b76a-9c6f76e1a4b0)

Estos inconvenientes llevaron a plantear una mejora en el diseño del sistema hidráulico. La solución propuesta consiste en reemplazar la electroválvula inicial por una de mayor sección de paso, permitiendo que un volumen superior de pintura circule sin restricciones. Además, se determinó que la bomba debe instalarse después de la electroválvula, con el propósito de aumentar el caudal únicamente cuando el flujo ya ha sido liberado correctamente. De esta manera, la electroválvula controla el volumen exacto de pintura extraída, mientras que la bomba se mantiene activa durante un tiempo adicional tras el cierre de la válvula. Este tiempo de purga permite evacuar la pintura retenida en las mangueras y evita que queden residuos dentro del sistema, garantizando líneas limpias y una mezcla más uniforme.

Paralelamente, el sistema de control implementado mediante ESP32 y MQTT facilita la activación remota de las bombas. Los comandos enviados al tópico configurado permiten encender y apagar cada actuador en tiempo real, evidenciándose en las pruebas que la comunicación se mantiene estable y que el procesamiento de mensajes es inmediato. Las imágenes registradas muestran el ciclo completo: conexión WiFi del dispositivo, enlace con el broker MQTT, recepción de órdenes como “B1_ON” y respuesta efectiva mediante el encendido o apagado físico de las bombas. Este esquema de control proporciona una plataforma flexible, escalable y adecuada para sistemas de automatización industrial liviana como el propuesto en este proyecto.



![Imagen de WhatsApp 2025-11-29 a las 08 57 36_79f7413b](https://github.com/user-attachments/assets/4b70aed0-2f70-474e-8bbc-d1b754aef914)

Conclusiones

Las pruebas iniciales permitieron identificar limitaciones importantes en el sistema de dosificación, especialmente relacionadas con el caudal y la acumulación de fluido en la salida de la bomba.

La implementación de una electroválvula de mayor paso, junto con la reubicación de la bomba después de la misma, mejora el flujo disponible y evita la acumulación de pintura en las líneas.

Mantener la bomba activa durante un intervalo de purga después del cierre de la electroválvula permite eliminar residuos de pintura en las tuberías, contribuyendo a un proceso más limpio y eficiente.

El sistema de control basado en ESP32 y MQTT demostró ser confiable, permitiendo la activación precisa y remota de las bombas, así como una comunicación constante con el broker.

La integración del control digital con mejoras en la parte hidráulica genera un sistema de mezcla más estable, exacto y adaptable a futuras expansiones o automatizaciones.

<!-- Subir en una carpeta src los códigos que tienen hasta el momento y esta sección agregar lo que consideren necesario referente a sus avances. -->
