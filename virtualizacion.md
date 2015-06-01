##Virtualización de servidores

Tipos de virtualización completa y ligera.

### Virtualización completa

* vmware
* Microsoft
* Xen
* KVM
* VirtualBox

### Virtualización ligera

* vserver
* LXC
* OpenVZ
* Docker

#### KVM

Kernel-based Virtual Machine o KVM, (Máquina virtual basada en el núcleo) es una solución para implementar virtualización completa con Linux. Está formada por un módulo del núcleo (con el nombre kvm.ko) y herramientas en el espacio de usuario, siendo en su totalidad software libre.

Para saber que si la máquina soporta la virtualización, probar los siguientes comandos:

*Procesador INTEL*

```
grep vmx /proc/cpuinfo
```

*Procesador AMD*

```
grep svm /proc/cpuinfo
```

**Instalar KVM**

	$apt-get install qemu-kvm libvirt-bin virtinst virt-viewer

Para administrar maquinas virtuales en modo grafico instalar virt-manager:
	
	$apt-get install virt-manager

Adicionar el usuario a kvm

	$adduser user kvm
	$adduser user libvirt

Luego reiniciar la máquina (Laptop)

**En el Virtual Manager**

Click derecho --> detalles

Almacenamiento --> en el icono más +

	Nombre: maquinas virtuales
	tipo: qcow2
	ruta: explorar (buscar nuestra carpeta datos) 	

luego click en maquinas virtuales --> **nuevo volumen**
	
	Nombre: lvm1
	maxima cpacidad: 3GB
	finalizr

Creación de una maquina virtual

	Abrir --> nuevo
	medio de instacion local: ISO
		Linux
		Debian Wheezy
	Memoria RAM: 1024
	CPU:1
	Elegir administrado o algun otro tipo de almacenamiento existente
		Explorar... (elegir maquinas virtules: lvm1.qcow2)
		Nombre: vm-lvm1
	Elegir idiomas: Español
	Opciones avanzadas
	Red: NAT (inactivo)
	Finalizar.

**Tipos de particiones :**

* ** Particiones primarias:** Este tipo de partición es importante en cuanto nos permite arrancar desde aquí nuestro o nuestros sistemas operativos. Es lo que se conoce como una partición booteable. (Cada partición primaria sería un Sistema Operativo diferente).

* **Extendida:** Este tipo de partición sirve sólo como almacén de datos, ya que no es booteable. No podemos instalar en una partición de este estilo ningún Sistema Operativo. Sin embargo tiene una ventaja y que este tipo de particiones las podemos dividir en todas las partes que queramos.

* **Lógica:** Las particiones de este tipo son las particiones que podemos hacer dentro de una partición extendida. Y su límite lo impone el mismo tamaño de la partición extendida.

**Instalación de 3 particiones primarias y 2 particiones lógicas LVM **

Configuración de particiones primarias **/boot, /swap, /raiz** con los siguientes pasos:

	Spanish  //elegir idioma
	Bolivia	 //elegir region
	Teclado	 //elegir el idioma del teclado
	Nombre de la maquina: debian
	Nombre de dominio: (vacio)
	Contraseña de superadministrador: *******
	usuario sin privilegios: user
	contraseña: ********
	
Método Particionado **/boot**

	Manual
	Partición guiado
	Disco duro virtual
	Si
	Espacio libre
	Crear nueva partición
		256 MB   // para boot
		primaria
		principio
	Configuración de partición
		Sistema de ficheros ext2   // es igual de fat32
		Punto de montaje: /boot
		Finalizar partición

Método Particionado: Memoria virtual **/swap**

	Espacio libre
	Nuevo tamaño: 512 Mb
	Tipo: Logica
	Principio
	Tipo de fichero: área de intercambio
	Finalizar

Método Particionado: Memoria virtual **/raiz**

	Espacio libre
	Nuevo tamaño: 1Gb
	Tipo: Logica
	Sistema de ficheros ext4
	Punto de montaje: /raiz
	Finalizar

Configuración de Volumenes Lógicos LVM **/home, /tmp** con los siguientes pasos:

	Configurar como (LVM)
		Guardar cambios: Si
		Resumen de la configuracion LVM actual:
			- Crear grupo de volumen: SISTEMAS
				Dispositivo para el nuevo volumen: (Elegir donde esta vacio)
				Guardar los cambios: Si
			- Crear volumen logico
				grupo: SISTEMAS
				Nombre: home
				Tamaño: 500 Mb
			- Crear volumen logico
				grupo: SISTEMAS
				Nombre: tmp
				Tamaño: 200 Mb
	Terminar.

	Configurar los volumenes logicos creados:
	
		Seleccionar: home
			Sistema de ficheros: etx4
			Punto de montaje: /home
		Terminar.
		
		Seleccionar: tmp
			Sistema de ficheros: etx4
			Punto de montaje: /tmp
		Terminar.

	Finalizar el particionamiento y escribir los cambios en el disco
		Si
		Instalando . . .

* *Ver las particiones en consola***Instalación de 3 particiones primarias y 2 particiones lógicas LVM **

```
$df -h
```

* *Ver información del grupo LVM  en consola*

```
$vgdisplay
```

* *Ver información de los volumenes logicos LVM  en consola*

**Redimencionar un archivo lógico **

Con el comando *lvextend* al archivo tmp, le aumentaremos 100 Mb más:

	$lvextend -L+100 /dev/mapper/sistemas-tmp
	$lvs  //nos muestra el nuevo tamaño

Redimencionar el sistema de archivos */tmp*

Primero desmontar el archivo:
	
	$umount /tmp		// (dev/mapper/sistemas-tmp)
	$df -h

Verificar si esta correctamente el sistema de archivos

	$e2fsck -f /dev/mapper/sistemas-tmp

Redimencionar el sistema de archivos "tmp"

	$resize2fs /dev/mapper/sistemas-tmp

Montar el sistema de archivos

	$mount /tmp
	$df -h

Redimencionar para reducir el tamaño del sistema de archivos /home

	umont  /dev/mapper/sistemas-home	//desmontar el archivo
	e2fsk -f /dev/mapper/sistemas-home
	resize2fs /dev/mapper/sistemas-home 	100M
	mount /home								//montar el archivo
	lvreduce -L100M /dev/sistemas/home 	//*-L100M= es el tamaño que se quiere tener.

**Administrando la maquina virtual **

En la maquina virtual --> localhost (QEMU) --> click derecho: *Detalles*
	
	Ir a la pestaña de almacenamiento --> Nuevo Volumen
		Nombre: hdaux
		Formato: qcow2
		Maxima capacidad: 2 Gb
	Luego entrar al foquito de la maquina virtual --> click derecho
		Agregar hardware --> storage --> o Elija administrado
		Explorar . . . // y elegir el volumen que creamos
		Tipo: VirtioDisk
		Formato almacenaje: raw

	Finalizar...

Iniciar la maquina virtual.
	
	$ df -h

**Particionar y dar formato al nuevo volumen creado**
	$fdisk -l  | more
	$cfdisk	/dev/vdb

Nos muestra una pantalla para particionar
	New
	primary
	size (in Mb): 1024
	Begging
	write: Yes
	Quit
	Reiniciar la maquina virtual

Para ver el disco particionado

	$fdisk -l /dev/sda1

Para crear grupos y volumenes logicos

	$pvcreate /dev/sda1

Crear nuevo volumen de grupo
	
	$vgcreate grupoA /dev/sda1
	$pvdisplay

Nuevo volumen para el grupoA

	$lvcreate -L 500m -n volumen01 grupoA
	$lvs

Para dar formato al volumen creado

	$mkfs.ext3 -m 0 /dev/mapper/grupoA-volumen01

Montar la particion

	$mkdir /mnt/volumen-01
	$mount /dev/mapper/grupoA-volumen01 	/mnt/volumen-01
	$df -h

Para que se inicie automaticamente el volumen creado, despues de reiniciar editar el archivo *fstab*

	$nano	 /etc/fstab

dentro del archivo, añadir la linea debajo de:

	/dev/mapper/sistemas-tmp 			/tmp					ext4	default

Añadir la linea:

	/dev/mapper/grupoA-volumen01 	/mnt/volumen-01		ext3	default

Luego reiniciar la maquina virtual.

**Extender el tamaño al Disco Duro**

Fuera de la maquina virtual, entrar a la carpeta donde se creo la maquina virtual creada. Le añadiremos 7Gb más.

	$cd /datos
	$sudo qemu-img resize lvm1.qcow2 	+7G

Reiniciar el servicio *libvirt*

	$ sudo service libvirt-bin restart

Iniciar la maquina virtual y verificar con *cfdisk /dev/sda*

* **Agregar un CD-ROM a la maquina virtual**
	
En la maquina virtual --> localhost (QEMU) --> click derecho: *Detalles*
	
	Luego entrar al foquito de la maquina virtual --> click derecho
		Agregar hardware --> storage -->  Elija administrado
		Explorar . . . // y elegir el ISO del gparted.iso
		Tipo de dispositivo: Dispositivo CDROM
	finalizar

	Luego ir a "Boot Option" --> Orden de los dispositivos de arranque:
		Habilitar: IDE CDROM 1
		con la fechas de ariba y abajo: subirlo arriba

Iniciar la maquina virtual:

Nos mostrara el inicio de *gparted.iso*

	Don't touch keymap
	spanish: 25
	0 (Enter)

Muestra en modo grafico, las particiones

	Seleccionar: "extended"
	Luego clik redimencionar --> con las flechitas extenderlo hasta el tope.
	Aplicar.
	Salir

Iniciar la maquina virtual y revisar si se extendio el disco duro:
	
	$fdisk -l /dev/sda	//nos muestra el tamaño extendido.

	



		


