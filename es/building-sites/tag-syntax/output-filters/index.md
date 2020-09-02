---
title: "Filtros/modificadores de salida"
_old_id: "164"
_old_uri: "2.x/making-sites-with-modx/customizing-content/input-and-output-filters-(output-modifiers)"
---

## ¿Qué son los filtros?

Los filtros en Revolution te permiten manipular la forma en que se presentan o analizan los datos en una etiqueta. Te permiten modificar valores directamente desde tus plantillas.

## Filtro de entrada

[Filtro de entrada](building-sites/tag-sintax/input-filter.md)

## Filtro de salida

En Revolution, el filtro de salida aplica una o más series de modificadores de salida; su sintaxis se ve así:

``` php
[[elemento:modificador=`valor`]]
```

También se pueden encadenar (ejecutados de izquierda a derecha):

``` php
[[elemento:modificador:otromodificador=`valor`:yotromodificador:yotromas=`valor2`]]
```

También puedes utilizarlos para modificar la salida de un Snippet; ten en cuenta que el modificador viene después del nombre del Snippet y antes del signo de interrogación, por ej.

``` php
[[miSnippet:modificador=`valor`? &miSnippetParam=`algo`]]
```

Si tienes un código más largo en una declaración :then=``:else=`` y deseas hacerlo más legible colocándolo en varias líneas, debes hacerlo así:

``` php
[[+placeholder:is=`0`:then=`
 // code
`:else=`
 // code
`]]
```

## Modificadores de salida

La siguiente tabla enumera algunos de los modificadores existentes y muestra ejemplos de su uso. Aunque los ejemplos siguientes son etiquetas de marcador de posición, los modificadores de salida se pueden usar con cualquier etiqueta de MODX. **Asegúrate de que el marcador de posición utilizado esté recibiendo datos.**

### Modificadores de salida condicionales

| Modificador                                                     | Descripción                                                                                                             | Ejemplo                                                                                                      |
| ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------ |
| if, input                                                    | if, input                                                                                                               | -                                                                                                            |
| or                                                           | Se puede utilizar para encadenar modificadores de salida unidos con una relación "OR".                                              | ```[[+numlibros::is=`5`:or:is=`6`:then=`¡Hay 5 o 6 libros!`:else=`No sabemos cuantos libros hay`]]```           |
| and                                                          | Se puede utilizar para encadenar modificadores de salida unidos con una relación "AND".                                             |                                                                                                              |
| isequalto, isequal, equalto, equals, is, eq                  | Se compara con un valor pasado y continúa si coincide. Usado con "then" y "else"                                  | ```[[+numlibros::isequalto=`5`:then=`¡Hay 5 libros!`:else=`No sabemos cuantos libros hay`]]```                   |
| notequalto, notequals, isnt, isnot, neq, ne                  | Se compara con un valor pasado y continúa si no coincide. Usado con "then" y "else"                             | ```[[+numlibros:notequalto=`5`:then=`No sabemos cuantos libros hay`:else=`¡Hay 5 libros!`]]```                  |
| greaterthanorequalto, equalorgreaterthen, ge, eg, isgte, gte | Se compara con un valor pasado y continúa si es mayor o igual que el valor. Se usa con "then" y "else".      | ```[[+numlibros:gte=`5`:then=`Hay 5 libros o más`:else=`Hay menos 5 libros`]]``` |
| isgreaterthan, greaterthan, isgt, gt                         | Se compara con un valor pasado y continúa si es mayor que el valor. Se usa con "then" y "else".                  | ```[[+numlibros:gt=`5`:then=`Hay más de 5 libros`:else=`Hay menos de 5 libros`]]```             |
| equaltoorlessthan, lessthanorequalto, el, le, islte, lte     | Se compara con un valor pasado y sigue adelante si es menor o igual que el valor. Se usa con "then" y "else".         | ```[[+numlibros:lte=`5`:then=`Hay 5 o menos de 5 libros`:else=`Hay más de 5 libros`]]```       |
| islowerthan, islessthan, lowerthan, lessthan, islt, lt       | Se compara con un valor pasado y sigue adelante si es menos del valor. Se usa con "then" y "else".                     | ```[[+numlibros:lt=`5`:then=`Hay menos de 5 libros`:else=`Hay más de 5 libros`]]```             |
| contains                                                     | Comprueba si el valor contiene una cadena pasada.                                                                        | ```[[+author:contains=`Samuel Clemens`:then=`Mark Twain`]]```                                                |
| containsnot                                                  | Comprueba si el valor no contiene la cadena pasada.                                                           | ```[[+author:containsnot=`Samuel Clemens`:then=`Alguien más`]]```                                          |
| in, IN, inarray, inArray                                     | Comprueba si el valor está en un array(valores separados por comas)                                                              | ```[[+id:in=`5,15,22`:then=`Sí, está en el array`]]```                                                               |
| hide                                                         | Comprobará las condiciones anteriores y ocultará el elemento si se cumplieron las condiciones.                                         | ```[[+numlibros:lt=`1`:hide]]```                                                                              |
| show                                                         | Comprobará las condiciones anteriores y mostrará el elemento si se cumplieron las condiciones.                                         | ```[[+numlibros:gt=`0`:show]]```                                                                              |
| then                                                         | Uso condicional.                                                                                                      | ```[[+numlibros:gt=`0`:then=`¡Disponible ahora!`]]```                                                             |
| else                                                         | Uso de condicional, junto con then.                                                                                  | ```[[+numlibros:gt=`0`:then=`¡Disponible ahora!`:else=`Lo sentimos, actualmente agotado.`]]```                           |
| select                                                       | Genera un reemplazo, si el valor se encuentra en la lista de valores antes del signo igual. De lo contrario, el resultado está vacío. | ```[[+numlibros:select=`0=Value 0&1=Value 1&2=Value 2`]]```                                                   |
| memberof, ismember, mo                                       | Comprueba si el usuario es miembro de los grupos especificados.                                                               | ```[[+modx.user.id:memberof=`Administrator`]]```                                                             |

### Modificadores de salida de cadena

| Modificador                                 | Descripción                                                                                                                                                                                                                                                 | Ejemplo                                                |
| ---------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------ |
| cat                                      | Agrega el valor de la opción (si no está vacío) al valor de entrada                                                                                                                                                                                                | ```[[+numlibros:cat=`libros`]]```                        |
| after, append                            | Agrega el valor de las opciones al valor de entrada (si ambos no están vacíos) Añadido en 2.6.0.                                                                                                                                                                            | ```[[+totalnumber:after=`total`]]```                   |
| before, prepend                          | Antepone el valor de las opciones al valor de entrada (si ambos no están vacíos) Añadido en 2.6.0.                                                                                                                                                                          | ```[[+booknum:before=`book #`]]```                     |
| lcase, lowercase, strtolower             | Transforma cadenas a minúsculas. Similar a la función de PHP [strtolower](http://www.php.net/manual/es/function.strtolower.php)                                                                                                                                        | `[[+titulo:lcase]]`                                     |
| ucase, uppercase, strtoupper             | Transforma cadenas a mayúsculas. Similar a la función de PHP [strtoupper](http://www.php.net/manual/es/function.strtoupper.php)                                                                                                                                        | `[[+cabecera:ucase]]`                                  |
| ucwords                                  | Transforma la primera letra de una palabra en mayúscula. Similar a la función de PHP [ucwords](http://www.php.net/manual/es/function.ucwords.php)                                                                                                                           | `[[+titulo:ucwords]]`                                   |
| ucfirst                                  | Transforma la primera letra de la cadena en mayúsculas. Similar a [ucfirst](http://www.php.net/manual/es/function.ucfirst.php)                                                                                                                       | `[[+nombre:ucfirst]]`                                    |
| htmlent, htmlentities                    | Reemplaza cualquier carácter que tenga una entidad HTML con esa entidad. Similar a la función de PHP [htmlentities](http://www.php.net/manual/es/function.htmlentities.php). Utiliza el valor actual de la configuración del sistema `modx_charset` con el indicador `ENT_QUOTES`                   | `[[+email:htmlent]]`                                   |
| esc,escape                               | Evita de forma segura los valores de los caracteres utilizando expresiones regulares y str\ _replace.                                                                                                                                                                                               | También evita \[, \] y `[[+email:escape]]`            |
| strip                                    | Reemplaza todos los saltos de línea, tabulaciones y espacios múltiples con un solo espacio.                                                                                                                                                                                      | `[[+textodocumento:strip]]`                              |
| stripString                              | Elimina la cadena del valor especificado                                                                                                                                                                                                                            | ```[[+nombre:stripString=`Sr.`]]```                      |
| replace                                  | Reemplaza un valor por otro                                                                                                                                                                                                                             | ```[[+pagetitle:replace=`Mr.==Mrs.`]]```               |
| striptags, stripTags,notags,strip\_tags  | Elimina las etiquetas HTML de la entrada. Opcionalmente acepta un valor para indicar qué etiquetas permitir. Similar a la función de PHP  [strip\ _tags](http://www.php.net/manual/es/function.strip-tags.php)                                                                          | ```[[+code:strip_tags=`  `]]```                        |
| stripmodxtags                            | Elimina las etiquetas MODX de la entrada. (añadido en v2.7)                                                                                                                                                                                                           | ```[[+code:stripmodxtags]]```                          |
| len,length, strlen                       | Cuenta la longitud de la cadena pasada. Similar a [strlen](http://www.php.net/manual/es/function.strlen.php)                                                                                                                                         | `[[+longstring:strlen]]`                               |
| reverse, strrev                          | Invierte la entrada, carácter a carácter. Similar a [strrev](http://www.php.net/manual/es/function.strrev.php)                                                                                                                                     | `[[+textoespejo:reverse]]`                              |
| wordwrap                                 | Inserta un carácter de nueva línea después de la cantidad establecida de caracteres. Similar a [wordwrap](http://www.php.net/manual/es/function.wordwrap.php). Toma un valor opcional para establecer la posición del ajuste de palabras.                                                             | ```[[+bodytext:wordwrap=`80`]]```                      |
| wordwrapcut                              | Inserta un carácter de nueva línea después de la cantidad establecida de caracteres, independientemente de los límites de las palabras. Similar a [wordwrap](http://www.php.net/manual/es/function.wordwrap.php), con el corte de palabras habilitado. Toma un valor opcional para establecer la posición del ajuste de palabras. | ```[[+bodytext:wordwrapcut=`80`]]```                   |
| limit                                    | Limita una cadena a un cierto número de caracteres. El valor predeterminado es 100.                                                                                                                                                                                         | ```[[+description:limit=`50`]]```                      |
| ellipsis                                 | Agrega puntos suspensivos y trunca una cadena si tiene más de un cierto número de caracteres. Solo usa espacios como puntos de interrupción. El valor predeterminado es 100.                                                                                                            | ```[[+description:ellipsis=`50`]]```                   |
| tag                                      | Muestra el elemento sin formato sin la etiqueta. Útil para documentación.                                                                                                                                                                                       | `[[+showThis:tag]]`                                    |
| tvLabel                                  | Muestra la Etiqueta de una VdP (*variable de plantilla*). Útil cuando se usan selectores o casillas de verificación, etc. donde se usa `Etiqueta==1llOtra etiqueta==2llMás opciones==3`, con lo que si el valor fuese 2, esto devolvería Otra etiqueta.                                                                         | `[[+mySelectboxTv:tvLabel]]`                           |
| math                                     | Devuelve el resultado de un cálculo avanzado (Costoso para el procesador. No recomendado) Eliminado en Revolution 2.2.6.                                                                                                                                        | ```[[+blackjack:add=`21`]]```                          |
| add,increment,incr                       | Devuelve la entrada incrementada por la opción (predeterminado: +1)                                                                                                                                                                                                           | `[[+downloads:incr]]`                                  |
| subtract,decrement,decr                  | Devuelve la entrada decrementada por la opción (predeterminado: -1)                                                                                                                                                                                                           | `[[+countdown:decr]]` ```[[+moneys:subtract=`100`]]``` |
| multiply,mpy                             | Devuelve la entrada multiplicada por la opción (predeterminado: \*2)                                                                                                                                                                                                           | ```[[+trifecta:mpy=`3`]```                             |
| divide,div                               | Devuelve la entrada dividida por la opción (predeterminado: /2). No acepta 0.                                                                                                                                                                                            | ```[[+rating:div=`4`]]```                              |
| modulus,mod                              | Devuelve el módulo de la opción sobre la entrada (predeterminado:%2, devuelve 0 o 1)                                                                                                                                                                                           | `[[+numero:mod]]` o ```[[+numero:mod=`3`]]```         |
| ifempty,default,empty, isempty           | Devuelve el valor si la entrada está vacía                                                                                                                                                                                                                            | ```[[+nombre:default=`anonymous`]]```                    |
| notempty, !empty, ifnotempty, isnotempty | Devuelve el valor si la entrada no está vacía                                                                                                                                                                                                                        | ```[[[[+nombre:notempty=`¡Hola `[[+nombre]]`!`]]```        |
| nl2br                                    | Convierte un carácter de nueva línea (\\ n) en un elemento html. Usa esto si estás recibiendo entradas y crées que debería haber nuevas líneas y no las hay. Similar a la función de PHP [nl2br](http://www.php.net/manual/es/function.nl2br.php).                                                                            | `[[+textfile:nl2br]]`                                  |
| date                                     | Formatea una marca de tiempo Unix en un formato diferente. Similar a [strftime](http://www.php.net/manual/es/function.strftime.php). El valor es el formato. Consulta [Formatos de fecha](building-sites/tag-syntax/date-formats "Formatos de fecha").                              | ```[[+birthyear:date=`%Y`]]```                         |
| strtotime                                | Convierte una cadena de fecha en una marca de tiempo Unix. Útil para combinar esto con el filtro de salida de fecha. Similar a [strtotime](http://www.php.net/strtotime). Toma una fecha. Consulta [Formatos de fecha](building-sites/tag-syntax/date-formats "Formatos de fecha").   | `[[+thetime:strtotime]]`                               |
| fuzzydate                                | Devuelve un bonito formato de fecha con ayer y hoy filtrados. Toma una fecha.                                                                                                                                                                      | `[[+createdon:fuzzydate]]`                             |
| ago                                      | Devuelve un bonito formato de fecha en segundos, minutos, semanas o meses. Toma una fecha (strtotime).                                                                                                                                                         | ```[[+createdon:date=`%d-%m-%Y`:ago]]```               |
| md5                                      | Crea un hash MD5 de la cadena de entrada. Similar a la función de PHP  [md5](http://www.php.net/manual/es/function.md5.php).                                                                                                                                             | `[[+password:md5]]`                                    |
| cdata                                    | Envuelve el texto con etiquetas CDATA                                                                                                                                                                                                                              | `[[+content:cdata]]`                                   |
| userinfo                                 | Devuelve los datos de usuario solicitados. El elemento debe ser un ID de modUser. El campo de valor es la columna a tomar, por ej. fullname, email. Ver los ejemplos a continuación.                                                                                                         | ```[[+modx.user.id:userinfo=`username`]]```            |
| isloggedin                               | Devuelve verdadero si el usuario está autenticado en este contexto.                                                                                                                                                                                                      | `[[+modx.user.id:isloggedin]]`                         |
| isnotloggedin                            | Devuelve verdadero si el usuario no está autenticado en este contexto.                                                                                                                                                                                                  | `[[+modx.user.id:isnotloggedin]]`                      |
| toPlaceholder                            | Pone el valor de entrada en el marcador de posición pasado. No impide la salida del valor de VdP, así que agrega ```[[*algunaVdP:toPlaceholder =`placeholder`:notempty =``]]```si no deseas generar el valor de la VdP en sí.                                     | ```[[*someTV:toPlaceholder=`placeholder`]]```          |
| cssToHead                                | Coloca un elemento `<link>` en <head>, donde el valor de entrada se coloca dentro del atributo href. Utiliza [modX.regClientCSS](extending-modx/modx-class/reference/modx.regclientcss "modX.regClientCSS").                                                                                                                                                             | `[[+cssTV:cssToHead]]`                                 |
| htmlToHead                               | Inserta un bloque de código HTML en el encabezado de la página, antes de `</head>`. Utiliza [modX.regClientStartupHTMLBlock](extending-modx/modx-class/reference/modx.regclientstartuphtmlblock "modX.regClientStartupHTMLBlock")                                                                                                                       | `[[+htmlTV:htmlToHead]]`                               |
| htmlToBottom                             | Inserta el código HTML al final de la página, antes de `</body>`. Utiliza [modX.regClientHTMLBlock](extending-modx/modx-class/reference/modx.regclienthtmlblock "modX.regClientHTMLBlock").                                                                           | `[[+htmlTV:htmlToBottom]]`                             |
| jsToHead                                 | Inserta el código JS (o un enlace) en el encabezado de la página, antes de `</head>`. Utiliza [modX.regClientStartupScript](extending-modx/modx-class/reference/modx.regclientstartupscript "modX.regClientStartupScript").                                                 | `[[+jsTV:jsToHead]]`                                   |
| jsToBottom                               | Inserta el código JS (o un enlace) al final de la página, antes de `</body>`. Utiliza [modX.regClientScript](extending-modx/modx-class/reference/modx.regclientscript "modX.regClientScript").                                                                        | `[[+jsTV:jsToBottom]]`                                 |
| urlencode                                | Convierte la entrada en una cadena compatible con URL similar a cómo lo haría un formulario HTML. Similar a la función de PHP [urlencode](http://www.php.net/manual/es/function.urlencode.php)                                                                                    | `[[+mystring:urlencode]]`                              |
| urldecode                                | Convierte la entrada de una cadena compatible con URL. Similar a [urldecode](http://www.php.net/manual/es/function.urldecode.php)                                                                                                                           | `[[+myparam:urldecode]]`                               |
| filterPathSegment                        | Agregado en 2.7. Convierte la entrada en una cadena compatible con URL con el mismo mecanismo que convierte un título de página en un alias, incluida la transliteración si está habilitada. Útil para URL personalizadas.                                                                     | `[[+pagetitle:filterPathSegment]]`                     |

### Almacenamiento en caché

En general, cualquier contenido de un marcador de posición que creas que **podría cambiar dinámicamente**, debe eliminarse en caché. Por ejemplo:

``` php
[[+placeholder:default=`A default value!`]]
```

Lo que significa que esto podría, **a veces**, estar vacío y, a veces, no. ¿Por qué querrías eso en caché? Eso eliminaría el punto del modificador de salida.

A veces, los modificadores de salida se pueden usar en un marcador de posición guardado en caché, pero solo si se está llamando al snippet que los establece también guardado en caché. De lo contrario, estás realizando una maniobra ilógica: tratando de almacenar en caché estáticamente algo que nunca estuvo destinado a ser estático.

En general, la regla es: si estableces un marcador de posición en un snippet no almacenado en caché, el marcador de posición también debe eliminarse en caché si esperas que el contenido del marcador de posición sea diferente.

### Uso de un modificador de salida con propiedades de etiqueta

Si tienes propiedades en la etiqueta, querrás especificarlas **después** del modificador:

``` php
[[!getResources:default=`Disculpas, su búsqueda no tuvo resultados.`?
    &tplFirst=`blogTpl`
    &parents=`2,3,4,8`
    &tvFilters=`blog_tags==%[[!tag:htmlent]]%`
    &includeTVs=`1`
]]
```

### Crear un modificador de salida personalizado

Además, los [Snippets](extending-modx/snippets "Snippets") se pueden usar como modificadores personalizados. Simplemente ponga el nombre del [Snippet](extending-modx/snippets "Snippets") en lugar del modificador. Ejemplo con un snippet llamado 'makeExciting' que agrega una cantidad variable de signos de exclamación:

``` php
[[*pagetitle:makeExciting=`4`]]
```

Esta llamada de variable de documento con un modificador de salida pasará estas propiedades al snippet:

| Parametro   | Valor                             | Ejemplo de resultado                        |
| ------- | --------------------------------- | ------------------------------------- |
| input   | El valor del elemento              | El valor de `[[*pagetitle]]`         |
| options | Cualquier valor pasado al modificador. | '4'                                   |
| token   | El tipo de elemento padre.   | \* (el token en `pagetitle`)         |
| name    | El nombre del elemento padre.   | pagetitle                             |
| tag     | La etiqueta completa.          | ```[[*pagetitle:makeExciting=`4`]]``` |

Aquí hay una implementación de muestra para nuestro snippet makeExciting:

``` php
$defaultExcitementLevel = 1;
$result = $input;
if (isset($options)) {
    $numberOfExclamations = $options;
} else {
    $numberOfExclamations = $defaultExcitementLevel;
}
for ( $i = $numberOfExclamations; $i > 0; $i-- ) {
    $result = $result . '!';
}
return $result;
```

El valor de retorno de la llamada será el que devuelva el snippet . Para nuestro ejemplo, el resultado será el valor de la variable del documento pagetitle con cuatro signos de exclamación añadidos al final.

El valor de entrada original se devolverá si el snippet devuelve una cadena vacía.

## Encadenamiento (múltiples filtros de salida)

Un buen ejemplo de encadenamiento sería formatear una cadena de fecha a otro formato, así:

``` php
[[+mydate:strtotime:date=`%Y-%m-%d`]]
```

El acceso directo a la tabla `modx_user_attributes` en la base de datos usando modificadores de salida en lugar de un [Snippet](extending-modx/snippets "Snippets") se puede lograr simplemente utilizando el modificador userinfo. Selecciona la columna adecuada de la tabla y especifícala como propiedad del modificador de salida, así:

``` php
Clave interna del usuario: [[!+modx.user.id:userinfo=`internalKey`]]<br />
Nombre de usuario: [[!+modx.user.id:userinfo=`username`]]<br />
Nombre completo: [[!+modx.user.id:userinfo=`fullname`]]<br />
Role:  [[!+modx.user.id:userinfo=`role`]]<br />
E-mail: [[!+modx.user.id:userinfo=`email`]]<br />
Teléfono: [[!+modx.user.id:userinfo=`phone`]]<br />
Teléfono móvil: [[!+modx.user.id:userinfo=`mobilephone`]]<br />
Fax: [[!+modx.user.id:userinfo=`fax`]]<br />
Fecha de nacimiento: [[!+modx.user.id:userinfo=`dob`:date=`%d-%m-%Y`]]<br />
Género: [[!+modx.user.id:userinfo=`gender`]]<br />
País: [[+modx.user.id:userinfo=`country`]]<br />
Estado: [[+modx.user.id:userinfo=`state`]]<br />
Código postal: [[+modx.user.id:userinfo=`zip`]]<br />
Foto: [[+modx.user.id:userinfo=`photo`]]<br />
Comentario: [[+modx.user.id:userinfo=`comment`]]<br />
Contraseña: [[+modx.user.id:userinfo=`password`]]<br />
Contraseña de caché: [[+modx.user.id:userinfo=`cachepwd`]]<br />
Último acceso: [[+modx.user.id:userinfo=`lastlogin`:date=`%d-%m-%Y`]]<br />
El inicio de sesión:[[+modx.user.id:userinfo=`thislogin`:date=`%d-%m-%Y`]]<br />d
Número de inicios de sesión: [[+modx.user.id:userinfo=`logincount`]]
```

`[[! + modx.user.id]]` tiene como valor predeterminado el ID de usuario actualmente conectado. Por supuesto, puedes reemplazarlo con `[[* createdby]]` u otro campo de recursos o marcadores de posición que devuelvan una ID numérica que represente a un usuario.

Ten en cuenta que el ID de usuario y el nombre de usuario ya están disponibles de forma predeterminada en MODX, por lo que no es necesario utilizar el modificador "userinfo":

``` php
[[!+modx.user.id]] - Imprime el ID
[[!+modx.user.username]] - Imprime el nombre de usuario
```

Lo más probable es que desees llamarlos no almacenados en caché (consulta la nota sobre el almacenamiento en caché más arriba) para evitar resultados inesperados.

## Ver también

- [Propiedades y conjuntos de propiedades](building-sites/properties-and-property-sets "Propiedades y conjuntos de propiedades")
- [Plantillas](building-sites/elements/templates "Plantillas")
- [Variables de Plantilla](building-sites/elements/template-variables "TVariables de Plantilla")
- [Snippets](extending-modx/snippets "Snippets")
