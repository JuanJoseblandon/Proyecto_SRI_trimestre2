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
Ahora el servicio ssh ya esta operativo 

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
