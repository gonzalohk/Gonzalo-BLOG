---
title: 'Contenedores con Linux Containers'
published: false
date: '10-07-2020 15:01'
publish_date: '10-07-2020 15:01'
metadata:
    keyworks: 'lxc, lxd, linux containers, container, contendedores, virtualization,virtualizacion'
taxonomy:
    category:
        - Linux
    tag:
        - Virtualizacion
        - Contenedores
---


Los contenedores son tecnologías que permiten **empaquetar y aislar las aplicaciones junto a todo el entorno que necesitan durante su tiempo ejecución**.  En efecto, los contenedores comparten el mismo kernel del sistema operativo y separan los procesos de las aplicaciones del resto del sistema. 

Existen varias opciones entre las que se que aplican esta tecnología, pero es necesario diferenciar entre contendores de sistema operativo como LXC (LinuX Containers), OpenVZ y contenedores de aplicación como Docker.


## ¿Que es es LXC?
LXC o  Linux Containers, ofrece un **entorno de virtualización a nivel del sistema operativo para linux.**  En efecto, este tipo de virtualización permite que sistemas operativos se ejecuten simultáneamente en un solo sistema de hardware.

Es decir, LXC permite que un servidor físico ejecute múltiples instancias de sistemas operativos aislados. Sin embargo, LXC no provee de una máquina virtual más bien provee un entorno virtual que tiene su propio espacio de procesos y redes, esto es posible gracias a que básicamente **LXC utiliza Cgroups y Namespaces para proporcionar el aislamiento** requerido. 

Para ser más específicos LXC usa las siguientes características del kernel para contener procesos:
- Kernel namespaces (ipc, uts, mount, pid, network and user)
- Apparmor y  perfiles SELinux
- Politicas Seccomp
- Chroot (using pivot_root)
- CGroups (control groups)

LXC define su arquitectura de la siguiente manera.

<p align="center">
<img src="img/lxc-architecture.jpg">
</p>

Linux Containers tiene como objetivo crear un ambiente lo más cercano posible a una instalación estándar de Linux pero sin la necesidad de un kernel separado.

## Instalacion
Para instalar lxc utilizaremos el gestor de paquete snappy o snap, en efecto lo instalamos en caso de no contar con el mismo.
```
apt update
apt install snapd
```
Posteriormente, procedemos a instalar lxd que incluirá lxc y las herramientas necesarias para crear los contenedores.
```
snap install lxd
```
Es necesario inicializar lxd, de modo tal que procedemos a establecer configuraciones iniciales para ello nombramos al storage pool, seleccionar el sistema de archivos, definir la dimensión del mismo además de establecer configuraciones iniciales con el siguiente comando.
```
lxd init
```
## Imagenes
Las imágenes son obtenidas por defecto de https://images.linuxcontainers.org, esto puede ser comprobado ejecutando el siguiente comando. 
```
lxc remote list
```
Así mismo, puede ser personalizado para que en un futuro se obtengan de otros repositorios incluyendo locales.

El listado de las imágenes disponibles puede ser visualizado de la siguiente manera.
```
lxc image list images:
```
Para este ejemplo se obtendrá una imagen de Ubuntu 18.04, el mismo será copiado a nuestro equipo local con el alias original (ubuntu) por defecto.
```
lxc image copy ubuntu:18.04 local: --copy-aliases
```
Verificamos si la imagen ya se encuentra en nuestro repositorio local.
```
lxc image list
```
La imagen obtenida anteriormente puede ser descrita, editada, eliminada de la siguiente manera.
```
lxc image info ubuntu
lxc image edit ubuntu
lxc image delete ubuntu
```
## Creación Contenedores
La creación de los contenedores a partir de las imágenes locales se realiza ejecutando los siguientes comandos especificando el sistemas operativo conjuntamente el alias que tiene la imagen. En en ejemplo se crea un contenedor denominado webserver.

Crear un contenedor
```
lxc init images:ubuntu/18.04 webserver
```
Crear e iniciar un contenedor
```
lxc launch images:ubuntu/18.04 webserver
```
El contenedor es creado en cuestión de minutos, una vez finalizado se puede ver el estado y detalles de los contenedores.
```
lxc list
```
Por otro lado, es posible crear el contenedor en función a profiles que especifique la asignación de recursos para este ejemplo se usa el profile por defecto.

Existen múltiples operaciones básicas que pueden ser realizadas como obtener información, iniciar, detener, pausar, eliminar, renombrar un contenedor.
```
lxc info webserver
lxc start webserver
lxc stop webserver
lxc pause webserver
lxc delete webserver
lxc move webserver webserver01
```
Para clonar un contenedor se indica el contenedor original seguido del nombre asignado al nuevo.
```
lxc copy webserver webserver10
```
## Acceso al contenedor
Para acceder e ir directamente al bash del contenedor denominado webserver utilizamos el siguiente comando de modo tal que ya es posible realizar cualquier operación tal como si se estuviera en una nueva instalación de linux.
```
lxc exec webserver bash
```
Así mismo, la ejecución de comandos puede ser realizada fuera del contenedor de la siguiente manera.
```
lxc exec webserver -- /bin/bash
lxc exec webserver -- apt-get update
lxc exec webserver -- ps -ef
```
LXC permite obtener archivos del contenedor y mover archivos al contenedor.
Extraer archivos del contenedor
```
lxc file pull webserver/etc/hosts
```
Llevar/mover archivos al contenedor
```
lxc file push webserver/tmp/
```

## Networking 
La gestión de redes es completa, pero cuando se crea un contenedor  por defecto se le es asignado una interfaz de tipo bridge que responde a un segmento de red y nat habilitado.
```
lxc network list
```
Crear una nueva red con configuraciones por defecto.
```
lxc network create testbr0
```
Crear una nueva red especificando el segmento y estableciendo nat.
```
lxc network create testbr0 ipv6.address=none ipv4.address=10.0.3.1/24 ipv4.nat=true
```
Eliminar una red existente.
```
lxc network delete testbr0
```
Obtener información y propiedades de la red.
```
lxc network info testbr0
lxc network show testbr0
```
Asociar la nueva red.
```
lxc network attach testbr0 webserver eth1
```
Desasociar una red.
```
lxc network detach testbr0 webserver
```
En caso de tener la necesidad de asignar una IP estática ejecutamos lo siguiente.
```
lxc config device set webserver eth1 ipv4.address 10.220.251.200
```

## Storage
El almacenamiento es gestionado mediante un storage pool y volúmenes que pueden ser personalizados.

Para este ejemplo en un inicio se nombró al storage pool como "lxd"
```
lxc storage list
```
Obtener la información y propiedades del storage pool.
```
lxc storage info lxd
lxc storage show lxd
```
Los volúmenes permitirán gestionar mejor el almacenamiento de nuestros contenedores permitiendo limitar el espacio de uso del disco y organizarla de mejor manera.
 
Listar volúmenes del pool denominado lxd.
```
lxc storage volume list lxd
```
La creación volúmenes se realiza mediando lxc, especificando el storage pool y el nombre que se le será asignado.
```
lxc storage volume create lxd vol10
```
En efecto, se necesita asociar/desasociar los volúmenes a los contenedores de la forma siguiente.

Asociar 
```
lxc storage volume attach lxd vol10 webserver data /opt/data
```
Desasociar
```
lxc storage volume detach lxd vol10 webserver data
```
Copiar volumen
```
lxc storage volume copy lxd/vol10 lxd/vol11
```
Eliminar
```
lxc storage volume delete lxd vol11
```

## Manejo de Recursos
Cuando los contenedores fueron creados con el profile por defecto, este usa exactamente los mismos recursos que la máquina host, esto puede ser verificado con el siguiente comando, mostrando la misma cantidad de CPUs usados en la máquina host como el contenedor. 
```
lxc exec webserver -- cat /proc/cpuinfo | grep ^proces
```
CPU
Para limitar el uso de cores del CPU, se puede establecer la cantidad o especificar que cores trabajaran con el contenedor.
```
lxc config set webserver limits.cpu 2
```
Hacer uso de los cores 1 y 4.
```
lxc config set webserver limits.cpu 1,4
```
RAM
Limites de memoria
```
lxc config set webserver limits.memory 256MB
```
Deshabilitar el swap del contenedor
```
lxc config set webserver limits.memory.swap false
```
DISCO
Verificar el espacio de un contenedor
```
lxc exec webserver -- df -h
```
Limitar espacio
```
lxc config device set webserver root size 5GB
```
Límites de escritura y lectura
```
lxc config device set webserver root limits.read 30MB
lxc config device set webserver root limits.write 10MB
```
RED
Limites por medio del profile
```
lxc profile device set default eth0 limits.ingress 100Mbit
lxc profile device set default eth0 limits.egress 100Mbit
```
## Profiles
La asignación de recursos de forma manual para cada contenedor es una tarea mecánica y morosa. En tal sentido, usar profiles resulta más eficiente para la asignación de recursos y creación de contenedores con características particulares.

En efecto, se pueden tener diferentes profiles donde cada uno podría ser usado para un determinado tipo de contenedor considerando los recursos que pudiera necesitar. 

Para ver el listado de los profiles y el detalle de alguno ejecutamos el comando siguiente.
```
lxc profile list
lxc profile show default
```
La creación de un profile y su posterior personalización es realizada de la siguiente manera donde se pueden delimitar y especificar los recursos de la misma manera que se hizo en la sección anterior. 
```
lxc profile create perfil01
lxc profile set perfil01 limits.cpu 1
```
Los profiles deben ser asociados para aplicar las configuraciones establecidas o desasociados en caso de querer quitar las mimas.

Asociar 
```
lxc profile assign webserver perfil01
```
Desasociar
```
lxc profile remove webserver default
```
Así mismo, la red como el storage puede ser asociado a un profile en particular de modo tal que se tenga una personalización más completa y controlada.

Asociar una red.
```
lxc network attach-profile testbr0 perfil01 eth1
```
Desasociar red.
```
lxc network detach-profile testbr0 perfil01
```
Asociar un storage.
```
lxc storage volume attach-profile lxd vol10 perfil01 data /opt/data
```
Desasociar storage.
```
lxc storage volume detach-profile lxd vol10 perfil01
```
Por otro lado,  en profile o varios profiles pueden ser aplicados al momento de crear un contenedor.  
Asociar varios profiles
```
lxc init centos/7 webserver03 --profile default --profile perfil01
```
Crear y ejecutar un contenedor con cierto profile
```
lxc launch images:centos/7 webserver04 --profile perfil01
```

## Snapshot
La creación de snapshot permite guardar una copia del estado actual del contenedor en un determinado instante. En tal sentido, es útil al momento de establecer configuraciones iniciales o registrar ciertos hitos en ciclo de vida de los contenedores.

Listar snapshots
```
lxc snapshot list
```
Creación de un snapshot
```
lxc snapshot webserver
```
Creación de un snapshot estableciendo el nombre
```
lxc snapshot webserver snap-20180827
```
Restaurar
```
lxc restore webserver snap-20180827
```
Renombrar
```
lxc move webserver/snap-20180827 webserver/snap-01
```
Eliminar
```
lxc delete websever/snap-20180827
```
Crear un contenedor desde un snapshot
```
lxc copy webserver/snap-20180827 webserver100
```
## Conexiones Remotas
Para habilitar conexiones remotas inicialmente es necesario habilitar el puerto 8443 en el equipo remoto y si fuera requerido limitar el segmento al cual los clientes deberán pertenecer para poder acceder. En este ejemplo solo se habilita el puerto para que cualquier cliente en la misma red pueda ingresar.
```
lxc config set core.https_address "[::]:8443"
```
Se establece una clave en el equipo remoto, este será requerido posteriormente por el equipo cliente.
```
lxc config set core.trust_password password
```
En el equipo cliente, se procede a registrar el equipo remoto nombrando e introduciendo la IP o el dominio que tiene el mismo. 
```
lxc remote add mirepodeimagenes 192.168.100.110
lxc remote add mirepodeimagenes images.mirepodeimagenes.org
```
Listado de repositorios
```
lxc remote list
```
Una vez ya establecida la conexión con el contenedor remoto, es posible realizar y ejecutar la gestión de contenedores desde el cliente. Para ello, la ejecución de los comandos básicos, el manejo de redes, storage, snapshots, profiles ya es posible en el equipo remoto desde el cliente.

Listado de imágenes.
```
lxc image list mirepodeimagenes:
```
Crear un contenedor en el cliente utilizando el repositorio de imágenes del contenedor remoto.
```
lxc launch mirepodeimagenes:centos-7 dnsserver
```
Listado de  los contenedores del equipo remoto
```
lxc list mirepodeimagenes:
```
Creación de un nuevo contenedor de forma remota.
```
lxc launch mirepodeimagenes:centos-7 mirepodeimagenes:printserver
```
Detener un contenedor remoto.
```
lxc stop mirepodeimagenes:printserver
```








