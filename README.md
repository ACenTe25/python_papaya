# CLUB DE APRENDIZAJE DE PYTHON

Este documento explica cómo utilizar los recursos que compartimos para 
el club de aprendizaje de Python.

## 0. ÍNDICE

1. [Entorno de desarrollo](#1-entorno-de-desarrollo)

## 1. ENTORNO DE DESARROLLO

Vamos a desarrollar en Ubuntu 24.04 LTS, usando Python 3.12. Para que todos 
tengamos acceso a un entorno similar, vamos a usar una máquina virtual de Multipass. 

> Nota: VirtualBox no le da al host acceso de red al guest (ni viceversa) excepto al usar modo bridge. Multipass configura una interfaz de red por default para establecer su comunicación con el host, pero eso solo funciona en la shell de Multipass. Los nombres asignados a las interfaces de red del guest no son determinísticos, y desde el guest no tenemos acceso a información del host. Todo esto nos permite concluir que no es sencillo ni automático configurar el server de VS Code del guest para que el host tenga acceso cuando el backend es VirtualBox.  
> Además, Multipass no soporta VirtualBox para hosts Linux, y los hosts Windows cuyos dueños requieren Hyper-V para VMware, no deberían interferir con la configuración de Hyper-V para poder usar Multipass.  
> Para simplificar nuestro setup vamos a usar un backend QEMU para Multipass, por lo que el host no puede ser Windows.

Aquí las instrucciones para que tengamos un entorno adecuado:

1. Generar llaves SSH para interactuar con GitHub: sigue [las instrucciones de GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) utilizando la opción de RSA 4096 y el email que tienes registrado en GitHub. [Agrega tu llave pública (.pub) a GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) y [pruébala](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection).

2. Descarga e [instala Multipass](https://multipass.run/install).

3. Descarga el archivo [`papaya-init.yaml`](papaya-init.yaml) de este repositorio.

4. Crea una VM usando Multipass con la configuración adecuada. Desde el directorio donde tengas descargado `papaya-init.yaml`, ejecuta: 
```bash
multipass launch -c 4 -m 3G -d 20G -n pythonic-papaya --cloud-init papaya-init.yaml noble
```
Y espera a que termine de inicializar la VM. Si `multipass` te da un error de timeout esperando la inicialización, puedes entrar a la shell de la VM y ejecutar:
```bash
cloud-init status --wait
```
Y ese comando va a esperar a que termine de aplicar todas las configuraciones de `cloud-init`.

5. Transfiere tu llave pública a la VM para tener acceso a la VM por SSH sin lanzar una shell de Multipass:
```bash
multipass transfer <RUTA DE TU LLAVE PÚBLICA> pythonic-papaya:/home/ubuntu/llave_host
```

Usualmente tus llaves SSH se guardan en `~/.ssh` y usualmente tu llave pública se llama `id_rsa.pub` (usualmente termina en `.pub`). Si usaste valores por defecto para generar tu llave, la orden es:

```bash
multipass transfer ~/.ssh/id_rsa.pub pythonic-papaya:/home/ubuntu/llave_host
```

Luego entra a la VM, y copia el contenido de la llave a una línea nueva en el archivo de authorized keys de `papajohn`:
```bash
sudo sh -c 'echo $(cat /home/ubuntu/llave_pub_host) >> /home/papajohn/.ssh/authorized_keys'
```

6. Instala un cliente RDP en tu máquina host: [Remmina](https://remmina.org/) para Linux, o [Jump Desktop](https://apps.apple.com/ua/app/jump-desktop-rdp-vnc-fluid/id524141863?l=ru&mt=12) para macOS. Esto se va a utilizar cuando vayamos a probar/usar aplicaciones con GUI.

7. Desde tu navegador en tu máquina host, entra a `https://<IP DE LA VM>:8000` y empieza a desarrollar. Para consultar la IP de la VM, puedes hacer desde tu host:
```bash
multipass list
``` 

Y se va a desplegar la IP en alguna de las columnas, en la línea correspondiente a `pythonic-papaya`. O desde la VM puedes hacer:
```bash
hostname -I
```

8. Para probar apps con CLI/TUI, conéctate por SSH a la máquina virtual, o usa Multipass para acceder a una shell de manera directa.

9. Para probar apps con GUI, usa el cliente RDP con la dirección IP de tu VM. Los datos de acceso son usuario `papajohn` y contraseña `SafePapaya123`.

También podríamos seguir pasos similares usando LXD y sería más rápido y eficiente, pues son contenedores de sistema en lugar de VMs, pero eso no funcionaría con hosts macOS, solo para hosts con Linux.
