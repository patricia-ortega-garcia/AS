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

```
- Mostrar el cuántos binarios ejecutables hay en la carpeta /bin. ¿Hay más o menos elementos que en la carpeta /bin de un Ubuntu Server?
```bash

```
- Mostrar cuantas interfaces de red tiene y qué IPs tienen asignadas.
```bash

```
