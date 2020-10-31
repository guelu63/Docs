---
title: "Elementos y recursos estáticos"
_old_id: "195"
_old_uri: "2.x/case-studies-and-tutorials/managing-resources-and-elements-via-svn"
nota: 'Este documento no describe un flujo de trabajo completo para el uso de recursos y elementos estáticos, y le vendría bien una reescritura.'
---

## El problema

Cuando se trabaja en colaboración, los equipos de desarrolladores y diseñadores a menudo colaboran a través de Subversion (SVN) para facilitar el desarrollo entre varias personas. MODX, sin embargo, almacena sus datos en la base de datos. Esto tiene muchos beneficios en general, pero en el código almacenado en la base de datos no puede controlarse la versión a través de SVN.

Sin embargo, la solución en MODX Revolution es bastante simple.

## La solución

Para Recursos, es simple. Simplemente usa [Recursos estáticos](building-sites/resources/static-resource "Recurso estático") y apunta el contenido a un archivo en tu repositorio de SVN.

Lo siguiente es relevante para versiones antiguas de MODX. Para MODX 2.2.x, al igual que con los recursos estáticos, simplemente usa [Static Elements](getting-started/maintenance/upgrading/2.2#Upgradingto2.2.x-StaticElements). Los elementos estáticos tienen la ventaja adicional de poder utilizar [Orígenes Multimedia](getting-started/maintenance/upgrading/2.2#Upgradingto2.2.x-MediaSources).

Para Elements, todo lo que necesitas es un simple "include" [snippet](extending-modx/snippets "Snippets"). El código:

``` php
if (!file_exists($file)) return '';
$o = include $file;
return $o;
```

Luego puedes llamarlo así en tus Recursos estáticos:

``` php
[[include? &file=`/path/to/my/svn/checkout/snippet.php`]]
```

Y listo. También puedes usar etiquetas dentro del parámetro 'file', como esta:

``` php
[[include? &file=`[[++assets_path]]/js/myscript.js`]]
```

## Conclusión

Esto te permite administrar fácilmente el contenido a través de SVN. También se puede lograr con [Plantillas](building-sites/elements/templates "Templates") y [VdP](building-sites/elements/template-variables "Variables de plantilla"); simplemente coloca el snippet de inclusión donde necesite archivos basados en el sistema de archivos.
