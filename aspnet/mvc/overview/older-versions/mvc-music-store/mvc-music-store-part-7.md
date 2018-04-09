---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Pertenencia y la autorización | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 7 cubre la pertenencia y la autorización.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: a0f599da4691c5bb7c8e6f01625fc0e94ce0eac8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-7-membership-and-authorization"></a>Parte 7: Pertenencia y la autorización
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 7 cubre la pertenencia y la autorización.


Nuestro controlador Store Manager está actualmente accesible para cualquiera que visite nuestro sitio. Vamos a cambiar esta opción para restringir los permisos a los administradores del sitio.

## <a name="adding-the-accountcontroller-and-views"></a>Agregar el AccountController y vistas

Una diferencia entre la plantilla de aplicación Web de ASP.NET MVC 3 completa y la plantilla de aplicación de Web vacía de ASP.NET MVC 3 es que la plantilla vacía no incluye un controlador de la cuenta. Vamos a agregar un controlador de la cuenta mediante la copia de unos pocos archivos desde una aplicación de MVC de ASP.NET nuevo creada a partir de la plantilla de aplicación Web de ASP.NET MVC 3 completa.

Crear una nueva aplicación de ASP.NET MVC mediante la plantilla de aplicación Web de ASP.NET MVC 3 completa y copie los archivos siguientes en los mismos directorios en el proyecto:

1. Copie AccountController.cs en el directorio de controladores
2. Copie AccountModels en el directorio de modelos
3. Cree un directorio de cuenta en el directorio de vistas y copie todas las cuatro vistas de

Cambiar el espacio de nombres para las clases de controlador y el modelo de modo que comienzan por MvcMusicStore. La clase AccountController debería utilizar el espacio de nombres MvcMusicStore.Controllers y la clase AccountModels debe utilizar el espacio de nombres MvcMusicStore.Models.

*Nota: Estos archivos también están disponibles en la descarga de MvcMusicStore Assets.zip desde el que se copian los archivos de diseño de sitio al principio del tutorial. Los archivos de suscripción se encuentran en el directorio de código.*

La solución actualizada debe ser similar al siguiente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Agregar un usuario administrativo con el sitio de configuración de ASP.NET

Antes de que se necesita autorización en nuestro sitio Web, necesitamos crear un usuario con acceso. La manera más fácil de crear un usuario es usar el sitio Web de configuración de ASP.NET integrado.

Inicie el sitio Web de configuración de ASP.NET, haga clic en siguiente el icono en el Explorador de soluciones.

![](mvc-music-store-part-7/_static/image2.png)

Se abre un sitio Web de configuración. Haga clic en la ficha seguridad en la pantalla principal, haga clic en el vínculo "Habilitar los roles" en el centro de la pantalla.

![](mvc-music-store-part-7/_static/image3.png)

Haga clic en el vínculo "crear o administrar funciones".

![](mvc-music-store-part-7/_static/image4.png)

Escriba "Administrador" como el nombre de rol y presione el botón Agregar rol.

![](mvc-music-store-part-7/_static/image5.png)

Haga clic en el botón Atrás y, a continuación, haga clic en el vínculo de usuario de crear en el lado izquierdo.

![](mvc-music-store-part-7/_static/image6.png)

Rellene los campos de información de usuario de la izquierda con la siguiente información:

| **Campo** | **Valor** |
| --- | --- |
| **Nombre de usuario** | Administrador |
| **Contraseña** | ¡password123! |
| **Confirmar contraseña** | ¡password123! |
| **Correo electrónico** | (funcionará cualquier dirección de correo electrónico) |
| **Pregunta de seguridad** | (nombre que quiera) |
| **Respuesta de seguridad** | (nombre que quiera) |

*Nota: Por supuesto puede utilizar cualquier contraseña que desea. La configuración de seguridad de la contraseña de forma predeterminada, requiere una contraseña que tiene 7 caracteres y contiene un carácter no alfanumérico.*

Seleccione el rol de administrador para este usuario y haga clic en el botón Crear usuario.

![](mvc-music-store-part-7/_static/image7.png)

En este punto, debería ver un mensaje que indica que el usuario se creó correctamente.

![](mvc-music-store-part-7/_static/image8.png)

Ahora puede cerrar la ventana del explorador.

## <a name="role-based-authorization"></a>Autorización basada en roles

Ahora se puede restringir el acceso a la StoreManagerController con el atributo [Authorize], especifica que el usuario debe ser en el rol de administrador para tener acceso a cualquier acción de controlador de la clase.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: Se puede colocar el atributo [Authorize] en los métodos de acción específica, así como en el nivel de clase de controlador.*

Vaya ahora a /StoreManager aparecerá un cuadro de diálogo de inicio de sesión:

![](mvc-music-store-part-7/_static/image9.png)

Después de iniciar sesión con la nueva cuenta de administrador, podemos acceder a la pantalla Editar álbum como antes.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-6.md)
> [Siguiente](mvc-music-store-part-8.md)
