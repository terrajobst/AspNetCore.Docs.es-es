---
uid: mvc/mvc3
title: ASP.NET MVC 3 | Microsoft Docs
author: rick-anderson
description: (abril de 2011 de incluye herramientas de actualización) ASP.NET MVC 3 es un marco para crear aplicaciones web escalable y basada en estándares con el patrón de diseño estandarizados...
ms.author: riande
ms.date: 10/05/2010
ms.assetid: dddc8812-a0bc-49f9-aafb-caf2064c2b8c
msc.legacyurl: /mvc/mvc3
msc.type: content
ms.openlocfilehash: 82d18865815568c5df9768fd9dd403f11ebd1714
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829043"
---
<a name="aspnet-mvc-3"></a>ASP.NET MVC 3
====================
> *(abril de 2011 de incluye herramientas de actualización)*
> 
> ASP.NET MVC 3 es un marco para crear aplicaciones web escalable y basada en estándares con los patrones de diseño bien establecido y la eficacia de ASP.NET y .NET Framework.
> 
> Se instala en paralelo con ASP.NET MVC 2, para empezar a usarlo hoy mismo!
> 
> Descargue el [instalador desde aquí](https://go.microsoft.com/fwlink/?LinkID=208140)


## <a name="top-features"></a>Características principales

- Sistema de Scaffolding integrado extensible a través de NuGet
- Plantillas de proyecto habilitado 5 HTML
- Vistas expresivas como el nuevo motor de vistas Razor
- Enlaces eficaces con inserción de dependencias y los filtros de acción Global
- Soporte técnico de Rich JavaScript con enlace de JSON, la validación de jQuery y JavaScript discreto
- *Leer la lista de todas las características [debajo](#overview)*

## <a name="top-links"></a>Vínculos superiores

Novedades de ASP.NET MVC 3

- Phil Haack: [ASP.NET MVC 3 Released](http://haacked.com/archive/2011/01/13/aspnetmvc3-released.aspx)
- Scott Hanselman: [MVC3 de ASP.NET, WebMatrix, NuGet, IIS Express y Orchard publicado: la versión Web de enero de Microsoft en contexto](http://www.hanselman.com/blog/ASPNETMVC3WebMatrixNuGetIISExpressAndOrchardReleasedTheMicrosoftJanuaryWebReleaseInContext.aspx)
- Scott Guthrie: [anuncio de versión de ASP.NET MVC 3, IIS Express, SQL CE 4, marco de granja de servidores Web, Orchard, WebMatrix](https://weblogs.asp.net/scottgu/archive/2011/01/13/announcing-release-of-asp-net-mvc-3-iis-express-sql-ce-4-web-farm-framework-orchard-webmatrix.aspx)
- [Notas de la versión de ASP.NET MVC 3](../whitepapers/mvc3-release-notes.md)

Instalación y la Ayuda

- ASP.NET MVC 3 se instalan con el [instalador de plataforma Web (recomendado)](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MVC3)
- ASP.NET MVC 3 se instalan con el [instalador ejecutable](https://go.microsoft.com/fwlink/?LinkID=208140)
- Instalar [ASP.NET MVC 3 para Visual Studio 11 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=208140)
- Leer la [Introducción al tutorial de ASP.NET MVC 3](overview/older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)
- Obtener ayuda y comente ASP.NET MVC 3 en el [foros](https://forums.asp.net/1146.aspx)

<a id="overview"></a>
## <a name="aspnet-mvc-3-overview"></a>Información general sobre ASP.NET MVC 3

ASP.NET MVC 3 se basa en ASP.NET MVC 1 y 2, agregando características excelentes que simplificarán su código y permite más extensibilidad. En este tema se proporciona información general de muchas de las nuevas características que se incluyen en esta versión, que se organiza en las secciones siguientes:

- [Scaffolding extensible con la integración de MvcScaffold](#BM_MvcScaffolding)
- [Plantillas de proyecto habilitado 5 HTML](#BM_HTML5)
- [El motor de vistas Razor](#BM_TheRazorViewEngine)
- [Compatibilidad con varios motores de vista](#BM_Support_for_Multiple_View_Engines)
- [Mejoras de controlador](#BM_Controller_Improvements)
- [JavaScript y Ajax](#BM_JavaScript_and_Ajax_Improvements)
- [Mejoras de validación del modelo](#BM_Model_Validation_Improvements)
- [Mejoras de inyección de dependencia](#BM_Dependency_Injection_Improvements)
- [Otras características nuevas](#BM_Other_New_Features)

<a id="BM_MvcScaffolding"></a>

## <a name="extensible-scaffolding-with-mvcscaffold-integration"></a>Scaffolding extensible con la integración de MvcScaffold

El nuevo sistema de Scaffolding facilita para recoger y empezar a usar de manera productiva si está completamente familiarizado con el marco de trabajo y automatizar tareas comunes de desarrollo si tiene experiencia y ya sabe lo que está haciendo.

Esto es compatible con NuGet nuevo *scaffolding* paquete llamado **MvcScaffolding**. El término "Scaffolding" se utiliza por muchas tecnologías de software para indicar "generar rápidamente una descripción básica del software que puede, a continuación, editar y personalizar". El paquete de scaffolding que estamos creando para ASP.NET MVC tiene muchas ventajas en varios escenarios:

- **Si está aprendiendo MVC de ASP.NET por primera vez**, ya que proporciona una manera más rápida para algunos útil, código de trabajo, que, a continuación, puede editar y adaptar según sus necesidades. Le evita las lesiones de mirando una página en blanco y no tener ninguna idea de dónde empezar.
- **Si conoce bien ASP.NET MVC y ahora está explorando una nueva tecnología de complemento** como un mapeador relacional de objetos, un motor de vista, una biblioteca de pruebas, etc., ya que el creador de esa tecnología puede crear un paquete de scaffolding para él.
- **Si su trabajo implica la creación de varias veces similares las clases o archivos de algún tipo**, ya que puede crear procesos scaffolding personalizados que den como resultado accesorios de prueba, los scripts de implementación o cualquier otra cosa que necesite. Todos los usuarios de su equipo pueden usar a los procesos scaffolding personalizados, demasiado.

Otras características de MvcScaffolding incluyen:

- Compatibilidad con proyectos de C# y VB
- Soporte técnico para los motores de vista de la Razor y ASPX
- Admite scaffolding en las áreas de ASP.NET MVC y utiliza a patrones y diseños de vista personalizada
- Puede personalizar fácilmente la salida mediante la edición de plantillas T4
- Puede agregar a scaffolders completamente nuevos con lógica personalizada de PowerShell y plantillas T4 personalizadas. Estos (y los parámetros personalizados que les ha concedido) aparecen automáticamente en la lista de finalización con tabulación de la consola.
- Puede obtener paquetes de NuGet que contienen procesos adicionales para diferentes tecnologías (por ejemplo, hay una prueba de concepto uno para LINQ to SQL ahora) y mezclar y combinar ellos juntos

ASP.NET MVC 3 Tools Update incluye excelente compatibilidad de Visual Studio para este sistema de scaffolding, tales como:

- Agregar que cuadro de diálogo controlador ahora admite scaffolding automática completa de creación, lectura, actualización y eliminación de las acciones de controlador y las vistas correspondientes. De forma predeterminada, se aplica la técnica scaffolding código de acceso a datos con EF Code First.
- Agregar cuadro de diálogo controlador admite *scaffolds extensibles* a través de NuGet, como paquetes *MvcScaffolding*. Esto permite integrar scaffolding personalizado en el cuadro de diálogo que le permite crear el scaffolding para otras tecnologías de acceso de datos como NHibernate o incluso JET con ODBCDirect si le interesa.

Para obtener más información acerca de la técnica Scaffolding en ASP.NET MVC 3, vea los siguientes recursos:

- Steve Sanderson registrar la serie, que incluye: 

    1. [Introducción: Aplicar la técnica scaffolding su proyecto de ASP.NET MVC 3 con el paquete MvcScaffolding](http://blog.stevensanderson.com/2011/01/13/scaffold-your-aspnet-mvc-3-project-with-the-mvcscaffolding-package/)
    2. [Uso estándar: casos de uso habitual y opciones](http://blog.stevensanderson.com/2011/01/13/mvcscaffolding-standard-usage/)
    3. [Relaciones uno a varios](http://blog.stevensanderson.com/2011/01/28/mvcscaffolding-one-to-many-relationships/)
    4. [Las acciones de scaffolding y pruebas unitarias](http://blog.stevensanderson.com/2011/03/28/scaffolding-actions-and-unit-tests-with-mvcscaffolding/)
    5. [Reemplazar las plantillas T4](http://blog.stevensanderson.com/2011/04/06/mvcscaffolding-overriding-the-t4-templates/)
    6. [Esta publicación: crear procesos scaffolding personalizados](http://blog.stevensanderson.com/2011/04/07/mvcscaffolding-creating-custom-scaffolders/)
- Post de Scott Hanselman desde su sesión PDC 2010 [creación de un Blog con Microsoft "Amor de paquete sin nombre de Web"](http://www.hanselman.com/blog/PDC10BuildingABlogWithMicrosoftUnnamedPackageOfWebLove.aspx)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_HTML5"></a>

## <a name="html-5-project-templates"></a>Plantillas de proyecto 5 HTML

El cuadro de diálogo nuevo proyecto incluye una casilla de verificación Habilitar HTML 5 las versiones de plantillas de proyecto. Estas plantillas aprovechan a Modernizr 1.7 para proporcionar compatibilidad de HTML 5 y 3 de CSS en los exploradores de nivel inferior.

<a id="BM_TheRazorViewEngine"></a>

## <a name="the-razor-view-engine"></a>El motor de vistas Razor

ASP.NET MVC 3 incluye un nuevo motor de vista denominado Razor que ofrece las siguientes ventajas:

- Sintaxis de Razor es clara y concisa, que requieren un número mínimo de pulsaciones de teclas.
- Razor es fácil de aprender, en parte porque se basa en existentes lenguajes como C# y Visual Basic.
- Visual Studio incluye IntelliSense y el código coloración de sintaxis Razor.
- Las vistas de Razor puede realizar pruebas unitarias sin necesidad de que ejecute la aplicación o iniciar un servidor web.

Algunas características nuevas de Razor incluyen lo siguiente:

- `@model` sintaxis para especificar el tipo que se pasa a la vista.
- `@* *@` sintaxis de comentario.
- La capacidad de especificar los valores predeterminados (como `layoutpage`) una vez para todo el sitio.
- El `Html.Raw` método para mostrar el texto sin codificación HTML.
- Soporte técnico para compartir código entre varias vistas (*\_viewstart.cshtml* o  *\_viewstart.vbhtml* archivos).

Razor también incluye nuevas aplicaciones auxiliares HTML, como las siguientes:

- `Chart`. Representa un gráfico, que ofrece las mismas características que el control chart en ASP.NET 4.
- `WebGrid`. Representa una cuadrícula de datos, junto con la funcionalidad de paginación y ordenación.
- `Crypto`. Utiliza un algoritmo hash algoritmos para crear correctamente cifradas con sal y el valor hash de contraseñas.
- `WebImage`. Presenta una imagen.
- `WebMail`. Envía un mensaje de correo electrónico.

Para obtener más información acerca de Razor, consulte los siguientes recursos:

- [Entrada de blog de Scott Guthrie presentación de Razor](https://weblogs.asp.net/scottgu/archive/2010/07/02/introducing-razor.aspx)
- [Introducción la publicación de blog de Scott Guthrie el @model palabra clave](https://weblogs.asp.net/scottgu/archive/2010/10/19/asp-net-mvc-3-new-model-directive-support-in-razor.aspx)
- [Introducción a los diseños de Razor para exponer del blog de Scott Guthrie](https://weblogs.asp.net/scottgu/archive/2010/10/22/asp-net-mvc-3-layouts.aspx)
- [Referencia rápida de la API de Razor](../web-pages/overview/api-reference/asp-net-web-pages-api-reference.md)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Support_for_Multiple_View_Engines"></a>

## <a name="support-for-multiple-view-engines"></a>Compatibilidad con varios motores de vista

El **agregar vista** cuadro de diálogo en ASP.NET MVC 3 le permite elegir el motor de vistas que desea trabajar, y el **nuevo proyecto** cuadro de diálogo le permite especificar el motor de vistas predeterminado para un proyecto. Puede elegir el motor de vistas de formularios Web Forms (ASPX), Razor o un motor de vista de código abierto como [Spark](http://sparkviewengine.com/), [NHaml](https://code.google.com/p/nhaml/), o [NDjango](http://ndjango.org/).

<a id="BM_Controller_Improvements"></a>

## <a name="controller-improvements"></a>Mejoras de controlador

### <a name="global-action-filters"></a>Filtros de acción global

A veces desea realizar la lógica antes de que se ejecuta un método de acción o después de que se ejecuta un método de acción. Para ello, ASP.NET MVC 2 proporciona filtros de acción. Los filtros de acción son atributos personalizados que proporcionan un medio declarativo para agregar comportamiento previo y posterior a la acción a los métodos de acción de un controlador específico. Sin embargo, en algunos casos es posible que desee especificar el comportamiento de acción previa o posterior a la acción que se aplica a todos los métodos de acción. MVC 3 le permite especificar los filtros globales agregándolos a la `GlobalFilters` colección. Para obtener más información acerca de los filtros de acción global, consulte los siguientes recursos:

- [Blog de Scott Guthrie sobre la versión preliminar 3 de MVC](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx)
- [Filtrar en ASP.NET MVC](https://msdn.microsoft.com/library/gg416513(VS.98).aspx)

### <a name="new-viewbag-property"></a>Nueva propiedad "ViewBag"

Compatibilidad con los controladores MVC 2 un `ViewData` propiedad que le permite pasar datos a una plantilla de vista mediante un API de diccionario en tiempo de ejecución. En MVC 3, también se puede usar sintaxis un poco más fácil con la `ViewBag` propiedad para lograr el mismo propósito. Por ejemplo, en lugar de escribir `ViewData["Message"]="text"`, puede escribir `ViewBag.Message="text"`. No es necesario definir las clases fuertemente tipadas para usar el `ViewBag` propiedad. Dado que es una propiedad dinámica, en su lugar puede simplemente obtener o establecer las propiedades y se resolverá ellos dinámicamente en tiempo de ejecución. Internamente, `ViewBag` propiedades se almacenan como pares de nombre/valor en el `ViewData` diccionario. (Nota: en la mayoría de las versiones preliminar de MVC 3, el `ViewBag` propiedad se denominó el `ViewModel` propiedad.)

### <a name="new-actionresult-types"></a>Nuevos tipos de "ActionResult"

La siguiente `ActionResult` tipos y métodos auxiliares correspondientes son nuevas o mejoradas en MVC 3:

- [HttpNotFoundResult](https://msdn.microsoft.com/library/system.web.mvc.httpnotfoundresult(v=vs.98).aspx). Devuelve un código de estado HTTP 404 al cliente.
- [RedirectResult](https://msdn.microsoft.com/library/system.web.mvc.redirectresult(v=VS.98).aspx). Devuelve una redirección temporal (código de estado HTTP 302) o una redirección permanente (código de estado HTTP 301), según un parámetro booleano. Junto con este cambio, el [controlador](https://msdn.microsoft.com/library/system.web.mvc.controller(v=VS.98).aspx) clase tiene ahora tres métodos para realizar redirecciones permanentes: `RedirectPermanent`, `RedirectToRoutePermanent`, y `RedirectToActionPermanent`. Estos métodos devuelven una instancia de `RedirectResult` con el `Permanent` propiedad establecida en `true`.
- [HttpStatusCodeResult](https://msdn.microsoft.com/library/system.web.mvc.httpstatuscoderesult(v=VS.98).aspx). Devuelve un código de estado HTTP especificado por el usuario.

<a id="BM_JavaScript_and_Ajax_Improvements"></a>

## <a name="javascript-and-ajax-improvements"></a>Mejoras de Ajax y JavaScript

De forma predeterminada, las aplicaciones auxiliares de Ajax y la validación en MVC 3 usan un enfoque de JavaScript discreto. JavaScript discreto evita la inyección de JavaScript alineado en HTML. Esto hace que el código HTML menor y menos desordenado y resulta más fácil intercambiar o personalizar las bibliotecas de JavaScript. Aplicaciones auxiliares de validación en MVC 3 también utilizan el `jQueryValidate` complemento de forma predeterminada. Si desea que el comportamiento de MVC 2, puede deshabilitar JavaScript discreto utilizando un *web.config* archivo de configuración. Para obtener más información acerca de las mejoras de Ajax y JavaScript, consulte los siguientes recursos:

- [Introducción básica a JavaScript discreto en el sitio de Wikipedia](http://en.wikipedia.org/wiki/Unobtrusive_JavaScript)
- [Brad Wilson Post de JavaScript discreto](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-ajax.html)
- [Brad Wilson JavaScript discreto validación Post](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)
- [Creación de una aplicación MVC 3 con Razor y JavaScript discreto](overview/older-versions/creating-a-mvc-3-application-with-razor-and-unobtrusive-javascript.md) (tutorial en el sitio de ASP.NET)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="client-side-validation-enabled-by-default"></a>Validación del lado cliente habilitada de forma predeterminada

En versiones anteriores de MVC, deberá llamar explícitamente a la `Html.EnableClientValidation` método desde una vista con el fin de habilitar la validación del lado cliente. En MVC 3 esto ya no es necesario porque la validación del lado cliente está habilitado de forma predeterminada. (Se puede deshabilitar mediante la configuración en el *web.config* archivo.)

Para la validación del lado cliente para que funcione, se debe hacer referencia a la correspondiente jQuery y bibliotecas de validación de jQuery en su sitio. Puede hospedar esas bibliotecas en su propio servidor o hacer referencia a ellos desde una red de entrega de contenido (CDN), como las CDN de Microsoft o Google.

### <a name="remote-validator"></a>Validador remoto

ASP.NET MVC 3 es compatible con el nuevo [RemoteAttribute](https://msdn.microsoft.com/library/system.web.mvc.remoteattribute(v=VS.98).aspx) clase que permite aprovechar las ventajas de la validación de jQuery complemento del soporte técnico de validación remota. Esto permite que la biblioteca de validación del lado cliente que llaman automáticamente a un método personalizado que se define en el servidor con el fin de realizar la lógica de validación que solo puede realizarse del lado servidor.

En el ejemplo siguiente, la `Remote` atributo especifica que la validación del cliente llamará a una acción denominada `UserNameAvailable` en el `UsersController` clase con el fin de validar la `UserName` campo.

[!code-csharp[Main](mvc3/samples/sample1.cs)]

El ejemplo siguiente muestra el controlador correspondiente.

[!code-csharp[Main](mvc3/samples/sample2.cs)]

Para obtener más información sobre cómo usar el `Remote` atributo, vea [Cómo: implementar la validación remota en ASP.NET MVC](https://msdn.microsoft.com/library/gg508808(VS.98).aspx) en MSDN library.

### <a name="json-binding-support"></a>Compatibilidad con enlaces de JSON

ASP.NET MVC 3 incluye compatibilidad de enlace de JSON integrada que permite a los métodos de acción recibir datos codificados por JSON y enlace de modelo a los parámetros de método de acción. Esta funcionalidad es útil en escenarios que implican las plantillas de cliente y el enlace de datos. (Las plantillas de cliente permiten aplicar formato y mostrar un único elemento de datos o un conjunto de elementos de datos mediante el uso de plantillas que se ejecutan en el cliente). MVC 3 le permite conectar fácilmente las plantillas de cliente con los métodos de acción en el servidor que envían y reciben datos JSON. Para obtener más información sobre la compatibilidad de enlace de JSON, consulte el **mejoras de AJAX y JavaScript** sección de [entrada de blog de Scott Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx).

<a id="BM_Model_Validation_Improvements"></a>

## <a name="model-validation-improvements"></a>Mejoras de validación del modelo

### <a name="dataannotations-metadata-attributes"></a>Atributos de metadatos "DataAnnotations"

Es compatible con ASP.NET MVC 3 `DataAnnotations` atributos de metadatos como `DisplayAttribute`.

### <a name="validationattribute-class"></a>Clase "ValidationAttribute"

El `ValidationAttribute` clase se ha mejorado en .NET Framework 4 para admitir un nuevo `IsValid` sobrecarga que proporciona más información sobre el contexto de validación actual, como el objeto que se va a validar. Esto permite escenarios más completos, donde puede validar el valor actual en función de otra propiedad del modelo. Por ejemplo, el nuevo `CompareAttribute` atributo le permite comparar los valores de dos propiedades de un modelo. En el ejemplo siguiente, la `ComparePassword` propiedad debe coincidir con el `Password` campo para que sean válidos.

[!code-csharp[Main](mvc3/samples/sample3.cs)]

### <a name="validation-interfaces"></a>Interfaces de validación

El [IValidatableObject](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.ivalidatableobject.aspx) interfaz le permite realizar la validación de nivel de modelo, y le permite proporcionar mensajes de error que son específicos para el estado del modelo general, o entre dos propiedades dentro del modelo de validación . MVC 3 ahora recupera los errores desde el `IValidatableObject` interfaz cuando el enlace de modelos y automáticamente marcas o temas destacados afectados los campos dentro de una vista con las aplicaciones auxiliares integradas formulario HTML.

El [IClientValidatable](https://msdn.microsoft.com/library/system.web.mvc.iclientvalidatable(v=VS.98).aspx) interfaz permite que ASP.NET MVC detectar en tiempo de ejecución si un validador tiene compatibilidad para la validación del cliente. Esta interfaz se ha diseñado para que se puede integrar con una variedad de marcos de validación.

Para obtener más información sobre las interfaces de validación, vea el **mejoras de validación del modelo** sección de [entrada de blog de Scott Guthrie MVC 3 Preview](https://weblogs.asp.net/scottgu/archive/2010/07/27/introducing-asp-net-mvc-3-preview-1.aspx). (Sin embargo, tenga en cuenta que la referencia a "IValidateObject" en el blog de debe ser "IValidatableObject".)

<a id="BM_Dependency_Injection_Improvements"></a>

## <a name="dependency-injection-improvements"></a>Mejoras de inyección de dependencia

ASP.NET MVC 3 ofrece compatibilidad mejorada para aplicar la inserción de dependencias (DI) y para la integración con contenedores de inversión de Control (IOC) o de inserción de dependencias. Se ha agregado compatibilidad con DI en las áreas siguientes:

- Controladores (registro y la inyección de generadores de controladores, inserción de controladores).
- Vistas (registro y la inserción de motores de vista, inyección de dependencias en las páginas de vista).
- Filtros de acción (Buscar y la inserción de filtros).
- Enlazadores de modelos (registro y la inserción).
- Proveedores de validación del modelo (registro y la inserción).
- Proveedores de metadatos del modelo (registro y la inserción).
- Proveedores de valor (registro y la inserción).

MVC 3 es compatible con la [Common Service Locator](https://github.com/unitycontainer/commonservicelocator) biblioteca y cualquier contenedor de DI que admita esa biblioteca `IServiceLocator` interfaz. También admite un nuevo `IDependencyResolver` interfaz que facilita la integración de marcos de DI.

Para obtener más información acerca de la inserción de dependencias de MVC 3, vea los siguientes recursos:

- [Serie de Brad Wilson de entradas de blog en la ubicación del servicio](http://bradwilson.typepad.com/blog/2010/07/service-location-pt1-introduction.html)
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

<a id="BM_Other_New_Features"></a>

## <a name="other-new-features"></a>Otras características nuevas

### <a name="nuget-integration"></a>Integración de NuGet

ASP.NET MVC 3 automáticamente se instala y habilita NuGet como parte de su programa de instalación. NuGet es un administrador de paquetes de código abierto gratuito que hace que sea más fácil buscar, instalar y usar herramientas y bibliotecas de .NET en los proyectos. Funciona con todos los tipos de proyecto de Visual Studio (incluidos los formularios Web Forms ASP.NET y ASP.NET MVC).

NuGet permite a los desarrolladores que mantienen los proyectos de código abierto (por ejemplo, proyectos como Moq, NHibernate, Ninject, StructureMap, NUnit, Windsor, RhinoMocks y Elmah) para empaquetar sus bibliotecas y registrarlos en una galería en línea. A continuación, es fácil para los desarrolladores de .NET que desean usar una de estas bibliotecas para buscar el paquete e instalarlo en los proyectos que están trabajando.

Con ASP.NET 3 Tools Update, las plantillas de proyecto incluyan JavaScript bibliotecas preinstalados paquetes de NuGet, por lo que se puede actualizar a través de NuGet. Entity Framework Code First es también instalaron previamente como un paquete de NuGet.

Para más información sobre NuGet, consulte la [documentación correspondiente](https://docs.microsoft.com/nuget/).

### <a name="partial-page-output-caching"></a>Caché de resultados de página parcial

MVC de ASP.NET ha admitido la caché de resultados de las respuestas de página completa desde la versión 1. MVC 3 también admite la caché de resultados de página parcial, que le permite fácilmente las regiones de memoria caché o fragmentos de una respuesta. Para obtener más información sobre almacenamiento en caché, consulte el **caché de resultados de página parcial** sección de [entrada de blog de Scott Guthrie en MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx) y **caché de resultados de acciónsecundarios** sección de la [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).

### <a name="granular-control-over-request-validation"></a>Control granular sobre la validación de solicitudes

ASP.NET MVC tiene validación de solicitud integrada que automáticamente le ayuda a protegerse frente a ataques de inyección de XSS y HTML. Sin embargo, en ocasiones, deseará explícitamente disable validación de la solicitud, por ejemplo, si desea permitir que los usuarios registrar el HTML de contenido (por ejemplo, en el contenido CMS o entradas de blog). Ahora puede agregar un [AllowHtml](https://msdn.microsoft.com/library/system.web.mvc.allowhtmlattribute(v=VS.98).aspx) atributo a los modelos o ver los modelos para deshabilitar la validación de solicitud en una base por propiedad durante el enlace del modelo. Para obtener más información acerca de la validación de solicitudes, consulte los siguientes recursos:

- El **JavaScript discreto y validación** sección [entrada de blog de Scott Guthrie en MVC 3 release candidate](https://weblogs.asp.net/scottgu/archive/2010/11/09/announcing-the-asp-net-mvc-3-release-candidate.aspx).
- [Notas de la versión MVC 3](../whitepapers/mvc3-release-notes.md)

### <a name="extensible-new-project-dialog-box"></a>Extensible "New Project" cuadro de diálogo

En ASP.NET MVC 3 puede agregar plantillas de proyecto, los motores de vista, y marcos del proyecto de prueba unitaria el **nuevo proyecto** cuadro de diálogo.

### <a name="template-scaffolding-improvements"></a>Mejoras de Scaffolding de plantilla

Las plantillas de scaffolding de ASP.NET MVC 3 hacer mejor su trabajo de identificar las propiedades de clave principal en los modelos y controlarlos adecuadamente que en versiones anteriores de MVC. (Por ejemplo, las plantillas de scaffolding ahora Asegúrese de que la clave principal no ha aplicado scaffolding como un campo de formulario editable.)

De forma predeterminada, el scaffolding de Create y Edit ahora usa el `Html.EditorFor` auxiliares en lugar de la `Html.TextBoxFor` auxiliar. Esto mejora la compatibilidad con metadatos en el modelo en el formulario de datos de atributos de anotación cuando el **agregar vista** cuadro de diálogo genera una vista.

### <a name="new-overloads-for-htmllabelfor-and-htmllabelformodel"></a>Nuevas sobrecargas para "Html.LabelFor" y "Html.LabelForModel"

Se han agregado nuevas sobrecargas de método para el `LabelFor` y `LabelForModel` métodos auxiliares. Las nuevas sobrecargas permiten especificar o reemplazar el texto de etiqueta.

### <a name="sessionless-controller-support"></a>Compatibilidad del controlador sin sesión

En ASP.NET MVC 3, puede indicar si desea que una clase de controlador para usar el estado de sesión y si es así, si el estado de sesión debe ser de sólo lectura o de solo lectura. Para obtener más información sobre la compatibilidad de controlador sin sesión, vea [MVC 3 Release Notes](../whitepapers/mvc3-release-notes.md).

### <a name="new-additionalmetadataattribute-class"></a>Nueva clase "AdditionalMetadataAttribute"

Puede usar el [AdditionalMetadata](https://msdn.microsoft.com/library/system.web.mvc.additionalmetadataattribute(v=VS.98).aspx) atributo para rellenar el `ModelMetadata.AdditionalValues` diccionario para una propiedad de modelo. Por ejemplo, si un modelo de vista tiene una propiedad que debe mostrarse solo a un administrador, puede anotar esa propiedad como se muestra en el ejemplo siguiente:

[!code-csharp[Main](mvc3/samples/sample4.cs)]

Estos metadatos está disponible para cualquier plantilla de pantalla o el editor cuando se procesa un modelo de vista del producto. Es decisión suya interpretar la información de metadatos.

### <a name="accountcontroller-improvements"></a>Mejoras de AccountController

Se ha mejorado considerablemente AccountController en la plantilla de proyecto de Internet.

### <a name="new-intranet-project-template"></a>Nueva plantilla de proyecto de Intranet

Se incluye una nueva plantilla de proyecto de Intranet que habilita la autenticación de Windows y quita AccountController.
