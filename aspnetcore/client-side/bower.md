---
title: Uso de Bower en ASP.NET Core
author: rick-anderson
description: Administrar paquetes de cliente con Bower.
manager: wpickett
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 02/14/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/bower
ms.openlocfilehash: ee628ee14aa38969cdb4443718c378fd36192596
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/11/2018
---
# <a name="manage-client-side-packages-with-bower-in-aspnet-core"></a>Administrar paquetes de cliente con Bower en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT), [Noel arroz](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/), y [Scott Addie](https://scottaddie.com) 

> [!IMPORTANT]
> Mientras se mantiene Bower, sus mantenedores recomienda utilizar una solución distinta. Yarn con Webpack es una alternativa para la que [instrucciones de migración](https://bower.io/blog/2017/how-to-migrate-away-from-bower/) están disponibles.

[Bower](https://bower.io/) llama a sí mismo "Administrador de paquetes para la web". Dentro del ecosistema de. NET, que se llena el espacio vacío a la izquierda incapacidad de NuGet para entregar archivos de contenido estático. Para los proyectos de ASP.NET Core, estos archivos estáticos son inherentes a las bibliotecas de cliente como [jQuery](http://jquery.com/) y [arranque](http://getbootstrap.com/). Para las bibliotecas. NET, seguir usando [NuGet](https://www.nuget.org/) Administrador de paquetes.

Proceso de compilación de proyectos nuevos creados con las plantillas de proyecto de ASP.NET Core configurar el cliente. [jQuery](http://jquery.com/) y [arranque](http://getbootstrap.com/) están instalados, y se admite Bower.

Paquetes de cliente se muestran en la *bower.json* archivo. Configura las plantillas de proyecto de ASP.NET Core *bower.json* con jQuery, validación de jQuery y arranque.

En este tutorial, vamos a agregar compatibilidad para [fuente Maravilla](http://fontawesome.io). Se pueden instalar paquetes de bower con el **administrar paquetes de Bower** interfaz de usuario o manualmente en el *bower.json* archivo.

### <a name="installation-via-manage-bower-packages-ui"></a>Instalación mediante la opción administrar paquetes de Bower interfaz de usuario

* Crear una nueva aplicación Web de ASP.NET Core con el **aplicación Web de ASP.NET Core (.NET Core)** plantilla. Seleccione **aplicación Web** y **sin autenticación**.

* Haga clic en el proyecto en el Explorador de soluciones y seleccione **administrar paquetes de Bower** (o bien en el menú principal, **proyecto** > **administrar paquetes de Bower**).

* En el **Bower: \<nombre del proyecto\>**  ventana, haga clic en la ficha "Examinar" y, a continuación, filtre la lista de paquetes especificando `font-awesome` en el cuadro de búsqueda:

 ![Administrar paquetes de bower](bower/_static/manage-bower-packages.png)

* Confirme que la "guardar los cambios en *bower.json*" casilla de verificación está activada. Seleccione una versión de la lista desplegable y haga clic en el **instalar** botón. El **salida** ventana muestra los detalles de instalación.

### <a name="manual-installation-in-bowerjson"></a>Instalación manual en bower.json

Abra la *bower.json* de archivos y agregar "fuente maravillosa" a las dependencias. IntelliSense muestra los paquetes disponibles. Cuando se selecciona un paquete, se muestran las versiones disponibles. Las siguientes imágenes anteriores y no coincide con lo que se ve.

![IntelliSense de explorador de paquetes de bower](bower/_static/add-package.png)

![versión de bower IntelliSense](bower/_static/version-intelliSense.png)

Usos de bower [control de versiones semántico](http://semver.org/) para organizar las dependencias. Control de versiones semántico, también conocido como SemVer, identifica los paquetes con el esquema de numeración \<principal >.\< secundaria >. \<revisión >. IntelliSense simplifica el control de versiones semántico presentando unas cuantas opciones comunes. El elemento superior en la lista de IntelliSense (4.6.3 en el ejemplo anterior) se considera la versión estable más reciente del paquete. El símbolo de intercalación (^) coincide con la versión principal más reciente y la tilde (~) coincide con la versión secundaria más reciente.

Guardar el *bower.json* archivo. Visual Studio busca la *bower.json* archivo para los cambios. Al guardar, el *bower install* se ejecuta el comando. Vea la ventana de salida **npm/Bower** vista para el comando exacto que se ejecuta.

Abra la *bowerrc* de archivos en *bower.json*. El `directory` propiedad está establecida en *wwwroot/lib* que indica la ubicación Bower instalará los activos de paquete.

```json
{
 "directory": "wwwroot/lib"
}
```

Puede usar el cuadro de búsqueda en el Explorador de soluciones para buscar y mostrar el paquete de fuente Maravilla.

Abra la *Views\Shared\_Layout.cshtml* de archivos y agregar el archivo CSS de fuente Maravilla en el entorno de [etiqueta auxiliar](xref:mvc/views/tag-helpers/intro) para `Development`. En el Explorador de soluciones, arrastre y coloque *fuente awesome.css* dentro de la `<environment names="Development">` elemento.

[!code-html[Main](bower/sample/_Layout.cshtml?highlight=4&range=9-13)]

En una aplicación de producción agregaría *fuente awesome.min.css* a la aplicación auxiliar de etiquetas de entorno para `Staging,Production`.

Reemplace el contenido de la *Views\Home\About.cshtml* archivo Razor con el marcado siguiente:

[!code-html[Main](bower/sample/About.cshtml)]

Ejecutar la aplicación y navegue hasta la vista acerca para comprobar el funciona de Maravilla de fuente del paquete.

## <a name="exploring-the-client-side-build-process"></a>Explorar el proceso de compilación de cliente

Plantillas de proyecto de ASP.NET Core mayoría ya están configuradas para usar Bower. En este tutorial siguiente comienza con un proyecto vacío de ASP.NET Core y agrega cada pieza manualmente, por lo que puede hacerse una idea de cómo se usa Bower en un proyecto. Puede ver lo que ocurre con la estructura del proyecto y el tiempo de ejecución como resultado que se realiza cada cambio de configuración.

Los pasos generales para usar el proceso de compilación de cliente con Bower son:

* Definir los paquetes utilizados en el proyecto. <!-- once defined, you don't need to download them, VS does -->
* Paquetes de referencia desde las páginas web.

### <a name="define-packages"></a>Definir paquetes

Una vez que enumerar los paquetes en el *bower.json* archivo, Visual Studio, descargará. En el ejemplo siguiente se utiliza Bower para cargar jQuery y arranque a la *wwwroot* carpeta.

* Crear una nueva aplicación Web de ASP.NET Core con el **aplicación Web de ASP.NET Core (.NET Core)** plantilla. Seleccione el **vacía** plantilla de proyecto y haga clic en **Aceptar**.

* En el Explorador de soluciones, haga clic en el proyecto > **Agregar nuevo elemento** y seleccione **archivo de configuración de Bower**. Nota: Una *bowerrc* también se agrega el archivo.

* Abra *bower.json*y agregar jquery y arrancar en el `dependencies` sección. Resultante *bower.json* archivo tendrá un aspecto similar al ejemplo siguiente. Las versiones cambiarán con el tiempo y no pueden coincidir con la imagen siguiente.

[!code-json[Main](bower/sample/bower.json?highlight=5,6)]

* Guardar el *bower.json* archivo.

 Compruebe que el proyecto incluye la *arranque* y *jQuery* directorios en *wwwroot/lib*. Bower utiliza el *bowerrc* archivo para instalar los activos en *wwwroot/lib*.

 Nota: La interfaz de usuario "Administrar paquetes de Bower" proporciona una alternativa a modificar el archivo manualmente.

### <a name="enable-static-files"></a>Habilitar archivos estáticos

* Agregar el `Microsoft.AspNetCore.StaticFiles` paquete NuGet para el proyecto.
* Habilitar archivos estáticos que se sirvan con el [middleware de archivos estáticos](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions). Agregue una llamada a [UseStaticFiles](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.staticfileextensions) a la `Configure` método `Startup`.

[!code-csharp[Main](bower/sample/Startup.cs?highlight=9)]

### <a name="reference-packages"></a>Paquetes de referencia

En esta sección, creará una página HTML para comprobar puede tener acceso a los paquetes implementados.

* Agregar una nueva página HTML denominada *Index.html* a la *wwwroot* carpeta. Nota: Debe agregar el archivo HTML para la *wwwroot* carpeta. De forma predeterminada, no se pueda servir contenido estático fuera *wwwroot*. Vea [trabajar con archivos estáticos](xref:fundamentals/static-files) para obtener más información.

 Reemplace el contenido de *Index.html* con el siguiente marcado:

[!code-html[Main](bower/sample/Index.html)]

* Ejecute la aplicación y vaya a `http://localhost:<port>/Index.html`. O bien, con *Index.html* abierto, presione `Ctrl+Shift+W`. Compruebe que se aplica el estilo jumbotron, el código de jQuery responde cuando se hace clic en el botón y que el botón de arranque cambia el estado.

 ![estilo de Jumbotron aplicado](bower/_static/jumbotron.png)
