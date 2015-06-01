##Administración de archivos y permisos##

Linux es un sistema multiusuario por lo que necesita proveer un sistema de permisos para controlar el conjunto de operaciones autorizadas sobre archivos y directorios, lo que incluye todos los recursos del sistema y los dispositivos.

Cada archivo o directorio tiene permisos específicos para tres categorías de usuarios:

    dueño (representado con u por «usuario»);
    grupo dueño (representado con g por «grupo»), que incluye a todos los miembros del grupo;
    y los demás (representado con o por «otros»). 

Puede combinar tres tipos de permisos:

    lectura (representado por r por «read»: leer);
    escritura (o modificación, representado con w por «write»: escribir);
    ejecución (representado con x por «eXecute»: ejecutar). 

El programa **ls** por defecto presenta los nombres de archivos que no comienzan con el caracter '.' y que están en el directorio de trabajo del usuario. 

Comando para administrar archivos, con una breve descripción y unas pocas opciones:

	ln [-s] fuente destino

Crea un nuevo enlace para el archivo referenciado por fuente pero con el nombre destino. Por ejemplo si en el directorio de trabajo hay un archivo enlazado con el nombre carta.txt puede crearse otro enlace llamado diario.txt con:

	mkdir [-p] ruta

Crea un directorio con la ruta especificada. Con la opción -p el comando mkdir creará todos los directorios necesarios para construir la ruta. 

	mv fuente destino

Renombra o mueve el archivo fuente en la localización/nombre destino. 

	rm [-i] [-rf] archivo

Borra un archivo. Una vez se borra un archivo no hay un método sencillo para recuperarlo.

	cp [-rf] fuente destino

Copia del archivo fuente al destino especificado. Si se usa la opción -rf también se copiaran subdirectorios. 

	df [-h]

Para examinar espacio disponible en los dispositivos de almacenamiento, en particular el de las particiones cuyos sistemas de archivos estén montados. Puede emplearse con la opción -h para obtener datos en unidades más conocidas (en Kilobytes, Megabytes y Gigabytes) [14]. 

	du [-s] [ruta [ruta] ... ]

Para examinar espacio empleado por cada una de las ruta y sus archivos y subdirectorios.

###Tipos de permisos###

En los Sistemas Unix, la gestión de los permisos que los usuarios y los grupos de usuarios tienen sobre los archivos y las carpetas, se realiza mediante un sencillo esquema de tres tipos de permisos que son:

    Permiso de lectura 	   = 	r	4
    Permiso de escritura =  w 	2
    Permiso de ejecución = x	1

* **Permiso de lectura**

Cuando un usuario tiene permiso de lectura de un archivo significa que puede leerlo o visualizarlo, bien sea con una aplicación o mediante comandos. Ejemplo, si tenemos permiso de lectura sobre el archivo examen.txt, significa que podemos ver el contenido del archivo. Si el usuario no tiene permiso de lectura, no podrá ver el contenido del archivo.

Cuando un usuario tiene permiso de lectura de una carpeta, significa que puede visualizar el contenido de la carpeta, es decir, puede ver los archivos y carpetas que contiene.

 El permiso de lectura se simboliza con la letra **'r'** del inglés 'read'.

* **Permiso de escritura**

Cuando un usuario tiene permiso de escritura sobre un archivo significa que puede modificar su contenido, e incluso borrarlo. También le da derecho a cambiar los permisos del archivo mediante el comando chmod así como cambiar su propietario y el grupo propietario mediante el comando chown. Si el usuario no tiene permiso de escritura, no podrá modificar el contenido del archivo.

Cuando un usuario tiene permiso de escritura sobre una carpeta, significa que puede modificar el contenido de la carpeta, es decir, puede crear y eliminar archivos y otras carpetas dentro de ella. Si el usuario no tiene permiso de escritura sobre la carpeta, no podrá crear ni eliminar archivos ni carpetas dentro de ella.

El permiso de escritura se simboliza con la letra **'w'** del inglés 'write'.

* **Permiso de ejecución**

Cuando un usuario tiene permiso de ejecución de un archivo significa que puede ejecutarlo. Si el usuario no dispone de permiso de ejecución, no podrá ejecutarlo aunque sea una aplicación.

Los únicos archivos ejecutables son las aplicaciones y los archivos de comandos (scripts). Si tratamos de ejecutar un archivo no ejecutable, dará errores.

Cuando un usuario tiene permiso de ejecución sobre una carpeta, significa que puede entrar en ella, bien sea con el comando 'cd' o con un explorador de archivos como Konqueror. Si no dispone del permiso de ejecución significa que no puede ir a dicha carpeta.

El permiso de ejecución se simboliza con la letra **'x'** del inglés 'eXecute'. 

###Cambio de permisos###

Para cambiar los permisos de un archivo o una carpeta es necesario disponer del permiso de escritura (w) sobre dicho archivo o carpeta. Para hacerlo, se utiliza el comando chmod. La sintaxis del comando chmod es la siguiente:

    chmod [opciones] permiso nombre_archivo_o_carpeta 

Los permisos se pueden representar de dos formas. La primera es mediante las iniciales de a quién va dirigido el permiso (usuario=u, grupo=g, resto=o (other)), seguido de un signo + si se quiere añadir permiso o un signo - si se quiere quitar y seguido del tipo de permiso (lectura=r, escritura=w y ejecución=x). Ejemplos:

// Dar permiso de escritura al usuario propietario sobre el archivo 'examen.txt'

	$ chmod u+w examen.txt

// Quitar permiso de escritura al resto de usuarios sobre el archivo 'examen.txt'

	$ chmod o-w examen.txt

// Dar permiso de ejecución al grupo propietario sobre el archivo '/home/examen.txt'

	$ chmod g+x /home/examen.txt

// Dar permiso de lectura al grupo propietario sobre el archivo 'examen.txt'

	$ chmod g+r examen.txt

// Se pueden poner varios permisos juntos separados por comas
	
	$ chmod u+w,g-r,o-r examen.txt

// Se pueden poner varios usuarios juntos

	$ chmod ug+w examen.txt

La segunda forma de representar los permisos es mediante un código numérico cuya transformación al binario representaría la activación o desactivación de los permisos. El código numérico está compuesto por tres cifras entre 0 y 7. La primera de ellas representaría los permisos del usuario propietario, la segunda los del grupo propietario y la tercera los del resto de usuarios.

	Cód	    Binario     Permisos efectivos

	0       0 0 0       - - -
	1       0 0 1       - - x
	2       0 1 0       - w -
	3       0 1 1       - w x
	4       1 0 0       r - -
	5       1 0 1       r - x
	6       1 1 0       r w -
	7       1 1 1       r w x 

 Si deseamos otorgar sólo permiso de lectura, el código a utilizar es el 4. Si deseamos otorgar sólo permiso de lectura y ejecución, el código es el 5. Si deseamos otorgar sólo permiso de lectura y escritura, el código es el 6. Si deseamos otorgar todos los permisos, el código es el 7. Si deseamos quitar todos los permisos, el código es el 0. Ejemplos:

    // Dar todos los permisos al usuario y ninguno ni al grupo ni al resto
    chmod 700 examen.txt

    // Dar al usuario y al grupo permisos de lectura y ejecución y ninguno al resto
    chmod 550 examen.txt

    // Dar todos los permisos al usuario y lectura y ejecución al grupo y al resto
    chmod 755 /usr/bin/games/tetris

    // Dar todos los permisos al usuario y de lectura al resto, sobre todos los archivos
    chmod 744 *

    // Cambiar permisos a todos los archivos incluyendo subcarpetas
    chmod -R 744 *

###Creación de usuarios###

	$useradd usuario1
	$passwd usuario

Ver usuarios que estan conectados con:
	
	$getent passwd

Cambiar de usuario

	$sudo du - usuario1

Otra forma de crear un usuario, modo más completo

	$adduser usuario2