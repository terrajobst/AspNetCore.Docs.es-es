---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Descripción de servicios Web ASP.NET AJAX | Documentos de Microsoft
author: scottcate
description: Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque Web...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-web-services"></a>Descripción de servicios Web de AJAX de ASP.NET
====================
por [Scott ndicar](https://github.com/scottcate)

[Descarga de PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque los servicios Web se usan normalmente para permitir que distintos sistemas operativos, modelos de objetos y los lenguajes de programación para enviar y recibir datos, también puede utilizar para insertar datos en una página AJAX de ASP.NET o enviar datos de una página a un sistema back-end dinámicamente. Todo esto puede realizarse sin tener que recurrir para las operaciones de devolución de datos.


## <a name="calling-web-services-with-aspnet-ajax"></a>Llamar a los servicios Web con AJAX de ASP.NET

Dan Wahlin

Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque los servicios Web se usan normalmente para permitir que distintos sistemas operativos, modelos de objetos y los lenguajes de programación para enviar y recibir datos, también puede utilizar para insertar datos en una página AJAX de ASP.NET o enviar datos de una página a un sistema back-end dinámicamente. Todo esto puede realizarse sin tener que recurrir para las operaciones de devolución de datos.

Mientras que el control ASP.NET AJAX UpdatePanel proporciona una manera sencilla de AJAX habilitar cualquier página ASP.NET, puede haber ocasiones cuando necesite tener acceso dinámicamente a datos en el servidor sin usar un UpdatePanel. En este artículo verá cómo hacerlo mediante la creación y consumir servicios Web en las páginas de AJAX de ASP.NET.

En este artículo se centrará en la funcionalidad disponible en las principales extensiones de AJAX de ASP.NET, así como un control habilitado el servicio Web en el Kit de herramientas de AJAX de ASP.NET llama a la AutoCompleteExtender. Los temas tratados incluyen definir los servicios Web con AJAX habilitado, crear servidores proxy de cliente y llamar a servicios Web con JavaScript. También verá cómo se pueden realizar llamadas a servicios Web directamente a los métodos de página ASP.NET.

## <a name="web-services-configuration"></a>Configuración de servicios Web

Cuando se crea un nuevo proyecto de sitio Web con Visual Studio 2008, el archivo web.config tiene un número de nuevas adiciones que puede ser poco familiar para los usuarios de versiones anteriores de Visual Studio. Algunas de estas modificaciones asignan el prefijo "asp" a los controles de AJAX de ASP.NET para que se puedan utilizar en las páginas mientras que otros usuarios definen necesario HttpHandlers y HttpModules. Las modificaciones realizadas en el listado 1 muestra la `<httpHandlers>` elemento en el archivo web.config que afecta a las llamadas de servicio Web. El valor predeterminado que HttpHandler usado para procesar las llamadas de .asmx es eliminado y reemplazado por una clase ScriptHandlerFactory ubicada en el ensamblado System.Web.Extensions.dll. System.Web.Extensions.dll contiene toda la funcionalidad básica que se usa AJAX de ASP.NET.

**Lista 1. Configuración de controlador de servicio Web de ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Este reemplazo HttpHandler se realiza con el fin de permitir que las llamadas de JavaScript Object Notation (JSON) que se realizan desde las páginas de AJAX de ASP.NET en los servicios Web .NET utilizando a un proxy de servicio Web de JavaScript. AJAX de ASP.NET envía mensajes JSON a los servicios Web en lugar de las llamadas de Protocolo Simple de acceso a objetos (SOAP) estándar normalmente asociadas con los servicios Web. Esto hace más pequeño mensajes de solicitud y respuesta generales. También permite un procesamiento más eficaz de los datos de cliente desde la biblioteca de JavaScript de AJAX de ASP.NET está optimizada para trabajar con objetos JSON. Enumerar 2 y 3 de lista muestran ejemplos de mensajes de solicitud y respuesta de servicio Web serializados en formato JSON. El mensaje de solicitud que se muestra en el listado 2 pasa un parámetro de país con un valor de "Bélgica" mientras que el mensaje de respuesta en el listado 3 pasa una matriz de objetos de cliente y sus propiedades asociadas.

**La lista 2. Mensaje de solicitud de servicio Web serializada a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] el nombre de la operación se define como parte de la dirección URL del servicio web; Además, los mensajes de solicitud no se envían siempre a través de JSON. Servicios Web pueden utilizar el atributo ScriptMethod con el parámetro de UseHttpGet establecido en true, lo que hace que los parámetros que se pasan a través de un los parámetros de cadena de consulta.*


**El listado 3. Mensaje de respuesta del servicio Web serializada a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

En la siguiente sección verá cómo crear servicios Web capaz de controlar los mensajes de solicitud JSON y responde con tipos simples y complejos.

## <a name="creating-ajax-enabled-web-services"></a>Creación de servicios Web con AJAX habilitado

El marco de AJAX de ASP.NET proporciona varias maneras de llamar a servicios Web. Puede usar el control de AutoCompleteExtender (disponible en el Kit de herramientas de AJAX de ASP.NET) o JavaScript. Sin embargo, antes de llamar a un servicio tiene habilitar AJAX para, por lo que se puede llamar mediante código de script de cliente.

Si no está familiarizado con los servicios Web ASP.NET, encontrará que fáciles de crear y habilitar AJAX servicios. .NET framework admite la creación de servicios Web ASP.NET desde su lanzamiento inicial en 2002 y las extensiones de AJAX de ASP.NET proporcionan una funcionalidad AJAX adicional que se basa en el conjunto predeterminado de .NET framework de características. 2008 Beta 2 de Visual Studio .NET tiene compatibilidad integrada para crear archivos del servicio Web de ASMX y deriva automáticamente código asociado al lado de las clases de la clase System.Web.Services.WebService. A medida que agregue métodos en la clase que se debe aplicar el atributo WebMethod puedan ser llamado por los consumidores del servicio Web.

Listado 4 muestra un ejemplo de aplicar el atributo WebMethod a un método denominado GetCustomersByCountry().

**Listado 4. Utilizar el atributo WebMethod en un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

El método GetCustomersByCountry() acepta un parámetro de país y devuelve a un cliente matriz de objetos. El valor de país pasado al método se reenvía a una clase de la capa de negocio que a su vez llama a una clase de la capa de datos para recuperar los datos de la base de datos, rellene las propiedades del objeto de cliente con datos y devolver la matriz.

## <a name="using-the-scriptservice-attribute"></a>Con el atributo ScriptService

Al agregar un atributo permite que el método GetCustomersByCountry() ser llamado por los clientes que envían mensajes estándares de SOAP al servicio Web WebMethod, que no permite llamadas JSON a realizarse desde las aplicaciones de AJAX de ASP.NET de fábrica. Se permiten llamadas JSON a realizarse tenga que aplicar la extensión de AJAX de ASP.NET `ScriptService` atributo a la clase de servicio Web. Esto permite que un servicio Web para enviar mensajes de respuesta con formato mediante JSON y permite el script del lado cliente llamar a un servicio mediante el envío de mensajes JSON.

Listado 5 muestra un ejemplo de aplicar el atributo ScriptService a una clase de servicio Web denominada CustomersService.

**Listado 5. Con el atributo ScriptService AJAX-habilitar un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

El atributo ScriptService actúa como un marcador que indica que se puede llamar desde código de script de AJAX. Realmente no controla cualquiera de las tareas de serialización o deserialización de JSON que tienen lugar en segundo plano. El ScriptHandlerFactory (configurado en el archivo web.config) y otras clases relacionadas realizan la mayor parte del procesamiento de JSON.

## <a name="using-the-scriptmethod-attribute"></a>Con el atributo ScriptMethod

El atributo ScriptService es el único atributo de AJAX de ASP.NET que debe definirse en un servicio Web de .NET en orden para lo que va a usar las páginas de AJAX de ASP.NET. Sin embargo, también puede aplicarse otro atributo denominado ScriptMethod directamente a los métodos Web en un servicio. ScriptMethod define tres propiedades incluidos `UseHttpGet`, `ResponseFormat` y `XmlSerializeString`. Cambiar los valores de estas propiedades puede ser útil en casos donde el tipo de solicitud aceptada por un método Web debe cambiarse a GET, cuando se necesita un método Web devolver datos XML sin formato en forma de un `XmlDocument` o `XmlElement` objeto o cuando los datos se devuelven desde una  servicio siempre se debe serializar como XML en lugar de JSON.

La propiedad se puede utilizar cuando un método Web debe aceptar de UseHttpGet obtener solicitudes frente a las solicitudes POST. Las solicitudes se envían mediante una dirección URL con parámetros de entrada de método Web convierte en parámetros de cadena de consulta. El UseHttpGet propiedad valor predeterminado es false y solo debe establecerse en `true` cuando las operaciones son conocidas son seguros y cuando los datos confidenciales no se pasan a un servicio Web. Listado 6 muestra un ejemplo de cómo usar el atributo ScriptMethod con la propiedad UseHttpGet.

**Listado 6. Usar el atributo ScriptMethod con la propiedad UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Se muestra un ejemplo de los encabezados que se envían cuando se llama al método de Web HttpGetEcho se muestra en el listado 6 siguiente:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Además de permitir que los métodos Web Aceptar las solicitudes HTTP GET, el atributo ScriptMethod también se puede usar cuando necesitan respuestas en XML que se devuelve desde un servicio en lugar de JSON. Por ejemplo, un servicio Web puede recuperar una fuente RSS de un sitio remoto y devuélvalo como un objeto XmlDocument o XmlElement. Procesamiento de XML con datos, a continuación, pueden producirse en el cliente.

Listado 7 muestra un ejemplo del uso de la propiedad ResponseFormat para especificar que se deben devolver datos XML desde un método Web.

**Listado 7. Usar el atributo ScriptMethod con la propiedad ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

La propiedad ResponseFormat también puede usarse junto con la propiedad XmlSerializeString. La propiedad XmlSerializeString tiene un valor predeterminado de false, lo que significa que todas devuelven tipos excepto cadenas devueltas desde un método Web se serializan como XML cuando la `ResponseFormat` propiedad está establecida en `ResponseFormat.Xml`. Cuando `XmlSerializeString` se establece en `true`, todos los tipos devueltos de un método Web se serializan como XML, incluidos los tipos de cadena. Si la propiedad ResponseFormat tiene un valor de `ResponseFormat.Json` se omite la propiedad XmlSerializeString.

Listado 8 muestra un ejemplo del uso de la propiedad XmlSerializeString para forzar las cadenas serializarse como XML.

**Enumerar 8. Usar el atributo ScriptMethod con la propiedad XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

El valor devuelto de una llamada al método de Web GetXmlString se muestra en el listado 8 se muestra la siguiente:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Aunque el formato JSON predeterminado reduce el tamaño total de mensajes de solicitud y respuesta y más fácil es utilizado por los clientes de AJAX de ASP.NET en forma de varios exploradores, las propiedades ResponseFormat y XmlSerializeString pueden ser usada al cliente aplicaciones como Internet Explorer 5 o superior esperan que los datos XML que se devuelve desde un método Web.

## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

Listado 5 se ha mostrado un ejemplo de cómo devolver un tipo complejo denominado a cliente desde un servicio Web. La clase Customer define varios tipos simples internamente como propiedades, como FirstName y LastName. Tipos complejos que se utilizan como parámetro de entrada o tipo de valor devuelto de un método de Web con AJAX habilitado automáticamente se serializan en JSON antes de enviarse al lado cliente. Sin embargo, los tipos complejos anidados (aquellas definido internamente dentro de otro tipo) no están disponibles para el cliente como objetos independientes de forma predeterminada.

En casos donde un tipo complejo anidado usado por un servicio Web también debe utilizarse en una página de cliente, el atributo GenerateScriptType de AJAX de ASP.NET puede agregarse al servicio Web. Por ejemplo, la clase CustomerDetails se muestra en el listado 9 contiene propiedades de dirección y el sexo que ***representan tipos complejos anidados.***

**Listado 9. La clase CustomerDetails que se muestra a continuación contiene dos tipos complejos anidados.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Los objetos de dirección y el sexo definidos dentro de la clase CustomerDetails se muestra en el listado 9 no automáticamente como disponibles para su uso en el cliente a través de JavaScript ya que son los tipos anidados (dirección es una clase y género es una enumeración). En situaciones donde un tipo anidado que se utiliza dentro de un servicio Web debe estar disponible en el lado del cliente, se puede utilizar el atributo GenerateScriptType se ha mencionado anteriormente (consulte la lista de 10). Este atributo se puede agregar varias veces en casos donde se devuelven diferentes tipos complejos anidados de un servicio. Se puede aplicar directamente a la clase de servicio Web o por encima de los métodos Web específicos.

**Lista de 10. Utilizar el atributo GenerateScriptService para definir los tipos anidados que deben estar disponibles para el cliente.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Aplicando el `GenerateScriptType` atributo a los tipos de servicio Web, la dirección y el género estarán automáticamente disponible para su uso por código de JavaScript de AJAX de ASP.NET en el cliente. Se muestra un ejemplo de JavaScript que se genera automáticamente y se envía al cliente si se agrega el atributo GenerateScriptType en un servicio Web del listado 11. Verá cómo usar tipos complejos anidados más adelante en el artículo.

**Listado 11. Tipos complejos anidados ponerse a disposición de una página AJAX de ASP.NET.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ahora que ha visto cómo crear servicios Web y hágalos accesibles a las páginas de ASP.NET AJAX, echemos un vistazo a cómo crear y utilizar a servidores proxy de JavaScript para que los datos se pueden recuperar o enviarse a los servicios Web.

## <a name="creating-javascript-proxies"></a>Crear a servidores proxy de JavaScript

Llamar a un servicio Web estándar (.NET u otra plataforma) normalmente implica la creación de un objeto proxy que intercepta las complejidades de envío de mensajes de solicitud y respuesta SOAP. Con las llamadas de servicio Web de AJAX de ASP.NET, puede crear servidores proxy de JavaScript y permiten llamar fácilmente a los servicios sin preocuparse de serializar y deserializar los mensajes JSON. Servidores proxy de JavaScript se puede generar automáticamente mediante el control ScriptManager de ASP.NET AJAX.

Crear a un proxy de JavaScript que puede llamar a servicios Web se realiza utilizando la propiedad de los servicios de ScriptManager. Esta propiedad le permite definir uno o varios servicios que una página AJAX de ASP.NET puede llamar de forma asincrónica para enviar o recibir datos sin necesidad de realizar operaciones de devolución de datos. Definir un servicio mediante ASP.NET AJAX `ServiceReference` control y la asignación de la dirección URL del servicio Web para el control `Path` propiedad. Enumerar 12 muestra un ejemplo de hacer referencia a un servicio denominado CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Enumerar 12. Definir un servicio Web utilizado en una página AJAX de ASP.NET.**

Agregar una referencia a la CustomersService.asmx a través del control ScriptManager hace que un proxy de JavaScript se generan dinámicamente y que se hace referencia a la página. El proxy está incrustado utilizando el &lt;script&gt; etiqueta y se cargan dinámicamente al llamar al archivo CustomersService.asmx y anexar /js al final de la misma. En el ejemplo siguiente se muestra cómo el proxy de JavaScript se incrusta en la página cuando se deshabilita la depuración en el archivo web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Si desea ver el código de proxy de JavaScript real que se genera puede escribir la dirección URL del servicio Web de .NET deseado en el cuadro de dirección de Internet Explorer y anexar /js al final de la misma.*


Si está habilitada la depuración en el archivo web.config que una versión de depuración del proxy de JavaScript se incrustarán en la página como se muestra a continuación:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

El proxy de JavaScript creado por el objeto ScriptManager también pueden incrustarse directamente en la página en lugar de hacer referencia a mediante el &lt;script&gt; atributo src de la etiqueta. Puede hacerlo estableciendo la ServiceReference propiedad del control InlineScript en true (el valor predeterminado es false). Esto puede ser útil cuando un proxy no se comparte entre varias páginas y cuando se desea reducir el número de llamadas de red realizadas al servidor. Cuando InlineScript se establece en true la secuencia de comandos de proxy no se almacena en caché del explorador, se recomienda el valor predeterminado de false en casos donde se utiliza el proxy por varias páginas en una aplicación AJAX de ASP.NET. Se muestra un ejemplo del uso de la propiedad InlineScript siguiente:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Usar a servidores proxy de JavaScript

Una vez que una página de AJAX de ASP.NET mediante el control ScriptManager hace referencia a un servicio Web, se puede realizar una llamada al servicio Web y los datos devueltos se pueden controlar mediante funciones de devolución de llamada. Hacer referencia a su espacio de nombres (si existe), el nombre de clase y el nombre del método Web llama a un servicio Web. Se pueden definir los parámetros que se pasan al servicio Web junto con una función de devolución de llamada que controla los datos devueltos.

Se muestra un ejemplo del uso de un proxy de JavaScript para llamar a un método Web denominado GetCustomersByCountry() en el listado 13. La función GetCustomersByCountry() se invoca cuando un usuario final hace clic en un botón en la página.

**Lista de 13. Llamar a un servicio Web con un proxy de JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

El espacio de nombres InterfaceTraining hace referencia a esta llamada, CustomersService clase y método de Web GetCustomersByCountry definen en el servicio. Pasa un valor de país obtenido de un cuadro de texto, así como una función de devolución de llamada denominada OnWSRequestComplete que se debe invocar cuando finaliza la llamada asincrónica del servicio Web. OnWSRequestComplete controla la matriz de objetos de cliente devuelto desde el servicio y los convierte en una tabla que se muestra en la página. El resultado generado a partir de la llamada se muestra en la figura 1.


[![Enlace los datos obtenidos mediante la realización de una llamada AJAX asincrónica a un servicio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: enlace de datos obtenido mediante la realización de una llamada AJAX asincrónica a un servicio Web.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-web-services/_static/image3.png))


Servidores proxy de JavaScript también puede realizar las llamadas unidireccionales a los servicios Web en los casos en que debe llamarse a un método Web pero el proxy no debe esperar una respuesta. Por ejemplo, puede que desee llamar a un servicio Web para iniciar un proceso como un flujo de trabajo, pero no se espera un valor devuelto desde el servicio. En casos donde una llamada unidireccional debe convertirse a un servicio, simplemente se puede omitir la función de devolución de llamada que se muestra en el listado 13. Puesto que no se define ninguna función de devolución de llamada no esperará el objeto proxy para que el servicio Web devolver datos.

## <a name="handling-errors"></a>Control de errores

Devoluciones de llamadas asincrónicas a los servicios Web pueden encontrarse con distintos tipos de errores, como la red esté caído, el servicio Web no estaba disponible o una excepción que se devuelven. Afortunadamente, los objetos de proxy de JavaScript generados por el objeto ScriptManager permiten varias devoluciones de llamada esté definida para controlar errores y errores, además de la devolución de llamada correcta mostrado anteriormente. Puede definirse una función de devolución de llamada de error inmediatamente después de la función de devolución de llamada estándar en la llamada al método Web tal y como se muestra en el listado 14.

**Enumerar 14. Definir una función de devolución de llamada de error y mostrar los errores.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Los errores que se producen cuando se llama al servicio Web desencadenarán la función de devolución de llamada OnWSRequestFailed() para llamar que acepta un objeto que representa el error como un parámetro. El objeto de error expone varias funciones diferentes para determinar la causa del error, así como si la llamada agotó el tiempo. El listado 14 muestra un ejemplo del uso de las funciones de error diferentes y la figura 2 muestra un ejemplo de la salida generada por las funciones.


[![Salida generada al llamar a funciones de error de AJAX de ASP.NET.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: salida generada al llamar a funciones de error de AJAX de ASP.NET.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Control de datos XML devueltos desde un servicio Web

Anteriormente se mostró cómo un método Web puede devolver datos XML sin formato mediante el atributo ScriptMethod junto con su propiedad ResponseFormat. Cuando ResponseFormat se establece en ResponseFormat.Xml, los datos devueltos desde el servicio Web se serializan como XML en lugar de JSON. Esto puede ser útil cuando necesitan datos XML que se pasarán directamente al cliente para procesamiento usando JavaScript o XSLT. En el momento actual, Internet Explorer 5 o superior proporciona el mejor modelo de objetos del lado cliente para su análisis y filtrar los datos XML debido a su compatibilidad integrada para MSXML.

Recuperar datos XML de un servicio Web no es diferente a otros tipos de datos de la recuperación. Iniciar mediante la invocación del proxy de JavaScript para llamar a la función adecuada y definir una función de devolución de llamada. Una vez que se devuelve la llamada, a continuación, puede procesar los datos de la función de devolución de llamada.

Listado 15, muestra un ejemplo de una llamada a un método Web denominado GetRssFeed() que devuelve un objeto XmlElement. GetRssFeed() acepta un parámetro único que representa la dirección URL de la fuente RSS para recuperar.

**Lista de 15. Trabajar con datos XML devueltos desde un servicio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

En este ejemplo se pasa una dirección URL a una fuente RSS y procesa los datos XML devueltos en la función OnWSRequestComplete(). OnWSRequestComplete() comprueba primero si el explorador es Internet Explorer para saber si el analizador MSXML está disponible o no. Si es así, se utiliza una instrucción XPath para buscar todos los &lt;elemento&gt; etiquetas dentro de la fuente RSS. A continuación, se recorre en iteración cada elemento y asociado &lt;título&gt; y &lt;vínculo&gt; etiquetas se encuentra y se procesan para mostrar los datos de cada elemento. Figura 3 muestra un ejemplo de la salida generada por realizar una AJAX de ASP.NET llamar a través de un proxy de JavaScript para el método de Web GetRssFeed().

## <a name="handling-complex-types"></a>Control de tipos complejos

Tipos complejos aceptan o devueltos por un servicio Web automáticamente se exponen a través de un proxy de JavaScript. Sin embargo, los tipos complejos anidados no son accesibles directamente en el cliente a menos que se aplica el atributo GenerateScriptType al servicio como se indicó anteriormente. ¿Por qué se desea usar un tipo complejo anidado en el lado del cliente?

Para responder a esta pregunta, suponga que una página AJAX de ASP.NET muestra los datos del cliente y permite a los usuarios finales al actualizar la dirección de un cliente. Si el servicio Web especifica que el tipo de dirección (un tipo complejo definido dentro de una clase CustomerDetails) se puede enviar al cliente, a continuación, el proceso de actualización puede dividirse en funciones independientes para una mejor código volver a usar.


[![Crear a partir de una llamada a un servicio Web que devuelve datos RSS de salida.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: crear a partir de una llamada a un servicio Web que devuelve datos RSS de salida.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-web-services/_static/image9.png))


El listado 16 se muestra un ejemplo de código del lado cliente que invoca un objeto de dirección definido en un espacio de nombres del modelo, lo rellena con datos actualizados y lo asigna a la propiedad de dirección de un objeto CustomerDetails. El objeto CustomerDetails, a continuación, se pasa al servicio Web para su procesamiento.

**Enumerar 16. Uso de tipos complejos anidados**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Crear y usar métodos de página

Los servicios Web proporcionan una manera excelente para exponer los servicios puedan volver a utilizar para una variedad de clientes, incluidas las páginas de AJAX de ASP.NET. Sin embargo, puede haber casos donde se necesita una página para recuperar datos que alguna vez no usados o compartidos por otras páginas. En este caso, convertir un archivo .asmx para permitir que la página de acceso a los datos puede parecer excesivos puesto que el servicio solo se usa por una sola página.

AJAX de ASP.NET proporciona otro mecanismo para realizar llamadas de tipo de servicio Web sin necesidad de crear los archivos .asmx independiente. Esto se realiza mediante una técnica que se conoce como "métodos de página". Página son métodos estáticos (shared en VB.NET) incrustados directamente en un archivo de página o código lateral que tienen el atributo WebMethod aplicado a ellos. Aplicando el atributo WebMethod se puede llamar mediante un objeto de JavaScript especial denominado PageMethods que se crea dinámicamente en tiempo de ejecución. El objeto PageMethods actúa como un proxy que intercepta el proceso de serialización o deserialización de JSON. Tenga en cuenta que para poder usar el objeto PageMethods debe establecer propiedad de EnablePageMethods de ScriptManager en true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Enumerar 17 muestra un ejemplo de definición de dos métodos de página en una clase de código subyacente de ASP.NET. Estos métodos recuperan datos de una clase de la capa de negocio ubicada en la aplicación\_carpeta de código del sitio Web.

**Enumerar 17. Definir métodos de página.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Cuando ScriptManager detecta la presencia de métodos Web en la página genera una referencia dinámica al objeto PageMethods mencionada anteriormente. Llamar a un método Web se consigue haciendo referencia a la clase PageMethods seguida del nombre del método y los datos de parámetro necesario que se deben pasar. Enumerar 18, muestra ejemplos de llamar a los dos métodos de página mostrados anteriormente.

**Enumerar 18. Llamar a métodos de página con el objeto PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Uso del objeto PageMethods es muy similar al uso de un objeto de proxy de JavaScript. En primer lugar debe especificar todos los datos del parámetro que deben pasarse al método de la página y, a continuación, definen la función de devolución de llamada que debe llamarse cuando finaliza la llamada asincrónica. También se puede especificar una devolución de llamada de error (consulte listado 14 para obtener un ejemplo de control de errores).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>El AutoCompleteExtender y el Kit de herramientas de AJAX de ASP.NET

El Kit de herramientas de AJAX de ASP.NET (disponible en [ http://ajax.asp.net ](http://ajax.asp.net)) ofrece varios controles que pueden utilizarse para tener acceso a servicios Web. En concreto, el Kit de herramientas contiene un control útil denominado `AutoCompleteExtender` que se puede utilizar para llamar a servicios Web y mostrar los datos en las páginas sin tener que escribir ningún código JavaScript en absoluto.

El control AutoCompleteExtender puede usarse para ampliar la funcionalidad existente de un cuadro de texto y ayudar a los usuarios más localizar fácilmente los datos que está buscando. A medida que escriben en un cuadro de texto el control puede usarse para consultar un servicio Web y muestra los resultados a continuación el cuadro de texto dinámicamente. Figura 4 muestra un ejemplo del uso del control AutoCompleteExtender para mostrar los identificadores de cliente para una aplicación de soporte técnico. El usuario escribe caracteres diferentes en el cuadro de texto, se mostrará diferentes elementos por debajo de él en función de su entrada. Los usuarios, a continuación, pueden seleccionar el identificador de cliente deseado.

Utilizando el AutoCompleteExtender dentro de una página AJAX de ASP.NET requiere que el ensamblado AjaxControlToolkit.dll se agreguen a la carpeta bin del sitio Web. Una vez que se ha agregado el ensamblado del Kit de herramientas, desea hacer referencia a él en el archivo web.config para que los controles que contiene están disponibles para todas las páginas de una aplicación. Esto puede hacerse agregando la siguiente etiqueta dentro del archivo web.config &lt;controles&gt; etiqueta:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

En casos donde solo es necesario usar el control en una página específica puede hacer referencia a él mediante la adición de la directiva de referencia a la parte superior de una página tal y como se muestra a continuación en lugar de actualizar el archivo web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Usando el control AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: usar el control AutoCompleteExtender.  ([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-web-services/_static/image12.png))


Una vez que el sitio Web se configuró para usar el Kit de herramientas de AJAX de ASP.NET, un control AutoCompleteExtender puede agregarse en la página mucho que agregaría un control de servidor ASP.NET normal. Enumerar 19, muestra un ejemplo del uso del control para llamar a un servicio Web.

**Enumerar 19. Usando el control AutoCompleteExtender de Kit de herramientas de AJAX de ASP.NET.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

El AutoCompleteExtender tiene varias propiedades, incluidas las propiedades de ID y runat estándares que se encuentra en controles de servidor. Además, permite definir cuántos caracteres se consulta un tipo de usuario final antes que el servicio Web para los datos. La propiedad MinimumPrefixLength mostrada en la lista de 19 hace que el servicio se llama cada vez que se escribe un carácter en el cuadro de texto. Deberá tener cuidado al establecer este valor ya que cada vez que el usuario escribe un carácter al servicio Web que se llamará para buscar valores que coincidan con los caracteres en el cuadro de texto. El servicio Web para llamar a, así como el método Web de destino se define mediante las propiedades ServicePath y ServiceMethod respectivamente. Por último, la propiedad TargetControlID identifica qué cuadro de texto para enlazar el control AutoCompleteExtender.

El servicio Web que se la llame debe tener el atributo ScriptService aplicar tal y como se indicó anteriormente y el método Web de destino debe aceptar dos parámetros denominados prefixText y recuento. El parámetro prefixText representa los caracteres escritos por el usuario final y el parámetro count representa el número de elementos para devolver (el valor predeterminado es 10). Lista de 20, muestra un ejemplo del método Web GetCustomerIDs llamado por el control de AutoCompleteExtender que se explicó anteriormente en la lista de 19. El método Web llama a un método de la capa de negocio que a su vez llama un nivel de datos método que controla el filtrado de los datos y devolver los resultados coincidentes. El código para el método de la capa de datos se muestra en el listado 21.

**Lista de 20. Filtrado de los datos enviado desde el control AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Enumerar 21. Filtrar los resultados en función de los proporcionados por el usuario final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusión

AJAX de ASP.NET proporciona una excelente compatibilidad para llamar a servicios Web sin tener que escribir una gran cantidad de código JavaScript personalizado para controlar los mensajes de solicitud y respuesta. En este artículo ha visto cómo los servicios Web de AJAX enable .NET para habilitarlos para procesar mensajes JSON y cómo definir servidores proxy de JavaScript mediante el control ScriptManager. También hemos visto cómo JavaScript se pueden utilizar proxies para llamar a servicios Web, controlar los tipos simples y complejos y resolver los errores. Por último, ha visto cómo se pueden utilizar métodos de página para simplificar el proceso de crear y realizar llamadas a servicios Web y cómo el control AutoCompleteExtender puede proporcionar ayuda a los usuarios finales a medida que escriben. Aunque el UpdatePanel disponible en ASP.NET AJAX sin duda será el control de elección para muchos programadores de AJAX debido a su simplicidad, saber cómo llamar a servicios Web a través de servidores proxy de JavaScript puede ser útil en muchas aplicaciones.

## <a name="bio"></a>Bio

Dan Wahlin (Microsoft los profesionales más valiosos de ASP.NET y servicios Web XML) es un consultor de instructor y arquitectura de desarrollo de .NET en formación técnica de interfaz ([http://www.interfacett.com](http://www.interfacett.com)). Dan fundada el XML para el sitio Web de desarrolladores de ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se encuentra en la oficina del orador INETA y ofrece varias conferencias. Dan coautor Professional Windows ADN (Wrox), ASP.NET: sugerencias, tutoriales y código (SAM), ASP.NET 1.1 Insider soluciones, Professional ASP.NET 2.0 AJAX (Wrox), ataques de MVP de ASP.NET 2.0 y XML creado para los programadores de ASP.NET (SAM). Si no, que está escribiendo código, artículos o libros, Dan disfruta de escribir y grabar música y reproducir golf y baloncesto con su esposa e hijos.

Scott categoría ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el director general de myKB.com ([www.myKB.com](http://www.myKB.com)) donde está especializado en la escritura de ASP.NET en función de las aplicaciones que se centra en las soluciones de Software de Base de conocimiento. Scott se puede contactar a través de correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-localization.md)
> [Siguiente](understanding-asp-net-ajax-debugging-capabilities.md)
