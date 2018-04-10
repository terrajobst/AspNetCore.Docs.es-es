---
uid: mvc/overview/performance/bundling-and-minification
title: Agrupar y Minificar | Documentos de Microsoft
author: Rick-Anderson
description: Agrupar y minificar son dos técnicas puede usar en ASP.NET 4.5 para mejorar el tiempo de carga de solicitud. Agrupar y minificar mejora el tiempo de carga por reducin...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/23/2012
ms.topic: article
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 001ebf89cda66a50cddcd7e4944f27b9396d4450
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="bundling-and-minification"></a>Agrupar y Minificar
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Agrupar y minificar son dos técnicas puede usar en ASP.NET 4.5 para mejorar el tiempo de carga de solicitud. Agrupar y minificar mejora el tiempo de carga, lo que reduce el número de solicitudes al servidor y reducir el tamaño de los activos solicitados (por ejemplo, CSS y JavaScript).


La mayoría de los principales exploradores actuales limita el número de [conexiones simultáneas](http://www.browserscope.org/?category=network) por cada nombre de host a seis. Esto significa que mientras se están procesando seis solicitudes, las solicitudes adicionales de activos en un host se pondrá en cola por el explorador. En la imagen siguiente, las pestañas de red de herramientas de desarrollador de F12 de Internet Explorer muestra el tiempo para activos requeridos por la vista de información acerca de una aplicación de ejemplo.

![B/M](bundling-and-minification/_static/image1.png)

Las barras grises muestran el tiempo que la solicitud se pone en cola con el explorador que se espera en el límite de seis conexiones. La barra amarilla es el tiempo de solicitud hasta el primer byte, es decir, el tiempo dedicado a enviar la solicitud y recibir la primera respuesta desde el servidor. Las barras azules muestran el tiempo dedicado a recibir los datos de respuesta del servidor. Puede hacer doble clic en un recurso para obtener información de tiempo detallada. Por ejemplo, la siguiente imagen muestra los detalles de tiempo para cargar la */Scripts/MyScripts/JavaScript6.js* archivo.

![](bundling-and-minification/_static/image2.png)

La imagen anterior se muestra la **iniciar** eventos, que proporciona la hora de la solicitud se puso en cola por el explorador limita el número de conexiones simultáneas. En este caso, la solicitud se puso en cola para 46 milisegundos de espera otra solicitud en completarse.

## <a name="bundling"></a>Cómo agrupar

Cómo agrupar es una nueva característica de ASP.NET 4.5 que facilita el proceso combinar o agrupar varios archivos en un único archivo. Puede crear CSS, JavaScript y otros paquetes. Menos archivos significa menos solicitudes HTTP y que puede mejorar el rendimiento de carga de primera página.

La siguiente imagen muestra la misma vista de control de tiempo de la vista de acerca mostrada anteriormente, pero esta vez con agrupación y minificación habilitado.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minificación

Minificación realiza una serie de optimizaciones de código diferente para las secuencias de comandos o css, como la eliminación de espacio en blanco innecesario y comentarios y acortar los nombres de variable con un único carácter. Considere la siguiente función de JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Después de la reducción, la función se reduce a lo siguiente:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes parámetros y nombres de variables se cambió el nombre (abreviar) como se indica a continuación:

| **Original** | **Cambiar el nombre** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | m |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impacto de la agrupación y Minificación

En la tabla siguiente muestra algunas diferencias importantes entre enumerar todos los activos de forma individual y el uso de agrupación y minificación (B/M) en el programa de ejemplo.

|  | **Uso de B/M** | **Sin B/M** | **Change** |
| --- | --- | --- | --- |
| **Solicitudes de archivos** | 9 | 34 | 256% |
| **KB enviado** | 3.26 | 11.92 | 266% |
| **KB recibido** | 388.51 | 530 | 36% |
| **Tiempo de carga** | 510 MS | 780 MS | 53% |

Los bytes enviados tenían una reducción significativa de la agrupación sea bastante detallados con los encabezados HTTP de que se aplican en las solicitudes los exploradores. La reducción de bytes recibidos no es tan grande porque los archivos de mayor tamaño (*Scripts\jquery-ui-1.8.11.min.js* y *Scripts\jquery-1.7.1.min.js*) ya se ha reducido. Nota: Los intervalos en el programa de ejemplo que usa el [Fiddler](http://www.fiddler2.com/fiddler2/) herramienta para simular una red lenta. (Desde el Fiddler **reglas** menú, seleccione **rendimiento** , a continuación, **simular un módem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Depuración de empaquetado y reducido JavaScript

Es fácil depurar el código JavaScript en un entorno de desarrollo (donde el [compilación elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo se establece en `debug="true"` ) porque no se incluyen los archivos JavaScript o reducido. También puede depurar una versión de lanzamiento donde están incluidos y reduce los archivos JavaScript. Con las herramientas de desarrollo F12 de Internet Explorer, depurar una función de JavaScript que se incluyen en un paquete reducido mediante el siguiente método:

1. Seleccione el **Script** ficha y, a continuación, seleccione la **iniciar la depuración** botón.
2. Seleccione el paquete que contiene la función de JavaScript que va a depurar mediante el botón de activos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formato JavaScript reducida, seleccione la **botón configuración** ![](bundling-and-minification/_static/image5.png)y, a continuación, seleccione **formato JavaScript**.
4. En el **búsqueda script** cuadro de entrada de t, seleccione el nombre de la función que va a depurar. En la siguiente imagen, **AddAltToImg** se escribió en el **búsqueda script** cuadro de entrada de t.  
    ![](bundling-and-minification/_static/image6.png)

Para obtener más información sobre cómo depurar con las herramientas de desarrollo F12, vea el artículo MSDN [mediante las herramientas de desarrollo F12 para depurar errores de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Controlar agrupar y Minificar

Agrupar y minificar está habilitada o deshabilitada estableciendo el valor del atributo de depuración en el [compilación elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo. En el siguiente código XML, `debug` se establece en true, agrupación y minificación está deshabilitado.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar la agrupación y minificación, establezca el `debug` valor en "false". Puede invalidar la *Web.config* establecer con el `EnableOptimizations` propiedad en la `BundleTable` clase. El código siguiente permite agrupar y minificar e invalida cualquier configuración en el *Web.config* archivo.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` es `true` o el atributo de depuración en el [compilación elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo se establece en `false`, los archivos no se incluye o se reduce. Además, no se usará la versión .min de archivos, se seleccionará las versiones de depuración completa. `EnableOptimizations` invalida el atributo de depuración en el [compilación elemento](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Mediante la agrupación y Minificación con ASP.NET Web Forms y páginas Web

- Para las páginas Web, consulte la entrada de blog [agregar optimización Web a un sitio Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Para los formularios Web Forms, vea la entrada de blog [Agregar agrupación y Minificación a formularios Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Mediante la agrupación y Minificación con ASP.NET MVC

En esta sección se creará un ASP.NET MVC proyecto para examinar la agrupación y minificación. En primer lugar, cree un nuevo proyecto de ASP.NET MVC internet denominado **MvcBM** sin cambiar cualquiera de los valores predeterminados.

Abra la *aplicación\_Start\BundleConfig.cs* de archivo y examine la `RegisterBundles` método que se utiliza para crear, registrar y configurar paquetes. El código siguiente muestra una parte de la `RegisterBundles` método.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

El código anterior crea un nuevo paquete de JavaScript denominado *~/bundles/jquery* que incluye todos los controles (que es debug o reducido pero no.* vsdoc*) archivos en el *Scripts* carpeta que coincide con la cadena de carácter comodín "{versión} ~/Scripts/jquery-.js". En ASP.NET MVC 4, esto significa que con una configuración de depuración, el archivo *1.7.1.js jquery* se agregará a la agrupación. En una configuración de lanzamiento, *1.7.1.min.js jquery* se agregará. El marco de agrupación sigue varias convenciones comunes como:

- Seleccionar archivo de ".min" para la versión cuando existen "FileX.min.js" y "FileX.js".
- Seleccione la versión no es ".min" para la depuración.
- Se omitirá "-vsdoc" archivos (por ejemplo, jquery-1.7.1-vsdoc.js), que solo son utilizados por IntelliSense.

El `{version}` comodín coincidencia mostrado anteriormente se utiliza para crear automáticamente un paquete jQuery con la versión adecuada de jQuery en su *Scripts* carpeta. En este ejemplo, el uso de un carácter comodín proporciona las siguientes ventajas:

- Permite usar NuGet para actualizar a una versión más reciente de jQuery sin cambiar el código anterior de agrupación o referencias de jQuery en las páginas de vista.
- Selecciona la versión completa de configuraciones de depuración y la versión de ".min" para la versión genera automáticamente.

## <a name="using-a-cdn"></a>Uso de una CDN

 El código siguiente reemplaza la agrupación de jQuery local con una agrupación de jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

En el código anterior, será necesario jQuery desde la red CDN mientras en versión modo y la versión de depuración de jQuery se capturarán localmente en el modo de depuración. Cuando se usa una CDN, debe tener un mecanismo de reserva en caso de que se produce un error en la solicitud de red CDN. El siguiente marcado fragmentar desde el final de la secuencia de comandos de muestra de archivo diseño, agrega para solicitar jQuery falla la red CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Crear un paquete

El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `Include` método toma una matriz de cadenas, donde cada cadena es una ruta de acceso virtual al recurso. El código siguiente en el método RegisterBundles en el *aplicación\_Start\BundleConfig.cs* archivo muestra cómo varios archivos se agregan a una agrupación:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `IncludeDirectory` método se proporciona para agregar todos los archivos en un directorio (y, opcionalmente, todos los subdirectorios) que coinciden con un patrón de búsqueda. El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `IncludeDirectory` API se muestra a continuación:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Los paquetes se hace referencia en vistas utilizando el método Render, ( `Styles.Render` de CSS y `Scripts.Render` de JavaScript). El siguiente marcado de la *Views\Shared\\_Layout.cshtml* archivo muestra cómo hacer referencia a las vistas de proyecto ASP.NET internet predeterminadas agrupaciones CSS y JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Tenga en cuenta los métodos de representación toma una matriz de cadenas, por lo que puede agregar varios paquetes en una sola línea de código. Por lo general deseará utilizar los métodos de representación que crear el código HTML necesario para hacer referencia al recurso. Puede usar el `Url` método para generar la dirección URL para el recurso sin el formato necesario para hacer referencia al recurso. Imagine que desea utilizar la nueva HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atributo. El código siguiente muestra cómo hacer referencia a modernizr utilizando el `Url` método.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Mediante la "\*" carácter comodín para seleccionar archivos

La ruta de acceso virtual especificada en el `Include` método y la búsqueda de patrón en el `IncludeDirectory` método puede aceptar una "\*" carácter comodín como un prefijo o sufijo que en el último segmento de ruta de acceso. La cadena de búsqueda distingue mayúsculas de minúsculas. El `IncludeDirectory` método tiene la opción de buscar en los subdirectorios.

Considere la posibilidad de un proyecto con los siguientes archivos JavaScript:

- *Scripts\Common\AddAltToImg.js*
- *Scripts\Common\ToggleDiv.js*
- *Scripts\Common\ToggleImg.js*
- *Scripts\Common\Sub1\ToggleLinks.js*

![imag dir](bundling-and-minification/_static/image7.png)

La siguiente tabla muestra los archivos agregados a una agrupación mediante el carácter comodín, como se muestra:

| **Call** | **Archivos agregados o produjo una excepción** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js, ToggleDiv.js, ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Excepción de patrón no válida. El carácter comodín solo se permite en el prefijo o sufijo. |
| Include("~/Scripts/Common/\*og.\*") | Excepción de patrón no válida. Se permite sólo un carácter comodín. |
| "Include("~/Scripts/Common/T\*") | *ToggleDiv.js, ToggleImg.js* |
| "Include("~/Scripts/Common/\*") | Excepción de patrón no válida. Un segmento de carácter comodín pura no es válido. |
| IncludeDirectory("~/Scripts/Common", "T\*") | *ToggleDiv.js, ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js, ToggleImg.js, ToggleLinks.js* |

Agregar explícitamente cada archivo a una agrupación es generalmente el preferido sobre comodín carga de archivos por los motivos siguientes:

- Agregar secuencias de comandos por los valores predeterminados de comodín para cargarlos en orden alfabético, que normalmente no es lo desea. Con frecuencia, los archivos CSS y JavaScript deben agregarse en un orden específico (no alfabéticos). Puede mitigar este riesgo mediante la adición de un personalizado [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementación pero agregar explícitamente cada archivo es menos propenso a errores. Por ejemplo, puede agregar nuevos activos en una carpeta en el futuro que podrían requerir que se modifique su [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementación.
- Agregado a un directorio mediante la carga de carácter comodín de determinados archivos de vista pueden incluirse en todas las vistas que hacen referencia a ese paquete. Si la secuencia de comandos específicos de la vista se agrega a una agrupación, puede obtener un error de JavaScript en otras vistas que hacen referencia a la agrupación.
- Como resultado archivos CSS que importan otros archivos en los archivos importados cargados dos veces. Por ejemplo, el código siguiente crea un paquete con la mayoría de los archivos CSS de jQuery UI tema cargados dos veces. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  El selector de comodín "\*.css" pone en cada archivo CSS en la carpeta, incluido el *Content\themes\base\jquery.ui.all.css* archivo. El *jquery.ui.all.css* archivo importa otros archivos CSS.

## <a name="bundle-caching"></a>Agrupar el almacenamiento en caché

Agrupaciones de establecer el encabezado Expires de HTTP un año de cuando se crea el paquete. Si navega a una página consultada anteriormente, Internet Explorer no realizar una solicitud condicional para la agrupación, se muestra en Fiddler es decir, hay ninguna solicitud HTTP GET desde Internet Explorer para las agrupaciones y ninguna respuesta HTTP 304 desde el servidor. Puede hacer que Internet Explorer para realizar una solicitud condicional para cada agrupación con la tecla F5 (lo que genera una respuesta HTTP 304 por cada agrupación). Puede forzar una actualización completa mediante ^ F5 (lo que genera una respuesta HTTP 200 por cada agrupación).

La siguiente imagen muestra la **Caching** ficha del panel de respuesta de Fiddler:

![imagen de almacenamiento en caché de Fiddler](bundling-and-minification/_static/image8.png)

La solicitud   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 es para la agrupación **AllMyScripts** y contiene un par de cadena de consulta **v = r0sLDicvP58AIXN\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La cadena de consulta **v** tiene un valor de símbolo (token) que es un identificador único usado para almacenar en caché. Siempre que el paquete no cambia, la aplicación ASP.NET, se solicitará la **AllMyScripts** empaquetar con este token. Si cambia cualquier archivo en el paquete, el marco de trabajo de optimización de ASP.NET generará un nuevo token para garantizar que las solicitudes del explorador para la agrupación obtendrán el paquete más reciente.

Si ejecuta las herramientas de desarrollo F12 de IE9 y navegar a una página previamente cargada, Internet Explorer incorrectamente muestra solicitudes GET condicionales realizadas en cada paquete y el servidor devuelve HTTP 304. Puede leer por qué IE9 tiene problemas para determinar si se realizó una solicitud condicional en la entrada de blog [utilizando CDN y Expires para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>MENOR, CoffeeScript, SCSS, Sass, cómo agrupar.

El marco de agrupación y minificación proporciona un mecanismo para procesar idiomas intermedios como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [menos](http://www.dotlesscss.org/) o [Coffeescript ](http://coffeescript.org/)y aplicar transformaciones como minificación a la agrupación resultante. Por ejemplo, para agregar [. less](http://www.dotlesscss.org/) archivos al proyecto de MVC 4:

1. Cree una carpeta para la menor cantidad de contenido. En el ejemplo siguiente se usa el *Content\MyLess* carpeta.
2. Agregar el [. less](http://www.dotlesscss.org/) paquete NuGet **punto** al proyecto.  
    ![Instalación de punto de NuGet](bundling-and-minification/_static/image9.png)
3. Agregue una clase que implementa el [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfaz. Para la transformación. less, agregue el código siguiente a su proyecto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Crear un paquete de menos archivos con la `LessTransform` y [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformar. Agregue el código siguiente a la `RegisterBundles` método en el *aplicación\_Start\BundleConfig.cs* archivo.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Agregue el código siguiente a cualquier vista que hace referencia a la agrupación de menos.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Consideraciones de agrupación

Una buena convención deben seguir al crear agrupaciones consiste en incluir "paquete" como prefijo en el nombre del paquete. Esto evitará que una posible [conflicto de enrutamiento](https://forums.asp.net/post/5012037.aspx).

Una vez que se actualiza un archivo en un paquete, se genera un nuevo token para el parámetro de cadena de consulta de agrupación y se debe descargar el paquete completo la próxima vez que un cliente solicita una página que contiene el paquete. En el marcado tradicional donde cada activo se enumera individualmente, se descargarán solo el archivo modificado. Activos que cambian con frecuencia no pueden ser buenas candidatas para la agrupación.

Agrupar y minificar mejoran principalmente el primer tiempo de carga de solicitud de página. Una vez que se ha solicitado una página Web, el explorador se almacena en memoria caché los activos (JavaScript, CSS e imágenes) para agrupar y minificar no proporcionan ningún aumento del rendimiento cuando se solicita la misma página o páginas en el mismo sitio que solicita los activos de la mismos. Si no establece la expira encabezado correctamente en los activos y no use agrupar y minificar, la heurística de actualización de los exploradores, se marcarán como los recursos obsoletos después de unos días y el explorador requerirá una solicitud de validación para cada recurso. En este caso, agrupar y minificar proporcionan un aumento del rendimiento después de la primera solicitud de página. Para obtener más información, consulte el blog de [utilizando CDN y Expires para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitación del navegador de seis conexiones simultáneas por cada nombre de host se puede mitigar mediante el uso de un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Debido a que la red CDN tiene un nombre de host diferente que el sitio de hospedaje, las solicitudes de activos de la red CDN se descontará el límite de conexiones simultáneas seis a su entorno de hospedaje. También puede proporcionar una CDN comunes del almacenamiento en caché de paquete y borde ventajas de caché.

Los paquetes se deben dividir por las páginas que las necesiten. Por ejemplo, el valor predeterminado de plantilla de MVC de ASP.NET para una aplicación de internet crea una agrupación de validación de jQuery independiente de jQuery. Dado que las vistas predeterminadas que se crea no tienen ninguna entrada y no registrar los valores, no incluyen la agrupación de validación.

El `System.Web.Optimization` espacio de nombres se implementa en System.Web.Optimization.DLL. Aprovecha la biblioteca de WebGrease (WebGrease.dll) para capacidades de reducción, que a su vez usa Antlr3.Runtime.dll.

*Utilizo Twitter realizar entradas rápidos y compartir vínculos. Es el identificador de Twitter*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionales

- Vídeo:[agrupación y optimizar](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) por [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Adición de optimización Web a un sitio de páginas Web](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Agregar agrupación y Minificación a formularios Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicaciones de rendimiento de agrupación y Minificación de exploración Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) por [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Usar CDN y caduca para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar RTT (tiempos de ida y vuelta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
