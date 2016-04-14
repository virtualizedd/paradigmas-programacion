#Completando el lenguaje Kernel

Ahora que ya vimos todos los conceptos elementales que rodean al paradigma funciones, podemos completar el lenguaje kernel que dejamos pendiente en la sección anterior. Una vez definido este *lenguaje kernel completo*, lo usaremos para comprender qué hace exactamente un programa funcional, tal como hicimos la sección anterior cuando usamos su versión parcial para demostrar que las *funciones que van construyendo listas son recursivas por cola*. En la siguiente sección, de hecho, ya lo usaremos cómo parte de la semántica formal. Recuerda que junto con la *máquina abstracta*, es una de los partes principales que componen la semántica de un lenguaje. Adicionalmente, tal como se mencióno cuando se presentó la metodología de aprendizaje en la introducción, una vez comprendido el lenguaje kernel para el paradigma funcional, iremos comprendiendo los demás paradigmas extendiendo con nuevos conceptos el lenguaje kernel aquí expuesto, de modo que para cada paradigma que veamos presentaremos un lenguaje kernel que lo identifique, y que nos ayude a comprender su funcionamiento. Y es que, aunque incluso sea un sólo concepto lo que separe a un paradigma de otro, ese único concepto trae consigo formas muy diferentes de pensar en un problema.

Bien, este es el lenguaje kernel que hemos visto hasta ahora:

```
<s> :: = skip
        | <s>1 <s>2
        | declare <x>
        | <x>1 = <x>2
        | <x> = <v>
        | if <x> then <s>1 else <s>2 end
        | proc<x> (<x>1 ... <x>n) <s> end
        | <x>(<y>1 ... <y>n)
        | case <x> of <pattern> then <s>1 else <s>2 end


<v> :: = <number> | <list> | ...

<number> ::= <int> | <float>

<list>, <pattern> ::= nil | <x> | <x> '|' <list>
```

Incluye una serie de instrucciones, valores, y definiciones de valores. Por supuesto, aún nos faltan cosas. Ya mencionamos, por ejemplo, que nos faltaban incluir ciertos valores (y con ello sus definiciones), tal cómo se muestra con los puntos suspensivos que dejamos en esta definición. En específico, para convertir el lenguaje anterior en un lenguaje completo nos están faltando dos conceptos que hemos visto en esta sección: los procedimientos como valor y los registros. En nuestra definición actual los procedimientos se representan como instrucciones, y no cómo valores. Así que esa es una de las cosas que tenemos que cambiar. Realicemos ese cambio:

```
<s> :: = skip
        | <s>1 <s>2
        | declare <x>
        | <x>1 = <x>2
        | <x> = <v>
        | if <x> then <s>1 else <s>2 end
        | <x>(<y>1 ... <y>n)
        | case <x> of <pattern> then <s>1 else <s>2 end

<v> :: = <number> | <list> | <procedimiento> | ...

<number> ::= <int> | <float>

<list>, <pattern> ::= nil | <x> | <x> '|' <list>

<procedimiento> ::= proc(<x>1 ... <x>n) <s> end
```

Eliminamos la descripción como instrucción que teníamos para los procedimientos, y los incluimos como un nuevo tipo de valor en la definición de <v>. Adicionalmente, incluimos una definición para este «nuevo tipo», en la forma de un procedimiento anónimo con la sintaxis que hemos estado usando en esta sección. Bien, con ello, ya podemos dar por incluida la idea de «procedimiento como valor» a nuestro lenguaje kernel. El otro cambio que nos estaría quedando es incluir a los registros. De hecho, dado que como hemos visto, en base a los registros podemos construir todos los otros tipos compuestos, entonces podemos simplificar el lenguaje kernel especificando a los registros como su único tipo compuesto. Ahora, dado que hasta el momento sólo hemos presentado a las listas entre los tipos compuestos, son el único tipo que hace falta eliminar al momento de incluir a los registros. Realicemos esta inclusión:

```
<s> :: = skip
        | <s>1 <s>2
        | declare <x>
        | <x>1 = <x>2
        | <x> = <v>
        | if <x> then <s>1 else <s>2 end
        | <x>(<y>1 ... <y>n)
        | case <x> of <pattern> then <s>1 else <s>2 end

<v> :: = <number> | <procedimiento> | <registro>

<number> ::= <int> | <float>

<registro>, <pattern> ::= <literal> | <literal>(<f>1 : <x>1 ... <f>n : <x>n)

<procedimiento> ::= proc(<x>1 ... <x>n) <s> end
```

Como mencionamos, eliminamos las listas entre los valores del lenguaje kernel, y añadimos en su lugar a los registros. De aquí en adelante, tanto las listas como otros datos compuestos, como los árboles, se podrán definir en términos de registros. Podemos decir, por lo tanto, que con esta inclusión estamos incluyendo de forma generalizada a todos los otros tipos compuestos al mismo tiempo. Bien, junto a la adición de los registros como un nuevo tipo de valor, especificamos más abajo su definición. En este caso, la definición también sigue de cerca la sintaxis que hemos estado usando para representarlos. Un registro «se define como» un literal (o símbolo) o como un literal asociado a una colección de pares <f>:<x>, nombre de campo y campo, respectivamente. Nota como, en particular, los campos se definen como variables.

Una vez que incluimos estos dos conceptos, ya no nos queda nada más por incluir, ni valores ni instrucciones. Ahora ya contamos con un lenguaje kernel completo que reúne todos los conceptos de la programación funcional y con el que podemos expresar cualquier programa bajo este paradigma. Y junto a ello, analizarlo, observar sus propiedades, y comprenderlo en detalle. Adicionalmente, usando este lenguaje kernel en la siguiente sección podremos definir su semántica. También, aún más a futuro, podremos definir construcciones como las abstracciones de datos de la programación orientada a objetos. Bien, en adición a todo esto, vale la pena también destacar que cada una de los conceptos, cada una de las instrucciones y valores que se incluyen en él están allí por un motivo. Todos son importantes, ya sea en un sentido de ayuda al programador (case), de incrementar el potencial de los datos con los que se pueda trabajar (registros), o de servir como un concepto fundamental, sin el cual simplemente no ha paradigma funcional. Por ejemplo, la instrucción `skip` de la que poco se ha dicho, permite finalizar instrucciones antes de tiempo, o especificar, por ejemplo, que una instrucción `if` o `case` no incluya su parte `else`.

### Comparando los distintos enfoques al estudio de los lenguajes de programación

Para finalizar esta sección, permíteme regresar a uno de los primeros temas. Específicamente, a un tema que concierne a la metodología usada en este curso. Cómo el enfoque del lenguaje kernel que usamos aquí se sitúa a sí mismo con respecto a otros enfoques de estudio para comprender y definir la ejecución de los lenguajes. En general, se podría decir que existen tres enfoques principales. Por supuesto, uno de ellos es el enfoque del lenguaje kernel. Los otros dos son el uso de las bases del cálculo, y el uso de una máquina virtual. Claramente, en los tres casos comenzamos con una serie de lenguajes de programación prácticos como nuestro objeto de estudio. Y la pregunta es, ¿cómo hacemos para estudiar a todos estos lenguajes bajo una perspectiva unificada?. En ello surgen las tres opciones mencionadas. Cada una ofrece un mirada distinta, y por lo tanto, sirve para un propósito distinto, como se muestra a continuación.

####Enfoque del cálculo fundamental

Como se puede inferir, en este enfoque analizamos a los lenguajes de programación desde la perspectiva de las matemáticas. Es útil cuando queremos hacer justamente ello, estudiar matemáticamente las propiedades de los distintos lenguajes. Para ello, los lenguajes son reducidos a un mínimo posible de conceptos, y sobre este conjunto mínimo se realizan demostraciones de la expresividad de otros conceptos y de propiedades que caractericen al lenguaje, además de comparaciones entre distintos lenguajes. Dado que las matemáticas son la herramienta más rigurosa para el análisis, este enfoque tiene la ventaja de ser la forma más sólida de comprender la esencia de los lenguajes de programación. Sin embargo, dado que los conceptos se ven reducidos al mínimo, es muy complicado realizar programación real con este enfoque. Para ello, es necesario, decodificar cada uno de estos conceptos mínimos a algo con lo que se pueda representar la programación, así que para programar no es un enfoque muy eficiente.

####Enfoque de la máquina virtual

La meta de este enfoque es modelar la ejecución eficiente de un lenguaje sobre una máquina real. Para ello se define la idea de «máquina virtual», y se le incorporan conceptos relacionados a la arquitectura del procesador que son necesarios para que el lenguaje se ejecute eficientemente. El ejemplo más conocido de máquina virtual es la máquina virtual de Java, usada para la ejecución del lenguaje del mismo nombre. Este enfoque también puede ser usado por diseñadores de compiladores con el objetivo de definir una máquina virtual para compilar el código de un lenguaje en particular a código máquina eficiente.

####Enfoque del lenguaje kernel

Este es el enfoque que tomamos en este curso. Aquí la meta en sí misma, no es ni matemática ni definir lo necesario para la ejecución eficiente de un lenguaje de programación. Aquí la meta es ayudar al programador a comprender y razonar sobre los programas que escribe. Cada uno de los lenguajes kernel que se ven en este curso son ejemplos de este enfoque. Y en cada caso, los conceptos elegidos para definir a un lenguaje kernel van asociados a un paradigma en particular, de modo que el programador pueda expresar todo lo que quiera, al mismo tiempo que se alínea a un paradigma en específico. Adicionalmente, estos conceptos no son mínimos, como en el estudio matemático, ni están vinculados a características de hardware, como la arquitectura del procesador. Por el contrario, son conceptos elegidos para que el programador pueda comprender distintas técnicas de programación. Ahora, por supuesto, cada uno de los conceptos involucrados puede tener su propia implementación (la definición no es lo mismo que la implementación), pero en base a estas definiciones el programador puede comprender una serie de técnicas, paradigmas en un sentido general, sencillo, riguroso, y compacto.
