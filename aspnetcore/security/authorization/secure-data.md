---
title: Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización
author: rick-anderson
description: Obtenga información sobre cómo crear una aplicación de páginas de Razor con datos de usuario protegidos por autorización. Incluye HTTPS, autenticación, seguridad, ASP.NET Core Identity.
ms.author: riande
ms.date: 12/18/2018
ms.custom: mvc, seodec18
uid: security/authorization/secure-data
ms.openlocfilehash: 225d0e3aa51745253d03e614b1c8568b3a6ba731
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017492"
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)

::: moniker range="<= aspnetcore-1.1"

Consulte [este PDF](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_1-16-18.pdf) para la versión de ASP.NET Core MVC. La versión 1.1 de ASP.NET Core de este tutorial se encuentra en [esto](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta. La 1.1 de ejemplo de ASP.NET Core se encuentra en la [ejemplos](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

::: moniker-end

::: moniker range="= aspnetcore-2.0"

Consulte [este pdf](https://webpifeed.blob.core.windows.net/webpifeed/Partners/asp.net_repo_pdf_July16_18.pdf)

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario. Muestra una lista de contactos que autentican los usuarios (registrados) ha creado. Hay tres grupos de seguridad:

* **Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.
* **Los administradores de** puede aprobar o rechazar los datos de contacto. Solo contactos aprobados son visibles para los usuarios.
* **Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.

Las imágenes de este documento no coinciden exactamente con las plantillas más recientes.

En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión. Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos. El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos. Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.

En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

El administrador tiene todos los privilegios. Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.

Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

El ejemplo contiene los siguientes controladores de autorización:

* `ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.
* `ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.
* `ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se avanza. Debe estar familiarizado con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticación](xref:security/authentication/identity)
* [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
* [Autorización](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>El inicio y la aplicación completada

[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app. [Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.

### <a name="the-starter-app"></a>La aplicación de inicio

[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.

Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.

## <a name="secure-user-data"></a>Proteger los datos de usuario

Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura. Resultarle útil consultar el proyecto completado.

### <a name="tie-the-contact-data-to-the-user"></a>Vincular los datos de contacto para el usuario

Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios. Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:

[!code-csharp[](secure-data/samples/final3/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos. El `Status` campo determina si un contacto es visible para los usuarios en general.

Crear una nueva migración y actualización de la base de datos:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Agregar servicios de función a la identidad

Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet2&highlight=9)]

### <a name="require-authenticated-users"></a>Requerir que los usuarios autenticados

Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet&highlight=15-99)] 

 Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo. Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores. Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.

Agregue [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) a las páginas de índice y privacidad para que los usuarios anónimos puedan obtener información sobre el sitio antes de registrarse.

[!code-csharp[](secure-data/samples/final3/Pages/Index.cshtml.cs?highlight=1,7)]

### <a name="configure-the-test-account"></a>Configurar la cuenta de prueba

La `SeedData` clase crea dos cuentas: administrador y administrador. Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas. Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.

Actualización `Main` usar la contraseña de la prueba:

[!code-csharp[](secure-data/samples/final3/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Crear las cuentas de prueba y actualizar los contactos

Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet_Initialize)]

Agregue el identificador de usuario administrador y `ContactStatus` a los contactos. Realice uno de los contactos "Enviado" y un "rechazada". Agregue el Id. de usuario y el estado a todos los contactos. Póngase en contacto un solo con se muestra:

[!code-csharp[](secure-data/samples/final3/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Crear controladores de autorización de administrador, administrador y propietario

Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto. Controladores de autorización con carácter general:

* Devolver `context.Succeed` cuando se cumplen los requisitos.
* Devolver `Task.CompletedTask` cuando no se cumplen los requisitos. `Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.

Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos. `ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Crear un controlador de autorización de administrador

Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador. Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).

[!code-csharp[](secure-data/samples/final3/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Cree un controlador de autorización de administrador

Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta. El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador. Administrador puede realizar todas las operaciones.

[!code-csharp[](secure-data/samples/final3/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar los controladores de autorización

Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core. Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection). Agregue el código siguiente al final de `ConfigureServices`:

[!code-csharp[](secure-data/samples/final3/Startup.cs?name=snippet_defaultPolicy&highlight=23-99)]

`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton. Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.

## <a name="support-authorization"></a>Admitir la autorización

En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.

### <a name="review-the-contact-operations-requirements-class"></a>Revisión de la clase de los requisitos de las operaciones de contacto

Revise el `ContactOperations` clase. Esta clase contiene los requisitos que admite la aplicación:

[!code-csharp[](secure-data/samples/final3/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Crear una clase base para las páginas de Razor de contactos

Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor. La clase base coloca el código de inicialización en una ubicación:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/DI_BasePageModel.cs)]

El código anterior:

* Agrega el `IAuthorizationService` service acceda a los controladores de autorización.
* Agrega la identidad `UserManager` service.
* Agregue la `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Actualizar el CreateModel

Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Actualización de la `CreateModel.OnPostAsync` método:

* Agregue el identificador de usuario para el `Contact` modelo.
* Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Actualizar el IndexModel

Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Actualizar el EditModel

Agregar un controlador de autorización para comprobar que el usuario propietario del contacto. Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente. La aplicación no tiene acceso al recurso cuando se evalúan los atributos. Autorización basada en el recurso debe ser un imperativo. Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo. Con frecuencia acceder al recurso pasando la clave de recurso.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Actualizar el DeleteModel

Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.

[!code-csharp[](secure-data/samples/final3/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Insertar el servicio de autorización en las vistas

Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.

Inserte el servicio de autorización en el archivo *pages/_ViewImports. cshtml* para que esté disponible para todas las vistas:

[!code-cshtml[](secure-data/samples/final3/Pages/_ViewImports.cshtml?highlight=6-99)]

El marcado anterior agrega varios `using` instrucciones.

Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:

[!code-cshtml[](secure-data/samples/final3/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación. Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido. Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen. La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.

### <a name="update-details"></a>Detalles de la actualización

Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Actualice el modelo de página de detalles:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Agregar o quitar un usuario a un rol

Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:

* Quita los privilegios de un usuario. Por ejemplo, silenciar a un usuario en una aplicación de chat.
* Agregar privilegios a un usuario.

## <a name="test-the-completed-app"></a>Probar la aplicación completada

Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:

* Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas. Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.
* Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

Si la aplicación tiene contactos:

* Eliminar todos los registros en el `Contact` tabla.
* Reinicie la aplicación para inicializar la base de datos.

Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito). En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`). Inicie sesión con un usuario diferente en cada explorador. Compruebe las siguientes operaciones:

* Usuarios registrados pueden ver todos los datos de contacto aprobados.
* Los usuarios registrados pueden editar o eliminar sus propios datos.
* Los administradores pueden aprobar o rechazar datos de contacto. El `Details` ver muestra **aprobar** y **rechazar** botones.
* Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.

| Usuario                | Propagadas por la aplicación | Opciones                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | No                | Editar o eliminar los datos propios.                |
| manager@contoso.com | Sí               | Aprobar o rechazar y editar o eliminar los datos propios. |
| admin@contoso.com   | Sí               | Aprobar o rechazar y editar o eliminar todos los datos. |

Crear un contacto en el explorador del administrador. Copie la dirección URL para su eliminación y editar en el contacto del administrador. Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.

## <a name="create-the-starter-app"></a>Crear la aplicación de inicio

* Cree una aplicación de páginas de Razor denominada "ContactManager"
  * Creación de la aplicación con **cuentas de usuario individuales**.
  * Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.
  * `-uld` Especifica LocalDB en lugar de SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Agregue *Models/contact. CS*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold el `Contact` modelo.
* Crear la migración inicial y actualizar la base de datos:

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet tool install -g dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
dotnet ef database drop -f
dotnet ef migrations add initial
dotnet ef database update
  ```

Si experimenta un error con el `dotnet aspnet-codegenerator razorpage` comando, consulte [este problema de github](https://github.com/aspnet/Scaffolding/issues/984).

* Actualice el delimitador de **ContactManager** en el archivo *pages/Shared/_Layout. cshtml* :

 ```cshtml
<a class="navbar-brand" asp-area="" asp-page="/Contacts/Index">ContactManager</a>
  ```

* Probar la aplicación mediante la creación, edición y eliminación de un contacto

### <a name="seed-the-database"></a>Inicializar la base de datos

Agregue la clase [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter3/Data/SeedData.cs) a la carpeta de *datos* :

[!code-csharp[](secure-data/samples/starter3/Data/SeedData.cs)]

Llame a `SeedData.Initialize` desde `Main`:

[!code-csharp[](secure-data/samples/starter3/Program.cs)]

Probar que la aplicación había propagado la base de datos. Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.

::: moniker-end

::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

Este tutorial muestra cómo crear una aplicación web ASP.NET Core con los datos protegidos por autorización del usuario. Muestra una lista de contactos que autentican los usuarios (registrados) ha creado. Hay tres grupos de seguridad:

* **Usuarios registrados** puede ver todos los datos aprobados y pueden sus propios datos de editar o eliminar.
* **Los administradores de** puede aprobar o rechazar los datos de contacto. Solo contactos aprobados son visibles para los usuarios.
* **Los administradores** puede aprobar o rechazar y editar o eliminar todos los datos.

En la siguiente imagen, el usuario Rick (`rick@example.com`) ha iniciado sesión. Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos para sus contactos. El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos. Otros usuarios no verán el último registro hasta un administrador o un administrador cambia el estado a "Aprobado".

![Captura de pantalla con Rick ha iniciado sesión](secure-data/_static/rick.png)

En la siguiente imagen, `manager@contoso.com` ha iniciado sesión y en el rol del administrador:

![Captura de pantalla mostrando manager@contoso.com iniciada](secure-data/_static/manager1.png)

La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:

![Vista del Administrador de un contacto](secure-data/_static/manager.png)

El **aprobar** y **rechazar** solo se muestran los botones para administradores y administradores.

En la siguiente imagen, `admin@contoso.com` ha iniciado sesión y en el rol de administrador:

![Captura de pantalla mostrando admin@contoso.com iniciada](secure-data/_static/admin.png)

El administrador tiene todos los privilegios. Puede leer, editar o eliminar cualquier contacto y cambiar el estado de los contactos.

Se ha creado la aplicación por [scaffolding](xref:tutorials/first-mvc-app/adding-model#scaffold-the-movie-model) siguiente `Contact` modelo:

[!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

El ejemplo contiene los siguientes controladores de autorización:

* `ContactIsOwnerAuthorizationHandler`: Garantiza que un usuario solo puede editar sus datos.
* `ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos.
* `ContactAdministratorsAuthorizationHandler`: Permite a los administradores aprobar o rechazar contactos y editar o eliminar contactos.

## <a name="prerequisites"></a>Requisitos previos

En este tutorial se avanza. Debe estar familiarizado con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticación](xref:security/authentication/identity)
* [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
* [Autorización](xref:security/authorization/introduction)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>El inicio y la aplicación completada

[Descargar](xref:index#how-to-download-a-sample) el [completado](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples) app. [Prueba](#test-the-completed-app) la aplicación completa, por lo que se familiarice con sus características de seguridad.

### <a name="the-starter-app"></a>La aplicación de inicio

[Descargar](xref:index#how-to-download-a-sample) el [starter](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/) app.

Ejecute la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.

## <a name="secure-user-data"></a>Proteger los datos de usuario

Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario segura. Resultarle útil consultar el proyecto completado.

### <a name="tie-the-contact-data-to-the-user"></a>Vincular los datos de contacto para el usuario

Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios. Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:

[!code-csharp[](secure-data/samples/final2.1/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` es el identificador del usuario desde el `AspNetUser` de tabla en la [identidad](xref:security/authentication/identity) base de datos. El `Status` campo determina si un contacto es visible para los usuarios en general.

Crear una nueva migración y actualización de la base de datos:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="add-role-services-to-identity"></a>Agregar servicios de función a la identidad

Anexar [AddRoles](/dotnet/api/microsoft.aspnetcore.identity.identitybuilder.addroles#Microsoft_AspNetCore_Identity_IdentityBuilder_AddRoles__1) para agregar servicios de rol:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet2&highlight=12)]

### <a name="require-authenticated-users"></a>Requerir que los usuarios autenticados

Establezca la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet&highlight=17-99)] 

 Puede rechazar la autenticación en el nivel de método de página de Razor, controlador o acción con el `[AllowAnonymous]` atributo. Configuración de la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregada de las páginas de Razor y controladores. Tener la autenticación requerida por el valor predeterminado es más segura que confiar en los nuevos controladores y las páginas de Razor debe incluir el `[Authorize]` atributo.

Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, acerca de y póngase en contacto con páginas para los usuarios anónimos pueden obtener información sobre el sitio antes de que se registren.

[!code-csharp[](secure-data/samples/final2.1/Pages/Index.cshtml.cs?highlight=1,6)]

### <a name="configure-the-test-account"></a>Configurar la cuenta de prueba

La `SeedData` clase crea dos cuentas: administrador y administrador. Use la [herramienta Secret Manager](xref:security/app-secrets) para establecer una contraseña para estas cuentas. Establecer la contraseña del directorio de proyecto (el directorio que contiene *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Si no se especifica una contraseña segura, se produce una excepción cuando `SeedData.Initialize` se llama.

Actualización `Main` usar la contraseña de la prueba:

[!code-csharp[](secure-data/samples/final2.1/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Crear las cuentas de prueba y actualizar los contactos

Actualización de la `Initialize` método en el `SeedData` clase para crear las cuentas de prueba:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet_Initialize)]

Agregue el identificador de usuario administrador y `ContactStatus` a los contactos. Realice uno de los contactos "Enviado" y un "rechazada". Agregue el Id. de usuario y el estado a todos los contactos. Póngase en contacto un solo con se muestra:

[!code-csharp[](secure-data/samples/final2.1/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Crear controladores de autorización de administrador, administrador y propietario

Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúan en un recurso posee el recurso.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Se realizan correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto. Controladores de autorización con carácter general:

* Devolver `context.Succeed` cuando se cumplen los requisitos.
* Devolver `Task.CompletedTask` cuando no se cumplen los requisitos. `Task.CompletedTask`no es correcto o erróneo&mdash;permite que se ejecuten otros controladores de autorización.

Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos. `ContactIsOwnerAuthorizationHandler` no es necesario comprobar la operación que se pasa en el parámetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Crear un controlador de autorización de administrador

Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador. Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Cree un controlador de autorización de administrador

Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta. El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúan en el recurso es un administrador. Administrador puede realizar todas las operaciones.

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar los controladores de autorización

Se deben registrar los servicios mediante Entity Framework Core para [inserción de dependencias](xref:fundamentals/dependency-injection) mediante [AddScoped](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). El `ContactIsOwnerAuthorizationHandler` utiliza ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core. Registre los controladores con la colección de servicios para que estén disponibles para el `ContactsController` a través de [inserción de dependencias](xref:fundamentals/dependency-injection). Agregue el código siguiente al final de `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2.1/Startup.cs?name=snippet_defaultPolicy&highlight=27-99)]

`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton. Lo singletons con estado porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.

## <a name="support-authorization"></a>Admitir la autorización

En esta sección, actualice las páginas de Razor y agregue una clase de los requisitos de operaciones.

### <a name="review-the-contact-operations-requirements-class"></a>Revisión de la clase de los requisitos de las operaciones de contacto

Revise el `ContactOperations` clase. Esta clase contiene los requisitos que admite la aplicación:

[!code-csharp[](secure-data/samples/final2.1/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-contacts-razor-pages"></a>Crear una clase base para las páginas de Razor de contactos

Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor. La clase base coloca el código de inicialización en una ubicación:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/DI_BasePageModel.cs)]

El código anterior:

* Agrega el `IAuthorizationService` service acceda a los controladores de autorización.
* Agrega la identidad `UserManager` service.
* Agregue la `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Actualizar el CreateModel

Actualizar el constructor del modelo de página create para usar el `DI_BasePageModel` clase base:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Actualización de la `CreateModel.OnPostAsync` método:

* Agregue el identificador de usuario para el `Contact` modelo.
* Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Actualizar el IndexModel

Actualización de la `OnGetAsync` método por lo que solo contactos aprobados se muestran a los usuarios generales:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Actualizar el EditModel

Agregar un controlador de autorización para comprobar que el usuario propietario del contacto. Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente. La aplicación no tiene acceso al recurso cuando se evalúan los atributos. Autorización basada en el recurso debe ser un imperativo. Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de página o cargándola en el controlador de sí mismo. Con frecuencia acceder al recurso pasando la clave de recurso.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Actualizar el DeleteModel

Actualice el modelo de página de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Insertar el servicio de autorización en las vistas

Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos para los contactos que no se puede modificar el usuario.

Insertar el servicio de autorización en el *Views/_viewimports.cshtml* para que esté disponible para todas las vistas de archivos:

[!code-cshtml[](secure-data/samples/final2.1/Pages/_ViewImports.cshtml?highlight=6-99)]

El marcado anterior agrega varios `using` instrucciones.

Actualización de la **editar** y **eliminar** vincula en *Pages/Contacts/Index.cshtml* por lo que sólo se representen a los usuarios con los permisos adecuados:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Index.cshtml?highlight=34-36,62-999)]

> [!WARNING]
> Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación. Ocultar vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido. Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen. La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.

### <a name="update-details"></a>Detalles de la actualización

Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:

[!code-cshtml[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml?name=snippet)]

Actualice el modelo de página de detalles:

[!code-csharp[](secure-data/samples/final2.1/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="add-or-remove-a-user-to-a-role"></a>Agregar o quitar un usuario a un rol

Consulte [este problema](https://github.com/aspnet/AspNetCore.Docs/issues/8502) para obtener información sobre:

* Quita los privilegios de un usuario. Por ejemplo, silenciar a un usuario en una aplicación de chat.
* Agregar privilegios a un usuario.

## <a name="test-the-completed-app"></a>Probar la aplicación completada

Si ya no ha establecido una contraseña para cuentas de usuario inicializados, utilice el [herramienta Secret Manager](xref:security/app-secrets#secret-manager) para establecer una contraseña:

* Elija una contraseña segura: Use ocho o más caracteres y al menos un carácter, número y símbolo en mayúsculas. Por ejemplo, `Passw0rd!` cumple los requisitos de contraseña segura.
* Ejecute el siguiente comando desde la carpeta del proyecto, donde `<PW>` es la contraseña:

  ```console
  dotnet user-secrets set SeedUserPW <PW>
  ```

* Quitar y actualizar la base de datos

    ```console
     dotnet ef database drop -f
     dotnet ef database update  
     ```

* Reinicie la aplicación para inicializar la base de datos.

Es una manera fácil de probar la aplicación completa iniciar los tres distintos exploradores (o las sesiones de InPrivate o incognito). En un explorador, registre un nuevo usuario (por ejemplo, `test@example.com`). Inicie sesión con un usuario diferente en cada explorador. Compruebe las siguientes operaciones:

* Usuarios registrados pueden ver todos los datos de contacto aprobados.
* Los usuarios registrados pueden editar o eliminar sus propios datos.
* Los administradores pueden aprobar o rechazar datos de contacto. El `Details` ver muestra **aprobar** y **rechazar** botones.
* Los administradores pueden aprobar o rechazar y editar o eliminar todos los datos.

| Usuario                | Propagadas por la aplicación | Opciones                                  |
| ------------------- | :---------------: | ---------------------------------------- |
| test@example.com    | No                | Editar o eliminar los datos propios.                |
| manager@contoso.com | Sí               | Aprobar o rechazar y editar o eliminar los datos propios. |
| admin@contoso.com   | Sí               | Aprobar o rechazar y editar o eliminar todos los datos. |

Crear un contacto en el explorador del administrador. Copie la dirección URL para su eliminación y editar en el contacto del administrador. Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.

## <a name="create-the-starter-app"></a>Crear la aplicación de inicio

* Cree una aplicación de páginas de Razor denominada "ContactManager"
  * Creación de la aplicación con **cuentas de usuario individuales**.
  * Asígnele el nombre "ContactManager" para el espacio de nombres coincida con el espacio de nombres utilizado en el ejemplo.
  * `-uld` Especifica LocalDB en lugar de SQLite

  ```console
  dotnet new webapp -o ContactManager -au Individual -uld
  ```

* Agregue *Models/contact. CS*:

  [!code-csharp[](secure-data/samples/starter2.1/Models/Contact.cs?name=snippet1)]

* Scaffold el `Contact` modelo.
* Crear la migración inicial y actualizar la base de datos:

  ```console
  dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
  dotnet ef database drop -f
  dotnet ef migrations add initial
  dotnet ef database update
  ```

* Actualización de la **ContactManager** anclar en el *Pages/_Layout.cshtml* archivo:

  ```cshtml
  <a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
  ```

* Probar la aplicación mediante la creación, edición y eliminación de un contacto

### <a name="seed-the-database"></a>Inicializar la base de datos

Agregar el [SeedData](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2.1/Data/SeedData.cs) clase a la *datos* carpeta.

Llame a `SeedData.Initialize` desde `Main`:

[!code-csharp[](secure-data/samples/starter2.1/Program.cs?name=snippet)]

Probar que la aplicación había propagado la base de datos. Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.

::: moniker-end

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Recursos adicionales

* [Compilar una aplicación web de .NET Core y SQL Database en Azure App Service](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)
* [Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Este laboratorio explica con más detalle en las características de seguridad, presentadas en este tutorial.
* <xref:security/authorization/introduction>
* [Autorización personalizada basada en directivas](xref:security/authorization/policies)
