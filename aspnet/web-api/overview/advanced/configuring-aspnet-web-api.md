---
uid: web-api/overview/advanced/configuring-aspnet-web-api
title: "Configuración de ASP.NET Web API 2 | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/31/2014
ms.topic: article
ms.assetid: 9e10a700-8d91-4d2e-a31e-b8b569fe867c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/configuring-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 1c007c4c327b7cde6ff52c6b0022acdff3c9b137
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="configuring-aspnet-web-api-2"></a>Configuración de ASP.NET Web API 2
====================
por [Mike Wasson](https://github.com/MikeWasson)

En este tema se describe cómo configurar ASP.NET Web API.

- [Opciones de configuración](#settings)
- [Configuración de Web API con el hospedaje de ASP.NET](#webhost)
- [Configuración de Web API con OWIN hospeda a sí mismo](#selfhost)
- [Servicios de API Web global](#services)
- [Configuración por controlador](#percontrollerconfig)

<a id="settings"></a>
## <a name="configuration-settings"></a>Opciones de configuración

Configuración de la configuración de Web API se define en el [HttpConfiguration](https://msdn.microsoft.com/en-us/library/system.web.http.httpconfiguration.aspx) clase.

| Miembro | Descripción |
| --- | --- |
| **DependencyResolver** | Permite la inserción de dependencias para los controladores. Vea [mediante la resolución de dependencia de la API Web](dependency-injection.md). |
| **Filtros** | Filtros de acción. |
| **Formateadores** | [Formateadores de tipo de medio](../formats-and-model-binding/media-formatters.md). |
| **IncludeErrorDetailPolicy** | Especifica si el servidor debe incluir detalles del error, como los mensajes de excepción y los seguimientos de pila, en mensajes de respuesta HTTP. Vea [IncludeErrorDetailPolicy](https://msdn.microsoft.com/en-us/library/system.web.http.includeerrordetailpolicy(v=vs.108)). |
| **Inicializador** | Una función que realiza la inicialización final de la **HttpConfiguration**. |
| **MessageHandlers** | [Controladores de mensajes HTTP](http-message-handlers.md). |
| **ParameterBindingRules** | Una colección de reglas para enlazar parámetros en acciones del controlador. |
| **Propiedades** | Un contenedor de propiedades genéricas. |
| **Rutas** | La colección de rutas. Vea [enrutar en ASP.NET Web API](../web-api-routing-and-actions/routing-in-aspnet-web-api.md). |
| **Servicios de** | La colección de servicios. Vea [Services](#services). |


## <a name="prerequisites"></a>Requisitos previos

[Visual Studio de 2017](https://www.visualstudio.com/vs/) Community, Professional o Enterprise Edition.

<a id="webhost"></a>
## <a name="configuring-web-api-with-aspnet-hosting"></a>Configuración de Web API con el hospedaje de ASP.NET

En una aplicación ASP.NET, configurar la API Web mediante una llamada a [GlobalConfiguration.Configure](https://msdn.microsoft.com/en-us/library/system.web.http.globalconfiguration.configure.aspx) en el **aplicación\_iniciar** método. El **configurar** método toma un delegado con un único parámetro de tipo **HttpConfiguration**. Realizar todas las de su configuración en el delegado.

Este es un ejemplo de cómo utilizar a un delegado anónimo:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample1.cs)]

En Visual Studio de 2017, la plantilla de proyecto de "Aplicación Web de ASP.NET" configura automáticamente el código de configuración, si selecciona "API Web" en la **nuevo proyecto ASP.NET** cuadro de diálogo.

[![](configuring-aspnet-web-api/_static/image2.png)](configuring-aspnet-web-api/_static/image1.png)

La plantilla de proyecto crea un archivo denominado WebApiConfig.cs dentro de la aplicación\_carpeta de inicio. Este archivo de código define al delegado que se debe colocar el código de configuración de API Web.

![](configuring-aspnet-web-api/_static/image3.png)

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample2.cs?highlight=12)]

La plantilla de proyecto también agrega el código que llama al delegado de **aplicación\_iniciar**.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample3.cs?highlight=5)]

<a id="selfhost"></a>
## <a name="configuring-web-api-with-owin-self-hosting"></a>Configuración de Web API con OWIN hospeda a sí mismo

Si estás autohospedaje con OWIN, cree un nuevo **HttpConfiguration** instancia. Realizar la configuración de esta instancia y, a continuación, pasar la instancia a la **Owin.UseWebApi** método de extensión.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample4.cs)]

El tutorial [OWIN de uso a Self-Host ASP.NET Web API 2](../hosting-aspnet-web-api/use-owin-to-self-host-web-api.md) muestra todos los pasos.

<a id="services"></a>
## <a name="global-web-api-services"></a>Servicios de API Web global

El **HttpConfiguration.Services** colección contiene un conjunto de servicios globales que API Web se utiliza para realizar diversas tareas, como la negociación de selección y el contenido del controlador.

> [!NOTE]
> El **servicios** colección no es un mecanismo de uso general para la inyección de dependencia o detección de servicio. Solo almacena los tipos de servicio que se sabe que el marco Web API.


El **servicios** colección se inicializa con un conjunto predeterminado de servicios y puede proporcionar sus propias implementaciones personalizadas. Algunos servicios admiten varias instancias, mientras que otros usuarios puedan tener sólo una instancia. (Sin embargo, también puede proporcionar servicios en el nivel de controlador; vea [configuración por controlador](#percontrollerconfig).

Servicios de instancia única


| Servicio | Descripción |
| --- | --- |
| **IActionValueBinder** | Obtiene un enlace para un parámetro. |
| **IApiExplorer** | Obtiene las descripciones de las API expuestas por la aplicación. Vea [crear una página de Ayuda de una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IAssembliesResolver** | Obtiene una lista de los ensamblados de la aplicación. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IBodyModelValidator** | Valida un modelo que se lee desde el cuerpo de solicitud por un formateador de tipo de medio. |
| **IContentNegotiator** | Realiza la negociación de contenido. |
| **IDocumentationProvider** | Proporciona documentación de API. El valor predeterminado es **null**. Vea [crear una página de Ayuda de una API Web](../getting-started-with-aspnet-web-api/creating-api-help-pages.md). |
| **IHostBufferPolicySelector** | Indica si el host debe almacenar en búfer cuerpos de entidad de mensaje HTTP. |
| **IHttpActionInvoker** | Se invoca una acción de controlador. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpActionSelector** | Selecciona una acción de controlador. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerActivator** | Activa un controlador. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerSelector** | Selecciona un controlador. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **IHttpControllerTypeResolver** | Proporciona una lista de los tipos de controlador de API Web en la aplicación. Vea [enrutamiento y selección de acción](../web-api-routing-and-actions/routing-and-action-selection.md). |
| **ITraceManager** | Inicializa el marco de trabajo de seguimiento. Vea [seguimiento en ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **ITraceWriter** | Proporciona un escritor de seguimiento. El valor predeterminado es un sistema de escritura de seguimiento "no-op". Vea [seguimiento en ASP.NET Web API](../testing-and-debugging/tracing-in-aspnet-web-api.md). |
| **IModelValidatorCache** | Proporciona una caché de validadores de modelo. |

Servicios de varias instancias


| Servicio | Descripción |
| --- | --- |
| **IFilterProvider** | Devuelve una lista de filtros para una acción de controlador. |
| **ModelBinderProvider** | Devuelve un enlazador de modelos para un tipo determinado. |
| **ModelMetadataProvider** | Proporciona los metadatos para un modelo. |
| **ModelValidatorProvider** | Proporciona un validador para un modelo. |
| **ValueProviderFactory** | Crea un proveedor de valores. Para obtener más información, consulte el blog de Mike Stall [cómo crear un proveedor de valor personalizado en WebAPI](https://blogs.msdn.com/b/jmstall/archive/2012/04/23/how-to-create-a-custom-value-provider-in-webapi.aspx) |.

Para agregar una implementación personalizada a un servicio de varias instancias, llame **agregar** o **insertar** en el **servicios** colección:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample5.cs)]

Para reemplazar un servicio de instancia única con una implementación personalizada, llame **reemplazar** en el **servicios** colección:

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample6.cs)]

<a id="percontrollerconfig"></a>
## <a name="per-controller-configuration"></a>Configuración por controlador

Puede invalidar la configuración siguiente en una base por cada controlador:

- Formateadores de tipo de medio
- Reglas de enlace de parámetro
- Servicios

Para ello, defina un atributo personalizado que implementa la **IControllerConfiguration** interfaz. A continuación, aplique el atributo al controlador.

En el ejemplo siguiente se reemplaza a los formateadores de tipo de medio predeterminado con un formateador personalizado.

[!code-csharp[Main](configuring-aspnet-web-api/samples/sample7.cs)]

El **IControllerConfiguration.Initialize** método toma dos parámetros:

- Un **HttpControllerSettings** objeto
- Un **HttpControllerDescriptor** objeto

El **HttpControllerDescriptor** contiene una descripción del controlador, que se puede examinar con carácter informativo (por ejemplo, para distinguir entre los dos controladores).

Use la **HttpControllerSettings** objeto que se va a configurar el controlador. Este objeto contiene el subconjunto de los parámetros de configuración que se pueden invalidar en una base por cada controlador. Cualquier configuración que no cambie de forma predeterminada global **HttpConfiguration** objeto.
