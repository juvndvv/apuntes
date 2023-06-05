
# Unidad 7: GNU/Linux

1. [Ajustes despues de la instalacion de Ubuntu](#id1)
2. [Sistema de archivos en Linux](#id2)
3. [Comandos](#id3)
4. [Administracion de usuarios locales](#id4)
5. [Administracion de grupos locales](#id5)
6. [Servicios](#id6)
7. [Programacion de tareas](#id7)
8. [Backup](#id8)
9. [GRUB](#id9)

## Ajustes despues de la instalacion de Ubuntu <a id="id1">

### Habilitar el usuario root

```bash
sudo passwd root
```

### Cambiar el nombre al equipo

Para cambiar el nombre al equipo debemos editar el archivo */etc/hostname* y eliminamos el contenido y introducimos el nombre que queramos como host. A continuacion deberemos de cambiar el contenido */etc/hosts* que contendra algo como lo siguiente:

### Iniciar el sistema en manera texto

```bash
sudo systemctl set-defaul multi-user.target
```

Y para volver a interfaz grafica:

```bash
sudo systemctl set-default graphical.target
```

Si queremos iniciar la interfaz grafica desde la manera texto:

```bash
systemctl start graphical.target
systemctl start gdm3.service
```

## Sistema de ficheros en linux <a id="id2">

### Caracteristicas generales del sistema de ficheros de linux

#### Directorio raiz

El sistema de ficheros es la manera en la que el sistema operativo organiza los ficheros en el disco duro, gestionandolo de manera que los datos esten de forma estructurada y sin errores.

El sistema de ficheros de linux tiene una estructura jerarquica, donde el directorio principal o *directorio raiz* es el directorio */* del que cuelga toda la estructura del sistema.

Por tanto en Linux existe solo un arbol de directorios, no como en Windows que tenemos uno por particion (C:, D:, ...)

#### Los inodos o nodes-i

En Linux, a cada archivo o directorio se le asigna un numero identificador unico llamado inodo.

De esta manera, existe una tabla de inodos en la cual hay una entrada para cada inodo en la cual se guarda toda la informacion importante del fichero: propietario, grupo, permisos, tipo de fichero, data de creacion, data ultima de modificacion, direccion de los bloques de disco donde se encuentra el fichero, numero de enlaces, tamaño ,directorio al que pertenece.

Es importante recalcar que no se guarda el contenido del directorio.

### Tipos de sistemas de archivos

Linux soporta una gran variedad de sistemas de ficheros, des de sistemas basados en discos, como pueden ser **ext2, ext3, ext4, ReiserFS, XFS, JFS, UFS, ISO9660, FAT, FAT32 o NTFS** a sistemas de ficheros que sirven para comunicar equipos en la red de diferentes sistemas operativos, como **NFS** (utilizado para compartir recursos entre equipos Linux) o **SMB** (para compartir recursos entre maquinas Linux y Windows)

### Directorios importantes de linux

- ***/bin*** y ***usr/bin***: contienen la mayoria de ficheros ejecutables y comandos comunes de Linux que pueden usar todos los usuarios.

- ***/boot***: contiene los ficheros necesarios para el arranque del sistema

- ***/dev***: almacena los "ficheros especiales" que representan los dispositivos de E/S. En realidad no son "ficheros" propiamente dichos sino que es la forma en que Linux implementa los controladores de dispositivos.

- ***/etc***: contiene los ficheros de configuracion del sistema. Solo - ***root*** puede modificarlos.

- ***/home***: donde se almacena el *directorio home* o carpeta personal de cada uno de los usuarios.

- ***/lib*** y ***/usr/lib***: contienen libreries compartidas del sistema.

- ***/media***: cuando se monta automaticamente un CD_ROM, o pendrive o disco duro externo, se crea aqui su subdirectorio. PE: */media/usbdisk*

- ***/mnt***: es el directorio por defecto para realizar el montaje otros dispositivos de almacenamiento.

- ***/opt***: directorio opcional donde se pueden instalar aplicaciones, ademas de en /usr. En algunas distribuciones no existe este directorio.

- ***/root***: es el home del usuario root

- ***/sbin*** y ***/ust/sbin***: contienen comandos y ejecutables de tareas de administracion que en su mayoria solo puede usar, evidentemente, el usuario root.

- ***/usr***: almacena las aplicaciones de uso general para todos los usuarios, por lo cual, si hay muchos paquetes instalados, puede ocupar mucho espacio.

  - ***/usr/bin***: contiene programas y comandos para los usuarios

  - ***/usr/share***: datos compartidos independientes de la maquina, como documentacion de programas, imagenes para el Escritorio de Linux...

### Tipos de archivos

- **Archivos regulares**: Contienen la informacion con la cual trabaja cada usuario. Son los archivos ordinarios de datos.

- **Directorios**: Son archivos especiales que contienen referencias a otros archivos o directorios.

- **Hard links**: No es especificamente una clase de archivo sino un segundo nombre que se le da a un archivo.

- **Soft links**: Tambien se utilizan para asignar un segundo nombre a un archivo. La diferencia con los enlaces duros es que los simbolicos solo hacen referencia al nombre del archivo original, mientras que los duros hacen referencia al inodo en el cual estan situados los datos del archivo original. Seria como los "accesos directos" de Windows.

- **Archivos especiales**: Suelen representar dispositivos fisicos, como unidades de almacenamiento, impresoras, terminales, etc. En Linu, todo dispositivo fisico que se conecte al ordenador esta asociado a un archivo. Pueden ser de dos tipos: de bloques o caracteres. Un disco duro es un dispositivo de bloque y una impresora es un dispositivo de caracter.

- Otro tipos de archivo son las *cañerias (pipes) o los sockets*:

    | Indicador | Descripcion |
    | -- | -- |
    | - | Archivo ordinario |
    | d | Directorio |
    | \| | Enlace simbolico
    | b / c | Archivo especial (c=caracteres y b=bloques) |
    | p | Tuberia |
    | s | Socket |

### Ficheros y directorios ocultos

Los ficheros ocultos en Linux son aquellos que empiezan por .

### Permisos y atributos

```bash
drwxr-xr--  2 juvndv  juvndv  4096  2005-02-16 14:47  tmp
```

En la linea antrior de codigo identificamos lo siguiente:

- ***drwxr-xr--***: Permisos del archivo

  - ***d***: Tipo de archivo
  - ***rwx***: Permisos de propietario
  - ***r-x***: Permisos de grupo
  - ***r--***: Permisos de otros

- ***2***: Enlaces duros
- ***juvndv***: Propietario
- ***juvndv***: Grupo
- ***4096***: Tamaño en bytes
- ***2005-02-16 14:47***: Fecha y hora de la ultima modificacion
- ***tmp***: Nombre

#### Permisos y modificar permisos

En linux los permisos de archivos son los siguientes:

- ***r***: lectura
- ***w***: escritura
- ***x***: ejecucion

##### Modificar permisos en manera comando

```bash
chmod o|g|o|a =|+|- r|w|x fichero(s)|directorio(s)

chmod o=rwx, g=rx, o=- ej1.txt
chmod g-wx ej1.txt
```

##### Modificar permisos en manera octal

Siendo los permisos del propietario ***rw-*** se puede convertir al octal ***110*** que en decimal es *6*, si aplicamos esto a los tres campos que se le aplican los permisos, es decir, propietario, grupo y otros y queremos aplicar los siguientes permisos:

- ***p***: rwx
- ***g***: rw-
- ***o***: r-x

Convertimos cada uno a valor octal:

- ***p***: 7
- ***g***: 6
- ***o***: 5

y con el siguiente formato

```bash
chmod XXX fichero(s)|directorio(s)
```

ejecutamos el comando

```bash
chmod 765 ej1.sh
```

#### Permisos especiales

- ***SUID***: El bit SUID activo en un archivo significa que el que lo ejecute tendra los mismo permisos que el que creo el archivo.

  Ejemplo:

  ```bash
  $ ls -l /bin/el seu
  -rwsr-xr-x 1 root root 31012 2016-03-04 07.49 /bin/la seua
  ```

  ```bash
  chmod 4775 <fichero>
  chmod o+s <fichero>
  chmod o-s <fichero>
  ```

- ***SGID***: Es el mismo que el SUID pero a nivel de grupo. Es decir, todo archivo que tenga activo el SGID, en ser ejecutado, tendra los privilegios del grupo al cual pertenecen

  Ejemplo:

  ```bash
  $ ls -l
  drwxrws--- 2 mar mar 4096 2016-03-04 21.27 compartit
  ```

  ```bash
  chmod 2775 <directiorio>
  chmod g+s <directorio>
  chmod g-s <directorio>
  ```

- ***Sticky bit***: Se utiliza para permitir que cualquiera pueda escribir y modificar sobre un archivo o directorio, pero que solo su propietario o root puedan eliminarlo.

  Ejemplo:

  ```bash
  drwxr-xr-t 13 root root 4096 2016-04-24 20.55 tmp
  ```

  ```bash
  chmod 1755 <directorio>
  chmod o+t <directorio>
  chmod o-t <directorio>
  ```

## Comandos <a id="id3">

### Comandos para el manejo de la interfaz

- ***man*** o ***help***: informacion de ayuda de los comandos
- ***echo***: muestra un texto literal por la pantalla
- ***clear***: borra la pantalla
- ***exit***: salir del shell

### Comandos generales de configuracion del sistema

- ***date***: ver/poner la fecha del sistema
- ***cal***: muestra un calendario
- ***shutdown***: apaga el sistema
- ***reboot***: reinicia el sistema

### Comandos de tratamiento de unidades de disco

- ***du***: muestra un resumen del uso de disco para cada fichero o directorio, recursivo para los directorios
- ***df***: muestra informacion sobre el sistema de ficheros en el cual reside cada fichero. Por defecto, de todos los sistemas de ficheros

### Comandos de manejo de ficheros y directorios

- ***touch***: Crea un fichero vacio
- ***vi*** o ***nano***: crear o modificar un fichero
- ***cat***, ***more*** o ***less***: ver el contenido de un archivo
- ***grep***: muestra las lineas de un fichero que contienen un determinado patron
- ***head***: muestra las 10 primeras lineas de un fichero
- ***tail***: muestra las 10 ultimas lineas de un fichero
- ***wc***: muestra el numero de lineas, palabras o caracteres
- ***cut***: muestra solo ciertas columnas del fichero
- ***sort***: ordena las lineas de un fichero
- ***pwd***: muestra la ruta completa del directorio actual
- ***ls***: muestra el contenido de un directorio
- ***cd***: cambia el directorio
- ***mkdir***: crea un directorio
- ***rm***: elimina un archivo, o un directorio con la opcion -r
- ***cp***: copia un archivo
- ***mv***: cambia el nombre o mueve un archivo
- ***ln***: crea enlace a un fichero
- ***file***: determina el tipo de fichero
- ***chmod***: establece los permisos en un archivo o directorio
- ***chown***:  cambia el amo i/o grupo del fichero
- ***tar***: empaquetar ficheros y directorios
- ***gzip***: comprimir y descomprimir un archivo o directorio

### Direccionamientos

- ***comando > fichero***: crea un nuevo fichero cuyo contenido es el resultado del comando de la izquierda, si ya existia lo destruye
- ***comando >> fichero***: añade al final del fichero el resultado del comando de la izquiera
- ***comando < fichero***: introduce como datos de entrada el fichero
- ***comando 2> fichero***: envia la salida de error de comando a fichero
- ***comando &> fichero***: envia la salida estandar y de error a fichero

### Tuberias

Las tuberias permiten conectar la salida de un comando con la entrada del siguiente

***comando1 | comando2***

La salida de comando1 se introduciria como entrada a comando2. Por ejemplo

```bash
cat /etc/passwd | grep mar
```

el comando anterior mostraria por pantalla unicamente las lineas de */etc/passwd* que contuvieran la palabra mar

## Administracion de usuarios locales <a id="id4">

### El fichero /etc/passwd

El fichero */etc/passwd* almacena las cuentas de usuario del sistema, cada linea corresponde a un usuario y tiene el siguiente formato:

```bash
mar:x:1002:1002:Maria M. Soler,,,:/homre/mar:/bin/bash
```

- ***mar***: nombre de usuario
- ***x***: contraseña, una x indica que se encuentra encriptada en */etc/shadow*
- ***UID***: ID de usuario. 0 reservado para root, 1-99 para cuentas predefinidas, y 100-999 para cuentas administrtivas del sistema.
- ***GID***: ID del grupo principal del usuario
- ***Maria M. Soler,,,***: informacion del usuario (nombre, ubicacion, telefono del trabajo, de la oficina)
- ***/home/mar***: ruta de la carpeta personal
- ***/bin/bash***: shell

### El fichero /etc/shadow

Para mayor seguridad en el fichero ***/etc/shadow*** no aparecen las contraseñas de los usuarios del sistema, estas se almacenan cifradas. Es propiedad del usuario root para que ningun usuario pueda ver su contenido. Cada linea corresponde a un usuario y tiene el siguiente formato:

```bash
mar:$6$nBqT6u6q$dT6KCw5WDXPXZOkSWJtSFgYLTulkNb/pqxmUk9SkuIQah8o8radr/kT7k9.y7niIigtlcjTYYmaASbkNtR5mA0:19417:0:99999:7:::
```

- ***mar***: nombre de usuario
- ***$6$nBqT6u6q$dT6KCw5WD...***: contraseña encriptada. Si comienza por el caracter ! indica que la cuenta esta bloqueada
- ***19417***: dias que han pasado desde la ultima vez que la contraseña fue cambiada contados desde el 1 de enero de 1970
- ***0***: dias que deben pasar como minimo para que el usuario pueda cambiar la contraseña
- ***99999***: dias durante los que la contraseña es valida
- ***7***: dias a los que el usuario sera avisado de que debe cambiar la contraseña antes de que esta caduque
- ***[]***: dias a los que se deshabilita la cuenta despues de que caduque la contraseña
- ***[]***: dias a los que se deshabilita la cuenta contados desde el 1 de enero de 1970

### El directorio /etc/skel

En este directorio se encuentra los ficheros de perfiles: *.bash_logout*, *.bashrc* y *.profile*. Cuando se crea un nuevo usuario se copian en su directorio home estos tres ficheros.

- ***.bash_logout***: se ejecuta al finalizar la sesion
- ***.basrc***: se ejecuta cuando se invoca un nuevo shell
- ***.profile***: se ejecuta cuando el usuario inicia sesion en el sistema

### Creacion de usuarios con comandos

#### **sudo useradd [-opciones] login**

Opciones:

- ***-c***: infica el nombre completo del usuario
- ***-d***: indica el directorio home del usuario
- ***-m***: indica que se cree el directorio home que hemos escrito en -d
- ***-s***: indica el shell
- ***-g***: indica el grupo

### Eliminacion de usuarios con comandos

#### sudo userdel [-opciones] login

Opciones:

- ***-r***: elimina el directorio home del usuario
- ***-f***: elimna la cuenta de usuario aunque este conectado al sistema

### Cambiar la contraseña de un usuario con comandos

#### sudo passwd login

### Modificar cuentas de usuario

#### usermod [-opciones] login

Opciones:

- ***-c***: nuevo comentario del campo GECOS
- ***-d***: nuevo directorio home
- ***-m***: mueve el contenido del directorio home al nuevo directorio home indicado en -d
- ***-e***: establece fecha de expiracion
- ***-f***: establece contraseña inactiva despues de expirar
- ***-g***: fuerza el GID como grupo principal
- ***-G***: nueva lista de grupos complementarios
- ***-a***: añade al usuario grupos suplementarios
- ***-l***: nuevo valor del login name
- ***-L***: bloquea la cuenta del usuario
- ***-U***: desbloquea la cuenta
- ***-p***: usa password encriptada como nuevo password
- ***-s***: nuevo shell
- ***-u***: nuevo UID

## Administracion de grupos locales <a id="id5">

### El fichero /etc/group

Los grupos que hay en el sistema se almacenan en el archivo de texto ***/etc/shadow***, en el cual podemos ver los diferentes grupos del sistema asi como los usuarios que pertenecen a cada grupo.

```bash
sudo:x:27:mar,eva,david
```

- ***sudo***: nombre del grupo
- ***x***: contraseña del grupo
- ***27***: ID del grupo
- ***mar,eva,david***: usuarios miembros del grupo

#### Crear nuevos grupos

##### sudo addgroup grupo

#### Eliminar grupos

##### sudo groupdel grupo

#### Añadir un usuario a un grupo

##### usermod [-opciones] login (grupos)

Opciones:

- ***-G***: indicamos los grupos a los cuales va a pertencer el usuario
- ***-a***: adicion de grupos a la lista de grupos del usuario

## Servicios <a id="id6">

Los servicios son procesos que se ejecutan en segundo plano a la espera de ser llamados por los usuarios para ofrecerle cierta funcion

Algunos ejemplos: ***acpid, anacron, atd, cron, cups, dbus, dhcpd, httpd, named, netfs, networking, network-manager, nfs, smb, sshd, udev***

### Manejo de servicios en manera comando

Los scripts que se encargan de arrancar y parar los servicios se encuentran en el directorio ***/etc/init.d.***.

Los parametros que se le suelen pasar, dependiendo del script, pero normalmente son:

- ***start***: arrancar servicio
- ***stop***: parar servicio
- ***restart***: reniciar servicio
- ***status***: ver el estado del servicio

Por ejemplo, para comprobar el estado del servicio cron y arrancarlo si no lo estubiera gastariamos los siguientes comandos:

```bash
sudo service cron status
sudo service cron start
```

## Programacion de tareas <a id="id7">

### El comando at

En algunas ocasiones necesitamos ejecutar una tarea en un momento en particular y no con frecuencia. Para este caso utilizamos el comando ***at***. Por defecto en Ubuntu no viene instalado.

Ejemplos:

- Apagar el sistema hoy a las 11:55pm

  ```bash
  at -f /sbin/shutdown 11:55 pm today
  ```

- Ejecutar el scrip hola.sh la semana que viene a las 2:00

  ```bash
  at -f hola.sh 2:00 next week
  ```

- Listar las tareas

  ```bash
  at -l
  ```

- Eliminar una tarea

  ```bash
  at -d id
  ```

### El comando cron

## Backup <a id="id8">

## GRUB <a id="id9">
