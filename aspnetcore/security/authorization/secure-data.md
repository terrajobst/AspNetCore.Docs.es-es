---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
description: "Obtenga información acerca de cómo crear una aplicación de páginas de Razor con datos de usuario protegidos mediante autorización. Incluye HTTPS, la autenticación, la seguridad, ASP.NET Core Identity."
manager: wpickett
ms.author: riande
ms.date: 01/24/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/secure-data
ms.openlocfilehash: 5acb65be078fd39b9e7a17ce2d8167b8f7b7db22
ms.sourcegitcommit: 7ac15eaae20b6d70e65f3650af050a7880115cbf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/02/2018
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)

Este tutorial muestra cómo crear una aplicación web de ASP.NET Core con datos de usuario protegidos mediante autorización. Muestra una lista de contactos que autentica los usuarios (registrados) ha creado. Hay tres grupos de seguridad:

* **Usuarios registrados** puede ver todos los datos aprobados y editar puede/eliminar sus propios datos.
* **Administradores de** puede aprobar o rechazar los datos de contacto. Sólo los contactos aprobados son visibles para los usuarios.
* **Los administradores** puede aprobar o rechazar y editar o eliminar los datos.

En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión. Rick solo puede ver los contactos aprobados y **editar**/**eliminar**/**crear nuevo** vínculos de sus contactos. El último registro, creado por Rick, muestra **editar** y **eliminar** vínculos. Otros usuarios no verán el último registro hasta que un administrador o un administrador cambia el estado a "Aprobado".

![imagen describe anterior](secure-data/_static/rick.png)

En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador:

![imagen describe anterior](secure-data/_static/manager1.png)

La siguiente imagen muestra a los administradores de la vista de detalles de un contacto:

![imagen describe anterior](secure-data/_static/manager.png)

El **aprobar** y **rechazar** solo se muestran los botones para los administradores y administradores.

En la siguiente imagen, `admin@contoso.com` está firmado en y en la función Administradores:

![imagen describe anterior](secure-data/_static/admin.png)

El administrador tiene todos los privilegios. Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.

La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:

[!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

El ejemplo contiene los siguientes controladores de autorización:

* `ContactIsOwnerAuthorizationHandler`: Se asegura de que un usuario solo puede editar sus datos.
* `ContactManagerAuthorizationHandler`: Permite a los administradores aprobar o rechazar los contactos.
* `ContactAdministratorsAuthorizationHandler`: Permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos.

## <a name="prerequisites"></a>Requisitos previos

Este tutorial se avanza. Debe estar familiarizado con:

* [ASP.NET Core](xref:tutorials/first-mvc-app/start-mvc)
* [Autenticación](xref:security/authentication/index)
* [Confirmación de cuentas y recuperación de contraseñas](xref:security/authentication/accconfirm)
* [Autorización](xref:security/authorization/index)
* [Entity Framework Core](xref:data/ef-mvc/intro)

Vea [este archivo PDF](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/asp.net_repo_pdf_1-16-18.pdf) para la versión principal de ASP.NET MVC. La versión 1.1 de ASP.NET Core de este tutorial está en [esto](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data) carpeta. La 1.1 incluye el ejemplo de ASP.NET Core el [ejemplos](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2).

## <a name="the-starter-and-completed-app"></a>El inicio y la aplicación completada

[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final2) aplicación. [Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad.

### <a name="the-starter-app"></a>La aplicación de inicio

[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter2) aplicación.

Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.

## <a name="secure-user-data"></a>Proteger los datos de usuario

Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras. Le resultará útil para hacer referencia al proyecto completado.

### <a name="tie-the-contact-data-to-the-user"></a>Asociar los datos de contacto para el usuario

Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios. Agregar `OwnerID` y `ContactStatus` a la `Contact` modelo:

[!code-csharp[](secure-data/samples/final2/Models/Contact.cs?name=snippet1&highlight=5-6,16-999)]

`OwnerID` es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos. El `Status` campo determina si un contacto es visible para los usuarios en general.

Crear una nueva migración y actualizar la base de datos:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
```

### <a name="require-https-and-authenticated-users"></a>Requerir HTTPS y los usuarios autenticados

Agregar [IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) a `Startup`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_env)]

En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_SSL&highlight=10-999)]

Si está utilizando Visual Studio, habilitar HTTPS.

Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting). Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:

  Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo.

### <a name="require-authenticated-users"></a>Requerir a los usuarios autenticados

Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen. Puede rechazar la autenticación en el nivel de método página Razor, controlador o acción con el `[AllowAnonymous]` atributo. Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen protege recién agregado las páginas Razor y controladores. Existencia de autenticación que requiere de forma predeterminada es más segura que confiar en los nuevos controladores y las páginas de Razor para incluir la `[Authorize]` atributo. 

Con el requisito de todos los usuarios autenticados, el [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder?view=aspnetcore-2.0#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) y [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage?view=aspnetcore-2.0) llamadas no son necesarias.

Actualización `ConfigureServices` con los cambios siguientes:

* Convertir en comentario `AuthorizeFolder` y `AuthorizePage`.
* Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen.

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=snippet_defaultPolicy&highlight=23-27,31-999)]

Agregar [AllowAnonymous](/dotnet/api/microsoft.aspnetcore.authorization.allowanonymousattribute) al índice, las páginas sobre y, a continuación, póngase en contacto con por lo que los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar. 

[!code-csharp[](secure-data/samples/final2/Pages/Index.cshtml.cs?name=snippet&highlight=2)]

Agregar `[AllowAnonymous]` a la [LoginModel y RegisterModel](https://github.com/aspnet/templating/issues/238).

### <a name="configure-the-test-account"></a>Configurar la cuenta de prueba

La `SeedData` clase crea dos cuentas: administrador y el administrador. Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas. Establecer la contraseña desde el directorio del proyecto (el directorio que contiene *Program.cs*):

```console
dotnet user-secrets set SeedUserPW <PW>
```

Actualización `Main` usar la contraseña de prueba:

[!code-csharp[](secure-data/samples/final2/Program.cs?name=snippet)]

### <a name="create-the-test-accounts-and-update-the-contacts"></a>Crear las cuentas de prueba y actualizar los contactos

Actualización de la `Initialize` método en la `SeedData` clase para crear las cuentas de prueba:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet_Initialize)]

Agregue el identificador de usuario de administrador y `ContactStatus` a los contactos. Realice uno de los contactos "Enviado" y un "rechazada". Agregue el Id. de usuario y el estado para todos los contactos. Póngase en contacto un solo con se muestra:

[!code-csharp[](secure-data/samples/final2/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Crear propietario, el administrador y los controladores de autorización de administrador

Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactIsOwnerAuthorizationHandler` comprueba que el usuario que actúa en un recurso posee el recurso.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

El `ContactIsOwnerAuthorizationHandler` llamadas [contexto. Correctamente](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.succeed#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_Succeed_Microsoft_AspNetCore_Authorization_IAuthorizationRequirement_) si el usuario autenticado actual es el propietario del contacto. Controladores de autorización general:

* Devolver `context.Succeed` cuando se cumplen los requisitos.
* Devolver `Task.CompletedTask` cuando no se cumplen los requisitos. `Task.CompletedTask` no correcto o error&mdash;permite que otros controladores de autorización para que se ejecute.

Si necesita explícitamente un error, devolver [contexto. Un error](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.fail).

La aplicación permite a los propietarios de contacto para editar, eliminar o crear sus propios datos. `ContactIsOwnerAuthorizationHandler` no tiene que comprobar la operación pasada en el parámetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Crear un controlador del Administrador de autorización

Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactManagerAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador. Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).

[!code-csharp[](secure-data/samples/final2/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Crear un controlador de autorización de administrador

Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta. El `ContactAdministratorsAuthorizationHandler` comprueba que el usuario que actúa en el recurso es un administrador. Administrador puede hacer que todas las operaciones.

[!code-csharp[](secure-data/samples/final2/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar los controladores de autorización

Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core. Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection). Agregue el código siguiente al final de `ConfigureServices`:

[!code-csharp[](secure-data/samples/final2/Startup.cs?name=ConfigureServices&highlight=41-999)]

`ContactAdministratorsAuthorizationHandler` y `ContactManagerAuthorizationHandler` se agregan como singleton. Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.

## <a name="support-authorization"></a>Admitir la autorización

En esta sección, actualice las páginas Razor y agregar una clase de requisitos de las operaciones.

### <a name="review-the-contact-operations-requirements-class"></a>Revisión de la clase de requisitos de operaciones de contacto

Revise la `ContactOperations` clase. Esta clase contiene los requisitos de la aplicación admite:

[!code-csharp[](secure-data/samples/final2/Authorization/ContactOperations.cs)]

### <a name="create-a-base-class-for-the-razor-pages"></a>Crear una clase base para las páginas de Razor

Cree una clase base que contiene los servicios usados en los contactos de las páginas de Razor. La clase base coloca ese código de inicialización en una ubicación:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/DI_BasePageModel.cs)]

El código anterior:

* Agrega el `IAuthorizationService` servicio para tener acceso a los controladores de autorización.
* Agrega la identidad `UserManager` servicio.
* Agregue la `ApplicationDbContext`.

### <a name="update-the-createmodel"></a>Actualizar el CreateModel

Actualizar el constructor de modelo de página de create para usar la `DI_BasePageModel` clase base:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippetCtor)]

Actualización de la `CreateModel.OnPostAsync` método:

* Agregar el identificador de usuario para el `Contact` modelo.
* Llamar al controlador de autorización para comprobar que el usuario tiene permiso para crear contactos.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Create.cshtml.cs?name=snippet_Create)]

### <a name="update-the-indexmodel"></a>Actualizar la IndexModel

Actualización de la `OnGetAsync` método por lo que sólo los contactos aprobados se muestran a los usuarios en general:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Index.cshtml.cs?name=snippet)]

### <a name="update-the-editmodel"></a>Actualizar la EditModel

Agregue un controlador de autorización para comprobar que el usuario es propietario del contacto. Dado que se está validando la autorización de recursos, el `[Authorize]` atributo no es suficiente. La aplicación no tiene acceso al recurso cuando se evalúan los atributos. Autorización basada en recursos debe ser imperativo. Las comprobaciones deben realizarse una vez que la aplicación tenga acceso al recurso, cargándolo en el modelo de páginas o cargándolo en su propio controlador. Con frecuencia tener acceso al recurso pasando la clave de recurso.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Edit.cshtml.cs?name=snippet)]

### <a name="update-the-deletemodel"></a>Actualizar la DeleteModel

Actualizar el modelo de páginas de delete para usar el controlador de autorización para comprobar que el usuario tiene permiso delete en el contacto.

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Delete.cshtml.cs?name=snippet)]

## <a name="inject-the-authorization-service-into-the-views"></a>Insertar el servicio de autorización en las vistas

Actualmente, se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario. La interfaz de usuario es fijo aplicando el controlador de autorización a las vistas.

Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* para que esté disponible para todas las vistas de archivos:

[!code-cshtml[](secure-data/samples/final2/Pages/_ViewImports.cshtml?highlight=6-9)]

El marcado anterior agrega varias `using` instrucciones.

Actualización de la **editar** y **eliminar** vincula *Pages/Contacts/Index.cshtml* por lo que solo se procesan para los usuarios con los permisos adecuados:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Index.cshtml?highlight=34-36,64-999)]

> [!WARNING]
> Ocultar los vínculos de los usuarios que no tienen permiso para cambiar los datos no proteger la aplicación. Ocultar los vínculos hace que la aplicación más fácil de usar mostrando vínculos solo es válido. Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen. La página de Razor o el controlador debe exigir comprobaciones de acceso para proteger los datos.

### <a name="update-details"></a>Detalles de la actualización

Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:

[!code-cshtml[](secure-data/samples/final2/Pages/Contacts/Details.cshtml?range=48-999)]

Actualice el modelo de páginas de detalles:

[!code-csharp[](secure-data/samples/final2/Pages/Contacts/Details.cshtml.cs?name=snippet)]

## <a name="test-the-completed-app"></a>Probar la aplicación completada

Si está usando Visual Studio Code o pruebas en una plataforma local que no incluye un certificado de prueba para HTTPS:

* Establecer `"LocalTest:skipSSL": true` en el *appsettings. Developement.JSON* archivo para omitir el requisito de HTTPS. Skip HTTPS solamente en un equipo de desarrollo.

Si la aplicación tiene contactos:

* Eliminar todos los registros en la `Contact` tabla.
* Reinicie la aplicación para inicializar la base de datos.

Registrar un usuario para examinar los contactos.

Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate). En un explorador, registrar un nuevo usuario (por ejemplo, `test@example.com`). Inicie sesión con un usuario diferente en cada explorador. Compruebe las operaciones siguientes:

* Los usuarios registrados pueden ver todos los datos de contacto aprobados.
* Los usuarios registrados pueden editar o eliminar sus propios datos.
* Los administradores pueden aprobar o rechazar datos de contacto. El `Details` vista muestra **aprobar** y **rechazar** botones.
* Los administradores pueden aprobar o rechazar y editar o eliminar los datos.

| Usuario| Opciones |
| ------------ | ---------|
| test@example.com | Editar puede/eliminar datos propios |
| manager@contoso.com | Pueden aprobar o rechazar y editar o eliminar datos de propietario |
| admin@contoso.com | Puede editar o eliminar y aprobar o rechazar todos los datos|

Crear un contacto en el explorador del administrador. Copie la dirección URL para eliminar y editar en el contacto del administrador. Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.

## <a name="create-the-starter-app"></a>Crear la aplicación de inicio

* Crear una aplicación de páginas de Razor denominada "ContactManager"

  * Crear la aplicación con **cuentas de usuario individuales**.
  * Asígnele el nombre "ContactManager" para el espacio de nombres coincide con el espacio de nombres utilizado en el ejemplo.

  ```console
  dotnet new razor -o ContactManager -au Individual -uld
  ```

  * `-uld` Especifica LocalDB en lugar de SQLite

* Agregue las siguientes `Contact` modelo:

  [!code-csharp[](secure-data/samples/starter2/Models/Contact.cs?name=snippet1)]

* Scaffold el `Contact` modelo:

```console
dotnet aspnet-codegenerator razorpage -m Contact -udl -dc ApplicationDbContext -outDir Pages\Contacts --referenceScriptLibraries
```

* Actualización de la **ContactManager** fijar en el *Pages/_Layout.cshtml* archivo:

```cshtml
<a asp-page="/Contacts/Index" class="navbar-brand">ContactManager</a>
```

* Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos:

```console
dotnet ef migrations add initial
dotnet ef database update
```

* Probar la aplicación mediante la creación, edición y eliminación de un contacto

### <a name="seed-the-database"></a>Inicializar la base de datos

Agregar el `SeedData` clase a la *datos* carpeta. Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.

Llame a `SeedData.Initialize` de `Main`:

[!code-csharp[](secure-data/samples/starter2/Program.cs?name=snippet)]

Compruebe que la aplicación había propagado la base de datos. Si no hay ninguna fila en la base de datos de contacto, no se ejecuta el método de inicialización.

<a name="secure-data-add-resources-label"></a>

### <a name="additional-resources"></a>Recursos adicionales

* [Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.
* [Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado](xref:security/authorization/index)
* [Autorización personalizada basada en directivas](xref:security/authorization/policies)
