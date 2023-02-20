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
      - Alojamiento
        ```bash
        ``` 
- Host virtual en apache
- Creación de usuario del sistema para acceso a ftp, ssh, smtp, …
- Se creará un subdominio en el servidor DNS con las resolución directa e inversa
- Se creará una base de datos además de un usuario con todos los permisos sobre dicha base de datos (ALL PRIVILEGES)
- Se habilitará la ejecución de aplicaciones Python con el servidor web
