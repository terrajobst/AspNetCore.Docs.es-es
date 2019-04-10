---
title: Migración de ASP.NET MVC a ASP.NET Core MVC
author: ardalis
description: Obtenga información sobre cómo empezar a migrar un proyecto de MVC de ASP.NET a ASP.NET Core MVC.
ms.author: riande
ms.date: 04/06/2019
uid: migration/mvc
ms.openlocfilehash: a85b9f15be8ad9ca66b20ef1f4422fe67806a797
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2019
ms.locfileid: "59468545"
---
# <a name="migrate-from-aspnet-mvc-to-aspnet-core-mvc"></a>Migración de ASP.NET MVC a ASP.NET Core MVC

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Daniel Roth](https://github.com/danroth27), [Steve Smith](https://ardalis.com/), y [Scott Addie](https://scottaddie.com)

Este artículo muestra cómo empezar a migrar un proyecto ASP.NET MVC a [ASP.NET Core MVC](../mvc/overview.md). En el proceso, destaca muchas de las cosas que han cambiado respecto a ASP.NET MVC. Migración de ASP.NET MVC es un proceso de varios pasos y este artículo describe la configuración inicial, controladores básicos y vistas, contenido estático y las dependencias del lado cliente. Otros artículos cubren migrar la configuración y código de identidad que se encuentra en muchos proyectos de ASP.NET MVC.

> [!NOTE]
> Los números de versión en los ejemplos podrían no estar actualizados. Es posible que deba actualizar sus proyectos en consecuencia.

## <a name="create-the-starter-aspnet-mvc-project"></a>Crear el proyecto de ASP.NET MVC iniciador

Para demostrar la actualización, comenzamos creando una aplicación ASP.NET MVC. Crea uno con el nombre *WebApp1* para el espacio de nombres coincida con el proyecto de ASP.NET Core que creamos en el paso siguiente.

![El cuadro de diálogo de Visual Studio de nuevo proyecto](mvc/_static/new-project.png)

![Cuadro de diálogo nueva aplicación Web: Plantilla de proyecto MVC seleccionada en el panel de plantillas ASP.NET](mvc/_static/new-project-select-mvc-template.png)

*Opcional:* Cambiar el nombre de la solución desde *WebApp1* a *Mvc5*. Visual Studio muestra el nuevo nombre de la solución (*Mvc5*), lo que facilita indicar este proyecto desde el proyecto siguiente.

## <a name="create-the-aspnet-core-project"></a>Crear el proyecto de ASP.NET Core

Cree un nuevo *vacía* aplicación web de ASP.NET Core con el mismo nombre que el proyecto anterior (*WebApp1*) para que coincidan con los espacios de nombres en los dos proyectos. Tener el mismo espacio de nombres resulta más fácil copiar código entre los dos proyectos. Tendrá que crear este proyecto en un directorio diferente que el proyecto anterior para usar el mismo nombre.

![Cuadro de diálogo Nuevo proyecto](mvc/_static/new_core.png)

![Cuadro de diálogo nueva aplicación Web de ASP.NET: Plantilla de proyecto vacía seleccionada en el panel de plantillas de ASP.NET Core](mvc/_static/new-project-select-empty-aspnet5-template.png)

* *Opcional:* Crear una nueva aplicación ASP.NET Core mediante la *aplicación Web* plantilla de proyecto. Denomine el proyecto *WebApp1*y seleccione una opción de autenticación de **cuentas de usuario individuales**. Cambiar el nombre de esta aplicación *FullAspNetCore*. Este proyecto le ahorra tiempo a crear en la conversión. Puede mirar el código generado con plantilla para ver el resultado final o copie el código en el proyecto de conversión. También es útil cuando se bloquea en un paso de conversión que se compara con el proyecto generado con plantilla.

## <a name="configure-the-site-to-use-mvc"></a>Configurar el sitio para usar MVC

::: moniker range=">= aspnetcore-2.1"

* Cuando el destino es .NET Core, el [Microsoft.AspNetCore.App metapaquete](xref:fundamentals/metapackage-app) se hace referencia de forma predeterminada. Este paquete contiene paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC. Si el destino es .NET Framework, las referencias de paquete deben aparecer por separado en el archivo de proyecto.

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* Cuando el destino es .NET Core, el [metapaquete Microsoft.AspNetCore.All](xref:fundamentals/metapackage) se hace referencia de forma predeterminada. Este paquete contiene paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC. Si el destino es .NET Framework, las referencias de paquete deben aparecer por separado en el archivo de proyecto.

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* Cuando el destino es .NET Core o .NET Framework, los paquetes de paquetes que se usan con frecuencia las aplicaciones de MVC se enumeran individualmente en el archivo de proyecto.

::: moniker-end

`Microsoft.AspNetCore.Mvc` es el marco de ASP.NET Core MVC. `Microsoft.AspNetCore.StaticFiles` es el controlador de archivo estático. El tiempo de ejecución de ASP.NET Core es modular, y debe elegir explícitamente en para servir archivos estáticos (consulte [archivos estáticos](xref:fundamentals/static-files)).

* Abra el *Startup.cs* de archivos y cambiar el código para que coincida con lo siguiente:

  [!code-csharp[](mvc/sample/Startup.cs?highlight=13,26-31)]

El `UseStaticFiles` método de extensión agrega el controlador de archivos estáticos. Como se mencionó anteriormente, el tiempo de ejecución ASP.NET es modular, y debe elegir explícitamente en para servir archivos estáticos. El `UseMvc` método de extensión agrega enrutamiento. Para obtener más información, consulte [inicio de la aplicación](xref:fundamentals/startup) y [enrutamiento](xref:fundamentals/routing).

## <a name="add-a-controller-and-view"></a>Agregar un controlador y vista

En esta sección, agregará un controlador mínima y la vista para que actúe como marcadores de posición para el controlador de MVC de ASP.NET y las vistas, debe migrar en la sección siguiente.

* Agregar un *controladores* carpeta.

* Agregar un **clase de controlador** denominado *HomeController.cs* a la *controladores* carpeta.

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/add_mvc_ctl.png)

* Agregar un *vistas* carpeta.

* Agregar un *vistas/inicio* carpeta.

* Agregar un **vista Razor** denominado *Index.cshtml* a la *vistas/inicio* carpeta.

![Cuadro de diálogo Agregar nuevo elemento](mvc/_static/view.png)

A continuación se muestra la estructura del proyecto:

![Explorador de soluciones con archivos y carpetas de WebApp1](mvc/_static/project-structure-controller-view.png)

Reemplace el contenido de la *Views/Home/Index.cshtml* archivo por lo siguiente:

```html
<h1>Hello world!</h1>
```

Ejecutar la aplicación.

![Aplicación Web abierta en Microsoft Edge](mvc/_static/hello-world.png)

Consulte [controladores](xref:mvc/controllers/actions) y [vistas](xref:mvc/views/overview) para obtener más información.

Ahora que tenemos un proyecto ASP.NET Core trabajo mínimo, podemos comenzar a migrar la funcionalidad desde el proyecto de MVC de ASP.NET. Es necesario mover los siguientes:

* contenido del lado cliente (CSS, fuentes y scripts)

* controladores

* vistas

* modelos

* La Unión

* filtros

* Registro de entrada/salida, la identidad (Esto se hace en el siguiente tutorial.)

## <a name="controllers-and-views"></a>Controladores y vistas

* Copie cada uno de los métodos de ASP.NET MVC `HomeController` al nuevo `HomeController`. Tenga en cuenta que en ASP.NET MVC, tipo devuelto de método de acción de controlador de la plantilla integrada [ActionResult](https://msdn.microsoft.com/library/system.web.mvc.actionresult(v=vs.118).aspx); en ASP.NET Core MVC, los métodos de acción devueltos `IActionResult` en su lugar. `ActionResult` implementa `IActionResult`, por lo que no es necesario para cambiar el tipo de valor devuelto de los métodos de acción.

* Copia el *About.cshtml*, *Contact.cshtml*, y *Index.cshtml* archivos de vista de Razor para el proyecto de ASP.NET Core desde el proyecto de ASP.NET MVC.

* Ejecute la aplicación de ASP.NET Core y cada método de prueba. No hemos migramos los estilos o el archivo de diseño, por lo que las vistas representadas contienen solo el contenido de los archivos de vista. No tendrá los vínculos de archivo generado de diseño para el `About` y `Contact` vistas, por lo que tendrá que invocar a ellos desde el explorador (reemplace **4492** con el número de puerto utilizado en el proyecto).

  * `http://localhost:4492/home/about`

  * `http://localhost:4492/home/contact`

![Página de contacto](mvc/_static/contact-page.png)

Tenga en cuenta la falta de estilo y elementos de menú. Esto lo corregiremos en la sección siguiente.

## <a name="static-content"></a>Contenido estático

En versiones anteriores de ASP.NET MVC, contenido estático se hospedaba desde la raíz del proyecto web y se ha combinado con los archivos del servidor. En ASP.NET Core, el contenido estático se hospeda en el *wwwroot* carpeta. Desea copiar el contenido estático de la antigua aplicación ASP.NET MVC para la *wwwroot* carpeta del proyecto de ASP.NET Core. En esta conversión de ejemplo:

* Copia el *favicon.ico* archivo desde el proyecto MVC anterior a la *wwwroot* carpeta en el proyecto de ASP.NET Core.

Proyecto de la antigua de ASP.NET MVC usa [Bootstrap](https://getbootstrap.com/) para su estilo y almacenes de archivos de la secuencia de inicio de la *contenido* y *Scripts* carpetas. La plantilla, que genera el proyecto de ASP.NET MVC anterior, hace referencia a Bootstrap en el archivo de diseño (*Views/Shared/_Layout.cshtml*). Puede copiar el *bootstrap.js* y *bootstrap.css* proyecto de archivos de ASP.NET MVC a la *wwwroot* carpeta en el nuevo proyecto. En su lugar, vamos a agregar compatibilidad con Bootstrap (y otras bibliotecas de cliente) uso de CDN en la sección siguiente.

## <a name="migrate-the-layout-file"></a>Migrar el archivo de diseño

* Copia el *_ViewStart.cshtml* archivo desde el proyecto de ASP.NET MVC antiguo *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta. El *_ViewStart.cshtml* archivo no ha cambiado en ASP.NET Core MVC.

* Crear un *Views/Shared* carpeta.

* *Opcional:* Copia *_ViewImports.cshtml* desde el *FullAspNetCore* del proyecto de MVC *vistas* carpeta en el proyecto de ASP.NET Core *vistas* carpeta. Quitar las declaraciones de espacio de nombres en el *_ViewImports.cshtml* archivo. El *_ViewImports.cshtml* archivo proporciona los espacios de nombres para todos los archivos de vista y se pone [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro). Aplicaciones auxiliares de etiquetas se usan en el nuevo archivo de diseño. El *_ViewImports.cshtml* archivo es nuevo en ASP.NET Core.

* Copia el *_Layout.cshtml* archivo desde el proyecto de ASP.NET MVC antiguo *Views/Shared* carpeta en el proyecto de ASP.NET Core *Views/Shared* carpeta.

Abra *_Layout.cshtml* de archivos y realice los cambios siguientes (el código completo se muestra a continuación):

* Reemplace `@Styles.Render("~/Content/css")` con un `<link>` elemento cargar *bootstrap.css* (ver abajo).

* Quite `@Scripts.Render("~/bundles/modernizr")`.

* Marque como comentario el `@Html.Partial("_LoginPartial")` línea (Rodear con la línea de `@*...*@`). Para obtener más información, consulte [migrar autenticación e Identity a ASP.NET Core](xref:migration/identity)

* Reemplace `@Scripts.Render("~/bundles/jquery")` con un `<script>` elemento (ver abajo).

* Reemplace `@Scripts.Render("~/bundles/bootstrap")` con un `<script>` elemento (ver abajo).

El marcado de reemplazo para la inclusión de CSS de Bootstrap:

```html
<link rel="stylesheet"
    href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css"
    integrity="sha384-BVYiiSIFeK1dGmJRAkycuHAHRg32OmUcww7on3RYdg4Va+PmSTsz/K68vbdEjh4u"
    crossorigin="anonymous">
```

El marcado de reemplazo para jQuery y la inclusión de JavaScript de Bootstrap:

```html
<script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"
    integrity="sha384-Tc5IQib027qvyjSMfHjOMaLkfuWVxZxUPnCJA7l2mCWNIpG9mGCD8wGNIcPD7Txa" crossorigin="anonymous"></script>
```

La actualización *_Layout.cshtml* archivo se muestra a continuación:

[!code-cshtml[](mvc/sample/Views/Shared/_Layout.cshtml?highlight=7-10,29,41-44)]

Ver el sitio en el explorador. Ahora deberían cargarse correctamente, con los estilos esperados en su lugar.

* *Opcional:* Es posible que desee intentar usar el nuevo archivo de diseño. Para este proyecto puede copiar el archivo de diseño de la *FullAspNetCore* proyecto. El nuevo archivo de diseño usa [aplicaciones auxiliares de etiquetas](xref:mvc/views/tag-helpers/intro) y tiene otras mejoras.

## <a name="configure-bundling-and-minification"></a>Configurar la unión y minificación

Para obtener información sobre cómo configurar la unión y minificación, consulte [unión y Minificación](../client-side/bundling-and-minification.md).

## <a name="solve-http-500-errors"></a>Solucionar errores HTTP 500

Hay muchos problemas que pueden causar un mensaje de error HTTP 500 que no contienen información sobre el origen del problema. Por ejemplo, si la *Views/_viewimports.cshtml* archivo contiene un espacio de nombres que no existe en el proyecto, obtendrá un error HTTP 500. De forma predeterminada en las aplicaciones ASP.NET Core, el `UseDeveloperExceptionPage` extensión se agrega a la `IApplicationBuilder` y se ejecutan cuando la configuración está *desarrollo*. Esto se detalla en el código siguiente:

[!code-csharp[](mvc/sample/Startup.cs?highlight=19-22)]

ASP.NET Core convierte las excepciones no controladas en una aplicación web en las respuestas de error HTTP 500. Normalmente, los detalles del error no se incluyen en estas respuestas para evitar la divulgación de información potencialmente confidencial sobre el servidor. Consulte **mediante la página de excepciones de desarrollador** en [controlar los errores](../fundamentals/error-handling.md) para obtener más información.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:razor-components/index>
* <xref:mvc/views/tag-helpers/intro>
