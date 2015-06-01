## Instalación de Gnu/Linux LVM
###¿Qué es LVM?

LVM es el acrónimo de Logical volume managent, es a una forma de asignar espacio de forma más flexible que las formas tradicionales como el particionado. En particular un volume manager puede concatenar, dividir o combinar particiones en otras virtuales más grandes que los administradores pueden redimensionar o mover, potencialmente sin ni siquiera interrumpir su uso. También permite la administración de volúmenes definidos por grupos de usuarios, otorgándole al administrador del sistema lidiar con grupos de volúmenes con nombres más sensibles como “desarrollo” o “sistema” en vez de nombres de discos físicos que poco nos dicen como “sda” y “sdb”.

### Arquitectura del computador

Soporte de virtualización de hadware, tambien se debe saber el tipo de procesador que se tiene: alfha, i386, PowerPC, SPARC, amd64, etc.

### Sistema de paquetes
Un sistema de gestión de paquetes, también conocido como gestor de paquetes, es una colección de herramientas que sirven para automatizar el proceso de instalación, actualización, configuración y eliminación de paquetes de software.

* deb --> debian

* RPM --> RED HAT

* tgz --> Slacware

### Distribución Gnu/ Linux Debian
Debian es una organización formada totalmente por voluntarios dedicada a desarrollar software libre y promocionar los ideales de la comunidad del software libre.

### Filesystem Hierarchy Standard - Estándar de Jerarquía de Sistema de Archivos FHS.
Éste estándar asume que el sistema operativo entiende y soporta las mismas especificaciones de seguridad que se puede encontrar en la mayoría de los sistemas de archivos de sistemas tipo UNIX, como Linux y BSD.

### El Sistema de Archivos

En un sistema que cumpla con las especificaciones del FHS, los directorios del sistema de archivos raíz o “/”, deben ser suficientes para arrancar, reparar y/o recuperar el sistema.

Los siguientes directorios son necesarios en el directorio raíz o “/”:

    bin: Directorio que contiene los binarios (programas) de comandos escenciales.
    boot: Directorio que contiene los archivos estaticos del cargador de arranque.
    dev: Directorio que contiene “archivos de dispositivos”, los que son enlaces simbolicos a perifericos.
    etc: Directorio que contiene archivos de configuración especificos del sistema.
    lib: Directorio que contiene Librerias compartidas y modulos del kernel escenciales para el funcionamiento del sistema.
    media: Punto de montaje para dispositivos removibles (pendrives, etc.)
    mnt: Directorio que sirve como base para montar sistemas de archivos temporalmente.
    opt: Directorio donde se instalarán los archivos de las aplicaciones o paquetes agregados.
    sbin: Directorio de programas (binarios) escenciales del sistema.
    srv: Datos para servicios provistos por el sistema.
    tmp: Directorio para archivos temporales.
    usr: Jerarquía Secundaria. contiene la mayoría de las utilidades y aplicaciones multiusuario. En otras palabras, contiene los archivos compartidos de solo lectura. Este directorio puede incluso ser compartido con otros computadores.
    var: Directorio para almacenar datos variables como registros de bitacoras (logs), etc.

Otros directorios que se encuentran en el sistema son:

    home: Directorio que contiene los documentos y configuraciones de cada usuario.
    root: Directorio “home” para el administrador del sistema.
    proc: Sistema de archivos virtuales de información de procesos y el kernel. El contenido de este directorio es creado cada vez que el sistema es iniciado, y no existe en el disco duro.
    Lost+Found: (Perdidos-y-encontrados). Este directorio lo crea el sistema de ficheros ext2 para poder tener una espacio temporal donde poner los archivos que recupera después de una caida del sistema y su consiguiente “e2fsck” (verificación del disco).
    
### Instalación de LVM en la inicialización de un sistema Gnu/Linux Debian versión 7.0 Wheezy

Particion de 4 paticiones:

    /swap 

    /raiz

    /home

    /datos



Al instalar un sistema basado en una distribución GNU/Linux Debian, nos encontramos con las siguientes opciones después de haber seleccionado idioma, teclado, etc…

    Guiado – Utilizar todo el disco.
    Guiado – Utilizar el disco completo y configurar LVM.
    Guiado – Utilizar todo el disco y configurar LVM cifrado.
    Manual

Elegimos opcion **Manual**.


Crear nueva partición

```
/swap = 2G
```

**Pasos:**

Primaria

principio

area de intercambio

Crear nueva partición

```
/raiz = 9G
```

Crear nueva partición

```
/home = 5G
```

Crear nueva partición


```
/datos = 5G
```

Punto de montaje --> Introducir manualmente: /datos

