---
title: "Empezando a desarrollar"
_old_id: "153"
_old_uri: "2.x/developing-in-modx/overview-of-modx-development/developer-introduction/getting-started-developing"
---

## Programación en MODX Revolution

MODX Revolution es un Framework OOP, construido alrededor de la base de datos ORM [xPDO](extending-modx/xpdo "Inicio").

## Componentes de terceros (3PCs)

Los componentes de terceros (3PC) son colecciones de cualquier tipo de objetos MODX. Pueden ser una colección de Snippets, Plugins y Chunks, o un solo Snippet, o simplemente una colección de archivos. Por lo general, se transportan e instalan a través de [Paquetes de transporte](extending-modx/transport-packages "Paquetes de transporte").

## core/components y assets/components

MODX no limita necesariamente dónde puede colocar sus archivos de componentes personalizados de terceros, pero tenemos algunas recomendaciones. Para los archivos que no necesitan estar en la raíz web (archivos de configuración, .php, etc.), recomendamos colocarlos en:

> core/components/myname

Por tanto, si tuvieras un componente llamado 'prueba', pondrías tus archivos que no son webroot en "core/components/prueba/". Para archivos que necesitan ser accesibles desde la web, como css, js y otros archivos, recomendamos:

> assets/components/myname

Así que, para 'prueba', "assets/components/prueba". Esta estandarización de rutas facilita que otros desarrolladores que usan tus componentes encuentren tus archivos fácilmente.

## [Snippets](extending-modx/snippets "Snippets")

Los Snippets son simplemente scripts php que se pueden ejecutar en cualquier página u otro elemento. Son la piedra angular del desarrollo de MODX y la personalización dinámica. Puedes leer más sobre Snippets [aquí](extending-modx/snippets "Snippets").

## [Plugins](extending-modx/plugins "Plugins")

Los [Plugins](extending-modx/plugins "Plugins") son similares a los snippets en que son fragmentos de código que tienen acceso a la API MODX; sin embargo, la gran diferencia es que los plugins están asociados a eventos específicos del sistema. Por ejemplo, en una solicitud de página MODX estandard, ocurren varios eventos en ciertos puntos dentro del proceso de análisis de la página y se pueden adjuntar plugings a cualquiera de estos eventos para cumplir una función deseada. Los [Plugins](extending-modx/plugins "Plugins") no se limitan solo al procesamiento del front-end, hay muchos eventos que están disponibles en el Administrador MODX.

## [Propiedades y conjuntos de propiedades](building-sites/properties-and-property-sets "Propiedades y conjuntos de propiedades")

Las propiedades son simplemente marcadores de posición en los elementos (Snippets/Plugins/Chunks/VdPs/Plantillas), que se pueden analizar por cada elemento individual. Permiten la personalización y el paso de argumentos a cada elemento.

Los conjuntos de propiedades son agrupaciones de propiedades definidas por el usuario que se pueden utilizar para simplificar rápidamente la sintaxis en las llamadas de etiquetas personalizadas.

Puedes encontrar más información sobre conjuntos de propiedades [aquí](building-sites/properties-and-property-sets "Propiedades y conjuntos de propiedades").

## [Páginas de administrador personalizadas](extending-modx/custom-manager-pages "Páginas de administrador personalizadas") (CMPs)

Las [Páginas de administrador personalizadas](extending-modx/custom-manager-pages  "Páginas de administrador personalizadas"), o CMPs, son páginas personalizadas en el administrador creadas por desarrolladores de extras para permitir la administración de Componentes. Usan los objetos `modAction` y` modMenu` para crear dinámicamente páginas de administrador que se pueden modificar y agregar fácilmente sin modificar el core.

## Uso de MODX externamente

Usar el objeto MODX (y todas sus clases respectivas) es bastante simple. Todo lo que necesitas es este código:

``` php
require_once '/absolute/path/to/modx/config.core.php';
require_once MODX_CORE_PATH.'model/modx/modx.class.php';
$modx = new modX();
$modx->initialize('web');
$modx->getService('error', 'error.modError');
```

Esto inicializará el objeto MODX en el  [Contexto](building-sites/contexts "Contextos") 'web'. Ahora, si deseas acceder a él en un [Contexto](building-sites/contexts "Contextos") diferente (y, por lo tanto, cambiar sus permisos de acceso, políticas, etc.), solo tendrás que cambiar 'web' por cualquier [Contexto](building-sites/contexts "Contextos") que desées cargar.
