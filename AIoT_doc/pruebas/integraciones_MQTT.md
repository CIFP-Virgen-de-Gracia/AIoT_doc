# Configurar Shelly H&T para enviar mensajes MQTT

```{warning}
Desde la aplicación **Shelly** para el móvil se resuelven muchos problemas derivados de la configuración por AP que se muestra a continuación.
En la aplicación móvil iríamos la configuración e introduciríamos lo siguiente:
![alt text](image-23.png)
```

- Pulsar el botón de `Reset` para activar el modo AP
- Desde otro dispositivo en la red WiFi conéctate al AP
    - Escanear las redes WiFi y conectar al SSID=Shelly-HTGT-<dir MAC>
- Abrir un navegador desde el dispositivo y escribir la URL http://192.168.33.1
- Configurar el cliente MQTT (publisher)
- `Settings -> Connectivity Settings -> MQTT`
    
    `Enable MQTT network`

    `No SSL`

    `MQTT prefix: topic en el mensaje, por ejemplo “shellyh&t1”`
    
    Dirección IP y puerto 1883 del servidor MQTT
    Nombre de usuario y password que escribiste cuando instalaste MQTT (opcional)
    Save Settings

# Obtener un mensaje MQTT del dispositivo Shelly H&T

- Abrir un terminal en el servidor de Home Assistant
- Ejecutar un subscriptor MQTT de cualquier topic “#”

```bash
mosquitto_sub -v -h localhost -p 1883 -t '#'
```

Esperar a recibir un mensaje del dispositivo.

![alt text](image-24.png)
