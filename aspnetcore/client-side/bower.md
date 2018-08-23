---
title: Administrar los paquetes del lado cliente con Bower en ASP.NET Core
author: rick-anderson
description: Administración de paquetes del lado cliente con Bower.
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 08/09/2018
uid: client-side/bower
ms.openlocfilehash: 8606c21596a5d9d6ada9c60b55b2f54da21c601b
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2018
ms.locfileid: "41902724"
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Administrar los paquetes del lado cliente con Bower en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), y [Scott Addie](https://scottaddie.com)

> [!IMPORTANT]
> Mientras se mantiene Bower, sus maintainers recomienda usar una solución diferente. [Administrador de bibliotecas](https://blogs.msdn.microsoft.com/webdev/2018/04/18/what-happened-to-bower/) (LibMan para abreviar) es la herramienta de adquisición de biblioteca de cliente nuevo de Visual Studio (Visual Studio 15,8 o posterior). Para obtener más información, consulta <xref:client-side/libman/index>. Bower es compatible con Visual Studio a través de la versión 15.5.
>
> Yarn con Webpack es una alternativa popular para el que [instrucciones de migración](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) están disponibles.

[Bower](https://bower.io/) llama a sí mismo "Administrador de paquetes para la web". En el ecosistema de. NET, llena este vacío a la incapacidad de NuGet para entregar archivos de contenido estático a la izquierda. Para los proyectos de ASP.NET Core, estos archivos estáticos son inherentes a las bibliotecas de cliente como [jQuery](http://jquery.com/) y [Bootstrap](http://getbootstrap.com/). Para las bibliotecas. NET, seguir usando [NuGet](https://www.nuget.org/) Administrador de paquetes.

Proceso de compilación de nuevos proyectos creados con las plantillas de proyecto de ASP.NET Core que configurar el cliente. [jQuery](http://jquery.com/) y [Bootstrap](http://getbootstrap.com/) están instalados, y es compatible con Bower.

Los paquetes del lado cliente se muestran en el *bower.json* archivo. Las plantillas de proyecto de ASP.NET Core configura *bower.json* con jQuery, validación de jQuery y Bootstrap.

En este tutorial, vamos a agregar compatibilidad para [Font Awesome](http://fontawesome.io). Los paquetes de bower pueden instalarse con la **administrar paquetes de Bower** la interfaz de usuario o manualmente en el *bower.json* archivo.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalación a través de paquetes de Bower de administrar la interfaz de usuario

* Crear una nueva aplicación Web de ASP.NET Core con la **aplicación Web de ASP.NET Core (.NET Core)** plantilla. Seleccione **aplicación Web** y **sin autenticación**.

* Haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes de Bower** (o bien en el menú principal, **proyecto** > **administrar paquetes de Bower**).

* En el **Bower: \<nombre del proyecto\>**  , haga clic en la pestaña "Examinar" y, a continuación, filtre la lista de paquetes escribiendo `font-awesome` en el cuadro de búsqueda:

  ![Administrar paquetes de bower](bower/_static/manage-bower-packages.png)

* Confirme que el "guardar los cambios en *bower.json*" está activada la casilla de verificación. Seleccione una versión de la lista desplegable y haga clic en el **instalar** botón. El **salida** ventana muestra los detalles de instalación.

### <a name="manual-installation-in-bowerjson"></a>Instalación manual en bower.json

Abra el *bower.json* archivo y agregue "font awesome" a las dependencias. IntelliSense muestra los paquetes disponibles. Cuando se selecciona un paquete, se muestran las versiones disponibles. Las imágenes siguientes sean más antiguas y no coincidirán con lo que ve.

![IntelliSense de explorador de paquetes de bower](bower/_static/add-package.png)

![versión de bower IntelliSense](bower/_static/version-intelliSense.png)

Usos de bower [versionamiento semántico](http://semver.org/) para organizar las dependencias. Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<principal >.\< secundaria >. \<revisión >. IntelliSense simplifica el control de versiones semántico mostrando solo algunas opciones comunes. El elemento superior de la lista de IntelliSense (4.6.3 en el ejemplo anterior) se considera la última versión estable del paquete. El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) con la versión secundaria más reciente.

Guardar el *bower.json* archivo. Visual Studio inspecciona el *bower.json* archivo para los cambios. Al guardar, el *bower install* se ejecuta el comando. Vea la ventana de salida **Bower o npm** vista para el comando exacto que se ejecuta.

Abra el *bowerrc* archivo *bower.json*. El `directory` propiedad está establecida en *wwwroot/lib* que indica la ubicación Bower instalará los activos del paquete.

```json
{
 "directory": "wwwroot/lib"
}
```

Puede usar el cuadro de búsqueda en el Explorador de soluciones para buscar y mostrar el paquete font awesome.

Abra el *Views\Shared\_Layout.cshtml* y agréguele el archivo CSS font awesome el entorno [aplicación auxiliar de etiquetas](xref:mvc/views/tag-helpers/intro) para `Development`. En el Explorador de soluciones, arrastre y coloque *fuente awesome.css* dentro de la `<environment names="Development">` elemento.

[!code-html[](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

En una aplicación de producción agregaría *fuente awesome.min.css* a la aplicación auxiliar de etiquetas de entorno para `Staging,Production`.

Reemplace el contenido de la *Views\Home\About.cshtml* archivo Razor por el marcado siguiente:

[!code-html[](bower/sample/About.cshtml)]

Ejecute la aplicación y vaya a la vista About para comprobar que funciona la impresionante de fuente del paquete.

## <a name="exploring-the-client-side-build-process"></a>Explorar el proceso de compilación del lado cliente

La mayoría de las plantillas de proyecto de ASP.NET Core ya están configuradas para usar Bower. En este tutorial siguiente se inicia con un proyecto vacío de ASP.NET Core y agrega cada pieza manualmente, por lo que puede hacerse una idea de cómo se usa Bower en un proyecto. Puede ver lo que ocurre con la estructura del proyecto y el tiempo de ejecución tal como se realiza cada cambio de configuración de salida.

Los pasos generales para usar el proceso de compilación del lado cliente con Bower son:

* Definir los paquetes utilizados en el proyecto. <!-- once defined, you don't need to download them, VS does -->
* Paquetes de referencia de las páginas web.

### <a name="define-packages"></a>Definir paquetes

Una vez que la lista de paquetes en el *bower.json* archivo, Visual Studio, descargará. En el ejemplo siguiente se usa Bower para cargar jQuery y Bootstrap para el *wwwroot* carpeta.

* Crear una nueva aplicación Web de ASP.NET Core con la **aplicación Web de ASP.NET Core (.NET Core)** plantilla. Seleccione el **vacía** plantilla de proyecto y haga clic en **Aceptar**.

* En el Explorador de soluciones, haga clic en el proyecto > **Agregar nuevo elemento** y seleccione **archivo de configuración de Bower**. Nota: Una *bowerrc* también se agrega el archivo.

* Abra *bower.json*así como agregar jquery y bootstrap para el `dependencies` sección. Resultante *bower.json* archivo tendrá un aspecto similar al ejemplo siguiente. Las versiones cambiarán con el tiempo y no pueden coincidir con la imagen siguiente.

[!code-json[](bower/sample/bower.json?highlight=5,6)]

* Guardar el *bower.json* archivo.

  Compruebe que el proyecto incluye el *bootstrap* y *jQuery* directorios en *wwwroot/lib*. Bower usa el *bowerrc* archivo para instalar los activos de *wwwroot/lib*.

  Nota: La interfaz de usuario "Administrar paquetes de Bower" proporciona una alternativa a modificar el archivo manualmente.

### <a name="enable-static-files"></a>Habilitar archivos estáticos

* Agregar el `Microsoft.AspNetCore.StaticFiles` paquete NuGet al proyecto.
* Habilitar archivos estáticos atender las solicitudes con el [middleware de archivos estáticos](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions). Agregue una llamada a [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions) a la `Configure` método `Startup`.

[!code-csharp[](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Paquetes de referencia

En esta sección, creará una página HTML para comprobar puede tener acceso a los paquetes implementados.

* Agregue una nueva página HTML denominada *Index.html* a la *wwwroot* carpeta. Nota: Debe agregar el archivo HTML para el *wwwroot* carpeta. De forma predeterminada, no se pueden atender contenido estático fuera *wwwroot*. Consulte [archivos estáticos](xref:fundamentals/static-files) para obtener más información.

  Reemplace el contenido de *Index.html* con el siguiente marcado:

[!code-html[](bower/sample/Index.html)]

* Ejecute la aplicación y vaya a `http://localhost:<port>/Index.html`. Como alternativa, con *Index.html* abierto, presione `Ctrl+Shift+W`. Compruebe que se aplica el estilo jumbotron, el código jQuery responde cuando se hace clic en el botón y que el botón Bootstrap cambia de estado.

  ![estilo de Jumbotron aplicado](bower/_static/jumbotron.png)
