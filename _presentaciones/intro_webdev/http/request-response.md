!SLIDE 

## Desarrollo web en python

!SLIDE bullets

##La base de todo: HTTP

* Tu computadora habla HTTP con un servidor
* HTTP: Protocolo de Transporte de Hiper-Texto
* **¡Sólo hay archivos de texto involucrados!**
* Todo está basado en el ciclo **Solicitud-Respuesta**

!SLIDE center

![rr](rr.gif)


!SLIDE bullets

##El cliente

* Cualquier programa que pueda hablar HTTP
* Crea una solicitud en texto y la envía
* Partes: *Verbo*, *Ruta*, [*Encabezados*]
* Qué hacer, adónde hacerlo, información extra (opcional)
 
!SLIDE code small
## Magia

    @@@python
    def application(environ, response_callback)
        text = 'Hola Mundo!\n'
        response_callback(
              "200 OK",
              [('Content-Type', 'text/plain'),
               ('Content-Length', str(len(text)))]
              )
        return [text]

    from wsgiref.simple_server import make_server

    daemon = make_server('127.0.0.1', 8000, application)

    daemon.handle_request()


!SLIDE commandline

    $ curl -v http://127.0.0.1:8000 

    GET / HTTP/1.1
    User-Agent: curl/7.19.7 (i486-pc-linux-gnu)
    Host: 127.0.0.1:8000
    Accept: */*


!SLIDE bullets

## http://google.com:80/?q=python#results

* `http://` : el *protocolo*
* `google.com`: *host* (o dominio)
* `:80`: puerto (opcional, `80` por omisión)
* `/`: *ruta (path)*
* `?q=python` : *querystring*, los parámetros
* `#results`: *fragmento* (sólo accesible en el cliente)

!SLIDE bullets 

##El servidor

* Un programa está esperando conexiones (`Apache`, `nginx`, etc.) 
* Usa *magia* para generar una respuesta 
* Partes: *Código de estado*, *Encabezados*, [*Cuerpo*]
* Cómo salió todo, meta-data, lo que pediste (opcional)

!SLIDE commandline 

    $ curl -v http://127.0.0.1:8000 

    HTTP/1.0 200 OK
    Date: Mon, 11 Apr 2011 17:39:00 GMT
    Server: WSGIServer/0.1 Python/2.6.5
    Content-Type: text/plain
    Content-Length: 12

    Hola Mundo!

!SLIDE bullets

##Verbos HTTP:

* GET: leer un recurso
* POST: actualizar un recurso
* PUT: crear un recurso
* DELETE: borrar un recurso
* HEAD et al.

!SLIDE bullets

##Códigos de estado

* 1XX y 2XX: todo salió bien
* 3XX: redirecciones
* 4XX: error de cliente (*vos* hiciste algo mal)
* 5XX: error de servidor

!SLIDE bullets

##Encabezados

* Son totalmente arbitratios
* Meta-data

!SLIDE bullets

##HTTP no tiene estado

* El servidor no tiene memoria entre solicitudes
* Toda la información tiene que estar en la solicitud
* Si querés que persista, podrías usar el truco de las
  cookies: mandar la información en encabezados
