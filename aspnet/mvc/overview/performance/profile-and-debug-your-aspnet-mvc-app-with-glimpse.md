---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: "Cree un perfil y depurar la aplicación de ASP.NET MVC con perspectiva | Documentos de Microsoft"
author: Rick-Anderson
description: "Perspectiva es un éxito y aumenta la familia de paquetes de NuGet de código abierto que proporciona rendimiento detallados, depuración e información de diagnóstico para ASP.NET un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 9cfdced21251b482ca527dda9c3a698de77cc8ca
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>Cree un perfil y depurar la aplicación de ASP.NET MVC con perspectiva
====================
Por [Rick Anderson](https://github.com/Rick-Anderson)

> Perspectiva es un éxito y aumenta la familia de paquetes de NuGet de código abierto que proporciona rendimiento detallados, depuración e información de diagnóstico para las aplicaciones ASP.NET. Es muy fácil de instalar, ultrarrápido ligero y muestra las métricas de rendimiento clave en la parte inferior de cada página. Permite profundizar en la aplicación cuando necesite averiguar qué está sucediendo en el servidor. Perspectiva proporciona sea tan valiosa información que le recomendamos que use a lo largo del ciclo de desarrollo, incluidos el entorno de prueba de Azure. Mientras [Fiddler](http://www.telerik.com/fiddler) y [herramientas de desarrollo de F-12](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx) proporcionan un cliente, vista de perspectiva proporciona una vista detallada del servidor. Este tutorial se centrará en utilizar los paquetes EF y vistazo ASP.NET MVC, pero hay muchos otros paquetes disponibles. Siempre que sea posible vinculará a la correspondiente [echarle documentos](http://getglimpse.com/Docs/) que ayudar a mantener. Perspectiva es un proyecto de código abierto, demasiado pueden contribuir a los documentos, el código fuente.


- [Instalación de perspectiva](#ig)
- [Habilitar la perspectiva para el host local](#eg)
- [La pestaña escala de tiempo](#Time)
- [Enlace de modelos](#mb)
- [Rutas](#route)
- [Con la perspectiva en Azure](#da)
- [Recursos adicionales](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>Instalación de perspectiva

Puede instalar perspectiva desde la consola del Administrador de paquetes de NuGet o desde el **administrar paquetes de NuGet** consola. En esta demostración, instalarán los paquetes Mvc5 y EF6:

![instalar la perspectiva de NuGet Dlg](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

Busque *Glimpse.EF*

![Glimpse.EF desde el cuadro de diálogo de instalación de NuGet](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

Si selecciona **paquetes instalados**, puede ver los módulos dependientes de perspectiva instalados:

![Instalar paquetes de perspectiva de cuadro de diálogo](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

El siguiente comando instala módulos Glimpse MVC5 y EF6 desde la consola de administrador de paquetes:

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>Habilitar la perspectiva para el host local

Vaya a http://localhost:&lt;º de puerto&gt;/glimpse.axd y haga clic en el **activar Glimpse** botón.

![Página de perspectiva axd](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

Si tienes que muestra la barra de favoritos, puede arrastrar y colocar los botones de la perspectiva y agregarlas como bookmarklets:

![Internet Explorer con boookmarklets de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

Ahora puede navegar por la aplicación y el **cabezales de una presentación** (HUD pantalla) se muestra en la parte inferior de la página.

![Póngase en contacto con el administrador página con HUD](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

El [página perspectiva HUD](http://getglimpse.com/Docs/Heads-up-Display) se detalla la información de control de tiempo mostrada anteriormente. La muestra de datos el HUD de rendimiento discreto puede notificar a un problema inmediatamente - antes de llegar al ciclo de prueba. Al hacer clic en el &quot;g&quot; abre el panel de la perspectiva en la esquina inferior derecha:

![Panel de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

En la imagen anterior, el [ficha ejecución](http://getglimpse.com/Docs/Execution-Tab) está seleccionada, que muestra detalles de tiempo de las acciones y los filtros en la canalización. Puede ver mi [temporizador de filtro de inspección detener](http://www.nuget.org/packages/StopWatch/) empiezan en la fase 6 de la canalización. Aunque puede proporcionar el temporizador ligero útil perfil/datos de control de tiempo, pierde todo el tiempo empleado en la autorización y la representación de la vista. Puede leer sobre mi temporizador en [de perfil y la hora de la aplicación de ASP.NET MVC a Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx). El [pestañas](http://getglimpse.com/Docs/Tabs) página proporciona vínculos a información detallada sobre cada ficha.

<a id="Time"></a>
## <a name="the-timeline-tab"></a>La pestaña escala de tiempo

Modifiqué de Tom Dykstra pendientes [tutorial de 6/MVC 5 EF](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) con el código siguiente, cambiar el controlador de instructores:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

El código anterior me permite pasar en la cadena de consulta (`eager`) al control diligente o explícita la carga de datos. En la imagen siguiente, se utiliza la carga explícita y la página de control de tiempo muestra cada inscripción cargado en el `Index` método de acción:

![carga explícita](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

En el código siguiente, se especifica diligente y se captura cada inscripción después de la `Index` vista se denomina:

![de forma diligente](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

Puede desplazar el puntero sobre un segmento de tiempo para obtener información de tiempo detallada:

![al mantener el mouse para ver el control de tiempo detallado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>Enlace de modelos

El [pestaña modelo de enlace](http://getglimpse.com/Docs/Model-Binding-Tab) proporciona una gran cantidad de información para ayudarle a entender cómo se enlazan las variables de formulario y por qué algunos no se enlazan tal y como se esperaría. ¿La imagen siguiente muestra el **?** icono, lo que puede hacer clic en para abrir la página de Ayuda de perspectiva para esa característica.

![enlace de vista de modelos de perspectiva](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>Rutas

 La ficha rutas de perspectiva le puede permitirá depurar y entender el enrutamiento. En la imagen siguiente, se selecciona la ruta de producto (y se muestra en verde, una convención de perspectiva). ![nombre de producto seleccionado](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png) también se muestran los tokens de datos, las áreas y las restricciones de ruta. Vea [Glimpse rutas](http://getglimpse.com/Docs/Routes-Tab) y [atributo enrutamiento en ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx) para obtener más información. 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>Con la perspectiva en Azure

La directiva de seguridad predeterminada de perspectiva permite solo vistazo datos mostrarán desde el host local. Puede cambiar esta directiva de seguridad para que pueda ver estos datos en un servidor remoto (por ejemplo, una aplicación web en Azure). Para entornos de prueba en Azure, agregue la marca de resaltado hasta la parte inferior de la *Web.config* archivo para habilitar la perspectiva:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

Con este cambio por sí sola, cualquier usuario puede ver los datos de la perspectiva en un sitio remoto. Considere la posibilidad de agregar el marcado anteriormente a un perfil de publicación para que solo ha implementado un aplicado cuando se usa ese perfil de publicación (por ejemplo, su proifle de prueba de Azure.) Para restringir los datos de la perspectiva, agregaremos el `canViewGlimpseData` rol y solo se permiten a los usuarios de este rol para ver los datos de la perspectiva.

Quite los comentarios de la *GlimpseSecurityPolicy.cs* y cambie el [IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx) llamar desde `Administrator` a la `canViewGlimpseData` rol:

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> Seguridad: los datos enriquecidos proporcionados por perspectiva pudieron exponer la seguridad de la aplicación. Microsoft no ha realizado una auditoría de seguridad de Glimpse para su uso en aplicaciones de producción.


Para obtener información sobre cómo agregar roles, consulte mi [implementar una aplicación web de Secure ASP.NET MVC 5 con pertenencia, OAuth y base de datos SQL en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/) tutorial.

<a id="addRes"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [Implementar una aplicación de MVC de ASP.NET 5 segura con pertenencia, OAuth y base de datos SQL en Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [Configuración de echarle](http://getglimpse.com/Docs/Configuration) -página de documentación sobre la configuración de las fichas, directiva de tiempo de ejecución, el registro y mucho más.
