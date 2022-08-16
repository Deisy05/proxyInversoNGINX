# proxyInversoNGINX
Instrucciones para llevar  a cabo el despliegue de un ambiente computacional distribuido.


**Requisitos**

>Tú PC
>Un web browser o navegador
>Un proxy inverso (IP: 192.168.28.30 y ejecutándose sobre una máquina virtual)
>Un aplicativo ejecutado con la herramienta ‘docker-compose’ (IP: 192.168.28.31 y ejecutándose sobre una máquina virtual)
>Un servidor web Apache (IP: 192.168.28.32 y ejecutándose sobre una máquina virtual)


## MV APACHE

Instalar apache
```
    $ sudo apt-get install apache2
```
Crear una página nueva :
```
		$ cd  /var/www/
		$ nano index.html
```
Editar ruta de la página por defecto
```
		$ cd /etc/apache2/sites-enabled
		$ nano 000-default.conf
```
Recar el servicio para actualizar la configuración.
```
    $ systemctl reload apache2
```
Ahora se puede ejecutar el localHost en tu navegador.

## MV DOCKER COMPOSE
Instalar docker compose
```
    $ sudo curl -L "https://github.com/docker/compose/releases/download/1.26.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
conceder permisos
```
    $ sudo chmod +x /usr/local/bin/docker-compose
```
verificar version
```
    $ docker-compose --version
```
Configurar un archivo docker-compose.yml creando un nuevo directorio en su carpeta de inicio, y luego lo movimos a él
```
    $ mkdir ~/compose-demo
    $ cd ~/compose-demo
```
configurar una carpeta de aplicaciones y crear un archivo index.html, se edita este archivo a gusto.
```
    $ mkdir app
    $ nano app/index.html
```
crea el archivo docker-compose.yml:
```
    $ nano docker-compose.yml
```

En el archivo que se creó se copia lo siguiente:
```
version: '3.7'
services:
web:
image: nginx:alpine

ports:
      - "8000:80"
volumes:
      - ./app:/usr/share/nginx/html
```
descargar la imágen Docker
```
    $ docker-compose up -d
```

Finalmente podemos  ejecutamos localhost:8000 en el navegador de esta máquina virtual.

## NGINX COMO PROXY INVERSO

Instalar nginx
```
    $ sudo apt-get install ngnix
```

Editar el archivo para el proxy
```
    $ cd etc/nginx/sites-available
    $ sudo nano default
```
Una vez en el archivo ubicate en la sección de  ‘root/var/www/html’ y comenta esa línea
luego en 'service_name' indica la dirección IP de la maquina virtual que hará de proxy
por ultimo en la sección de location /{} indica las rutas deseadas y en proxy_pass indica la dirección IP a la cual se hara la redirección:

>location /apache{                                 location  /compose{
>     proxy_passs  IPAPACHE                            proxy_pass  IPCOMPOSE;     
>}                                                 } 


##Crear una red local

Con esa red local vamos a conectar la máquinas virtuales y al anfitrion (la maquina física)
para ellos 
[Guia para crear la red local solo anfitrión](https://carleton.ca/scs/2019/creating-a-new-host-only-adapter-in-virtualbox/)


##Implementación

En el browser del anfitrion, digitar las direcciones: una que apunta al proxy 'localHost' y las otras que apuntan a las maquinas creadas


## **INTEGRANTES**
Borja Muñoz Carlos Andres - borja.carlos@correounivalle.edu.co <br />
Riaños Horta Natalia - rianos.natalia@correounivalle.edu.co <br />
Melo Burbano Deisy Catalina - deisy.melo@correounivalle.edu.co <br />
