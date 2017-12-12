---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Documentos de Microsoft
author: rick-anderson
description: "(abril de 2011 de incluye herramientas de actualización) ASP.NET MVC 3 es un marco para crear aplicaciones web escalable, basada en estándares con el modelo de diseño establecidos..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/05/2010
ms.topic: article
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 1aa059e92b5637b9ba7ce488da4b44322dab6d8e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(abril de 2011 de incluye herramientas de actualización)*
> 
> ASP.NET MVC 3 es un marco para crear aplicaciones web escalable, basada en estándares con hacia patrones de diseño y la eficacia de ASP.NET y .NET Framework.
> 
> Se instala en paralelo con ASP.NET MVC 2, para empezar a usarlo hoy mismo!
> 
> Descargue el [instalador aquí](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Características principales

- Sistema integrado de Scaffolding extensible a través de NuGet
- Plantillas de proyecto habilitado 5 HTML
- Vistas expresivas incluido el nuevo motor de vista Razor
- Enlaces eficaces con inserción de dependencias y los filtros de acción Global
- Compatibilidad de Rich JavaScript con JavaScript discreto, validación de jQuery y enlace de JSON
- *Leer la lista de todas las características [a continuación](#overview)*

## <a name="top-links"></a>Vínculos superiores

' S New en ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 publicado](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [MVC3 de ASP.NET, WebMatrix, NuGet, IIS Express y Orchard libera - la versión Web de enero de Microsoft en contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anunciar el lanzamiento de ASP.NET MVC 3, IIS Express, SQL CE 4, marco de granja de servidores Web, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de la versión de ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalación y la Ayuda

- ASP.NET MVC 3 se instalan con el [instalador de plataforma Web (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 se instalan con el [ejecutable del instalador](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalar [ASP.NET MVC 3 de Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leer la [Introducción a ASP.NET MVC 3 tutorial](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtener ayuda y se tratan de ASP.NET MVC 3 en el [foros](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Información general sobre ASP.NET MVC 3

ASP.NET MVC 3 se basa en ASP.NET MVC 1 y 2, agregando características que simplifican el código y permite más extensibilidad. Este tema proporciona información general de muchas de las nuevas características que se incluyen en esta versión, que se organizan en las siguientes secciones:

- [Scaffolding extensible con la integración de MvcScaffold](#BM_MvcScaffolding)
- [Plantillas de proyecto habilitado 5 HTML](#BM_HTML5)
- [El motor de vista Razor](#BM_TheRazorViewEngine)
- [Compatibilidad con varios motores de vista](#BM_Support_for_Multiple_View_Engines)
- [Mejoras de controlador](#BM_Controller_Improvements)
- [JavaScript y Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Mejoras de validación de modelo](#BM_Model_Validation_Improvements)
- [Mejoras de inyección de dependencia](#BM_Dependency_Injection_Improvements)
- [Otras características nuevas](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding extensible con la integración de MvcScaffold

El nuevo sistema de Scaffolding resulta más fácil para recoger y empezar a usar productiva si está completamente nuevo para el marco de trabajo y automatizar tareas comunes de desarrollo si tiene experiencia y ya sabe lo que está haciendo.

Esto es compatible con NuGet nueva *scaffolding* paquete denominado **MvcScaffolding**. El término "Scaffolding" se utiliza por muchas tecnologías de software para indicar "generar rápidamente un esquema básico del software que puede, a continuación, editar y personalizar". El paquete de scaffolding que estamos creando para ASP.NET MVC es beneficioso en gran medida en diversos escenarios:

- **Si está aprendiendo ASP.NET MVC por primera vez**, ya que ofrece una forma rápida de obtener un código de trabajo útil, que, a continuación, puede modificar y adaptar según sus necesidades. Evita la lesiones de búsqueda en una página en blanco y no tener sabe dónde empezar a!
- **Si conoce bien ASP.NET MVC y ahora está explorando una nueva tecnología de complemento** como un asignador relacional de objetos, un motor de vista, una biblioteca de prueba, etc., dado que el creador de esa tecnología puede ha también creado un paquete de la técnica scaffolding para él.
- **Si su trabajo implica la creación de varias veces similares clases o archivos de algún tipo**, porque se puede crear procesos scaffolding personalizados que den como resultado accesorios de prueba, los scripts de implementación o cualquier otra cosa necesita. Todos los usuarios de su equipo pueden usar a los procesos scaffolding personalizados, demasiado.

Otras características de MvcScaffolding incluyen:

- Compatibilidad con proyectos de C# y VB
- Soporte para motores de vista de la Razor y ASPX
- Admite scaffolding en áreas de ASP.NET MVC y utiliza a patrones y diseños de vista personalizada
- Puede personalizar fácilmente la salida mediante la edición de plantillas T4
- Puede agregar a completamente nuevos procesos scaffolding con lógica personalizada de PowerShell y plantillas T4 personalizadas. Estos (y los parámetros personalizados que les ha concedido) aparecen automáticamente en la lista de finalización mediante tabulador de consola.
- Puede obtener paquetes de NuGet que contienen procesos scaffolding adicionales para las distintas tecnologías (por ejemplo, hay una prueba de concepto uno de LINQ to SQL ahora) y mezclar y combinar ellos juntos

ASP.NET MVC 3 Tools Update incluye gran compatibilidad de Visual Studio para este sistema de la técnica scaffolding, tales como:

- Agregar cuadro de diálogo controlador ahora es compatible con la técnica scaffolding completa automática de creación, lectura, actualización y eliminación de las acciones de controlador y vistas correspondientes. De forma predeterminada, esto scaffolds código de acceso a datos mediante EF Code First.
- Agregar cuadro de diálogo controlador admite *scaffolds extensibles* a través de NuGet paquetes como *MvcScaffolding*. Esto permite conectarse a scaffolds personalizados en el cuadro de diálogo que le permitirá crear scaffolds para otras tecnologías de acceso a datos, como NHibernate o incluso JET con ODBCDirect si así está quieran!

Para obtener más información acerca de la técnica Scaffolding en ASP.NET MVC 3, vea los siguientes recursos:

- Steve Sanderson contabilizar series, incluidos: 

    1. [Introducción: Aplicar la técnica scaffolding el proyecto de ASP.NET MVC 3 con el paquete MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso estándar: casos de uso habitual y opciones](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relaciones uno a varios](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Las acciones de scaffolding y pruebas unitarias](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Reemplazar las plantillas T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta entrada: crear procesos scaffolding personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Post de Scott Hanselman de su sesión de PDC 2010 [compilar un Blog con Microsoft "Amor de paquete sin nombre de Web"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Plantillas de proyecto 5 HTML

El cuadro de diálogo nuevo proyecto incluye una casilla de verificación Habilitar HTML 5 las versiones de plantillas de proyecto. Estas plantillas aprovechan Modernizr 1.7 para proporcionar soporte de compatibilidad para HTML 5 y 3 de CSS en los exploradores de nivel inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>El motor de vista Razor

ASP.NET MVC 3 incluye un nuevo motor de vista denominado Razor que ofrece las siguientes ventajas:

- La sintaxis Razor es clara y concisa, que requiere un número mínimo de pulsaciones de teclas.
- Razor es fácil de aprender, en parte porque se basa en existentes lenguajes como C# y Visual Basic.
- Visual Studio incluye coloración de IntelliSense y el código para sintaxis Razor.
- Vistas de Razor pueden ser unidad probado sin necesidad de que se ejecute la aplicación o inicia un servidor web.

Algunas características nuevas de Razor son los siguientes:

- `@model`sintaxis para especificar el tipo que se pasa a la vista.
- `@* *@`sintaxis de comentario.
- La capacidad de especificar los valores predeterminados (como `layoutpage`) una vez para todo un sitio.
- El `Html.Raw` método para mostrar texto sin codificación HTML.
- Compatibilidad para compartir código entre varias vistas (*\_viewstart.cshtml* o  *\_viewstart.vbhtml* archivos).

Razor también incluye nuevos métodos auxiliares HTML, como las siguientes:

- `Chart`. Representa un gráfico, que ofrece las mismas características que el control chart en ASP.NET 4.
- `WebGrid`. Representa una cuadrícula de datos, junto con la funcionalidad de paginación y ordenación.
- `Crypto`. Utiliza algoritmos para crear correctamente hash salt y el valor hash de contraseñas.
- `WebImage`. Representa una imagen.
- `WebMail`. Envía un mensaje de correo electrónico.

Para obtener más información acerca de Razor, vea los siguientes recursos:

- [Entrada de blog de Guthrie Introducción Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Introducción a entrada de blog de Guthrie el @model (palabra clave)](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Entrada del blog de Guthrie diseños de presentación Razor](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referencia rápida de la API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Compatibilidad con varios motores de vista

El **agregar vista** cuadro de diálogo en ASP.NET MVC 3 le permite elegir el motor de vista que desea trabajar con, y el **nuevo proyecto** cuadro de diálogo permite especificar el motor de vista predeterminada para un proyecto. Puede elegir el motor de vistas de formularios Web Forms (ASPX), Razor o un motor de vista de código abierto como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Mejoras de controlador

### <a name="global-action-filters"></a>Filtros de acción global

A veces, desea realizar lógica antes de ejecutar un método de acción o después de ejecuta un método de acción. Para ello, ASP.NET MVC 2 proporciona filtros de acción. Filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar comportamiento previo y posterior a la acción a los métodos de acción de un controlador específico. Sin embargo, en algunos casos puede especificar el comportamiento de la acción anterior o posterior a la acción que se aplica a todos los métodos de acción. MVC 3 le permite especificar filtros globales agregándolos a la `GlobalFilters` colección. Para obtener más información acerca de los filtros de acción global, vea los siguientes recursos:

- [Blog de Guthrie en la vista previa de 3 de MVC](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrar en ASP.NET MVC](https://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nueva propiedad "ViewBag"

Compatibilidad con los controladores MVC 2 un `ViewData` propiedad que le permite pasar datos a una plantilla de vista con un diccionario de tiempo de ejecución API. En MVC 3, también puede utilizar sintaxis algo más sencilla con la `ViewBag` propiedad para lograr el mismo propósito. Por ejemplo, en lugar de escribir `ViewData["Message"]="text"`, puede escribir `ViewBag.Message="text"`. No es necesario definir las clases fuertemente tipadas para usar el `ViewBag` propiedad. Dado que es una propiedad dinámica, en su lugar, puede simplemente obtener o establecer las propiedades y se resolverán dinámicamente en tiempo de ejecución. Internamente, `ViewBag` propiedades se almacenan como pares de nombre/valor en el `ViewData` diccionario. (Nota: en la mayoría de las versiones preliminar de MVC 3, el `ViewBag` propiedad se denominó el `ViewModel` propiedad.)

### <a name="new-actionresult-types"></a>Nuevos tipos de "ActionResult"

El siguiente `ActionResult` tipos y métodos auxiliares correspondientes son nuevas o mejoradas en MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Devuelve un código de estado 404 de HTTP al cliente.
- [RedirectResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.redirectresult(v=VS.98).aspx). Devuelve una redirección temporal (código de estado HTTP 302) o a una redirección permanente (código de estado HTTP 301), según un parámetro booleano. Junto con este cambio, el [controlador](https://msdn.microsoft.com/en-us/library/system.web.mvc.controller(v=VS.98).aspx) clase ahora tiene tres métodos para realizar las redirecciones permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`, y `RedirectToActionPermanent`. Estos métodos devuelven una instancia de `RedirectResult` con el `Permanent` propiedad establecida en `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/en-us/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Devuelve un código de estado HTTP especificado por el usuario.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Mejoras de Ajax y JavaScript

De forma predeterminada, las aplicaciones Ajax y validación auxiliares en MVC 3 usan un enfoque de JavaScript discreto. JavaScript discreto evita insertar JavaScript alineado en HTML. Esto hace que el código HTML más pequeños y menos desordenado y resulta más fácil intercambiar o personalizar las bibliotecas de JavaScript. Aplicaciones auxiliares de validación en MVC 3 también usan el `jQueryValidate` complemento de forma predeterminada. Si desea que el comportamiento de MVC 2, puede deshabilitar el uso de JavaScript discreto un *web.config* archivo de configuración. Para obtener más información acerca de las mejoras de Ajax y JavaScript, consulte los siguientes recursos:

- [Introducción básica a JavaScript discreto en el sitio de Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Entrada de JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Entrada de validación de JavaScript discreto de Brad Wilson](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Crear una aplicación de MVC 3 con JavaScript Razor y discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial en el sitio ASP.NET)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validación del lado cliente habilitada de forma predeterminada

En versiones anteriores de MVC, debe llamar explícitamente a la `Html.EnableClientValidation` método desde una vista con el fin de habilitar la validación del lado cliente. En MVC 3 esto ya no es necesario porque la validación del lado cliente está habilitada de forma predeterminada. (Se puede deshabilitar mediante la configuración en el *web.config* archivo.)

En orden para la validación del lado cliente para que funcione, deberá hacer referencia a jQuery adecuada y las bibliotecas de validación de jQuery en su sitio. Puede hospedar dichas bibliotecas en su propio servidor o hacer referencia a ellos desde una red de entrega de contenido (CDN) como la CDN de Microsoft o de Google.

### <a name="remote-validator"></a>Validador remoto

ASP.NET MVC 3 es compatible con el nuevo [RemoteAttribute](https://msdn.microsoft.com/en-us/library/system.web.mvc.remoteattribute(v=VS.98).aspx) clase que permite aprovechar las ventajas de la validación de jQuery plug-in del soporte técnico de validador remoto. Esto permite a la biblioteca de validación del lado cliente que llame automáticamente a un método personalizado que define en el servidor con el fin de realizar la lógica de validación que solo puede realizarse del servidor.

En el ejemplo siguiente, la `Remote` atributo especifica que la validación del cliente llamará una acción denominada `UserNameAvailable` en el `UsersController` clase con el fin de validar la `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

En el ejemplo siguiente se muestra el controlador correspondiente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obtener más información sobre cómo usar el `Remote` de atributo, vea [Cómo: implementar la validación remota en ASP.NET MVC](https://msdn.microsoft.com/en-us/library/gg508808(VS.98).aspx) en MSDN library.

### <a name="json-binding-support"></a>Compatibilidad de enlace de JSON

ASP.NET MVC 3 incluye compatibilidad de enlace de JSON integrada que permite a los métodos de acción recibir datos codificados por JSON y el modelo de enlace a los parámetros de método de acción. Esta funcionalidad resulta útil en escenarios que impliquen plantillas de cliente y el enlace de datos. (Plantillas de cliente permiten aplicar formato y mostrar un único elemento de datos o un conjunto de elementos de datos mediante el uso de plantillas que se ejecutan en el cliente.) MVC 3 le permite conectarse fácilmente plantillas de cliente con métodos de acción en el servidor que envían y reciben datos JSON. Para obtener más información sobre la compatibilidad de enlace de JSON, consulte el **JavaScript y mejoras de AJAX** sección de [entrada de blog de Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Mejoras de validación de modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadatos de "DataAnnotations"

ASP.NET MVC 3 admite `DataAnnotations` atributos de metadatos como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Clase "ValidationAttribute"

El `ValidationAttribute` clase se ha mejorado en .NET Framework 4 para admitir un nuevo `IsValid` sobrecarga que proporciona más información sobre el contexto de validación actual, por ejemplo, el objeto que se está validando. Esto habilita escenarios más enriquecidos que puede validar el valor actual en función de otra propiedad del modelo. Por ejemplo, el nuevo `CompareAttribute` atributo le permite comparar los valores de dos propiedades de un modelo. En el ejemplo siguiente, la `ComparePassword` propiedad debe coincidir con el `Password` campo para que sean válidos.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validación

El [IValidatableObject](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) le permite realizar la validación de nivel de modelo, y le permite proporcionar mensajes de error que son específicos para el estado de todo el modelo, o entre dos propiedades dentro del modelo de validación . MVC 3 ahora recupera los errores de la `IValidatableObject` interfaz cuando el enlace de modelos y automáticamente marcas o resaltados afectado campos dentro de una vista con las aplicaciones auxiliares integradas formulario HTML.

El [IClientValidatable](https://msdn.microsoft.com/en-us/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfaz permite que ASP.NET MVC detectar en tiempo de ejecución si un validador tiene compatibilidad para la validación del cliente. Esta interfaz se ha diseñado para que se puede integrar con una variedad de marcos de trabajo de validación.

Para obtener más información sobre las interfaces de validación, vea la **mejoras de validación del modelo** sección de [entrada de blog de Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Sin embargo, tenga en cuenta que la referencia a "IValidateObject" en el blog de debe ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Mejoras de inyección de dependencia

ASP.NET MVC 3 ofrece compatibilidad mejorada para aplicar la inyección de dependencia (DI) y para la integración con contenedores de inversión de Control (IOC) o de inserción de dependencias. Se ha agregado compatibilidad para DI en las áreas siguientes:

- Controladores (registro e inyectar generadores de controladores, insertar controladores).
- Vistas (registro e inyectar motores de vista, inserte las dependencias en las páginas de vista).
- Filtros de acción (buscar e insertar filtros).
- Enlazadores de modelos (registro e inyectar).
- Proveedores de validación del modelo (registro e inyectar).
- Proveedores de metadatos del modelo (registro e inyectar).
- Proveedores de valores (registro e inyectar).

MVC 3 es compatible con la [localizador de servicios común](https://github.com/unitycontainer/commonservicelocator) biblioteca y cualquier contenedor de DI que admita esa biblioteca `IServiceLocator` interfaz. También admite un nuevo `IDependencyResolver` interfaz que facilita la integración DI marcos.

Para obtener más información acerca de DI en MVC 3, vea los siguientes recursos:

- [Serie de Brad Wilson de entradas de blog sobre la ubicación del servicio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Otras características nuevas

### <a name="nuget-integration"></a>Integración de NuGet

ASP.NET MVC 3 automáticamente se instala y habilita NuGet como parte de su instalación. NuGet es un administrador de paquetes de código abierto gratuito que hace más fácil buscar, instalar y usar herramientas y las bibliotecas de .NET en sus proyectos. Funciona con todos los tipos de proyecto de Visual Studio (incluidos los formularios Web Forms de ASP.NET y MVC de ASP.NET).

NuGet permite a los desarrolladores que mantienen los proyectos de código abierto (por ejemplo, los proyectos como Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks y Elmah) para empaquetar sus bibliotecas y registrarlos en una galería en línea. A continuación, es fácil para los desarrolladores de .NET que desea utilizar una de estas bibliotecas para buscar el paquete e instalarlo en los proyectos que están trabajando.

Con la actualización de herramientas de 3 de ASP.NET, plantillas de proyecto incluyen paquetes NuGet preinstalados a JavaScript bibliotecas, por lo que son actualizables a través de NuGet. Entity Framework Code First es también preinstalado como un paquete de NuGet.

Para más información sobre NuGet, consulte la [documentación correspondiente](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Caché de resultados parcial de página

Admite la caché de resultados de las respuestas de página completa desde la versión 1 de ASP.NET MVC. 3 de MVC también admite la caché de resultados de parcial de página, que le permite fácilmente las regiones de caché o fragmentos de una respuesta. Para obtener más información sobre el almacenamiento en caché, vea la **caché de resultados de página parcial** sección de [entrada de blog de Guthrie en MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) y **caché de resultados de acciónsecundarios** sección de la [notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Control granular sobre validación de solicitudes

ASP.NET MVC tiene la validación de solicitudes integrado que automáticamente le ayuda a protegerse de ataques de inyección de código XSS y HTML. Sin embargo, en ocasiones, deseará explícitamente la validación de solicitudes disable, por ejemplo, si desea que los usuarios puedan publicar contenido (por ejemplo, en las entradas de blog o contenido CMS) HTML. Ahora puede agregar un [AllowHtml](https://msdn.microsoft.com/en-us/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) de atributo a los modelos o ver los modelos para deshabilitar la validación de solicitud en una base por propiedad durante el enlace del modelo. Para obtener más información acerca de la validación de solicitud, vea los siguientes recursos:

- El **JavaScript discreto y validación** sección [entrada de blog de Guthrie en MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible "nuevo proyecto" cuadro de diálogo

En ASP.NET MVC 3 puede agregar plantillas de proyecto, los motores de vista, y marcos de trabajo proyecto de prueba unitaria el **nuevo proyecto** cuadro de diálogo.

### <a name="template-scaffolding-improvements"></a>Mejoras de Scaffolding de plantilla

Plantillas de scaffolding de ASP.NET MVC 3 es una mejor de identificar las propiedades de clave principal en los modelos y controlarlos adecuadamente que en versiones anteriores de MVC. (Por ejemplo, las plantillas de scaffolding ahora Asegúrese de que la clave principal no es scaffolding como un campo de formulario editable.)

De forma predeterminada, la creación y edición de scaffolds usan ahora el `Html.EditorFor` auxiliar en lugar de la `Html.TextBoxFor` auxiliar. Esto mejora la compatibilidad con metadatos del modelo en el formulario de datos de atributos de anotación cuando la **agregar vista** cuadro de diálogo genera una vista.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuevas sobrecargas para "Html.LabelFor" y "Html.LabelForModel"

Se han agregado nuevas sobrecargas de método para el `LabelFor` y `LabelForModel` métodos auxiliares. Las nuevas sobrecargas permiten especificar o reemplazar el texto del rótulo.

### <a name="sessionless-controller-support"></a>Compatibilidad del controlador sin sesión

En ASP.NET MVC 3, puede indicar si desea que una clase de controlador para usar el estado de sesión y si es así, si el estado de sesión debe ser lectura/escritura o de solo lectura. Para obtener más información sobre la compatibilidad de controlador sin sesión, vea [notas de la versión de MVC 3](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nueva clase de "AdditionalMetadataAttribute"

Puede usar el [AdditionalMetadata](https://msdn.microsoft.com/en-us/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atributos para rellenar el `ModelMetadata.AdditionalValues` diccionario para una propiedad de modelo. Por ejemplo, si un modelo de vista tiene una propiedad que se debe mostrar sólo a un administrador, puede anotar esa propiedad tal y como se muestra en el ejemplo siguiente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Estos metadatos debe ponerse a disposición a cualquier plantilla de pantalla o editor cuando se procesa un modelo de vista de producto. Depende de usted para interpretar la información de metadatos.

### <a name="accountcontroller-improvements"></a>Mejoras de AccountController

Se ha mejorado en gran medida la AccountController en la plantilla de proyecto de Internet.

### <a name="new-intranet-project-template"></a>Nueva plantilla de proyecto de Intranet

Se incluye una nueva plantilla de proyecto de Intranet que habilita la autenticación de Windows y quita el AccountController.
