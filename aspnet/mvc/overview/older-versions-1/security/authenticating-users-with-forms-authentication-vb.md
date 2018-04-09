---
uid: mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
title: Autenticar a los usuarios con la autenticación (VB) de formularios | Documentos de Microsoft
author: microsoft
description: Obtenga información acerca de cómo utilizar el atributo [Authorize] para proteger por contraseña páginas específicas de la aplicación de MVC. Obtenga información acerca de cómo usar el sitio Web de administración demasiado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 4341f5b1-6fe5-44c5-8b8a-18fa84f80177
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/security/authenticating-users-with-forms-authentication-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ff425a4c9728de2eec3d0c94e76cb51a15de487
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="authenticating-users-with-forms-authentication-vb"></a>Autenticar a los usuarios con autenticación de formularios (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información acerca de cómo utilizar el atributo [Authorize] para proteger por contraseña páginas específicas de la aplicación de MVC. Obtenga información acerca de cómo usar la herramienta de administración de sitios Web para crear y administrar usuarios y roles. También aprenderá a configurar donde se almacena la información de cuenta y el rol de usuario.


El objetivo de este tutorial es explicar cómo puede utilizar formularios de autenticación de contraseña de proteger las vistas en las aplicaciones de ASP.NET MVC. Obtenga información acerca de cómo usar la herramienta de administración de sitios Web para crear usuarios y roles. También aprenderá cómo impedir que los usuarios no autorizados de invocar acciones del controlador. Por último, aprenderá a configurar dónde se almacenan los nombres de usuario y contraseñas.

#### <a name="using-the-web-site-administration-tool"></a>Con la herramienta de administración de sitios Web

Antes de hacer nada más, debemos empezamos creando algunos usuarios y roles. La manera más fácil de crear nuevos usuarios y roles es aprovechar las ventajas de la herramienta de administración de sitios Web de Visual Studio 2008. Puede iniciar esta herramienta seleccionando la opción de menú **proyecto, la configuración de ASP.NET**. Como alternativa, puede iniciar la herramienta de administración de sitios Web, haga clic en el icono del martillo alcanzando el mundo que aparece en la parte superior de la ventana Explorador de soluciones (un poco terror) (consulte la figura 1).

**Ilustración 1: iniciar la herramienta de administración de sitios Web**

![clip_image002[4]](authenticating-users-with-forms-authentication-vb/_static/image1.jpg)

Dentro de la herramienta de administración de sitios Web, cree nuevos usuarios y roles, seleccione la pestaña seguridad. Haga clic en el **crear usuario** vínculo para crear un nuevo usuario denominado Stephen (consulte la figura 2). Proporcionar al usuario Stephen con cualquier contraseña que desee (por ejemplo, *secreto*).

**Figura 2: crear un nuevo usuario**

![clip_image004[4]](authenticating-users-with-forms-authentication-vb/_static/image2.jpg)

Crear nuevos roles de habilitación de roles y definir uno o varios roles. Habilitar los roles, haga clic en el **habilitar funciones** vínculo. A continuación, cree un rol denominado *administradores* haciendo clic en el **crear o administrar roles** vincular (consulte la figura 3).

**Figura 3: crear un nuevo rol**

![clip_image006[4]](authenticating-users-with-forms-authentication-vb/_static/image3.jpg)

Por último, cree un nuevo usuario denominado Sally y asociar Sally con la función administradores haciendo clic en el vínculo de Create User y seleccionando los administradores al crear Sally (consulte la figura 4).

**Figura 4: agregar un usuario a un rol**

![clip_image008[4]](authenticating-users-with-forms-authentication-vb/_static/image4.jpg)

Cuando todo está listo, debe tener dos nuevos usuarios denominados Stephen y Sally. También debe tener una nueva función llamada Administrators. Juan es un miembro de la función Administradores y Stephen no lo es.

#### <a name="requiring-authorization"></a>Requerir autorización

Puede requerir un usuario se autentiquen antes de que el usuario invoca una acción de controlador agregando el atributo [Authorize] para la acción. Puede aplicar el atributo [Authorize] con una acción de controladores individuales o puede aplicar este atributo a una clase de controlador completo.

Por ejemplo, el controlador en el listado 1 expone una acción denominada CompanySecrets(). Dado que esta acción se decora con el atributo [Authorize], esta acción no se puede invocar a menos que un usuario se autentica.

**Lista 1 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample1.vb)]

Si se invoca la acción CompanySecrets() escribiendo la dirección URL /Home/CompanySecrets en la barra de direcciones del explorador y no es un usuario autenticado, a continuación, se le redirigirá a la vista de inicio de sesión automáticamente (consulte la figura 5).

**Figura 5: la vista de inicio de sesión**

![clip_image010[4]](authenticating-users-with-forms-authentication-vb/_static/image5.jpg)

Puede usar la vista de inicio de sesión para escribir su nombre de usuario y contraseña. Si no es un usuario registrado, a continuación, puede hacer clic en el **registrar** vínculo para navegar a la caja registradora ver (consulte la figura 6). Puede utilizar la vista de registro para crear una nueva cuenta de usuario.

**Figura 6: la vista de registro**

![clip_image012](authenticating-users-with-forms-authentication-vb/_static/image6.jpg)

Después de iniciar sesión correctamente, se puede ver el CompanySecrets (consulte la figura 7). De forma predeterminada, se continuará iniciar sesión hasta que cierre la ventana del explorador.

**Figura 7: la vista de CompanySecrets**

![clip_image014](authenticating-users-with-forms-authentication-vb/_static/image7.jpg)

#### <a name="authorizing-by-user-name-or-user-role"></a>Autorizar por nombre de usuario o rol de usuario

Puede utilizar el atributo [Authorize] para restringir el acceso a una acción de controlador a un conjunto determinado de usuarios o un conjunto determinado de roles de usuario. Por ejemplo, el controlador Home modificado en el listado 2 contiene dos nuevas acciones denominadas StephenSecrets() y AdministratorSecrets().

**La lista 2 – Controllers\HomeController.vb**

[!code-vb[Main](authenticating-users-with-forms-authentication-vb/samples/sample2.vb)]

Solo un usuario con el nombre de usuario Stephen puede invocar la acción StephenSecrets(). Todos los demás usuarios le redirigirá a la vista de inicio de sesión. La propiedad de los usuarios acepta una lista separada por comas de nombres de cuenta de usuario.

Solo los usuarios de la función administradores pueden invocar la acción AdministratorSecrets(). Por ejemplo, porque Sally es miembro del grupo Administradores, puede invocar la acción AdministratorSecrets(). Todos los demás usuarios le redirigirá a la vista de inicio de sesión. La propiedad Roles acepta una lista separada por comas de nombres de función.

#### <a name="configuring-authentication"></a>Configurar la autenticación

En este punto, tal vez se pregunte dónde se se almacena la información de cuenta y el rol de usuario. De forma predeterminada, la información se almacena en una base de datos (RANU) SQL Express ASPNETDB.mdf con nombre se encuentra en la aplicación de la aplicación MVC\_carpeta de datos. Esta base de datos se genera automáticamente el marco de trabajo de ASP.NET cuando se inicia mediante la suscripción.

Para ver la base de datos ASPNETDB.mdf en la ventana Explorador de soluciones, primero debe seleccionar la opción de menú proyecto, mostrar todos los archivos.

Uso de la base de datos de SQL Express de forma predeterminada es correcto al desarrollar una aplicación. Probablemente, sin embargo, no será necesario utilizar la base de datos de ASPNETDB.mdf predeterminado para una aplicación de producción. En ese caso, puede cambiar donde se almacena la información de la cuenta de usuario siguiendo los dos pasos siguientes:

1. Agregar los objetos de base de datos de servicios de aplicación a la base de datos de producción: cambiar la cadena de conexión de la aplicación para que apunte a la base de datos de producción

El primer paso es agregar todos los objetos de base de datos necesarios (tablas y procedimientos almacenados) a la base de datos de producción. La manera más fácil de agregar estos objetos a una base de datos consiste en aprovechar el Asistente para la instalación de ASP.NET SQL Server (consulte la figura 8). Puede iniciar esta herramienta, abra el símbolo del sistema de Visual Studio 2008 desde el grupo de programas de Microsoft Visual Studio 2008 y ejecutar el comando siguiente desde el símbolo del sistema:

aspnet\_regsql

**Figura 8: el Asistente para la instalación del servidor de SQL de ASP.NET**

![clip_image016](authenticating-users-with-forms-authentication-vb/_static/image8.jpg)

El Asistente para la instalación de ASP.NET SQL Server le permite seleccionar una base de datos de SQL Server en la red e instalar todos los objetos de base de datos requeridos por los servicios de aplicación de ASP.NET. El servidor de base de datos no es necesario que se encuentre en el equipo local.

> [!NOTE]
> Si no desea usar al Asistente para la instalación de ASP.NET SQL Server, puede encontrar secuencias de comandos SQL para agregar los objetos de base de datos de servicios de aplicación en la siguiente carpeta:
> 
> 
> C:\Windows\Microsoft.NET\Framework\v2.0.50727


Después de crear los objetos de base de datos necesarios, debe modificar la conexión de base de datos usada por la aplicación de MVC. Modifique la cadena de conexión de ApplicationServices en el archivo de configuración (web.config) web para que apunte a la base de datos de producción. Por ejemplo, la conexión modificada en el listado 3 apunta a una base de datos denominada MyProductionDB (la cadena de conexión de ApplicationServices original se convirtió en comentario).

**Enumerar 3: Web.config**

[!code-xml[Main](authenticating-users-with-forms-authentication-vb/samples/sample3.xml)]

#### <a name="configuring-database-permissions"></a>Configurar permisos de base de datos

Si utiliza la seguridad integrada para conectarse a la base de datos, a continuación, debe agregar la cuenta de usuario de Windows correcta como un inicio de sesión a la base de datos. La cuenta correcta depende de si se usa el servidor de desarrollo de ASP.NET o servicios de Internet Information Server como el servidor web. La cuenta de usuario correcta también depende de su sistema operativo.

Si está utilizando el servidor de desarrollo de ASP.NET (el servidor de web predeterminado utilizado por Visual Studio), a continuación, la aplicación se ejecuta en el contexto de la cuenta de usuario de Windows. En ese caso, debe agregar la cuenta de usuario de Windows como un inicio de sesión del servidor de base de datos.

O bien, si está utilizando Internet Information Services, a continuación, debe agregar la cuenta ASPNET o la cuenta de servicio de red o autoridad de NT como un inicio de sesión del servidor de base de datos. Si está utilizando Windows XP, agregue la cuenta ASPNET como un inicio de sesión a la base de datos. Si está utilizando un sistema operativo más reciente, como Windows Vista o Windows Server 2008, a continuación, agregue la cuenta de servicio de entidad o de la red de NT como el inicio de sesión de base de datos.

Puede agregar una nueva cuenta de usuario a la base de datos mediante el uso de Microsoft SQL Server Management Studio (consulte la figura 9).

**Figura 9: crear un nuevo inicio de sesión de Microsoft SQL Server**

![clip_image018](authenticating-users-with-forms-authentication-vb/_static/image9.jpg)

Después de crear el inicio de sesión necesario, debe asignar el inicio de sesión a un usuario de base de datos con los roles de base de datos correcto. Haga doble clic en el inicio de sesión y seleccione la pestaña asignación de usuario. Seleccione uno o varios roles de base de datos de servicios de aplicación. Por ejemplo, con el fin de autenticar a los usuarios, debe habilitar aspnet\_pertenencia\_BasicAccess rol de base de datos. Para crear nuevos usuarios, debe habilitar aspnet\_pertenencia\_FullAccess rol de base de datos (consulte la figura 10).

**Figura 10: agregar roles de base de datos de servicios de aplicaciones**

![clip_image020](authenticating-users-with-forms-authentication-vb/_static/image10.jpg)

#### <a name="summary"></a>Resumen

En este tutorial, aprendió a utilizar la autenticación de formularios al crear una aplicación ASP.NET MVC. En primer lugar, aprendió a crear nuevos usuarios y roles aprovechando las ventajas de la herramienta de administración de sitios Web. A continuación, aprendió a utilizar el atributo [Authorize] para evitar que usuarios no autorizados de invocar acciones del controlador. Por último, aprendió a configurar la aplicación de MVC para almacenar información de usuarios y roles en una base de datos de producción.

> [!div class="step-by-step"]
> [Anterior](preventing-javascript-injection-attacks-cs.md)
> [Siguiente](authenticating-users-with-windows-authentication-vb.md)
