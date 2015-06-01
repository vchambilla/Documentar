## Configuración de Red por línea de comando

Para imprimir las listas de información detallada sobre todos los buses PCI y dispositivos del sistema, es el comando **lspci**.

```
lscpi | grep -i network
```

```
lscpi | grep -i ethernet
```

```
lscpi | grep -i wireles
```

### Configuración de Red

* *Configuración modo gráfico*

```
gnome-control-center network
```

Para cambiar/añadir/modificar los repositorios ingresar a:

```
nano /etc/apt/sources.list
```

*Añadir un usuario a un grupo*

```
adduser nombre_usuario sudo
```

*Ver los grupos al que se añadio el usuario*

```
groups nombre_usuario
```

* *Configuración modo consola*

Para configurar la Red en modo consola entrar a la dirección:

```
nano /etc/network/interfaces
```

**Configuración en modo dinámico (DHCP):**

	auto lo
	iface lo inet loopback
	auto eth0
	iface eth0 inet dhcp

Luego reiniciar el servicio con:

	#service networking restart
	
o con este comando:

	#/etc/init.d/networking restart 

Para ver las interfaces de red:

	ls /sys/class/net

Para ver si tenemos una IP:

	#ip addr show

Para ver más especifico, con el siguiente comando:

	#ip addr show eth0

otro comando

	#ifconfig

**Configuración en modo estatico:**

Entrar al archivo "interfaces"

	#nano /etc/network/interfaces

Y añadir las siguientes lineas:

	auto eth0
	iface eth0
		address 192.168.3.12
		netmask 255.255.255.0
		gateway 192.168.3.1

Para crear una red virtual, en la maquina virtual ir a la pestaña 

"localhost (qemu)" --> click derecho --> details --> al icono del más (**+**)

	Nombre: egpp
	red: 192.168.3.0/24
	Habilitar DHCP: deshabilitar
	Red fisica: NAT
	Finalizar

Luego ir a la maquina virtual, en el icono del foquito: ir a la interface de red virtual NIC:

Elegir la red virtual que creamos:

	Dispositivo fuente: egpp
	Modo de dispositivo: (defecto)

Guardar.

**Configuración del DNS:**

entrar al achivo "resolv.conf".

	$nano /etc/resolv.conf

Añadir las siguientes lineas.

	nameserver 192.168.3.1  //gateway de la red que creamos egpp.
