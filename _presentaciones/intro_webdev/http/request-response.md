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
 
!SLIDE code

    GET / HTTP/1.1
    Host: google.com
    Accept-Language: es
    Connection: Keep-Alive
    User-Agent: Mozilla/4.0 
    
!SLIDE bullets 

##El servidor

* Un programa está esperando conexiones (`Apache`, `nginx`, etc.) 
* Usa *magia* para generar una respuesta 
* Partes: *Código de estado*, *Encabezados*, [*Cuerpo*]
* Cómo salió todo, meta-data, lo que pediste (opcional)

!SLIDE code small

    HTTP/1.1 200 OK
    Date: Sat, 09 Apr 2011 04:55:40 GMT
    Cache-Control: private, max-age=0
    Content-Type: text/html; charset=ISO-8859-1
    Server: gws
    
    <html>
    ...
