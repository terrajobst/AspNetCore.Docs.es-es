---
uid: web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
title: Crear páginas de ayuda para ASP.NET Web API | Documentos de Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2013
ms.topic: article
ms.assetid: 0150e67b-c50d-4613-83ea-7b4ef8cacc5a
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/creating-api-help-pages
msc.type: authoredcontent
ms.openlocfilehash: 37fd26ebaea192cb540c443eff8a07343ab8c15b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "28037908"
---
<a name="creating-help-pages-for-aspnet-web-api"></a>Crear páginas de ayuda para ASP.NET Web API
====================
por [Mike Wasson](https://github.com/MikeWasson)

Cuando se crea una API web, a menudo resulta útil crear una página de ayuda, para que otros desarrolladores sepan cómo llamar a la API. Puede crear toda la documentación de forma manual, pero es mejor para generar automáticamente tanto como sea posible.

Para facilitar esta tarea, ASP.NET Web API proporciona una biblioteca para generar automáticamente las páginas de ayuda en tiempo de ejecución.

![](creating-api-help-pages/_static/image1.png)

## <a name="creating-api-help-pages"></a>Crear páginas de Ayuda de API

Instalar [Web ASP.NET y herramientas de actualización 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650). Esta actualización integra en las páginas de ayuda en la plantilla de proyecto de API Web.

A continuación, cree un nuevo proyecto de ASP.NET MVC 4 y seleccione la plantilla de proyecto de API Web. La plantilla de proyecto crea un controlador de API de ejemplo denominado `ValuesController`. La plantilla también crea las páginas de Ayuda de API. Todos los archivos de código para la página de ayuda se colocan en la carpeta de áreas del proyecto.

![](creating-api-help-pages/_static/image2.png)

Al ejecutar la aplicación, la página principal contiene un vínculo a la página de Ayuda de API. En la página principal, la ruta de acceso relativa es/Help.

![](creating-api-help-pages/_static/image3.png)

Este vínculo le lleva a una página de resumen de la API.

![](creating-api-help-pages/_static/image4.png)

La vista MVC de esta página se define en Areas/HelpPage/Views/Help/Index.cshtml. Puede editar esta página para modificar el diseño, introducción, título, estilos y así sucesivamente.

La parte principal de la página es una tabla de API, agrupadas por el controlador. Las entradas de tabla se generan de forma dinámica, mediante la **IApiExplorer** interfaz. (Hablaremos más acerca de esta interfaz más adelante.) Si agrega un nuevo controlador de API, la tabla se actualiza automáticamente en tiempo de ejecución.

La columna "API" muestra el método HTTP y el URI relativo. La columna "Descripción" contiene documentación para cada API. Inicialmente, la documentación es simplemente texto de marcador de posición. En la sección siguiente, mostraré cómo agregar la documentación de los comentarios XML.

Cada API incluye un vínculo a una página con información más detallada, incluidos los cuerpos de solicitud y respuesta de ejemplo.

![](creating-api-help-pages/_static/image5.png)

## <a name="adding-help-pages-to-an-existing-project"></a>Agregar páginas de ayuda a un proyecto existente

Puede agregar páginas de ayuda a un proyecto existente de la API Web mediante el Administrador de paquetes de NuGet. Esta opción es útil que empezar desde una plantilla de proyecto diferente de la plantilla de "API Web".

Desde el **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**y, a continuación, seleccione **Package Manager Console**. En el [Package Manager Console](http://docs.nuget.org/docs/start-here/using-the-package-manager-console) ventana, escriba uno de los siguientes comandos:

Para una **C#** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage`

Para una **Visual Basic** aplicación: `Install-Package Microsoft.AspNet.WebApi.HelpPage.VB`

Hay dos paquetes, uno para C# y otro para Visual Basic. Asegúrese de utilizar uno que se adapte al proyecto.

Este comando instala a los ensamblados necesarios y agrega las vistas MVC para las páginas de ayuda (que se encuentra en la carpeta áreas/HelpPage). Debe agregar manualmente un vínculo a la página de ayuda. El URI es/Help. Para crear un vínculo en una vista razor, agregue lo siguiente:

[!code-cshtml[Main](creating-api-help-pages/samples/sample1.cshtml)]

Además, asegúrese de registrar las áreas. En el archivo Global.asax, agregue el código siguiente a la **aplicación\_iniciar** método, si no está allí ya:

[!code-csharp[Main](creating-api-help-pages/samples/sample2.cs?highlight=4)]

## <a name="adding-api-documentation"></a>Agregar la documentación de API

De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación. Puede usar [comentarios de documentación XML](https://msdn.microsoft.com/library/b2s063f7.aspx) para crear la documentación. Para habilitar esta característica, abra el archivo áreas/HelpPage/aplicación\_Start/HelpPageConfig.cs y elimine la línea siguiente:

[!code-csharp[Main](creating-api-help-pages/samples/sample3.cs)]

Ahora habilitar la documentación XML. En el Explorador de soluciones, haga clic en el proyecto y seleccione **propiedades**. Seleccione el **generar** página.

![](creating-api-help-pages/_static/image6.png)

En **salida**, comprobar **archivo de documentación XML**. En el cuadro de edición, escriba "aplicación\_Data/XmlDocument.xml".

![](creating-api-help-pages/_static/image7.png)

A continuación, abra el código para el `ValuesController` controlador de API, que se define en /Controllers/ValuesControler.cs. Agregar algunos comentarios de documentación a los métodos de controlador. Por ejemplo:

[!code-csharp[Main](creating-api-help-pages/samples/sample4.cs)]

> [!NOTE]
> Sugerencia: Si coloca el símbolo de intercalación en la línea encima del método y escribe tres barras diagonales, Visual Studio inserta automáticamente los elementos XML. A continuación, puede rellenar los espacios en blanco.


Ahora compilar y ejecutar la aplicación de nuevo y vaya a las páginas de ayuda. Las cadenas de documentación deben aparecer en la tabla de la API.

![](creating-api-help-pages/_static/image8.png)

La página de ayuda lee las cadenas desde el archivo XML en tiempo de ejecución. (Cuando se implementa la aplicación, asegúrese de que implementar el archivo XML.)

## <a name="under-the-hood"></a>En segundo plano

Las páginas de ayuda se crean en la parte superior de la **ApiExplorer** (clase), que forma parte del marco Web API. El **ApiExplorer** clase proporciona las materias primas para la creación de una página de ayuda. Para cada API **ApiExplorer** contiene un **ApiDescription** que describe la API. Para ello, una "API" se define como la combinación del método HTTP y URI relativo. Por ejemplo, incluimos algunas API distinto:

- OBTENER /api/Products
- OBTENER/API/Products / {id}
- REGISTRAR/api/productos

Si una acción de controlador es compatible con varios métodos HTTP, el **ApiExplorer** trata cada método como una API distinta.

Para ocultar una API de la **ApiExplorer**, agregue el **ApiExplorerSettings** atribuir a la acción y el conjunto *IgnoreApi* en true.

[!code-csharp[Main](creating-api-help-pages/samples/sample5.cs)]

También puede agregar este atributo al controlador, para excluir el controlador de todo.

La clase ApiExplorer Obtiene las cadenas de documentación de la **IDocumentationProvider** interfaz. Como se vio anteriormente, la biblioteca de páginas de Ayuda proporciona un **IDocumentationProvider** que obtiene la documentación de las cadenas de documentación XML. El código se encuentra en /Areas/HelpPage/XmlDocumentationProvider.cs. Puede obtener documentación de otro origen escribiendo su propio **IDocumentationProvider**. Para vincularla, llame a la **SetDocumentationProvider** método de extensión, definido en **HelpPageConfigurationExtensions**

**ApiExplorer** llama automáticamente a la **IDocumentationProvider** interfaz para obtener cadenas de documentación para cada API. Almacena en la **documentación** propiedad de la **ApiDescription** y **ApiParameterDescription** objetos.

## <a name="next-steps"></a>Pasos siguientes

No se limita a las páginas de ayuda que se muestra aquí. De hecho, **ApiExplorer** no se limita a la creación de páginas de ayuda. Yao Huang Lin ha escrito que las entradas de algunos blog excelente para que pueda pensar de fábrica:

- [Agregar a un cliente de prueba simple a la página de Ayuda de ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/02/adding-a-simple-test-client-to-asp-net-web-api-help-page.aspx)
- [Para que la página de Ayuda de API Web de ASP.NET se funcionen en los servicios autohospedados](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/20/making-asp-net-web-api-help-page-work-on-self-hosted-services.aspx)
- [Generación de tiempo de diseño de página de ayuda (o cliente) para ASP.NET Web API](https://blogs.msdn.com/b/yaohuang1/archive/2013/01/20/design-time-generation-of-help-page-or-proxy-for-asp-net-web-api.aspx)
- [Personalizaciones avanzadas de la página de ayuda](https://blogs.msdn.com/b/yaohuang1/archive/2012/12/10/asp-net-web-api-help-page-part-3-advanced-help-page-customizations.aspx)
