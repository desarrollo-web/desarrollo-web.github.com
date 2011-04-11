!SLIDE bullets

##En las entrañas del servidor

* Hay un "daemon" que está escuchando conexiones
* A veces él sabe qué hacer
* A veces le pasa el control a programas
* Y éstos a veces interactúan con bases de datos


!SLIDE center 

![wsgi](wsgi.png)

!SLIDE bullets

##¿Magia?

* WSGI: Interfaz Estándar de Servidor Web
* Una convención para escribir web-apps en python
* Todo lo demás funciona sobre ésta

!SLIDE code small

    @@@python

    def application(environ, start_response):
      text = 'Hola Mundo!'
      start_response(
              "200 OK",
              [('Content-Type', 'text/plain'),
               ('Content-Length', str(len(text)))]
              )
      return [text]

    from paste.httpserver import serve
    serve(application)



!SLIDE bullets

##Inventando realidades

* Existen varios frameworks, pero todos se basan en *abstracciones*
* Lo más básico: Solicitudes/Respuestas como objetos y no archivos
* Lo más avanzado: MVC: Modelos-Vistas-Controladores

!SLIDE code small

    @@@python 
    from webob import Request, Response

    def sum_nums(text):
    return sum([int(e) for e in text if e.isdigit()])

    def app(environ, start_response):
    request = Request(environ)
    text = ''.join(request.GET.keys())
    response = Response(content_type='text/html',
          body="""<html>
                    <body style='font-size: 250px;'>
                    %s
                    <body>
                 </html>"""%sum_nums(text))
    return response(environ, start_response)

    from paste.httpserver import serve
    serve(app)


