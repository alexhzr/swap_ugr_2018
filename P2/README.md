# Práctica 2
##### Alejandro Manuel Hernández Recio y Francisco Miguel Toledo Aguilera

## Escenario
Hemos utilizado VirtualBox en host Ubuntu 16.04, con dos máquinas virtuales Ubuntu Server 16.04 conectadas ambas a un adaptador *host only*.

Las IP son las siguientes:
 - Máquina 1: 10.0.2.15
 - Máquina 2: 10.0.2.16

## Desarrollo
### Crear un *tar* con ficheros locales en un equipo remoto
Con el comando siguiente, creamos un contenedor *tar* y lo copiamos mediante *ssh* al equipo destino. En este caso, se copia desde la máquina 2 hacia la máquina 1.
``tar czf - directorio | ssh equipodestino 'cat > ~/tar.tgz'``

![](https://imgur.com/XFlDWeR.jpg)

### Instalar rsync
Instalamos ``rsync`` usando ``apt-get``:
``sh
sudo apt-get install rsync
``
Ahora cambiamos el propietario del directorio ``/var/www``:
``sh
sudo chown usuario:usuario /var/www
``

![](https://imgur.com/tWtm6SJ.jpg)

Por último, para probar ``rsync``, hacemos una copia del directorio web de la máquina 1 en la máquina 2:
``sh
rsync -avz --delete --exclude=**/stats --exclude=**/error --
exclude=**/files/pictures -e ssh usuario@10.0.2.16:/var/www/ /var/www/
``

### Acceso sin contraseña para ssh
En este apartado vamos a automatizar el acceso entre máquinas generando pares de claves para el ``ssh``. La orden a utilizar será:
``sh
ssh-keygen -b 4090 -t rsa
``
Seguimos los pasos que nos dice la terminal y al final tendremos el par de claves generados bajo nuestro directorio ``/home/usuario/.ssh``. Lanzamos el comando ``ssh-copy-id 10.0.2.16`` para copiar la clave pública a la máquina 2. En este punto, haciendo un ``ssh`` a la máquina 2, debería conectarnos sin pedir contraseña:

![](https://imgur.com/YJgiXLU.jpg)


### Programar tareas con crontab
El contenido de nuestro fichero ``crontab`` es el siguiente:

![](https://imgur.com/Sh5423X.jpg)

Y aquí la prueba de que el fichero se replica entre máquinas:

![](https://imgur.com/eYXkKKp.jpg)
