---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: Generar perfiles y depurar la aplicación de ASP.NET MVC con Glimpse | Microsoft Docs
author: Rick-Anderson
description: Glimpse es un próspero y aumentando la familia de paquetes de NuGet de código abierto que proporciona detallados de rendimiento, depuración e información de diagnóstico para ASP.NET un...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 4d044217d6c33594b39747a765165b8702338027
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808546"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Generar perfiles y depurar la aplicación de ASP.NET MVC con Glimpse
====================
por [Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse es un próspero y aumentando la familia de paquetes de NuGet de código abierto que proporciona detallados de rendimiento, depuración e información de diagnóstico para aplicaciones de ASP.NET. Es muy fácil instalar, ultrarrápidas ligero y muestra las métricas clave de rendimiento en la parte inferior de cada página. Permite explorar en profundidad de la aplicación cuando necesite averiguar qué está ocurriendo en el servidor. Glimpse proporciona tanto información valiosa y que se recomienda que utilizarlo durante su ciclo de desarrollo, incluidos el entorno de prueba de Azure. Mientras [Fiddler](http://www.telerik.com/fiddler) y [herramientas de desarrollo F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan un cliente, vista de Glimpse proporciona una vista detallada del servidor. En este tutorial se centrará en utilizar los paquetes EF y un vistazo a ASP.NET MVC, pero hay muchos otros paquetes disponibles. Siempre que sea posible se vinculará a la correspondiente [vislumbrar docs](http://getglimpse.com/Docs/) que ayudan a mantener. Glimpse es un proyecto de código abierto, también puede contribuir en el código fuente y los documentos.


- [Instalación de Glimpse](#ig)
- [Habilitar visión para el host local](#eg)
- [La pestaña escala de tiempo](#Time)
- [Enlace de modelos](#mb)
- [Rutas](#route)
- [Uso de Glimpse en Azure](#da)
- [Recursos adicionales](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalación de Glimpse

Puede instalar Glimpse desde la consola del Administrador de paquetes de NuGet o desde el **administrar paquetes de NuGet** consola. Para esta demostración, deberá instalar los paquetes Mvc5 y EF6:

![instalar Glimpse desde NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Busque *Glimpse.EF*

![Glimpse.EF desde el cuadro de diálogo de instalación de NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Seleccionando **paquetes instalados**, puede ver los módulos dependientes Glimpse instalados:

![Instalar paquetes de Glimpse desde DLg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

El siguiente comando instala los módulos de Glimpse MVC5 y EF6 desde la consola del Administrador de paquetes:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar visión para el host local

Vaya a http://localhost:&lt; número de puerto&gt;/glimpse.axd y haga clic en el <strong>activar Glimpse</strong> botón.

![Página de glimpse axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Si tienes que muestra la barra de favoritos, puede arrastrar y colocar los botones de Glimpse y agregarlas como bookmarklets:

![IE con Glimpse boookmarklets](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Ahora puede desplazarse la aplicación y el **cabezas de presentación** (HUD) se muestra en la parte inferior de la página.

![Póngase en contacto con el administrador página con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

El [página HUD de Glimpse](http://getglimpse.com/Docs/Heads-up-Display) detalla la información de tiempo mostrada anteriormente. La muestra de datos el HUD rendimiento discreta puede avisarle de un problema inmediatamente - antes de obtener el ciclo de prueba. Al hacer clic en el &quot;g&quot; abre el panel de Glimpse en la esquina inferior derecha:

![Panel de glimpse](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

En la imagen anterior, el [pestaña ejecución](http://getglimpse.com/Docs/Execution-Tab) está seleccionada, que muestra detalles de tiempo de las acciones y los filtros de la canalización. Puede ver mi [temporizador de inspección Detener filtro](http://www.nuget.org/packages/StopWatch/) a partir de la fase 6 de la canalización. Si bien puede proporcionar mi temporizador ligero útil perfil o datos de control de tiempo, se pierde todo el tiempo invertido en la autorización y la representación de la vista. Puede leer sobre mi temporizador en [de perfil y la hora de la aplicación de ASP.NET MVC hasta Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). El [pestañas](http://getglimpse.com/Docs/Tabs) página proporciona vínculos a información detallada sobre cada pestaña.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>La pestaña escala de tiempo

He modificado de Tom Dykstra pendientes [tutorial EF 6 y MVC 5](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con el código siguiente, cambie el controlador de instructors:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

El código anterior me permite introducir en la cadena de consulta (`eager`) al control explícito o diligente la carga de datos. En la imagen siguiente, se usa la carga explícita y la página de control de tiempo muestra cada inscripción que se cargan en el `Index` método de acción:

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

En el código siguiente, se especifica diligente y se captura cada inscripción después de la `Index` se denomina vista:

![diligente se especifica](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Puede mantener el puntero sobre un segmento de tiempo para obtener información de tiempo detallada:

![al mantener el mouse para ver el control de tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Enlace de modelos

El [pestaña modelo de enlace](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información que le ayudarán a comprender cómo se enlazan las variables de formulario y por qué algunos no están enlazados como cabría esperar. ¿La imagen siguiente muestra el **?** icono que se puede hacer clic para que aparezca la página de Ayuda de glimpse para esa característica.

![Eche un vistazo a la vista modelo de enlace](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rutas

 La pestaña rutas vistazo puede le ayudará depurar y entender el enrutamiento. En la imagen siguiente, se selecciona la ruta de producto (y lo muestra en verde, una convención de Glimpse). ![nombre del producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) también se muestran los tokens de datos, las áreas y restricciones de ruta. Consulte [Glimpse rutas](http://getglimpse.com/Docs/Routes-Tab) y [enrutamiento mediante atributos en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obtener más información. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Uso de Glimpse en Azure

La directiva de seguridad predeterminada de Glimpse solo permite que los datos de observar lo que se muestran desde el host local. Puede cambiar esta directiva de seguridad para que pueda ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure). Para entornos de prueba en Azure, agregue la marca de resaltado hasta la parte inferior de la *web.confg* archivo para habilitar la perspectiva:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con este cambio por sí solo, cualquier usuario puede ver los datos de Glimpse en un sitio remoto. Considere la posibilidad de agregar el marcado anterior a un perfil de publicación, por lo que solo ha implementado un aplicada al usar ese perfil de publicación (por ejemplo, su proifle de prueba de Azure.) Para restringir los datos de Glimpse, agregaremos el `canViewGlimpseData` rol y permitir sólo a los usuarios con este rol para ver los datos de Glimpse.

Quite los comentarios de la *GlimpseSecurityPolicy.cs* y cambie el [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) llamar desde `Administrator` a la `canViewGlimpseData` rol:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Seguridad: los datos enriquecidos proporcionados por Glimpse podrían exponer la seguridad de la aplicación. Microsoft no ha realizado una auditoría de seguridad de Glimpse para su uso en aplicaciones de producción.


Para obtener información sobre cómo agregar roles, consulte mi [implementar una aplicación web de ASP.NET MVC 5 segura con suscripción, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Implementar una aplicación ASP.NET MVC 5 segura con pertenencia, OAuth y SQL Database en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Eche un vistazo configuración](http://getglimpse.com/Docs/Configuration) -página de documentación sobre la configuración de las fichas, directiva de tiempo de ejecución, registro y mucho más.
