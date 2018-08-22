---
uid: mvc/overview/performance/bundling-and-minification
title: Unión y Minificación | Microsoft Docs
author: Rick-Anderson
description: Unión y minificación son dos técnicas puede usar en ASP.NET 4.5 para mejorar el tiempo de carga de solicitud. Unión y minificación mejora el tiempo de carga por reducin...
ms.author: riande
ms.date: 08/23/2012
ms.assetid: 5894dc13-5d45-4dad-8096-136499120f1d
msc.legacyurl: /mvc/overview/performance/bundling-and-minification
msc.type: authoredcontent
ms.openlocfilehash: 7e54bdd2f50edb5001982ada9b6ce023584ce5b0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828006"
---
<a name="bundling-and-minification"></a>Unión y Minificación
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Unión y minificación son dos técnicas puede usar en ASP.NET 4.5 para mejorar el tiempo de carga de solicitud. Unión y minificación mejora el tiempo de carga por lo que reduce el número de solicitudes al servidor y reducir el tamaño de los activos solicitados (por ejemplo, CSS y JavaScript).


La mayoría de los principales exploradores actuales limita el número de [conexiones simultáneas](http://www.browserscope.org/?category=network) por cada nombre de host a seis. Esto significa que mientras se están procesando seis solicitudes, las solicitudes adicionales para los recursos en un host se pondrán en el explorador. En la imagen siguiente, las fichas de red de herramientas de desarrollador F12 de Internet Explorer muestra el tiempo para los recursos requeridos por la vista About de una aplicación de ejemplo.

![B/M](bundling-and-minification/_static/image1.png)

Las barras grises muestran el tiempo que se pone en cola la solicitud mediante el explorador a la espera en el límite de conexiones de seis. La barra amarilla es el tiempo de solicitud hasta el primer byte, es decir, el tiempo necesario para enviar la solicitud y recibir la primera respuesta del servidor. Las barras azules muestran el tiempo dedicado a recibir los datos de respuesta del servidor. Puede hacer doble clic en un recurso para obtener información de tiempo detallada. Por ejemplo, la siguiente imagen muestra los detalles de tiempo para cargar el */Scripts/MyScripts/JavaScript6.js* archivo.

![](bundling-and-minification/_static/image2.png)

La imagen anterior se muestra la **iniciar** eventos, que proporciona el tiempo que se puso en cola la solicitud porque el Explorador de limita el número de conexiones simultáneas. En este caso, la solicitud se puso en cola para 46 milisegundos de espera otra solicitud para completarse.

## <a name="bundling"></a>La Unión

La unión es una nueva característica de ASP.NET 4.5 que facilita combinar o agrupar varios archivos en un único archivo. Puede crear otros paquetes, CSS y JavaScript. Menos archivos significa menos solicitudes HTTP y que puede mejorar el rendimiento de carga de primera página.

La siguiente imagen muestra la misma vista de control de tiempo de la vista About mostrada anteriormente, pero esta vez con la unión y minificación habilitado.

![](bundling-and-minification/_static/image3.png)

## <a name="minification"></a>Minificación

Realiza la minificación una variedad de optimizaciones de código diferente para las secuencias de comandos o css, como la eliminación de espacio en blanco innecesario y comentarios y acortar los nombres de variable a un carácter. Considere la siguiente función de JavaScript.

[!code-javascript[Main](bundling-and-minification/samples/sample1.js)]

Después de la minificación, la función se reduce a lo siguiente:

[!code-javascript[Main](bundling-and-minification/samples/sample2.js)]

Además de quitar los comentarios y espacios en blanco innecesarios, los siguientes parámetros y los nombres de variable se cambió el nombre (abreviado) como sigue:

| **Texto original en** | **Cambiar el nombre** |
| --- | --- |
| imageTagAndImageID | n |
| imageContext | m |
| imageElement | i |

## <a name="impact-of-bundling-and-minification"></a>Impacto de la unión y Minificación

En la tabla siguiente se muestra varias diferencias importantes entre el listado de todos los recursos individualmente y el uso de unión y minificación (B/M) en el programa de ejemplo.

|  | **Uso de B/M** | **Sin B/M** | **Cambio** |
| --- | --- | --- | --- |
| **Solicitudes de archivos** | 9 | 34 | 256% |
| **KB enviado** | 3.26 | 11.92 | 266% |
| **KB recibido** | 388.51 | 530 | 36% |
| **Tiempo de carga** | 510 MS | 780 MS | 53% |

Los bytes enviados tenían una reducción significativa con unión tal como los exploradores son bastante detallados con los encabezados HTTP que se aplican en las solicitudes. La reducción de bytes recibidos no es tan grande porque los archivos más grandes (*Scripts\\jquery-ui-1.8.11.min.js* y *Scripts\\jquery 1.7.1.min.js*) ya se han minimizado . Nota: Los intervalos en el programa de ejemplo usa el [Fiddler](http://www.fiddler2.com/fiddler2/) herramienta para simular una red lenta. (Desde el Fiddler **reglas** menú, seleccione **rendimiento** , a continuación, **simular un módem**.)

## <a name="debugging-bundled-and-minified-javascript"></a>Depuración y Minificado JavaScript

Es fácil de depurar el código JavaScript en un entorno de desarrollo (donde el [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo se establece en `debug="true"` ) porque no se empaquetan los archivos JavaScript o han minimizado. También puede depurar una versión de lanzamiento donde los archivos JavaScript se agrupan y han minimizado. Con las herramientas de desarrollo F12 de IE, depurar una función de JavaScript incluida en un paquete minimizado mediante el siguiente método:

1. Seleccione el **Script** pestaña y, a continuación, seleccione el **iniciar la depuración** botón.
2. Seleccione el paquete que contiene la función de JavaScript que desea depurar mediante el botón de recursos.  
    ![](bundling-and-minification/_static/image4.png)
3. Formato de JavaScript minimizado, seleccione el **botón configuración** ![](bundling-and-minification/_static/image5.png)y, a continuación, seleccione **formato JavaScript**.
4. En el **secuencia de comandos de búsqueda** cuadro de entrada, seleccione el nombre de la función que desea depurar. En la siguiente imagen, **AddAltToImg** se escribió en el **secuencia de comandos de búsqueda** cuadro de entrada.  
    ![](bundling-and-minification/_static/image6.png)

Para obtener más información sobre la depuración con herramientas de desarrollo F12, consulte el artículo de MSDN [mediante las herramientas de desarrollo F12 para depurar errores de JavaScript](https://msdn.microsoft.com/library/ie/gg699336(v=vs.85).aspx).

## <a name="controlling-bundling-and-minification"></a>Control de unión y Minificación

Unión y minificación está habilitada o deshabilitada, establezca el valor del atributo de depuración en el [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo. En el siguiente código XML, `debug` está establecida en true, la unión y minificación está deshabilitada.

[!code-xml[Main](bundling-and-minification/samples/sample3.xml?highlight=2)]

Para habilitar la unión y minificación, establezca el `debug` valor en "false". Puede invalidar el *Web.config* establecer con el `EnableOptimizations` propiedad en el `BundleTable` clase. El código siguiente permite la unión y minificación y reemplaza cualquier configuración en el *Web.config* archivo.

[!code-csharp[Main](bundling-and-minification/samples/sample4.cs?highlight=7)]

> [!NOTE]
> A menos que `EnableOptimizations` es `true` o el atributo de depuración en el [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo se establece en `false`, archivos no se incluye o se han minimizado. Además, no se usará la versión .min de archivos, seleccionará las versiones de depuración completa. `EnableOptimizations` invalida el atributo de depuración en el [elemento compilation](https://msdn.microsoft.com/library/s10awwz0.aspx) en el *Web.config* archivo


## <a name="using-bundling-and-minification-with-aspnet-web-forms-and-web-pages"></a>Uso de la unión y Minificación con formularios Web Forms ASP.NET y páginas Web

- Para las páginas Web, consulte la entrada de blog [adición de optimización Web a un sitio Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- Para los formularios Web Forms, vea la entrada de blog [agregar unión y Minificación para formularios Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).

## <a name="using-bundling-and-minification-with-aspnet-mvc"></a>Uso de la unión y Minificación con ASP.NET MVC

En esta sección se creará un ASP.NET MVC project para examinar la unión y minificación. En primer lugar, cree un nuevo proyecto de ASP.NET MVC internet denominado **MvcBM** sin cambiar cualquiera de los valores predeterminados.

Abra el *aplicación\\\_iniciar\\BundleConfig.cs* de archivo y examine el `RegisterBundles` método que se usa para crear, registrar y configurar paquetes. El código siguiente muestra una parte de la `RegisterBundles` método.

[!code-csharp[Main](bundling-and-minification/samples/sample5.cs)]

El código anterior crea un nuevo paquete de JavaScript denominado *~/bundles/jquery* que incluye todos los controles (que es la depuración o se han minimizado pero no. *vsdoc*) archivos en el *Scripts* carpeta que coincide con la cadena comodín "~/Scripts/jquery-{version} .js". En ASP.NET MVC 4, esto significa que con una configuración de depuración, el archivo *jquery 1.7.1.js* se agregará a la agrupación. En una configuración de lanzamiento, *jquery 1.7.1.min.js* se agregará. El marco de unión sigue varias convenciones comunes, como:

- Seleccionar archivo ".min" para la versión cuando *FileX.min.js* y *FileX.js* existe.
- Seleccionar la versión que no sea ".min" para la depuración.
- Se omitirá "-vsdoc" archivos (como *jquery-1.7.1-vsdoc.js*), que solo son utilizados por IntelliSense.

El `{version}` con caracteres comodín de coincidencia que se muestra arriba se usa para crear automáticamente un paquete jQuery con la versión adecuada de jQuery en su *Scripts* carpeta. En este ejemplo, el uso de un carácter comodín proporciona las siguientes ventajas:

- Permite usar NuGet para actualizar a una versión más reciente de jQuery sin cambiar el código anterior de agrupación o referencias de jQuery en las páginas de vista.
- Selecciona la versión completa de las configuraciones de depuración y la versión ".min" versión crea automáticamente.

## <a name="using-a-cdn"></a>Usa una red CDN

 El código siguiente reemplaza la agrupación de jQuery local con un conjunto de jQuery CDN.

[!code-csharp[Main](bundling-and-minification/samples/sample6.cs)]

En el código anterior, se solicitará jQuery desde la red CDN mientras que en la versión modo y la versión de depuración de jQuery se capturarán localmente en modo de depuración. Cuando se usa una red CDN, debe tener un mecanismo de reserva en caso de que se produce un error en la solicitud de la red CDN. El marcado siguiente fragmento del final de la secuencia de comandos muestra del archivo de diseño agregó a la solicitud de jQuery debe el error de la red CDN.

[!code-cshtml[Main](bundling-and-minification/samples/sample7.cshtml?highlight=5-13)]

## <a name="creating-a-bundle"></a>Creación de un paquete

El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `Include` método toma una matriz de cadenas, donde cada cadena es una ruta de acceso virtual al recurso. El siguiente código de la `RegisterBundles` método en el *aplicación\\\_iniciar\\BundleConfig.cs* archivo muestra cómo varios archivos se agregan a una agrupación:

[!code-csharp[Main](bundling-and-minification/samples/sample8.cs)]

El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `IncludeDirectory` método se proporciona para agregar todos los archivos en un directorio (y, opcionalmente, todos los subdirectorios) que coinciden con un patrón de búsqueda. El [agrupación](https://msdn.microsoft.com/library/system.web.optimization.bundle(v=VS.110).aspx) clase `IncludeDirectory` API se muestra a continuación:

[!code-csharp[Main](bundling-and-minification/samples/sample9.cs)]

Se hace referencia a agrupaciones en vistas mediante el método Render (`Styles.Render` para CSS y `Scripts.Render` para JavaScript). El siguiente marcado de la *vistas\\Shared\\\_Layout.cshtml* archivo muestra cómo hacer referencia a las vistas de proyecto predeterminada ASP.NET internet agrupaciones CSS y JavaScript.

[!code-cshtml[Main](bundling-and-minification/samples/sample10.cshtml?highlight=5-6,11)]

Tenga en cuenta los métodos de representación toma una matriz de cadenas, por lo que puede agregar varios paquetes en una línea de código. Por lo general, deseará utilizar los métodos de representación que creación el código HTML necesario para hacer referencia a los activos. Puede usar el `Url` método para generar la dirección URL al recurso sin el marcado necesario para hacer referencia al asset. Imagine que quiere usar el nuevo HTML5 [async](http://www.whatwg.org/specs/web-apps/current-work/#attr-script-async) atributo. El código siguiente muestra cómo hacer referencia a modernizr utilizando el `Url` método.

[!code-cshtml[Main](bundling-and-minification/samples/sample11.cshtml?highlight=11)]

## <a name="using-the--wildcard-character-to-select-files"></a>Mediante la "\*" carácter comodín para seleccionar archivos

La ruta de acceso virtual especificada en el `Include` método y la búsqueda de patrones en el `IncludeDirectory` método puede aceptar una "\*" carácter comodín como prefijo o sufijo que en el último segmento de ruta de acceso. La cadena de búsqueda distingue mayúsculas de minúsculas. El `IncludeDirectory` método tiene la opción de buscar en los subdirectorios.

Considere la posibilidad de un proyecto con los siguientes archivos JavaScript:

- *Las secuencias de comandos\\común\\AddAltToImg.js*
- *Las secuencias de comandos\\común\\ToggleDiv.js*
- *Las secuencias de comandos\\común\\ToggleImg.js*
- *Las secuencias de comandos\\común\\Sub1\\ToggleLinks.js*

![imag dir](bundling-and-minification/_static/image7.png)

La siguiente tabla muestra los archivos agregados a una agrupación mediante el carácter comodín, como se muestra:

| **Call** | **Archivos agregados o una excepción** |
| --- | --- |
| Include("~/Scripts/Common/\*.js") | *AddAltToImg.js*, *ToggleDiv.js*, *ToggleImg.js* |
| Include("~/Scripts/Common/T\*.js") | Excepción de patrón no válido. Solo se permite el carácter comodín en el prefijo o sufijo. |
| Incluir ("~/Scripts/Common/\*og.\*") | Excepción de patrón no válido. Se permite sólo un carácter comodín. |
| Incluir ("~/Scripts/Common/T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| Incluir ("~/Scripts/Common/\*") | Excepción de patrón no válido. Un segmento comodín puro no es válido. |
| IncludeDirectory ("~/Scripts/Common", "T\*") | *ToggleDiv.js*, *ToggleImg.js* |
| IncludeDirectory ("~/Scripts/Common", "T\*", true) | *ToggleDiv.js*, *ToggleImg.js*, *ToggleLinks.js* |

Agregar explícitamente cada archivo a una agrupación es normalmente el preferido a través de la carga de comodín de archivos por las razones siguientes:

- Agregar secuencias de comandos con valores predeterminados de comodín para cargarlos en orden alfabético, que normalmente no es lo que desea. Con frecuencia, los archivos CSS y JavaScript deben agregarse en un orden específico (no alfabéticos). Puede mitigar este riesgo mediante la adición de un personalizado [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementación, pero agregar explícitamente cada archivo es menos propenso a errores. Por ejemplo, puede agregar nuevos recursos en una carpeta en el futuro que podrían requerir que se va a modificar su [IBundleOrderer](https://msdn.microsoft.com/library/system.web.optimization.ibundleorderer(VS.110).aspx) implementación.
- Ver archivos específicos agregados a un directorio mediante la carga de carácter comodín pueden incluirse en todas las vistas que hacen referencia a ese paquete. Si la secuencia de comandos específicos de la vista se agrega a una agrupación, es posible que obtenga un error de JavaScript en otras vistas que hacen referencia a la agrupación.
- Como resultado archivos CSS que importan otros archivos en los archivos importados cargados dos veces. Por ejemplo, el código siguiente crea un paquete con la mayoría de los archivos CSS de jQuery UI tema cargados dos veces. 

    [!code-csharp[Main](bundling-and-minification/samples/sample12.cs)]

  El selector de carácter comodín "\*.css" pone en cada archivo CSS en la carpeta, incluido el *contenido\\temas\\base\\jquery.ui.all.css* archivo. El *jquery.ui.all.css* archivo importa otros archivos CSS.

## <a name="bundle-caching"></a>Lote de almacenamiento en caché

Agrupaciones establecer el encabezado Expires de HTTP un año de cuando se crea el paquete. Si navega a una página consultada anteriormente, se muestra en Fiddler IE no realizar una solicitud condicional para la agrupación, es decir, hay ninguna solicitud HTTP GET desde Internet Explorer para los paquetes y no hay respuestas HTTP 304 desde el servidor. Puede forzar que Internet Explorer para realizar una solicitud condicional para cada agrupación con la tecla F5 (lo que conlleva una respuesta HTTP 304 por cada paquete). Puede forzar una actualización completa mediante el uso de ^ F5 (lo que conlleva una respuesta HTTP 200 por cada paquete).

La siguiente imagen muestra la **Caching** ficha del panel de respuesta de Fiddler:

![imagen de almacenamiento en caché de Fiddler](bundling-and-minification/_static/image8.png)

La solicitud   
`http://localhost/MvcBM_time/bundles/AllMyScripts?v=r0sLDicvP58AIXN_mc3QdyVvVj5euZNzdsa2N1PKvb81`  
 es para la agrupación **AllMyScripts** y contiene un par de cadena de consulta **v = r0sLDicvP58AIXN\\\_mc3QdyVvVj5euZNzdsa2N1PKvb81**. La cadena de consulta **v** tiene un valor que es un identificador único usado para almacenar en caché del token. Siempre que el paquete no cambia, la aplicación de ASP.NET solicitará el **AllMyScripts** usando este token de lote. Si cambia cualquier archivo en el paquete, el marco de optimización de ASP.NET generará un nuevo token, lo que garantiza que las solicitudes del explorador para la agrupación obtendrán el paquete más reciente.

Si ejecuta las herramientas de desarrollo F12 de Internet Explorer 9 y navegar a una página previamente cargada, IE incorrectamente muestra solicitudes GET condicionales realizadas en cada lote y el servidor devuelve HTTP 304. Puede leer por qué IE9 tiene problemas para determinar si se realizó una solicitud condicional en la entrada de blog [uso de CDN y Expires para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

## <a name="less-coffeescript-scss-sass-bundling"></a>LESS, CoffeeScript, SCSS, Sass, la unión.

El marco de trabajo de unión y minificación proporciona un mecanismo para procesar lenguajes intermedios como [SCSS](http://sass-lang.com/), [Sass](http://sass-lang.com/), [menos](http://www.dotlesscss.org/) o [Coffeescript ](http://coffeescript.org/)y aplicar transformaciones, como la minificación a la agrupación resultante. Por ejemplo, para agregar [.less](http://www.dotlesscss.org/) archivos al proyecto de MVC 4:

1. Cree una carpeta para la menor cantidad de contenido. En el ejemplo siguiente se usa el *contenido\\MyLess* carpeta.
2. Agregar el [.less](http://www.dotlesscss.org/) paquete NuGet **dotless** al proyecto.  
    ![Instalación de NuGet dotless](bundling-and-minification/_static/image9.png)
3. Agregue una clase que implementa el [IBundleTransform](https://msdn.microsoft.com/library/system.web.optimization.ibundletransform(VS.110).aspx) interfaz. Para la transformación .less, agregue el código siguiente al proyecto.

    [!code-csharp[Main](bundling-and-minification/samples/sample13.cs)]
4. Crear un paquete de menos archivos con la `LessTransform` y [CssMinify](https://msdn.microsoft.com/library/system.web.optimization.cssminify(VS.110).aspx) transformar. Agregue el código siguiente a la `RegisterBundles` método en el *aplicación\\_iniciar\\BundleConfig.cs* archivo.

    [!code-csharp[Main](bundling-and-minification/samples/sample14.cs)]
5. Agregue el código siguiente a todas las vistas que se hace referencia a la agrupación de menos.

    [!code-cshtml[Main](bundling-and-minification/samples/sample15.cshtml)]

## <a name="bundle-considerations"></a>Consideraciones sobre la agrupación

Es una buena convención deben seguir al crear agrupaciones incluir "agrupar" como prefijo en el nombre del lote. Esto evitará que una posible [conflicto de enrutamiento](https://forums.asp.net/post/5012037.aspx).

Una vez que actualice un archivo de un paquete, se genera un nuevo token para el parámetro de cadena de consulta de agrupación y el paquete completo debe descargarse la próxima vez que un cliente solicita una página que contiene el paquete. En el marcado tradicional donde se muestra cada activo de forma individual, se descargarían solo el archivo modificado. Los activos que cambian con frecuencia no pueden ser buenos candidatos para la unión.

Unión y minificación principalmente mejoran el tiempo de carga de solicitud de primera página. Una vez que se ha solicitado una página Web, el explorador almacena en caché los activos (JavaScript, CSS e imágenes) por lo que la unión y minificación no proporcionan ningún aumento del rendimiento cuando se solicita la misma página o páginas en el mismo sitio que solicita los mismos recursos. Si no establece la expira el encabezado correctamente en los recursos y no use unión y minificación, la heurística de actualización de los exploradores marcará los recursos obsoletos después de unos días y el explorador requiere una solicitud de validación para cada recurso. En este caso, unión y minificación proporcionan un aumento del rendimiento tras la primera solicitud de página. Para obtener más información, consulte el blog de [uso de CDN y Expires para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx).

La limitación del explorador de seis conexiones simultáneas por cada nombre de host se puede mitigar mediante el uso de un [CDN](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx). Debido a que la red CDN tiene un nombre de host diferente que el sitio de hospedaje, las solicitudes de activos de la red CDN se descontarán el límite de conexiones simultáneas de seis a su entorno de hospedaje. También puede proporcionar una red CDN comunes del almacenamiento en caché de paquete y las ventajas de almacenamiento en caché de borde.

Los paquetes se deben particionar por las páginas que las necesiten. Por ejemplo, la plantilla de MVC de ASP.NET para una aplicación de internet predeterminada crea un paquete de validación de jQuery independiente de jQuery. Dado que las vistas predeterminadas que se crea no tienen ninguna entrada y no las publique los valores, no incluyen la agrupación de validación.

El `System.Web.Optimization` espacio de nombres se implementa en *System.Web.Optimization.dll*. Aprovecha la biblioteca de WebGrease (*WebGrease.dll*) para las capacidades de la minificación, que a su vez utiliza *Antlr3.Runtime.dll*.

*Usar Twitter para realizar publicaciones rápidos y compartir vínculos. Mi identificador de Twitter es*: [@RickAndMSFT](http://twitter.com/RickAndMSFT)

## <a name="additional-resources"></a>Recursos adicionales

- Vídeo:[unión y optimizar](https://channel9.msdn.com/Events/aspConf/aspConf/Bundling-and-Optimizing) por [Howard Dierking](https://twitter.com/#!/howard_dierking)
- [Adición de optimización Web a un sitio Web Pages](https://blogs.msdn.com/b/rickandy/archive/2012/08/15/adding-web-optimization-to-a-web-pages-site.aspx).
- [Adición de unión y Minificación para formularios Web Forms](https://blogs.msdn.com/b/rickandy/archive/2012/08/14/adding-bundling-and-minification-to-web-forms.aspx).
- [Implicaciones de rendimiento de la unión y Minificación de exploración Web](https://blogs.msdn.com/b/henrikn/archive/2012/06/17/performance-implications-of-bundling-and-minification-on-http.aspx) por [Henrik F Nielsen](http://en.wikipedia.org/wiki/Henrik_Frystyk_Nielsen) [@frystyk](https://twitter.com/frystyk)
- [Uso de CDN y caduca para mejorar el rendimiento del sitio Web](https://blogs.msdn.com/b/rickandy/archive/2011/05/21/using-cdns-to-improve-web-site-performance.aspx) Rick Anderson [@RickAndMSFT](https://twitter.com/#!/RickAndMSFT)
- [Minimizar el RTT (tiempos de ida y vuelta)](https://developers.google.com/speed/docs/best-practices/rtt)

## <a name="contributors"></a>Colaboradores

- Hao Kung
- [Howard Dierking](https://twitter.com/#!/howard_dierking)
- Diana LaRose
