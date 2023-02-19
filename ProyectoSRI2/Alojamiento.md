## Instalar Pila LAMP
La pila LAMP es necesaria para dar alojamiento a páginas web.
### Instalar Apache2
Para instalar apache debemos primero estar en modo sudo su:
```bash
$ sudo su
$ apt update
$ apt install apache2
```
Para comprobar que se ha instalado debemos hacer systemctl status apache2
```bash
$ systemctl status apache2
```
![image](https://user-images.githubusercontent.com/91255763/204372625-2ecbcc3b-ca82-4ea9-9aa0-a7203bfa854c.png)

Ahora debemos ajustar el cortafuegos para permitir el tráfico web:
```bash
$ ufw app list
```
![image](https://user-images.githubusercontent.com/91255763/204376644-33900e2d-61c5-46da-9b10-930c1df3d1bc.png)

```bash
$ ufw app info "Apache Full"
```
![image](https://user-images.githubusercontent.com/91255763/204377064-3b60570f-cbb7-4a45-a2ad-ee7873e5ee75.png)

Para saber la ip del localhost hacemos:

```bash
$ cd/etc 
$ nano hosts
y cuando lo terminemos de editar ctrl+o y ctrl+x
```
![image](https://user-images.githubusercontent.com/91255763/204378081-a3392769-9dce-4b38-a520-906a0ef829f7.png)


Para comprobar que funciona nos movemos al navegador y escribimos 
```bash
http://ip_del_server
```
![image](https://user-images.githubusercontent.com/91255763/204378218-54848d7a-9c7f-4683-a55f-3895f5c0d849.png)

## Instalar PHP
Para instalar php se introducen los siguiente comando

```bash
$ apt install php libapache2-mod-php php-mysql
``` 

Con esto ya esta instalado php, para comprobar que funciona creamos un fichero llamado test.php 
```bash
$ nano /var/www/html/test.php
``` 
en test.ph debes popner esto
```php
<?php
phpinfo();
?>
```
Lo guardamos y en navegador ponemos la ruta localhost/test.php y deberiamos ver esto
![image](https://user-images.githubusercontent.com/91255763/219902674-02e0757f-582e-4c2d-959c-9464f2ef9e88.png)

Con esto ya se podrían alojar páginas web, para ello solo habria que crear carpertas con el nombre de las paginas web en /var/www, asignarles una ip de las red interna en el archivo /etc/hosts ademas de crear un virtual host para cada página web
