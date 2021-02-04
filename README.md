# HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose
<h1 id="práctica-https-con-lets-encrypt-docker-y-docker-compose"><span class="header-section-number">1</span> Práctica: HTTPS con Let’s Encrypt, Docker y Docker Compose</h1>
<p>En esta práctica vamos a habilitar el protocolo <a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">HTTPS</a> en un sitio web <a href="https://wordpress.org">PrestaShop</a> que se estará ejecutando sobre contenedores <a href="https://www.docker.com">Docker</a> en una instancia EC2 de <a href="https://aws.amazon.com/es/ec2/">Amazon Web Services (AWS)</a>.</p>
<h2 id="conceptos-básicos"><span class="header-section-number">1.1</span> Conceptos básicos</h2>
<h3 id="qué-es-https"><span class="header-section-number">1.1.1</span> ¿Qué es HTTPS?</h3>
<p><a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">HTTPS</a> (<em>Hyptertext Transfer Protocol Secure</em>) o protocolo seguro de transferencia de hipertexto, es un protocolo de la capa de aplicación basado en el protocolo HTTP, destinado a la transferencia segura de datos de hipertexto. (Fuente: <a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">Wikipedia</a>)</p>
<p>Para poder habilitar el protocolo <a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">HTTPS</a> en un sitio web es necesario obtener un <strong>certificado de seguridad</strong>. Este certificado tiene que ser emitido por una <strong>autoridad de certificación</strong> (AC). En esta práctica vamos a obtener un certificado para un dominio de la Autoriidad de Certificación <a href="https://letsencrypt.org">Let’s Encrypt</a>.</p>
<h3 id="qué-es-lets-encrypt"><span class="header-section-number">1.1.2</span> ¿Qué es Let’s Encrypt?</h3>
<p><a href="https://letsencrypt.org">Let’s Encrypt</a>​ es una autoridad de certificación que se puso en marcha el 12 de abril de 2016 y que proporciona <a href="https://es.wikipedia.org/wiki/X.509">certificados X.509</a> <strong>gratuitos</strong> para el cifrado de seguridad de nivel de transporte (<a href="https://es.wikipedia.org/wiki/Seguridad_de_la_capa_de_transporte">TLS</a>) a través de un proceso automatizado diseñado para eliminar el complejo proceso actual de creación manual, la validación, firma, instalación y renovación de los certificados de sitios web seguros. (Fuente: <a href="https://es.wikipedia.org/wiki/Let%27s_Encrypt">Wikipedia</a>)</p>
<h3 id="cómo-funciona-lets-encrypt"><span class="header-section-number">1.1.3</span> Cómo funciona Let’s Encrypt</h3>
<p>Se recomienda la lectura de la sección <strong>Cómo Funciona Let’s Encrypt</strong> de la <a href="https://letsencrypt.org/es/how-it-works/">documentación oficial</a>.</p>
<h3 id="qué-es-el-protocolo-acme"><span class="header-section-number">1.1.4</span> ¿Qué es el protocolo ACME?</h3>
<p>Para poder obtener un certificado de <a href="https://letsencrypt.org">Let’s Encrypt</a> para un dominio de un sitio web es necesario demostrar que se tiene control sobre ese dominio. Para realizar esta tarea es necesario utilizar un cliente del <a href="https://en.wikipedia.org/wiki/Automated_Certificate_Management_Environment">protocolo ACME (Automated Certificate Management Environment)</a>.</p>
<h3 id="qué-es-https-portal"><span class="header-section-number">1.1.5</span> ¿Qué es HTTPS-PORTAL?</h3>
<p><a href="https://hub.docker.com/r/steveltn/https-portal/">HTTPS-PORTAL</a> es una imagen <a href="https://www.docker.com">Docker</a> que contiene un servidor <a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">HTTPS</a> totalmente automatizado que hace uso de las tecnologías <a href="https://nginx.org/en/">Nginx</a> y <a href="https://letsencrypt.org">Let’s Enctrypt</a>. Los certificados SSL se obtienen y renuevan de <a href="https://letsencrypt.org">Let’s Encrypt</a> automáticamente.</p>
<p>Esta imagen está preparada para permitir que cualquier aplicación web pueda ejecutarse a través de <a href="https://es.wikipedia.org/wiki/Protocolo_seguro_de_transferencia_de_hipertexto">HTTPS</a> con una configuración muy sencilla.</p>
<p>Puede encontrar más información sobre <a href="https://hub.docker.com/r/steveltn/https-portal/">HTTPS-PORTAL</a> en la web oficial de <a href="https://hub.docker.com/r/steveltn/https-portal/">Docker Hub</a>.</p>

<h2 id="tareas-a-realizar"><span class="header-section-number">1.2</span> Tareas a realizar</h2>
<p>A continuación se describen <strong>muy brevemente</strong> algunas de las tareas que tendrá que realizar.</p>
<h3 id="paso-1"><span class="header-section-number">1.2.1</span> Paso 1</h3>
<p><strong>Crear una instancia EC2</strong> en <a href="https://wordpress.org">Amazon Web Services (AWS)</a>.</p>
<p>Cuando esté creando la instancia deberá <strong>configurar los puertos</strong> que estarán abiertos para poder conectarnos por SSH y para poder acceder por HTTP/HTTPS.</p>
<ul>
<li>SSH (22/TCP)</li>
<li>HTTP (80/TCP)</li>
<li>HTTPS (443/TCP)</li>
</ul>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura1.PNG "imagen")

<h3 id="paso-2"><span class="header-section-number">1.2.2</span> Paso 2</h3>
<p><strong>Obtener la dirección IP pública</strong> de su instancia EC2 en AWS.</p>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura2.PNG "imagen")

<h3 id="paso-3"><span class="header-section-number">1.2.3</span> Paso 3</h3>
<p><strong>Registrar un nombre de dominio</strong> en algún proveedor de nombres de dominio gratuito. Por ejemplo, puede hacer uso de <a href="http://www.freenom.com/">Freenom</a>.</p>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura3.PNG "imagen")

<h3 id="paso-4"><span class="header-section-number">1.2.4</span> Paso 4</h3>
<p><strong>Configurar los registros DNS del proveedor de nombres de dominio</strong> para que el nombre de dominio de ha registrado pueda resolver hacia la dirección IP pública de su instancia EC2 de AWS.</p>
<p>Si utiliza el proveedor de nombres de dominio <a href="http://www.freenom.com/">Freenom</a> tendrá que acceder desde el panel de control, a la sección de sus dominios contratados y una vez allí seleccionar <strong>Manage Freenom DNS</strong>.</p>
<p>Tendrá que añadir dos registros DNS de tipo A con la dirección IP pública de su instancia EC2 de AWS. Un registro estará en blanco para que pueda resolver el nombre de dominio sin las <code>www</code> y el otro registro estará con las <code>www</code>.</p>


![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura4.PNG "imagen")


<p><strong>Nota:</strong> Tenga en cuenta que una vez que ha realizado los cambios en el DNS habrá que esperar hasta que los cambios se progaguen. Puede hacer uso de la utilidad <a href="https://dnschecker.org/">dnschecker.org</a> para comprobar el estado de propagación de las DNS.</p>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura5.PNG "imagen")

<h3 id="paso-5"><span class="header-section-number">1.2.5</span> Paso 5</h3>
<p><strong>Realizar la instalación y configuración de Docker y Docker Compose</strong> en la instancia EC2 de AWS.</p>

```

ubuntu@ip-172-31-14-3:~/iaw-Pr-ctica-PrestaShop$ sudo docker-compose up

```

<h3 id="paso-6"><span class="header-section-number">1.2.6</span> Paso 6</h3>
<p><strong>Modificar el archivo <code>docker-compose.yml</code> de alguna de las prácticas anteriores</strong> para incluir el servicio de <a href="https://hub.docker.com/r/steveltn/https-portal/">HTTPS-PORTAL</a>.</p>
<p>Una vez llegado a este punto, sólo queda desplegar los servicios con <a href="https://docs.docker.com/compose/">Docker Compose</a> y ya tendríamos nuestro sitio web con <strong>HTTPS habilidado y todo configurado para que el certificado se vaya renovando automáticamente</strong>.</p>

```
https-portal:
    image: steveltn/https-portal:1
    ports:
      - 80:80
      - 443:443
    restart: always
    environment:
      DOMAINS: 'jesus-docker-iwa.ga -> http://prestashop:80'
      #STAGE: 'staging'
      STAGE: 'production' # Don't use production until staging works
      # FORCE_RENEW: 'true'
    networks: 
      - frontend-network
 ```
      
      
      
      
<h3 id="paso-6"><span class="header-section-number">1.2.7</span> Paso 7</h3>
<p><strong>Segimos el menu de intalacion</strong>.</p>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura7.PNG "imagen")

<h3 id="paso-6"><span class="header-section-number">1.2.8</span> Paso 7</h3>
<p><strong>inciamos sesion con jesus@gmail.com/usuario123</strong>.</p>

![imagen](https://github.com/jesus2307/HTTPS-con-Let-s-Encrypt-Docker-y-Docker-Compose/blob/main/imagen/Captura8.PNG "imagen")

