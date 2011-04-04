---
title: Introducción a trabajo en grupo con git y github.com 
layout: post
---

En esta corta guía aprenderemos a usar [git](http://git-scm.com) en conjunto con [github.com](http://github.com) para administrar los cambios de nuestro código y trabajar en equipo ordenadamente. 


##Prerrequisitos##

Para seguir exitosamente esta guía, se debe tener creada una cuenta en [github.com](http://github.com) y se debe haber agregado la llave pública ssh al perfil de github personal. Aquí hay una guía para [crear y agregar la llave pública](http://help.github.com/linux-key-setup/). Evidentemente, se asume que se tiene ya instalado git (se instala con `sudo apt-get git-core`).

Para familiarizarse con git, se puede leer este [tutorial/sitio de referencia](http://gitref.org/) escrito por uno de los creadores de github.com.

##Preludio##

En esta guía usaré un repositorio de ejemplo llamado `ejemplo_heroku`. Los integrantes del grupo son [John Galt](http://github.com/jgalt) y [Luis Borjas](http://github.com/lfborjas). El repositorio original es `progra4/ejemplo_heroku`. Cada uno de ellos hará una copia remota de ese y luego la clonará a su computadora personal. Estarán siguiendo el modelo distribuido de trabajo: ambos tienen copias remotas y actualizan sus propios cambios a ellas, trayendo ocasionalmente cambios del otro y actualizando el repositorio original en las etapas de *integración*. Ese modelo de trabajo  encontrar explicado [aquí (Integration manager workflow)](http://whygitisbetterthanx.com/#any-workflow), donde ambos cumplen el rol de `developer` e `integration manager`. Esa manera de trabajar se puede resumir en la siguiente ilustración:

![Integration manager wf](http://whygitisbetterthanx.com/images/workflow-b.png)

A continuación, detallaré la historia desde cómo cada uno de ellos hará sus copias locales y trabajará hasta la primera integración del proyecto. 

##Encontrando el repositorio de tu grupo##

Si fuéramos el usuario `jgalt` al entrar a github.com veríamos un *dashboard* o "pizarra", para los que lo tienen en español. Ese dashboard corresponde a la cuenta personal de `jgalt`, tiene una lista de sus repositorios, noticias de programadores que sigue, etc. **Pero**, se cambiará de contexto, para, en vez de ver sus cosas personales, ver lo de progra 4: así que al entrar a github, vería una pantalla así (nótese el menú de cambio de contexto en la parte media derecha):

![Entrando a github](http://farm5.static.flickr.com/4145/5085547406_b35fa924c4.jpg)

En el menú de cambio de contexto, si da click en progra4, estaría cambiándose de contexto y vería algo así:

![Cambio de contexto](http://farm5.static.flickr.com/4083/5084974167_81c2e72035.jpg)

Como se ve en la imagen, ahora John Galt ve noticias de lo que pasa en la [organización](http://github.com/blog/674-introducing-organizations) progra4 y una lista de los repositorios a los que tiene acceso. Al dar click en el repositorio que tiene el nombre de su grupo (en la imagen, el grupo es `ejemplo_heroku`) John Galt vería lo siguiente:

![Viendo el repositorio del grupo](http://farm5.static.flickr.com/4109/5085602426_65881b84f3.jpg)

En este y en cualquier repositorio que se tenga acceso se pueden notar dos cosas:

1. Hay un botón que permite **hacer una copia (fork)** del repositorio, eso implica poner un nuevo repositorio *a nombre de uno* basado en el que se está viendo. 
2. Hay una **url de protocolo git**, con esta, se puede  *agregar el repositorio que se está viendo como un remoto* y así poder hacer `pull` y hasta `push` (si se tiene permiso, como es el caso del repositorio del grupo de John Galt).

##Crear tu propia copia remota del repositorio del grupo##

Ahora que John Galt encontró el repositorio de su grupo, hará una copia remota: de esta manera, tendrá su propia versión del repositorio original sobre la cual podrá hacer cambios (teniendo el respaldo en internet) sin preocuparse por *arruinar el código para los demás*. Esa es una de las ventajas de git: **permite trabajar de manera no centralizada** (a diferencia de [otros sistemas de control de versiones](http://whygitisbetterthanx.com/#distributed)).

Para hacer la copia remota, John Galt da click en el botón de `fork` (o `hacer copia`) y ve algo como esto:

![Luego de un fork](http://farm5.static.flickr.com/4103/5085045531_2654de18b3.jpg)

##Crear una copia local del fork que acabás de hacer##

Ok, John Galt ya tiene su copia remota del repositorio original de su grupo (Luis Borjas también tiene una). Pero aquí lo más que puede hacer es ver código y el historial de cambios, y John Galt lo que quiere es ¡comenzar a trabajar!. Excelente. Hora de hacer una copia local.

John Galt nota, como se ve en la foto de arriba, que hay una url de git ahí donde hizo el fork, así que la copiará al portapapeles. Abre una terminal y hace `cd` a una carpeta donde hará hacer el clon (en el caso de este usuario, será `/home/johngalt/proyectos`) y ahí ejecuta lo siguiente:

    git clone git@github.com:jgalt/ejemplo_heroku.git

Si todo sale bien, debería aparecer algo muy similar a esto:

    Initialized empty Git repository in /home/johngalt/proyectos/ejemplo_heroku/.git/
    remote: Counting objects: 39, done.
    remote: Compressing objects: 100% (21/21), done.
    remote: Total 39 (delta 15), reused 39 (delta 15)
    Receiving objects: 100% (39/39), 6.53 KiB, done.
    Resolving deltas: 100% (15/15), done.

Ahora John Galt tiene una carpeta llamada igual que el repositorio que acaba de clonar (en este ejemplo, la carpeta es `ejemplo_heroku`). Se cambia a esa carpeta (ejecuta `cd ejemplo_heroku`) **y se encuentra dentro de su nueva copia local del proyecto, de ahora en adelante, todos los comandos se asumirá que los ejecuta dentro de ésta**.

##Agregar a tus compañeros como remotos##

En su nueva copia local, si Galt ejecuta esto
    
    git remote

Le saldría lo siguiente
    
    origin

Esto quiere decir que tiene un *apuntador* a un repositorio remoto: **su** propia copia remota. Pero esto de `remote` es más poderoso que eso: puede tener apuntadores a las **copias remotas de sus compañeros y al mismísimo repositorio original (el blessed repository)**. 

Digamos que el apodo de Luis Borjas es `luisfelipe` y el de John Galt es `galt`. Estas dos personas quieren tener en su lista de repositorios remotos al otro. Nuestro John Galt quiere agregar al repositorio de Luis Felipe como remoto (nótese que esto no es un clon ni nada, sólo una referencia a otro repositorio, útil, como veremos después, para traer cambios de allá e integrarlos con los propios). En fin, para agregar la referencia a la copia remota de Luis Borjas, Galt ejecuta algo como esto:

    git remote add luisfelipe git@github.com:lfborjas/ejemplo_heroku.git

Y, al ejecutar `git remote` para listar sus remotos, vería algo como esto:

    origin
    luisfelipe

Si quisiera también tener una referencia al repositorio original (para cuando le toque hacer integraciones) ejecutaría algo como esto:
    
    git remote add blessed_repo git@github.com:progra4/ejemplo_heroku.git

Y la ejecución de `git remote` ahora resultaría en
    
    origin
    luisfelipe
    blessed_repo


Y también Luis Borjas (y cualquier otro miembro del grupo) podría hacer algo como eso para cuantas personas quiera. Nótese que el último parámetro del comando es una **URI de copia**, como la que vimos en la foto del fork.

Todo esto que acabamos de hacer (tener una copia remota, hacer un clon local y agregar referencias a otros repositorios para integración) se puede apreciar en el siguiente diagrama:

![El modelo de integración](http://www.gliffy.com/pubdoc/2280827/L.png)

## Hacer tus propios cambios, actualizar tu copia remota con ellos y traer los cambios de otros##

El flujo de trabajo de git es sencillo

1. Uno edita uno o varios archivos: estos cambios se mantienen en algo conocido como `directorio de trabajo`
2. Uno ve qué cambios ha hecho con `git status` y qué ha cambiado con `git diff`
3. Cuando uno considere que los cambios que hizo sobre uno o más archivos están bien, hace un `git add`: en esta etapa pasarán a una etapa conocida como `índice`: le estás diciendo a git que esté pendiente de esos cambios.
4. Cuando uno quiere "guardar" en el historial permantente (en terminología de git, se conoce como el `repositorio`), ejecutará un `git commit`: los cambios se hacen definitivos y se agregan a la historia de cambios.
5. Cuando uno quiere actualizar el historial de su copia remota -todos los commits que ha hecho en una sesión de trabajo- ejecuta un `git push`: de esta manera se sincronizan los cambios locales con el historial remoto.
6. Cuando uno quiera traer los cambios de otros a tu copia local (integrar), hace un `git pull`

###Haciendo cambios y guardándolos en el historial###

John Galt, quien hizo su propia copia local del repositorio `jgalt/ejemplo_heroku`, decide editar el archivo `app.rb`. Cuando termina de editarlo, ejecuta `git status` y ve algo como esto:

    # On branch master
    # Changed but not updated:
    #   (use "git add <file>..." to update what will be committed)
    #   (use "git checkout -- <file>..." to discard changes in working directory)
    #
    #   modified:   app.rb
    #
    no changes added to commit (use "git add" and/or "git commit -a")

Resulta que John Galt es algo olvidadizo y no se acuerda qué cambió en el archivo `app.rb`, para recordarlo, ejecuta `git diff` y ve algo como esto: 

    diff --git a/app.rb b/app.rb
    index a5d800f..ea4e33a 100644
    --- a/app.rb
    +++ b/app.rb
    @@ -11,5 +11,11 @@ get('/'){send_file "index.html"}
     Esta otra linea
      =end
       
       -get('/'){erb :index}
       -eval %w[/hackernotes /codewar /mailmaniac].collect{
       |idea| "get('#{idea}'){@title = '#{idea.capitalize}'; erb :#{idea[1..-1]}
       +get '/' do
       +    erb :index
       +end
       +
       +get '/*' do 
       +    erb params[:splat][0].to_sym
       +end
       +


Como se puede observar, las líneas precedidas por el signo menos (`-`) son lo que él quitó y las precedidas por el signo mas (`+`), lo que él agregó. Decide que ese cambio es algo de lo que git debería estar al tanto, así que ejecuta `git add app.rb`.
Si volviese a ejecutar `git status`, esto es lo que vería:

    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #   modified:   app.rb
    #

Nótese que ahora git sabe que son cambios que se deben considerar, pero no los ha agregado al historial.

John Galt decide que esos cambios están listos, y antes de seguir trabajando, decide "guardar su progreso" (pensá en los commits de git como los checkpoints de los juegos). Galt ejecuta esto:

    git commit -m "Cambié app.rb porque el código de Luis era una afrenta a la sanidad"

Y la terminal le devuelve esto:

    [master 48a6d64] Cambié app.rb porque el código de Luis era una afrenta a la sanidad
    1 files changed, 10 insertions(+), 0 deletions(-)

Nótese que se usó el parámetro `-m` para agregar un mensaje: siempre es recomendable agregar mensajes descriptivos a los commits, para que los otros sepan qué hicimos sin tener que leer el código.

En cualquier momento de la sesión de trabajo, se puede ejecutar `git log` para ver el historial de todos los commits que se han hecho (con autor, fecha y todo).

###Actualizando tu copia remota con los cambios de tu copia local ###

Ahora, John Galt está a punto de apagar su computadora porque el trabajo fue extenuante, de modo que antes quiere dejar sincronizadas sus copias local y remota. Para eso, ejecuta el comando `git push origin master` y ve algo como esto:

    Counting objects: 9, done.
    Delta compression using up to 2 threads.
    Compressing objects: 100% (7/7), done.
    Writing objects: 100% (7/7), 923 bytes, done.
    Total 7 (delta 4), reused 0 (delta 0)
    To git@github.com:jgalt/ejemplo_heroku.git
       625f15a..7e59ec1  master -> master

Ahora la copia que está guardada en github.com está sincronizada con la local.

### Actualizando tu copia local con los cambios de las copias remotas de tus compañeros ###

En la madrugada, Galt ve en el newsfeed de github.com que Luis Borjas hizo un push a su copia remota, revisa esos cambios (en github.com se puede dar click en un commit y ver qué archivos se cambiaron y qué se cambió en cada uno) y se da cuenta que son importantes. Así que ejecuta lo siguiente

    git pull luisfelipe master

Y en la terminal ve algo como esto:

    remote: Counting objects: 7, done.
    remote: Compressing objects: 100% (4/4), done.
    remote: Total 4 (delta 3), reused 0 (delta 0)
    Unpacking objects: 100% (4/4), done.
    From github:lfborjas/ejemplo_heroku
     * branch            master     -> FETCH_HEAD
    Merge made by recursive.
        views/layout.erb |    2 +-
        1 files changed, 1 insertions(+), 1 deletions(-)


Luis Borjas cambió otro archivo y ahora John Galt **actualizó su copia local** con lo que Luis Felipe hizo en su copia remota. Ahora ambos tienen la misma versión del código: John Galt **integró** su código y el de Luis Felipe.

###Resumen del flujo de trabajo###

El flujo de trabajo se puede ver gráficamente así:

![Git workflow](http://www.gliffy.com/pubdoc/2280486/L.png)

###Resolución de conflictos###

Pero no todo es felicidad en el mundo de git: a veces más de una persona trabaja en un mismo archivo que otras y todo se pone ... interesante.

Digamos que esta vez Luis Borjas decide trabajar en el archivo `app.rb` y hace un commit de sus cambios. Pero, hace unos párrafos, vimos que John Galt también había trabajado en ese archivo, y, para colmo, Luis Borjas no ha actualizado su copia local con los cambios que John hizo. Se da cuenta que John Galt hizo esos cambios y decide ejecutar esto:

    git pull galt master

Pero ve, muy a su disgusto, que en la terminal aparece lo siguiente:

    remote: Counting objects: 12, done.
    remote: Compressing objects: 100% (10/10), done.
    remote: Total 10 (delta 5), reused 0 (delta 0)
    Unpacking objects: 100% (10/10), done.
    From github.com:jgalt/ejemplo_heroku
     * branch            master     -> FETCH_HEAD
     Auto-merging app.rb
     CONFLICT (content): Merge conflict in app.rb
     Automatic merge failed; fix conflicts and then commit the result.

¡Oh no! hubo un conflicto y git dice que sucedió en el archivo `app.rb`. Si Luis ejecutase un `git status`, vería esto:

    # On branch master
    # Unmerged paths:
    #   (use "git add/rm <file>..." as appropriate to mark resolution)
    #
    #   both modified:      app.rb
    #
    no changes added to commit (use "git add" and/or "git commit -a")

En efecto, git le está diciendo que ambos tocaron ese archivo y le está pidiendo que lo revise. Luis abre el archivo con `vim` (porque otros editores de texto, como `gedit` le dan acidez y depresión) y se encuentra con algo así:

    <<<<<<< HEAD
    get('/'){erb :index}
    eval %w[/hackernotes /codewar /mailmaniac].collect{
    |idea| "get('#{idea}'){@title = '#{idea.capitalize}'; erb :#{idea[1..-1]
    =======
    get '/' do
        erb :index
    end

    get '/*' do 
        erb params[:splat][0].to_sym
    end
    >>>>>>> 858397fb3cc403b39921d2bcdf445c9ecaed0b18

Como se ve, todo lo que está entre separadores etiquetado como `HEAD` es lo que Luis hizo. Y lo que está en la otra porción, lo que John Galt hizo. Luis acepta que el código de Galt es mejor, así que decide borrar la porción suya y dejar la de Galt. Para terminar, hace un `commit` para decirle a git que ya resolvió el conflicto y todo está bien otra vez.

###Integración de cambios###

Recordemos que el modelo de trabajo que John Galt y Luis Borjas están usando es uno distribuido: ambos tienen referencias a sus propias copias remotas (listadas como `origin`), referencias al otro (en la computadora de John Galt, la referencia al repositorio de Luis Borjas está listada como  `luisfelipe`) y una referencia al repositorio original (a la cual le pusieron `blessed_repo`). Al final de la semana, John Galt ejecuta un `git pull luisfelipe master` para integrar los cambios de Luis Borjas a los suyos y descubre que la aplicación funciona perfectamente (luego de resolver un par de conflictos). Llama a Luis y deciden que hoy él será el *integrador*: subirá esta versión del proyecto al repositorio original.
Así que ejecuta `git push blessed_repo master` y de este modo esta versión estable del proyecto está en el repositorio original y ambos pueden ejecutar un `git pull blessed_repo master` para obtenerla. La otra semana, Luis podría jugar el papel de integrador, no importa quién lo haga, lo que importa es que **sólo hagan push a ese repositorio de versiones estables del proyecto**. Las versiones inestables las pueden mantener perfectamente en sus copias remotas personales; así, si alguno cambia de computadora o quiere saber qué fue lo último estable que el grupo decidió, puede hacer un pull.

Para sacarle provecho a git, se recomienda leer el [tutorial recomendado por github](http://gitref.org) y quizá el [artículo de uno de los creadores de github](http://tom.preston-werner.com/2009/05/19/the-git-parable.html) sobre la razón de ser de git.

Para ayuda sobre los distintos aspectos de github, se recomienda la [ayuda oficial](http://help.github.com/). 
