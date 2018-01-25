---
uid: web-api/samples-list
title: Lista de ejemplos de API Web | Documentos de Microsoft
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 1e1f43bbeedfc052f0b3a3924f51b544a5a79dca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="web-api-samples-list"></a>Lista de ejemplos de API Web
====================
## <a name="httpclient-samples"></a>Ejemplos de HttpClient

**Ejemplo de Bing traducir** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Muestra cómo llamar a la [servicio de Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) mediante la **HttpClient** clase. La API del servicio Microsoft Translator requiere un token de OAuth, que obtiene de la aplicación mediante el envío de una solicitud al servidor de símbolo (token) de Azure para cada solicitud para el servicio de traductor. El resultado desde el servidor de token se introduce en la solicitud enviada al servicio de traducción. Antes de ejecutar este ejemplo, debe obtener un [clave de aplicación de Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) y rellene la información de la clase de ejemplo AccessTokenMessageHandler.

**Ejemplo de mapas de Google** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Usa **HttpClient** para descargar un mapa de Redmond, WA de [API de Google Maps](https://developers.google.com/maps/), lo guarda como un archivo local y se abre el Visor de imágenes de forma predeterminada.

**Ejemplo de cliente de Twitter** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Muestra cómo escribir un cliente de Twitter simple utilizando **HttpClient**. El ejemplo usa un **HttpMessageHandler** para insertar información de autenticación de OAuth en saliente **HttpRequestMessage**. El resultado de Twitter se lee utilizando JSON.NET. Antes de ejecutar este ejemplo, debe obtener un [clave de aplicación de Twitter](https://dev.twitter.com/)y rellene la información de la clase de ejemplo OAuthMessageHandler.

**Ejemplo de banco mundial** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Muestra cómo recuperar datos desde el sitio de datos banco mundial, uso de JSON.NET para analizar el resultado.

## <a name="web-api-samples"></a>Ejemplos de API Web

**Introducción a ASP.NET Web API** | [origen VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Muestra cómo crear un sitio web básico API que admite solicitudes HTTP GET. Contiene el código fuente para el tutorial [su primer ASP.NET Web API](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Escenarios de JavaScript de la API Web de ASP.NET: comentarios** | [origen VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Muestra cómo usar ASP.NET Web API para compilar las API que admiten a los clientes del explorador y se pueden llamar fácilmente mediante jQuery web.

**Póngase en contacto con el Administrador de** | [origen de VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Este ejemplo utiliza ASP.NET Web API para compilar una aplicación de administrador de contactos simple. La aplicación está formada por un administrador de contactos API web que se usa una aplicación ASP.NET MVC y una aplicación de Windows Phone para mostrar y administrar una lista de contactos.

**Procesamiento por lotes de ejemplo** | [descripción detallada](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Muestra cómo implementar el procesamiento por lotes HTTP en ASP.NET. El procesamiento por lotes se compone de colocar varias solicitudes HTTP dentro de un cuerpo de entidad de varias partes MIME única, que se envía al servidor como una solicitud HTTP POST. Las solicitudes se procesan de forma individual y las respuestas se colocan en otro cuerpo de entidad de varias partes de MIME, que se devuelve al cliente.

**Ejemplo de controlador de contenido** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Muestra cómo leer y escribir las entidades de solicitud y respuesta asincrónicamente mediante secuencias. El controlador de ejemplo tiene dos acciones: una acción de PUT que lee de forma asincrónica el cuerpo de la entidad de solicitud y lo almacena en un archivo local y una acción de GET que devuelve el contenido del archivo local.

**Ejemplo de resolución de ensamblado personalizado** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Muestra cómo modificar la API de Web de ASP.NET para admitir la detección de controladores de un ensamblado de biblioteca cargada dinámicamente. El ejemplo implementa un personalizado **IAssembliesResolver** que llama a la implementación predeterminada y, a continuación, agrega el ensamblado de biblioteca a los resultados de forma predeterminada.

**Ejemplo de formateador de tipo de medio personalizado** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Muestra cómo crear un formateador de tipo de medio personalizado mediante la **BufferedMediaTypeFormatter** clase base. Esta clase base está diseñada para formateadores que principalmente utiliza read sincrónica y las operaciones de escritura. Además de mostrar el tipo de medio formateador, el ejemplo muestra cómo enlazar mediante el registro como parte de la **HttpConfiguration** para la aplicación. Tenga en cuenta que también es posible utilizar el **elemento MediaTypeFormatter** base clase directamente, para leerán los formateadores que usan principalmente asincrónica y las operaciones de escritura.

**Ejemplo de enlace de parámetro personalizado** | [descripción detallada](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Muestra cómo personalizar el proceso de enlace de parámetro, que es el proceso que determina la forma en que se enlaza la información de una solicitud a los parámetros de acción. En este ejemplo, el controlador Home tiene cuatro acciones:

1. BindPrincipal muestra cómo enlazar un parámetro de IPrincipal desde una entidad de seguridad genérico personalizado, no desde un mensaje HTTP GET;
2. BindCustomComplexTypeFromUriOrBody muestra cómo enlazar un parámetro de tipo complejo, que puede proceder del cuerpo del mensaje o el URI de solicitud de un mensaje HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty muestra cómo enlazar un parámetro de tipo complejo con una propiedad ha cambiado el nombre que se incluye el URI de solicitud de un mensaje HTTP POST;
4. PostMultipleParametersFromBody muestra cómo enlazar varios parámetros desde el cuerpo de un mensaje POST;

**Ejemplo de carga de archivos** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Muestra cómo cargar archivos a un **ApiController** mediante la carga de archivos de varias partes MIME y cómo configurar las notificaciones de progreso con **HttpClient** con **ProgressNotificationHandler**. El controlador lee el contenido de una carga de archivos HTML de forma asincrónica y escribe una o más partes de cuerpo en un archivo local. La respuesta contiene información sobre el archivo cargado (o archivos).

**Archivo de carga al ejemplo de almacén de blobs de Azure** | [descripción detallada](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Este ejemplo es similar al ejemplo de carga de archivo, pero en lugar de guardar los archivos cargados en el disco local, que se cargan asincrónicamente los archivos a [almacén de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) con [Windows Azure SDK para .NET](https://www.windowsazure.com/develop/net/). También proporciona un mecanismo para enumerar los blobs se encuentra actualmente en un [contenedor de almacenamiento de blobs de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Puede probar el ejemplo ejecutando en **emulador de almacenamiento de Azure** que viene con el SDK de Azure. Si tiene una [cuenta de almacenamiento de Azure](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), puede ejecutar en el servicio de almacenamiento real así.

**Ejemplo de canalización de controlador de mensaje HTTP** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Muestra cómo conectarlo **HttpMessageHandler** instancias en tanto el cliente (**HttpClient**) y el servidor (ASP.NET Web API). En el ejemplo, se usa el mismo controlador en el cliente y el servidor. Aunque es poco frecuente que el mismo controlador exacto se ejecutaría en ambos lugares, el modelo de objetos es el mismo en el lado cliente y el servidor.

**Ejemplo de carga de JSON** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Muestra cómo cargar y descargar JSON desde y hacia un **ApiController**. El ejemplo utiliza un mínimo **ApiController** y tiene acceso a él mediante **HttpClient**.

**Ejemplo de mashup** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Muestra cómo obtener acceso a varios sitios remotos de forma asincrónica desde una **ApiController** acción. Cada vez que se alcanza la acción, las solicitudes se realizan de forma asincrónica, por lo que no hay ningún subproceso se bloquea.

**Ejemplo de seguimiento de memoria** | [descripción detallada](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Este proyecto de ejemplo crea un paquete de Nuget que va a instalar un sistema de escritura de seguimiento personalizado de en memoria en aplicaciones ASP.NET Web API.

**Ejemplo de MongoDB** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Muestra cómo utilizar MongoDB como el almacén persistente de un **ApiController**, con un modelo de repositorio.

**Ejemplo de procesador de cuerpo de respuesta** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Muestra cómo copiar una entidad de la respuesta (es decir, un cuerpo de respuesta HTTP) en un archivo local antes de que se transmite al cliente y realizar otros procesamientos adicionales en ese archivo de forma asincrónica. El ejemplo implementa un **HttpMessageHandler** que contiene la entidad de respuesta con uno que ambos propio escribe en la salida como normal y en un archivo local.

**Cargar XDocument ejemplo** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Muestra cómo cargar un XDocument para un **ApiController** con **PushStreamContent** y **HttpClient**.

**Ejemplo de validación** | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Muestra cómo puede utilizar atributos de validación en los modelos en ASP.NET WebAPI para validar el contenido de la solicitud HTTP. Muestra cómo marcar propiedades como sea necesario, cómo se usan ambos definidas por el marco y atributos de validación personalizados para anotar el modelo y cómo devolver las respuestas de error para los Estados del modelo no válido.

**Ejemplo de formulario de Web** | [descripción detallada](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Muestra un ApiController agregado a un proyecto de formularios Web Forms.

**[Ejemplo de RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs es un aplicación que muestra cómo utilizar ASP.NET Web API y la nueva biblioteca de cliente HTTP para crear un sistema controlado por hipermedia de seguimiento de errores simple. El ejemplo incluye implementaciones de cliente y servidor, mediante ASP.NET Web API. El servidor utiliza a un formateador de Razor personalizado para generar representaciones de recursos. El ejemplo también proporciona un servidor node.js para mostrar las ventajas que proceden de mediante un diseño de hipermedia desacoplar los clientes y servidores.

## <a name="web-api-extensions-preview-samples"></a>Ejemplos de vista previa de las extensiones de API Web

**Ejemplo de OData consultable** | [descripción detallada](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Muestra cómo se presentan las consultas de OData en ASP.NET Web API con el `[Queryable]` atributo o usando la **ODataQueryOptions** parámetro de acción que permite la acción que se va a inspeccionar manualmente la consulta antes de que se está ejecutando.

La CustomerController muestra cómo utilizar el atributo [Queryable] y el OrderController muestra cómo utilizar el parámetro ODataQueryOptions. El ResponseController es similar a la CustomerController, pero en lugar de la acción de GET devuelve `IEnumerable<Customer>` devuelve un **HttpResponseMessage**. Esto nos permite agregar campos de encabezado adicional, manipular el código de estado, etc. mientras se sigue usando la funcionalidad de consulta. El ejemplo ilustra las consultas que utilizan $orderby, $skip, $top, any(), all() y $filter.

**Ejemplo de servicio de OData** | [descripción detallada](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origen de VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Este ejemplo muestra cómo crear un servicio de OData que consta de tres entidades y tres controladores de API Web. Los controladores proporcionan diversos niveles de funcionalidad en cuanto a la funcionalidad de OData que exponen:

La SupplierController expone un subconjunto de funcionalidad de consulta, obtenga clave y crear, controlando estas solicitudes:

- OBTENER /Suppliers
- OBTENER /Suppliers(key)
- GET /Suppliers?$filter=..&amp;$orderby=..&amp;$top=..&amp;$skip=..
- POST /Suppliers

Los expone ProductsController GET, PUT, POST, elimina y revisión mediante la implementación de una acción para cada una de estas operaciones directamente.

El ProductFamilesController aprovecha la clase base EntitySetController que expone un modelo útil para implementar un servicio de OData enriquecido.

Además el servicio de OData expone un documento $metadata que permite a los datos que el utilizado por los clientes de servicio de datos de WCF y otros clientes que acepta el formato de $metadata.
