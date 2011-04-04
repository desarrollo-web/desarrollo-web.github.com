!SLIDE

#Python para programadores


!SLIDE bullets

##Si no tenés python

* Acá hay un [intérprete](http://shell.appspot.com/)
* Y acá un lugar donde [escribir y ver ejecutado código](http://codepad.org/)


!SLIDE bullets

#¿De qué hay que hablar?

* Cómo ejecutar programas
* Primitivos y sintaxis
* Combinación/Abstracción


!SLIDE bullets

##Generalidades

* Cuenta con un intérprete, que se ejecuta sólo
  escribiendo `python`
* Para ejecutar un programa, sólo hay que escribir
  `python` seguido del nombre de archivo
* Es *totalmente* orientado a objetos
* No necesita tipos explícitos, los *adivina*
* Python está basado en *indentación*.

!SLIDE smbullets

##Tipos primitivos
* Números: `1` (int), `666L` (long), `3.14` (float), `1 + 2j` (complex)
* Booleanos: `True`, `False`
  (cualquier cosa excepto 0, None y las secuencias vacías es implícitamente verdadera)
* Strings: `'sencillos'`, `"dobles"`, `"""triples"""`, `'''mas triples'''`
* Nulidad: `None`
* Secuencias: `[1,2,'a', True, False, [3,4,5]]` (listas), `(1, 2, 3, 'x', (None))` (tuplas)
* Colecciones: `{'a', 1, 'b': 2, 'c':"eso"}` (diccionarios), `set([1,2,3])` (conjuntos)

!SLIDE bullets

#Operaciones

* Las numéricas clásicas, junto a `**` (exponenciación) y `//`(división entera)
* Operaciones relacionales: `<`,  `>`, `<=`, `>=`, `!=`, `==`
* Operaciones booleanas: `not`, `and`, `or`
* Se importan cosas con `import x` o `from x import y,z,w` o `from x import *` o `from x import algo as y` 

!SLIDE smbullets

##Estructuras de control

* Comentarios empiezan con `#`
* Los bloques se identifican por dos puntos (`:`) e indentación
* `if...else`
* `if...elif..else`
* `while`
* `for ... in`
* `try... except...else`

!SLIDE code small
    
    @@@python
    from datetime import datetime as dt
    ahora = dt.now
    dia = ahora().day
    meses = {1 : "Enero", 2: "Febrero", 3: "Marzo"}

    if dia < 20:
        while ahora().second % 5 :
            print "%d no es múltiplo de 5" % ahora().second
    elif dia == 20:
        for i in range(1, ahora().month+1):
            print "Pasó el %d mes: %s " % (i, meses[i])
    else:
        print "Algo así", "es python", "esto es una tupla..."


!SLIDE smbullets

##Operaciones avanzadas de iterables y diccionarios

* List slicing: `lista[inicio:fin_inclusivo:salto]`
* Acceso a elementos (*existe el acceso negativo*): `l[indice], l[-indice]`
* Operadores sobrecargados: `[0]*5 == [0,0,0,0,0]; [1]+[2,3] == [1,2,3]; 5 in [1,2,3] == False`
* List comprehensions: `another_list = [exp for elem in list]`; `a = [exp for elem in list if cond]`
* Los *strings se  comportan como listas*
* Iteración en diccionarios: `for k,v in d.items()`
* Pertenencia `k in d`

!SLIDE code small

    @@@python
    import calendar
    #Obtengamos los nombres de los dias:
    dias = [calendar.day_name[i] for i in range(7)]
    #Si estan en ingles, traduzcamoslos
    if 'Monday' in dias:
        trad = dict(zip(dias, """Lunes Martes Miercoles
                                 Jueves Viernes Sabado
                                 Domingo""".split()))
    else:
        trad = dict([(e,e) for e in dias]) #Lo mismo que zip...
    
    #Veamos que hay adentro:
    for dia, trans in trad.items():
        print "%s se dice %s" % (dia, trans)
    
    print "El fin de semana", dias[4:7], dias[-3:]
    dias[5:7] = ["Dormir", "Ver Tele"]
    print "El verdadero fin de semana", dias[4:7]
    print "Yo trabajo estos dias", dias[:4]
    print "Los dias habiles impares", dias[0:5:2]
    print "La semana al reves", dias[::-1]
    

