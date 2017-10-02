---
title: "Habilitación de la autenticación con Facebook, Google y otros proveedores externos"
author: rick-anderson
description: 
keywords: "ASP.NET Core, autenticación, social, proveedores de autenticación, google, facebook, twitter, cuenta de microsoft"
ms.author: riande
manager: wpickett
ms.date: 11/01/2016
ms.topic: article
ms.assetid: eda7ee17-f38c-462e-8d1d-63f459901cf3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/social/index
ms.openlocfilehash: 56036000535156b4b5814dde2a0145dcdfff28c3
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 09/28/2017
---
# <a name="enabling-authentication-using-facebook-google-and-other-external-providers"></a>Habilitación de la autenticación con Facebook, Google y otros proveedores externos

<a name=security-authentication-social-logins></a>

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.

En las siguientes secciones se tratan los proveedores [Facebook](facebook-logins.md), [Twitter](twitter-logins.md), [Google](google-logins.md) y [Microsoft](microsoft-logins.md). Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

Permitir que los usuarios inicien sesión con sus credenciales es práctico para los usuarios y transfiere muchas de las complejidades existentes en la administración del proceso de inicio de sesión a un tercero. Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).

Nota: Los paquetes que se presentan aquí sintetizan en gran parte la complejidad del flujo de autenticación de OAuth, pero conocer los detalles puede ser necesario a la hora de solucionar problemas. Hay disponibles numerosos recursos; por ejemplo, vea [An Introduction to OAuth 2](https://www.digitalocean.com/community/tutorials/an-introduction-to-oauth-2) (Introducción a OAuth 2) o [Understanding OAuth2](http://www.bubblecode.net/2016/01/22/understanding-oauth2/) (Descripción de OAuth2). Algunos problemas se pueden resolver examinando el [código fuente de ASP.NET Core de los paquetes de los proveedores](https://github.com/aspnet/Security/tree/dev/src).

## <a name="create-a-new-aspnet-core-project"></a>Crear un proyecto de ASP.NET Core

* En Visual Studio 2017, cree un proyecto en la página de inicio o a través de **Archivo > Nuevo > Proyecto**.

* Seleccione la plantilla **Aplicación web de ASP.NET Core** disponible en la categoría **Visual C# > .NET Core**:

![Cuadro de diálogo Nuevo proyecto](index/_static/new-project.png)

* Pulse **Aplicación web** y compruebe que la opción **Autenticación** está establecida en **Cuentas de usuario individuales**:

![Cuadro de diálogo Nueva aplicación web](index/_static/select-project.png)

Nota: Este tutorial se aplica a la versión 2.0 del SDK de ASP.NET Core, que se puede seleccionar en la parte superior del asistente.

## <a name="require-ssl"></a>Requerir SSL

OAuth 2.0 requiere el uso de SSL para la autenticación mediante el protocolo HTTPS.

Nota: Los proyectos creados con plantillas de proyecto de **aplicación web** o **API Web** de ASP.NET Core 2.x se configuran automáticamente para habilitar SSL e iniciarse con una dirección URL https si la opción **Cuentas de usuario individuales** estaba seleccionada en el cuadro de diálogo **Cambiar autenticación** del asistente de proyectos, como se muestra arriba.

* Establezca que SSL sea obligatorio en el sitio siguiendo los pasos descritos en el tema [Exigir SSL en una aplicación ASP.NET básica](xref:security/enforcing-ssl).

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión

Los proveedores de inicio de sesión de las redes sociales asignan tokens de **Id. de aplicación** y de **secreto de aplicación** durante el proceso de registro (la nomenclatura exacta varía según el proveedor).

Estos valores son el *nombre de usuario* y la *contraseña* que usa la aplicación para obtener acceso a la API. Conforman los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda del **Administrador de secretos**, en vez de almacenarlos directamente en archivos de configuración o de codificarlos de forma rígida.

Siga los pasos descritos en el tema [Safe storage of app secrets during development in ASP.NET Core](xref:security/app-secrets) (Almacenamiento seguro de secretos de aplicación durante el desarrollo en ASP.NET Core) para poder almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.

## <a name="setup-login-providers-required-by-your-application"></a>Configuración de los proveedores de inicio de sesión requeridos por la aplicación

En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:

* Instrucciones para [Facebook](facebook-logins.md)
* Instrucciones para [Twitter](twitter-logins.md)
* Instrucciones para [Google](google-logins.md)
* Instrucciones para [Microsoft](microsoft-logins.md)
* Instrucciones para [otros proveedores](other-logins.md)

## <a name="optionally-set-password"></a>Establecimiento opcional de contraseña

Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación. De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo. Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.

Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:

* Pulse el vínculo **Hola <email alias>** situado en la esquina superior derecha para ir a la vista **Administración**.

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* Pulse **Crear**.

![Página para establecer la contraseña](index/_static/pass2a.png)

* Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.

* Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.
