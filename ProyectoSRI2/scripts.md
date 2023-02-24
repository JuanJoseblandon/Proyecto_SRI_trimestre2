# Aqui se podran ver los scripts que hacen las siguientes funciones
- La creación de usuarios y del directorio correspondiente para el alojamiento web
En mi caso tengo 2 scripts para esto
  - creacion de usuarios
     ```bash
     #!/bin/bash
      echo "dime un nuevo usuario"
      read usuario
      while id $usuario &> /dev/null;
      do
        echo "ya existe"
        echo "escribe usuario nuevo"
        read usuario
      done
        useradd -d /home/$usuario -g 1000 -m -s /bin/bash $usuario
        passwd $usuario
    ```
    ![image](https://user-images.githubusercontent.com/91255763/220192553-d5e07669-dedb-43d6-b43e-efff32cb8aae.png)
   - Creación del directorio.
      Yo poseo 2 scripts ya que uno crea el directorio para el usuario y otro para el alojamiento
      - usuario 
        ```bash
            #!/bin/bash
            # Script para crear un sitio virtual en Apache
            # utilizando un nombre de usuario como argumento

            # Comprueba si se ha proporcionado un argumento
            if [ $# -eq 0 ]; then
               echo Error!. Introduce usuario !
               exit 1;
            fi

            # Establece la variable USER en el primer argumento del script
            USER=$1

            # Establece las variables de configuración del sitio virtual
            CONF="${USER}.conf"
            PATH_AVAILABLE="/etc/apache2/sites-available/${CONF}"
            PATH_ENABLED="/etc/apache2/sites-enabled/${CONF}"
            SUB_DOMAIN="${USER}.marisma.local"
            DOCUMENT_ROOT="/var/www/html/$1"
            INDEX="${DOCUMENT_ROOT}/index.html"

            # Crea el directorio de documento raíz si no existe
            if ! [ -d $DOCUMENT_ROOT ] ; then
               echo "Creando documento root"
               mkdir -p "$DOCUMENT_ROOT"
            fi

            # Crea el archivo de configuración del sitio virtual
            touch $PATH_AVAILABLE
            if [ -f $PATH_AVAILABLE ] ; then
               echo "creando fichero de config"
               echo "<VirtualHost *:80>
                       ServerAdmin admin@$SUB_DOMAIN
                       ServerName ${USER}
                       DocumentRoot $DOCUMENT_ROOT
                       ErrorLog /var/log/apache2/$SUB_DOMAIN.errorLog.log
                       LogLevel error
                       CustomLog /var/log/apache2/$SUB_DOMAIN.customLog.log combined
                   </VirtualHost>" >>$PATH_AVAILABLE

               # Crea el archivo index.html
               echo "Creando index.html"
               echo "<p>Subdominio: $SUB_DOMAIN</p>" >>$INDEX
               echo "<p>usuario: $USER</p>" >>$INDEX

               # Habilita el sitio virtual
               a2ensite $CONF
            fi

            # Reinicia el servicio de Apache
            echo "Reiniciando servicio apache"
            sudo service apache2 restart
        ```
        ![image](https://user-images.githubusercontent.com/91255763/220300234-3f9b10ad-80b3-4bcd-9d05-e06863fdc2b3.png)
        ![image](https://user-images.githubusercontent.com/91255763/220303872-c5cda4a7-59e5-48b1-bc53-a7d760508cb0.png)

        ![image](https://user-images.githubusercontent.com/91255763/220300362-5f2c271f-5059-4d05-81ac-28273c7937cd.png)


      - Alojamiento
        ```bash
                 #!/bin/bash

        # Script para crear un sitio virtual en Apache
        # utilizando un nombre de usuario como argumento

        # Comprueba si se ha proporcionado un argumento
        if [ $# -eq 0 ];then
           echo Error!. Introduce usuario !
           exit 1;
        fi


        # Establece la variable USER en el primer argumento del script
        USER=$1
        # Establece las variables de configuración del sitio virtualCONF="${USER}.marisma.conf"
        CONF="${USER}.marisma.conf"
        PATH_AVAILABLE="/etc/apache2/sites-available/${CONF}"
        PATH_ENABLED="/etc/apache2/sites-enabled/${CONF}"
        SUB_DOMAIN="${USER}.marisma.local"
        DOCUMENT_ROOT="/var/www/${SUB_DOMAIN}"
        INDEX="${DOCUMENT_ROOT}/index.html"
        # Crea el directorio de documento raíz si no existe
        if ! [ -d $DOCUMENT_ROOT ] ; then
           echo "Creando documento root"
           mkdir -p "$DOCUMENT_ROOT"
        fi

        # Crea el archivo de configuración del sitio virtual
        touch $PATH_AVAILABLE
        if [ -f $PATH_AVAILABLE ] ; then
           echo "creando fichero de config"
           echo "<VirtualHost *:80>
                   ServerAdmin admin@$SUB_DOMAIN
                   ServerName www.$SUB_DOMAIN
             ServerAlias $SUB_DOMAIN
                   DocumentRoot $DOCUMENT_ROOT
             #WSGIScriptAlias / /var/www/${SUB_DOMAIN} 
                   ErrorLog /var/log/apache2/$SUB_DOMAIN.errorLog.log
                   LogLevel error
                   CustomLog /var/log/apache2/$SUB_DOMAIN.customLog.log combined
               </VirtualHost>" >>$PATH_AVAILABLE


         # Crea el archivo index.html
           echo "Creando index.html"
           echo "<h1>BIENVENIDO ${USER}</h1>" >>$INDEX
         # Habilita el sitio virtual
           a2ensite $CONF
        fi
         #reinicia el servicio apache y habilita la ejecución de aplicaciones python
        echo "Habilitando aplicaciones python"
        sudo apt install libapache2-mod-wsgi
        sudo service apache2 restart
        ```
          ![image](https://user-images.githubusercontent.com/91255763/220302183-141a9ffe-0254-478b-91e1-7d9d62ec3d61.png)
         ![image](https://user-images.githubusercontent.com/91255763/220303574-91bf551a-9d24-454a-bc8c-7dc9e065a8a0.png)
- Host virtual en apache
  Los scripts anteriores tambien crean los host de apache, para comprobarlo solo hay que ir a la carpeta sites-enabled para ver los virtualhost
  ![image](https://user-images.githubusercontent.com/91255763/220304597-b355d984-ec71-4b5d-96d7-f281d3bf8881.png)
- Creación de usuario del sistema para acceso a ftp, ssh
  ```bash
  #!/bin/bash
      echo "dime un nuevo usuario"
      read usuario
      while id $usuario &> /dev/null;
      do
        echo "ya existe"
        echo "escribe usuario nuevo"
        read usuario
      done
        useradd -d /home/$usuario -g 1000 -m -s /bin/bash $usuario
        passwd $usuario
  ```
- Se creará un subdominio en el servidor DNS con las resolución directa e inversa
   ```bash
          #!/bin/bash

      #verificacion de argumentos validos
      if [ $# -le 1 ];then
         echo Error!. Introduce subdominio e IP!
         exit 1;
      fi


      # Variables
      USER=$1
      IP=$2
      SUB_DOMAIN="${USER}.marisma.local"
      DOCUMENT="/var/www/${USER}.marisma.local"
      ZONE_FILE="/etc/bind/zones/db.marisma.local"
      REVERSE_FILE="/etc/bind/zones/db.0.4.10.in-addr.arp"

      # se añaden la ip y el nombren de subdominio al dominio 
      echo "Actualizando fichero de zona"
      echo "\$ORIGIN ${SUB_DOMAIN}."  >> $ZONE_FILE
      echo "@ IN  A   ${IP}"  >> $ZONE_FILE
      echo "www   IN  A   ${IP}"  >> $ZONE_FILE

      # se añaden la ip y el subdominio a hosts
      echo "		IN	PTR	${SUB_DOMAIN}" >> $REVERSE_FILE
      echo "${IP}	www.${SUB_DOMAIN}" >>/etc/hosts
      echo "${IP}	${SUB_DOMAIN}" >>/etc/hosts
      # se habilita la ejecucion de aplicaciones python
      echo "Habilitando ejecución de aplicaciones python"
      sudo apt install libapache2-mod-wsgi
      # se reinician los servicios
      echo "Reiniciar servicios"
      service apache2 reload > /dev/null
      service bind9 reload > /dev/null
      service vsftp reload > /dev/null

   ```
- Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
     ```bash
     #!/bin/bash

      # Pedir al usuario el nombre de la base de datos y la contraseña
      read -p "Introduzca el nombre de usuario: " username
      read -sp "Introduzca la contraseña: " password

      # Crear la base de datos
      mysql -u root -p -e "CREATE DATABASE IF NOT EXISTS $username; GRANT ALL PRIVILEGES ON $username.* TO '$username'@'localhost' IDENTIFIED BY '$password'; GRANT           USAGE ON phpmyadmin.* TO '$username'@'localhost';"

      # Confirmar que la base de datos se creó con éxito
      if [ $? -eq 0 ]; then
          echo "La base de datos $username se ha creado con éxito."
      else
          echo "No se pudo crear la base de datos $username."
      fi

     ```
    Este script pide  un usuario de sistema existente para crear con el una base de datos con su nombre y un usuario con todos los privilegios sobre esta base de datos y tambien le permite la administración de esta atraves de phpmyadmin.
    
    Comprobación:
    
    ![image](https://user-images.githubusercontent.com/91255763/220886543-d537840c-6551-4466-b00d-d9a761cbab55.png)
    ![image](https://user-images.githubusercontent.com/91255763/220886706-934343c1-fdd5-4c68-89f4-9c0d9d8722e2.png)
    ![image](https://user-images.githubusercontent.com/91255763/220887081-6a2d43c2-6597-4503-b719-9dff8be9a27b.png)
    ![image](https://user-images.githubusercontent.com/91255763/221199489-5525244b-5504-4627-b4d4-e2286a24ee5b.png)


- Se habilitará la ejecución de aplicaciones Python con el servidor web
