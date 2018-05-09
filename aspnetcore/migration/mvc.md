---
title: Migrar de MVC de ASP.NET a ASP.NET Core MVC
author: ardalis
description: Obtenga información acerca de cómo empezar a migrar un proyecto de MVC de ASP.NET a ASP.NET MVC de núcleo.
manager: wpickett
ms.author: riande
ms.date: 03/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/mvc
ms.openlocfilehash: b8c913c0a6f47a1c993d508f9baae54981327957
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/08/2018
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migrar de MVC de ASP.NET a ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), y [Scott Addie](https://scottaddie.com)

Este artículo muestra cómo empezar a migrar un proyecto de MVC de ASP.NET a [MVC de ASP.NET Core](../mvc/overview.md). En el proceso, resalta muchas de las cosas que han cambiado respecto a ASP.NET MVC. La migración de ASP.NET MVC es un proceso en varias fases y este artículo trata la instalación inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente. Migrar la configuración y el código de identidad que se encuentra en muchos proyectos de ASP.NET MVC tratan otros artículos.

> [!NOTE]
> Los números de versión en los ejemplos podrían no estar actualizados. Debe actualizar los proyectos en consecuencia.

## <a name="create-the-starter-aspnet-mvc-project"></a>Crear el proyecto de MVC de ASP.NET de inicio

Para demostrar la actualización, comenzaremos mediante la creación de una aplicación de ASP.NET MVC. Crear con el nombre *WebApp1* para el espacio de nombres coincida con el proyecto de ASP.NET Core que creamos en el paso siguiente.

![El cuadro de diálogo de Visual Studio nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: plantilla de proyecto MVC seleccionado en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcional:* cambiar el nombre de la solución de *WebApp1* a *Mvc5*. Visual Studio muestra el nuevo nombre de la solución (*Mvc5*), lo que facilita indicar este proyecto desde el proyecto siguiente.

## <a name="create-the-aspnet-core-project"></a>Crear el proyecto de ASP.NET Core

Crear un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos. Tener el mismo espacio de nombres resulta más fácil copiar el código entre los dos proyectos. Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para utilizar el mismo nombre.

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: plantilla de proyecto vacía seleccionada en el panel de plantillas de núcleo de ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcional:* crear una nueva aplicación ASP.NET Core usando la *aplicación Web* plantilla de proyecto. Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**. Cambiar el nombre de esta aplicación *FullAspNetCore*. Creación de este proyecto le permite ahorrar tiempo en la conversión. También puede buscar en el código generado de plantilla para ver el resultado final o copiar el código al proyecto de conversión. También es útil cuando bloquea en un paso de la conversión que se compara con el proyecto de plantilla generado.

## <a name="configure-the-site-to-use-mvc"></a>Configurar el sitio para usar MVC

* Cuando el destino es .NET Core, el metapackage de ASP.NET Core se agrega al proyecto, denominado `Microsoft.AspNetCore.All` de forma predeterminada. Este paquete contiene paquetes como `Microsoft.AspNetCore.Mvc` y `Microsoft.AspNetCore.StaticFiles`. Si el destino es .NET Framework, las referencias del paquete tiene que mostrar individualmente en el archivo *.csproj.

`Microsoft.AspNetCore.Mvc` es el marco de MVC de ASP.NET Core. `Microsoft.AspNetCore.StaticFiles` es el controlador de archivos estáticos. El tiempo de ejecución de ASP.NET Core es modular y explícitamente debe participar en servir archivos estáticos (vea [archivos estáticos](xref:fundamentals/static-files)).

* Abra la *Startup.cs* y cambie el código para que coincida con lo siguiente:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos. Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos. El `UseMvc` método de extensión agrega enrutamiento. Para obtener más información, consulte [inicio de la aplicación](xref:fundamentals/startup) y [enrutamiento](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Agregar un controlador y una vista

En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas a migrar en la sección siguiente.

* Agregar un *controladores* carpeta.

* Agregar un **clase de controlador** denominado *HomeController.cs* a la *controladores* carpeta.

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/add_mvc_ctl.png)

* Agregar un *vistas* carpeta.

* Agregar un *vistas/inicio* carpeta.

* Agregar un **vista Razor** denominado *Index.cshtml* a la *vistas/inicio* carpeta.

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/view.png)

A continuación se muestra la estructura del proyecto:

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con lo siguiente:

```html
<h1>Hello world!</h1>
```

Ejecute la aplicación.

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

Vea [controladores](xref:mvc/controllers/actions) y [vistas](xref:mvc/views/overview) para obtener más información.

Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, se podemos empezar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET. Es preciso mover lo siguiente:

* contenido del lado cliente (CSS, fuentes y secuencias de comandos)

* controladores

* vistas

* modelos

* Cómo agrupar

* filtros

* Registro de entrada/salida, la identidad (Esto se hace en el siguiente tutorial.)

## <a name="controllers-and-views"></a>Controladores y vistas

* Copiar cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`. Tenga en cuenta que en ASP.NET MVC, tipo de valor devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en el núcleo de ASP.NET MVC, los métodos de acción devueltos `IActionResult` en su lugar. `ActionResult` implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.

* Copia la *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista Razor desde el proyecto de MVC de ASP.NET para el proyecto de ASP.NET Core.

* Ejecutar la aplicación de ASP.NET Core y cada método de prueba. No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas solo contienen el contenido de los archivos de vista. No tendrá los vínculos de archivo generado de diseño para la `About` y `Contact` vistas, por lo que tendrá que invocar a los mismos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página contacto](mvc/_static/contact-page.png)

Tenga en cuenta la falta de un estilo y elementos de menú. Esto lo corregiremos en la sección siguiente.

## <a name="static-content"></a>Contenido estático

En versiones anteriores de ASP.NET MVC, contenido estático se alojó desde la raíz del proyecto web y se ha combinado con archivos de servidor. En el núcleo de ASP.NET, contenido estático se hospeda en el *wwwroot* carpeta. Desea copiar el contenido estático de la aplicación de MVC de ASP.NET anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core. En esta conversión de ejemplo:

* Copia la *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta del proyecto de ASP.NET Core.

La antigua ASP.NET MVC project utiliza [Bootstrap](https://getbootstrap.com/) para su aplicación de estilos y almacena el arranque de los archivos del *contenido* y *Scripts* carpetas. Hace referencia a la plantilla, que genera el proyecto de MVC de ASP.NET anterior, arranque en el archivo de diseño (*Views/Shared/_Layout.cshtml*). Podría copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto. En su lugar, vamos a agregar compatibilidad para el arranque (y otras bibliotecas de cliente) con CDN en la sección siguiente.

## <a name="migrate-the-layout-file"></a>Migrar el archivo de diseño

* Copia la *_ViewStart.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta. El *_ViewStart.cshtml* archivo no ha cambiado en MVC de ASP.NET Core.

* Crear un *vistas/compartidas* carpeta.

* *Opcional:* copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto MVC *vistas* carpeta en el proyecto de ASP.NET Core  *Vistas* carpeta. Quite las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo. El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y la pone [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro). Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño. El *_ViewImports.cshtml* archivo es una novedad de ASP.NET Core.

* Copia la *_Layout.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas/compartidas* carpeta en el proyecto de ASP.NET Core *vistas/compartidas* carpeta.

Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):

* Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento que se va a cargar *bootstrap.css* (ver abajo).

* Quite `@Scripts.Render("~/bundles/modernizr")`.

* Convierta en comentario la `@Html.Partial("_LoginPartial")` línea (incluya la línea con `@*...*@`). Se tendrá que volver a él en un tutorial posterior.

* Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).

* Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo).

El marcado de reemplazo para la inclusión de CSS de arranque:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

El marcado de reemplazo de jQuery y la inclusión de JavaScript de arranque:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

La actualización *_Layout.cshtml* archivo se muestra a continuación:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Ver el sitio en el explorador. Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.

* *Opcional:* desea intentar usar el nuevo archivo de diseño. Para este proyecto se puede copiar el archivo de diseño de la *FullAspNetCore* proyecto. El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y tiene otras mejoras.

## <a name="configure-bundling-and-minification"></a>Configurar la agrupación y minificación

Para obtener información sobre cómo configurar la agrupación y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Solucionar errores de HTTP 500

Hay muchos problemas que pueden provocar un mensaje de error HTTP 500 que no contienen ninguna información en el origen del problema. Por ejemplo, si la *Views/_ViewImports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500. De forma predeterminada en las aplicaciones ASP.NET Core, el `UseDeveloperExceptionPage` extensión se agrega a la `IApplicationBuilder` y se ejecuta cuando la configuración es *desarrollo*. Esto se detalla en el código siguiente:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core convierte las excepciones no controladas en una aplicación web en las respuestas de error HTTP 500. Normalmente, los detalles del error no se incluyen en estas respuestas para evitar la divulgación de información potencialmente confidencial acerca del servidor. Vea **mediante la página de excepción para desarrolladores** en [controlar los errores](../fundamentals/error-handling.md) para obtener más información.

## <a name="additional-resources"></a>Recursos adicionales

* [Desarrollo del lado cliente](xref:client-side/index)
* [Aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro)
