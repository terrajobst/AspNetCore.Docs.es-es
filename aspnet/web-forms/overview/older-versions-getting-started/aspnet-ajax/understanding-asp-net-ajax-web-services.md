---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: Descripción de servicios Web ASP.NET AJAX | Microsoft Docs
author: scottcate
description: Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque Web...
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: b98ef4c27ab7b4b729e9e5b68e7d2642a6418ab6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37838606"
---
<a name="understanding-aspnet-ajax-web-services"></a>Descripción de servicios Web ASP.NET AJAX
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque normalmente se usan los servicios Web para permitir que distintos sistemas operativos, modelos de objetos y lenguajes de programación para enviar y recibir datos, también puede usar para insertar datos en una página ASP.NET AJAX dinámicamente o enviar datos desde una página a un sistema back-end. Todo esto puede realizarse sin tener que recurrir para las operaciones de devolución de datos.


## <a name="calling-web-services-with-aspnet-ajax"></a>Llamar a los servicios Web con ASP.NET AJAX

Dan Wahlin

Servicios Web son una parte integral de .NET framework que proporcionan una solución multiplataforma para intercambiar datos entre sistemas distribuidos. Aunque normalmente se usan los servicios Web para permitir que distintos sistemas operativos, modelos de objetos y lenguajes de programación para enviar y recibir datos, también puede usar para insertar datos en una página ASP.NET AJAX dinámicamente o enviar datos desde una página a un sistema back-end. Todo esto puede realizarse sin tener que recurrir para las operaciones de devolución de datos.

Mientras que el control UpdatePanel de ASP.NET AJAX proporciona una manera sencilla de AJAX habilitar cualquier página ASP.NET, puede haber ocasiones cuando necesite tener acceso dinámicamente a los datos en el servidor sin usar un control UpdatePanel. En este artículo verá cómo realizar esta tarea mediante la creación y consumo de servicios Web dentro de las páginas de AJAX de ASP.NET.

En este artículo se centrará en la funcionalidad disponible en las extensiones de AJAX de ASP.NET core, así como un control habilitado el servicio Web del Kit de herramientas de AJAX de ASP.NET llamado el AutoCompleteExtender. Los temas incluyen la definición de los servicios de Web habilitado para AJAX, crear servidores proxy de cliente y llamar a servicios Web con JavaScript. También verá cómo se pueden hacer llamadas a servicios Web directamente a los métodos de página ASP.NET.

## <a name="web-services-configuration"></a>Configuración de servicios Web

Cuando se crea un nuevo proyecto de sitio Web con Visual Studio 2008, el archivo web.config tiene un número de nuevas adiciones que no es posible que le resulte familiar a los usuarios de versiones anteriores de Visual Studio. Algunas de estas modificaciones para que puedan usar en las páginas mientras que otras personas definen requiere HttpHandlers y HttpModules, asigne el prefijo "asp" a los controles de AJAX de ASP.NET. Las modificaciones realizadas en el listado 1 muestra el `<httpHandlers>` elemento en el archivo web.config que afecta a las llamadas al servicio Web. El valor predeterminado que HttpHandler usado para procesar las llamadas de ASMX es eliminado y reemplazado por una clase ScriptHandlerFactory ubicada en el ensamblado de System.Web.Extensions.dll. System.Web.Extensions.dll contiene toda la funcionalidad básica usa ASP.NET AJAX.

**Listado 1. Configuración de controlador de servicio Web de ASP.NET AJAX**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

Este reemplazo HttpHandler se realiza con el fin de permitir las llamadas de JavaScript Object Notation (JSON) se realiza desde las páginas de AJAX de ASP.NET para servicios Web de .NET mediante un proxy de servicio Web de JavaScript. AJAX de ASP.NET envía mensajes JSON a los servicios Web en lugar de las llamadas de Protocolo Simple de acceso a objetos (SOAP) estándar asociadas normalmente a los servicios Web. Esto da como resultado más pequeño mensajes de solicitud y respuesta general. También permite para un procesamiento más eficaz de los datos del lado cliente dado que la biblioteca de JavaScript de AJAX de ASP.NET está optimizada para trabajar con objetos JSON. Listado 2 y listado 3 muestran ejemplos de mensajes de solicitud y respuesta del servicio Web serializados en formato JSON. El mensaje de solicitud que se muestra en el listado 2 pasa un parámetro de país con un valor de "Bélgica" mientras el mensaje de respuesta en el listado 3 pasa una matriz de objetos Customer y sus propiedades asociadas.

**Listado 2. Mensaje de solicitud de servicio Web serializada a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] el nombre de la operación se define como parte de la dirección URL del servicio web; Además, los mensajes de solicitud no se envían siempre a través de JSON. Servicios Web pueden usar el atributo ScriptMethod con el parámetro UseHttpGet establecido en true, lo que hace que los parámetros que se pasarán a través de un los parámetros de cadena de consulta.*


**Listado 3. Mensaje de respuesta del servicio Web serializada a JSON**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

En la sección siguiente verá cómo crear servicios Web capaz de controlar los mensajes de solicitud JSON y responder con tipos simples y complejos.

## <a name="creating-ajax-enabled-web-services"></a>Creación de servicios Web con AJAX habilitado

El marco de ASP.NET AJAX proporciona varias maneras de llamar a servicios Web. Puede usar el control AutoCompleteExtender (disponible en el Kit de herramientas de AJAX de ASP.NET) o JavaScript. Sin embargo, antes de llamar a un servicio esté a AJAX para que se puede llamar mediante código de script de cliente.

Si está familiarizado con los servicios Web de ASP.NET, encontrará que fácilmente se pueden crear y servicios de habilitación de AJAX. .NET framework admite la creación de servicios Web de ASP.NET desde su lanzamiento inicial en 2002 y ASP.NET AJAX Extensions ofrecen funcionalidad ampliada de AJAX que se basa en el conjunto predeterminado de .NET framework de características. 2008 Beta 2 de Visual Studio .NET tiene compatibilidad integrada para crear archivos de servicio Web de ASMX y deduce automáticamente el código asociado al lado de las clases de la clase System.Web.Services.WebService. A medida que agregue los métodos en la clase que se debe aplicar el atributo WebMethod en orden para poder llamar a los consumidores del servicio Web.

Listado 4 muestra un ejemplo de cómo aplicar el atributo WebMethod a un método denominado GetCustomersByCountry().

**Listado 4. Con el atributo WebMethod en un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

El método GetCustomersByCountry() acepta un parámetro de país y devuelve a un cliente de matriz de objetos. El valor de país que se pasa al método se reenvía a una clase de nivel empresarial que a su vez llama a una clase de capa de datos para recuperar los datos de la base de datos, rellene las propiedades del objeto de cliente con datos y devolver la matriz.

## <a name="using-the-scriptservice-attribute"></a>Mediante el atributo ScriptService

Al agregar el atributo permite que el método GetCustomersByCountry() ser llamado por los clientes que envían mensajes estándar de SOAP al servicio Web WebMethod, no permite las llamadas JSON se realice desde aplicaciones de ASP.NET AJAX desde el principio. Para permitir llamadas JSON realizarse tiene que aplicar la extensión de ASP.NET AJAX `ScriptService` a la clase de servicio Web. Esto permite que un servicio Web enviar mensajes de respuesta con formato mediante JSON y permite el script de cliente llamar a un servicio mediante el envío de mensajes JSON.

Listado 5 muestra un ejemplo de cómo aplicar el atributo ScriptService a una clase de servicio Web denominada CustomersService.

**Listado 5. Mediante el atributo ScriptService AJAX-habilitar un servicio Web**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

El atributo ScriptService actúa como un marcador que indica que se puede llamar desde código de script de AJAX. Realmente no controla cualquiera de las tareas de serialización o deserialización de JSON que se producen en segundo plano. El ScriptHandlerFactory (configurado en web.config) y otras clases relacionadas para realizar la mayor parte del procesamiento de JSON.

## <a name="using-the-scriptmethod-attribute"></a>Mediante el atributo ScriptMethod

El atributo ScriptService es el único atributo de AJAX de ASP.NET que tiene que definirse en un servicio Web de .NET para poder que va a usar las páginas de AJAX de ASP.NET. Sin embargo, otro atributo denominado ScriptMethod también se puede aplicar directamente a los métodos Web en un servicio. ScriptMethod define tres propiedades incluidos `UseHttpGet`, `ResponseFormat` y `XmlSerializeString`. Cambiar los valores de estas propiedades puede ser útil en casos donde el tipo de solicitud aceptada por un método Web debe cambiarse a GET, cuando un método Web debe devolver datos XML sin formato en forma de un `XmlDocument` o `XmlElement` objeto o cuando se devuelven los datos de un  servicio siempre se debe serializar como XML en lugar de JSON.

La propiedad se puede utilizar cuando un método Web debe aceptar de UseHttpGet obtener solicitudes en lugar de las solicitudes POST. Las solicitudes se envían mediante una dirección URL con parámetros de entrada del método Web convierte en parámetros de cadena de consulta. El UseHttpGet propiedad valor predeterminado es false y solo debe establecerse en `true` cuando las operaciones son conocidas son seguros y cuando los datos confidenciales no se pasan a un servicio Web. Listado 6 muestra un ejemplo de cómo utilizar el atributo ScriptMethod con la propiedad UseHttpGet.

**Listado 6. Usar el atributo ScriptMethod con la propiedad UseHttpGet.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

Se muestra un ejemplo de los encabezados enviados cuando se llama al método de Web HttpGetEcho se muestra en el listado 6 siguiente:

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Además de permitir que los métodos Web Aceptar las solicitudes HTTP GET, también se puede usar el atributo ScriptMethod cuando las respuestas XML deben devolverse desde un servicio en lugar de JSON. Por ejemplo, un servicio Web puede recuperar una fuente RSS de un sitio remoto y lo devuelve como un objeto XmlDocument o un XmlElement. Datos de procesamiento de XML, a continuación, pueden producirse en el cliente.

Listado 7 muestra un ejemplo del uso de la propiedad ResponseFormat para especificar que se deben devolver los datos XML desde un método Web.

**Listado 7. Usar el atributo ScriptMethod con la propiedad ResponseFormat.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

También se puede usar la propiedad ResponseFormat junto con la propiedad XmlSerializeString. La propiedad XmlSerializeString tiene un valor predeterminado es False, lo que significa que todas devuelven tipos, excepto las cadenas devueltas desde un método Web se serializan como XML cuando el `ResponseFormat` propiedad está establecida en `ResponseFormat.Xml`. Cuando `XmlSerializeString` está establecido en `true`, todos los tipos devueltos desde un método Web se serializan como XML, incluidos los tipos de cadena. Si la propiedad ResponseFormat tiene un valor de `ResponseFormat.Json` XmlSerializeString se omite.

Listado 8 muestra un ejemplo del uso de la propiedad XmlSerializeString para forzar las cadenas debe serializarse como XML.

**Listado 8. Usar el atributo ScriptMethod con la propiedad XmlSerializeString**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

El valor devuelto de una llamada al método de Web GetXmlString se muestra en el listado 8 se muestra la siguiente:

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

Aunque el formato JSON predeterminado minimiza el tamaño total de mensajes de solicitud y respuesta y se consume más fácilmente por los clientes de AJAX de ASP.NET de una manera de exploradores, las propiedades ResponseFormat y XmlSerializeString pueden ser utilizados al cliente aplicaciones como Internet Explorer 5 o superior esperan que los datos XML que se devuelve desde un método Web.

## <a name="working-with-complex-types"></a>Trabajar con tipos complejos

Listado 5 mostró un ejemplo de cómo devolver un tipo complejo denominado a cliente de un servicio Web. La clase Customer define varios tipos diferentes de simple internamente como propiedades como FirstName y LastName. Tipos complejos se usan como un parámetro de entrada o tipo de valor devuelto en un método Web con AJAX habilitado automáticamente se serializan en JSON antes de enviarse al lado de cliente. Sin embargo, los tipos complejos anidados (aquellos definidos de forma interna dentro de otro tipo) no están disponibles para el cliente como objetos independientes de forma predeterminada.

En casos donde un tipo complejo anidado usado por un servicio Web también se debe usar en una página de cliente, el atributo GenerateScriptType de AJAX de ASP.NET puede agregarse al servicio Web. Por ejemplo, la clase CustomerDetails se muestra en el listado 9 contiene las propiedades de dirección y el sexo que ***representan tipos complejos anidados.***

**Listado 9. La clase CustomerDetails que se muestra aquí contiene dos tipos complejos anidados.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

Los objetos dirección y el sexo definidos dentro de la clase CustomerDetails se muestra en el listado 9 no convierte automáticamente en estar disponibles para su uso en el lado cliente a través de JavaScript, ya que son tipos anidados (dirección es una clase y género es una enumeración). En situaciones donde un tipo anidado que se usa dentro de un servicio Web debe estar disponible en el lado cliente, se puede usar el atributo GenerateScriptType se ha mencionado anteriormente (consulte el listado 10). Este atributo se puede agregar varias veces en los casos donde se devuelven los diferentes tipos complejos anidados de un servicio. Se puede aplicar directamente a la clase de servicio Web o por encima de los métodos Web específicos.

**Listado 10. Mediante el atributo GenerateScriptService para definir los tipos anidados deben estar disponibles para el cliente.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

Aplicando la `GenerateScriptType` atributo a los tipos de servicio Web, la dirección y el sexo estarán automáticamente disponible para su uso por código de JavaScript de AJAX de ASP.NET del cliente. En el listado 11 se muestra un ejemplo de JavaScript que se genera automáticamente y se envía al cliente agregando el atributo GenerateScriptType en un servicio Web. Verá cómo usar tipos complejos anidados más adelante en el artículo.

**Listado 11. Tipos complejos anidados disponible a una página AJAX de ASP.NET.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Ahora que ha visto cómo crear servicios Web y hacerlos accesibles a las páginas de ASP.NET AJAX, echemos un vistazo a cómo crear y utilizar a servidores proxy de JavaScript para que se pueden recuperar los datos o envía a los servicios Web.

## <a name="creating-javascript-proxies"></a>Crear a servidores proxy de JavaScript

Llamar a un servicio Web estándar (.NET u otra plataforma) normalmente implica la creación de un objeto proxy que intercepta las complejidades de envío de mensajes de solicitud y respuesta SOAP. Con las llamadas de servicio Web de ASP.NET AJAX, se pueden crear servidores proxy de JavaScript y usar llamar fácilmente a los servicios sin preocuparse por serializar y deserializar los mensajes JSON. Servidores proxy de JavaScript pueden generarse automáticamente mediante el control ScriptManager de ASP.NET AJAX.

Creación de un proxy de JavaScript que puede llamar a servicios Web se logra mediante el uso de la propiedad de los servicios de ScriptManager. Esta propiedad permite definir uno o varios servicios que una página AJAX de ASP.NET puede llamar de forma asincrónica para enviar o recibir datos sin necesidad de realizar operaciones de devolución de datos. Definir un servicio mediante el uso de ASP.NET AJAX `ServiceReference` control y asignar la dirección URL del servicio Web para el control `Path` propiedad. Listado 12 muestra un ejemplo de hacer referencia a un servicio denominado CustomersService.asmx.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**Lista 12. Definir un servicio Web en una página AJAX de ASP.NET.**

Agregar una referencia a la CustomersService.asmx a través del control ScriptManager, hace que un proxy de JavaScript que se genera dinámicamente y se hace referencia a la página. El proxy está insertado utilizando el &lt;script&gt; etiquetar y se carga dinámicamente llamando el archivo CustomersService.asmx y anexar /js al final de la misma. El ejemplo siguiente muestra cómo el proxy de JavaScript se incrusta en la página cuando se deshabilita la depuración en el archivo web.config:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] Si gustaría ver el código de proxy de JavaScript real que se genera puede escribir la dirección URL del servicio Web de .NET deseado en el cuadro de dirección de Internet Explorer y anexar /js al final de la misma.*


Si está habilitada la depuración en el archivo web.config de que una versión de depuración del proxy de JavaScript se incrustarán en la página como se muestra a continuación:

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

El proxy de JavaScript creado por el control ScriptManager también se puede insertar directamente en la página en lugar de hacer referencia mediante el &lt;script&gt; atributo src de la etiqueta. Esto puede hacerse estableciendo la propiedad de InlineScript ServiceReference del control en true (el valor predeterminado es false). Esto puede ser útil cuando un proxy no se comparte entre varias páginas y cuando se desea reducir el número de llamadas de red realizadas al servidor. Cuando InlineScript se establece en true el script de proxy no se almacena en caché del explorador, se recomienda el valor predeterminado es False en casos donde el proxy se usa por varias páginas en una aplicación de ASP.NET AJAX. Se muestra un ejemplo del uso de la propiedad InlineScript siguiente:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>Uso de servidores proxy de JavaScript

Una vez que una página de ASP.NET AJAX mediante el control ScriptManager hace referencia a un servicio Web, puede realizar una llamada al servicio Web y los datos devueltos pueden controlarse mediante funciones de devolución de llamada. Se llama a un servicio Web que hacen referencia a su espacio de nombres (si existe), nombre de clase y nombre del método Web. Todos los parámetros pasados al servicio Web se pueden definir junto con una función de devolución de llamada que controla los datos devueltos.

En el listado 13 se muestra un ejemplo del uso de un proxy de JavaScript para llamar a un método Web denominado GetCustomersByCountry(). La función GetCustomersByCountry() se llama cuando un usuario final hace clic en un botón en la página.

**Lista 13. Una llamada a un servicio Web con un proxy de JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

El espacio de nombres InterfaceTraining hace referencia a esta llamada, CustomersService clase y método de Web GetCustomersByCountry definen en el servicio. Pasa un valor de país obtenido de un control textbox, así como una función de devolución de llamada denominado OnWSRequestComplete que se debe invocar cuando finaliza la llamada asincrónica del servicio Web. OnWSRequestComplete controla la matriz de objetos de cliente devuelto desde el servicio y los convierte en una tabla que se muestra en la página. La salida generada por la llamada se muestra en la figura 1.


[![Enlace de datos obtenidos mediante la realización de una llamada AJAX asincrónica a un servicio Web.](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**Figura 1**: enlace de datos obtenido mediante la realización de una llamada AJAX asincrónica a un servicio Web.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-web-services/_static/image3.png))


Servidores proxy de JavaScript también pueden realizar llamadas unidireccionales a servicios Web en casos donde se debe llamar un método Web pero el proxy no debe esperar una respuesta. Por ejemplo, es posible que desee llamar a un servicio Web para iniciar un proceso como un flujo de trabajo, pero no espere a que un valor devuelto desde el servicio. En casos donde debe realizarse a un servicio de una llamada unidireccional, simplemente se puede omitir la función de devolución de llamada que se muestra en el listado 13. Puesto que no se define ninguna función de devolución de llamada no esperará el objeto proxy para que el servicio Web devolver los datos.

## <a name="handling-errors"></a>Control de errores

Devoluciones de llamadas asincrónicas a servicios Web pueden encontrar diferentes tipos de errores, como la red inactivo, el servicio Web que no está disponible o una excepción que se devuelven. Afortunadamente, objetos proxy de JavaScript generados por el control ScriptManager permiten varias devoluciones de llamada a definirse para controlar errores y además de la devolución de llamada correcta que se muestra anteriormente. Puede definirse una función de devolución de llamada de error inmediatamente después de la función de devolución de llamada estándar en la llamada al método Web tal como se muestra en el listado 14.

**Lista 14. Definir una función de devolución de llamada de error y mostrar los errores.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Los errores que se producen cuando se llama al servicio Web desencadenarán la función de devolución de llamada OnWSRequestFailed() para llamarse que acepta un objeto que representa el error como un parámetro. El objeto de error expone varias funciones diferentes para determinar la causa del error, así como si la llamada agotó el tiempo. Listado 14 muestra un ejemplo del uso de las funciones de error diferentes y la figura 2 muestra un ejemplo de la salida generada por las funciones.


[![Salida generada mediante una llamada a funciones de error de AJAX de ASP.NET.](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**Figura 2**: salida generada mediante una llamada a funciones de error de AJAX de ASP.NET.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Controlar los datos XML devueltos desde un servicio Web

Antes vimos cómo un método Web puede devolver datos XML sin formato mediante el atributo ScriptMethod junto con su propiedad ResponseFormat. Cuando ResponseFormat se establece en ResponseFormat.Xml, los datos devueltos desde el servicio Web se serializan como XML en lugar de JSON. Esto puede ser útil cuando los datos XML deben pasarse directamente al cliente para el procesamiento con JavaScript o XSLT. En este momento, Internet Explorer 5 o superior proporciona el mejor modelo de objeto del lado cliente para analizar y filtrar los datos XML debido a su compatibilidad integrada para MSXML.

Recuperar datos XML de un servicio Web no es diferente de recuperar otros tipos de datos. Iniciar mediante la invocación del proxy de JavaScript para llamar a la función adecuada y definir una función de devolución de llamada. Una vez que se devuelve la llamada, a continuación, puede procesar los datos de la función de devolución de llamada.

Listado 15, muestra un ejemplo de una llamada a un método Web denominado GetRssFeed() que devuelve un objeto XmlElement. GetRssFeed() acepta un parámetro único que representa la dirección URL de la fuente RSS para recuperar.

**Lista 15. Trabajar con datos XML devueltos desde un servicio Web.**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

En este ejemplo se pasa una dirección URL a una fuente RSS y procesa los datos XML devueltos en la función OnWSRequestComplete(). OnWSRequestComplete() comprueba primero si el explorador es Internet Explorer para saber si el analizador MSXML está disponible. Si es así, se utiliza una instrucción XPath para buscar todos &lt;elemento&gt; etiquetas dentro de la fuente RSS. A continuación, se recorre en iteración cada elemento y asociado &lt;título&gt; y &lt;vínculo&gt; etiquetas se encuentra y se procesan para mostrar los datos de cada elemento. Figura 3 muestra un ejemplo de la salida generada por la que hace ASP.NET AJAX a llamar a través de un proxy de JavaScript al método Web GetRssFeed().

## <a name="handling-complex-types"></a>Administración de los tipos complejos

Tipos complejos que aceptan o devueltos por un servicio Web se exponen automáticamente a través de un proxy de JavaScript. Sin embargo, los tipos complejos anidados no son accesibles directamente en el lado cliente a menos que se aplica el atributo GenerateScriptType al servicio como se explicó anteriormente. ¿Por qué querría usar un tipo complejo anidado en el cliente?

Para responder esta pregunta, suponga que una página AJAX de ASP.NET muestra los datos del cliente y permite que los usuarios finales actualizar la dirección de un cliente. Si el servicio Web especifica que el tipo de dirección (un tipo complejo definido en una clase CustomerDetails) se puede enviar al cliente, a continuación, el proceso de actualización puede dividirse en funciones independientes para una mejor reutilización del código.


[![Creación de la llamada a un servicio Web que devuelve datos de RSS de salida.](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**Figura 3**: creación de la llamada a un servicio Web que devuelve datos de RSS de salida.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-web-services/_static/image9.png))


Lista 16 se muestra un ejemplo de código del lado cliente que invoca un objeto de dirección definido en un espacio de nombres del modelo, lo rellena con datos actualizados y lo asigna a la propiedad de dirección de un objeto CustomerDetails. El objeto CustomerDetails, a continuación, se pasa al servicio Web para su procesamiento.

**Lista 16. Uso de tipos complejos anidados**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>Crear y usar los métodos de página

Los servicios Web proporcionan una manera excelente para exponer servicios puedan volver a utilizar en una variedad de clientes, incluidas las páginas de AJAX de ASP.NET. Sin embargo, puede haber casos donde se necesita una página para recuperar datos que alguna vez no usa o compartidos por otras páginas. En este caso, convertir en un archivo .asmx para permitir que la página para acceder a los datos puede parecer excesivo puesto que el servicio sólo sirve para una sola página.

ASP.NET AJAX ofrece otro mecanismo para realizar las llamadas como de servicio Web sin necesidad de crear los archivos .asmx independiente. Esto se realiza mediante una técnica que se denominan "métodos de página". Página son métodos estáticos (compartidos en VB.NET) incrustados directamente en un archivo de página o código lateral que tienen el atributo WebMethod se les aplica. Al aplicar el atributo WebMethod se puede llamar mediante un objeto de JavaScript especial denominado PageMethods que se crea dinámicamente en tiempo de ejecución. El objeto PageMethods actúa como un proxy que intercepta el proceso de serialización y deserialización de JSON. Tenga en cuenta que para poder usar el objeto PageMethods debe establecer propiedad EnablePageMethods de ScriptManager en true.

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

Enumerar 17 muestra un ejemplo de definir dos métodos de página en una clase de código subyacente de ASP.NET. Estos métodos recuperan datos de una clase de la capa de negocio ubicada en la aplicación\_carpeta de código del sitio Web.

**Listado 17. Definir los métodos de página.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Cuando ScriptManager detecta la presencia de los métodos Web en la página genera una referencia dinámica a un objeto de PageMethods mencionado anteriormente. Llamar a un método Web se consigue haciendo referencia a la clase PageMethods seguida del nombre del método y los datos de parámetro necesarios que deben pasarse. Enumerar 18 muestra ejemplos de llamar a los dos métodos de página que se mostró anteriormente.

**Enumerar 18. Llamar a métodos de página con el objeto de PageMethods JavaScript.**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

Uso del objeto PageMethods es muy similar al uso de un objeto de proxy de JavaScript. En primer lugar debe especificar todos los datos del parámetro que se deben pasar al método de página y, a continuación, definen la función de devolución de llamada que se debe llamar cuando se devuelve la llamada asincrónica. También se puede especificar una devolución de llamada de error (consulte listado 14 para obtener un ejemplo de control de errores).

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>El AutoCompleteExtender y el Kit de herramientas de AJAX de ASP.NET

El Kit de herramientas de AJAX de ASP.NET (disponible en [ http://ajax.asp.net ](http://ajax.asp.net)) ofrece varios controles que pueden utilizarse para tener acceso a servicios Web. En concreto, el Kit de herramientas contiene un control útil denominado `AutoCompleteExtender` que se puede utilizar para llamar a servicios Web y mostrar datos en las páginas sin escribir ningún código de JavaScript en absoluto.

El control AutoCompleteExtender puede utilizarse para extender la funcionalidad existente de un cuadro de texto y ayudar a los usuarios más localizar fácilmente los datos que está buscando. A medida que escriben en un cuadro de texto el control puede usarse para consultar un servicio Web y muestra resultados debajo del cuadro de texto de forma dinámica. Figura 4 muestra un ejemplo del uso del control AutoCompleteExtender para mostrar los identificadores de cliente para una aplicación de soporte técnico. Como el usuario escribe caracteres diferentes en el cuadro de texto, se mostrarán los diferentes elementos debajo de ella según los datos proporcionados. Los usuarios, a continuación, pueden seleccionar el identificador de cliente deseada.

Uso de la AutoCompleteExtender dentro de una página AJAX de ASP.NET requiere que el ensamblado AjaxControlToolkit.dll agregarse a la carpeta bin del sitio Web. Una vez que se ha agregado el ensamblado del Kit de herramientas, desea hacer referencia a él en el archivo web.config para que los controles que contiene están disponibles para todas las páginas de una aplicación. Esto puede hacerse agregando la siguiente etiqueta dentro del archivo web.config &lt;controles&gt; etiqueta:

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

En casos donde sólo es necesario usar el control en una página específica puede hacer referencia a él mediante la adición de la directiva de referencia a la parte superior de una página como se muestra a continuación, en lugar de actualizar el archivo web.config:

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![Uso del control AutoCompleteExtender.](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**Figura 4**: uso del control AutoCompleteExtender.  ([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-web-services/_static/image12.png))


Una vez que el sitio Web se ha configurado para usar el Kit de herramientas de AJAX de ASP.NET, un control AutoCompleteExtender puede agregarse en la página mucho que agregaría un control de servidor ASP.NET normal. Listado de 19, muestra un ejemplo del uso del control para llamar a un servicio Web.

**Enumerar 19. Uso del control AutoCompleteExtender de Kit de herramientas de AJAX de ASP.NET.**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

El AutoCompleteExtender tiene varias propiedades, incluidas las propiedades ID y runat estándar que se encuentra en los controles de servidor. Además, permite definir cuántos caracteres de un tipo de usuario final antes de que el servicio Web recibe consultas de datos. La propiedad MinimumPrefixLength que se muestra en la lista de 19 hace que el servicio se llama cada vez que se escribe un carácter en el cuadro de texto. Conviene tener cuidado al establecer este valor, puesto que cada vez que el usuario escribe un carácter, el servicio Web que se llamará para buscar valores que coinciden con los caracteres en el cuadro de texto. El servicio Web para llamar a, así como el método Web de destino se define mediante las propiedades ServicePath y ServiceMethod respectivamente. Por último, la propiedad TargetControlID identifica qué textbox para enlazar el control AutoCompleteExtender.

El servicio Web que se llama debe tener el atributo ScriptService aplican como se explicó anteriormente y el método Web de destino debe aceptar dos parámetros denominados prefixText y count. El parámetro prefixText representa los caracteres escritos por el usuario final y el parámetro count representa cuántos elementos se devuelven (el valor predeterminado es 10). Enumerar 20 muestra un ejemplo del método Web GetCustomerIDs llamado por el control AutoCompleteExtender mostrado anteriormente en la lista de 19. El método Web llama a un método de la capa de negocio que a su vez llama a un nivel de datos método que controla el filtrado de los datos y devolver los resultados coincidentes. El código para el método de la capa de datos se muestra en la lista de 21.

**Lista de 20. Filtrado de datos enviados desde el control AutoCompleteExtender.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**Enumerar 21. Filtrar los resultados en función de entrada del usuario final.**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>Conclusión

ASP.NET AJAX proporciona un soporte excelente para llamar a servicios Web sin escribir mucho código JavaScript personalizado para controlar los mensajes de solicitud y respuesta. En este artículo, ha visto cómo habilitar AJAX .NET Web Services para que puedan procesar mensajes JSON y cómo definir los servidores proxy de JavaScript mediante el control ScriptManager. También hemos visto cómo JavaScript se pueden utilizar proxies para llamar a servicios Web, controlar los tipos simples y complejos y lidiar con errores. Por último, ha visto cómo se pueden usar los métodos de página para simplificar el proceso de creación y realización de llamadas de servicio Web y cómo puede proporcionar el control AutoCompleteExtender ayuda a los usuarios finales a medida que escriben. Aunque ciertamente, disponible en ASP.NET AJAX UpdatePanel será el control de elección para muchos programadores de AJAX debido a su simplicidad, saber cómo llamar a servicios Web a través de servidores proxy de JavaScript puede ser útil en muchas aplicaciones.

## <a name="bio"></a>Biografía

Dan Wahlin (Microsoft Most Valuable Professional para ASP.NET y servicios Web XML) es .NET development instructor y arquitectura consultor en formación técnica de interfaz ([http://www.interfacett.com](http://www.interfacett.com)). Dan fundó el XML para el sitio Web de desarrolladores de ASP.NET ([www.XMLforASP.NET](http://www.XMLforASP.NET)), se encuentra en la oficina del orador de INETA y es orador en varios congresos. Dan coautor DNA de Windows (Wrox), ASP.NET Professional: sugerencias, tutoriales y código (SAM), especialista en soluciones de ASP.NET 1.1, Professional ASP.NET 2.0 AJAX (Wrox), Hacks de MVP de ASP.NET 2.0 y XML creado para desarrolladores de ASP.NET (SAM). Cuando no está escribiendo código, artículos o libros, Dan disfruta de escribir y grabar música y jugar golf y baloncesto con su esposa e hijos.

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-localization.md)
> [Siguiente](understanding-asp-net-ajax-debugging-capabilities.md)
