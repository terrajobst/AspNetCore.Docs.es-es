---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
title: 'Parte 7: Pertenencia y autorización | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 7 cubre la pertenencia y autorización.
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/13/2010
ms.topic: article
ms.assetid: c8511ebe-68bc-4240-87c3-d5ced84a3f37
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-7
msc.type: authoredcontent
ms.openlocfilehash: 41b17315cbe1f6d93001a736bc24bf003df24061
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393175"
---
<a name="part-7-membership-and-authorization"></a>Parte 7: Pertenencia y autorización
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 7 cubre la pertenencia y autorización.


Nuestro controlador Store Manager está actualmente accesible para cualquier usuario visitar nuestro sitio. Vamos a cambiar esta opción para restringir los permisos a los administradores del sitio.

## <a name="adding-the-accountcontroller-and-views"></a>Adición de las vistas y AccountController

Una diferencia entre la plantilla completa de la aplicación Web de ASP.NET MVC 3 y la plantilla de aplicación de Web vacía de ASP.NET MVC 3 es que la plantilla vacía no incluye un controlador de cuentas. Vamos a agregar un controlador de cuentas copiando unos cuantos archivos desde una aplicación ASP.NET MVC nuevo creada a partir de la plantilla completa de la aplicación Web de ASP.NET MVC 3.

Cree una nueva aplicación de ASP.NET MVC mediante la plantilla completa de la aplicación Web de ASP.NET MVC 3 y copie los archivos siguientes en los mismos directorios en nuestro proyecto:

1. Copie AccountController.cs en el directorio de controladores
2. Copie AccountModels en el directorio de modelos
3. Cree un directorio de la cuenta en el directorio vistas y copie todas las cuatro vistas de

Cambie el espacio de nombres para las clases de controlador y el modelo para que comienzan por MvcMusicStore. La clase AccountController debe usar el espacio de nombres MvcMusicStore.Controllers, y la clase AccountModels debe utilizar el espacio de nombres MvcMusicStore.Models.

*Nota: Estos archivos también están disponibles en la descarga de MvcMusicStore Assets.zip desde la que se copian los archivos de diseño de sitio al principio del tutorial. Los archivos de suscripción se encuentran en el directorio de código.*

La solución actualizada debe ser similar al siguiente:

![](mvc-music-store-part-7/_static/image1.png)

## <a name="adding-an-administrative-user-with-the-aspnet-configuration-site"></a>Agregar un usuario administrativo con el sitio de configuración de ASP.NET

Antes de que se requiere autorización en nuestro sitio Web, necesitamos crear un usuario con acceso. La manera más fácil de crear un usuario es usar el sitio Web de configuración de ASP.NET integrado.

Inicie el sitio Web de configuración de ASP.NET, haga clic en siguiente el icono en el Explorador de soluciones.

![](mvc-music-store-part-7/_static/image2.png)

Esto inicia un sitio Web de configuración. Haga clic en la ficha seguridad en la pantalla principal y, a continuación, haga clic en el vínculo "Habilitar roles" en el centro de la pantalla.

![](mvc-music-store-part-7/_static/image3.png)

Haga clic en el vínculo "crear o administrar roles".

![](mvc-music-store-part-7/_static/image4.png)

Escriba el nombre de rol "Administrador" y presione el botón Agregar rol.

![](mvc-music-store-part-7/_static/image5.png)

Haga clic en el botón Atrás, a continuación, haga clic en el vínculo de usuario de crear en el lado izquierdo.

![](mvc-music-store-part-7/_static/image6.png)

Rellene los campos de información de usuario de la izquierda con la información siguiente:

| **Campo** | **Valor** |
| --- | --- |
| **Nombre de usuario** | Administrador |
| **Contraseña** | ¡password123! |
| **Confirmar contraseña** | ¡password123! |
| **Correo electrónico** | (funcionará cualquier dirección de correo electrónico) |
| **Pregunta de seguridad** | (el que desee) |
| **Respuesta de seguridad** | (el que desee) |

*Nota: Por supuesto puede utilizar cualquier contraseña que desea. La configuración de seguridad de la contraseña de forma predeterminada, requiere una contraseña que es de 7 caracteres de largo y contiene un carácter no alfanumérico.*

Seleccione el rol de administrador para este usuario y haga clic en el botón Create User.

![](mvc-music-store-part-7/_static/image7.png)

En este punto, debería ver un mensaje que indica que el usuario se creó correctamente.

![](mvc-music-store-part-7/_static/image8.png)

Ahora puede cerrar la ventana del explorador.

## <a name="role-based-authorization"></a>Autorización basada en roles

Ahora nos podemos restringir el acceso a la StoreManagerController mediante el atributo [Authorize], especificar que el usuario debe estar en el rol de administrador para tener acceso a cualquier acción de controlador en la clase.

[!code-csharp[Main](mvc-music-store-part-7/samples/sample1.cs)]

*Nota: El atributo [Authorize] puede colocarse en los métodos de acción específica, así como en el nivel de clase de controlador.*

Vaya ahora a /StoreManager abre un cuadro de diálogo Iniciar sesión:

![](mvc-music-store-part-7/_static/image9.png)

Después de iniciar sesión con la nueva cuenta de administrador, podemos ir a la pantalla Editar álbum como antes.

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-6.md)
> [Siguiente](mvc-music-store-part-8.md)
