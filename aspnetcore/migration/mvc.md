---
title: "Migración de MVC de ASP.NET a núcleo de ASP.NET MVC"
author: ardalis
description: 
keywords: "Núcleo de ASP.NET, MVC, migrar"
ms.author: riande
manager: wpickett
ms.date: 03/07/2017
ms.topic: article
ms.assetid: 3155cc9e-d0c9-424b-886c-35c0ec6f9f4e
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/mvc
ms.openlocfilehash: ccdceed927d90a1f3201be9d9f92ebb4f2f66e66
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="migrating-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migración de MVC de ASP.NET a núcleo de ASP.NET MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](http://ardalis.com), y [Scott Addie](https://scottaddie.com)

Este artículo muestra cómo empezar a migrar un proyecto de MVC de ASP.NET a [MVC de ASP.NET Core](../mvc/overview.md). En el proceso, resalta muchas de las cosas que han cambiado respecto a ASP.NET MVC. La migración de ASP.NET MVC es un proceso en varias fases y este artículo trata la instalación inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente. Migrar la configuración y el código de identidad que se encuentra en muchos proyectos de ASP.NET MVC tratan otros artículos.

> [!NOTE]
> Los números de versión en los ejemplos podrían no estar actualizados. Debe actualizar los proyectos en consecuencia.

## <a name="create-the-starter-aspnet-mvc-project"></a>Crear el proyecto de MVC de ASP.NET de inicio

Para demostrar la actualización, comenzaremos mediante la creación de una aplicación de ASP.NET MVC. Crear con el nombre *WebApp1* para el espacio de nombres coincidirá con el proyecto de ASP.NET Core que creamos en el paso siguiente.

![El cuadro de diálogo de Visual Studio nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: plantilla de proyecto MVC seleccionado en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcional:* cambiar el nombre de la solución de *WebApp1* a *Mvc5*. Visual Studio mostrará el nuevo nombre de la solución (*Mvc5*), que resultará más fácil indicar a este proyecto desde el proyecto siguiente.

## <a name="create-the-aspnet-core-project"></a>Crear el proyecto de ASP.NET Core

Crear un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos. Tener el mismo espacio de nombres resulta más fácil copiar el código entre los dos proyectos. Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para utilizar el mismo nombre.

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: plantilla de proyecto vacía seleccionada en el panel de plantillas de núcleo de ASP.NET](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcional:* crear una nueva aplicación ASP.NET Core usando la *aplicación Web* plantilla de proyecto. Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**. Cambiar el nombre de esta aplicación *FullAspNetCore*. Creación de este proyecto, ahorrará tiempo en la conversión. También puede buscar en el código generado de plantilla para ver el resultado final o copiar el código al proyecto de conversión. También es útil cuando bloquea en un paso de la conversión que se compara con el proyecto de plantilla generado.

## <a name="configure-the-site-to-use-mvc"></a>Configurar el sitio para usar MVC

* Instalar el `Microsoft.AspNetCore.Mvc` y `Microsoft.AspNetCore.StaticFiles` paquetes de NuGet.

  `Microsoft.AspNetCore.Mvc`es el marco de MVC de ASP.NET Core. `Microsoft.AspNetCore.StaticFiles`es el controlador de archivos estáticos. El tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos (vea [trabajar con archivos estáticos](../fundamentals/static-files.md)).

* Abra la *.csproj* archivo (haga clic en el proyecto en **el Explorador de soluciones** y seleccione **editar WebApp1.csproj**) y agregue un `PrepareForPublish` destino:

  [!code-xml[Main](mvc/sample/WebApp1.csproj?range=21-23)]

  El `PrepareForPublish` destino es necesario para adquirir las bibliotecas de cliente a través de Bower. Hablaremos sobre el más adelante.

* Abra la *Startup.cs* y cambie el código para que coincida con lo siguiente:

  [!code-csharp[Main](mvc/sample/Startup.cs?highlight=14,27-34)]

  El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos. Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular y explícitamente debe participar en servir archivos estáticos. El `UseMvc` método de extensión agrega enrutamiento. Para obtener más información, consulte [inicio de la aplicación](../fundamentals/startup.md) y [enrutamiento](../fundamentals/routing.md).

## <a name="add-a-controller-and-view"></a>Agregar un controlador y una vista

En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas a migrar en la sección siguiente.

* Agregar un *controladores* carpeta.

* Agregar un **clase de controlador MVC** con el nombre *HomeController.cs* a la *controladores* carpeta.

![Agregar cuadro de diálogo nuevo elemento](mvc/_static/add_mvc_ctl.png)

* Agregar un *vistas* carpeta.

* Agregar un *vistas/inicio* carpeta.

* Agregar un *Index.cshtml* página de vista de MVC a la *vistas/inicio* carpeta.

![Agregar cuadro de diálogo nuevo elemento](mvc/_static/view.png)

A continuación se muestra la estructura del proyecto:

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

Reemplace el contenido de la *Views/Home/Index.cshtml* archivo con lo siguiente:

```html
<h1>Hello world!</h1>
```

Ejecutar la aplicación.

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

Vea [controladores](../mvc/controllers/index.md) y [vistas](../mvc/views/index.md) para obtener más información.

Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, se podemos empezar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET. Tendrá que mover lo siguiente:

* contenido del lado cliente (CSS, fuentes y secuencias de comandos)

* controladores

* vistas

* modelos

* Cómo agrupar

* filtros

* Registro de entrada/salida, la identidad (Esto se hace en el tutorial siguiente).

## <a name="controllers-and-views"></a>Controladores y vistas

* Copiar cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`. Tenga en cuenta que en ASP.NET MVC, tipo de valor devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en el núcleo de ASP.NET MVC, los métodos de acción devueltos `IActionResult` en su lugar. `ActionResult`implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.

* Copia la *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista Razor desde el proyecto de MVC de ASP.NET para el proyecto de ASP.NET Core.

* Ejecutar la aplicación de ASP.NET Core y cada método de prueba. No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas solo contendrá el contenido de los archivos de vista. No tendrá los vínculos de archivo generado de diseño para la `About` y `Contact` vistas, por lo que tendrá que invocar a los mismos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página contacto](mvc/_static/contact-page.png)

Tenga en cuenta la falta de un estilo y elementos de menú. Se corregirá en la siguiente sección.

## <a name="static-content"></a>Contenido estático

En versiones anteriores de ASP.NET MVC, contenido estático se alojó desde la raíz del proyecto web y se ha combinado con archivos de servidor. En el núcleo de ASP.NET, contenido estático se hospeda en el *wwwroot* carpeta. Desea copiar el contenido estático de la aplicación de MVC de ASP.NET anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core. En esta conversión de ejemplo:

* Copia la *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta del proyecto de ASP.NET Core.

La antigua ASP.NET MVC project utiliza [Bootstrap](http://getbootstrap.com/) para su aplicación de estilos y almacena el arranque de los archivos del *contenido* y *Scripts* carpetas. Hace referencia a la plantilla, que genera el proyecto de MVC de ASP.NET anterior, arranque en el archivo de diseño (*Views/Shared/_Layout.cshtml*). Podría copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto, pero ese método no usa el mecanismo mejorado para administrar las dependencias del lado cliente en ASP.NET Core.

En el nuevo proyecto, vamos a agregar compatibilidad con arranque (y otras bibliotecas de cliente) con [Bower](http://bower.io/):

* Agregar un [Bower](http://bower.io/) archivo de configuración denominado *bower.json* a la raíz del proyecto (haga doble clic en el proyecto y, a continuación, **Agregar > nuevo elemento > archivo de configuración de Bower**). Agregar [arranque](http://getbootstrap.com/) y [jQuery](https://jquery.com/) al archivo (vea las líneas resaltadas a continuación).

  [!code-json[Main](mvc/sample/bower.json?highlight=5-6)]

Al guardar el archivo, Bower descargará automáticamente las dependencias para el *wwwroot/lib* carpeta. Puede usar el **el Explorador de soluciones de búsqueda** cuadro para buscar la ruta de acceso de los activos:

![activos de jQuery que se muestra en los resultados de búsqueda del explorador de soluciones](mvc/_static/search.png)

Vea [administrar paquetes de cliente con Bower](../client-side/bower.md) para obtener más información.

<a name=migrate-layout-file></a>

## <a name="migrate-the-layout-file"></a>Migrar el archivo de diseño

* Copia la *_ViewStart.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta. El *_ViewStart.cshtml* archivo no ha cambiado en MVC de ASP.NET Core.

* Crear un *vistas/compartidas* carpeta.

* *Opcional:* copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto MVC *vistas* carpeta en el proyecto de ASP.NET Core *Vistas* carpeta. Quite las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo. El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y la pone [aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md). Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño. El *_ViewImports.cshtml* archivo es una novedad de ASP.NET Core.

* Copia la *_Layout.cshtml* archivo desde el proyecto de MVC de ASP.NET anterior *vistas/compartidas* carpeta en el proyecto de ASP.NET Core *vistas/compartidas* carpeta.

Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):

   * Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento que se va a cargar *bootstrap.css* (ver abajo).

   * Quitar `@Scripts.Render("~/bundles/modernizr")`.

   * Convierta en comentario la `@Html.Partial("_LoginPartial")` línea (incluya la línea con `@*...*@`). Se tendrá que volver a él en un tutorial posterior.

   * Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).

   * Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo)...

El vínculo CSS de reemplazo:

```html
<link rel="stylesheet" href="~/lib/bootstrap/dist/css/bootstrap.css" />
```

Las etiquetas de script de reemplazo:

```html
<script src="~/lib/jquery/dist/jquery.js"></script>
<script src="~/lib/bootstrap/dist/js/bootstrap.js"></script>
```

La actualización *_Layout.cshtml* archivo se muestra a continuación:

[!code-html[Main](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7,27,39-40)]

Ver el sitio en el explorador. Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.

* *Opcional:* desea intentar usar el nuevo archivo de diseño. Para este proyecto se puede copiar el archivo de diseño de la *FullAspNetCore* proyecto. El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md) y tiene otras mejoras.

## <a name="configure-bundling--minification"></a>Configurar cómo agrupar & Minificación

Para obtener información sobre cómo configurar la agrupación y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).

## <a name="solving-http-500-errors"></a>Para resolver errores de HTTP 500

Hay muchos problemas que pueden provocar un mensaje de error HTTP 500 que no contienen ninguna información en el origen del problema. Por ejemplo, si la *Views/_ViewImports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500. Para obtener un mensaje de error detallado, agregue el código siguiente:

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    if (env.IsDevelopment())
    {
         app.UseDeveloperExceptionPage();
    }

    app.UseStaticFiles();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

Vea **mediante la página de excepción para desarrolladores** en [control de errores](../fundamentals/error-handling.md) para obtener más información.

## <a name="additional-resources"></a>Recursos adicionales

* [Desarrollo de cliente](../client-side/index.md)

* [Aplicaciones auxiliares de etiquetas](../mvc/views/tag-helpers/index.md)
