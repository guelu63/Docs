---
title: "Ejemplos de filtros de salida personalizados"
_old_id: "353"
_old_uri: "2.x/making-sites-with-modx/customizing-content/input-and-output-filters-(output-modifiers)/custom-output-filter-examples"
---

## Introducción

 Los filtros de salida personalizados son Snippets de MODX dedicados a formatear la salida de un marcador de posición (en una plantilla o en un chunk). Si un marcador de posición sin formato, por ejemplo:

``` php
[[*pagetitle]]
```

 devuelve una cadena de texto, puedes modificarla a través de un filtro de salida personalizado, por ej.

``` php
[[*pagetitle:miFiltroDeSalida]]
```

 simplemente creando un Snippet llamado **miFiltroDeSalida**

 En el ejemplo anterior, el valor del título de la página será modificado por un Snippet llamado**miFiltroDeSalida**

 Revisa la página [filtros de salida incorporados en MODX](building-sites/tag-syntax/output-filters) "Filtros de entrada y salida (modificadores de salida)") antes de escribir tu propio filtro.

## Crear un modificador de salida personalizado

 Al escribir tu propio modificador de salida, tu Snippet puede tomar las siguientes entradas:

``` php
$input; // el valor que se formatea/filtra
$options; // valores opcionales pasados a través de comillas invertidas
```

 Un filtro de salida personalizado es simplemente un [Snippet](extending-modx/snippets "Snippets") destinado a modificar contenido. Simplemente, pon el nombre del [Snippet](extending-modx/snippets "Snippets") en vez de un modificador genérico de MODX.

 La sintaxis es de manera que el nombre del snippet se pone después de dos puntos. Ejemplo con un snippet llamado 'crearDownloadLink':

``` php
[[+file:crearDownloadLink=`notitle`]]
```

 Esto pasará estas propiedades al snippet:

| Param   | Valor                             | Ejemplo de resultado                             |
| ------- | --------------------------------- | ------------------------------------------ |
| input   | El valor del elemento.              | El valor de `[[+file]]`                   |
| options | Cualquier valor pasado al modificador. | 'notitle'                                  |
| token   | El tipo del elemento padre.   | + (el token de `file`)                    |
| name    | El nombre del elemento padre.   | file                                       |
| tag     | La etiqueta completa del padre.          | ```[[+file:crearDownloadLink=`notitle`]]``` |

 El más importante (y quizás el más obvio) de estos parámetros es el parámetro **$input**. Tu Snippet podría hacer algo tan simple como esto:

``` php
return strtolower($input);
```

## Ejemplos

 Como los ejemplos que se encuentran a continuación no están incluidos en el core, deberás agregarlos tu mismo. Afortunadamente, MODX hace que esto sea ridículamente fácil. Puedes usar snippets como filtros de salida, por lo que el proceso de agregar un filtro de salida personalizado es simplemente agregar un nuevo snippet. Para usar el filtro de salida, haz referencia al nombre del snippet.

Para los contribuidores en la documentación: por favor, agregar ejemplos en orden alfabético.

### alternateClass

 __alternateClass__ simplemente comprueba si un número entero pasado (por ejemplo, un marcador de posición de conteo) es divisible por 2. Si eso es posible, devuelve la clase que se especifique como propiedad del filtro de salida.

``` php
<?php
/*
 * Based on phx:alternateClass by Smashingred
 * Updated for Revolution by Mark Hamstra
 */
if ($input % 2) {
  return $options;
} else {
  return ''; // Aquí se puede especificar otra clase 
}
?>
```

Usar así:

``` php
[[+component.idx:alternateClass=`alt`]]
```

### parseLinks

 El filtro de salida __parseLinks__ encuentra enlaces y los reemplaza con un `<a>`atributo`</a>` html . 

``` php
<?php
/*
 * Based on phx:parseLinks
 */
$t = $input;
$t = ereg_replace("[a-zA-Z]+://([.]?[a-zA-Z0-9_/-])*", "<a href=\"\\0\">\\0</a>", $t);
$t = ereg_replace("(^| |\n)(www([.]?[a-zA-Z0-9_/-])*)", "\\1<a href=\"http://\\2\">\\2</a>", $t);
return $t;
```

### parseTags

Este __parseTags__ toma la entrada como una lista delimitada por comas y hace que todas las etiquetas individuales sean un enlace al recurso 9 con el parámetro de consulta tag=tagname adjunto al enlace.

``` php
<?php
/*
 * Based on phx:parseLinks
 */
$t = $input;
$t = ereg_replace("[a-zA-Z]+://([.]?[a-zA-Z0-9_/-])*", "<a href=\"\\0\">\\0</a>", $t);
$t = ereg_replace("(^| |\n)(www([.]?[a-zA-Z0-9_/-])*)", "\\1<a href=\"http://\\2\">\\2</a>", $t);
return $t;
```

### parseTags

This parseTags takes input as a comma delimited list, and makes all individual tags a link to resource 9 with tag=tagname query parameter appended to the link.

``` php
<?php
/*
 * parseTags output filter
 * by Mark Hamstra (http://www.markhamstra.nl)
 * free to use / modify / distribute to your will
 */
if ($input == '') { return ''; } // Los filtros de salida también se procesan cuando la entrada está vacía, así que ten en cuenta eso.
  $tags = explode(', ',$input); // Basado en un delimitador de coma-espacio.
  foreach ($tags as $key => $value) { // Se procesan individualmente
    $output[] = '<a href="'.$modx->makeurl(9, '', array('tag' => $value)).'">'.$value.'</a>';
  }
  return implode(', ',$output); // Se delimita de nuevo con un  coma-espacio
```

### shorten

Esto acorta la entrada como :puntos suspensivos pero no trunca palabras. Por defecto, ajusta la longitud de máx. en 50 caracteres. Basado en código de gOmp.

``` php
<?php
$output = '';
$options = !empty($options)?$options:50;
if (!empty($input) && !empty($options)) {
  if (strlen($input) > $options) {
    $output = substr($input, 0, strrpos(substr($input, 0, $options), ' ')).' …';
  }
  else{
    $output = $input;
  }
}
return $output;
?>
```

### substring

Obtiene una subcadena de la entrada.

``` php
<?php
$options=explode(',',$options);
return count($options)>1 ? substr($input,$options[0],$options[1]) : substr($input,$options[0]);
?>
```

Ejemplo:

``` php
<span>[[*introtext:substring=`0,1`]]</span>[[*introtext:substring=`1`]]
```

### numberformat

[PHP: number_format](http://php.net/manual/en/function.number-format.php)

``` php
<?php
$number = floatval($input);
$optionsXpld = @explode('&', $options);
$optionsArray = array();
foreach ($optionsXpld as $xpld) {
    $params = @explode('=', $xpld);
    array_walk($params, function(&$v) { return $v = trim($v);});
    if (isset($params[1])) {
        $optionsArray[$params[0]] = $params[1];
    } else {
        $optionsArray[$params[0]] = '';
    }
}
$decimals = isset($optionsArray['decimals']) ? $optionsArray['decimals'] : null;
$dec_point = isset($optionsArray['dec_point']) ? $optionsArray['dec_point'] : null;
$thousands_sep = isset($optionsArray['thousands_sep']) ? $optionsArray['thousands_sep'] : null;
$output = number_format($number, $decimals, $dec_point, $thousands_sep);
return $output;
```

Ejemplo:

``` php
[[+price:numberformat=`&decimals=2&dec_point=,&thousands_sep=.`]]
```
