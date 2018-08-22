---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
title: 'Parte 4: Agregar una vista de administración | Microsoft Docs'
author: MikeWasson
description: ''
ms.author: riande
ms.date: 07/04/2012
ms.assetid: 792f4513-a508-4d14-a0dd-1a2fe282c7bb
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-4
msc.type: authoredcontent
ms.openlocfilehash: ff46c7cbdf13a048e57ad88ab39077357e35c5b3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41839169"
---
<a name="part-4-adding-an-admin-view"></a>Parte 4: Agregar una vista de administración
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

## <a name="add-an-admin-view"></a>Agregar una vista de administración

Ahora se podrá activar en el lado del cliente y agregue una página que puede consumir datos desde el controlador de administración. La página permitirá a los usuarios crear, editar o eliminar los productos, mediante el envío de solicitudes AJAX al controlador.

En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo denominado HomeController.cs. Este archivo contiene un controlador MVC. Agregue un método denominado `Admin`:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample1.cs)]

El **HttpRouteUrl** método crea el identificador URI a la API web y la almacenamos esto en el contenedor de vista para su uso posterior.

A continuación, coloque el cursor de texto dentro de la `Admin` método de acción, a continuación, secundario y seleccione **agregar vista**. Esto le llevará a la **agregar vista** cuadro de diálogo.

![](using-web-api-with-entity-framework-part-4/_static/image1.png)

En el **agregar vista** cuadro de diálogo, nombre de la vista "Admin". Seleccione la casilla de verificación con la etiqueta **crear una vista fuertemente tipada**. En **clase modelo**, seleccione "Product (ProductStore.Models)". Deje el resto de opciones como sus valores predeterminados.

![](using-web-api-with-entity-framework-part-4/_static/image2.png)

Al hacer clic en **agregar** agrega un archivo denominado Admin.cshtml en vistas/inicio. Abra este archivo y agregue el siguiente código HTML. Este código HTML define la estructura de la página, pero ninguna funcionalidad está dispuesta todavía.

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample2.cshtml)]

## <a name="create-a-link-to-the-admin-page"></a>Crear un vínculo a la página de administración

En el Explorador de soluciones, expanda la carpeta Views y, a continuación, expanda la carpeta Shared. Abra el archivo denominado \_Layout.cshtml. Busque el **ul** elemento con id = "menu" y un vínculo de acción para la vista de administración:

[!code-cshtml[Main](using-web-api-with-entity-framework-part-4/samples/sample3.cshtml)]

> [!NOTE]
> En el proyecto de ejemplo, he realizado algunos otros cambios cosméticos, por ejemplo, reemplazando la cadena "Su logotipo aquí". Estos no afectan a la funcionalidad de la aplicación. Puede descargar el proyecto y compare los archivos.


Ejecute la aplicación y haga clic en el vínculo "Admin" que aparece en la parte superior de la página principal. La página de administración debe tener el siguiente aspecto:

![](using-web-api-with-entity-framework-part-4/_static/image3.png)

Derecha ahora, la página no hace nada. En la siguiente sección, vamos a usar Knockout.js para crear una interfaz de usuario dinámica.

## <a name="add-authorization"></a>Agregar autorización

La página de administración está actualmente accesible a cualquier persona que visite el sitio. Vamos a cambiar esta opción para restringir los permisos a los administradores.

Empiece agregando un rol de "Administrador" y un usuario administrador. En el Explorador de soluciones, expanda la carpeta filtros y abra el archivo denominado InitializeSimpleMembershipAttribute.cs. Busque el `SimpleMembershipInitializer` constructor. Después de llamar a **WebSecurity.InitializeDatabaseConnection**, agregue el código siguiente:

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample4.cs)]

Se trata de una manera de agregar el rol "Administrador" y cree un usuario para el rol rápidas en sucio.

En el Explorador de soluciones, expanda la carpeta Controllers y abra el archivo HomeController.cs. Agregar el **Authorize** atributo a la `Admin` método.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample5.cs)]

Abra el archivo AdminController.cs y agregue el **Authorize** atributo a toda la `AdminController` clase.

[!code-csharp[Main](using-web-api-with-entity-framework-part-4/samples/sample6.cs)]

> [!NOTE]
> MVC y Web API definen **Authorize** atributos, en diferentes espacios de nombres. MVC usa **System.Web.Mvc.AuthorizeAttribute**, mientras que usa la API Web **System.Web.Http.AuthorizeAttribute**.


Ahora solo los administradores pueden ver la página de administración. Además, si envía una solicitud HTTP al controlador de administración, la solicitud debe contener una cookie de autenticación. Si no es así, el servidor envía una respuesta HTTP 401 (no autorizado). Puede ver esto en Fiddler, envíe una solicitud GET a `http://localhost:*port*/api/admin`.

> [!div class="step-by-step"]
> [Anterior](using-web-api-with-entity-framework-part-3.md)
> [Siguiente](using-web-api-with-entity-framework-part-5.md)
