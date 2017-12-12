---
uid: whitepapers/aspnet4/overview
title: "ASP.NET 4 y Visual Studio 2010 Introducción al desarrollo Web | Documentos de Microsoft"
author: rick-anderson
description: "Este documento proporciona información general de muchas de las nuevas características de ASP.NET que se incluye en.NET Framework 4 y Visual Studio 2010."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: d7729af4-1eda-4ff2-8b61-dbbe4fc11d10
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet4
msc.type: content
ms.openlocfilehash: 226ef83f289b8fbe9a68f0d0741c7eca0d96ba94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-4-and-visual-studio-2010-web-development-overview"></a>ASP.NET 4 y Visual Studio 2010 Introducción al desarrollo Web
====================
> Este documento proporciona información general de muchas de las nuevas características de ASP.NET que se incluye en.NET Framework 4 y Visual Studio 2010.
> 
> [Descargar estas notas del producto](https://download.microsoft.com/download/7/1/A/71A105A9-89D6-4201-9CC5-AD6A3B7E2F22/ASP_NET_4_and_Visual_Studio_2010_Web_Development_Overview.pdf)


**Contenido**

**[Servicios principales](#0.2__Toc253429238 "_Toc253429238")**  
[Archivo Web.config refactorización](#0.2__Toc253429239 "_Toc253429239")  
[Caché de resultados extensible](#0.2__Toc253429240 "_Toc253429240")  
[Inicio automático de aplicaciones Web](#0.2__Toc253429241 "_Toc253429241")  
[Redirigir de forma permanente una página](#0.2__Toc253429242 "_Toc253429242")  
[La reducción de estado de sesión](#0.2__Toc253429243 "_Toc253429243")  
[Amplía el intervalo de direcciones URL permitidas](#0.2__Toc253429244 "_Toc253429244")  
[Validación de solicitudes extensible](#0.2__Toc253429245 "_Toc253429245")  
[Almacenamiento en caché de objetos y extensibilidad de almacenamiento en caché del objeto](#0.2__Toc253429246 "_Toc253429246")  
[HTML extensible, la dirección URL y la codificación de encabezado HTTP](#0.2__Toc253429247 "_Toc253429247")  
[Supervisión del rendimiento de las aplicaciones individuales en un solo proceso de trabajo](#0.2__Toc253429248 "_Toc253429248")  
[Compatibilidad con múltiples versiones](#0.2__Toc253429249 "_Toc253429249")

**[AJAX](#0.2__Toc253429250 "_Toc253429250")**  
[jQuery incluidos con formularios Web Forms y MVC](#0.2__Toc253429251 "_Toc253429251")  
[Compatibilidad con red de entrega de contenido](#0.2__Toc253429252 "_Toc253429252")  
[Las secuencias de comandos explícitos de ScriptManager](#0.2__Toc253429253 "_Toc253429253")

**[Formularios Web Forms](#0.2__Toc253429256 "_Toc253429256")**  
[Establecer etiquetas Meta con las propiedades Page.MetaKeywords y Page.MetaDescription](#0.2__Toc253429257 "_Toc253429257")  
[Habilitar el estado de vista para controles individuales](#0.2__Toc253429258 "_Toc253429258")  
[Cambios en las funciones del explorador](#0.2__Toc253429259 "_Toc253429259")  
[Enrutamiento en ASP.NET 4](#0.2__Toc253429260 "_Toc253429260")  
[Establecer los identificadores de cliente](#0.2__Toc253429261 "_Toc253429261")  
[Guardar selección de filas en los controles de datos](#0.2__Toc253429262 "_Toc253429262")  
[Control ASP.NET Chart](#0.2__Toc253429263 "_Toc253429263")  
[Filtrar datos con el Control QueryExtender](#0.2__Toc253429264 "_Toc253429264")  
[Expresiones de código codificado en HTML](#0.2__Toc253429265 "_Toc253429265")  
[Cambios de la plantilla de proyecto](#0.2__Toc253429266 "_Toc253429266")  
[Mejoras de CSS](#0.2__Toc253429267 "_Toc253429267")  
[Ocultar elementos alrededor de campos ocultos div](#0.2__Toc253429268 "_Toc253429268")  
[Representación de una tabla externa para controles con plantilla](#0.2__Toc253429269 "_Toc253429269")  
[Mejoras del Control ListView](#0.2__Toc253429270 "_Toc253429270")  
[Mejoras de Control RadioButtonList y CheckBoxList](#0.2__Toc253429271 "_Toc253429271")  
[Mejoras de Control de menú](#0.2__Toc253429272 "_Toc253429272")  
[Asistente y controles de CreateUserWizard 56](#0.2__Toc253429273 "_Toc253429273")

**[ASP.NET MVC](#0.2__Toc253429274 "_Toc253429274")**  
[Compatibilidad con las áreas](#0.2__Toc253429275 "_Toc253429275")  
[Compatibilidad de validación de atributos de anotación de datos](#0.2__Toc253429276 "_Toc253429276")  
[Aplicaciones auxiliares con plantilla](#0.2__Toc253429277 "_Toc253429277")

**[Datos dinámicos](#0.2__Toc253429278 "_Toc253429278")**  
[Habilitar datos dinámicos para los proyectos existentes](#0.2__Toc253429279 "_Toc253429279")  
[Sintaxis declarativa Control DynamicDataManager](#0.2__Toc253429280 "_Toc253429280")  
[Las plantillas de entidad](#0.2__Toc253429281 "_Toc253429281")  
[Nuevas plantillas de campo para las direcciones URL y direcciones de correo electrónico](#0.2__Toc253429282 "_Toc253429282")  
[Creación de vínculos con el Control DynamicHyperLink](#0.2__Toc253429283 "_Toc253429283")  
[Compatibilidad con herencia en el modelo de datos de](#0.2__Toc253429284 "_Toc253429284")  
[Soporte técnico para las relaciones de varios a varios (solo Entity Framework)](#0.2__Toc253429285 "_Toc253429285")  
[Nuevos atributos para controlar la presentación y admitir enumeraciones](#0.2__Toc253429286 "_Toc253429286")  
[Compatibilidad mejorada para filtros](#0.2__Toc253429287 "_Toc253429287")

**[Mejoras de desarrollo Web de Visual Studio 2010](#0.2__Toc253429288 "_Toc253429288")**  
[Mejorar la compatibilidad CSS](#0.2__Toc253429289 "_Toc253429289")  
[HTML y JavaScript fragmentos](#0.2__Toc253429290 "_Toc253429290")  
[Mejoras de IntelliSense para JavaScript](#0.2__Toc253429291 "_Toc253429291")

**[Implementación de aplicaciones con Visual Studio 2010 Web](#0.2__Toc253429292 "_Toc253429292")**  
[Web empaquetado](#0.2__Toc253429293 "_Toc253429293")  
[Transformación de Web.config](#0.2__Toc253429294 "_Toc253429294")  
[Implementación de la base de datos](#0.2__Toc253429295 "_Toc253429295")  
[Publicación de un solo clic para las aplicaciones Web](#0.2__Toc253429296 "_Toc253429296")  
[Recursos](#0.2__Toc253429297 "_Toc253429297")

**[Declinación de responsabilidades](#0.2__Toc253429298 "_Toc253429298")**

<a id="0.2__Toc224729018"></a><a id="0.2__Toc253429238"></a><a id="0.2__Toc243304612"></a>

## <a name="core-services"></a>Servicios principales

ASP.NET 4 presenta a una serie de características que mejoran los servicios principales de ASP.NET como el almacenamiento de estado de sesión y caché de resultados.

<a id="0.2__Toc243304613"></a><a id="0.2__Toc253429239"></a><a id="0.2__Toc224729019"></a>

### <a name="webconfig-file-refactoring"></a>Archivo Web.config de refactorización

El `Web.config` archivo que contiene la configuración para una aplicación Web ha aumentado considerablemente en las pocos las versiones anteriores de .NET Framework tal y como se han agregado nuevas características, como Ajax, enrutamiento y la integración con IIS 7. Esto ha hecho más difícil establecer o iniciar nuevas aplicaciones Web sin una herramienta como Visual Studio. En .NET Framework 4. seguidamente, los elementos de configuración principales que se han movido a la `machine.config` archivos y aplicaciones ahora heredan esta configuración. Esto permite la `Web.config` archivo en aplicaciones de ASP.NET 4 para que esté vacía o contener solo las líneas siguientes, que especifican para Visual Studio qué versión de framework que está destinada la aplicación:

[!code-xml[Main](overview/samples/sample1.xml)]

<a id="0.2__Toc253429240"></a><a id="0.2__Toc243304614"></a>

### <a name="extensible-output-caching"></a>Caché de resultados extensible

Desde el momento en que se lanzó ASP.NET 1.0, la caché de resultados se permitían a los desarrolladores almacenar el resultado generado de páginas, los controles y las respuestas HTTP en la memoria. En las solicitudes Web posteriores, ASP.NET puede servir contenido más rápidamente al recuperar el resultado generado de la memoria en lugar de volver a generar la salida desde el principio. Sin embargo, este enfoque tiene una limitación: contenido generado siempre tiene que almacenarse en memoria y en los servidores que están experimentando tráfico intenso, la memoria utilizada por la caché de resultados puede competir con peticiones de memoria de otras partes de una aplicación Web.

ASP.NET 4 agrega un punto de extensibilidad para el almacenamiento en caché de salida que le permite configurar uno o varios proveedores de caché de resultados personalizados. Proveedores de caché de resultados pueden usar cualquier mecanismo de almacenamiento para conservar el contenido HTML. Esto hace posible crear proveedores de caché de resultados personalizados para los mecanismos de persistencia diversos, que pueden incluir discos locales o remotos, almacenamiento en nube y distribuidas motores de memoria caché.

Crear un proveedor de caché de resultados personalizado como una clase que deriva de la nueva *System.Web.Caching.OutputCacheProvider* tipo. A continuación, puede configurar el proveedor en el `Web.config` archivo usando la nueva *proveedores* de la subsección de la *outputCache* elemento, tal como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample2.xml)]

De forma predeterminada en ASP.NET 4, todas las respuestas HTTP, representan las páginas y los controles utilizan la caché de resultados en memoria, como se muestra en el ejemplo anterior, donde la *defaultProvider* atributo está establecido en AspNetInternalProvider. Puede cambiar el proveedor de caché de resultados predeterminado utilizado para una aplicación Web especificando un nombre de proveedor diferente para *defaultProvider*.

Además, puede seleccionar diferentes proveedores de caché de resultados por control y para cada solicitud. La manera más fácil para elegir un proveedor de caché de resultados diferente para los distintos controles de usuario Web es hacerlo mediante declaración con el nuevo *providerName* atributo en una directiva de control, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample3.aspx)]

Especifica un proveedor de caché de resultados diferentes para una solicitud HTTP, requiere un poco más trabajo. En lugar de especificar el proveedor de manera declarativa, invalide el nuevo *GetOuputCacheProviderName* método en el `Global.asax` archivo para especificar mediante programación el proveedor que desea usar para una solicitud específica. En el ejemplo siguiente se muestra cómo hacerlo.

[!code-csharp[Main](overview/samples/sample4.cs)]

Con la adición de extensibilidad de proveedor de caché de resultados para ASP.NET 4, puede seguir ahora más inteligentes y más agresivos estrategias de almacenamiento en caché de salida para los sitios Web. Por ejemplo, ahora es posible almacenar en caché las páginas "Top 10" de un sitio en la memoria, al almacenamiento en caché de páginas que obtienen la menor cantidad de tráfico en el disco. Como alternativa, puede almacenar en caché cada combinación de variación para una página representada, pero usar una memoria caché distribuida para que se descarga el consumo de memoria de los servidores Web front-end.

<a id="0.2__Toc224729020"></a><a id="0.2__Toc253429241"></a><a id="0.2__Toc243304615"></a>

### <a name="auto-start-web-applications"></a>Inicio automático de aplicaciones Web

Algunas aplicaciones Web necesitan cargar grandes cantidades de datos o realizar una inicialización cara procesamiento antes de servir la primera solicitud. En versiones anteriores de ASP.NET, para estas situaciones era necesario idear enfoques personalizados para "reactive" una aplicación ASP.NET y, a continuación, ejecutar código de inicialización durante la *aplicación\_carga* método en el `Global.asax` archivo.

Una nueva característica de escalabilidad denominada *inicio automático* que directamente direcciones este escenario está disponible cuando ASP.NET 4 se ejecuta en IIS 7.5 en Windows Server 2008 R2. La característica de inicio automático proporciona un enfoque controlado para iniciar un grupo de aplicaciones, inicializar una aplicación ASP.NET y, a continuación, acepte las solicitudes HTTP.

> [!NOTE] 
> 
> Módulo de preparación de aplicaciones de IIS para IIS 7.5
> 
> El equipo IIS ha publicado la primera versión de prueba de la versión beta del módulo de preparación de aplicación de IIS 7.5. Esto hace que preparar las aplicaciones incluso más sencillo de lo descrito anteriormente. En lugar de escribir código personalizado, especifique las direcciones URL de recursos que se ejecutará antes de que la aplicación Web acepta solicitudes de la red. Esta preparación se produce durante el inicio del servicio IIS (si configuró el grupo de aplicaciones de IIS como *AlwaysRunning*) y cuando se recicla un proceso de trabajo IIS. Durante el reciclaje, el proceso de trabajo IIS anterior continúa ejecutándose solicitudes hasta que el proceso de trabajo recién generado completamente está activa, para que las aplicaciones experimentan sin interrupciones u otros problemas debido a las cachés unprimed. Tenga en cuenta que este módulo funciona con cualquier versión de ASP.NET, empezando con la versión 2.0.
> 
> Para obtener más información, consulte [Application warm-up](https://www.iis.net/extensions/applicationwarmup%20on%20the%20IIS.net) en el sitio IIS.net Web. Para ver un tutorial que muestra cómo utilizar la característica de preparación, consulte [cómo empezar a usar el módulo de preparación de aplicaciones de IIS 7.5](https://www.iis.net/learn/manage) en el sitio IIS.net Web.


Para usar la característica de inicio automático, un administrador de IIS, configura un grupo de aplicaciones en IIS 7.5 para iniciarse automáticamente mediante el uso de la configuración siguiente en el `applicationHost.config` archivo:

[!code-xml[Main](overview/samples/sample5.xml)]

Dado que un único grupo de aplicaciones puede contener varias aplicaciones, especifique las aplicaciones individuales para iniciarse automáticamente mediante el uso de la configuración siguiente en el `applicationHost.config` archivo:

[!code-xml[Main](overview/samples/sample6.xml)]

Cuando un servidor de IIS 7.5 se inicia en frío o cuando se recicla un grupo de aplicaciones individuales, IIS 7.5 usa la información de la `applicationHost.config` archivo para determinar qué necesidad de aplicaciones Web para iniciarse automáticamente. Para cada aplicación que está marcada para el inicio automático, IIS7.5 envía una solicitud a ASP.NET 4 para iniciar la aplicación en un estado durante el cual la aplicación temporalmente no acepta solicitudes HTTP. Cuando se encuentra en este estado, ASP.NET crea una instancia del tipo definido por el *serviceAutoStartProvider* atributo (como se muestra en el ejemplo anterior) y llama a su punto de entrada público.

Crear un tipo de inicio automático administrado con el punto de entrada necesario implementando la *IProcessHostPreloadClient* interfaz, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample7.cs)]

Después de la inicialización de código se ejecuta en el *Preload* y el método devuelve, la aplicación de ASP.NET está lista para procesar las solicitudes.

Con la adición de inicio automático para.5 IIS y ASP.NET 4, ahora tiene un enfoque bien definido para realizar la inicialización de aplicaciones caras antes de procesar la primera solicitud HTTP. Por ejemplo, puede usar la nueva característica de inicio automático para inicializar una aplicación y, a continuación, señalar un equilibrador de carga que la aplicación se ha inicializado y listo para aceptar tráfico HTTP.

<a id="0.2__Toc224729021"></a><a id="0.2__Toc253429242"></a><a id="0.2__Toc243304616"></a>

### <a name="permanently-redirecting-a-page"></a>Redirigir de forma permanente una página

Es habitual en aplicaciones Web a mover páginas y otro contenido alrededor de con el tiempo, lo que puede provocar una acumulación de vínculos obsoletos en los motores de búsqueda. En ASP.NET, los desarrolladores han administrado tradicionalmente las solicitudes a direcciones URL anteriores mediante a través del *Response.Redirect* método reenviar una solicitud a la nueva dirección URL. Sin embargo, el *redirigir* método emite una respuesta HTTP 302 encontrado (Redirección temporal), que da lugar a un HTTP adicional de ida y vuelta cuando los usuarios intentan tener acceso a las direcciones URL antiguas.

ASP.NET 4 agrega un nuevo *RedirectPermanent* método auxiliar que facilita el problema HTTP 301 mueve de forma permanente las respuestas, como en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample8.cs)]

Los motores de búsqueda y otros agentes de usuario que reconocen las redirecciones permanentes, se almacenarán la nueva dirección URL que está asociada con el contenido, lo que elimina lo realizado por el explorador para las redirecciones temporales de ida y vuelta innecesario.

<a id="0.2__Toc224729022"></a><a id="0.2__Toc253429243"></a><a id="0.2__Toc243304617"></a>

### <a name="shrinking-session-state"></a>La reducción de estado de sesión

ASP.NET proporciona dos opciones predeterminadas para almacenar el estado de sesión a través de una granja de servidores Web: un proveedor de estado de sesión que invoca a un servidor de estado de sesión fuera de proceso y un proveedor de estado de sesión que almacena los datos en una base de datos de Microsoft SQL Server. Porque ambas opciones almacenan la información de estado fuera de proceso de trabajo de la aplicación Web, el estado de sesión debe serializarse antes de enviarse al almacenamiento remoto. Según la cantidad de información un programador se guarda en el estado de sesión, el tamaño de los datos serializados puede crecer bastante grande.

ASP.NET 4 presenta una nueva opción de compresión para ambos tipos de proveedores de estado de sesión fuera de proceso. Cuando el *compressionEnabled* opción de configuración se muestra en el ejemplo siguiente se establece en *true*, ASP.NET se comprimir (y descomprimir) estado de sesión serializado con .NET Framework  *System.IO.Compression.GZipStream* clase.

[!code-xml[Main](overview/samples/sample9.xml)]

Con la adición del nuevo atributo a simple el `Web.config` archivo, las aplicaciones con repuesto ciclos de CPU en servidores Web puede obtener reducciones sustanciales en el tamaño de los datos de estado de sesión serializados.

<a id="0.2__Toc253429244"></a><a id="0.2__Toc243304618"></a>

### <a name="expanding-the-range-of-allowable-urls"></a>Amplía el intervalo de direcciones URL permitidas

ASP.NET 4 presenta nuevas opciones para aumentar el tamaño de las direcciones URL de aplicación. Las versiones anteriores de ASP.NET limitada longitudes de ruta de acceso de dirección URL a 260 caracteres, según el límite de la ruta de acceso de archivos NTFS. En ASP.NET 4, tiene la opción para aumentar (o disminuir) este límite según corresponda para sus aplicaciones, con dos nuevos *httpRuntime* atributos de configuración. El ejemplo siguiente muestra los nuevos atributos.

[!code-xml[Main](overview/samples/sample10.xml)]

Para permitir rutas de acceso de mayor o menos (la parte de la dirección URL que no incluya el protocolo, el nombre del servidor y la cadena de consulta), modifique la  *[maxUrlLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxurllength.aspx)*  atributo. Para permitir que las cadenas de consulta de mayor o menor, modifique el valor de la  *[maxQueryStringLength](https://msdn.microsoft.com/en-us/library/system.web.configuration.httpruntimesection.maxquerystringlength.aspx)*  atributo.

ASP.NET 4 también permite configurar los caracteres que se usan por la comprobación de caracteres de la dirección URL. Cuando ASP.NET encuentra un carácter no válido en la parte de la ruta de acceso de una dirección URL, se rechaza la solicitud y emite un error HTTP 400. En versiones anteriores de ASP.NET, las comprobaciones de caracteres de la dirección URL se limitaban a un conjunto fijo de caracteres. En ASP.NET 4, puede personalizar el conjunto de caracteres válidos con el nuevo *requestPathInvalidChars* atributo de la *httpRuntime* elemento de configuración, tal como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample11.xml)]

De forma predeterminada, el *requestPathInvalidChars* atributo define ocho caracteres como no válidos. (En la cadena que se asigna a *requestPathInvalidChars* predeterminada*,*el menor que (&lt;), mayor que (&gt;) y "y" comercial (&amp;) son caracteres codificar, porque el `Web.config` archivo es un archivo XML.) Puede personalizar el conjunto de caracteres no válidos según sea necesario.

> [!NOTE]
> Tenga en cuenta ASP.NET 4 siempre rechaza las rutas de acceso de dirección URL que contienen caracteres en el intervalo ASCII de 0 x 00 a 0x1F, ya que son caracteres de dirección URL no válidos, tal como se define en RFC 2396 de IETF ([http://www.ietf.org/rfc/rfc2396.txt](http://www.ietf.org/rfc/rfc2396.txt)). En versiones de Windows Server que ejecute IIS 6 o versiones posteriores, el controlador de dispositivo de protocolo de http.sys rechaza automáticamente las direcciones URL con estos caracteres.


<a id="0.2__Toc253429245"></a><a id="0.2__Toc243304619"></a>

### <a name="extensible-request-validation"></a>Validación de solicitudes extensible

Validación de solicitudes ASP.NET busca en los datos de solicitud HTTP entrantes para las cadenas que se usan con frecuencia en los ataques de scripting entre sitios (XSS). Si se detectan posibles cadenas XSS, validación de solicitudes indica la cadena sospechosa y devuelve un error. La validación de solicitudes integrado devuelve un error cuando encuentra las cadenas más comunes utilizadas en los ataques XSS. Los intentos anteriores para realizar la validación de XSS más agresiva generó demasiados falsos positivos. Sin embargo, los clientes conviene comprobaciones de validación de solicitud que es más agresiva, o a la inversa desee intencionadamente relajado XSS para páginas específicas o para tipos específicos de solicitudes.

En ASP.NET 4, la característica de validación de solicitud se ha realizado extensible para que pueda utilizar la lógica de validación de solicitudes personalizado. Para ampliar la validación de solicitud, se crea una clase que deriva de la nueva *System.Web.Util.RequestValidator* tipo y configura la aplicación (en el *httpRuntime* sección de la `Web.config`archivo) se utiliza el tipo personalizado. En el ejemplo siguiente se muestra cómo configurar una clase de validación de solicitudes personalizado:

[!code-xml[Main](overview/samples/sample12.xml)]

El nuevo *requestValidationType* atributo requiere una cadena de identificador de tipo de .NET Framework estándar que especifica la clase que proporciona la validación de solicitudes personalizado. Para cada solicitud, ASP.NET invoca el tipo personalizado para procesar cada fragmento de datos de la solicitud HTTP entrantes. La dirección URL entrante, todos los encabezados HTTP (cookies y encabezados personalizados) y el cuerpo de entidad están disponibles para su inspección mediante una clase de validación de solicitudes personalizado como el se muestra en el ejemplo siguiente:

[!code-csharp[Main](overview/samples/sample13.cs)]

Para los casos donde no desea inspeccionar una parte de los datos entrantes de HTTP, la clase de validación de solicitudes puede revertir para la validación de solicitudes ASP.NET de forma predeterminada para que se ejecute mediante la llamada a *base. IsValidRequestString.*

<a id="0.2__Toc253429246"></a><a id="0.2__Toc243304620"></a>

### <a name="object-caching-and-object-caching-extensibility"></a>Almacenamiento en caché de objetos y extensibilidad de almacenamiento en caché de objetos

Desde su primera versión, ASP.NET ha incluido una caché de objetos en memoria eficaz (*System.Web.Caching.Cache*). La implementación de la memoria caché ha sido tan conocida que se ha utilizado en otro tipo de aplicaciones. Sin embargo, resulta difícil para una aplicación de Windows Forms o WPF incluir una referencia a `System.Web.dll` para poder usar la caché de objetos ASP.NET.

Para disponer de almacenamiento en caché para todas las aplicaciones, .NET Framework 4 introduce un nuevo ensamblado, un nuevo espacio de nombres, algunos tipos base y una hormigón implementación el almacenamiento en caché. El nuevo `System.Runtime.Caching.dll` ensamblado contiene una nueva API de almacenamiento en caché en el *System.Runtime.Caching* espacio de nombres. El espacio de nombres contiene dos conjuntos principales de clases:

- Tipos abstractos que proporcionan la base para la creación de cualquier tipo de implementación de caché personalizado.
- Una implementación de caché de objeto en memoria concreto (el *System.Runtime.Caching.MemoryCache* clase).

El nuevo *MemoryCache* clase se modela estrechamente en la caché de ASP.NET, y comparte una gran parte de la lógica de motor de caché interna con ASP.NET. Aunque las API públicas de almacenamiento en caché en *System.Runtime.Caching* se han actualizado para admitir el desarrollo de las memorias caché personalizadas, si ha usado ASP.NET *caché* objeto, encontrará conceptos conocidos en el nuevas API.

Una explicación más detallada de la nueva *MemoryCache* clase y la compatibilidad de las API de base requeriría un documento completo. Sin embargo, en el ejemplo siguiente se ofrece una idea de cómo funciona la nueva API de caché. En el ejemplo se escribió para una aplicación de formularios Windows Forms, sin ninguna dependencia en `System.Web.dll`.

[!code-csharp[Main](overview/samples/sample14.cs)]

<a id="0.2__Toc253429247"></a><a id="0.2__Toc243304621"></a>

### <a name="extensible-html-url-and-http-header-encoding"></a>HTML extensible, la dirección URL y la codificación de encabezado HTTP

En ASP.NET 4, puede crear rutinas personalizadas de codificación para las tareas de codificación de texto comunes siguientes:

- Codificación HTML.
- Codificación de direcciones URL.
- Codificación del atributo HTML.
- Codificación de encabezados HTTP salientes.

Puede crear un codificador personalizado derivando de la nueva *System.Web.Util.HttpEncoder* tipo y, a continuación, configura ASP.NET para usar el tipo personalizado en el *httpRuntime* sección de la `Web.config` archivo, como se muestra en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample15.xml)]

Una vez configurado un codificador personalizado, ASP.NET llama automáticamente a la implementación de codificación personalizada siempre que sea pública de codificación de métodos de la *System.Web.HttpUtility* o *System.Web.HttpServerUtility* se denominan clases. Esto permite que una parte de un equipo de desarrollo de Web crear un codificador personalizado que implementa la codificación de caracteres agresivo, mientras que el resto del equipo de desarrollo Web continúa usando la codificación API de ASP.NET pública. Mediante la configuración de forma centralizada un codificador personalizado en el *httpRuntime* elemento, se garantiza que todas las llamadas de codificación de texto de la codificación API de ASP.NET pública se enrutan a través de codificador personalizado.

<a id="0.2__Toc253429248"></a><a id="0.2__Toc243304622"></a>

### <a name="performance-monitoring-for-individual-applications-in-a-single-worker-process"></a>Supervisión del rendimiento de las aplicaciones individuales en un solo proceso de trabajo

Para aumentar el número de sitios Web que se pueden hospedar en un solo servidor, muchos proveedores de hospedaje ejecutan varias aplicaciones de ASP.NET en un solo proceso de trabajo. Sin embargo, si varias aplicaciones utilizan un proceso de trabajo compartidos, es difícil para los administradores del servidor identificar una aplicación individual que está experimentando problemas.

ASP.NET 4 aprovecha la nueva funcionalidad de supervisión de recursos introducida por el CLR. Para habilitar esta funcionalidad, puede agregar el siguiente fragmento de código de configuración de XML para el `aspnet.config` archivo de configuración.

[!code-xml[Main](overview/samples/sample16.xml)]

> [!NOTE]
> Tenga en cuenta el `aspnet.config` archivo está en el directorio donde está instalada .NET Framework. No es el `Web.config` archivo.


Cuando el *appDomainResourceMonitoring* se ha habilitado la característica, dos nuevos contadores de rendimiento están disponibles en la categoría de rendimiento "Aplicaciones de ASP.NET": *% de tiempo de procesador administrado* y  *Administra la memoria utilizada*. Ambos de estos contadores de rendimiento de usan la nueva característica de administración de recursos de dominio de aplicación de CLR para realizar el seguimiento de tiempo de CPU estimado y la utilización de memoria administrada de aplicaciones ASP.NET individuales. Como resultado, con ASP.NET 4, los administradores tienen ahora una vista más detallada en el consumo de recursos de aplicaciones individuales que se ejecutan en un solo proceso de trabajo.

<a id="0.2__Toc253429249"></a><a id="0.2__Toc243304623"></a>

### <a name="multi-targeting"></a>Compatibilidad con múltiples versiones (multi-targeting)

Puede crear una aplicación que tenga como destino una versión específica de .NET Framework. En ASP.NET 4, un atributo nuevo en el *compilación* elemento de la `Web.config` archivo permite tener como destino .NET Framework 4 y versiones posterior. Si elige explícitamente como destino .NET Framework 4, y si incluye elementos opcionales en el `Web.config` archivo como las entradas de *system.codedom*, estos elementos deben ser correctos para .NET Framework 4. (Si elige no explícitamente como destino .NET Framework 4, la plataforma de destino se deduce de la ausencia de una entrada en el `Web.config` archivo.)

En el ejemplo siguiente se muestra el uso de la *targetFramework* de atributo en el *compilación* elemento de la `Web.config` archivo.

[!code-xml[Main](overview/samples/sample17.xml)]

Tenga en cuenta lo siguiente sobre cómo destinar una versión específica de .NET Framework:

- En un grupo de aplicaciones de .NET Framework 4, el sistema de compilación ASP.NET supone .NET Framework 4 como un destino si la `Web.config` archivo no incluye la *targetFramework* atributo o si la `Web.config` archivo no está presente. (Tendrá que realizar cambios en el código en la aplicación para que se ejecute en .NET Framework 4).
- Si incluye el *targetFramework* atributo y si la *system.codeDom* elemento está definido en el `Web.config` archivo, este archivo debe contener las entradas correctas para .NET Framework 4.
- Si usas el *aspnet\_compilador* comando para precompilar la aplicación (como en un entorno de compilación), debe utilizar la versión correcta de la *aspnet\_compilador* comando para la plataforma de destino. Utilice el compilador que se incluye con .NET Framework 2.0 (% WINDIR%\Microsoft.NET\Framework\v2.0.50727) para compilar para el .NET Framework 3.5 y versiones anteriores. Usar el compilador que se distribuye con .NET Framework 4 para compilar las aplicaciones creadas mediante ese marco de trabajo o con versiones posteriores.
- En tiempo de ejecución, el compilador utiliza los ensamblados de marco de trabajo más recientes que se instalan en el equipo (y, por tanto, en la GAC). Si se realiza una actualización más adelante en el marco de trabajo (por ejemplo, se instala una versión hipotética 4.1), podrá usar características de la versión más reciente de framework, aunque la *targetFramework* atributo tiene como destino una versión anterior (por ejemplo, 4.0). (Sin embargo, en tiempo de diseño en Visual Studio 2010 o cuando se usa el *aspnet\_compilador* comando, utiliza las características más recientes de framework provocará errores del compilador).

<a id="0.2__Toc224729023"></a><a id="0.2__Toc253429250"></a><a id="0.2__Toc243304624"></a>

## <a name="ajax"></a>Ajax

<a id="0.2__Toc253429251"></a><a id="0.2__Toc243304625"></a>

### <a name="jquery-included-with-web-forms-and-mvc"></a>jQuery incluidos con formularios Web Forms y MVC

Las plantillas de Visual Studio para formularios Web Forms y MVC incluyen la biblioteca de jQuery de código abierto. Cuando se crea un nuevo sitio Web o un proyecto, se crea una carpeta de Scripts que contiene los siguientes archivos de 3:

- jQuery-1.4.1.js: el lenguaje natural, unminified versión de la biblioteca de jQuery.
- jQuery-14.1.min.js: la versión reducida de la biblioteca de jQuery.
- jQuery-1.4.1-vsdoc.js: el archivo de documentación de Intellisense para la biblioteca de jQuery.

Incluya la versión unminified de jQuery al desarrollar una aplicación. Incluya la versión reducida de jQuery para las aplicaciones de producción.

Por ejemplo, la siguiente página de formularios Web Forms muestra cómo puede usar jQuery para cambiar el color de fondo de los controles de cuadro de texto de ASP.NET a amarillo cuando tiene el foco.

[!code-aspx[Main](overview/samples/sample18.aspx)]

<a id="0.2__Toc253429252"></a><a id="0.2__Toc243304626"></a>

### <a name="content-delivery-network-support"></a>Compatibilidad para red de entrega de contenido

La red contenido de entrega de Microsoft Ajax (CDN) le permite agregar fácilmente Ajax de ASP.NET y los scripts de jQuery a sus aplicaciones Web. Por ejemplo, puede empezar a usar la biblioteca de jQuery simplemente agregando un `<script>` etiqueta a la página que señala a Ajax.microsoft.com similar al siguiente:

[!code-html[Main](overview/samples/sample19.html)]

Aprovechando las ventajas de la CDN de Microsoft Ajax, puede mejorar significativamente el rendimiento de las aplicaciones Ajax. El contenido de CDN de Microsoft Ajax se almacenan en caché en servidores ubicados en todo el mundo. Además, CDN de Microsoft Ajax permite exploradores reutilicen los archivos JavaScript almacenados en caché para los sitios Web que se encuentran en dominios diferentes.

La red de entrega de contenido de Microsoft Ajax admite SSL (HTTPS) en caso de que necesite servir una página web con la capa de Sockets seguros.

Para obtener más información sobre la red CDN de Ajax de Microsoft, visite el siguiente sitio Web:

[https://www.ASP.NET/ajaxlibrary/CDN.ashx](../../ajax/cdn/overview.md)

El ScriptManager de ASP.NET es compatible con la CDN de Microsoft Ajax. Basta con establecer una propiedad, la propiedad EnableCdn, puede recuperar todos los archivos de JavaScript ASP.NET framework desde la red CDN:

[!code-aspx[Main](overview/samples/sample20.aspx)]

Después de establecer la propiedad EnableCdn en el valor true, el marco de ASP.NET recuperará todos los archivos de JavaScript ASP.NET framework desde la red CDN incluidos todos los archivos JavaScript que se usan para la validación y el control UpdatePanel. Al establecer esta una propiedad puede tener un impacto significativo en el rendimiento de la aplicación web.

Puede establecer la ruta de acceso CDN para sus propios archivos JavaScript mediante el atributo de recursos Web. La nueva propiedad CdnPath especifica la ruta de acceso a la red CDN utilizada al establecer la propiedad EnableCdn para el valor true:

[!code-csharp[Main](overview/samples/sample21.cs)]

<a id="0.2__Toc253429253"></a><a id="0.2__Toc243304627"></a>

### <a name="scriptmanager-explicit-scripts"></a>Secuencias de comandos ScriptManager explícita

En el pasado, si ha usado la ScriptManger de ASP.NET, a continuación, era necesario para cargar la biblioteca de Ajax de ASP.NET monolítico todo. Aprovechando las ventajas de la nueva propiedad ScriptManager.AjaxFrameworkMode, puede controlar exactamente qué componentes de la biblioteca de Ajax de ASP.NET que se cargan y cargar solo los componentes de la biblioteca de Ajax de ASP.NET que necesita.

La propiedad ScriptManager.AjaxFrameworkMode se puede establecer en los siguientes valores:

- Habilitado: Especifica que el control ScriptManager incluye automáticamente el archivo de script MicrosoftAjax.js, que es un archivo de script combinado de cada secuencia de comandos de framework core (comportamiento heredado).
- Deshabilitado: Especifica que todas las características de script de Microsoft Ajax están deshabilitadas y que el control ScriptManager no hace referencia a los scripts automáticamente.
- Explicit: Especifica que se van a incluir explícitamente las referencias de script al archivo de script de núcleo de .NET framework que requiere la página y que se van a incluir referencias a las dependencias que requiere que cada archivo de script.

Por ejemplo, si establece la propiedad AjaxFrameworkMode en el valor explícito, a continuación, puede especificar las secuencias de comandos de componente de Ajax de ASP.NET determinados que necesita:

[!code-aspx[Main](overview/samples/sample22.aspx)]

<a id="0.2__The_DataView_Control"></a><a id="0.2__The_DataContext_and"></a><a id="0.2__Refactoring_the_Microsoft"></a><a id="0.2__Toc224729032"></a><a id="0.2__Toc253429256"></a><a id="0.2__Toc243304630"></a>

## <a name="web-forms"></a>formularios Web Forms

Formularios Web Forms ha sido una característica principal de ASP.NET desde el lanzamiento de ASP.NET 1.0. Muchas mejoras han permanecido en esta área para ASP.NET 4, incluidas las siguientes:

- La capacidad de establecer *meta* etiquetas.
- Más control sobre el estado de vista.
- Métodos más sencillos para trabajar con las funciones del explorador.
- Compatibilidad para usar el enrutamiento de ASP.NET con formularios Web Forms.
- Más control sobre los identificadores generados.
- La capacidad para conservar las filas seleccionadas en controles de datos.
- Más control sobre el HTML representado en el *FormView* y *ListView* controles.
- Filtrado de soporte técnico para los controles de origen de datos.

<a id="0.2__Toc224729033"></a><a id="0.2__Toc253429257"></a><a id="0.2__Toc243304631"></a>

### <a name="setting-meta-tags-with-the-pagemetakeywords-and-pagemetadescription-properties"></a>Establecer etiquetas Meta con las propiedades Page.MetaKeywords y Page.MetaDescription

ASP.NET 4 agrega dos propiedades para el *página* (clase), *MetaKeywords* y *MetaDescription*. Estas dos propiedades representan correspondiente *meta* etiquetas en la página, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample23.aspx)]

Estas dos propiedades funcionan de la misma forma en que la página *título* propiedad sí. Siguen estas reglas:

1. Si no hay ningún *meta* etiquetas en el *head* elemento que coincide con los nombres de propiedad (es decir, el nombre = "palabras clave" para *Page.MetaKeywords* y el nombre = "Descripción" para  *Page.MetaDescription*, lo que significa que no se hayan establecido estas propiedades), el *meta* etiquetas se agregará a la página cuando se represente.
2. Si ya hay *meta* etiquetas con estos nombres, estas propiedades actúan como obtener y establecer métodos para el contenido de las etiquetas existentes.

Puede establecer estas propiedades en tiempo de ejecución, que le permite obtener el contenido de una base de datos u otro origen, y que le permite establecer las etiquetas dinámicamente para describir qué es una página determinada.

También puede establecer el *palabras clave* y *descripción* propiedades en el *@ Page* la directiva en la parte superior de marcado de la página de formularios Web Forms, como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample24.aspx)]

Esto anulará la *meta* contenido (si existe) ya declarado en la página de etiquetas.

El contenido de la descripción *meta* etiqueta se utiliza para la mejora de la búsqueda mostrar vistas previas en Google. (Para obtener más información, consulte [mejorar fragmentos de código con un makeover de descripción meta](http://googlewebmastercentral.blogspot.com/2007/09/improve-snippets-with-meta-description.html) en el blog del administrador del sitio Web Central de Google.) Google y Windows Live Search no usan el contenido de las palabras clave para cualquier elemento, pero podrían otros motores de búsqueda. Para obtener más información, consulte [consejos de palabras clave Meta](http://www.searchengineguide.com/richard-ball/meta-keywords-a.php) en el sitio Web de guía de motor de búsqueda.

Estas nuevas propiedades son una característica simple, pero guardar desde el requisito de agregarlas manualmente o escribir su propio código para crear el *meta* etiquetas.

<a id="0.2__Toc224729034"></a><a id="0.2__Toc253429258"></a><a id="0.2__Toc243304632"></a>

### <a name="enabling-view-state-for-individual-controls"></a>Habilitar el estado de vista para controles individuales

De forma predeterminada, el estado de vista está habilitado para la página, con lo que cada control en la página potencialmente almacena el estado de vista, incluso si no es necesaria para la aplicación. Datos de estado de vista se incluyen en el marcado que una página genera y aumenta la cantidad de tiempo necesario para enviar una página al cliente y registrarlo de nuevo. Almacenar el estado de vista más que sean necesarios puede provocar degradación significativa del rendimiento. En versiones anteriores de ASP.NET, los desarrolladores podrían deshabilitar el estado de vista de controles individuales para reducir el tamaño de página, pero que tuve que hacer de forma explícita para controles individuales. En ASP.NET 4, controles de servidor Web incluyen un *ViewStateMode* propiedad que le permite deshabilitar el estado de vista predeterminada y, a continuación, habilitarlo solo para los controles que requieren en la página.

El *ViewStateMode* propiedad toma una enumeración que tiene tres valores: *habilitado*, *deshabilitado*, y *heredar*. *Habilitado* permite ver el estado de ese control y para todos los controles secundarios que se establecen en *heredar* o que se nada establecido. *Deshabilitado* deshabilita el estado de vista, y *heredar* especifica que el control usa la *ViewStateMode* configuración del control primario.

El siguiente ejemplo se muestra cómo el *ViewStateMode* funciona de la propiedad. El marcado y código para los controles en la siguiente página incluye valores para la *ViewStateMode* propiedad:

[!code-aspx[Main](overview/samples/sample25.aspx)]

Como puede ver, el código deshabilita el estado de vista para el control PlaceHolder1. El control de label1 secundario hereda este valor de propiedad (*heredar* es el valor predeterminado de *ViewStateMode* para los controles.) y, por tanto, se guarda ningún estado de vista. En el control PlaceHolder2, *ViewStateMode* está establecido en *habilitado*, por lo que label2 hereda esta propiedad y guarda el estado de vista. Cuando se carga la página por primera vez, el *texto* propiedad *etiqueta* controles se establece en la cadena "[DynamicValue]".

El efecto de esta configuración es que cuando la página cargue la primera vez, se muestra en el explorador el siguiente resultado:

Deshabilitado`: [DynamicValue]`

Habilitado:`[DynamicValue]`

Después de una devolución de datos, sin embargo, se muestra el siguiente resultado:

Deshabilitado`: [DeclaredValue]`

Habilitado:`[DynamicValue]`

El control label1 (cuyo *ViewStateMode* valor se establece en *deshabilitado*) no se conserva el valor que se ha establecido en el código. Sin embargo, controlar la label2 (cuyo *ViewStateMode* valor se establece en *habilitado*) se conserva su estado.

También puede establecer *ViewStateMode* en el *@ Page* directivas, como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample26.aspx)]

El *página* clase es simplemente otro control; actúa como control primario para todos los demás controles en la página. El valor predeterminado de *ViewStateMode* es *habilitado* para instancias de *página*. Porque los controles de manera predeterminada a *heredar*, controles heredarán la *habilitado* valor de propiedad a menos que establezca *ViewStateMode* en el nivel de página o control.

El valor de la *ViewStateMode* propiedad determina si se mantiene el estado de vista sólo si el *EnableViewState* propiedad está establecida en *true*. Si el *EnableViewState* propiedad está establecida en *false*, estado de vista no se mantendrán incluso si *ViewStateMode* está establecido en *habilitado*.

Un buen uso de esta característica es con *ContentPlaceHolder* controles en las páginas maestras, donde puede establecer *ViewStateMode* a *deshabilitado* para el patrón de página y, a continuación, vuelva a habilitarla individualmente para *ContentPlaceHolder* controles que a su vez contienen los controles que requieren el estado de vista.

<a id="0.2__Toc224729035"></a><a id="0.2__Toc253429259"></a><a id="0.2__Toc243304633"></a>

### <a name="changes-to-browser-capabilities"></a>Cambios en las funciones del explorador

ASP.NET determina las capacidades del explorador que está usando un usuario para examinar el sitio mediante una característica denominada *las capacidades del explorador*. Las funciones del explorador se representan mediante la *HttpBrowserCapabilities* objeto (expuesto por la *Request.Browser* propiedad). Por ejemplo, puede usar el *HttpBrowserCapabilities* objeto para determinar si el tipo y la versión del explorador actual admite una versión determinada de JavaScript. O bien, puede usar el *HttpBrowserCapabilities* objeto para determinar si la solicitud se originó desde un dispositivo móvil.

El *HttpBrowserCapabilities* objeto está controlado por un conjunto de archivos de definición de explorador. Estos archivos contienen información acerca de las capacidades de exploradores determinados. En ASP.NET 4, estos archivos de definición de explorador se actualizaron para incluir información sobre los dispositivos, como Google Chrome, investigación en smartphones Motion BlackBerry y Apple iPhone y exploradores presentados recientemente.

La siguiente lista muestra nueva ventana del explorador de archivos de definición:

- *BlackBerry.Browser*
- *Chrome.Browser*
- *Default.Browser*
- *Firefox.Browser*
- *Gateway.Browser*
- *Generic.Browser que se encuentra*
- *IE.Browser*
- *Iemobile.Browser*
- *iPhone.Browser*
- *opera.Browser*
- *Safari.Browser*

#### <a name="using-browser-capabilities-providers"></a>Usar proveedores de funciones de explorador

En ASP.NET versión 3.5 Service Pack 1, puede definir las capacidades que tiene un explorador de las maneras siguientes:

- En el nivel de equipo, se crea o actualiza un `.browser` archivo XML en la carpeta siguiente:

- [!code-console[Main](overview/samples/sample27.cmd)]

- Después de definir la funcionalidad de explorador, ejecute el siguiente comando desde el símbolo Visual Studio con el fin de volver a generar el ensamblado de funciones de explorador y lo agrega a la GAC:

- [!code-console[Main](overview/samples/sample28.cmd)]

- Para una aplicación individual, se crea un `.browser` archivo en la aplicación `App_Browsers` carpeta.

Estos métodos requieren que se cambian los archivos XML, y para los cambios de nivel de equipo, debe reiniciar la aplicación después de ejecutar aspnet\_regbrowsers.exe proceso.

ASP.NET 4 incluye una característica que se conoce como *proveedores de funciones de explorador*. Como sugiere su nombre, este permite crear un proveedor que a su vez le permite usa su propio código para determinar las capacidades del explorador.

En la práctica, los programadores a menudo no definen las funciones del explorador personalizado. Explorador de archivos son difícil de actualizar, el proceso para actualizarlos es bastante complicado y la sintaxis XML de `.browser` archivos pueden ser bastante complicados y definir. Lo que haría que este proceso mucho más fácil es que si hubiera una sintaxis de definición de explorador común o una base de datos que contiene las definiciones de explorador actualizadas, o incluso un servicio Web para una base de datos. La nueva característica de proveedores de funciones de explorador hace que estos escenarios posibles y resulta muy práctico para los desarrolladores de aplicaciones de terceros.

Hay dos enfoques principales para usar la nueva característica de proveedor de funciones de explorador de ASP.NET 4: ampliar las capacidades del explorador ASP.NET la funcionalidad de definición o reemplazarlo completamente. Las siguientes secciones describen en primer lugar cómo reemplazar la funcionalidad y, a continuación, cómo hacerlo.

#### <a name="replacing-the-aspnet-browser-capabilities-functionality"></a>Reemplazar la funcionalidad de las capacidades del explorador de ASP.NET

Para reemplazar completamente la funcionalidad de definición de las capacidades del explorador ASP.NET, siga estos pasos:

1. Crear una clase de proveedor que se derive de *HttpCapabilitiesProvider* y que invalida la *GetBrowserCapabilities* método, como en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample29.cs)]

    El código de este ejemplo crea un nuevo *HttpBrowserCapabilities* de objetos, especificar solo la capacidad denominada Explorador y la configuración de esta capacidad a MyCustomBrowser.
2. Registre el proveedor con la aplicación. 

    Para usar un proveedor con una aplicación, debe agregar el *proveedor* atribuir a la *browserCaps* sección la `Web.config` o `Machine.config` archivos. (También puede definir los atributos de proveedor en un *ubicación* (elemento) para los directorios específicos de la aplicación, como se muestra en una carpeta para un dispositivo móvil específico.) En el ejemplo siguiente se muestra cómo establecer el *proveedor* atributo en un archivo de configuración:

    [!code-xml[Main](overview/samples/sample30.xml)]

    Otra manera de registrar la nueva definición de capacidad del explorador consiste en utilizar código, tal como se muestra en el ejemplo siguiente:

    [!code-csharp[Main](overview/samples/sample31.cs)]

    Este código debe ejecutarse el *aplicación\_iniciar* eventos de la `Global.asax` archivo. Cualquier cambio en el *BrowserCapabilitiesProvider* clase debe producir antes de que se ejecuta cualquier código de la aplicación, con el fin de asegurarse de que la memoria caché permanece en un estado válido para el resuelto *HttpCapabilitiesBase* objeto.

#### <a name="caching-the-httpbrowsercapabilities-object"></a>Almacenamiento en caché el objeto HttpBrowserCapabilities

El ejemplo anterior tiene un problema, que es que el código ejecutará cada vez que el proveedor personalizado se invoca con el fin de obtener la *HttpBrowserCapabilities* objeto. Esto puede producirse varias veces durante cada solicitud. En el ejemplo, el código para el proveedor hace mucho. Sin embargo, si el código en un proveedor personalizado realiza mucho trabajo en fin de obtener la *HttpBrowserCapabilities* objeto, esto puede afectar al rendimiento. Para evitar que esto suceda, puede almacenar en caché el *HttpBrowserCapabilities* objeto. Siga estos pasos:

1. Cree una clase que deriva de *HttpCapabilitiesProvider*, como el en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample32.cs)]

    En el ejemplo, el código genera una clave de caché mediante una llamada a un método BuildCacheKey personalizado y obtiene la longitud de tiempo en caché mediante una llamada a un método GetCacheTime personalizado. El código, a continuación, agrega el resuelto *HttpBrowserCapabilities* objeto a la memoria caché. El objeto se puede recuperar de la memoria caché y reutilizar en las solicitudes posteriores que usan el proveedor personalizado.
2. Registre el proveedor con la aplicación como se describe en el procedimiento anterior.

#### <a name="extending-aspnet-browser-capabilities-functionality"></a>Extender la funcionalidad de las capacidades del explorador de ASP.NET

La sección anterior describe cómo crear un nuevo *HttpBrowserCapabilities* objeto en ASP.NET 4. También puede ampliar la funcionalidad de las capacidades del explorador ASP.NET agregando nuevas definiciones de funciones de explorador a aquellos que ya están en ASP.NET. Puede hacerlo sin utilizar las definiciones del explorador XML. El siguiente procedimiento muestra cómo.

1. Cree una clase que deriva de *HttpCapabilitiesEvaluator* y que invalida la *GetBrowserCapabilities* método, tal como se muestra en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample33.cs)]

    En primer lugar, este código usa la funcionalidad de las capacidades del explorador ASP.NET para intentar identificar el explorador. Sin embargo, si no se identifica ningún explorador se basa en la información definida en la solicitud (es decir, si la *explorador* propiedad de la *HttpBrowserCapabilities* objeto es la cadena "Unknown"), las llamadas de código el proveedor personalizado (MyBrowserCapabilitiesEvaluator) para identificar el explorador.
2. Registre el proveedor con la aplicación tal como se describe en el ejemplo anterior.

#### <a name="extending-browser-capabilities-functionality-by-adding-new-capabilities-to-existing-capabilities-definitions"></a>Ampliar la funcionalidad de las capacidades del explorador agregando nuevas capacidades a las definiciones de las capacidades existentes

Además de crear un proveedor de definición de explorador personalizado y crear dinámicamente nuevas definiciones del explorador, puede ampliar las definiciones de explorador existentes con funcionalidades adicionales. Esto le permite usar una definición que está cerca de la acción que realizará pero carece de algunas funciones que solo. Para hacer esto, siga los pasos siguientes:

1. Cree una clase que deriva de *HttpCapabilitiesEvaluator* y que invalida la *GetBrowserCapabilities* método, tal como se muestra en el ejemplo siguiente: 

    [!code-csharp[Main](overview/samples/sample34.cs)]

    El código de ejemplo extiende ASP.NET existente *HttpCapabilitiesEvaluator* clase y obtiene la *HttpBrowserCapabilities* objeto que coincide con la definición de la solicitud actual con el código siguiente :

    [!code-csharp[Main](overview/samples/sample35.cs)]

    El código, a continuación, puede agregar o modificar una función para este explorador. Hay dos maneras de especificar una nueva funcionalidad de explorador:

    - Agregar un par clave-valor a la *IDictionary* objeto expuesto por el *capacidades* propiedad de la *HttpCapabilitiesBase* objeto. En el ejemplo anterior, el código agrega una función denominada multitoque con un valor de *true*.
    - Establecer las propiedades existentes de la *HttpCapabilitiesBase* objeto. En el ejemplo anterior, el código establece la *marcos* propiedad *true*. Esta propiedad es simplemente un descriptor de acceso para la *IDictionary* objeto expuesto por el *capacidades* propiedad. 

        > [!NOTE]
        > Tenga en cuenta este modelo se aplica a ninguna propiedad de *HttpBrowserCapabilities*, incluidos los adaptadores de control.
2. Registre el proveedor con la aplicación tal como se describe en el procedimiento anterior.

<a id="0.2__Toc224729036"></a><a id="0.2__Toc253429260"></a><a id="0.2__Toc243304634"></a>

### <a name="routing-in-aspnet-4"></a>Enrutamiento en ASP.NET 4

ASP.NET 4 agrega compatibilidad integrada para usar el enrutamiento con formularios Web Forms. Enrutamiento le permite que configurar una aplicación para aceptar la solicitud las direcciones URL que no se asignan a archivos físicos. En su lugar, puede usar el enrutamiento para definir direcciones URL que son significativos para los usuarios y que puede ayudarle con la optimización de motor de búsqueda (SEO) para la aplicación. Por ejemplo, la dirección URL de una página que muestra categorías de productos en una aplicación existente podría ser similar al ejemplo siguiente:

[!code-console[Main](overview/samples/sample36.cmd)]

Mediante el enrutamiento, puede configurar la aplicación para aceptar la siguiente dirección URL para representar la misma información:

[!code-console[Main](overview/samples/sample37.cmd)]

Enrutamiento ha estado disponible a partir de ASP.NET 3.5 SP1. (Para obtener un ejemplo de cómo usar el enrutamiento de ASP.NET 3.5 SP1, vea la entrada [utilizando enrutamiento con WebForms](http://haacked.com/archive/2008/03/11/using-routing-with-webforms.aspx "título de esta entrada.") en el blog de Phil Haack.) Sin embargo, ASP.NET 4 incluye algunas características que resulten más fácil utilizar el enrutamiento, incluidas las siguientes:

- El *PageRouteHandler* (clase), que es un controlador HTTP sencillo que utilizar al definir las rutas. La clase pasa los datos a la página que se enruta la solicitud.
- Las nuevas propiedades *HttpRequest.RequestContext* y *Page.RouteData* (que es un proxy para el *HttpRequest.RequestContext.RouteData* objeto). Estas propiedades facilitan información de acceso que se pasa desde la ruta de acceso.
- Los siguientes nuevos generadores de expresiones, que se definen en *System.Web.Compilation.RouteUrlExpressionBuilder* y *System.Web.Compilation.RouteValueExpressionBuilder*:
- *RouteUrl*, que proporciona una manera sencilla para crear una dirección URL que corresponde a una dirección URL de ruta dentro de un control de servidor ASP.NET.
- *RouteValue*, que proporciona una manera sencilla para extraer información de la *RouteContext* objeto.
- El *RouteParameter* (clase), lo que facilita al pasar los datos contenidos en un *RouteContext* objeto a una consulta para un control de origen de datos (similar a [ *FormParameter* ](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx)).

#### <a name="routing-for-web-forms-pages"></a>Enrutamiento para páginas de formularios Web

En el ejemplo siguiente se muestra cómo definir una ruta de formularios Web Forms con el nuevo *MapPageRoute* método de la *ruta* clase:

[!code-csharp[Main](overview/samples/sample38.cs)]

ASP.NET 4 presenta el *MapPageRoute* método. En el ejemplo siguiente es equivalente a la definición de SearchRoute se muestra en el ejemplo anterior, pero utiliza el *PageRouteHandler* clase.

[!code-csharp[Main](overview/samples/sample39.cs)]

El código en el ejemplo asigna la ruta a una página física (en la primera ruta a `~/search.aspx`). La primera definición de ruta también especifica que el parámetro denominado searchterm debe extraer de la dirección URL y se pasa a la página.

El *MapPageRoute* método es compatible con las sobrecargas del método siguiente:

- *MapPageRoute (cadena routeName, routeUrl de cadena, cadena physicalFile, bool checkPhysicalUrlAccess)*
- *MapPageRoute (cadena routeName, routeUrl de cadena, cadena physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary los valores predeterminados)*
- *MapPageRoute (cadena routeName, routeUrl de cadena, cadena physicalFile, bool checkPhysicalUrlAccess, RouteValueDictionary los valores predeterminados, restricciones de RouteValueDictionary)*

El *checkPhysicalUrlAccess* parámetro especifica si la ruta debe comprobar los permisos de seguridad de la página física que se enrutan a (en este caso, search.aspx) y los permisos en la dirección URL entrante (en este caso, buscar / {searchterm}). Si el valor de *checkPhysicalUrlAccess* es *false*, solo los permisos de la dirección URL entrante que se va a comprobar. Estos permisos se definen en el `Web.config` archivo utilizando parámetros como los siguientes:

[!code-xml[Main](overview/samples/sample40.xml)]

En la configuración del ejemplo, se deniega el acceso a la página física `search.aspx` para todos los usuarios excepto aquellos que están en el rol de administrador. Cuando el *checkPhysicalUrlAccess* parámetro está establecido en *true* (que es su valor predeterminado), se permiten solo los usuarios administrativos para tener acceso a la /search/ de dirección URL {searchterm}, porque es la search.aspx página física está restringido a los usuarios de ese rol. Si *checkPhysicalUrlAccess* está establecido en *false* y el sitio está configurado como se muestra en el ejemplo anterior, todos los usuarios autenticados pueden tener acceso a la /search/ de dirección URL {searchterm}.

#### <a name="reading-routing-information-in-a-web-forms-page"></a>Leer información de enrutamiento en una página de formularios Web

En el código de la página física de formularios Web Forms, puede tener acceso a la información de enrutamiento ha extraído de la dirección URL (u otra información que otro objeto haya agregado a la *RouteData* objeto) con dos nuevas propiedades:  *HttpRequest.RequestContext* y *Page.RouteData*. (*Page.RouteData* ajusta *HttpRequest.RequestContext.RouteData*.) En el ejemplo siguiente se muestra cómo usar *Page.RouteData*.

[!code-csharp[Main](overview/samples/sample41.cs)]

El código extrae el valor que se pasó para el parámetro searchterm, tal como se define en la ruta del ejemplo anterior. Tenga en cuenta la siguiente dirección URL de solicitud:

[!code-console[Main](overview/samples/sample42.cmd)]

Cuando se realiza esta solicitud, la palabra "scott" se representarían en la `search.aspx` página.

#### <a name="accessing-routing-information-in-markup"></a>Obtener acceso a información de enrutamiento de marcado

El método descrito en la sección anterior muestra cómo obtener datos de ruta en el código en una página de formularios Web Forms. También puede usar expresiones en el marcado que proporcionan acceso a la misma información. Los generadores de expresiones son una forma eficaz y elegante para trabajar con código declarativo. (Para obtener más información, vea la entrada [Express usted mismo con personalizado generadores de expresiones](http://haacked.com/archive/2006/11/29/Express_Yourself_With_Custom_Expression_Builders.aspx) en el blog de Phil Haack.)

ASP.NET 4 incluye dos nuevos generadores de expresión para el enrutamiento de formularios Web Forms. En el ejemplo siguiente se muestra cómo se utilizan.

[!code-aspx[Main](overview/samples/sample43.aspx)]

En el ejemplo, el *RouteUrl* expresión se utiliza para definir una dirección URL que se basa en un parámetro de ruta. Esto le evita tener que codificar la dirección URL completa en el marcado y le permite cambiar la estructura de la dirección URL más adelante sin necesidad de realizar cualquier cambio en este vínculo.

En función de la ruta definida anteriormente, este marcado genera la siguiente dirección URL:

[!code-console[Main](overview/samples/sample44.cmd)]

ASP.NET resuelve automáticamente la ruta correcta (es decir, genera la dirección URL correcta) en función de los parámetros de entrada. También puede incluir un nombre de ruta en la expresión, que le permite especificar una ruta que se usa.

En el ejemplo siguiente se muestra cómo utilizar el *RouteValue* expresión.

[!code-aspx[Main](overview/samples/sample45.aspx)]

Cuando se ejecuta la página que contiene este control, se muestra el valor "scott" en la etiqueta.

El *RouteValue* expresión, le resultará fácil de usar datos de ruta en el marcado y evita tener que trabajar con la más compleja Page.RouteData["x"] sintaxis en el marcado.

#### <a name="using-route-data-for-data-source-control-parameters"></a>Usar datos de ruta para los parámetros de Control de origen de datos

El *RouteParameter* clase le permite especificar datos de ruta como valor de parámetro para las consultas en un control de origen de datos. Se [funciona prácticamente igual que el](https://msdn.microsoft.com/en-us/library/system.web.ui.webcontrols.formparameter.aspx) de la clase, como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample46.aspx)]

En este caso, el valor de la searchterm de parámetro de ruta se usará para la @companyname parámetro en el *seleccione* instrucción.

<a id="0.2__Toc224729037"></a><a id="0.2__Toc253429261"></a><a id="0.2__Toc243304635"></a>

### <a name="setting-client-ids"></a>Establecer los identificadores de cliente

El nuevo *ClientIDMode* propiedad soluciona un problema desde hace mucho tiempo en ASP.NET, a saber cómo crean controles de la *identificador* atributo para los elementos que se presentan. Conocer el *identificador* atributos de los elementos presentados es importante si la aplicación incluye el script de cliente que hace referencia a estos elementos.

El *identificador* atributo en HTML que se presenta para controles de servidor Web se genera basándose en la *ClientID* propiedad del control. Hasta que ASP.NET 4, el algoritmo para generar el *identificador* de atributo de la *ClientID* propiedad ha sido concatenar el contenedor de nomenclatura (si existe) con el Id. y en el caso de los controles repetidos (como en controles de datos), para agregar un prefijo y un número secuencial. Si esto se garantiza que siempre que los identificadores de controles en la página son únicos, el algoritmo ha resultado en un control identificadores que no estaban predecible y, por lo tanto, eran difíciles de referencia en un script de cliente.

El nuevo *ClientIDMode* propiedad le permite especificar cómo se genera con mayor precisión el identificador de cliente para los controles. Puede establecer la *ClientIDMode* propiedad para cualquier control, incluidos los de la página. Valores posibles son los siguientes:

- *AutoID* : Esto es equivalente al algoritmo para generar *ClientID* valores de propiedad que se usó en versiones anteriores de ASP.NET.
- *Estática* : esta opción especifica que la *ClientID* valor será el mismo que el identificador sin concatenar los identificadores de elemento primario contenedores de nomenclatura. Esto puede ser útil en los controles de usuario Web. Dado que un control de usuario Web puede encontrarse en distintas páginas y en los controles de contenedor diferente, puede ser difícil de escribir el script de cliente para los controles que utilizan el *AutoID* algoritmo porque no se puede predecir cuál será los valores de Id. .
- *Predicción* : esta opción es principalmente para su uso en controles de datos que usan plantillas de repetición. Concatena las propiedades de identificación de contenedores de nomenclatura del control, pero generado *ClientID* valores no contienen cadenas como "ctlxxx". Esta configuración funciona junto con el *ClientIDRowSuffix* propiedad del control. Establece el *ClientIDRowSuffix* propiedad para el nombre de un campo de datos y el valor de ese campo se utiliza como el sufijo para las *ClientID* valor. Normalmente se usa la clave principal de un registro de datos como el *ClientIDRowSuffix* valor.
- *Heredar* : este valor es el comportamiento predeterminado para los controles; especifica que la generación de Id. de un control es igual a su elemento primario.

Puede establecer la *ClientIDMode* propiedad en el nivel de página. Esto define el valor predeterminado *ClientIDMode* valor para todos los controles en la página actual.

El valor predeterminado *ClientIDMode* valor en el nivel de página es *AutoID*y el valor predeterminado *ClientIDMode* valor en el nivel de control es *heredar*. Como resultado, si no establece esta propiedad en cualquier lugar en el código, todos los controles de forma predeterminada el *AutoID* algoritmo.

Establezca el valor de nivel de página el *@ Page* directiva, tal como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample47.aspx)]

También puede establecer el *ClientIDMode* valor en el archivo de configuración, en el nivel de equipo (equipo) o en el nivel de aplicación. Esto define el valor predeterminado *ClientIDMode* establecer para todos los controles en todas las páginas de la aplicación. Si establece el valor en el nivel de equipo, define el valor predeterminado *ClientIDMode* establecer para todos los sitios Web en ese equipo. El siguiente ejemplo se muestra la *ClientIDMode* el archivo de configuración:

[!code-xml[Main](overview/samples/sample48.xml)]

Como se indicó anteriormente, el valor de la *ClientID* propiedad se deriva el contenedor de nomenclatura para el elemento primario del control. En algunos escenarios, como cuando se utiliza páginas maestras, controles pueden acabar con identificadores similares a las de la siguiente elemento HTML representado:

[!code-html[Main](overview/samples/sample49.html)]

Aunque la *entrada* elemento mostrado en el marcado (desde un *cuadro de texto* control) es solo dos contenedores de nomenclaturas profundidad en la página (anidado *ContentPlaceholder* controles), debido al modo en que se procesan las páginas maestras, el resultado final es un Id. de control similar al siguiente:

[!code-console[Main](overview/samples/sample50.cmd)]

Este identificador se garantiza que sea único en la página, pero es innecesariamente largo para la mayoría de los objetivos. Imagine que desea para reducir la longitud del identificador representado y tener más control sobre cómo se genera el identificador. (Por ejemplo, desea eliminar los prefijos "ctlxxx"). Es la manera más fácil de lograr esto estableciendo el *ClientIDMode* propiedad tal y como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample51.aspx)]

En este ejemplo, el *ClientIDMode* propiedad está establecida en *estático* para el contenedor más externo *NamingPanel* y establezca a *Predictable* para interna *NamingControl* elemento. Esta configuración se producen en el siguiente marcado (el resto de la página y la página maestra se supone que es el mismo como en el ejemplo anterior):

[!code-html[Main](overview/samples/sample52.html)]

El *estático* configuración tiene el efecto de restablecimiento de la jerarquía de nomenclatura para todos los controles en el contenedor más externo *NamingPanel* elemento y de lo que elimina la *ContentPlaceHolder* y *MasterPage* identificadores desde el identificador generado. (El *nombre* atributo de elementos representados no se ve afectado, por lo que se conserva la funcionalidad ASP.NET normal de eventos, el estado de vista y así sucesivamente.) Un efecto secundario de restablecimiento de la jerarquía de nomenclatura es que, incluso si lo mueve el marcado para la *NamingPanel* elementos en otro *ContentPlaceholder* control, los identificadores de cliente representado siguen siendo los mismos.

> [!NOTE]
> Tenga en cuenta que depende de usted para asegurarse de que los identificadores de control representado sean únicos. Si no lo están, puede interrumpir cualquier función que requiera identificadores únicos de los elementos individuales de HTML, como el cliente de *document.getElementById* función.


#### <a name="creating-predictable-client-ids-in-data-bound-controls"></a>Creación de identificadores de cliente predecibles en controles enlazados a datos

El *ClientID* valores que se generan para los controles en un control de lista enlazado a datos mediante el algoritmo heredado pueden ser de longitud y no son realmente predecibles. El *ClientIDMode* funcionalidad puede ayudarle a tener más control sobre el uso de estos identificadores se generan.

El marcado en el ejemplo siguiente se incluye un *ListView* control:

[!code-aspx[Main](overview/samples/sample53.aspx)]

En el ejemplo anterior, el *ClientIDMode* y *RowClientIDRowSuffix* propiedades se establecen en el marcado. El *ClientIDRowSuffix* propiedad puede utilizarse únicamente en controles enlazados a datos y su comportamiento difiere dependiendo de qué control está utilizando. Éstas son las diferencias:

- *GridView* control, puede especificar el nombre de una o más columnas del origen de datos, que se combinan en tiempo de ejecución para crear el cliente de identificadores. Por ejemplo, si establece *RowClientIDRowSuffix* "ProductName, ProductId," identificadores de controles de elementos presentados tendrá un formato similar al siguiente:

- [!code-console[Main](overview/samples/sample54.cmd)]

- *ListView* control, puede especificar una sola columna en el origen de datos que se anexa al identificador de cliente. Por ejemplo, si establece *ClientIDRowSuffix* "ProductName", los identificadores de control representado tendrá un formato similar al siguiente:

- [!code-console[Main](overview/samples/sample55.cmd)]

- En este caso el 1 finales se deriva de la Id. de producto del elemento de datos actual.

- *Repetidor* control, este control no admite la *ClientIDRowSuffix* propiedad. En un *repetidor* se usa el control, el índice de la fila actual. Cuando usas ClientIDMode = "Predicción" con un *repetidor* controlar, se generan identificadores de cliente que tienen el formato siguiente:

- [!code-console[Main](overview/samples/sample56.cmd)]

- El 0 final es el índice de la fila actual.

El *FormView* y *DetailsView* controles no muestran varias filas, por lo que no admiten la *ClientIDRowSuffix* propiedad.

<a id="0.2__Toc224729038"></a><a id="0.2__Toc253429262"></a><a id="0.2__Toc243304636"></a>

### <a name="persisting-row-selection-in-data-controls"></a>Guardar selección de filas en los controles de datos

El *GridView* y *ListView* controles pueden permitir al usuario seleccionar una fila. En versiones anteriores de ASP.NET, la selección se ha basado en el índice de fila en la página. Por ejemplo, si selecciona el tercer elemento en la página 1 y, a continuación, moverse a la página 2, se selecciona el tercer elemento en la página.

Selección persistente inicialmente se admite únicamente en proyectos de datos dinámicos en .NET Framework 3.5 SP1. Cuando esta característica está habilitada, el elemento seleccionado actualmente se basa en la clave de datos para el elemento. Esto significa que si selecciona la tercera fila en la página 1 y moverse a la página 2, no se selecciona nada en la página 2. Al regresar a la página 1, la tercera fila aún está seleccionada. Selección persistente ahora es compatible la *GridView* y *ListView* controles en todos los proyectos utilizando el *EnablePersistedSelection* propiedad, como se muestra en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample57.aspx)]

<a id="0.2__Toc253429263"></a><a id="0.2__Toc243304637"></a>

### <a name="aspnet-chart-control"></a>Control Chart de ASP.NET

ASP.NET *gráfico* control expande las ofertas de visualización de datos de .NET Framework. Mediante el *gráfico* control, puede crear páginas ASP.NET que incluir gráficos intuitivos y visualmente atractivos para un análisis estadístico o financiero complejo. ASP.NET *gráfico* control se introdujo como un complemento a la versión de .NET Framework versión 3.5 SP1 y forma parte de la versión de .NET Framework 4.

El control incluye las siguientes características:

- 35 tipos de gráfico distintos.
- Un número ilimitado de áreas de gráfico, títulos, leyendas y anotaciones.
- Una gran variedad de opciones de apariencia para todos los elementos de gráfico.
- Compatibilidad con la mayoría de los tipos de gráficos 3D.
- Etiquetas de datos inteligentes que pueden ajustarse automáticamente alrededor de los puntos de datos.
- Las franjas, quiebres de escala y escala logarítmica.
- Más de 50 fórmulas financieras y estadísticas para la transformación y el análisis de datos.
- Enlace simple y la manipulación de datos del gráfico.
- Compatibilidad con formatos de datos comunes, como moneda, fechas y horas.
- Compatibilidad con interactividad y personalización orientada a eventos, como cliente click (eventos) mediante Ajax.
- Administración de estados.
- Transmisión de flujo binario.

Las ilustraciones siguientes muestran ejemplos de gráficos financieros producidos por el control Chart de ASP.NET.

<a id="0.2_graphic17"></a>![](overview/_static/image1.png)

Figura 2: Ejemplo de control de gráfico de ASP.NET

Para obtener más ejemplos de cómo usar el control Chart de ASP.NET, descargue el código de ejemplo en el [entorno de ejemplos para los controles de gráfico de Microsoft](https://go.microsoft.com/fwlink/?LinkId=128300) página en el sitio Web de MSDN. Encontrará más muestras de comunidad del contenido en el [foro de Control de gráfico](https://go.microsoft.com/fwlink/?LinkId=128713).

#### <a name="adding-the-chart-control-to-an-aspnet-page"></a>Agregar el Control Chart a una página ASP.NET

En el ejemplo siguiente se muestra cómo agregar un *gráfico* control a una página ASP.NET mediante el uso de marcado. En el ejemplo, el *gráfico* control genera un gráfico de columnas para los puntos de datos estáticos.

[!code-aspx[Main](overview/samples/sample58.aspx)]

#### <a name="using-3-d-charts"></a>Usar gráficos 3D

El *gráfico* control contiene un *ChartAreas* colección, que puede contener *ChartArea* objetos que definen las características de las áreas del gráfico. Por ejemplo, para utilizar efectos 3D para un área del gráfico, use la *Area3DStyle* propiedad como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample59.aspx)]

La siguiente ilustración muestra un gráfico 3D con cuatro series de la *barra* tipo de gráfico.

<a id="0.2_graphic18"></a>![](overview/_static/image2.png)

Figura 3: gráfico de barras 3D

#### <a name="using-scale-breaks-and-logarithmic-scales"></a>Uso de quiebres de escala y las escalas logarítmicas son

Los quiebres de escala y las escalas logarítmicas son otras dos formas de agregar sofisticación al gráfico. Estas características son específicas de cada eje en un área del gráfico. Por ejemplo, para usar estas características en el eje Y principal de un área del gráfico, use la *AxisY.IsLogarithmic* y *ScaleBreakStyle* propiedades en un *ChartArea* objeto. El fragmento de código siguiente muestra cómo utilizar quiebres de escala en el eje Y principal.

[!code-aspx[Main](overview/samples/sample60.aspx)]

La siguiente ilustración muestra el eje Y con quiebres de escala habilitados.

<a id="0.2_graphic19"></a>![](overview/_static/image3.png)

Figura 4: Quiebres de escala

<a id="0.2__QueryExtender"></a><a id="0.2__Toc224729041"></a><a id="0.2__Toc253429264"></a><a id="0.2__Toc243304638"></a>

### <a name="filtering-data-with-the-queryextender-control"></a>Filtrar datos con el Control QueryExtender

Es una tarea muy común para los programadores que crean páginas Web controladas por datos filtrar los datos. Esto tradicionalmente se ha realizado mediante la creación de *donde* controles de origen de las cláusulas de datos. Este enfoque puede resultar complicado y en algunos casos el *donde* sintaxis no le permite aprovechar las ventajas de la funcionalidad completa de la base de datos subyacente.

Para hacer que el filtrado, una nueva *QueryExtender* control se ha agregado en ASP.NET 4. Este control puede agregarse a *EntityDataSource* o *LinqDataSource* controles con el fin de filtrar los datos devueltos por estos controles. Dado que la *QueryExtender* control se basa en LINQ, el filtro se aplica en el servidor de base de datos antes de que los datos se envían a la página, lo que resulta en operaciones muy eficaces.

El *QueryExtender* control admite una variedad de opciones de filtro. Las siguientes secciones describen estas opciones y proporcionan ejemplos de cómo usarlas.

#### <a name="search"></a>Buscar

Para la opción de búsqueda, el *QueryExtender* control realiza una búsqueda en los campos especificados. En el ejemplo siguiente, el control utiliza el texto que se escribe en el control de TextBoxSearch y busca su contenido en el `ProductName` y `Supplier.CompanyName` columnas de los datos que se devuelven desde el *LinqDataSource* control.

[!code-aspx[Main](overview/samples/sample61.aspx)]

#### <a name="range"></a>Intervalo

La opción de intervalo es similar a la opción de búsqueda, pero especifica un par de valores para definir el intervalo. En el ejemplo siguiente, la *QueryExtender* búsquedas de control el `UnitPrice` columna en los datos devueltos por la *LinqDataSource* control. El intervalo es de lectura de los controles de TextBoxFrom y TextBoxTo en la página.

[!code-aspx[Main](overview/samples/sample62.aspx)]

#### <a name="propertyexpression"></a>PropertyExpression

La opción de expresión de propiedad le permite definir una comparación en un valor de propiedad. Si la expresión se evalúa como *true*, se devuelven los datos que se está examinando. En el ejemplo siguiente, la *QueryExtender* control filtra los datos mediante la comparación de los datos de la `Discontinued` columna en el valor del control CheckBoxDiscontinued en la página.

[!code-aspx[Main](overview/samples/sample63.aspx)]

#### <a name="customexpression"></a>Expresión personalizada

Por último, puede especificar una expresión personalizada que se usará con el *QueryExtender* control. Esta opción le permite llamar a una función en la página que define la lógica del filtro personalizado. En el ejemplo siguiente se muestra cómo especificar mediante declaración una expresión personalizada en el *QueryExtender* control.

[!code-aspx[Main](overview/samples/sample64.aspx)]

En el ejemplo siguiente se muestra la función personalizada que se invoca mediante el *QueryExtender* control. En este caso, en lugar de usar una consulta de base de datos que incluye una *donde* cláusula, el código usa una consulta LINQ para filtrar los datos.

[!code-csharp[Main](overview/samples/sample65.cs)]

Estos ejemplos muestran solo una expresión que se va a usar en el *QueryExtender* control a la vez. Sin embargo, puede incluir varias expresiones dentro de la *QueryExtender* control.

<a id="0.2__Toc253429265"></a><a id="0.2__Toc243304639"></a>

### <a name="html-encoded-code-expressions"></a>Expresiones de código codificado en HTML

Algunos sitios ASP.NET (sobre todo con ASP.NET MVC) dependen en gran medida utilizando `<%` =  `expression %>` sintaxis (también denominados "fragmentos de código") para escribir texto en la respuesta. Cuando se usan expresiones de código, es fácil olvidarse de codificar en HTML el texto si el texto proviene de usuario de entrada, puede permanecer activa páginas a un ataque XSS (Cross Site Scripting).

ASP.NET 4 presenta la nueva sintaxis de expresiones de código siguiente:

[!code-aspx[Main](overview/samples/sample66.aspx)]

Esta sintaxis utiliza codificación HTML de forma predeterminada cuando se escribe en la respuesta. Esta nueva expresión eficazmente se traduce en lo siguiente:

[!code-aspx[Main](overview/samples/sample67.aspx)]

Por ejemplo, &lt;%: solicitud ["/userinput"] %&gt; realiza la codificación HTML en el valor de *solicitar ["/userinput"]*.

El objetivo de esta característica es para que sea posible reemplazar todas las instancias de la sintaxis anterior con la nueva sintaxis para que no tiene que decidir en cada paso de cuál utilizar. Sin embargo, hay casos en los que el texto que se va a imprimir está destinado a ser HTML o que se ha codificado, en cuyo caso podría producir una codificación doble.

En esos casos, ASP.NET 4 presenta una nueva interfaz, *IHtmlString*, junto con una implementación concreta, *HtmlString*. Instancias de estos tipos le permiten indicar que el valor devuelto es ya correctamente codificado (o en caso contrario, se examinan) para mostrar como HTML, y que, por tanto, el valor no debería estar codificado en HTML nuevo. Por ejemplo, la siguiente no debería ser (y no es) codificado en HTML:

[!code-aspx[Main](overview/samples/sample68.aspx)]

Métodos de aplicación auxiliar de ASP.NET MVC 2 se han actualizado para trabajar con esta nueva sintaxis para que no sean dobles codificados, pero solo cuando se ejecuta ASP.NET 4. Esta nueva sintaxis no funciona cuando se ejecuta una aplicación mediante ASP.NET 3.5 SP1.

Tenga en cuenta que esto no garantiza la protección frente a ataques XSS. Por ejemplo, HTML que utiliza valores de atributo que no están entre comillas puede contener proporcionados por el usuario que sigue siendo vulnerable. Tenga en cuenta que el resultado de los controles ASP.NET y aplicaciones auxiliares de MVC de ASP.NET siempre incluye valores de atributo entre comillas, que es el enfoque recomendado.

Del mismo modo, esta sintaxis no realiza la codificación de JavaScript, como cuando se crea una cadena de JavaScript según proporcionados por el usuario.

<a id="0.2__Toc253429266"></a><a id="0.2__Toc243304640"></a>

### <a name="project-template-changes"></a>Cambios de la plantilla de proyecto

En versiones anteriores de ASP.NET, cuando se usa Visual Studio para crear un nuevo proyecto de sitio Web o proyecto de aplicación Web, los proyectos resultantes contienen solo una página Default.aspx, valor predeterminado es `Web.config` archivo y el `App_Data` carpeta, como se muestra en el siguiente ilustración:

<a id="0.2_graphic1A"></a>![](overview/_static/image4.png)

Visual Studio también admite un tipo de proyecto de sitio Web vacío, que no contiene ningún archivo en absoluto, tal como se muestra en la ilustración siguiente:

<a id="0.2_graphic1B"></a>![](overview/_static/image5.png)

El resultado es que para los principiantes, hay muy poca información sobre cómo compilar una aplicación Web de producción. Por consiguiente, ASP.NET 4 presenta tres nuevas plantillas, uno para un proyecto de aplicación Web vacío y uno para cada uno de un proyecto de aplicación Web y sitio Web.

#### <a name="empty-web-application-template"></a>Plantilla de aplicación Web vacía

Como sugiere su nombre, la plantilla de aplicación Web vacía es un proyecto de aplicación Web reducido. Seleccione esta plantilla de proyecto en el cuadro de diálogo nuevo proyecto de Visual Studio, tal como se muestra en la ilustración siguiente:

[![](overview/_static/image7.png)](overview/_static/image6.png)

([Haga clic aquí para ver la imagen a tamaño completo](overview/_static/image8.png))

Cuando se crea una aplicación Web de ASP.NET vacía, Visual Studio crea el diseño de la carpeta siguiente:

<a id="0.2_graphic1D"></a>![](overview/_static/image9.png)

Esto es similar al diseño del sitio Web vacío de versiones anteriores de ASP.NET, con una excepción. En Visual Studio 2010, los proyectos de aplicación Web vacía y el sitio Web vacío contienen la siguiente mínimo `Web.config` archivo que contiene información utilizada por Visual Studio para identificar el proyecto tiene como destino .NET framework:

<a id="0.2_graphic1E"></a>![](overview/_static/image10.png)

Sin este *targetFramework* propiedad, los valores predeterminados de Visual Studio a la versión 2.0 de .NET Framework de destino con el fin de mantener la compatibilidad al abrir las aplicaciones más antiguas.

#### <a name="web-application-and-web-site-project-templates"></a>Aplicación Web y las plantillas de proyecto de sitio Web

Las otras dos nuevas plantillas de proyecto que se suministran con Visual Studio 2010 contienen cambios más importantes. La siguiente ilustración muestra el diseño del proyecto que se crea cuando se crea un nuevo proyecto de aplicación Web. (El diseño de un proyecto de sitio Web es prácticamente idéntico).

- <a id="0.2_graphic1F"></a>![](overview/_static/image11.png)

El proyecto incluye un número de archivos que no se crearon en versiones anteriores. Además, el nuevo proyecto de aplicación Web se configura con la funcionalidad de pertenencia básica, que le permite empezar a trabajar rápidamente en la protección de acceso a la nueva aplicación. Debido a esta inclusión, el `Web.config` archivo para el nuevo proyecto incluye las entradas que se utilizan para configurar la pertenencia, funciones y perfiles. El siguiente ejemplo se muestra el `Web.config` archivo para un nuevo proyecto de aplicación Web. (En este caso, *roleManager* está deshabilitado.)

[![](overview/_static/image13.png)](overview/_static/image12.png)

([Haga clic aquí para ver la imagen a tamaño completo](overview/_static/image14.png))

El proyecto también contiene un segundo `Web.config` un archivo en el `Account` directory. El segundo archivo de configuración proporciona una manera de proteger el acceso a la página ChangePassword.aspx para no registradas en los usuarios. En el ejemplo siguiente se muestra el contenido del segundo `Web.config` archivo.

![](overview/_static/image15.png)

Las páginas que se crean de forma predeterminada en las nuevas plantillas de proyecto también contienen más contenido que en versiones anteriores. El proyecto contiene una página maestra predeterminada y el archivo CSS y la página predeterminada (Default.aspx) está configurada para usar la página maestra predeterminada. El resultado es que al ejecutar la aplicación Web o sitio Web por primera vez, la página predeterminada (principal) ya está operativo. De hecho, es similar a la página predeterminada que verá al iniciarse una nueva aplicación MVC.

[![](overview/_static/image17.png)](overview/_static/image16.png)

([Haga clic aquí para ver la imagen a tamaño completo](overview/_static/image18.png))

Es la intención de estos cambios en las plantillas de proyecto proporcionar instrucciones sobre cómo empezar a crear una nueva aplicación Web. Con correcto semánticamente estricta XHTML 1.0 conforme formato y diseño que se especifica el uso de CSS, las páginas en las plantillas representan los procedimientos recomendados para crear aplicaciones Web de ASP.NET 4. Las páginas de forma predeterminada también tienen un diseño de dos columnas que se puede personalizar fácilmente.

Por ejemplo, suponga que para una nueva aplicación Web que desea cambiar algunos de los colores e insertar el logotipo de su empresa en lugar del logotipo de mi aplicación ASP.NET. Para ello, cree un nuevo directorio en `Content` para almacenar la imagen de logotipo:

<a id="0.2_graphic23"></a>![](overview/_static/image19.png)

Para agregar la imagen a la página, a continuación, abra el `Site.Master` de archivos, busque donde se define el texto de mi aplicación ASP.NET y reemplazarlo con un *imagen* elemento cuyo *src* atributo se establece en el nuevo logotipo imagen, como en el ejemplo siguiente:

[![](overview/_static/image21.png)](overview/_static/image20.png)

([Haga clic aquí para ver la imagen a tamaño completo](overview/_static/image22.png))

A continuación, puede incluirse en el archivo Site.css y modificar definiciones de clase CSS para cambiar el color de fondo de la página, así como el encabezado.

El resultado de estos cambios es que puede mostrar una página de inicio personalizada con muy poco esfuerzo:

[![](overview/_static/image24.png)](overview/_static/image23.png)

([Haga clic aquí para ver la imagen a tamaño completo](overview/_static/image25.png))

<a id="0.2__Toc253429267"></a><a id="0.2__Toc243304641"></a>

### <a name="css-improvements"></a>Mejoras de CSS

Una de las principales áreas de trabajo de ASP.NET 4 ha sido ayudan a representar HTML que sea compatible con los estándares más recientes de HTML. Esto incluye los cambios en la forma en que los controles de servidor Web de ASP.NET utilizan los estilos CSS.

#### <a name="compatibility-setting-for-rendering"></a>Configuración de compatibilidad para la representación

De forma predeterminada, cuando una aplicación Web o sitio Web tiene como destino .NET Framework 4, el *controlRenderingCompatibilityVersion* atributo de la *páginas* elemento está establecido en "4.0". Este elemento se define en el nivel de equipo `Web.config` de archivos y de forma predeterminada se aplica a todas las aplicaciones de ASP.NET 4:

[!code-xml[Main](overview/samples/sample69.xml)]

El valor de *controlRenderingCompatibility* es una cadena, lo que permite posibles nuevas definiciones de versión en versiones futuras. En la versión actual, se admiten los siguientes valores para esta propiedad:

- "3.5". Este valor indica la representación heredada y marcado. Marcado que se representa mediante controles es compatible con versiones anteriores de 100% y la configuración de la *xhtmlConformance* se respeta la propiedad.
- "4.0". Si la propiedad tiene esta configuración, controles de servidor Web de ASP.NET, haga lo siguiente:
- El *xhtmlConformance* propiedad siempre se trata como "Strict". Como resultado, los controles representan marcado XHTML 1.0 Strict.
- Deshabilitar los controles que no es de entrada ya no representa los estilos no válidos.
- *div* elementos alrededor de los campos ocultos ahora tienen un estilo por lo que no interfieren con las reglas de CSS creados por el usuario.
- Los controles de menú representar el marcado que es semánticamente correcto y que cumplen con las directrices de accesibilidad.
- Controles de validación no representan los estilos en línea.
- Los controles que representan previamente borde = "0" (controles que derivan de ASP.NET *tabla* control y ASP.NET *imagen* control) ya no presentan este atributo.

#### <a name="disabling-controls"></a>Deshabilitar controles

En ASP.NET 3.5 SP1 y versiones anteriores, el marco de trabajo procesa los *deshabilitado* de atributo en el marcado HTML para todos los controles cuyo *habilitado* propiedad establecida en *false*. Sin embargo, según la especificación de HTML 4.01, solo *entrada* elementos deben tener este atributo.

En ASP.NET 4, puede establecer la *controlRenderingCompatabilityVersion* propiedad a "3.5", como en el ejemplo siguiente:

[!code-xml[Main](overview/samples/sample70.xml)]

Puede crear el marcado para un *etiqueta* control similar al siguiente, que deshabilita el control:

[!code-aspx[Main](overview/samples/sample71.aspx)]

El *etiqueta* control podría invalidar el código HTML siguiente:

[!code-html[Main](overview/samples/sample72.html)]

En ASP.NET 4, puede establecer la *controlRenderingCompatabilityVersion* a "4.0". En ese caso, sólo los controles que se representen *entrada* elementos se representarán un *deshabilitado* atributo cuando el control *habilitado* propiedad está establecida en *false* . Controles que no puede representar HTML *entrada* elementos se representan en su lugar un *clase* atributo que hace referencia a una clase CSS que puede utilizar para definir un aspecto deshabilitado para el control. Por ejemplo, el *etiqueta* control se muestra en el ejemplo anterior generará el siguiente marcado:

[!code-html[Main](overview/samples/sample73.html)]

El valor predeterminado de la clase que ha especificado para este control es "aspNetDisabled". Sin embargo, puede cambiar este valor predeterminado mediante el establecimiento estático *DisabledCssClass* propiedad estática de la *WebControl* clase. Para los programadores de controles, el comportamiento que se usará para un control concreto también pueden definirse mediante el *SupportsDisabledAttribute* propiedad.

<a id="0.2__Toc253429268"></a><a id="0.2__Toc243304642"></a>

### <a name="hiding-div-elements-around-hidden-fields"></a>Ocultar div elementos alrededor de campos ocultos

ASP.NET 2.0 y versiones posteriores representan los campos ocultos de específicas del sistema (como el *oculto* elemento utilizado para almacenar información de estado de vista) dentro de *div* elemento con el fin de cumplir el estándar XHTML. Sin embargo, esto puede causar un problema cuando afecta una regla CSS *div* elementos en una página. Por ejemplo, se puede producir en una línea de un píxel que aparecen en la página alrededor oculto *div* elementos. En ASP.NET 4, *div* elementos que incluya los campos ocultos que se genera por ASP.NET agregan una referencia a clase CSS como en el ejemplo siguiente:

[!code-html[Main](overview/samples/sample74.html)]

A continuación, puede definir una clase CSS que se aplica únicamente a la *oculto* elementos que se generan por ASP.NET, como en el ejemplo siguiente:

[!code-css[Main](overview/samples/sample75.css)]

<a id="0.2__Toc253429269"></a><a id="0.2__Toc243304643"></a>

### <a name="rendering-an-outer-table-for-templated-controls"></a>Representación de una tabla externa para controles con plantilla

De forma predeterminada, los siguientes controles de servidor Web de ASP.NET que admiten las plantillas se ajustan automáticamente en una tabla externa que se usa para aplicar estilos en línea:

- *FormView*
- *Inicio de sesión*
- *PasswordRecovery*
- *ChangePassword*
- *Asistente para*
- *CreateUserWizard*

Una nueva propiedad denominada *RenderOuterTable* se agregó a estos controles que permite que la tabla externa que se quitará de la marca. Por ejemplo, considere el siguiente ejemplo de un *FormView* control:

[!code-aspx[Main](overview/samples/sample76.aspx)]

Este marcado presenta el siguiente resultado en la página, que incluye una tabla HTML:

[!code-html[Main](overview/samples/sample77.html)]

Para evitar que se represente la tabla, puede establecer la *FormView* del control *RenderOuterTable* propiedad, como en el ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample78.aspx)]

En el ejemplo anterior se presenta el siguiente resultado, sin la *tabla*, *tr*, y *td* elementos:

> Contenido


Esta mejora puede resulte más fácil estilo el contenido del control con CSS, porque inesperado etiquetas no se representan mediante el control.

> [!NOTE]
> Tenga en cuenta este cambio deshabilita la compatibilidad con la función de formato automático en el Diseñador de Visual Studio 2010, porque ya no es un *tabla* elemento que puede contener atributos de estilo que se generan mediante la opción de formato automático.


<a id="0.2__Toc253429270"></a><a id="0.2__Toc243304644"></a>

### <a name="listview-control-enhancements"></a>Mejoras del Control ListView

El *ListView* control se ha realizado más fácil de usar en ASP.NET 4. La versión anterior del control es obligatorio, que especifique una plantilla de diseño que contenía un control de servidor con un identificador conocido. El marcado siguiente muestra un ejemplo típico de cómo usar el *ListView* control en ASP.NET 3.5.

[!code-aspx[Main](overview/samples/sample79.aspx)]

En ASP.NET 4, el *ListView* control no requiere una plantilla de diseño. El marcado que se muestra en el ejemplo anterior puede reemplazarse con el marcado siguiente:

[!code-aspx[Main](overview/samples/sample80.aspx)]

<a id="0.2__Toc253429271"></a><a id="0.2__Toc243304645"></a>

### <a name="checkboxlist-and-radiobuttonlist-control-enhancements"></a>Mejoras de Control RadioButtonList y CheckBoxList

En ASP.NET 3.5, puede especificar el diseño de la *CheckBoxList* y *RadioButtonList* mediante las dos opciones siguientes:

- *Flujo de*. Representa el control *abarcan* elementos para que contenga su contenido.
- *Tabla*. El control representa un *tabla* elemento para que contenga su contenido.

En el ejemplo siguiente se muestra el marcado para cada uno de estos controles.

[!code-aspx[Main](overview/samples/sample81.aspx)]

De forma predeterminada, los controles de representan HTML similar al siguiente:

[!code-html[Main](overview/samples/sample82.html)]

Dado que estos controles contienen listas de elementos, para representar HTML semánticamente correcto, debe representar su contenido mediante la lista en HTML (*li*) elementos. Así resulta más fácil para los usuarios que leen páginas Web mediante la tecnología de ayuda y hace que los controles más fáciles de estilo mediante CSS.

En ASP.NET 4, el *CheckBoxList* y *RadioButtonList* controles admiten los siguientes valores nuevos para el *RepeatLayout* propiedad:

- *OrderedList* : el contenido se presenta como *li* elementos dentro de un *ol* elemento.
- *UnorderedList* : el contenido se presenta como *li* elementos dentro de un *ul* elemento.

En el ejemplo siguiente se muestra cómo utilizar estos nuevos valores.

[!code-aspx[Main](overview/samples/sample83.aspx)]

El marcado anterior genera el siguiente código HTML:

[!code-html[Main](overview/samples/sample84.html)]

> [!NOTE]
> Tenga en cuenta si establece *RepeatLayout* a *OrderedList* o *UnorderedList*, *RepeatDirection* ya no se puede utilizar la propiedad y le inicia una excepción en tiempo de ejecución si se ha establecido la propiedad en su código o marcado. La propiedad no tendría ningún valor porque el aspecto visual de estos controles se define utilizando CSS en su lugar.


<a id="0.2__Toc253429272"></a><a id="0.2__Toc243304646"></a>

### <a name="menu-control-improvements"></a>Mejoras de Control de menú

Antes de ASP.NET 4, el *menú* control representa una serie de tablas HTML. Esto hace más difícil aplicar estilos CSS fuera de establecer propiedades en línea y no era compatible con los estándares de accesibilidad también.

En ASP.NET 4, el control ahora representa HTML mediante marcado semántico que consta de una lista sin ordenar y elementos de lista. En el ejemplo siguiente se muestra el marcado de una página ASP.NET para la *menú* control.

[!code-aspx[Main](overview/samples/sample85.aspx)]

Cuando se representa la página, el control genera el siguiente código HTML (la *onclick* código se ha omitido para mayor claridad):

[!code-html[Main](overview/samples/sample86.html)]

Además de mejoras de representación, navegación mediante el teclado del menú se ha mejorado con administración de foco. Cuando el *menú* control recibe el foco, puede usar las teclas de dirección para navegar por los elementos. El *menú* control ahora también asocia accesible roles de aplicación (ARIA) de internet enriquecida y atributos SIG[uiente el](http://www.w3.org/TR/wai-aria-practices/#menu "directrices de menú ARIA")para mejorado accesibilidad.

Estilos para el control de menú se representan en un bloque de estilo en la parte superior de la página, en lugar de en línea con los elementos HTML representados. Si desea ejercer un control completo sobre el estilo del control, puede establecer la nueva *IncludeStyleBlock* propiedad *false*, en cuyo caso no se genera el bloque de estilo. Una manera de utilizar esta propiedad es utilizar la característica de formato automático en el Diseñador de Visual Studio para establecer el aspecto del menú. A continuación, puede ejecutar la página, abra el origen de la página y, a continuación, copie el bloque style representado en un archivo CSS externo. En Visual Studio, deshacer la aplicación de estilos y establezca *IncludeStyleBlock* a *false*. El resultado es que el aspecto del menú se define mediante los estilos de una hoja de estilos externa.

<a id="0.2__Toc253429273"></a><a id="0.2__Toc243304647"></a>

### <a name="wizard-and-createuserwizard-controls"></a>Asistente y controles de CreateUserWizard

ASP.NET *asistente* y *CreateUserWizard* controles admiten plantillas que le permiten definir el código HTML que se presentan. (*CreateUserWizard* deriva *asistente*.) En el ejemplo siguiente se muestra el marcado de una plantilla totalmente *CreateUserWizard* control:

[!code-aspx[Main](overview/samples/sample87.aspx)]

El control representa HTML similar al siguiente:

[!code-html[Main](overview/samples/sample88.html)]

En ASP.NET 3.5 SP1, aunque se puede cambiar el contenido de la plantilla, todavía tiene un control limitado sobre el resultado de la *asistente* control. En ASP.NET 4, puede crear un *LayoutTemplate* plantilla e insert *marcador de posición* controles (mediante nombres reservados) para especificar cómo desea que el *control Wizard* para representar. En el ejemplo siguiente se muestra cómo hacerlo:

[!code-aspx[Main](overview/samples/sample89.aspx)]

El ejemplo contiene los siguientes marcadores de posición en la *LayoutTemplate* elemento:

- *headerPlaceholder* : en tiempo de ejecución, ésta se reemplazará por el contenido de la *HeaderTemplate* elemento.
- *sideBarPlaceholder* : en tiempo de ejecución, ésta se reemplazará por el contenido de la *SideBarTemplate* elemento.
- *wizardStepPlaceHolder* : en tiempo de ejecución, ésta se reemplazará por el contenido de la *WizardStepTemplate* elemento.
- *navigationPlaceholder* : en tiempo de ejecución, ésta se reemplazará por las plantillas de navegación que haya definido.

El marcado en el ejemplo que usa marcadores de posición representa el código HTML siguiente (sin contenido realmente definido en las plantillas de):

[!code-html[Main](overview/samples/sample90.html)]

El código HTML solo es ahora no definido por el usuario es un *abarcan* elemento. (Prevemos que en futuras versiones, incluso la *abarcan* elemento no se representará.) Ahora Esto le ofrece un control total sobre prácticamente todo el contenido generado por el *asistente* control.

<a id="0.2_dyndata"></a><a id="0.2__Toc253429274"></a><a id="0.2__Toc243304648"></a><a id="0.2__Toc224729042"></a>

## <a name="aspnet-mvc"></a>ASP.NET MVC

ASP.NET MVC se introdujo como un marco de trabajo de complemento de ASP.NET 3.5 SP1 en marzo de 2009. Visual Studio 2010 incluye ASP.NET MVC 2, que incluye capacidades y características nuevas.

<a id="0.2__Toc253429275"></a>

### <a name="areas-support"></a>Compatibilidad de áreas

Las áreas permiten grupo controladores y vistas en secciones de una aplicación grande de forma relativa aislada de otras secciones. Cada área se puede implementar como un proyecto de MVC de ASP.NET independiente que, a continuación, hacer referencia a la aplicación principal. Esto ayuda a administrar la complejidad al compilar una aplicación grande y resulta más sencillo para varios equipos trabajen juntos en una sola aplicación.

<a id="0.2__Toc253429276"></a>

### <a name="data-annotation-attribute-validation-support"></a>Compatibilidad de validación de atributos de anotación de datos

*DataAnnotations* atributos permiten adjuntar lógica de validación a un modelo mediante el uso de atributos de metadatos. *DataAnnotations* atributos se introdujeron en datos dinámicos de ASP.NET en ASP.NET 3.5 SP1. Estos atributos se han integrado en el enlazador de modelos predeterminado y proporcionan un medio basado en metadatos para validar la entrada del usuario.

<a id="0.2__Toc253429277"></a>

### <a name="templated-helpers"></a>Aplicaciones auxiliares con plantilla

Aplicaciones auxiliares con plantilla permiten asociar automáticamente Editar y mostrar las plantillas con tipos de datos. Por ejemplo, puede usar una aplicación auxiliar de plantilla para especificar que un elemento de interfaz de usuario de selector de fecha se representa automáticamente para una *System.DateTime* valor. Esto es similar a las plantillas de campo de datos dinámicos de ASP.NET.

El *Html.EditorFor* y *Html.DisplayFor* métodos auxiliares tienen compatibilidad integrada para representar datos estándares los tipos de objetos complejos, así como con varias propiedades. También personalizan la representación por lo que le permite aplicar los atributos de anotación de datos como *DisplayName* y *ScaffoldColumn* a la *ViewModel* objeto.

Frecuencia con que desea personalizar la salida de aún más aplicaciones auxiliares de la interfaz de usuario y tener control completo sobre lo que se genera. El *Html.EditorFor* y *Html.DisplayFor* métodos auxiliares admiten el uso de un mecanismo de creación de plantillas que le permite definir plantillas externas que pueden invalidar y control el resultado representado. Las plantillas se pueden representar individualmente para una clase.

<a id="0.2__Toc253429278"></a><a id="0.2__Toc243304649"></a>

## <a name="dynamic-data"></a>Datos dinámicos

Datos dinámicos se introdujeron en la versión de .NET Framework 3.5 SP1 a mediados de 2008. Esta característica proporciona muchas mejoras para crear aplicaciones controladas por datos, incluidas las siguientes:

- Una experiencia RAD para compilar rápidamente un sitio Web orientadas a datos.
- Validación automática que se basa en las restricciones definidas en el modelo de datos.
- La capacidad de cambiar fácilmente el marcado que se genera para los campos en el *GridView* y *DetailsView* controles mediante el uso de plantillas de campo que forman parte de su proyecto de datos dinámicos.

> [!NOTE]
> Tenga en cuenta para obtener más información, consulte el [documentación de datos dinámicos](https://msdn.microsoft.com/en-us/library/cc488545.aspx) en MSDN Library.


En ASP.NET 4, los datos dinámicos se ha mejorado para proporcionar a los programadores incluso más capacidad para crear rápidamente sitios Web controlados por datos.

<a id="0.2__Toc253429279"></a><a id="0.2__Toc243304650"></a>

### <a name="enabling-dynamic-data-for-existing-projects"></a>Habilitar datos dinámicos para los proyectos existentes

Características de datos dinámicos que se incluye en .NET Framework 3.5 SP1, vuelva a nuevas características como la siguiente:

- Plantillas de campo proporcionan plantillas basadas en el tipo de datos para controles enlazados a datos. Las plantillas de campo proporcionan una forma más fácil de personalizar la apariencia de los controles de datos que el uso de campos de la plantilla para cada campo.
- Validación: dinámico de datos le permite usar atributos en las clases de datos para especificar la validación de los escenarios comunes como campos obligatorios, la comprobación de intervalos, la comprobación de tipos, mediante expresiones regulares de coincidencia de patrón y la validación personalizada. La validación se aplica por los controles de datos.

Sin embargo, estas características tenían los siguientes requisitos:

- La capa de acceso a datos se tuvieron que basarse en Entity Framework o LINQ to SQL.
- Controles compatibles con estas características estuvieran del origen de datos solo la *EntityDataSource* o *LinqDataSource* controles.
- Las características necesitan de un proyecto Web que se había creado utilizando las plantillas de entidades de datos dinámicos o los datos dinámicos para todos los archivos que se necesitan para admitir la característica.

Es un objetivo importante de la compatibilidad con datos dinámicos de ASP.NET 4 habilitar la funcionalidad nueva de datos dinámicos para cualquier aplicación de ASP.NET. En el ejemplo siguiente se muestra el marcado para los controles que puede aprovechar las ventajas de la funcionalidad de datos dinámicos en una página.

[!code-aspx[Main](overview/samples/sample91.aspx)]

En el código de la página, debe agregarse el código siguiente para habilitar la compatibilidad con datos dinámicos de estos controles:

[!code-csharp[Main](overview/samples/sample92.cs)]

Cuando el *GridView* control está en modo de edición, los datos dinámicos automáticamente se comprueba si los datos introducidos en el formato correcto. Si no es así, se muestra un mensaje de error.

Esta funcionalidad también proporciona otras ventajas, por ejemplo, se pueden especificar de forma predeterminada los valores para el modo de inserción. Sin datos dinámicos, para implementar un valor predeterminado para un campo, debe asociar a un evento, busque el control (mediante *FindControl*) y establezca su valor. En ASP.NET 4, el *EnableDynamicData* llamada es compatible con un segundo parámetro que le permite pasar valores predeterminados para cualquier campo en el objeto, como se muestra en este ejemplo:

[!code-csharp[Main](overview/samples/sample93.cs)]

<a id="0.2__Toc224729043"></a><a id="0.2__Toc253429280"></a><a id="0.2__Toc243304651"></a>

### <a name="declarative-dynamicdatamanager-control-syntax"></a>Sintaxis de Control de DynamicDataManager declarativa

El *DynamicDataManager* control se ha mejorado para que se puede configurar mediante declaración, al igual que con la mayoría de los controles de ASP.NET, en lugar de solo en el código. El marcado para la *DynamicDataManager* control es similar al ejemplo siguiente:

[!code-aspx[Main](overview/samples/sample94.aspx)]

Este marcado habilita el comportamiento de datos dinámicos para el control de GridView1 que se hace referencia en el *controles de datos* sección de la *DynamicDataManager* control.

<a id="0.2__Toc224729044"></a><a id="0.2__Toc253429281"></a><a id="0.2__Toc243304652"></a>

### <a name="entity-templates"></a>Plantillas de entidad

Las plantillas de entidad ofrecen una nueva manera de personalizar el diseño de datos sin necesidad de crear una página personalizada. Uso de plantillas de página el *FormView* control (en lugar de la *DetailsView* controlar, tal y como se utilizan en las plantillas de página en versiones anteriores de datos dinámicos) y la *DynamicEntity* control para representar las plantillas de entidad. Esto proporciona mayor control sobre el marcado que representa los datos dinámicos.

En la lista siguiente se muestra el nuevo diseño de directorio de proyecto que contiene las plantillas de entidad:

[!code-console[Main](overview/samples/sample95.cmd)]

El `EntityTemplate` directorio contiene plantillas para mostrar objetos del modelo de datos. De forma predeterminada, los objetos se representan mediante el uso de la `Default.ascx` plantilla, que proporciona el marcado que parezca como el marcado creado por el *DetailsView* control utilizado por los datos dinámicos en ASP.NET 3.5 SP1. En el ejemplo siguiente se muestra el marcado para el `Default.ascx` control:

[!code-aspx[Main](overview/samples/sample96.aspx)]

Las plantillas predeterminadas pueden modificarse para cambiar la apariencia y funcionamiento para todo el sitio. Hay plantillas para mostrar, editar y las operaciones de inserción. Nuevas plantillas pueden agregarse según el nombre del objeto de datos con el fin de cambiar la apariencia de un solo tipo de objeto. Por ejemplo, puede agregar la siguiente plantilla:

[!code-console[Main](overview/samples/sample97.cmd)]

La plantilla podría contener el siguiente marcado:

[!code-aspx[Main](overview/samples/sample98.aspx)]

Las nuevas plantillas de entidad se muestran en una página con el nuevo *DynamicEntity* control. En tiempo de ejecución, este control se reemplaza con el contenido de la plantilla de entidad. El marcado siguiente se muestra la *FormView* controlar en el `Detail.aspx` plantilla de página que usa la plantilla de entidad. Observe el *DynamicEntity* elemento en el marcado.

[!code-aspx[Main](overview/samples/sample99.aspx)]

<a id="0.2__Toc224729045"></a><a id="0.2__Toc253429282"></a><a id="0.2__Toc243304653"></a>

### <a name="new-field-templates-for-urls-and-e-mail-addresses"></a>Nuevas plantillas de campo para las direcciones URL y direcciones de correo electrónico

ASP.NET 4 presenta dos nuevas plantillas de campo integrado, `EmailAddress.ascx` y `Url.ascx`. Estas plantillas se utilizan para los campos que están marcados como *EmailAddress* o *Url* con el *DataType* atributo. Para *EmailAddress* objetos, el campo se muestra como un hipervínculo que se crea mediante la *mailto:* protocolo. Cuando los usuarios haga clic en el vínculo, se abre el cliente de correo electrónico del usuario y crea un mensaje de esqueleto. Objetos de tipo *Url* aparecen como hipervínculos normales.

En el ejemplo siguiente se muestra cómo se podrían marcar campos.

[!code-csharp[Main](overview/samples/sample100.cs)]

<a id="0.2__Toc224729046"></a><a id="0.2__Toc253429283"></a><a id="0.2__Toc243304654"></a>

### <a name="creating-links-with-the-dynamichyperlink-control"></a>Creación de vínculos con el Control DynamicHyperLink

Datos dinámicos utilizan la nueva característica de enrutamiento que se agregó en .NET Framework 3.5 SP1 para controlar las direcciones URL que los usuarios finales verán cuando tengan acceso al sitio Web. El nuevo *DynamicHyperLink* control facilita el proceso crear vínculos a páginas de un sitio de datos dinámicos. En el ejemplo siguiente se muestra cómo utilizar el *DynamicHyperLink* control:

[!code-aspx[Main](overview/samples/sample101.aspx)]

Este marcado crea un vínculo que apunta a la página de lista para la `Products` tabla basándose en las rutas que se definen en el `Global.asax` archivo. El control usa automáticamente el nombre de tabla predeterminado que se basa la página de datos dinámicos.

<a id="0.2__Toc224729047"></a><a id="0.2__Toc253429284"></a><a id="0.2__Toc243304655"></a>

### <a name="support-for-inheritance-in-the-data-model"></a>Compatibilidad con la herencia en el modelo de datos

Entity Framework y LINQ to SQL admiten la herencia en los modelos de datos. Un ejemplo de esto podría ser una base de datos que tenga un `InsurancePolicy` tabla. Es posible que también contengan `CarPolicy` y `HousePolicy` tablas que tienen los mismos campos como `InsurancePolicy` y, a continuación, agregar más campos. Se ha modificado los datos dinámicos para comprender los objetos heredados en el modelo de datos y admitir scaffolding para las tablas heredadas.

<a id="0.2__Toc224729048"></a><a id="0.2__Toc253429285"></a><a id="0.2__Toc243304656"></a>

### <a name="support-for-many-to-many-relationships-entity-framework-only"></a>Soporte técnico para las relaciones de varios a varios (solo Entity Framework)

Entity Framework tiene compatibilidad enriquecida para las relaciones de varios a varios entre tablas, que se implementa mediante la exposición de la relación como una colección en un *entidad* objeto. Nueva `ManyToMany.ascx` y `ManyToMany_Edit.ascx` plantillas de campo se agregaron para proporcionar compatibilidad para mostrar y editar datos que intervienen en las relaciones de varios a varios.

<a id="0.2__Toc224729049"></a><a id="0.2__Toc253429286"></a><a id="0.2__Toc243304657"></a>

### <a name="new-attributes-to-control-display-and-support-enumerations"></a>Nuevos atributos para controlar la presentación y admitir enumeraciones

El *DisplayAttribute* se ha agregado para proporcionarle control adicional sobre cómo se muestran los campos. El *DisplayName* atributo en versiones anteriores de datos dinámicos que permite cambiar el nombre que se utiliza como un título para un campo. El nuevo *DisplayAttribute* clase le permite especificar más opciones para mostrar un campo, como el orden en que se muestra un campo y si un campo se utilizará como filtro. El atributo también proporciona control independiente del nombre que se utiliza para las etiquetas en un *GridView* controlar, el nombre utilizado en un *DetailsView* controlan, el texto de ayuda para el campo, y la marca de agua se usa para el campo (si el campo acepta la entrada de texto).

El *EnumDataTypeAttribute* se ha agregado la clase para poder asignar campos a las enumeraciones. Cuando se aplica este atributo a un campo, especifique un tipo de enumeración. Los datos dinámicos utilizan el nuevo `Enumeration.ascx` plantilla de campo para crear la interfaz de usuario para mostrar y editar los valores de enumeración. La plantilla asigna los valores de la base de datos a los nombres de la enumeración.

<a id="0.2__Toc224729050"></a><a id="0.2__Toc253429287"></a><a id="0.2__Toc243304658"></a>

### <a name="enhanced-support-for-filters"></a>Compatibilidad mejorada para filtros

Dinámico de datos de 1.0 se incluyó con filtros integrados para columnas booleanas y columnas de clave externa. Los filtros no permitió especificar si éstas se muestran o el orden en que se muestran. El nuevo *DisplayAttribute* atributo direcciones ambos de estos problemas, por lo que le proporciona controlan sobre si una columna se muestra como un filtro y en qué orden se mostrará.

Una mejora adicional es que el filtrado de soporte técnico ha sido[escribir para que usen la nueva](#0.2__QueryExtender "_QueryExtender") característica de formularios Web Forms. Esto le permite crear filtros sin necesidad de conocimiento del control de origen de datos que se usará con los filtros. Además de estas extensiones, los filtros se han convertido también en controles de plantilla, que le permite agregar nuevos. Por último, el *DisplayAttribute* clase se ha mencionado anteriormente permite que el filtro predeterminado para invalidarse en la misma forma en que *UIHint* permite que la plantilla de campo predeterminada para una columna para que se invalide.

<a id="0.2__Toc224729051"></a><a id="0.2__Toc253429288"></a><a id="0.2__Toc243304659"></a>

## <a name="visual-studio-2010-web-development-improvements"></a>Mejoras de desarrollo Web de Visual Studio 2010

Se ha mejorado el desarrollo Web en Visual Studio 2010 para una mayor compatibilidad CSS, aumentar la productividad a través de fragmentos de código de marcado HTML y ASP.NET y nuevo IntelliSense JavaScript dinámica.

<a id="0.2__Toc224729052"></a><a id="0.2__Toc253429289"></a><a id="0.2__Toc243304660"></a>

### <a name="improved-css-compatibility"></a>Compatibilidad de CSS mejorada

El Diseñador de Visual Web Developer en Visual Studio 2010 se actualizó para mejorar el cumplimiento de estándares de CSS 2.1. El diseñador mejor conserva la integridad de la fuente HTML y es más sólido que en versiones anteriores de Visual Studio. En segundo plano, mejoras arquitectónicas también se introdujeron que mejor habilitar futuras mejoras en la representación y diseño, capacidad de servicio.

<a id="0.2__Toc224729053"></a><a id="0.2__Toc253429290"></a><a id="0.2__Toc243304661"></a>

### <a name="html-and-javascript-snippets"></a>HTML y fragmentos de código de JavaScript

En el editor HTML, IntelliSense completa automáticamente los nombres de etiqueta. La característica de fragmentos de código de IntelliSense autocompleta etiquetas completas y mucho más. En Visual Studio 2010, se admiten fragmentos de código de IntelliSense para JavaScript, junto con C# y Visual Basic, que se admitían en versiones anteriores de Visual Studio.

Visual Studio 2010 incluye más de 200 fragmentos de código que le ayudarán a Autocompletar ASP.NET y el código HTML etiquetas comunes, incluidos los atributos necesarios (como runat = "server") y los atributos comunes específicos a una etiqueta (como *identificador*,  *DataSourceID*, *ControlToValidate*, y *texto*).

Puede descargar los fragmentos de código adicionales o puede escribir sus propios fragmentos de código que encapsulan los bloques de marcado que usted o su equipo se usan para realizar tareas comunes.

<a id="0.2__Toc224729054"></a><a id="0.2__Toc253429291"></a><a id="0.2__Toc243304662"></a>

### <a name="javascript-intellisense-enhancements"></a>Mejoras de IntelliSense para JavaScript

En Visual 2010, se ha rediseñado IntelliSense para JavaScript para proporcionar una experiencia de edición aún más sofisticada. Ahora, IntelliSense reconoce los objetos que han sido dinámicamente generados por métodos como *registerNamespace* y por técnicas similares usadas por otros marcos de JavaScript. Se ha mejorado el rendimiento para analizar grandes bibliotecas de scripts y para mostrar IntelliSense con poco o ningún retraso de procesamiento. Se ha aumentado considerablemente la compatibilidad para admitir casi todas las bibliotecas de terceros y para admitir diversos estilos de codificación. Ahora se analizan los comentarios de documentación a medida que escriba y se aprovecha inmediatamente por IntelliSense.

<a id="0.2__Toc224729055"></a><a id="0.2__Toc253429292"></a><a id="0.2__Toc243304663"></a>

## <a name="web-application-deployment-with-visual-studio-2010"></a>Implementación de aplicaciones Web con Visual Studio 2010

Cuando los programadores de ASP.NET implementación una aplicación Web, estos clientes encuentran a menudo que encuentren problemas como el siguiente:

- La implementación en un sitio de hospedaje compartido requiere tecnologías, como FTP, lo que puede resultar lento. Además, debe realizar manualmente las tareas como la ejecución de secuencias de comandos SQL para configurar una base de datos y debe cambiar la configuración de IIS, como cuando se configura una carpeta del directorio virtual como una aplicación.
- En un entorno empresarial, además de implementar los archivos de aplicación Web, los administradores deben modificar con frecuencia los archivos de configuración de ASP.NET y la configuración de IIS. Los administradores de base de datos deben ejecutar una serie de secuencias de comandos SQL para obtener la base de datos de aplicación que se ejecuta. Estas instalaciones laborioso, suele tardar horas en completarse y debe documentarse con cuidado.

Visual Studio 2010 incluye tecnologías que abordar estos problemas y que le permiten implementar fácilmente aplicaciones Web. Una de estas tecnologías es la herramienta de implementación Web de IIS (MsDeploy.exe).

Características de implementación Web en Visual Studio 2010, incluyen las siguientes áreas principales:

- Empaquetado de Web
- Transformación de Web.config
- Implementación de base de datos
- Publicación de un solo clic para las aplicaciones Web

Las secciones siguientes proporcionan detalles acerca de estas características.

<a id="0.2__Toc224729056"></a><a id="0.2__Toc253429293"></a><a id="0.2__Toc243304664"></a>

### <a name="web-packaging"></a>Empaquetado de Web

Visual Studio 2010, se usa la herramienta MSDeploy para crear un archivo comprimido (.zip) para la aplicación, que se conoce como un *paquete Web*. El archivo de paquete contiene metadatos acerca de la aplicación junto con el siguiente contenido:

- Configuración de IIS, que incluye la configuración del grupo de aplicaciones, configuración de la página de error y así sucesivamente.
- El contenido Web real, que incluye páginas Web, controles de usuario, contenido estático (imágenes y archivos HTML) y así sucesivamente.
- Esquemas de base de datos de SQL Server y los datos.
- Certificados de seguridad, componentes que desea instalar en la GAC, configuración del registro y así sucesivamente.

Un paquete de Web se puede copiar en cualquier servidor y, a continuación, instala manualmente mediante el Administrador de IIS. O bien, para la implementación automatizada, el paquete se puede instalar mediante comandos de línea de comandos o mediante las API de implementación.

Visual Studio 2010 proporciona tareas de MSBuild y destinos para crear paquetes de Web integrados. Para obtener más información, consulte [introducción de implementación de proyecto de aplicación de ASP.NET Web](https://msdn.microsoft.com/en-us/library/dd394698%28VS.100%29.aspx) en el sitio Web de MSDN y [10 + 20 motivos ¿por qué se debe crear un paquete de Web](http://vishaljoshi.blogspot.com/2009/07/10-20-reasons-why-you-should-create-web.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729057"></a><a id="0.2__Toc253429294"></a><a id="0.2__Toc243304665"></a>

### <a name="webconfig-transformation"></a>Transformación de Web.config

Para la implementación de aplicación Web, Visual Studio 2010 presenta [transformar de documento XML (XDT)](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html), que es una característica que permite transformar una `Web.config` archivo de configuración de desarrollo a la configuración de producción. Configuración de transformación se especifica en archivos de transformación denominados `web.debug.config`, `web.release.config`, y así sucesivamente. (Los nombres de estos archivos coinciden con las configuraciones de MSBuild.) Un archivo de transformación incluye solo los cambios que necesite realizar en un implementado `Web.config` archivo. Especificar los cambios mediante una sintaxis sencilla.

En el ejemplo siguiente se muestra una parte de un `web.release.config` archivo que se pueden producir para la implementación de la configuración de lanzamiento. La palabra clave de reemplazo en el ejemplo especifica que durante la implementación de la *connectionString* nodo en el `Web.config` archivo se reemplazará con los valores que se muestran en el ejemplo.

[!code-xml[Main](overview/samples/sample102.xml)]

Para obtener más información, consulte [sintaxis de transformación de Web.config para la implementación de proyecto de aplicación Web](https://msdn.microsoft.com/en-us/library/dd465326%28VS.100%29.aspx) en MSDN <a id="0.2_a"> </a> sitio Web y[implementación Web: transformación de Web.Config](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html)en el blog de Vishal Joshi.

<a id="0.2__Toc224729058"></a><a id="0.2__Toc253429295"></a><a id="0.2__Toc243304666"></a>

### <a name="database-deployment"></a>Implementación de base de datos

Un paquete de implementación de Visual Studio 2010 puede incluir las dependencias de las bases de datos de SQL Server. Como parte de la definición del paquete, puede proporcionar la cadena de conexión para la base de datos de origen. Cuando se crea el paquete Web, Visual Studio 2010 crea scripts SQL para el esquema de base de datos y, opcionalmente, para los datos y, a continuación, agrega al paquete. También puede proporcionar scripts SQL personalizados y especificar la secuencia en la que se deben ejecutar en el servidor. Durante la implementación, proporcionar una cadena de conexión que sea adecuada para el servidor de destino; el proceso de implementación, a continuación, utiliza esta cadena de conexión para ejecutar los scripts que crean el esquema de base de datos y agregan los datos.

Además, mediante el uso de un solo clic publicar, puede configurar la implementación para publicar la base de datos directamente cuando la aplicación se publica en un sitio de hospedaje compartido remoto. Para obtener más información, consulte [Cómo: implementar una base de datos con un proyecto de aplicación Web](https://msdn.microsoft.com/en-us/library/dd465343%28VS.100%29.aspx) en el sitio Web de MSDN y [implementación de base de datos con VS 2010](http://vishaljoshi.blogspot.com/2009/03/web-deployment-webconfig-transformation_23.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729059"></a><a id="0.2__Toc253429296"></a><a id="0.2__Toc243304667"></a>

### <a name="one-click-publish-for-web-applications"></a>Publicación de un solo clic para las aplicaciones Web

Visual Studio 2010 también permite utilizar el servicio de administración remota de IIS para publicar una aplicación Web en un servidor remoto. Puede crear un perfil de publicación para la cuenta de hospedaje o para probar servidores o servidores de ensayo. Cada perfil puede guardar las credenciales adecuadas de forma segura. A continuación, puede implementar en cualquier del destino de barra de herramientas de publicación de servidores con un solo clic mediante el uso de la Web con un solo clic. Con Visual Studio 2010, también puede publicar mediante la línea de comandos de MSBuild. Esto le permite configurar el entorno de compilación de equipo para incluir la publicación en un modelo de integración continua.

Para obtener más información, consulte [Cómo: implementar una aplicación utilizando un solo clic publicación de proyecto Web y Web Deploy](https://msdn.microsoft.com/en-us/library/dd465337%28VS.100%29.aspx) en el sitio Web de MSDN y [Web 1 y haga clic en Publicar con VS 2010](http://vishaljoshi.blogspot.com/2009/05/web-1-click-publish-with-vs-2010.html) en el blog de Vishal Joshi. Para ver presentaciones en vídeo sobre la implementación de aplicación Web en Visual Studio 2010, vea [VS 2010 para vistas previas de desarrollador Web](http://vishaljoshi.blogspot.com/2008/12/vs-2010-for-web-developer-previews.html) en el blog de Vishal Joshi.

<a id="0.2__Toc224729060"></a><a id="0.2__Toc253429297"></a><a id="0.2__Toc243304668"></a>

### <a name="resources"></a>Recursos

Los siguientes sitios Web proporcionan información adicional acerca de ASP.NET 4 y Visual Studio 2010.

- [ASP.NET 4](https://msdn.microsoft.com/en-us/library/ee532866%28VS.100%29.aspx) : la documentación oficial para ASP.NET 4 en el sitio Web de MSDN.
- [https://www.ASP.NET/](https://www.asp.net/) : ASP.NET el sitio Web del equipo.
- [https://www.ASP.NET/DynamicData/](https://msdn.microsoft.com/en-us/library/cc488545.aspx) y [ASP.NET Dynamic Data Content Map](https://msdn.microsoft.com/en-us/library/cc488545%28VS.100%29.aspx) : recursos en línea en el sitio de equipo ASP.NET y en la documentación oficial de datos dinámicos de ASP.NET.
- [https://www.ASP.NET/AJAX/](../../ajax/index.md) : el recurso Web principal para el desarrollo de Ajax de ASP.NET.
- [https://blogs.msdn.com/webdevtools/](https://blogs.msdn.com/webdevtools/) : The Visual Web Developer Team blog, que incluye información acerca de las características de Visual Studio 2010.
- [ASP.NET WebStack](https://github.com/aspnet/AspNetWebStack) : el recurso de Web principal de versiones preliminares de ASP.NET.

<a id="0.2__Toc224729061"></a><a id="0.2__Toc253429298"></a><a id="0.2__Toc243304669"></a>

## <a name="disclaimer"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, los nombres de las compañías, organizaciones, productos, dominios, direcciones de correo electrónico, logotipos, personas, lugares y hechos mencionados son ficticios. No se pretende indicar, ni debe deducirse ninguna asociación con ninguna compañía, organización, producto, dominio, dirección de correo electrónico, logotipo, persona, lugar o hecho reales.

© 2009 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
