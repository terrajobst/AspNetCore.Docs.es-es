---
uid: web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
title: "Parte 1: Información general y crear el proyecto | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/03/2012
ms.topic: article
ms.assetid: 94421d86-68c4-4471-bf5f-82d654a17252
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/using-web-api-1-with-entity-framework-5/using-web-api-with-entity-framework-part-1
msc.type: authoredcontent
ms.openlocfilehash: 47af34c72f1959756f5d68e0e80052e700c7b19c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="part-1-overview-and-creating-the-project"></a>Parte 1: Información general y crear el proyecto
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](http://code.msdn.microsoft.com/ASP-NET-Web-API-with-afa30545)

Entity Framework es un marco de trabajo de asignación relacional de objetos. Los objetos de dominio en el código se asigna a las entidades de una base de datos relacional. En general, no tiene que preocuparse de la capa de base de datos, dado que Entity Framework se encarga de ello automáticamente. El código manipula los objetos y cambios se mantienen en una base de datos.

## <a name="about-the-tutorial"></a>Acerca del Tutorial

En este tutorial, creará una aplicación de almacenamiento simple. Hay dos partes principales para la aplicación. Los usuarios normales pueden ver productos y crear pedidos:

![](using-web-api-with-entity-framework-part-1/_static/image1.png)

Los administradores pueden crear, eliminar o editar productos:

![](using-web-api-with-entity-framework-part-1/_static/image2.png)

## <a name="skills-youll-learn"></a>Obtendrá información de conocimientos

Esto es lo aprenderá:

- Cómo usar Entity Framework con ASP.NET Web API.
- Describe cómo usar knockout.js para crear una interfaz de usuario de cliente dinámico.
- Cómo usar la autenticación de formularios con API Web para autenticar a los usuarios.

Aunque este tutorial es independiente entre sí, debe leer primero los siguientes tutoriales:

- [Su primer ASP.NET Web API](../../getting-started-with-aspnet-web-api/tutorial-your-first-web-api.md)
- [Crear una API Web que admita operaciones CRUD](../creating-a-web-api-that-supports-crud-operations.md)

Algunos conocimientos de [ASP.NET MVC](../../../../mvc/index.md) también es útil.

## <a name="overview"></a>Información general

En un nivel alto, ésta es la arquitectura de la aplicación:

- ASP.NET MVC genera las páginas HTML para el cliente.
- ASP.NET Web API expone operaciones CRUD en los datos (productos y pedidos).
- Entity Framework traduce los modelos de C# usados por la API Web en entidades de base de datos.

![](using-web-api-with-entity-framework-part-1/_static/image3.png)

El diagrama siguiente muestra cómo se representan los objetos de dominio en varias capas de la aplicación: la capa de base de datos, el modelo de objetos y, por último, el formato, que se utiliza para transmitir datos al cliente a través de HTTP.

![](using-web-api-with-entity-framework-part-1/_static/image4.png)

## <a name="create-the-visual-studio-project"></a>Crear el proyecto de Visual Studio

Puede crear el proyecto de tutorial con Visual Web Developer Express o la versión completa de Visual Studio.

Desde el **iniciar** página, haga clic en **nuevo proyecto**.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto "ProductStore" y haga clic en **Aceptar**.

![](using-web-api-with-entity-framework-part-1/_static/image5.png)

En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione **aplicación de Internet** y haga clic en **Aceptar**.

![](using-web-api-with-entity-framework-part-1/_static/image6.png)

La plantilla de "Aplicación de Internet" crea una aplicación de ASP.NET MVC que admita la autenticación de formularios. Si se ejecuta la aplicación ahora, ya tiene algunas características:

- Pueden registrar los nuevos usuarios, haga clic en el vínculo "Register" en la esquina superior derecha.
- Los usuarios registrados pueden iniciar sesión, haga clic en el vínculo "Iniciar sesión".

Información de pertenencia se conserva en una base de datos que se crea automáticamente. Para obtener más información acerca de la autenticación de formularios en ASP.NET MVC, consulte [Tutorial: mediante la autenticación de formularios en ASP.NET MVC](https://msdn.microsoft.com/library/ff398049(VS.98).aspx).

## <a name="update-the-css-file"></a>Actualizar el archivo CSS

Este paso es cosmético, pero hará que las páginas de representar como las capturas de pantalla anterior.

En el Explorador de soluciones, expanda la carpeta de contenido y abra el archivo denominado Site.css. Agregue los siguientes estilos CSS:

[!code-css[Main](using-web-api-with-entity-framework-part-1/samples/sample1.css)]

>[!div class="step-by-step"]
[Siguiente](using-web-api-with-entity-framework-part-2.md)
