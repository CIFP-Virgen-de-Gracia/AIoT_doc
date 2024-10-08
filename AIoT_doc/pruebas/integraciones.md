# Integraciones de un sensor (Shelly H&T)

## Integración oficial

- Desde la UI de Home Assistant
  - **Device & Services -> Integrations -> + Integration**
  - Escribir Shelly
  - Escribir la IP del dispositivo y dejar el puerto por defecto 80
  ![alt text](image-7.png)
  - Si HA es capaz de comunicar con el dispositivo, la integración se ha realizado con éxito y el dispositivo se añade a HA.
  ![alt text](image-5.png)
- En **Devices-> Settings** aparecerá el dispositivo y entidades
![alt text](image-6.png)
- En el Dashboard principal (**Overview**) se muestran los valores de las entidades
![alt text](image-8.png)


## Integración usando MQTT (Message Queue Telemetry Transport)
![alt text](image-9.png)

### Instalamos **mosquitto** vía **docker-compose**
- Acceder a **Portainer**: https://127.0.0.1:9443
- Copiamos el fichero `stack-mosquitto.yaml`dentro del stack "Stack HA"
  `stack-mosquitto.yaml`

  ```yaml
    mqtt:
    image: eclipse-mosquitto:1.6
    container_name: mosquitto
    volumes:
      - /docker/mosquitto/config:/mqtt/config
      - /docker/mosquitto/log:/mqtt/log
      - /docker/mosquitto/data:/mqtt/data
    ports:
      - "1883:1883"
      - "8883:8883"
  ```
El stack de HA se quedará:
```yaml
services:
  homeassistant:
    container_name: homeassistant
    image: "ghcr.io/home-assistant/home-assistant:stable"
    volumes:
      - /docker/homeassistant:/config
      - /etc/localtime:/etc/localtime:ro
      - /run/dbus:/run/dbus:ro
    restart: unless-stopped
    privileged: true
    network_mode: host
  mqtt:
    image: eclipse-mosquitto:1.6
    container_name: mosquitto
    volumes:
      - /docker/mosquitto/config:/mqtt/config
      - /docker/mosquitto/log:/mqtt/log
      - /docker/mosquitto/data:/mqtt/data
    ports:
      - "1883:1883"
      - "8883:8883"
    network_mode: host
```

```{warning}
Vamos a crear un fichero de configuración, para ello debemos tener el contenido del curso descargado de:
**http://github.com/sescolar/cursos.git**
```

![alt text](image-10.png)
- Creamos un fichero de configuración `mosquitto.conf`
```bash
sudo mkdir -p /docker/mosquitto/config
sudo cp CursoHAyLoRA/instalacion/homeassistant/mosquitto.conf /docker/mosquitto/config
```

- Modificamos el stack -> **Deploy Stack**
- Verificar los logs
- Instalar utilidades para publicar/suscribir mensajes MQTT
```bash
sudo apt-get install mosquitto-clients
```
### Vincular MQTT a Home Assistant
- En Home Assistant **Settings->Devices & Services**
- **Integrations -> Add integration -> MQTT**
![alt text](image-11.png)

Miramos la IP del broker
![alt text](image-13.png)

Connfiguramos los datos en base a eso:
![alt text](image-14.png)

Si se ha configurado con éxito aparecerá en el dashboard de integraciones:
![alt text](image-15.png)

Vamos a comprobar que funciona:
`Settings -> Devices & Services -> Integration -> MQTT -> Configure`
- Subscribir al topic “/topic/prueba” 
- Publicar en el topic “/topic/prueba” el mensaje “hola”

![alt text](image-16.png)

También podemos publicar y suscribirnos por comandos. Por ejemplo, para publicar:
```bash
mosquitto_pub -h 172.18.0.2 -p 1883 -t /topic/prueba -m "Adios"
```

![alt text](image-17.png)

![alt text](image-18.png)