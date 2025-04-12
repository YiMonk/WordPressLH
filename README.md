
# WordPress Local Host
Guía para instalar WordPress en Local Host en win11 con Apache y MariaDB 

Resumen de comandos útiles

## Iniciar 

    Apache	httpd.exe -k start

## Reiniciar 

    Apache	httpd.exe -k restart

## Detener Apache	
  
    httpd.exe -k stop

## Acceder a MariaDB	

    mysql -u root -p


# Paso 1 Instalar Apache HTTP Server 

[Descarga Apache](https://www.apachelounge.com/download/)

Elige la versión para Windows (ej: Apache 2.4.63-250207 Win64).

Extrae el archivo ZIP en una carpeta, por ejemplo:
<b>Cambia estas líneas:</b>

### Configura Apache: 

Abre C:\Apache24\conf\httpd.conf con un editor de texto.

 - Cambia estas líneas:

        Define SRVROOT "C:/Apache24"  # Asegúrate de que coincida con tu ruta
        ServerName localhost:80        # Descomenta esta línea

## Instala Apache como servicio:

 - Abre CMD como Administrador y ejecuta:

        cd C:\Apache24\bin
        httpd.exe -k install

## Inicia Apache:

 - Ve al Administrador de servicios de Windows (services.msc) y busca "Apache", luego inícialo o desde CMD:

         httpd.exe -k start

Verifica que funcione:

Abre tu navegador y visita:
http://localhost
Deberías ver "It works!".

# Paso 2: Instalar PHP 


Descarga PHP desde https://windows.php.net/download/
Elige la versión Non Thread Safe y VS17 x64 (ej: php-8.4.6-nts-Win32-vs17-x64).

Extrae el ZIP en una carpeta, por ejemplo:
<b> C:\php </b>

## Configura PHP:

 - Renombra <b> php.ini-development </b> a <b> php.ini </b> en <b> C:\php.</b>

 - Edita php.ini y habilita estas extensiones (quita el ; al inicio):


          extension_dir = "ext"
          extension=mysqli
          extension=mbstring
          extension=curl
          extension=openssl


## Integra PHP con Apache:

 - Edita C:\Apache24\conf\httpd.conf y agrega al final:


          LoadModule php_module "C:/php/php8apache2_4.dll"
          AddHandler application/x-httpd-php .php
          PHPIniDir "C:/php"


 - Guarda y reinicia Apache:



          httpd.exe -k restart


## Prueba PHP:

 - Crea un archivo info.php en C:\Apache24\htdocs con:

        <?php phpinfo(); ?>

 -Visita http://localhost/info.php y verás la info de PHP.


# Paso 3: Instalar MariaDB (MySQL)
  Descarga MariaDB desde https://mariadb.org/download/

 - Elige la versión MSI Installer para Windows.
 - Sigue los pasos y configura una contraseña para root.
 - Elige "UTF8" como codificación.
 - Verifica que funcione:

Abre CMD y ejecuta:

        mysql -u root -p

 - Introduce tu contraseña. Si entras, MariaDB está funcionando.

# Paso 4: Instalar WordPress
 ### Descarga WordPress desde https://wordpress.org/download/.
 ### Extrae el ZIP en la carpeta de Apache:
   C:\Apache24\htdocs\wordpress
 ### Crea una base de datos para WordPress:
  -Abre MariaDB desde CMD:

        mysql -u root -p
  
  -Ejecuta estos comandos:

        CREATE DATABASE wordpress;
        CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'tupassword';
        GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
        FLUSH PRIVILEGES;
        EXIT;
  
 ### Configura WordPress:

 - Renombra wp-config-sample.php a wp-config.php en C:\Apache24\htdocs\wordpress.
 - Edita wp-config.php y actualiza estos datos:

        define('DB_NAME', 'wordpress');
        define('DB_USER', 'wpuser');
        define('DB_PASSWORD', 'tupassword');
        define('DB_HOST', 'localhost');
 

 ### Completa la instalación:

Visita http://localhost/wordpress en tu navegador.

Sigue el asistente de WordPress (elige idioma, usuario, contraseña, etc.).


#SOLUCIONES

## 'mysql' no se reconoce como un comando interno o externo, programa operable o archivo por lotes.
 - Significa que MariaDB/MySQL no está en el PATH del sistema, por lo que Windows no sabe dónde encontrar el ejecutable mysql.exe.

  1. Encontrar la ruta de mysql.exe
    Por defecto, MariaDB se instala en:

    MariaDB 10.6+: C:\Program Files\MariaDB\MariaDB XX.X\bin\

    MySQL: C:\Program Files\MySQL\MySQL Server X.X\bin\

  Si no la encuentras, haz una búsqueda en C:\ de mysql.exe.


### Verificar en una nueva ventana de CMD
   - Cierra y vuelve a abrir el CMD (para que cargue el nuevo PATH).

Ejecuta:

      mysql --version
      
Debería mostrar algo como:

      mysql  Ver 15.1 Distrib 10.11.2-MariaDB, for Win64...
      
Ahora puedes acceder con:

      mysql -u root -p
      
(Ingresa la contraseña que configuraste durante la instalación).

### Alternativa: Usar la ruta completa en CMD
  Si no quieres modificar el PATH, puedes ejecutar mysql.exe usando la ruta completa:
  
    "C:\Program Files\MariaDB\MariaDB 10.11\bin\mysql.exe" -u root -p

