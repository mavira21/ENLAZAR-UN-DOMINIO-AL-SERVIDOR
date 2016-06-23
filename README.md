###OBTENER IP SERVIDOR  

Abrimos sesión en Bitnami y accedemos a la consola de nuestro servidor.  
Copiamos la IP del servidor.  

###ABRIR GESTOR DE DOMINIO  

Vamos a nuestro gestor de dominio y abrimos sesión.  
- Seleccionamos **configuración DNS**  
- Donde pone **Registro A" (dirección IPV4)** pegamos la IP de nuestro servidor (obtenida en Bitnami).  
- **Aceptar ajustes**  

###VAMOS AL NAVEGADOR Y PONEMOS DOMINIO NUEVO.  

Aparece la pantalla de Bitnami "CONGRATULATIONS"  

###CONECTAR CON EL SERVIDOR CON CYBERDUCK  

Vamos a home/bitnami/apps/miweb/htdocs/site/config.php  

**1.- Editamos el archivo config.php del modo siguiente:**  

```
$config->httpHosts = array('miweb.es', 'www.miweb.es');  
```

**2.- Editamos el archivo oculto .htaccess**  

Para ello vamos a /site/ y seleccionamos "visualización" -> mostrar archivos ocultos  

- Seleccionamos .htaccess -> acción -> editar y seleccionamos Sublime Text  

  _Para configurado como predeterminado en Cyberduck ir a_   
  _Editar-> Preferencias-> Editor Externo-> seleccionar un editor por defecto-> Sublime Text-> usar el editor por defecto_  


Sobre la línea 117 debe quedar así:  
```
  # RewriteBase /  
  # RewriteBase /miweb/  
  # RewriteBase /~user/    
```

**3.- Editamos el archivo miweb.conf**  

Ir a home/bitnami/apps/miweb/conf  

Y editamos el archivo del siguiente modo:  
```
<VirtualHost *:80>
    ServerAdmin mariel
    DocumentRoot "/opt/bitnami/apps/miweb/htdocs"
    ServerName miweb.es
</VirtualHost>
<VirtualHost *:80>
    ServerAdmin mariel
    DocumentRoot "/opt/bitnami/apps/miweb/htdocs"
    ServerName www.miweb.es
</VirtualHost>

<Directory "/opt/bitnami/apps/miweb/htdocs">
    Options Indexes MultiViews
    AllowOverride All
    <IfVersion < 2.3 >
    Order allow,deny
    Allow from all
    </IfVersion>
    <IfVersion >= 2.3>
    Require all granted
    </IfVersion>
</Directory>

```

###REINICIAR APACHE  

Abrir con Putty la consola de acceso al servidor.  
Ir a 
 bitnami/apps/miweb/conf  
y poner  

```
sudo /opt/bitnami/ctlscript.sh restart apache  
```  

###IR AL NAVEGADOR Y PONER NUEVO DOMINIO  

miweb.es o www.miweb.es  


[VER VIDEOS](https://youtu.be/-IKkagNzSL8)


##POSIBLES INCIDENCIAS  

EN PW miweb.es/admin

Nos aparece en rojo el siguiente mensaje en la parte superior de ProcessWire cuando entramos:

Unrecognized HTTP host: "miweb.es" ->Please update your $config -> httpHost setting in site/config.php

Para solucionarlo vamos a config.php
y ponemos en la línea 82 aprox. lo siguiente:

$config->httpHosts = array('miweb.es', 'www.miweb.es');

SI NO SALEN FOTOS EN PÁGINA RECETAS

Hay que cambiar la url en el archivo ubicado en scripts llamado factories.js

.factory('pw', function($http, $location) {

    var webService = "http://"+$location.$$host+"/web-service/";

