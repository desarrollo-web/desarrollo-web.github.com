!SLIDE 

#Medios de abstracción: funciones

!SLIDE bullets

* En python, los archivos son *módulos*
* Cualquier cosa que pongamos ahí se puede importar
* Las funciones son como cualquier otro objeto
* Existen las *funciones anidadas*
* Y funciones que reciben o devuelven funciones

!SLIDE code

    @@@python
    def fib(n):
        a,b = 0,1
        nums = []
        while a < n:
            nums += [a,]
            a, b = b, a+b
        return nums
    print fib(2000)

!SLIDE bullets

* Usamos `def` para crear una función
* Si no tiene retorno explícito, retornará `None`
* No declaramos tipos de nada, es dinámico


!SLIDE code small

    @@@python
    def bubblesort(x, ascending=True, make_copy=False):
        x = x[:] if make_copy else x
        for i in range(len(x)):
            for j in range(len(x)-1, i, -1):
                if ascending:
                    if x[j-1] > x[j]:
                        x[j-1], x[j] = x[j], x[j-1]
                else:
                    if x[j-1] < x[j]:
                        x[j-1], x[j] = x[j], x[j-1]
        
        return x
    import random
    list= range(10)
    random.shuffle(list)
    print bubblesort(list) #la ordena ascendente y cambia
    random.shuffle(list)
    print bubblesort(list, False) #la ordena descendente y cambia
    print bubblesort(list, make_copy=True) #ascendente, no la cambia
    print bubblesort(make_copy=False, ascending=False, x=list)

!SLIDE bullets

* Los parámetros pueden tener valores por defecto, si no los mandamos, los tomarán
* Podemos alterar el orden de los parámetros si los mandamos con *nombre*
* Existen, entonces, dos tipos de parámetros: *posicionales y nombrados*
* *Si hay parámetros posicionales, tienen que ir primero*

!SLIDE code small
    @@@python 
    def sum_nums(*nums):
        acc = 0
        for num in nums:
            acc += num
        return acc
    
    print sum_nums(1,2,3,4) #=> 10
    l = range(11)
    print sum_nums(*l) #=> 55



!SLIDE code small
    @@@python 
    def print_hist(**table):
        for k, v in table.items():
            print "%s: %s" % (k, '#'*v)

    print_hist(e=3, a=2)
    t = {'x': 10, 'y': 3}
    print_hist(**t)

!SLIDE code small
    @@@python 
    def varargs(saludo, *args, **kwargs):
        print saludo
        if 'do_print' in kwargs and kwargs['do_print']:
            for arg in args:
                print arg
    
    varargs("hola") 
    varargs("hola", 1,2,3)
    varargs("mundo", 3,4,5, do_print=True)
    varargs("mundo", 3,4,5, do_print=False)
         

!SLIDE bullets

* Podemos recibir *parámetros variables*
* Los posicionales vienen en una lista
* Los nombrados, en un diccionario
* Para convertir una lista normal en una lista de args y viceversa, *empacamos* con el operador `*`
* Igual con parámetros nombrados, sólo que usamos el operador `**`

!SLIDE bullets

#Ojo con los parámetros por defecto, sus lugares son asignados **una vez**, no en cada llamada

!SLIDE code 

    @@@python
    def evil_fun(x, l=[]):
        l.append(x)
        return l

    evil_fun(3)
    evil_fun()
    evil_fun(4)
    evil_fun(5, [6])

!SLIDE code smaller
    @@@python
    def menor_que(a,b):
        return a < b
    def mayor_igual(a,b):
        return a >= b

    def quicksort(l, lt=menor_que, geq=mayor_igual):
        if l == []: return []
        else:
            s, xs = l[0], l[1:]
            return quicksort([e for e in xs if lt(e, s)], lt,geq)\
                   + [s]\
                   + quicksort([e for e in xs if geq(e, s)],lt,geq)

    import random
    list = range(10)
    random.shuffle(list)
    print quicksort(list)
    print list
    print quicksort(list, mayor_igual, menor_que)
    rand = random.randint
    #ordenar una lista de listas
    llist = [range(rand(1,5)) for e in range(1,5)]
    print quicksort(llist,
                    lt=lambda a,b: len(a) < len(b),
                    geq= lambda a,b: len(a) >= len(b))

!SLIDE bullets

* Podemos mandar funciones a otras funciones como parámetros
* `lambda` crea *funciones anónimas*

!SLIDE code
    
    @@@python
    def combine(f,g,h):
        def do_combination(x):
            return f(g(h(x)))
        
        return do_combination

    ua = combine(str.split,
                 str.title,
                 lambda s: s.replace('_',' '))
    print upper_alpha("a_veces_no_hay_espacio")

!SLIDE code
    
    @@@python
    def combine(f,g,h):
        return lambda x: f(g(h(x)))

    ua= combine(str.split,
                str.title,
                lambda s: s.replace('_',' '))
    print upper_alpha("a_veces_no_hay_espacio")

!SLIDE code small
    
    @@@python
    def partial(f, *partials):
        def application(*a, **kw):
            return f(*(partials + args), **kw)

        return application

    def sumas(x,y,z):
        return x+y+z

    print sumas(1,2,3)
    siempre_uno = partial(sumas, 1)
    print siempre_uno(2,3)
    siempre_uno_y_dos = partial(sumas, 1, 2)
    print siempre_uno_y_dos(3)

!SLIDE bullets
    
* Funciones que retornan funciones permiten hacer código más expresivo
* Nos podemos valer de funciones anidadas o de la forma especial *lambda*
* El uso más común de las funciones de orden superior es el patrón decorador:
  una función "plantilla" que modifica a otras y toma su lugar


!SLIDE code small

    @@@python
    import json
    
    def json_fun(f):
        def wrapper(json_string, *args, **kwargs):
            python_object = json.loads(json_string)
            result = f(python_object, *args, **kwargs) 
            return json.dumps(result)
        return wrapper
     
!SLIDE code
    @@@python
    def foo(arg):
        assert isinstance(arg, dict)
        arg.update({'bar': 42})
        return arg
    
    foo = json_fun(foo)

!SLIDE code

    @@@python
    @json_fun
    def foo(arg):
        assert isinstance(arg, dict)
        arg.update({'bar': 42})
        return arg

!SLIDE code small 

    @@@python
    import json, pickle
    def serialize(format="json"):
        def decorator(f):
            def wrapper(*args, **kwargs):
                result = f(*args, **kwargs)
                if format == 'json':
                    return json.dumps(result)
                elif format == 'pickle':
                    return pickle.dumps(result)
                else:
                    throw Exception("Invalid format")
            return wrapper
        return decorator

    @serialize()
    def fun(): return {'foo': 'bar'}

!SLIDE code small 
    @@@python
    class Punto(object):
        def __init__(self, x=0, y=0):
            self.x = x
            self.y = y
        def __str__(self):
            return "(%d, %d)" % (self.x, self.y)
        def dist(self, other):
            import math
            return math.sqrt((other.x-self.x)**2 + \
                             (other.y-self.y)**2)
        def __add__(self, o):
            return Punto(self.x+o.x, self.y+o.y)

    a = Punto(2,3)
    b = Punto()
    c = Punto(y=1)
    print b.dist(a)
    print c
    print a + b

!SLIDE bullets
* Las clases no tienen mucho de especial
* Herencia es con paréntesis después del nombre de la clase
* Instanciación es sólo una llamada al operador paréntesis (`__init__`)
* `self` es como `this`: un parámetro agregado por el entorno de ejecución
* Podemos *sobrecargar operadores*

!SLIDE code small
    @@@python
    
    def search(str, filename):
        #el segundo parametro es el modo,
        #r para leer, w para escribir, b para binario
        #a es para agregar
        file = open(filename, 'r') 
        count = 1
        for line in file.readlines():
            if str.lower() in line.lower():
                print "%d: %s" %(count, line)
            count += 1
        file.close()
        
    import sys
    if len(sys.argv) == 3:
        search(sys.argv[1], sys.argv[2])

!SLIDE bullets
##Más referencias

* [Tutorial oficial](http://docs.python.org/tutorial/index.html)
* [Python para programadores en java](http://python.computersci.org/Main/TableOfContents)
* [Idiomatic Python](http://python.net/~goodger/projects/pycon/2007/idiomatic/presentation.html)

!SLIDE bullets
##Libros recomendados

* [How to think like a computer scientist](http://openbookproject.net/thinkCSpy/)
* [Learn Python The Hard Way (pronto traducido a español)](http://learnpythonthehardway.org/index)
* [Invent your own computer games with python](http://inventwithpython.com/)
* [Python para todos](http://mundogeek.net/tutorial-python/)
* [A byte of python](http://www.swaroopch.com/notes/Python)
