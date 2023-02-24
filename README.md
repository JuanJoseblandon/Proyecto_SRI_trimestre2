# Proyecto_SRI_trimestre2
Proyecto del segundo trimestre del modulo SRI relacionado con servidores y dockers
Práctica 1- Servidor alojamiento web
Se pide las instalación, configuración y puesta en marcha de un servidor que ofrezca servicio de alojamiento web configurable:

- Se dará alojamiento a páginas web tanto estáticas como dinámicas con “php”
- Los clientes dispondrán de un directorio de usuario con una página web por defecto.
- Además contarán con una base de datos sql que podrán administrar con phpmyadmin]
- Los clientes podrán acceder mediante ftp para la administración de archivos configurando adecuadamente TLS
- Se habilitará el acceso mediante ssh y sftp.
- Se automatizará mediante el uso de scripts:
  - La creación de usuarios y del directorio correspondiente para el alojamiento web
  - Host virtual en apache
  - Creación de usuario del sistema para acceso a ftp, ssh …
  - Se creará un subdominio en el servidor DNS con las resolución directa e inversa
  - Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
  - Se habilitará la ejecución de aplicaciones Python con el servidor web


Ejercicios|Descripcion
----------|---------------------------------
[Alojamiento](/ProyectoSRI2/Alojamiento.md)|Instalación de la pila LAMP para el alojamiento de páginas web
[Acceso](/ProyectoSRI2/ssh_ftp_tls.md)|Instalacion y configuración de ssh, sftp y tls para el acceso remoto
[Scripts](/ProyectoSRI2/scripts.md)|Automatización mediante scripts
