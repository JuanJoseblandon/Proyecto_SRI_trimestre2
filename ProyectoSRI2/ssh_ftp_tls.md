# Instalación de openssh server
Para poder proporcionar a los usuarios el acceso atraves de ssh debemos instalar openssh server que proporciona el servicio ssh de una forma segura.
```bash
$ sudo apt install openssh-server
```
Tras instalarlo debemos modificar el fichero sshd_config
```bash
$ sudo nano /etc/ssh/sshd_config
``` 

En el debemos buscar la sección que especifica el purto con el que trabaja, aqui debemos descomentar la linea para que empiece a trabajar por el 
![image](https://user-images.githubusercontent.com/91255763/220370469-02ef85ff-cd84-44e3-9ac8-c2a8f96bd27a.png)
Después de esto reiniciamos el servicio ssh con
```bash
$ sudo service ssh restart
```
Ahora el servicio ssh ya esta operativo para probarlo solo sebemos introducir el comando
```bash
$ ssh nombre_de_usuario@ip
```
![image](https://user-images.githubusercontent.com/91255763/220383509-af84181a-8535-45fc-b277-67b420022ea6.png)
![image](https://user-images.githubusercontent.com/91255763/220383814-44787581-bdc5-427d-82d2-60d577399b2e.png)

# Instalación de vsftp
Este servicio provee un acceso fpt seguropara todos los usuarios. 
Primero debemos instalar e servicio vsftpd con
```bash 
$ sudo apt install vsftpd
```
Una vez instalado el servicio debemos acceder al archivo de configurtacion del servicio con
```bash 
$ sudo nano /etc/vsftpd.conf
```
En este archivo debemos buscar y modificar las siguientes lineas
  - anonymous_enable=NO
  - write_enable=YES
  - local_umask=022
  - chroot_local_user=YES
  - ssl_enable=YES
  - allow_annon_ssl=NO
  - force_local_data_ssl=YES
  - force_local_logins_ssl=YES
  - ssl_tlsv1=YES
  - ssl_tlsv2=NO
  - ssl_tlsv3=NO
  - rsa_cert_file=/etc/ssl/certs/ftpserver.crt
  - rsa_private_key_file=/etc/ssl/private/ftpserver.key
Despues de modificar el fichero lo guardamos y reiniciamos el servicio
```bash
$ sudo service vsftpd restart
```
Con esto todos los usuarios dispondran desde su creacion de un usuario ssh y sftp
para comrpobar el acceso por ftp podemos hacerlo con el comando 
```bash
$ sftp nombredeusuario@ip
```
![image](https://user-images.githubusercontent.com/91255763/220417070-a42bfcc4-bcc6-4071-a899-f3a5ec12122a.png)
Si disponemos de filezilla tambien podriamos conecatarnos a traves de el
![image](https://user-images.githubusercontent.com/91255763/220419487-182c1f64-c348-45de-bc3e-85aa11553b71.png)

