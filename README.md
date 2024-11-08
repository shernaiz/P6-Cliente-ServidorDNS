# P6-Cliente-ServidorDNS

Para engadir outro servizo que faga a función de cliente teremos que modificar o noso `.yml` de forma que lle añadamos os parámetros necesarios.

Para isto teremos que añadir outro contenedor ao archivo de configuración. Definiremolo como cliente e terá os seguintes parámetros.

**container_name** que indicará o nome co que visualizaremos o contenedor.

**image** onde poñeremos a imaxe que imos usar, neste casos erá a imaxe de `alpine`

**platform** esta parte é opcional xa que só indica o sistema operativo co que operamos.

**tty** este campo úsase para asignar un terminal teletipo ao contenedor, isto empregase en situacións nas que se necesita que o contenedor se execute como si interactuase cunha terminal.

**stdin_open** este parámetro servirá para manter aberto o flujo de entrada estándar no contenedor.

**dns**  este campo permitiranos especificar servidores DNS personalizados.

**networks** este último parámetrro indicará á rede a que se acopla o contenedor.

Unha vez feito esto teremos que montar os volumes nos que se aloxan as configuracións do noso servidor DNS. Para isto no mesmo cartafol onde temos a configuración `.yml` teremos que crear dúas carpetas mais: **conf** e **zonas**. Nestas carpetas teremos que engadir os archivos de configuración facilitados no repositorio. Estos seríam **

**zonas** aquí pegaremos o archivo db-asircastelao que contendrá a configuración do noso servidor DNS

**conf** neste teremos que añadir varios arquivos

1. `named.conf` onde se atopan os includes

2. `named.conf.default-zones`  onde se identifican as zonas

3. `named.conf.local` onde definimos a zona asircastelao.int

4. `named.conf.options` onde se definen as configuracións do servidor.

Unha vez creados estos arquivos, teremos que montar os volumes dentro do `.yml` para isto teremos que editalo e na parte onde temos definido o contenedor que fai de servidor, añadiremos as rutas das carpetas **conf** e **zonas**.

Por último no `.yml` configuraremos ao final a rede a que se enganchan os contenedores, solo lle daremos nome e indicaremos o driver.

**Inicio docker-compose y dig**
Unha vez feito esto xa teremos o necesario para que funcione. Para lanzar o archivo só teremos que lanzar o comando `docker compose up`.

Cando teñamos o compose arrincado podremos inspeccionar a rede co seguinte comando `docker network inspect red_bind9` e podremos ver como a esta rede están acoplados os contenedores do servidor e do cliente.

Por último, para facer unha consulta ao servidor teremos que entrar dentro do cliente co seguinte comando `docker exec -it contenedor_alpine sh` e unha vez dentro teremos que descargar o paquete que nos permita facer dig, usaremos o seguinte comando `apk update && apk add bind-tools`. Cando complete a descarga xa podremos facer dig a dirección do dns da seguinte forma: `dig @172.18.0.3`.