Se nos plantea realizar un ejercicio práctico que consiste en alojar dos sitios webs: 
web1 y web2 en una sola instancia de nginx, con dos 2 hosts virtuales configurados.

Un proxy inverso es un servidor que se sitúa delante de los servidores web y reenvía las solicitudes del cliente (por ejemplo, el navegador web) a esos servidores web. Los proxies inversos suelen implementarse para ayudar a aumentar la seguridad, el rendimiento y la fiabilidad. 

# Implementación

## Paso 1
He escrito el docker-compose.yml.

He utilizado la imagen nginx alojada en: https://hub.docker.com/_/nginx

Nota: He tenido problemas para utilizar la clausula: pull_policy: build porque estaba utilizando
una versión anterior a docker-compose, por lo que he tenido que hacer un purgue y reinstalar
la versión correcta, es muy importante a la hora de utilizar clausulas concretas de Docker-compose revisar el version del yml y que sea compatible.

La claúsula: pull_policy he decidido utilizarla para forzar a que se reconstruyese la imagen cada vez que se arranquen los contenedores, esto es así porque si modificamos algo de web1 y web2, puedan verse los cambios efectuados.

## Paso 2
Primero creo el nuevo proyecto y lo divido en tres carpetas: proxy, web1, web2. Cada una de ellas tendra su propio contenedor.

Su estructura será la siguiente:

	proxy
	  Dockerfile
	  conf
	    conf.d
	      web1.conf
	      web2.conf
	web1
	  Dockerfile
	  index.html
	web2
	  Dockerfile
	  index.html

En el archivo: "web1.conf", llevo a cabo la configuración del proxy inverso para la web1:

	server {
	  	listen 80;
	  	server_name web1;
	  
	  location / {
	  	proxy_pass http://web1/;
	  }
	}

## Paso 3
Después arranco los contenedores:

docker-compose up -d

y compruebo que me redirecciona a mi proxy web1.

Ejemplos de uso:

1. Abrir navegador Firefox
2. Poner las siguientes urls: web1.localhost, web2.localhost, si ponemos web3.localhost o loquesea.localhost automáticamente redirijirá al uno, porque así ha sido configurado.
