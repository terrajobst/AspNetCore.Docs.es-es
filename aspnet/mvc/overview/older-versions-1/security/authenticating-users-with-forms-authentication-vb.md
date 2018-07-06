---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Autenticar a los usuarios con la autenticación (VB) de formularios | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo usar el atributo [Authorize] a contraseña proteger determinadas páginas en la aplicación MVC. Aprenda a usar el sitio Web de administración demasiado...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: a9d55df3132a5b5ceeb49ed6d0b83b847f2f1e3b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828859"
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Autenticar a los usuarios con autenticación de formularios (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo usar el atributo [Authorize] a contraseña proteger determinadas páginas en la aplicación MVC. Aprenda a usar la herramienta de administración de sitios Web para crear y administrar usuarios y roles. También aprenderá a configurar dónde se almacena la información de cuenta y el rol de usuario.


El objetivo de este tutorial es explicar cómo puede usar formularios de autenticación de contraseña de proteger las vistas en las aplicaciones de ASP.NET MVC. Aprenda a usar la herramienta de administración de sitios Web para crear usuarios y roles. También aprenderá cómo impedir que los usuarios no autorizados de invocar acciones de controlador. Por último, aprenderá a configurar dónde se almacenan los nombres de usuario y contraseñas.

#### <a name="using-the-web-site-administration-tool"></a>Con la herramienta de administración del sitio Web

Antes de hacer nada más, debemos empezar vamos a crear algunos usuarios y roles. La manera más fácil de crear nuevos usuarios y roles es aprovechar las ventajas de la herramienta de administración de sitios Web de Visual Studio 2008. Puede iniciar esta herramienta seleccionando la opción de menú **proyecto, la configuración de ASP.NET**. Como alternativa, puede iniciar la herramienta de administración de sitios Web, haga clic en el icono del martillo alcanzando el mundo que aparece en la parte superior de la ventana del explorador de soluciones (un poco de miedo) (consulte la figura 1).

**Ilustración 1: iniciar la herramienta de administración de sitios Web**

![clip_image002 [4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Dentro de la herramienta de administración de sitios Web, crear nuevos usuarios y roles, seleccione la pestaña seguridad. Haga clic en el **crear usuario** vínculo para crear un nuevo usuario denominado Stephen (consulte la figura 2). Proporcione al usuario Stephen cualquier contraseña que desee (por ejemplo, *secreto*).

**Ilustración 2: crear un nuevo usuario**

![clip_image004 [4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Crear nuevos roles de habilitación de funciones y definir uno o varios roles. Habilitar los roles haciendo clic en el **habilitar roles** vínculo. A continuación, cree un rol denominado *administradores* haciendo clic en el **crear o administrar roles** vincular (consulte la figura 3).

**Ilustración 3: crear un nuevo rol**

![clip_image006 [4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Por último, cree un nuevo usuario denominado Sally y asociar Sally con la función administradores haciendo clic en el vínculo Create User y seleccionar los administradores al crear Sally (consulte la figura 4).

**Figura 4: agregar un usuario a un rol**

![clip_image008 [4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Cuando todo está listo, debe tener dos nuevos usuarios denominados Stephen y Sally. También debe tener un nuevo rol denominado Administradores. Sally es un miembro del rol de administradores y Stephen no es.

#### <a name="requiring-authorization"></a>Requerir autorización

Puede requerir un usuario se autentique antes de que el usuario invoque una acción de controlador agregando el atributo [Authorize] a la acción. Puede aplicar el atributo [Authorize] a una acción de controlador individuales o puede aplicar este atributo a una clase de controlador completo.

Por ejemplo, el controlador en el listado 1 expone una acción denominada CompanySecrets(). Dado que esta acción se decora con el atributo [Authorize], esta acción no se puede invocar a menos que un usuario está autenticado.

**Listado 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Si invocar la acción CompanySecrets() escribiendo el CompanySecrets / / Home de dirección URL en la barra de direcciones del explorador y no es un usuario autenticado, entonces se le redirigirá a la vista de inicio de sesión automáticamente (consulte la figura 5).

**Figura 5: la vista de inicio de sesión**

![clip_image010 [4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Puede usar la vista de inicio de sesión que escriba su nombre de usuario y contraseña. Si no es un usuario registrado, puede hacer clic en el **registrar** vínculo para navegar al registro ver (consulte la figura 6). Puede usar la vista Register para crear una nueva cuenta de usuario.

**Figura 6: la vista Register**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Una vez que inicie sesión correctamente, puede ver el CompanySecrets ver (consulte la figura 7). De forma predeterminada, aún iniciar sesión hasta que cierre la ventana del explorador.

**Figura 7: la vista CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizar por nombre de usuario o rol de usuario

Puede usar el atributo [Authorize] a restringir el acceso a una acción de controlador a un conjunto determinado de usuarios o un conjunto determinado de roles de usuario. Por ejemplo, el controlador Home modificado en el listado 2 contiene dos nuevas acciones denominadas StephenSecrets() y AdministratorSecrets().

**Listado 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Solo un usuario con el nombre de usuario Stephen puede invocar la acción StephenSecrets(). Todos los demás usuarios accedas a la vista de inicio de sesión. La propiedad de los usuarios acepta una lista separada por comas de nombres de cuenta de usuario.

Solo los usuarios del rol administradores pueden invocar la acción AdministratorSecrets(). Por ejemplo, dado que Sally es un miembro del grupo Administradores, puede invocar la acción de AdministratorSecrets(). Todos los demás usuarios accedas a la vista de inicio de sesión. La propiedad Roles acepta una lista separada por comas de nombres de función.

#### <a name="configuring-authentication"></a>Configurar la autenticación

En este momento, tal vez se pregunte dónde se almacenan la información de cuenta y el rol de usuario. De forma predeterminada, la información se almacena en una base de datos (RANU) de SQL Express ASPNETDB.mdf con nombre se encuentra en la aplicación de la aplicación MVC\_carpeta de datos. Esta base de datos se genera automáticamente el marco de ASP.NET cuando se inicia mediante la suscripción.

Para ver la base de datos ASPNETDB.mdf en la ventana Explorador de soluciones, primero deberá seleccionar la opción de menú proyecto, mostrar todos los archivos.

Uso de la base de datos de SQL Express de forma predeterminada está bien, al desarrollar una aplicación. Probablemente, sin embargo, no desea utilizar la base de datos ASPNETDB.mdf para una aplicación de producción. En ese caso, puede cambiar de donde se almacena la información de la cuenta de usuario siguiendo los dos pasos siguientes:

1. Agregar los objetos de base de datos de servicios de aplicación a la base de datos de producción: cambiar la cadena de conexión de la aplicación para que apunte a la base de datos de producción

El primer paso es agregar todos los objetos de base de datos necesarios (tablas y procedimientos almacenados) a la base de datos de producción. Es la manera más fácil de agregar estos objetos a una base de datos aprovechar las ventajas del Asistente para la instalación de ASP.NET SQL Server (consulte la figura 8). Puede iniciar esta herramienta, abra el símbolo del sistema de Visual Studio 2008 desde el grupo de programas de Microsoft Visual Studio 2008 y ejecutando el siguiente comando desde el símbolo del sistema:

ASPNET\_regsql

**Figura 8: el Asistente para la instalación del servidor de SQL de ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

El Asistente para instalación de servidor SQL de ASP.NET le permite seleccionar una base de datos de SQL Server en la red e instalar todos los objetos de base de datos necesarios para los servicios de aplicación de ASP.NET. El servidor de base de datos no es necesario estar ubicado en el equipo local.

> [!NOTE]
> Si no desea usar al Asistente para la instalación de ASP.NET SQL Server, puede encontrar scripts SQL para agregar los objetos de base de datos de servicios de aplicaciones en la siguiente carpeta:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Después de crear los objetos de base de datos necesarios, deberá modificar la conexión de base de datos utilizada por la aplicación de MVC. Modifique la cadena de conexión de ApplicationServices en el archivo de configuración (web.config) web para que apunte a la base de datos de producción. Por ejemplo, la conexión modificada en el listado 3 apunta a una base de datos denominada MyProductionDB (la cadena de conexión de ApplicationServices original se ha comentado con).

**Listado 3 – Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configuración de permisos de base de datos

Si usa seguridad integrada para conectarse a la base de datos, a continuación, deberá agregar la cuenta de usuario de Windows correcta como un inicio de sesión a la base de datos. La cuenta correcta depende de si usas el servidor de desarrollo de ASP.NET o Internet Information Services como servidor web. También depende de la cuenta de usuario correctos en el sistema operativo.

Si usa el servidor de desarrollo de ASP.NET (el servidor de web predeterminado utilizado por Visual Studio), a continuación, la aplicación se ejecuta dentro del contexto de su cuenta de usuario de Windows. En ese caso, deberá agregar su cuenta de usuario de Windows como un inicio de sesión del servidor de base de datos.

Como alternativa, si está utilizando Internet Information Services, a continuación, deberá agregar la cuenta ASPNET o la cuenta NT AUTHORITY o NETWORK SERVICE como un inicio de sesión del servidor de base de datos. Si está utilizando Windows XP, a continuación, agregue la cuenta ASPNET como inicio de sesión a la base de datos. Si usa un sistema operativo más reciente, como Windows Vista o Windows Server 2008, a continuación, agregue la cuenta NT AUTHORITY o NETWORK SERVICE como el inicio de sesión de base de datos.

Puede agregar una nueva cuenta de usuario a la base de datos mediante el uso de Microsoft SQL Server Management Studio (consulte la figura 9).

**Figura 9: crear un nuevo inicio de sesión de Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Después de crear el inicio de sesión necesario, deberá asignar el inicio de sesión a un usuario de base de datos con los roles de base de datos correcta. Haga doble clic en el inicio de sesión y seleccione la pestaña asignación de usuario. Seleccione uno o varios roles de base de datos de servicios de aplicación. Por ejemplo, con el fin de autenticar a los usuarios, deberá habilitar aspnet\_pertenencia\_BasicAccess rol de base de datos. Para crear nuevos usuarios, deberá habilitar aspnet\_pertenencia\_FullAccess rol de base de datos (consulte la figura 10).

**Ilustración 10: agregar roles de base de datos de servicios de aplicación**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Resumen

En este tutorial, aprendió a usar autenticación de formularios al compilar una aplicación ASP.NET MVC. En primer lugar, ha aprendido a crear nuevos usuarios y roles aprovechando las ventajas de la herramienta de administración de sitios Web. A continuación, ha aprendido a usar el atributo [Authorize] para impedir que los usuarios no autorizados invocar acciones de controlador. Por último, ha aprendido a configurar la aplicación de MVC para almacenar información de usuario y rol en una base de datos de producción.

> [!div class="step-by-step"]
> [Anterior](preventing-javascript-injection-attacks-cs.md)
> [Siguiente](authenticating-users-with-windows-authentication-vb.md)
