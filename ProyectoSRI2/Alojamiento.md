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
en test.ph debes poner esto
```php
<?php
phpinfo();
?>
```
Lo guardamos y en navegador ponemos la ruta localhost/test.php y deberiamos ver esto
![image](https://user-images.githubusercontent.com/91255763/219902674-02e0757f-582e-4c2d-959c-9464f2ef9e88.png)

Ahora para que se pueda administrar la base de datos de manera sencilla instalaremos phpmyadmin con
```bash 
$ apt install phpmyadmin
```
A la hora de configurar phpmyadmin seleccionamos la opcioón de apache2
![image](https://user-images.githubusercontent.com/91255763/220162390-5499f44a-e097-40d2-9b4e-52678f430c55.png)

![image](https://user-images.githubusercontent.com/91255763/220162617-c779772f-e696-40df-8fae-79c8af9d6400.png)

Tambien debemos crear una contraseña para phpmyadmin.
![image](https://user-images.githubusercontent.com/91255763/220162694-17004cef-6316-483f-ba37-57a22a694668.png)

Para comprobar que se ha instalado buscamos en el navegador localhost/phpmyadmin
Por defecto el usuario es phpmyadmin y no tiene ningun privilegio, es decir no puede ver ninguna base de datos, para que pueda servir como usuario root debemos hcaer lo siguiente.
En mysql debemos escribir lo siguinete como root:
```sql
grant all privileges on *.* 'phpmyadmin'@'localhost' identified by 'contraseña';
```
![image](https://user-images.githubusercontent.com/91255763/220167516-e82ff6f2-261a-456a-ad96-ed342765f98d.png)
Despues debemos recarcar mysql con 
```bash
$ sudo service mysql restart
```
y volver a phpmyadmin y al iniciar sesion podras ver y manipular todas la bases de datos de mysql
![image](https://user-images.githubusercontent.com/91255763/220167854-6206c034-15da-474c-af53-c932273cfa08.png)




Con esto ya se podrían alojar páginas web, para ello solo habria que crear carpertas con el nombre de las paginas web en /var/www, asignarles una ip de las red interna en el archivo /etc/hosts ademas de crear un virtual host para cada página web
