---
layout: post
title: Empezando, instalando cosas, creando cuentas y configurando
---

###La clase en unos pocos párrafos###

¡Bienvenidos a desarrollo de aplicaciones web! Como ya les debí mencionar, esta clase se trata de desarrollo web. Mi único fin es que al final de estos tres meses se *sepan* capaces de crear una aplicación web de verdad en el lenguaje/framework que deseen. Para este fin, ustedes van a **elegir una idea de una aplicación simple, pero útil** y deberán desarrollar un [producto mínimo viable](http://en.wikipedia.org/wiki/Minimum_viable_product) en **grupos por afinidad de, máximo, tres personas**.

Durante el curso vamos a estar usando el lenguaje de programación [python](http://python.org/); para explorar los conceptos preliminares usaremos el framework [flask](http://flask.pocoo.org/) y para el proyecto de la clase, un framework más maduro y hecho para sitios más grandes: [django](http://www.djangoproject.com/). 

Vamos a usar control de versiones, en concreto, [git](http://git-scm.com/). Los proyectos del grupo van a tener repositorios remotos privados de github asignados en la [página de github de la clase](http://github.com/desarrollo-web) en la que encontrarán, además, los proyectos de muestra. 

Todos los links que mencione en el curso estarán disponibles en [mi cuenta de delicious](http://delicious.com/lfborjas/desarrollo-web).

###Cuentas en internet qué crear###

* Para estarnos comunicando (sílabo, tareas, nuevas entradas en este nerdlog, etc) vamos a estar usando [class.io](http://my.class.io/course/desarrolloweb). Sí, es la onda que Jorge García, Fernando Escher, yo y los tipos de [icoms technologies](http://demos.icomstec.com/) estamos haciendo. Así que siéntanse libres de dar retroalimentación, aún está en súper beta. **Creen una cuenta con su correo de unitec.edu**
* Para administrar el código, tenemos una [organización de github](http://github.com/desarrollo-web). Ahí estaré supervisando el progreso en sus proyectos (pista: piensen en eso como su backup, no como el lugar donde entregan cosas cada vez que se piden, espero ver commits diarios, no semanales o mensuales). **Así que creen una cuenta ahí y me avisan para agregarlos a la org, de preferencia, agreguen un avatar y sigan a mi usuario (lfborjas) por conveniencia y porque soy narcisista**
* Para tener los proyectos en internerd, vamos a usar [google app engine](http://code.google.com/appengine/), **así que, también, creen una cuenta ahí** .

###Requerimientos técnicos y guía de instalación###

Deberán tener:
* Un sistema operativo linux, de preferencia [ubuntu](http://www.ubuntu.com/), versión **10.04** o mayor
* El sistema de gestión de versiones [git](http://git-scm.com/)
* El lenguaje de programación [python](http://python.org), versión **2.X**, para X > 4. Existe python 3.0 pero **no es recomendable instalarlo**.
* El sistema de bases de datos para desarrolladores [sqlite](http://www.sqlite.org/) 

A continuación, un poco de detalle de cómo instalar cada cosa (excepto ubuntu, ya deberían tenerlo o al menos saber instalarlo):

Para instalar git:

    sudo apt-get install git-core
    
También **deberán generar una llave pública y toda la onda**, todo ese proceso está [documentado en github](http://help.github.com/linux-set-up-git/).
Y ya que estén en eso, [RTFM](http://gitref.org/)

Para instalar setuptools:

    sudo apt-get install python-setuptools

Lo cual pondrá a tu disposición el comando `easy_install`, para instalar paquetes de python. Para probarlo, probá instalar `ipython`, una mejor consola de python:

    sudo easy_install ipython

Para instalar sqlite:

    sudo apt-get install sqlite


Instrucciones extra de instalación para django están en la [página oficial](http://docs.djangoproject.com/en/1.3/intro/install/), **para instalar django, no es necesario compilarlo a mano o usar apt-get, para eso tenemos easy_install**: `sudo easy_install Django`

En cuanto al entorno de desarrollo, dos IDEs recomendados son [pycharm](http://www.jetbrains.com/pycharm/) y [eclipse](http://www.eclipse.org/home/categories/index.php?category=ide). Pero la verdad es que con un editor como **gedit** o **vim** y la consola se tiene más que suficiente. En clase estaré usando vim y en un server es la mejor opción (frente a nano o sed), pero es elección de cada quién.
