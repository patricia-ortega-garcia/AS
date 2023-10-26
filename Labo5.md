# Laboratorio 5.- Docker

## Preparativos

### 1. Instalar Docker Engine en el entorno Ubuntu
```
https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository
```
### 2. Familiarizarnos con Redis
```
https://redis.io/
```

## Docker Engine

### Ejecutar ‘docker --help' en el terminal y revisar las opciones disponibles para manipular contenedores

```bash
docker --help
```
### Utilizando la imagen de busybox, obtener la siguiente información. Para ello, sobre-escribe el comando de arranque en cada ocasión:
- Mostrar los sistemas de ficheros que están montados.
```bash
sudo docker run busybox df -h
```
- Mostrar el contenido de la carpeta /etc. ¿Hay más o menos elementos que en la carpeta /etc de un Ubuntu Server?
```bash
sudo docker run busybox ls /etc
```
*veremos que hay muchos menos archivos que si hiciesemos '~$ ls /etc'*
- Mostrar el cuántos binarios ejecutables hay en la carpeta /bin. ¿Hay más o menos elementos que en la carpeta /bin de un Ubuntu Server?
```bash
sudo docker run busybox ls /bin | wc -l
405
```
*Si lo hacemos en la carpeta /bin de un Ubuntu Server obtenemos...*
```bash
ls /bin | wc -l
1096
```
*Con lo que concluimos que hay muchos menos elementos en la carpeta /bin de busybox*
- Mostrar cuantas interfaces de red tiene y qué IPs tienen asignadas.
```bash
sudo docker run busybox ifconfig

```
<br/><br/>
### Lanzar un contenedor de busybox con el comando “ping www.ehu.eus”. Sin interrumpir su ejecución, parar  el contenedor desde otro terminal con el comando “docker stop”. ¿Se realiza al instante?
```bash
sudo docker run busybox ping www.ehu.eus
```
*Desde otra terminal...*
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS     NAMES
4fc315a88e2b   busybox   "ping www.ehu.eus"   16 seconds ago   Up 15 seconds             elastic_lichterman

sudo docker stop 4fc315a88e2b
```
*No lo hace instaneamente*

### Repetir la tarea anterior, pero utilizando “docker kill”. ¿Qué diferencia hay con utilizar “docker stop”?
```bash
sudo docker run busybox ping www.ehu.eus
```
*Desde otra terminal...*
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND              CREATED          STATUS          PORTS     NAMES
4db168d7290c   busybox   "ping www.ehu.eus"   16 seconds ago   Up 15 seconds             recursing_cerf

sudo docker kill 4db168d7290c
```
*Si lo hace instantaneamente*
<br/><br/>
### Abrir una Shell dentro de un contenedor busybox utilizando “docker run” y realizar lo siguiente: 
```bash
sudo docker run -it busybox
```
- Mostrar el número de procesos en ejecución.
```
/ # ps | wc -l
```
- Crear un fichero llamado miFichero dentro de /home.
```
/ # cd home
/home # touch miFichero
```
- Cerrar la sesión.
```
/home # exit
```
### Abrir una Shell de nuevo en un contenedor busybox. ¿El fichero en /home sigue estando? ¿Por qué? 
 ```bash
sudo docker run -it busybox
```
```
/ # cd home
/home # ls
```
*El ficherno no sigue estando en /home porque al crear un nuevo contenedor no se guarda la info del anterior*
<br/><br/>
### Lanzar un contenedor con el servidor redis. Para ello, buscar en DockerHub el nombre de la imagen oficial  de Redis. Si se ha lanzado correctamente, debería mostrarse “Ready to accept connections” en la salida. 
```bash
sudo docker run redis
```
### Con el contenedor en ejecución, abrir una Shell y obtener la siguiente información
*En otra terminal*
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
ec11857fc301   redis     "docker-entrypoint.s…"   7 seconds ago   Up 5 seconds   6379/tcp   angry_wright

sudo docker exec -it ec11857fc301 /bin/bash
```
- Mostrar el contenido de la carpeta /etc. ¿Hay más o menos elementos que en la carpeta /etc de la  imagen busybox?
 ```
ls
adduser.conf            gshadow        motd           rc6.d
alternatives            gshadow-       mtab           rcS.d
apt                     gss            netconfig      resolv.conf
bash.bashrc             host.conf      nsswitch.conf  rmt
bindresvport.blacklist  hostname       opt            security
cron.d                  hosts          os-release     selinux
cron.daily              init.d         pam.conf       shadow
debconf.conf            issue          pam.d          shadow-
debian_version          issue.net      passwd         shells
default                 kernel         passwd-        skel
deluser.conf            ld.so.cache    profile        subgid
dpkg                    ld.so.conf     profile.d      subuid
e2scrub.conf            ld.so.conf.d   rc0.d          systemd
environment             libaudit.conf  rc1.d          terminfo
fstab                   localtime      rc2.d          timezone
gai.conf                login.defs     rc3.d          update-motd.d
group                   logrotate.d    rc4.d          xattr.conf
group-                  mke2fs.conf    rc5.d
```
- Mostrar el cuántos binarios ejecutables hay en la carpeta /bin. ¿Hay más o menos elementos que  en la carpeta /bin de busybox? 
```
ls /bin | wc -l
277

Hay menos elementos que en /bin de busybox
```
<br/><br/>
### Con el contenedor en ejecución, ejecutar el comando “redis-cli” dentro de él para acceder a una consola de  Redis. Añadir una variable llamada miVar con valor 7. Salir de la consola Redis. 
```bash
sudo docker run redis
```
*En otra terminal...*
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
ec11857fc301   redis     "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   6379/tcp   angry_wright

sudo docker exec -it ec11857fc301 redis-cli

127.0.0.1:6379> set miVar 7
OK
127.0.0.1:6379> exit
```
### Sin parar el contenedor, volver a entrar en la consola Redis. Leer el contenido de la variable miVar. 
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED          STATUS          PORTS      NAMES
ec11857fc301   redis     "docker-entrypoint.s…"   10 minutes ago   Up 10 minutes   6379/tcp   angry_wright

sudo docker exec -it ec11857fc301 redis-cli

127.0.0.1:6379> get miVar
"7"
127.0.0.1:6379> exit
```
### Reiniciar el contenedor Redis y entrar a la consola. ¿Es posible leer el contenido de la variable miVar? 
*Hacemos CTRL+C para parar la ejecución de redis*
```bash
sudo docker run redis
```
*En otra terminal...*
```bash
sudo docker ps
CONTAINER ID   IMAGE     COMMAND                  CREATED         STATUS         PORTS      NAMES
bca095a9bd7a   redis     "docker-entrypoint.s…"   2 minutes ago   Up 2 minutes   6379/tcp   pensive_mayer

sudo docker exec -it bca095a9bd7a redis-cli

127.0.0.1:6379> get miVar
(nil)
```
*No es posible leer el contenido de la variable miVar porque al reiniciar el contenedor no se han guardado los datos*
### Para acceder al contenedor mientras estaba en marcha habrás utilizado el comando “docker exec”. ¿Qué  diferencia hay al utilizar los siguientes parámetros?: -i, -t, -it, ninguno de ellos. 
```bash
-i (Interactivo)
    Permite interactuar con la terminal
-t (Terminal)
    Muestra la terminal de forma legible
-it (Interactivo y Terminal)
    Permite interactuar con la terminal y la muestra de forma legible
```
<br/><br/>
## Imagenes Docker Propias
### Tarea 1: Crear una imagen propia que ejecute un servidor Redis. El resultado será una imagen equivalente a la imagen oficial de Redis utilizada en la sección anterior del laboratorio, pero hecha por nosotros.
### Crear un directorio nuevo en nuestro sistema y crear un fichero Dockerfile dentro. El Dockerfile deberá tener las siguientes instrucciones: 
- Utilizar como imagen base “ubuntu”. 
- Instalar el paquete “redis” con apt. 
- Ejecutar como comando de arranque “redis-server”. 
```bash
mkdir lab5
mkdir lab5/miImagenRedis
cd lab5/miImagenRedis
nano Dockerfile
```

```Dockerfile
FROM ubuntu:latest
RUN apt -qq update && apt -qq -y install redis
CMD redis-server
```
### Utilizar los comandos de Docker para crear una imagen a partir del Dockerfile con el nombre  <usuario>/<redis-ubuntu> donde <usuario> es un nombre que vosotros elijáis, p.e. ulopez para Unai Lopez. 
```bash
sudo docker build -t="portega/redis-ubuntu" .
```
### En el proceso de creación de la imagen, ¿cuántos contenedores intermedios se han creado? ¿Por qué? 
```bash
sudo docker history portega/redis-ubuntu
IMAGE          CREATED          CREATED BY                                      SIZE      COMMENT
eea89e8c1093   58 seconds ago   CMD ["/bin/sh" "-c" "redis-server"]             0B        buildkit.dockerfile.v0
<missing>      58 seconds ago   RUN /bin/sh -c apt -qq update && apt -qq -y …   51.6MB    buildkit.dockerfile.v0
<missing>      3 weeks ago      /bin/sh -c #(nop)  CMD ["/bin/bash"]            0B        
<missing>      3 weeks ago      /bin/sh -c #(nop) ADD file:63d5ab3ef0aab308c…   77.8MB    
<missing>      3 weeks ago      /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      3 weeks ago      /bin/sh -c #(nop)  LABEL org.opencontainers.…   0B        
<missing>      3 weeks ago      /bin/sh -c #(nop)  ARG LAUNCHPAD_BUILD_ARCH     0B        
<missing>      3 weeks ago      /bin/sh -c #(nop)  ARG RELEASE                  0B   
```
*Los contenedores intermedios son aquellos en los que la imagen pone <missing>. En total hay 7*
<br/><br/>
### Lanzar un contenedor con la imagen recién creada y abrir una Shell mientras esté en ejecución. Utilizar los comandos vistos en la sección anterior para crear una variable “miOtraVar” con valor 8. Salir de la consola y de la Shell.
```bash
sudo docker run portega/redis-ubuntu

sudo docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS     NAMES
5d7ba87468b2   portega/redis-ubuntu   "/bin/sh -c redis-se…"   7 seconds ago   Up 7 seconds             heuristic_robinson

sudo docker exec -it 5d7ba87468b2 bin/bash

root@5d7ba87468b2:/# redis-cli

127.0.0.1:6379> set miOtraVar 8
OK
127.0.0.1:6379> exit
root@5d7ba87468b2:/# exit
exit
```
### Abrir una Shell de nuevo en el contenedor y verificar que la variable “miOtraVar” mantiene su valor.
```bash
sudo docker ps
CONTAINER ID   IMAGE                  COMMAND                  CREATED         STATUS         PORTS     NAMES
5d7ba87468b2   portega/redis-ubuntu   "/bin/sh -c redis-se…"   7 seconds ago   Up 7 seconds             heuristic_robinson

sudo docker exec -it 5d7ba87468b2 bin/bash

root@5d7ba87468b2:/# redis-cli

127.0.0.1:6379> get miOtraVar
"8"
127.0.0.1:6379> exit
```
<br/><br/>
### Tarea 2: Crear una imagen de las mismas características que la anterior, pero utilizando “alpine” como imagen base en lugar de “ubuntu”
### Crear un directorio nuevo en el sistema y crear un fichero Dockerfile dentro. El Dockerfile deberá tener las siguientes instrucciones:
- Utilizar como imagen base “alpine”.
- Instalar el paquete “redis” con el sistema de paquetes de Alpine (Alpine no utiliza apt).
- Ejecutar como comando de arranque “redis-server”.
### Utilizar los comandos de Docker para crear una imagen a partir del Dockerfile con el nombre <usuario>/<redis-alpine> donde <usuario> es el nombre elegido en la tarea anterior.
### Lanzar un contenedor con la imagen y verificar que funciona correctamente: abrir una Shell al contenedor y utilizar la consola de Redis para añadir una variable “miVar” con valor 9.
### Parar el contenedor y comparar los tamaños de las imágenes <redis-ubuntu> y <redis-alpine>. Teniendo en cuenta que ambas cumplen el mismo propósito, ¿cuál ocupa menos?
