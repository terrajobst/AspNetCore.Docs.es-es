---
uid: web-api/samples-list
title: Lista de ejemplos de API Web | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/18/2012
ms.topic: article
ms.assetid: 8cbd9d7f-7027-4390-b098-cb81a63ecd6f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/samples-list
msc.type: content
ms.openlocfilehash: 6c9fff024df6c485b8d904d39cb0a4e5b5bf7e44
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362569"
---
<a name="web-api-samples-list"></a>Lista de ejemplos de API Web
====================
## <a name="httpclient-samples"></a>Ejemplos de HttpClient

**Ejemplo de traducir de Bing** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fBingTranslateSample%2fReadMe.txt)

Muestra cómo llamar a la [servicio Microsoft Translator](https://msdn.microsoft.com/library/ff512419.aspx) utilizando el **HttpClient** clase. La API del servicio Microsoft Translator requiere un token de OAuth, que obtiene de la aplicación, envíe una solicitud al servidor símbolo (token) de Azure para cada solicitud para el servicio del traductor. El resultado desde el servidor testigo se introduce en la solicitud enviada al servicio de traducción. Antes de ejecutar este ejemplo, debe obtener un [clave de aplicación de Azure Marketplace](https://msdn.microsoft.com/library/hh454950.aspx) y rellene la información de la clase de ejemplo AccessTokenMessageHandler.

**Ejemplo de Google Maps** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/downloading-a-google-map-to-local-file.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fGoogleMapsSample%2fReadMe.txt)

Usa **HttpClient** para descargar un mapa de Redmond, WA de [API de Google Maps](https://developers.google.com/maps/), lo guarda como un archivo local y se abre el Visor de imágenes de forma predeterminada.

**Ejemplo de cliente de Twitter** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/extending-httpclient-with-oauth-to-access-twitter.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fTwitterSample%2fReadMe.txt)

Muestra cómo escribir un sencillo cliente de Twitter mediante **HttpClient**. El ejemplo usa un **HttpMessageHandler** para insertar información de autenticación de OAuth en la salida **HttpRequestMessage**. El resultado de Twitter se lee con JSON.NET. Antes de ejecutar este ejemplo, debe obtener un [clave de aplicación de Twitter](https://dev.twitter.com/)y rellene la información de la clase de ejemplo OAuthMessageHandler.

**Ejemplo de banco mundial** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/16/httpclient-is-here.aspx) | [VS 2010 origen](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fHttpClient%2fWorldBankSample%2fReadMe.txt)

Muestra cómo recuperar datos desde el sitio de datos del banco mundial, con JSON.NET para analizar el resultado.

## <a name="web-api-samples"></a>Ejemplos de API Web

**Introducción a ASP.NET Web API** | [origen VS 2012](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)

Muestra cómo crear una API que admite solicitudes HTTP GET de web básica. Contiene el código fuente para el tutorial [su primera API Web de ASP.NET](overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md).

**Escenarios de ASP.NET Web API JavaScript: comentarios** | [origen VS 2012](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7)

Se muestra cómo usar ASP.NET Web API para crear API web que admiten a clientes de explorador y se pueden llamar fácilmente mediante jQuery.

**Póngase en contacto con el Administrador de** | [origen VS 2010](https://code.msdn.microsoft.com/Contact-Manager-Web-API-0e8e373d)

Este ejemplo utiliza la API Web de ASP.NET para crear una aplicación de administrador de contactos sencilla. La aplicación consta de un administrador de contactos API web que utiliza una aplicación ASP.NET MVC y una aplicación de Windows Phone para mostrar y administrar una lista de contactos.

**Ejemplo de procesamiento por lotes** | [una descripción detallada de](http://trocolate.wordpress.com/2012/07/19/mitigate-issue-260-in-batching-scenario/) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fHostedBatchSample%2fReadMe.txt)

Muestra cómo implementar el procesamiento por lotes HTTP en ASP.NET. El procesamiento por lotes se compone de colocar varias solicitudes HTTP dentro de un cuerpo de entidad de varias partes MIME único, que, a continuación, se envía al servidor como una solicitud HTTP POST. Las solicitudes se procesan de forma individual y las respuestas se colocan en otro cuerpo de entidad de varias partes MIME, que se devuelve al cliente.

**Ejemplo de controlador de contenido** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/24/async-actions-in-asp-net-web-api.aspx) | [VS 2010 origen](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fContentControllerSample%2fReadMe.txt)

Se muestra cómo leer y escribir entidades de solicitud y respuesta asincrónicamente mediante secuencias. El controlador de ejemplo tiene dos acciones: una acción de PUT que lee de forma asincrónica el cuerpo de entidad de solicitud y lo almacena en un archivo local y una acción GET que devuelve el contenido del archivo local.

**Ejemplo de resolución de ensamblado personalizado** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fCustomAssemblyResolverSample%2fReadMe.txt)

Muestra cómo modificar la API Web de ASP.NET para admitir la detección de controladores de un ensamblado de biblioteca cargada dinámicamente. El ejemplo implementa un personalizado **IAssembliesResolver** que llama a la implementación predeterminada y, a continuación, agrega el ensamblado de biblioteca a los resultados de forma predeterminada.

**Ejemplo de formateador de tipo multimedia personalizado** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/04/23/using-cookies-with-asp-net-web-api.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomMediaTypeFormatterSample%2fReadMe.txt)

Muestra cómo crear un formateador de tipo de medio personalizado mediante la **BufferedMediaTypeFormatter** clase base. Esta clase base está destinada a los formateadores que principalmente utiliza read sincrónica y las operaciones de escritura. Además de mostrar el tipo de medio formateador, el ejemplo muestra cómo enlazar registrándose como parte de la **HttpConfiguration** para su aplicación. Tenga en cuenta que también es posible usar el **elemento MediaTypeFormatter** base clase directamente, para formateadores que usan principalmente asincrónicas de lectura y escritura de las operaciones.

**Ejemplo de enlace de parámetro personalizado** | [una descripción detallada de](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fCustomParameterBinding%2fReadMe.txt)

Muestra cómo personalizar el proceso de enlace de parámetro, que es el proceso que determina cómo se enlaza la información de una solicitud a parámetros de acción. En este ejemplo, el controlador Home tiene cuatro acciones:

1. BindPrincipal se muestra cómo enlazar un parámetro de IPrincipal desde una entidad de seguridad genérico personalizada, no desde un mensaje HTTP GET;
2. BindCustomComplexTypeFromUriOrBody se muestra cómo enlazar un parámetro de tipo complejo, que podría provenir del cuerpo del mensaje o el URI de solicitud de un mensaje HTTP POST;
3. BindCustomComplexTypeFromUriWithRenamedProperty se muestra cómo enlazar un parámetro de tipo complejo con una propiedad ha cambiado el nombre que se incluye el URI de solicitud de un mensaje HTTP POST;
4. PostMultipleParametersFromBody se muestra cómo enlazar varios parámetros desde el cuerpo de un mensaje POST;

**Ejemplo de carga de archivos** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/03/01/file-upload-and-asp-net-web-api.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fFileUploadSample%2fReadMe.txt)

Se muestra cómo cargar archivos a un **ApiController** mediante la carga de archivos de varias partes MIME y cómo configurar las notificaciones de progreso con **HttpClient** mediante **ProgressNotificationHandler**. El controlador lee el contenido de una carga de archivos HTML de forma asincrónica y escribe una o varias partes del cuerpo en un archivo local. La respuesta contiene información sobre el archivo cargado (o archivos).

**A Azure Blob Store muestra la carga de archivos** | [una descripción detallada de](https://blogs.msdn.com/b/yaohuang1/archive/2012/07/02/asp-net-web-api-and-azure-blob-storage.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/61dfed023e50#Samples%2fNet45%2fCS%2fWebApi%2fAzureBlobsFileUploadSample%2fReadMe.txt)

Este ejemplo es similar del ejemplo de carga de archivo, pero en lugar de guardar los archivos cargados en el disco local, cargan de manera asincrónica los archivos a [Azure Blob Store](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs) mediante [Windows Azure SDK para .NET](https://www.windowsazure.com/develop/net/). También proporciona un mecanismo para enumerar los blobs que se encuentra actualmente en un [contenedor de Azure Blob Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs). Puede probar el ejemplo se ejecuta contra **emulador de Azure Storage** que se incluye con el SDK de Azure. Si tiene un [cuenta de Azure Storage](https://docs.microsoft.com/azure/storage/blobs/storage-dotnet-how-to-use-blobs), puede ejecutar en el servicio de almacenamiento real también.

**Ejemplo de canalización de controlador de mensajes HTTP** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/08/07/httpclient-httpclienthandler-and-httpwebrequesthandler.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fHttpMessageHandlerPipelineSample%2fReadMe.txt)

Se muestra cómo conectar **HttpMessageHandler** instancias en el cliente (**HttpClient**) y el servidor (ASP.NET Web API). En el ejemplo, se usa el mismo controlador en el cliente y el servidor. Aunque es poco frecuente que ejecutaría el mismo controlador exacto en ambos lugares, el modelo de objetos es el mismo lado del cliente y servidor.

**Ejemplo de carga de JSON** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fJsonUploadSample%2fReadMe.txt)

Se muestra cómo cargar y descargar JSON desde y hacia un **ApiController**. El ejemplo utiliza un mínimo **ApiController** y tiene acceso a ella mediante **HttpClient**.

**Ejemplo de mashup** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/03/03/async-mashups-using-asp-net-web-api.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMashupSample%2fReadMe.txt)

Muestra cómo obtener acceso a varios sitios remotos de forma asincrónica desde una **ApiController** acción. Cada vez que se alcance la acción, las solicitudes se realizan de forma asincrónica, por lo que se bloquea ningún subproceso.

**Ejemplo de seguimiento de la memoria** | [una descripción detallada de](https://blogs.msdn.com/b/roncain/archive/2012/04/12/tracing-in-asp-net-web-api.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fMemoryTracingSample%2fReadMe.txt)

Este proyecto de ejemplo crea un paquete de Nuget que se instalará a un escritor de seguimiento personalizado de en memoria en aplicaciones de ASP.NET Web API.

**Ejemplo de MongoDB** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/19/using-web-api-with-mongodb.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fMongoSample%2fReadMe.txt)

Se muestra cómo usar MongoDB como almacén persistente para un **ApiController**, con un patrón de repositorio.

**Ejemplo de procesador de cuerpo de respuesta** | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fResponseEntityProcessorSample%2fReadMe.txt)

Muestra cómo copiar una entidad de respuesta (es decir, un cuerpo de respuesta HTTP) en un archivo local antes de transmitirse al cliente y realizar un procesamiento adicional en ese archivo de forma asincrónica. El ejemplo implementa un **HttpMessageHandler** que contiene la entidad de respuesta con uno que ambos propio escribe en la salida como normal y en un archivo local.

**Cargar muestra de XDocument** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/17/push-and-pull-streams-using-httpclient.aspx) | [origen VS 2012](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet45%2fCS%2fWebApi%2fUploadXDocumentSample%2fReadMe.txt)

Se muestra cómo cargar un XDocument para un **ApiController** mediante **PushStreamContent** y **HttpClient**.

**Ejemplo de validación** | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fValidationSample%2fReadMe.txt)

Muestra cómo puede utilizar atributos de validación en los modelos en ASP.NET WebAPI para validar el contenido de la solicitud HTTP. Muestra cómo marcar las propiedades según sea necesario, cómo usar ambas definido por el marco de trabajo y atributos de validación personalizados para anotar el modelo y cómo se devuelven las respuestas de error para los Estados de modelo no válido.

**Ejemplo de formulario de Web** | [una descripción detallada de](https://blogs.msdn.com/b/henrikn/archive/2012/02/23/using-asp-net-web-api-with-asp-net-web-forms.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fWebFormSample%2fReadMe.txt)

Se muestra un controlador ApiController agregado a un proyecto de formularios Web Forms.

**[Ejemplo de RestBugs](https://github.com/howarddierking/RestBugs)**

RestBugs es una aplicación que se muestra cómo usar ASP.NET Web API y la nueva biblioteca de cliente HTTP para crear un sistema de hipermedia de seguimiento de errores simple. El ejemplo incluye implementaciones de cliente y servidor, con ASP.NET Web API. El servidor utiliza a un formateador personalizado de Razor para generar representaciones de recursos. El ejemplo también proporciona un servidor de node.js para mostrar las ventajas que proceden del uso de un diseño de hipermedia para desacoplar los clientes y servidores.

## <a name="web-api-extensions-preview-samples"></a>Ejemplos de vista previa de extensiones de API Web

**Ejemplo consultable de OData** | [una descripción detallada de](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataQueryableSample%2fReadMe.txt)

Se muestra cómo se presentan las consultas de OData en ASP.NET Web API mediante el `[Queryable]` atributo o mediante el **ODataQueryOptions** parámetro de acción que permite la acción que se va a inspeccionar manualmente la consulta antes de que se está ejecutando.

El controlador muestra cómo utilizar el atributo [Queryable] y el OrderController muestra cómo usar el parámetro ODataQueryOptions. El ResponseController es similar para el controlador, pero en lugar de la acción GET devuelve `IEnumerable<Customer>` devuelve un **HttpResponseMessage**. Esto nos permite agregar campos de encabezado adicional, manipular el código de estado, etc. mientras sigue usando la funcionalidad de consulta. El ejemplo muestra las consultas mediante $filter, $skip, $top, any(), all() y $orderby.

**Ejemplo de servicio de OData** | [una descripción detallada de](https://blogs.msdn.com/b/alexj/archive/2012/08/15/odata-support-in-asp-net-web-api.aspx) | [origen VS 2010](http://aspnet.codeplex.com/SourceControl/changeset/view/15dfe7e0759f#Samples%2fNet4%2fCS%2fWebApi%2fODataServiceSample%2fReadMe.txt)

Este ejemplo muestra cómo crear un servicio de OData que consta de tres entidades y tres controladores de API Web. Los controladores proporcionan diversos niveles de funcionalidad en cuanto a la funcionalidad de OData que exponen:

La SupplierController expone un subconjunto de funcionalidad de consulta, obtener por su clave y crear, al controlar estas solicitudes:

- OBTENER /Suppliers
- OBTENER /Suppliers(key)
- GET /Suppliers? $filter =... &amp;$orderby =... &amp;$top =... &amp;$skip =...
- POST /Suppliers

El expone ProductsController GET, PUT, POST, DELETE y revisión mediante la implementación de una acción para cada una de estas operaciones directamente.

El ProductFamilesController aprovecha la clase base de EntitySetController que expone un modelo útil para implementar un servicio OData enriquecido.

Además, el servicio de OData expone un documento $metadata, lo que permite a los datos que el utilizado por los clientes de WCF Data Service y otros clientes que aceptan el formato de $metadata.
