---
title: 'Virtualización y KVM'
media_order: 'arquitectura-kvm.png,libvirt.png,qemu.png'
date: '10-06-2020 10:34'
publish_date: '10-06-2020 10:34'
metadata:
    Keywords: 'kvm, qemu, libvirt, virtualizacion, Virtualization '
taxonomy:
    category:
        - Linux
    tag:
        - Linux
        - Virtualizacion
---

![KVM](kvm-logo.png?classes=center-block)
Existen diversas alternativas para virtualizar sistemas operativos como VMware, VirtualBox, Hiper-V. Sin embargo, GNU/Linux va en paso mas adelante debido a que ofrece una alternativa nativa que se denomina KVM que con las herramientas adecuadas como  QEMU y Libvirt nos permiten virtualizar sistemas operativos de manera sencilla y con en alto performance.


![Virtualizacion KVM,QEMU y Libvirt](arquitectura-kvm.png?classes=center-block)
## ¿Qué es KVM?

KVM (Kernel-based Virtual Machine) es en módulo del kernel de Linux que permite utilizar características de virtualización por hardware ofrecidas por diferentes procesadores. KVM puede convertir a Linux en un hipervisor que permite que una máquina de host ejecute entornos virtuales múltiples y aislados llamados máquinas virtuales (VM) o huéspedes.

Se basa en extensiones de virtualización Intel VT-X y AMD-V. 

## ¿Qué es QEMU?

Es un emulador de máquinas virtuales genérico open source que permite emular en sistema completo, incluidos periféricos y el espacio de usuario.

QEMU tiene el poder de alcanzar casi la performance nativa debido a que ejecuta el código guest directamente en el CPU host. QEMU soporta virtualización cuando se ejecuta bajo el hipervisor Xen o a través del módulo del kernel KVM en Linux. 

Cuando se usa con KVM, QEMU puede virtualizar sistemas guest x86, PowerPC y S390, entre otros.

## ¿Que es Libvirt?
Conocida como libvirt virtualization API, es un conjunto de herramientas para interactuar con las capacidades de virtualización de las versiones recientes de Linux y otros sistemas operativos. En efecto, Libvirt soporta un gran conjunto de tecnologías entre las que se destacan:

* KVM/QEMU
* Xen (en Linux y Solaris)
* LXC
* OpenVZ
* VirtualBox
* VMware ESX y GSX
* Microsoft Hyper-V
* IBM PowerVM
* El hypervisor Parallels
* Redes virtuales usando bridging, NAT, VEPA y VN-LINK
* Almacenamiento en discos IDE/SCSI/USB, FibreChannel, LVM, iSCSI y NFS

libvirt permite hacer la administración de máquinas virtuales incluso de forma remota, manejando redes y almacenamiento con un API cliente portable para Linux, Solaris y Windows.
![Libvirt](libvirt.png?classes=center-block)

Tiene gran variedad de aplicaciones donde se destacan: virsh y virt-manager.

## Precondiciones
En en primera instancia se debe verificar si nuestro hardware soporta virtualización por lo que  determinamos si nuestra CPU es compatible con KVM. Para ello ejecutamos el comando
 ```sh
grep vmx /proc/cpuinfo
````
En caso de tener un procesador AMD ejecutamos el comando.
```sh
grep svm /proc/cpuinfo
```
O simplemente ejecutamos 
```sh
egrep -c '(svm|vmx)' /proc/cpuinfo
```
Es muy probable que nuestro hardware si sea compatible pero no haya sido habilitada esta característica desde el BIOS.

## Instalación 
En caso de ser compatible, se deben instalar los siguientes paquetes para crear y manejar nuestras máquinas virtuales.
```sh 
sudo apt install qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils libguestfs-tools genisoimage virtinst libosinfo-bin virt-viewer virt-manager
```
Habilitamos libvirt para que se ejecute desde el arranque
```sh
systemctl enable libvirtd
```
Validando la instalación 
Los siguientes comando 
```sh
kvm-ok
virt-host-validate
virsh nodeinfo
```
## Creación de maquinas virtuales
Creación de Máquinas Virtuales
Lo primero que hacemos es unir nuestro usuario al grupo KVM para no tener problemas con las tools que instalaremos y las dependencias.
```sh
adduser gonzalohk libvirtd
adduser gonzalohk kvm
```
Para iniciar las pruebas podemos utilizar imágenes en formato QEMU Copy On Write (qcow2). Para ello, se puede descargar Debian en la versión 10 desde el siguiente enlace para probar más adelante.

* https://cloud.debian.org/images/cloud/buster/20200511-260/debian-10-nocloud-amd64-20200511-260.qcow2

Una vez descargada la imagen base esta puede ser copiada y renombrada muchas veces dependiendo a la cantidad de máquinas virtuales que sean necesarias. En este ejemplo crearemos se creará una máquinas virtual Debian 10 por lo que copiamos y renombramos la imagen original.
```sh   
cp /home/gonzalohk/Downloads/debian-10-nocloud-amd64-20200511-260.qcow2 /home/gonzalohk/Documents/vm/vm-debian10.qcow2
```
Para la instalación de una máquina virtual utilizamos virt donde nombramos a la máquina virtual,  asignamos la cantidad de memoria, vcpus, introducimos la ruta y especificamos el os-variant. 
```sh
virt-install --name vm-debian10 \
--memory 2048 \
--vcpus=1 \
--disk path=/home/gonzalohk/Documents/vm/vm-debian10.qcow2 \
--os-variant debian10 \
--import \
--noautoconsole
```
En cuestión de segundos ya se tiene la máquina virtual lista para ser usada.

Para obtener el os-variant correcto podemos utilizar el siguiente comando para identificarlo.
```sh
osinfo-query os
```
Es posible obtener información de la imagen con el siguiente comando.
```sh
qemu-img info vm-debian10.qcow2
```
Para listar las máquinas virtuales podemos ejecutar.
```sh
virsh list –all
```
Lo primero que hacemos es asignar una contraseña al usuario root y quitamos características cloud que no son necesarias en este ejemplo.
```sh
virt-customize -a vm-debian10.qcow2 --root-password password:h45er2$ --uninstall cloud-init
```
Acceso 
Una vez instalada la máquina virtual se debe iniciar la misma con el siguiente comando. 
virsh start vm-debian10

También apagarlo si es que fuera necesario.
```sh
virsh shutdown vm-debian10
```
Para ingresar o conectarnos a la máquina virtual antes de instalar algún servicio ssh o similar. En un inicio podemos acceder con el siguiente comando utilizando el viewer. 
```sh
virt-viewer vm-debian10
```
Adicionalmente podemos conectarnos directamente a la consola.
```sh
virsh console vm-debian10
```
En ambos casos es posible realizar todas la operaciones que se harían en debian real como instalar paquete, configurar servicios, manipular ficheros, etc.

## Network
Para listar la redes disponibles se puede utilizar el siguiente comando.
```sh
virsh net-list --all
```
Por defecto se crea en archivo xml denominado default.xml en /etc/libvirt/qemu/networks/ en el mismo folder se pueden adicionar más redes creando otros archivos xml. 

Para crear nuevas redes podemos copiar y modificar el default.xml, una forma de hacerlo es con los siguientes comandos.
```sh
virsh net-dumpxml default > dev-network.xml
nano dev-network.xml
```
Ahora se puede configurar la nueva red, para ello debemos insertar el nombre, definir el bridge y establecer el segmento de red al cual representa. Es necesario borrar el uuid y el mac address que fueron obtenidos del archivo original.

Esta es en red isolate.
```xml
<network>
<name>dev-network</name>
<bridge name='virbr1' stp='on' delay='0'/>
<domain name='dev-network'/>
<ip address='10.11.12.1' netmask='255.255.255.0'>
<dhcp>
<range start='10.11.12.2' end='10.11.12.128'/>
</dhcp>
</ip>
</network>
```
En caso de necesitar en red NAT se debe especificar el forward mode de la siguiente manera.
```xml
<network connections='1'>
<name>dev-nat-network</name>
<forward mode='nat'>
<nat>
<port start='1024' end='65535'/>
</nat>
</forward>
<bridge name='virbr3' stp='on' delay='0'/>
<ip address='192.168.66.1' netmask='255.255.255.0'>
<dhcp>
<range start='192.168.66.2' end='192.168.66.254'/>
</dhcp>
</ip>
</network>
```
Una vez que el archivo xml fue creado, es necesario definir la red con el comando siguiente.
```sh
virsh net-define dev-network.xml
```
Se debe iniciar la red luego de definirla
```sh
virsh net-start dev-network
```
También se puede establecer que la red se autoinicie la siguiente vez de forma automática.
```sh
virsh net-autostart dev-network
```
La red ya se encuentra definida, pero debe ser asociada o acoplada a una máquina virtual. Esto es posible utilizando el siguiente comando enviando como parámetro el nombre de la máquina virtual como dominio, red virtual y para fines prácticos seleccionado en modelo virtio. Incluso es posible hacerlo en caliente con el parámetro --live.
```sh
sudo virsh attach-interface \
--domain vm-debian10 \
--source dev-network \
--type network \
--model virtio \
--config \
--live
```
De la misma forma es posible desasociar o desacoplar la interfaz creada de la máquina virtual, enviando los parámetros que identifican a la máquina virtual y mac address de la interfaz virtual.
```sh
sudo virsh detach-interface \
--domain vm-debian10 \
--type network \
--mac 52:54:00:02:25:9d \
--config \
--live
```
## Storage
Es posible añadir o acoplar unidades de disco virtuales de forma muy sencilla. Para el ejemplo se creará en disco virtual de 2Gb con el siguiente comando.
```sh
dd if=/dev/zero of=/home/gonzalohk/Documents/vm/vm-debian10-disk1.img bs=1G count=2
```
Para ver los detalles del disco creado y verificar la integridad podemos hacer uso de los comandos.
```sh
qemu-img info vm-debian10-disk1.img
qemu-img check vm-debian10-disk1.img
```
El acoplar en disco a una máquina virtual resulta sencillo debido a que solo se especifica la máquina virtual, la ruta del disco y su identificador. También es posible hacerlo en caliente. 
```sh
virsh attach-disk vm-debian10 home/gonzalohk/Documents/vm/vm-debian10-disk1.img vdb --config --live
```
El desacople se realiza de forma similar.
```sh
virsh detach-disk vm-debian10 /home/gonzalohk/Documents/vm/vm-debian10-disk1.img --config --live
```
Anteriormente se insertó en disco nuevo que necesita ser particionado, formateado y montado. Para ello, dentro de la máquina virtual ejecutamos los siguientes comandos de la siguiente manera.
```sh
lsblk -a
fdisk /dev/vdb
mkfs.ext3 /dev/vdb1
mount -t ext3 /dev/vdb1 /home/projects
```
En caso de querer desmontar el disco es necesario ejecutar en la máquina virtual.
```sh
umount /dev/vdb1
```
En aspecto interesante que solo es posible hacer en virtualización es el acople del mismo disco a dos o más máquinas virtuales simultáneamente, esto es posible con el parámetro –mode shareable 

Otra forma de acoplar el disco es editando directamente el archivo de configuración de la máquina virtual. También permite adicionar una nueva bahia PCI en el caso de que ya no se tengan disponibles.
```sh
virsh edit vm-debian10
```
Se adiciona
```xml
<disk type='file' device='disk'>
 <driver name='qemu' type='raw'/>
 <source file=' /home/gonzalohk/Documents/vm/vm-debian10-disk1.img'/>
 <target dev='vdb' bus='virtio'/>
 <address type='pci' domain='0x0000' bus='0x04' slot='0x00' function='0x1'/>
</disk>
```
## Snapshots
En aspecto importante de la virtualización es el poder que se tiene de crear snapshots de forma sencilla y constante en caso de tener que revertir cambios,

Para poder listar los snapshots creados o disponibles ejecutamos.
```sh
virsh snapshot-list vm-debian10
```
El detalle del snapshot puede ser visualizado de la siguiente manera especificando el id correspondiente.
```sh
virsh snapshot-info vm-debian10 1555695775
```
Para eliminar el snapshot se realiza una accion similar.
```sh
virsh snapshot-delete vm-debian10 1555695775
```
Para crear el snapshot de forma rápida simplemente especificamos el nombre de la máquina virtual.
```sh
virsh snapshot-create vm-debian10
```
Otra forma de crear el snapshot controlando más aspecto puede ser realizado de la siguiente manera. 
```sh
virsh snapshot-create-as vm-debian10 \
--name "Snapshot 0" \
--description"Primer snapshot" \
--atomic
```
Los snapshot puede ser visualizados con los siguientes comandos.
```sh
virsh snapshot-list vm-debian10  --parent
virsh snapshot-list vm-debian10  --tree
```
En caso de querer revertir la máquina virtual a un estado anterior procedemos a ejecutar el comando.
```sh
virsh snapshot-revert vm-debian10  --snapshotname "Snapshot 0"
```
## Templates
Cuando uno personaliza una máquina virtual con los paquetes, servicios que necesita puede convertirlo en una imagen que será considerada como template, que servirá para nuevas instalaciones más adelante o que sean compartidas para diferentes objetivos. 

Antes de crear el template o clonar la imagen actual, es muy importante limpiar la máquina virtual. Se pueden hacer la limpieza de operaciones específicas como: 
```sh
virt-sysprep --list-operation
```
Es decir, podemos ejecutar el siguiente comando y especificar las operaciones de limpieza.
```sh
virt-sysprep \
--operations ssh-hostkeys,udev-persistent-net \
-d vm-debian10
```
Por otra lado, también se puede hacer en limpieza general de la siguiente manera.
```sh
virt-sysprep -d vm-debian10
```
Finalmente, clonamos la máquina virtual creando una nueva imagen .qcow2
```sh
virt-clone --original vm-debian10--name vm-debian10-template --auto-clone
```
Virtualizar con KVM, QEMU y libvirt proporciona muchas ventajas como buena performance, personalización   que se obtiene, 
