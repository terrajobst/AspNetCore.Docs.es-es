---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: "Parte 4: Agregar una vista de administración | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/04/2012
ms.topic: article
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: 2960eee37201655a9e4632bf0196ba18a0e2e82a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Agregar una vista de administración
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Agregar una vista de administración

Ahora se podrá activar en el lado del cliente y agregar una página que puede consumir datos desde el controlador de administración. La página le permitirá a los usuarios crear, editar o eliminar productos, mediante el envío de solicitudes de AJAX al controlador.

En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo denominado HomeController.cs. Este archivo contiene un controlador MVC. Agregue un método denominado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

El **HttpRouteUrl** método crea el identificador URI a la API web y se almacena en el contenedor de la vista para más tarde.

A continuación, coloque el cursor de texto dentro de la `Admin` método de acción, a continuación, menú contextual y seleccione **agregar vista**. Esto le llevará a la **agregar vista** cuadro de diálogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

En el **agregar vista** cuadro de diálogo, nombre de la vista "Admin". Active la casilla de verificación **crear una vista fuertemente tipada**. En **clase modelo**, seleccione "Product (ProductStore.Models)". Deje el resto de opciones como sus valores predeterminados.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Haga clic en **agregar** agrega un archivo denominado Admin.cshtml en vistas/inicio. Abra este archivo y agregue el siguiente código HTML. Este código HTML define la estructura de la página, pero ninguna funcionalidad está dispuesta todavía.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Crear un vínculo a la página de administración

En el Explorador de soluciones, expanda la carpeta Views y, a continuación, expanda la carpeta Shared. Abra el archivo denominado \_Layout.cshtml. Busque la **ul** elemento con Id. = "menú" y un vínculo de acción para la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> En el proyecto de ejemplo, hice algunos otros cambios cosméticos, como reemplazando la cadena "Su logotipo aquí". Esta no afecta a la funcionalidad de la aplicación. Puede descargar el proyecto y comparar los archivos.


Ejecute la aplicación y haga clic en el vínculo "Admin" que aparece en la parte superior de la página principal. La página de administración debe ser similar al siguiente:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Derecha ahora, la página no hace nada. En la siguiente sección, vamos a usar Knockout.js para crear una IU dinámica.

## <a name="add-authorization"></a>Agregar autorización

La página de administración está actualmente accesible para cualquiera que visite el sitio. Vamos a cambiar esta opción para restringir los permisos a los administradores.

Empiece por agregar una función de "Administrador" y un usuario administrador. En el Explorador de soluciones, expanda la carpeta filtros y abra el archivo denominado InitializeSimpleMembershipAttribute.cs. Busque la `SimpleMembershipInitializer` constructor. Después de llamar a **WebSecurity.InitializeDatabaseConnection**, agregue el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Se trata de una forma rápida y compleja para agregar la función "Administrador" y crear un usuario para el rol.

En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo HomeController.cs. Agregar el **Authorize** atribuir a la `Admin` método.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra el archivo AdminController.cs y agregue el **Authorize** atributo a toda la `AdminController` clase.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC y API Web definen **Authorize** atributos en diferentes espacios de nombres. MVC usa **System.Web.Mvc.AuthorizeAttribute**, mientras que utiliza la API Web **System.Web.Http.AuthorizeAttribute**.


Ahora sólo los administradores pueden ver la página de administración. Además, si se envía una solicitud HTTP al controlador de administración, la solicitud debe contener una cookie de autenticación. De lo contrario, el servidor envía una respuesta HTTP 401 (no autorizado). Puede ver esto en Fiddler enviando una solicitud GET a `http://localhost:*port*/api/admin`.

>[!div class="step-by-step"]
[Anterior](using-web-api-with-entity-framework-part-3.md)
[Siguiente](using-web-api-with-entity-framework-part-5.md)
