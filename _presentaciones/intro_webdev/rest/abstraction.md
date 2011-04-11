!SLIDE bullets

##REST: la web como recursos

* En bases de datos: `CREATE`, `READ`, `UPDATE`, `DESTROY`
* En HTTP: `POST`, `GET`, `PUT`, `DELETE`
* En sistemas de archivos: `/home/lfborjas/conquistar_el_mundo.txt`
* En HTTP: `http://unblogx.com/posts/zalgo`


!SLIDE bullets

##REST: usar el protocolo

* *Malo*: `GET http://misitio.com/index.php?func=editarPerfil&nombre=luis&id=666`
* *Bueno*: `POST http://misitio.com/perfiles/666`
* REST aprovecha los verbos y la semántica de las rutas, así como los códigos de estado.
