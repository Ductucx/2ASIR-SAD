#  TAREA 4 - SSH

### ÍNDICE





---
### Fichero de configuración sshd.config

ATENCIÓN, antes de avanzar con la tarea hay que asegurarnos de instalar el openssh-server en el servidor y tener instalado el ssh-client en el equipo cliente.

Para entrar en el fichero de configuración tendremos que entrar en "sshd.config", si no sabemos en que ruta está podremos ejecutar el comando de la siguiente captura:

![alt image capturas](./IMG/captura1.png)

Una vez dentro cambiaremos las siguientes propiedades:
- Deshabilitar passwords en blanco

![alt image capturas](./IMG/captura2.png)

Esto lo necesitaremos para más adelante:

![alt image capturas](./IMG/captura2.png)


- Cambiar el puerto por defecto

![alt image capturas](./IMG/captura3.png)

- Deshabilitar el login de root a través de ssh

![alt image capturas](./IMG/captura4.png)

- Deshabilitar el protocolo 1 de ssh

![alt image capturas](./IMG/captura5.png)

- Configurar un intervalo de inactividad de la sesión

![alt image capturas](./IMG/captura6.png)

- Permitir el acceso únicamente a ciertos usuarios

![alt image capturas](./IMG/captura7.png)

Una vez hecho todo esto, haremos un `sudo systemctl restart sshd`

Para comprobar que las propiedades se han aplicado, intentaremos hacer un ssh normal para que nos diga que no roconoce la conexión por el puerto 22:

![alt image capturas](./IMG/captura9.png)

---
### Permitir login por cifrado asimétrico

Para poder hacer un loggin sin contraseña y utilizando exclusivamente cifrado asimétrico ssh, primero tendremos que deshabilitar el "PasswordAuthentication" en el archivo "sshd.config" como aclaramos anteriormente:

![alt image capturas](./IMG/captura8.png)

Continuamos con la generación de claves:

![alt image capturas](./IMG/captura10.png)

![alt image capturas](./IMG/captura11.png)

![alt image capturas](./IMG/captura12.png)

![alt image capturas](./IMG/captura13.png)

![alt image capturas](./IMG/captura14.png)