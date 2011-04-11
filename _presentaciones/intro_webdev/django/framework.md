!SLIDE bullets incremental

##Django (ver 1.3)

* Esta basado en el modelo MVC
* Bueno, más o menos...
* En realidad es MTV
* Está hecho en sexy Python! :D


!SLIDE code small

##Urls, las puertas de la aplicación

    @@@python
    url(r'^$', 'lista.views.crear',
        name='home'),

    url(r'^(?P<codigo>[a-zA-Z0-9]+)/$', 'lista.views.mostrar',
        name='lista'),

    url(r'^tarea/', include('tarea.urls')),

!SLIDE code small

##Views, el corazón

    @@@python
    def mostrar(request, codigo):
        lista = Lista.objects.get(codigo=codigo)

        #Guardamos para actualizar la fecha de acceso
        lista.save()
        
        return render_to_response('lista/mostar.html',
                        {'lista':lista},
                        context_instance=
                            RequestContext(request))


!SLIDE code small

##Modelos, el enlace a la base de datos

    @@@python
    class Lista(models.Model):
        codigo = models.TextField(max_length=10)
        titulo = models.TextField(null=True,
                                  blank=True,
                                  default='')
        fecha_creacion = models.DateTimeField(auto_now_add=True)
        fecha_acceso = models.DateTimeField(auto_now=True)


!SLIDE code small

##Templates, lo que el usuario ve

    @@@html
    {% if lista.tarea_set.exists %}
        <ul id="lista_tareas">
        {% for tarea in lista.tarea_set.all %}
          <li>
            <input type="checkbox" name="hecha"
                   value="{{ tarea.id }}" class="hecha-check"
                   {% if tarea.hecha %}checked{% endif %}>

            <span class=
                "{% if tarea.hecha %}tachado{% endif %}">
                {{ tarea.descripcion }}
            </span>
                
          </li>
        {% endfor %}
        </ul>
	{% endif %}


!SLIDE bullets incremental

##Otras cosas que deberías de aprender...

* HTML
* Javascript (not enough JQuery...)
* Sysadmin
