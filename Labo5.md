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
- Crear un fichero llamado miFichero dentro de /home. 
- Cerrar la sesión. 
