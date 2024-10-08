# Instalación de Ubuntu Server con GUI en MACOS con arquitectura ARM (M3)

## Instalación de VMWare Pro Fusion 13 

Instalamos el visor con **Licencia de uso personal** para hacer pruebas.

```{note}
https://support.broadcom.com/group/ecx/productdownloads?subfamily=VMware+Fusion
```

## Instalción Ubuntu Server ARM

Descargamos Ubuntu Server para ARM y posteriormente instalaremos el GUI para tener interfaz gráfica.

```{note}
https://ubuntu.com/download/server/arm
```
Durante la instalación casi todas las opciones serán por defecto, salvo que instalaremos el servidor **OpenSSH**

![alt text](image.png)

Para poder reiniciar, debemos expulsar el CD

![alt text](image-1.png)

## Instalción de GUI para Ubuntu Server

```{bash}
sudo apt install ubuntu-desktop
reboot
```

![alt text](image-2.png)

## Instalación de VMWare Tools

Entramos en la máquina virtual de Ubuntu server y abrimos un terminal:

```bash
sudo apt-get update
sudo apt-get install open-vm-tools-desktop
sudo reboot
```

