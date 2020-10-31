---
title: "Creating a Blog"
_old_id: "68"
_old_uri: "2.x/case-studies-and-tutorials/creating-a-blog-in-modx-revolution"
---

## Requisitos

1. Puede ser requerido el uso de FURL y cambiar ".html" a "/" en  *Contenido->Tipos de contenido->HTML* (.html -> /)

## Creación de un blog en MODX Revolution

 Este tutorial está aquí para ayudarte a configurar una solución de blogs potente y flexible en MODX Revolution. Dado que MODX Revolution no es un software de blogs, sino más bien una plataforma de aplicaciones de contenido en toda regla, no viene pre-empaquetado con una solución de blogs sencilla. Necesitarás configurar tu blog como lo desées.

 Afortunadamente, las herramientas para hacerlo ya están a tu disposición. Este tutorial te mostrará cómo configurarlas. Se recomienda que estés familiarizado con la [Sintaxis de etiquetas](building-sites/tag-syntax "Sintaxis de etiquetas") antes de comenzar.

 No obstante, una cosa antes de comenzar: este tutorial es extenso y te mostrará cómo configurar un blog muy potente, con publicaciones, archivo, etiquetado, comentarios y más. Si no necesitas alguna parte específica, omite esa parte. MODX es modular y su blog puede funcionar en cualquier ámbito que desées. Y, de nuevo, esta es solo una forma de hacerlo: hay muchas más formas de configurar un blog en MODX Revolution.

 Este tutorial se basó en la configuración del blog en [splittingred.com](http://splittingred.com/). Si deseas una demostración antes de leer, intenta ahí.

## Obtener los extras necesarios

 En primer lugar, necesitarás descargar e instalar algunos extras que usaremos en nuestro blog. La siguiente es una lista de los extras más utilizados:

### Extras necesarios

- [getResources](/extras/getresources "getResources") - Para enumerar publicaciones, páginas y otros recursos.
- [getPage](/extras/getpage "getPage") - Para paginación de listados.
- [Quip](/extras/quip "Quip") - Para todo lo relacionado con comentarios.
- [tagger](/extras/tagger "tagger") - Para administrar etiquetas y realizar una navegación basada en etiquetas.
- [Archivist](/extras/archivist "Archivist") - Para administrar tu sección de Archivos.

### Extras opcionales

- [Breadcrumbs](/extras/breadcrumbs "Breadcrumbs") - Para mostrar una ruta de navegación.
- [Gallery](/extras/gallery "Gallery") - Para gestionar galerías de fotos.
- [SimpleSearch](/extras/simplesearch "SimpleSearch") - Para agregar un cuadro de búsqueda simple a tu sitio.
- [getFeed](/extras/getfeed "getFeed") - Si deseas obtener otros dispensadores en tu sitio, como un feed de Twitter.
- [Login](/extras/login "Login") - Si deseas restringir los comentarios a los usuarios registrados únicamente, lo necesitarás.

## Creación de tu plantilla de publicación del blog

 En primer lugar, querrás tener una plantilla diseñada solo para publicaciones del blog. ¿Por qué? Bueno, si deseas comentarios y un formato especial, o vistas de página para tu blog, probablemente no querrás tener que arreglar cada publicación del blog. Por lo tanto, la mejor solución es configurar tu propia plantilla de publicación de blog. Este tutorial ya asume que tienes una plantilla base para tus páginas normales del sitio; en adelante nos referiremos a esta como 'Plantilla Base'.

 Crearemos una llamada 'BlogPostTemplate'. Nuestro contenido se parecerá a esto:

``` php
[[$pageHeader]]
<div id="content" class="blog-post">
  <h2 class="title"><a href="[[~[[*id]]]]">[[*pagetitle]]</a></h2>
  <p class="post-info">
  Publicado el [[*publishedon:strtotime:date=`%b %d, %Y`]] |
  Etiquetas: [[*tags:notempty=`[[!tolinks? &items=`[[*tags]]` &tagKey=`tag` &target=`1`]]`]] |
  <a href="[[~[[*id]]]]#comments" class="comments">
    Comentarios ([[!QuipCount? &thread=`blog-post-[[*id]]`]])
  </a>
  </p>
  <div class="entry">
    <p>[[*introtext]]</p>
    <hr />
    [[*content]]
   </div>
  <div class="postmeta">
    [[*tags:notempty=`
<span class="tags">Etiquetas: [[!tolinks? &items=`[[*tags]]` &tagKey=`tag` &target=`1`]]</span>
    `]]
    <br class="clear" />
  </div>
  <hr />
  <div class="post-comments" id="comments">[[!Quip?
      &thread=`blog-post-[[*id]]`
      &replyResourceId=`123`
      &closeAfter=`30`
    ]]
    <br /><br />
    [[!QuipReply?
       &thread=`blog-post-[[*id]]`
       ¬ifyEmails=`my@email.com`
       &moderate=`1`
       &moderatorGroup=`Moderators`
       &closeAfter=`30`
    ]]
  </div>
</div>
[[$pageFooter]]
```

 Examinemos la plantilla. A medida que avanzamos, recuerda esto: puedes mover cualquiera de estas piezas, cambiar sus parámetros y ajustar su ubicación. Esta es únicamente una estructura base; si deseas que tus etiquetas estén en la parte inferior, por ejemplo, ¡muévelas allí! MODX no te impide hacer eso.

### Encabezado y pié de página

 Primero, notarás que tengo dos partes: "pageHeader" y "pageFooter". Estos fragmentos contienen mis etiquetas HTML comunes que colocaría en el pie de página y en el encabezado de mi sitio, por lo que puedo usarlas en diferentes plantillas. Útil si no quiero tener que actualizar ningún cambio de encabezado/pie de página en cada una de mis Plantillas; puedo hacerlo en un solo chunk. Después de eso, pongo el título de la página de mi recurso y lo convierto en un enlace que lo lleva a la misma página.

### La información de la publicación

 A continuación, entramos en la "información" de la publicación, básicamente el autor y las etiquetas de la publicación. Veamos en detalle:

``` php
<p class="post-info">
Publicado el [[*publishedon:strtotime:date=`%b %d, %Y`]] |
  Etiquetas: [[*tags:notempty=`[[!tolinks? &items=`[[*tags]]` &tagKey=`tag` &target=`1`]]`]] |
  <a href="[[~[[*id]]]]#comments" class="comments">
    Comentarios ([[!QuipCount? &thread=`blog-post-[[*id]]`]])
  </a>
</p>
```

 La primera parte toma el campo de recursos _publishedon_  y lo formatea en una fecha agradable y bonita.

 En segundo lugar, mostramos una lista de etiquetas para esta publicación de blog. Puedes ver cómo hacemos referencia a una variable de plantilla "tags" (todavía no la hemos creado, así que no te preocupes) y luego la pasamos como una propiedad al snippet 'tolinks'. El snippet de tolinks viene con [tagLister](/extras/taglister "tagLister") y traduce etiquetas delimitadas en enlaces. ¡Esto significa que se puede hacer clic en nuestras etiquetas! Hemos especificado el Recurso con id 1 como 'destino'(*target*) , que es nuestra página de inicio. Si tu blog estuviera en otra página además de la de inicio, cambiarías el número de target a su id.

 Y finalmente, cargamos un conteo rápido del número de comentarios, junto con un enlace de etiqueta de ancla en el que se puede hacer clic para cargarlos. Observa cómo nuestra propiedad 'hilo' en la llamada del snippet de QuipCount (y más adelante en la llamada de Quip) usa 'blog-post-`[[* id]]`'. Esto significa que MODX creará automáticamente un hilo nuevo para cada nueva publicación de blog que creemos. ¡Ordenado!

### El contenido de la publicación

 Bien, volvamos a nuestra plantilla. Ahora estamos en la sección de contenido; observa cómo comenzamos con "[[*introtext]]". Este es un campo de recursos MODX útil; considéralo como un extracto inicial de una publicación de blog, que aparecerá en nuestra página principal cuando enumeremos las últimas publicaciones del blog.

### Agregando comentarios a publicaciones

 Bien, ahora estamos en la parte de comentarios de BlogPostTemplate. Como puedes ver aquí, estamos usando [Quip](/extras/quip "Quip")  para nuestro sistema de comentarios. Si lo deseas, puedes usar otro sistema, como Disqus, aquí. Para este tutorial, usaremos Quip. Nuestro código es el siguiente:

``` php
<div class="post-comments" id="comments">[[!Quip?
    &thread=`blog-post-[[*id]]`
    &replyResourceId=`19`
    &closeAfter=`30`
  ]]
  <br /><br />
  [[!QuipReply?
     &thread=`blog-post-[[*id]]`
     &moderate=`1`
     &moderatorGroup=`Moderators`
     &closeAfter=`30`
  ]]
</div>
```

 Está bien, genial. Ten en cuenta que tenemos dos llamadas de snippets aquí: una para mostrar los comentarios de este hilo ([Quip](/extras/quip/quip.quip "Quip.Quip")), y otra para mostrar el formulario de respuesta ([QuipReply](/extras/quip/quip.quipreply "Quip.QuipReply")).

 En nuestra llamada al snippet de Quip, especificamos un ID de hilo de la manera que describimos anteriormente y luego ajustamos algunas configuraciones. Nuestros comentarios van a estar enhebrados (el valor predeterminado), por lo que necesitamos especificar un ID de recurso donde va a estar nuestra publicación *Responder al hilo* (esto se detalla en la [Documentación de Quip](/extras/quip "Quip"). Recomendamos leer ahí para saber cómo configurarlo) con la propiedad 'replyResourceId'. Por ejemplo rápido, si tu &replyResourceId apunta a la página 123, entonces en la página 123, debes poner algo como lo siguiente:

``` php
[[!QuipReply]]
<br />
[[!Quip]]
```

 A continuación, queremos especificar, tanto en las llamadas de Quip como de Quip Reply, una propiedad 'closeAfter'. Esto le dice a Quip que cierre automáticamente los comentarios sobre estos hilos después de 30 días de la creación del hilo (cuando lo cargamos).  
 
 En nuestra llamada de QuipReply, queremos decirle a Quip que modere todas las publicaciones, y los moderadores de nuestra publicación se pueden encontrar en el Grupo de usuarios de moderadores (explicaremos cómo configurar esto más adelante en el tutorial).

 Hay un montón de otras configuraciones de Quip que podríamos cambiar, pero te lo dejaremos para una mayor personalización, que puedes averiguar cómo hacerla en los [Documentos de Quip](/extras/quip "Quip").

 **¿Qué es un Threading?(*Enhebrado*)**
 
 Si habilitas los comentarios _threaded_ (*enhebrados*), los usuarios pueden comentar otros comentarios. Los comentarios sin hilos permiten a los usuarios comentar solo en la publicación original del blog.

## Configuración del etiquetado

 Ahora que tenemos nuestra plantilla completamente configurada, necesitamos configurar la variable de plantilla 'tags' que usaremos para nuestro etiquetado.

 Continúa creando una variable de plantilla llamada 'tags' y ponle una descripción como "Etiquetas delimitadas por comas para el recurso actual". A continuación, asegúrate de que tenga acceso a la plantilla 'BlogPostTemplate' que creamos anteriormente.

 ![](tags-tv1.png)

 ¡Eso es! Ahora podrás agregar etiquetas a cualquier publicación de blog que creemos, simplemente al editar tu recurso especificando una lista de etiquetas separadas por comas.

## Creando las Secciones

 Si deseas que tu blog tenga 'Secciones' (también llamadas Categorías), primero deberás crear esos Recursos.

 Para el propósito de este tutorial, crearemos 2 secciones: "Personal" y "Tecnología". Continúa y crea 2 recursos en la raíz de tu sitio y conviértelos en "contenedores". Querrás  hacer que su alias sea "personal" y "tecnología", para que las URL de las publicaciones de su blog se vean bien.

 De ahora en adelante diremos que nuestros dos Recursos de Sección tienen ID de 34 y 35, como referencia.

 Asegúrate de no utilizar BlogPostTemplate en estos y utiliza en su lugar tu propia plantilla base. Estas páginas terminarán siendo una forma de navegar por todas las publicaciones dentro de una sección determinada. En el contenido de estos Recursos,  pon lo siguiente:

``` php
[[!getResourcesTag?
  &element=`getResources`
  &elementClass=`modSnippet`
  &tpl=`blogPost`
  &hideContainers=`1`
  &pageVarKey=`page`
  &parents=`[[*id]]`
  &includeTVs=`1`
  &includeContent=`1`
]]
[[!+page.nav:notempty=`
<div class="paging">  
<ul class="pageList">  
  [[!+page.nav]]  
</ul>  
</div>
`]]
```

 Bien, expliquemos esto. getResourcesTag es un snippet contenedor para [getResources](/extras/getresources "getResources") y [getPage](/extras/getpage "getPage"), el cuál filtra automáticamente los resultados por 'etiquetas' de VdP (*variable de plantilla*). Por tanto, básicamente, queremos tomar todos los recursos publicados dentro de esta sección (y también podemos filtrar por etiqueta si pasamos un parámetro '?tag=NonbreDeEtiqueta' en la URL.

 Debajo de la llamada getResourcesTag, colocamos nuestros enlaces de paginación, ya que por defecto getResourcesTag solo muestra 10 publicaciones por página.

### Configuración del Chunk blogPost 

 En esa llamada, también tenemos una propiedad llamada 'tpl' que configuramos como 'blogPost'. Este es nuestro chunk que muestra cada resultado de las listas de publicaciones de nuestro blog. Debe contener esto:

``` php
<div class="post">
    <h2 class="title"><a href="[[~[[+id]]]]">[[+pagetitle]]</a></h2>
    <p class="post-info">Publicado por [[+createdby:userinfo=`fullname`]]
 [[+tv.tags:notempty=` | <span class="tags">Etiquetas:
[[!tolinks? &items=`[[+tv.tags]]` &tagKey=`tags` &target=`1`]]
</span>`]]</p>
    <div class="entry">
        <p>[[+introtext]]</p>
    </div>
    <p class="postmeta">
      <span class="links">
<a href="[[~[[+id]]]]" class="readmore">Leer más...</a>
| <a href="[[~[[+id]]]]#comments" class="comments">
    Comentarios ([[!QuipCount? &thread=`blog-post-[[+id]]`]])
  </a>
| <span class="date">[[+publishedon:strtotime:date=`%b %d, %Y`]]</span>
      </span>
    </p>
</div>
```

 Genial, profundicemos. Comenzamos haciendo un enlace a la publicación en el que se puede hacer clic con el título de la página como título. Luego, configuramos nuestra parte 'publicado por' y la lista de etiquetas (similar a cómo lo hicimos anteriormente en BlogPostTemplate).

 A continuación, mostramos parte del extracto del contenido, que almacenamos en el campo 'introtext'  del contenido.

 Después de eso, tenemos un pequeño enlace 'leer más' que enlaza con la publicación, y luego nuestros comentarios y la fecha de publicación. ¡Eso es!

 ![](blogpost-tpl1.png)

## Configuración de la página de inicio de tu blog

 En la página de inicio de nuestro blog, que tenemos en el ID de recurso 1, el inicio de nuestro sitio, tenemos esto:

``` php
[[!getResourcesTag?
  &elementClass=`modSnippet`
  &element=`getResources`
  &tpl=`blogPost`
  &parents=`34,35`
  &limit=`5`
  &includeContent=`1`
  &includeTVs=`1`
  &showHidden=`0`
  &hideContainers=`1`
  &cache=`0`
  &pageVarKey=`page`
]]
[[!+page.nav:notempty=`
<div class="paging">  
<ul class="pageList">  
  [[!+page.nav]]  
</ul>  
</div>
`]]
```

 Esto nos permite mostrar todas las publicaciones de las dos secciones que hicimos, en los Recursos 34 y 35. También nos permite filtrar por etiqueta (ya que todas nuestras llamadas 'tolinks' y 'tagLister' tienen un objetivo de 1 (este recurso ID). En otras palabras, al poner nuestra llamada getResourcesTag aquí, ¡tenemos etiquetado automático!

 Fácilmente podrías hacer que esta página sea diferente a la de inicio de tu sitio (o ID 1); solo asegúrate de cambiar las propiedades de 'objetivo' (*target*) en tu tagLister y las llamadas al snippet  __tolinks__ para reflejar esto.

## Agregar publicaciones

 Bien, ahora estamos listos para agregar publicaciones al blog, ya que nuestra estructura está configurada.

### Estructura de  páginas dentro de las secciones

 Sin embargo, antes de comenzar, es importante tener en cuenta que la forma en que estructures tus publicaciones dentro de la sección depende totalmente de tí. Puedes agregar recursos de contenedor por año y mes para colocar estas publicaciones, o simplemente publicarlas directamente dentro de la sección. Depende totalmente de ti.

 Si eliges ordenar por fecha/año, o por subcontenedores, asegúrate de que tengan marcada la opción Ocultar de los menús, para que no aparezcan en tus llamadas de getResources.

 Recuerda, sin embargo, que cualquier estructura que construyas bajo las secciones, eso no va a determinar su navegación - [Archivo] (/extras/archivist "Archivo") se encargará de eso. Sin embargo, lo que determinará es la URL de las publicaciones de tu blog. ¡Diviertete!

### Agregando una nueva publicación al blog

 Bien, continúa y crea un nuevo recurso, y configura su plantilla en 'BlogPostTemplate'. Entonces puedes empezar a escribir tu publicación. Puedes especificar en el campo 'introtexto' el extracto de la publicación del blog y luego escribir el cuerpo completo en el campo de contenido.

 Y finalmente, cuando hayas terminado, ¡asegúrate de especificar las etiquetas para tu publicación en la variable de plantilla (VdP) 'tags' creada anteriormente!

## Configurar tus archivos

Genial, tienes tu primera publicación de blog. Y lo tienes para que puedas explorarlo también en Secciones. Ahora, querrás configurar alguna forma de navegar por las publicaciones de blogs antiguas. Aquí es donde entra en juego 'Archvist'.

### Creación del recurso de archivos

 Continúa y coloca un recurso en tu raíz llamado 'Archivos', y asígnale un alias de 'archivos'. Luego, dentro del contenido, coloca esto:

``` php
[[!getPage?
  &element=`getArchives`
  &elementClass=`modSnippet`
  &tpl=`blogPost`
  &hideContainers=`1`
  &pageVarKey=`page`
  &parents=`34,35`
  &includeTVs=`1`
  &toPlaceholder=`archives`
  &limit=`10`
  &cache=`0`
]]
<h3>[[+arc_month_name]] [[+arc_year]] Archivos</h3>
[[+archives]]
[[!+page.nav:notempty=`
<div class="paging">  
<ul class="pageList">  
  [[!+page.nav]]  
</ul>  
</div>
`]]
```

 ¿Parecer familiar? Es muy similar a getResourcesTag, descrito anteriormente en nuestra página de sección. Esta vez, getPage envuelve el snippet [getArchives](/extras/archivist "Archivist") y dice que queremos capturar publicaciones de los Recursos 34 y 35 (nuestras páginas de Sección). Estableceremos el resultado en un marcador de posición llamado 'archives' al que hacemos referencia más adelante.

 Luego, debajo de eso, agregamos algunos marcadores de posición que muestran el mes y año de navegación actual. Y finalmente, tenemos nuestra paginación. ¡Elegante! Hemos terminado con esto. Nuestro recurso, para fines de referencia, diremos que tiene un ID de ** 30 **.

### Configuración del widget de archivo

 Bien, ahora tienes un recurso para buscar archivos, pero necesita alguna forma de generar los meses que enumeran las publicaciones. Eso es realmente bastante simple, en algún lugar de tu sitio (digamos, en su pie de página, coloque este pequeño detalle:

``` php
<h3>Archivos</h3>
<ul>
[[!Archivist? &target=`30` &parents=`34,35`]]
</ul>
```

 Entonces, lo que hace el snippet [Archivist](/extras/archivist/archivist.archivist "Archivist") es generar una lista de publicaciones mes a mes (puedes agregar todo tipo de otras opciones, pero lee [su documentación](/extras/archivist/archivist.archivist "Archivist") para eso). Estamos diciendo que queremos que tus enlaces vayan a nuestro Recurso de archivos (30) y que solo obtengan publicaciones de los Recursos 34 y 35 (nuestros Recursos de sección).

 ¡Eso es! Archivist se encargará automáticamente del resto, incluida toda la generación de URL para archivos, archivos /2020/05/ mostrará todas las publicaciones en mayo de 2020, donde archivos /2019/ mostrará todas las publicaciones en 2009. Muy bien, ¿no?

## Opciones avanzadas

### Agregar un grupo de moderadores

 Acuérdate que antes, en nuestra llamada QuipReply, especificamos un moderatorGroup de 'Moderators'. Sigamos adelante y creemos ese grupo de usuarios ahora.

 Vete a Seguridad -> Controles de acceso y crea un nuevo grupo de usuarios llamado 'Moderators'. Agrega los usuarios que desees en el grupo (¡incluido tu mismo!) Y asígnales el rol que desees.

 Luego, vate a la pestaña Acceso al contexto. Agregue una ACL (una fila, básicamente) que le dé acceso a este grupo de usuarios al contexto 'mgr', con un rol mínimo de Miembro (9999) y la Política de acceso de 'QuipModeratorPolicy'.

 Lo que esto hace es permitir que cualquiera en el grupo de usuarios 'Moderators' modere publicaciones en tus hilos, y también les notifica por correo electrónico cuando se realizan nuevas publicaciones. A continuación, pueden iniciar sesión en el administrador para moderar los comentarios o hacer clic en los enlaces directamente en los correos electrónicos para aprobar o rechazar los comentarios. Su ACL debería verse así:

 ![](moderator-group.png)

 Guarda tu grupo de usuarios, ¡y listo! Puede que tengas que vaciar las sesiones (Seguridad -> Vaciar las sesiones) y volver a iniciar sesión para recargar tus permisos, pero Quip se encargará del resto.

### Agregar un widget de "Últimas publicaciones"

 Probablemente querrás "Últimas publicaciones" en algún lugar del sitio, y no temas: agregarlas es bastante fácil.

 En primer lugar, querrá hacer esta llamada donde quiera que aparezca la lista:

``` php
[[!getResources?
  &parents=`34,35`
  &hideContainers=`1`
  &tpl=`latestPostsTpl`
  &limit=`5`
  &sortby=`publishedon`
]]
```

 Con esto le decimos a [getResources](/extras/getresources "getResources") que muestre una lista de los 5 principales recursos en tu sección Recursos (34,35), y ordene por fecha de publicación.  
 
 Luego, crea el chunk `latestPostsTpl`, que has especificado con la llamada 'tpl' en la llamada al snippet getResources. Pon esto como el contenido en el chunk:

``` php
<li>
  <a href="[[~[[+id]]]]">[[+pagetitle]]</a>
  [[+publishedon:notempty=`<br /> - [[+publishedon:strtotime:date=`%b %d, %Y`]]`]]
</li>
```

 ¡Y listo! Últimas publicaciones del blog mostrándose en tu sitio:

 ![](latestposts.png)

### Agregar un widget de "Comentarios más recientes"

¿Qué pasa con un widget que muestre algunos de los últimos comentarios en tus publicaciones? Simple: Quip cuentacon un pequeño snippet llamado [QuipLatestComments](/extras/quip/quip.quiplatestcomments "Quip.QuipLatestComments") que puede manejar esto fácilmente.

 Coloca la llamada donde desees que se muestre la lista de comentarios:

``` php
[[!QuipLatestComments? &tpl=`latestCommentTpl`]]
```

 Ahora crea un chunk llamado 'latestCommentTpl':

``` php
<li class="[[+cls]][[+alt]]">
    <a href="[[+url]]">[[+body:ellipsis=`[[+bodyLimit]]`]]</a>
    <br /><span class="author">por [[+name]]</span>
    <br /><span class="ago">[[+createdon:ago]]</span>
</li>
```

 Antes de continuar, hay algunas cosas a tener en cuenta: QuipLatestComments truncará automáticamente el comentario y agregará puntos suspensivos después de la propiedad &bodyLimit que se le pasó, que tiene un valor predeterminado de 30 caracteres. En segundo lugar, observa el filtro 'ago', [Filtro de salida](building-sites/tag-syntax/output-filters) "Filtros de entrada y salida (modificadores de salida)") que usamos aquí. Este filtro está integrado en MODX Revolution y traduce una marca de tiempo en un bonito y atractivo formato de 'dos horas, 34 minutos' (u otras dos métricas de tiempo, como son  min/seg,  año/mes,  mes/semana).

 Ten en cuenta también que mostrará de forma predeterminada los 5 últimos comentarios. El resultado:

 ![](latestcomments.png)

 Puedes ver la [documentación del snippet](/extras/quip/quip.quiplatestcomments "Quip.QuipLatestComments") para obtener más opciones de configuración.

### Agregar un widget de "Etiquetas más utilizadas"

 Esta parte es ridículamente fácil; [tagLister](/extras/taglister "tagLister")  hace esto por ti. Simplemente coloca esto donde desees:

``` php
[[!tagLister? &tv=`tags` &target=`1`]]
```

 Y tagLister verificará las 'etiquetas' de VdP (variable de plantilla) y creará enlaces que vayan al objetivo (aquí, recurso con ID 1) con las 10 etiquetas principales que se utilizan. Hay muchas más [opciones de configuración](/extras/taglister "tagLister"), pero te lo dejamos a tu investigación.

## Conclusión

 ¡Así que ya tenemos una configuración de blog completa! Debería verse algo como esto en nuestro árbol ahora:

 ![](blog-tree2.png)

 Nuevamente, hay mucha más personalización y cosas que podrías agregar a tu blog. Este tutorial está pensado como un punto de partida, pero siéntete libre de personalizar y agregar cosas a tu gusto; lo mejor de MODX es que puedes personalizar, ajustar y escalar fácilmente cualquier solución: ¡incluido un blog!

Recuerda, este tutorial se basó en [splittingred.com](http://splittingred.com), si desea ver una demostración a gran escala en acción.
