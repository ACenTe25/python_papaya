# CLUB DE APRENDIZAJE DE PYTHON

Este documento explica cómo utilizar los recursos que compartimos para 
el club de aprendizaje de Python.

## 1. ENTORNO DE DESARROLLO

Vamos a desarrollar en Ubuntu 24.04 LTS, usando Python 3.12. Para que todos 
tengamos acceso vamos a usar una máquina virtual de Multipass. Aquí las instrucciones 
para que tengamos un entorno adecuado:

1. Generar llaves SSH para interactuar con GitHub: sigue [las instrucciones de GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) utilizando la opción de RSA 4096 y el email que tienes registrado en GitHub. [Agrega tu llave pública (.pub) a GitHub](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account) y [pruébala](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection).

2. Descarga e [instala Multipass](https://multipass.run/install).

3. Descarga el archivo [`papaya-init.yaml`](papaya-init.yaml) de este repositorio.

4. Crea una VM usando Multipass con la configuración adecuada: 
```bash
multipass launch -c 4 -m 3G -d 20G -n pythonic-papaya --cloud-init papaya-init.yaml noble
```

5. Transfiere tu llave pública a la VM: 
```bash
multipass transfer id_rsa.pub pythonic-papaya:/home/ubuntu/llave_host
``` 
Luego entra a la VM, y copia el contenido de la llave a una línea nueva en el archivo `/home/papajohn/.ssh/authorized_keys`.

6. Instala un cliente RDP en tu máquina host: [Remmina](https://remmina.org/) para Linux, [Microsoft Remote Desktop](https://apps.microsoft.com/detail/9wzdncrfj3ps?hl=en-US&gl=US) 
para Windows o macOS, o [Jump Desktop](https://apps.apple.com/ua/app/jump-desktop-rdp-vnc-fluid/id524141863?l=ru&mt=12) para macOS. Esto se va a utilizar cuando vayamos a 
probar/usar aplicaciones con GUI.

7. Desde tu navegador en tu máquina host, entra a `https://<IP DE LA VM>:8000` y empieza a desarrollar.

8. Para probar apps con CLI/TUI, conéctate por SSH a la máquina virtual, o usa Multipass para acceder a una shell de manera directa.

9. Para probar apps con GUI, usa el cliente RDP con la dirección IP de tu VM. Los datos de acceso son usuario `papajohn` y contraseña `SafePapaya123`.
