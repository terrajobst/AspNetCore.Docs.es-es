---
title: Autenticación con Facebook, Google y proveedores externos en ASP.NET Core
author: rick-anderson
description: En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x mediante OAuth 2.0 con proveedores de autenticación externos.
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2018
uid: security/authentication/social/index
ms.openlocfilehash: 063d452fb6ab91b712ade7f7b7ed99823dbdc657
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098823"
---
# <a name="facebook-google-and-external-provider-authentication-in-aspnet-core"></a>Autenticación con Facebook, Google y proveedores externos en ASP.NET Core

Por [Valeriy Novytskyy](https://github.com/01binary) y [Rick Anderson](https://twitter.com/RickAndMSFT)

En este tutorial se muestra cómo crear una aplicación de ASP.NET Core 2.x que permita a los usuarios iniciar sesión mediante OAuth 2.0 con credenciales de proveedores de autenticación externos.

En las siguientes secciones se tratan los proveedores [Facebook](xref:security/authentication/facebook-logins), [Twitter](xref:security/authentication/twitter-logins), [Google](xref:security/authentication/google-logins) y [Microsoft](xref:security/authentication/microsoft-logins). Hay disponibles otros proveedores en paquetes de terceros como [AspNet.Security.OAuth.Providers](https://github.com/aspnet-contrib/AspNet.Security.OAuth.Providers) y [AspNet.Security.OpenId.Providers](https://github.com/aspnet-contrib/AspNet.Security.OpenId.Providers).

![Iconos de redes sociales para Facebook, Twitter, Google Plus y Windows](index/_static/social.png)

Permitir que los usuarios inicien sesión con sus credenciales es práctico para los usuarios y transfiere muchas de las complejidades existentes en la administración del proceso de inicio de sesión a un tercero. Para ver ejemplos de cómo los inicios de sesión de las redes sociales pueden controlar las conversiones del tráfico y de clientes, vea los casos prácticos de [Facebook](https://www.facebook.com/unsupportedbrowser) y [Twitter](https://dev.twitter.com/resources/case-studies).

## <a name="create-a-new-aspnet-core-project"></a>Crear un proyecto de ASP.NET Core

* En Visual Studio 2017, cree un proyecto en la página de inicio o a través de **Archivo** > **Nuevo** > **Proyecto**.

* Seleccione la plantilla **Aplicación web de ASP.NET Core** disponible en la categoría **Visual C#** > **.NET Core**:

![Cuadro de diálogo Nuevo proyecto](index/_static/new-project.png)

* Pulse **Aplicación web** y compruebe que la opción **Autenticación** está establecida en **Cuentas de usuario individuales**:

![Cuadro de diálogo Nueva aplicación web](index/_static/select-project.png)

Nota: Este tutorial se aplica a la versión 2.0 del SDK de ASP.NET Core, que se puede seleccionar en la parte superior del asistente.

## <a name="apply-migrations"></a>Aplicación de migraciones

* Ejecute la aplicación y seleccione el vínculo **Iniciar sesión**.
* Seleccione el vínculo **Register as a new user** Registrarse como usuario nuevo).
* Escriba el correo electrónico y la contraseña de la cuenta nueva y, luego, seleccione **Registrarse**.
* Siga estas instrucciones para aplicar las migraciones.

## <a name="require-https"></a>Requisito de HTTPS

OAuth 2.0 requiere el uso de SSL o TLS para la autenticación mediante el protocolo HTTPS.

Los proyectos creados con las plantillas de proyecto **Aplicación web** o **API web** con ASP.NET Core 2.1 o versiones posteriores se configuran automáticamente para habilitar HTTPS. La aplicación se inicia con un punto de conexión predeterminado seguro si la opción **Cuentas de usuario individuales** está seleccionada en el **cuadro de diálogo Cambiar autenticación** del asistente para el proyecto.

Para obtener más información, vea <xref:security/enforcing-ssl>.

[!INCLUDE[Forward request information when behind a proxy or load balancer section](includes/forwarded-headers-middleware.md)]

## <a name="use-secretmanager-to-store-tokens-assigned-by-login-providers"></a>Uso de SecretManager para almacenar los tokens asignados por los proveedores de inicio de sesión

Los proveedores de inicio de sesión de las redes sociales asignan los tokens **Id. de aplicación** y **Secreto de la aplicación** durante el proceso de registro. La nomenclatura puede variar en función del proveedor. Estos tokens representan las credenciales que usa la aplicación para acceder a su API. Los tokens constituyen los "secretos" que se pueden vincular a la configuración de la aplicación con la ayuda de [Secret Manager](xref:security/app-secrets#secret-manager). Secret Manager es una alternativa más segura al almacenamiento de los tokens en un archivo de configuración, como, por ejemplo, *appsettings.json*.

> [!IMPORTANT]
> Secret Manager solo está pensado para fines de desarrollo. Puede almacenar y proteger sus secretos de producción y pruebas de Azure con el [proveedor de configuración de Azure Key Vault](xref:security/key-vault-configuration).

Siga los pasos descritos en el tema [Ubicación de almacenamiento segura de secretos de la aplicación en el desarrollo de ASP.NET Core](xref:security/app-secrets) para almacenar los tokens asignados por cada uno de los siguientes proveedores de inicio de sesión.

## <a name="setup-login-providers-required-by-your-application"></a>Configuración de los proveedores de inicio de sesión requeridos por la aplicación

En los temas siguientes encontrará información para configurar la aplicación a fin de usar los proveedores correspondientes:

* Instrucciones para [Facebook](xref:security/authentication/facebook-logins)
* Instrucciones para [Twitter](xref:security/authentication/twitter-logins)
* Instrucciones para [Google](xref:security/authentication/google-logins)
* Instrucciones para [Microsoft](xref:security/authentication/microsoft-logins)
* Instrucciones para [otros proveedores](xref:security/authentication/otherlogins)

[!INCLUDE[](includes/chain-auth-providers.md)]

## <a name="optionally-set-password"></a>Establecimiento opcional de contraseña

Si el registro se realiza mediante un proveedor de inicio de sesión externo, no se tiene ninguna contraseña en la aplicación. De esta forma no hace falta crear y recordar una contraseña para el sitio, aunque le hace depender del proveedor de inicio de sesión externo. Si el proveedor de inicio de sesión externo no está disponible, no podrá iniciar sesión en el sitio web.

Para crear una contraseña e iniciar sesión con el correo electrónico establecido durante el proceso de inicio de sesión con proveedores externos:

* Pulse el vínculo **Hola, &lt;alias de correo electrónico&gt;** situado en la esquina superior derecha para ir a la vista **Administración**.

![Vista Administración de la aplicación web](index/_static/pass1a.png)

* Pulse **Crear**.

![Página para establecer la contraseña](index/_static/pass2a.png)

* Establezca una contraseña válida. Podrá usarla para iniciar sesión con su correo electrónico.

## <a name="next-steps"></a>Pasos siguientes

* En este artículo se introdujo la autenticación externa y se explicaron los requisitos previos necesarios para agregar inicios de sesión externos a la aplicación de ASP.NET Core.

* Páginas de referencia específicas del proveedor para configurar los inicios de sesión para los proveedores requeridos por la aplicación.

* Le recomendamos que conserve los datos adicionales sobre el usuario y sus tokens de acceso y actualización. Para obtener más información, vea <xref:security/authentication/social/additional-claims>.
