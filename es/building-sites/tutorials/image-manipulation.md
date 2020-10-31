---
title: "Manipulación de imágenes"
_old_id: "347"
_old_uri: "2.x/case-studies-and-tutorials/quick-and-easy-modx-tutorials/automated-server-side-image-editing"
---

## Introduction

¿No quieres esperar diez segundos para que se cargue Photoshop? Yo tampoco. Este tutorial te presentará el maravilloso script PHP [phpthumbof](http://phpthumb.sourceforge.net/)  portado a  [MODX](/extras/phpthumbof "phpThumbOf") por Shaun McCormick.
[phpthumbof](/extras/phpthumbof "phpThumbOf") permite que MODX genere automáticamente miniaturas de un ancho y alto elegido por nosotros. Además, hay mil millones de otras funciones interesantes para realizar trabajos de imagen básicos que están a nuestra disposición. Hablemos de algunos de los más útiles.

Este tutorial asume que sabes cómo crear y llamar [variables de plantilla](building-sites/elements/template-variables "Variables de plantilla"). Conocer los [filtros de salida](building-sites/tag-syntax/output-filters "Filtros de entrada y salida") también es útil, pero seguiremos haciendo que las cosas funcionen aunque  no las conozcas.

Cada uno de estos ejemplos asumirá que has creado una Variable de plantilla de imagen llamada `big_image`, que se aplica al documento actual, que has especificado una imagen válida y que estás listo para impresionar a los demás. tus clientes, amigos y familiares. Empecemos.

**Advertencia sobre rendimiento**
Si estás en un servidor compartido, recuerda que el procesamiento excesivo de imágenes puede afectar a otros usuarios. Su anfitrión puede ponerse en contacto contigo y/o suspender tu cuenta si causa problemas. Reducir el tamaño de la imagen, incluso si no tiene las dimensiones exactas, reducirá el uso de recursos y el tiempo de procesamiento.

## Uso básico (cambio de tamaño y recorte)

El uso más obvio de phpthumbof es generar miniaturas a partir de imágenes más grandes. Ya no tienes que preocuparte de que tus clientes proporcionen imágenes que sean demasiado grandes: ¡traiga esas fotos de 5 MB 4800x3000! Supongamos que deseas cambiar el tamaño de tu foto de 5 MB a algo que tenga 960 píxeles de ancho y 300 de alto. Llamamos al snippet phpthumbof como un [filtro de salida](building-sites/tag-syntax/output-filters "Filtros de entrada y salida") y pasamos un ancho (w) de 960 y una altura (h) de 300:

``` php
[[*big_image:phpthumbof=`w=960&h=300`]]
```

Impresionante, nuestra imagen ahora tiene el tamaño correcto. Desafortunadamente, a menos que la imagen tenga la relación de aspecto correcta, es posible que tengamos algo de relleno en una dirección. Si prefieres recortar la dimensión más larga y hacer que la imagen se ajuste al cuadro, puedes usar el parámetro de recorte de zoom (zc):

``` php
[[*big_image:phpthumbof=`w=960&h=300&zc=1`]]
```

¡Se ve bien!

## Quitar fondos

¿Tienes un montón de imágenes con un fondo blanco (o de cualquier color) que quieras convertir en un png transparente? Vamos a hacerlo. Necesitamos usar uno de los filtros de phpthumb, "stc". STC significa "color transparente de origen".

Para nuestro ejemplo, lo mantendremos en 960x300 y sacaremos un fondo blanco (#FFFFFF). También lo convertiremos a png para usar esa acción de transparencia:

``` php
[[*big_image:phpthumbof=`w=960&h=300&fltr[]=stc|ffffff&f=png`]]
```

¡Buen trabajo!

## Desaturando

También podemos hacer muchas otras cosas usando los filtros de phpthumb. Desaturemos la imagen en un 90%.

``` php
[[*big_image:phpthumbof=`w=960&h=300&fltr[]=sat|-90`]]
```

¡Interesante!

## Coloreando

¿Quieres teñir la imagen? ¡Podemos hacerlo! Tiñámosla un 30% con # ff00ff:

``` php
[[*big_image:phpthumbof=`w=960&h=300&fltr[]=clr|30|ff00ff`]]
```

¡Genial!

## Encadenamiento

Todo esto está genial, pero podemos hacerlo mejor. Lo bueno de estos efectos es que se pueden encadenar.

Desaturemos completamente la imagen, aclaremos un 20% y luego tiñamos un 6% con # 00ab86:

``` php
[[*big_image:phpthumbof=`w=960&h=300&fltr[]=gray&fltr[]=brit|20&fltr[]=clr|6|00ab86`]]
```

Con un encadenamiento muy fino.

## Más lectura

Realmente solo hemos descubierto la punta del iceberg. phpthumb tiene muchos otros usos, [documentados en el sitio web de phpthumb](http://phpthumb.sourceforge.net/). ¡Ve y haz cosas geniales! Una vez que te sientas cómodo con lo anterior, revisa el [léame de phpthumb](http://phpthumb.sourceforge.net/demo/docs/phpthumb.readme.txt) y prepárate para que tu mente vuelva a volar. [Aquí](http://www.belafontecode.com/image-manipulation-with-phpthumbof-in-modx-revolution/) hay otro tutorial de phpthumb.
