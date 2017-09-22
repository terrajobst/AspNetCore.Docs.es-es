---
title: "Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización"
author: rick-anderson
keywords: "Núcleo de ASP.NET, MVC, autorización, roles, seguridad, Administrador"
ms.author: riande
manager: wpickett
ms.date: 05/22/2017
ms.topic: article
ms.assetid: abeb2f8e-dfbf-4398-a04c-338a613a65bc
ms.technology: aspnet
ms.prod: aspnet-core
uid: security/authorization/secure-data
ms.openlocfilehash: 54a737f140a8434035e5fc5abfefa458fdb69321
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/22/2017
---
# <a name="create-an-aspnet-core-app-with-user-data-protected-by-authorization"></a>Crear una aplicación de ASP.NET Core con datos de usuario protegidos por autorización

Por [Rick Anderson](https://twitter.com/RickAndMSFT) y [Joe Audette](https://twitter.com/joeaudette)

Este tutorial muestra cómo crear una aplicación web con datos de usuario protegidos mediante autorización. Muestra una lista de contactos que autentica los usuarios (registrados) ha creado. Hay tres grupos de seguridad:

* Los usuarios registrados pueden ver todos los datos de contacto aprobados.
* Los usuarios registrados pueden editar o eliminar sus propios datos. 
* Los administradores pueden aprobar o rechazar datos de contacto. Sólo los contactos aprobados son visibles para los usuarios.
* Los administradores pueden aprobar o rechazar y editar o eliminar los datos.

En la siguiente imagen, usuario Rick (`rick@example.com`) ha iniciado sesión. Usuario Rick solo pueden ver los contactos aprobados y editar o eliminar sus contactos. El último registro, creado por Rick, muestra Editar y eliminar los vínculos

![imagen que se ha descrito anteriormente](secure-data/_static/rick.png)

En la siguiente imagen, `manager@contoso.com` está firmado en y en el rol de administrador. 

![imagen que se ha descrito anteriormente](secure-data/_static/manager1.png)

La siguiente imagen muestra a los administradores de la vista de detalles de un contacto.

![imagen que se ha descrito anteriormente](secure-data/_static/manager.png)

Sólo los administradores y los administradores tienen la aprobar y rechazan botones.

En la siguiente imagen, `admin@contoso.com` está firmado en y en el rol del administrador. 

![imagen que se ha descrito anteriormente](secure-data/_static/admin.png)

El administrador tiene todos los privilegios. Puede leer, editar o eliminar cualquier contacto y cambiar el estado de contactos.

La aplicación se creó con [scaffolding](xref:tutorials/first-mvc-app-xplat/adding-model#scaffold-the-moviecontroller) siguiente `Contact` modelo:

[!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

Un `ContactIsOwnerAuthorizationHandler` controlador autorización garantiza que un usuario solo puede editar sus datos. Un `ContactManagerAuthorizationHandler` controlador de autorización permite a los administradores aprobar o rechazar los contactos.  Un `ContactAdministratorsAuthorizationHandler` controlador de autorización permite a los administradores para aprobar o rechazar los contactos y editar o eliminar contactos. 

## <a name="prerequisites"></a>Requisitos previos

Esto no es un tutorial de inicio. Debe estar familiarizado con:

* [Núcleo de ASP.NET MVC](xref:tutorials/first-mvc-app/start-mvc)
* [Entity Framework Core](xref:data/ef-mvc/intro)

## <a name="the-starter-and-completed-app"></a>El inicio y la aplicación completada

[Descargar](xref:tutorials/index#how-to-download-a-sample) el [completado](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/final) aplicación. [Prueba](#test-the-completed-app) la aplicación completada por lo que se familiarice con sus características de seguridad. 

### <a name="the-starter-app"></a>La aplicación de inicio

Es útil comparar el código con el ejemplo completo.

[Descargar](xref:tutorials/index#how-to-download-a-sample) el [starter](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/secure-data/samples/starter) aplicación. 

Vea [crear la aplicación de inicio](#create-the-starter-app) si le gustaría crear desde cero.

Actualizar la base de datos:

```none
   dotnet ef database update
```

Ejecutar la aplicación, pulse el **ContactManager** vincular y compruebe que puede crear, editar y eliminar un contacto.

Este tutorial tiene todos los pasos principales para crear la aplicación de datos de usuario seguras. Le resultará útil para hacer referencia al proyecto completado.

## <a name="modify-the-app-to-secure-user-data"></a>Modificar la aplicación para proteger los datos de usuario

Las siguientes secciones contienen todos los pasos principales para crear la aplicación de datos de usuario seguras. Le resultará útil para hacer referencia al proyecto completado.

### <a name="tie-the-contact-data-to-the-user"></a>Asociar los datos de contacto para el usuario

Usar ASP.NET [identidad](xref:security/authentication/identity) Id. de usuario para garantizar que los usuarios puede editar sus datos, pero no otros datos de los usuarios. Agregar `OwnerID` a la `Contact` modelo:

[!code-csharp[Main](secure-data/samples/final/Models/Contact.cs?name=snippet1&highlight=5-6,16-)]

`OwnerID`es el identificador del usuario desde el `AspNetUser` tabla el [identidad](xref:security/authentication/identity) base de datos. El `Status` campo determina si un contacto es visible para los usuarios en general. 

Aplicar la técnica scaffolding una nueva migración y actualizar la base de datos:

```console
dotnet ef migrations add userID_Status
dotnet ef database update
 ```

### <a name="require-ssl-and-authenticated-users"></a>Requerir SSL y los usuarios autenticados

En el `ConfigureServices` método de la *Startup.cs* , agregue el [RequireHttpsAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.requirehttpsattribute) filtro de autorización:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_SSL&highlight=1)]

Si está utilizando Visual Studio, consulte [configurar IIS Express para HTTPS/SSL](xref:security/enforcing-ssl#set-up-iis-express-for-sslhttps). Para redirigir las solicitudes HTTP a HTTPS, consulte [Middleware de reescritura de dirección URL](xref:fundamentals/url-rewriting). Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:

- Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.

### <a name="require-authenticated-users"></a>Requerir a los usuarios autenticados

Establecer la directiva de autenticación predeterminado para exigir que los usuarios se autentiquen. Puede rechazar la autenticación en el método de acción o controlador con el `[AllowAnonymous]` atributo. Con este enfoque, los nuevos controladores que se agrega automáticamente requerirá la autenticación, que es más segura que confiar en los nuevos controladores para incluir la `[Authorize]` atributo. Agregue lo siguiente a la `ConfigureServices` método de la *Startup.cs* archivo:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=snippet_defaultPolicy)]

Agregar `[AllowAnonymous]` al controlador home para los usuarios anónimos pueden recibir información sobre el sitio antes de que registrar.

[!code-csharp[Main](secure-data/samples/final/Controllers/HomeController.cs?name=snippet1&highlight=2,6)]

### <a name="configure-the-test-account"></a>Configurar la cuenta de prueba

La `SeedData` clase crea dos cuentas, el administrador y el administrador. Use la [herramienta secreto administrador](xref:security/app-secrets) para establecer una contraseña para estas cuentas. Desde el directorio del proyecto (el directorio que contiene *Program.cs*).

```console
dotnet user-secrets set SeedUserPW <PW>
```

Actualización `Configure` usar la contraseña de prueba:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=Configure&highlight=19-21)]

Agregue el identificador de usuario de administrador y `Status = ContactStatus.Approved` a los contactos. Se muestra sólo un contacto, agregue el identificador de usuario para todos los contactos:

[!code-csharp[Main](secure-data/samples/final/Data/SeedData.cs?name=snippet1&highlight=17,18)]

## <a name="create-owner-manager-and-administrator-authorization-handlers"></a>Crear propietario, el administrador y los controladores de autorización de administrador

Crear un `ContactIsOwnerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactIsOwnerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es propietario del recurso.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactIsOwnerAuthorizationHandler.cs)]

El `ContactIsOwnerAuthorizationHandler` llamadas `context.Succeed` si el usuario autenticado actual es el propietario del contacto. Controladores de autorización generalmente devuelven `context.Succeed` cuando se cumplen los requisitos. Devuelven `Task.FromResult(0)` cuando no se cumplen los requisitos. `Task.FromResult(0)`no correcto o error, permite que otro controlador de autorización para que se ejecute. Si necesita explícitamente un error, devuelve `context.Fail()`.

Se permiten los propietarios de contacto editar o eliminar sus propios datos, por lo que no es necesario comprobar la operación pasada en el parámetro de requisito.

### <a name="create-a-manager-authorization-handler"></a>Crear un controlador del Administrador de autorización

Crear un `ContactManagerAuthorizationHandler` clase en el *autorización* carpeta. El `ContactManagerAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador. Solo los administradores pueden aprobar o rechazar los cambios de contenido (nuevos o modificados).

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactManagerAuthorizationHandler.cs)]

### <a name="create-an-administrator-authorization-handler"></a>Crear un controlador de autorización de administrador

Crear un `ContactAdministratorsAuthorizationHandler` clase en el *autorización* carpeta. El `ContactAdministratorsAuthorizationHandler` comprobará que el usuario que actúa en el recurso es un administrador. Administrador puede hacer que todas las operaciones.

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactAdministratorsAuthorizationHandler.cs)]

## <a name="register-the-authorization-handlers"></a>Registrar los controladores de autorización

Servicios mediante Entity Framework Core deben estar registrados para [inyección de dependencia](xref:fundamentals/dependency-injection) con [AddScoped](/aspnet/core/api/microsoft.extensions.dependencyinjection.servicecollectionserviceextensions). El `ContactIsOwnerAuthorizationHandler` usa ASP.NET Core [identidad](xref:security/authentication/identity), que se basa en Entity Framework Core. Registre los controladores con la colección de servicio para que estén disponibles para la `ContactsController` a través de [inyección de dependencia](xref:fundamentals/dependency-injection). Agregue el código siguiente al final de `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=AuthorizationHandlers)]

`ContactAdministratorsAuthorizationHandler`y `ContactManagerAuthorizationHandler` se agregan como singleton. Son singletons porque no usan EF y toda la información necesaria se encuentra en la `Context` parámetro de la `HandleRequirementAsync` método.

La sección completa `ConfigureServices`:

[!code-csharp[Main](secure-data/samples/final/Startup.cs?name=ConfigureServices)]

## <a name="update-the-code-to-support-authorization"></a>Actualizar el código para admitir la autorización

En esta sección, actualice el controlador y vistas y agregar una clase de requisitos de las operaciones.

### <a name="update-the-contacts-controller"></a>Actualice el controlador de contactos

Actualización de la `ContactsController` constructor:

* Agregar el `IAuthorizationService` servicio para tener acceso a los controladores de autorización. 
* Agregar el `Identity` `UserManager` servicio:

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_ContactsControllerCtor)]

### <a name="add-a-contact-operations-requirements-class"></a>Agregar una clase de requisitos de operaciones de contacto

Agregar el `ContactOperationsRequirements` clase a la *autorización* carpeta. Esta clase contiene los requisitos de la aplicación admite:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

### <a name="update-create"></a>Crear actualización

Actualización de la `HTTP POST Create` método:

* Agregar el identificador de usuario para el `Contact` modelo.
* Llamar al controlador de autorización para comprobar que el usuario es propietario del contacto.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Create)]

### <a name="update-edit"></a>Edición de actualización

Actualizarlas `Edit` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto. Dado que se está realizando la autorización de recurso no podemos usar la `[Authorize]` atributo. No tenemos acceso a los recursos cuando se evalúan los atributos. Autorización basada en recursos debe ser imperativo. Las comprobaciones deben realizarse una vez que tenemos acceso a los recursos, cargándolo en un control o mediante la carga dentro de su propio controlador. Con frecuencia tendrán acceso al recurso pasando la clave de recurso.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Edit)]

### <a name="update-the-delete-method"></a>El método Delete de actualización

Actualizarlas `Delete` métodos para usar el controlador de autorización para comprobar que el usuario propietario del contacto.

[!code-csharp[Main](secure-data/samples/final/Controllers/ContactsController.cs?name=snippet_Delete)]

## <a name="inject-the-authorization-service-into-the-views"></a>Insertar el servicio de autorización en las vistas

Actualmente se muestra en la interfaz de usuario edita y elimina los vínculos de datos que no se puede modificar el usuario. Se corregirá aplicando el controlador de autorización a las vistas.

Insertar el servicio de autorización en el *Views/_ViewImports.cshtml* por lo que estará disponible para todas las vistas de archivos:

[!code-html[Main](secure-data/samples/final/Views/_ViewImports.cshtml)]

Actualización de la *Views/Contacts/Index.cshtml* vista Razor para mostrar la edición solo y eliminar los vínculos para los usuarios que pueden el contacto de editar o eliminar.

Agregar`@using ContactManager.Authorization;`

Actualización de la `Edit` y `Delete` vincula por lo que solo se representan para que los usuarios con permiso Modificar y eliminar el contacto.

[!code-html[Main](secure-data/samples/final/Views/Contacts/Index.cshtml?range=63-84)]

Advertencia: Ocultar los vínculos de los usuarios que no tienen permiso para editar o eliminar los datos no protege la aplicación. Ocultar los vínculos hace que la aplicación de usuario más descriptivo mostrando vínculos solo es válido. Los usuarios pueden hack las direcciones URL generadas para invocar Editar y eliminar operaciones en los datos que no poseen.  El controlador debe repetir que comprueba el acceso para que sea seguro.

### <a name="update-the-details-view"></a>Actualizar la vista de detalles

Actualice la vista de detalles para que los administradores pueden aprobar o rechazar contactos:

[!code-html[Main](secure-data/samples/final/Views/Contacts/Details.cshtml?range=53-)]

## <a name="test-the-completed-app"></a>Probar la aplicación completada

Si está utilizando Visual Studio Code o pruebas en plataforma local no incluye un certificado de prueba para SSL:

- Establecer `"LocalTest:skipSSL": true` en el *appSettings.JSON que se* archivo.

Si se ha ejecutado la aplicación y tener contactos, elimine todos los registros en el `Contact` de tabla y reinicie la aplicación para inicializar la base de datos. Si se utiliza Visual Studio, debe salir y reiniciar IIS Express al valor de inicialización de la base de datos.

Registrar un usuario para examinar los contactos.

Una manera sencilla de probar la aplicación final consiste en iniciar tres distintos exploradores (o versiones de incógnito/InPrivate). En un explorador, registrar un nuevo usuario, por ejemplo, `test@example.com`. Inicie sesión con un usuario diferente en cada explorador. Compruebe lo siguiente:

* Los usuarios registrados pueden ver todos los datos de contacto aprobados.
* Los usuarios registrados pueden editar o eliminar sus propios datos. 
* Los administradores pueden aprobar o rechazar datos de contacto. El `Details` vista muestra **aprobar** y **rechazar** botones. 
* Los administradores pueden aprobar o rechazar y editar o eliminar los datos.

| Usuario| Opciones |
| ------------ | ---------|
| test@example.com | Editar puede/eliminar datos propios |
| manager@contoso.com | Pueden aprobar o rechazar y editar o eliminar datos de propietario  |
| admin@contoso.com | Puede editar o eliminar y aprobar o rechazar todos los datos|

Crear un contacto en el Explorador de administradores. Copie la dirección URL para eliminar y editar en el contacto del administrador. Pegue estos vínculos en el explorador del usuario de prueba para comprobar que el usuario de prueba no puede realizar estas operaciones.

## <a name="create-the-starter-app"></a>Crear la aplicación de inicio

Siga estas instrucciones para crear la aplicación de inicio.

* Crear un **aplicación Web de ASP.NET Core** con [2017 de Visual Studio](https://www.visualstudio.com/) denominado "ContactManager"

  * Crear la aplicación con **cuentas de usuario individuales**.
  * Asígnele el nombre "ContactManager" para que el espacio de nombres coincidirá con el uso de espacio de nombres en el ejemplo.

* Agregue las siguientes `Contact` modelo:

  [!code-csharp[Main](secure-data/samples/starter/Models/Contact.cs?name=snippet1)]

* Scaffold el `Contact` modelados con Entity Framework Core y `ApplicationDbContext` contexto de datos. Acepte todos los valores predeterminados de la técnica scaffolding. Usar `ApplicationDbContext` para el contexto de datos de clase pone la tabla contacto la [identidad](xref:security/authentication/identity) base de datos. Vea [agregar un modelo](xref:tutorials/first-mvc-app/adding-model) para obtener más información.

* Actualización del **ContactManager** fijar en el *Views/Shared/_Layout.cshtml* de archivos de `asp-controller="Home"` a `asp-controller="Contacts"` por lo que al puntear en el **ContactManager** vínculo invoca el controlador de contactos. El marcado original:

```html
   <a asp-area="" asp-controller="Home" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

El marcado actualizado:

```html
   <a asp-area="" asp-controller="Contacts" asp-action="Index" class="navbar-brand">ContactManager</a>
   ```

* Aplicar la técnica scaffolding la migración inicial y actualizar la base de datos

```none
   dotnet ef migrations add initial
   dotnet ef database update
   ```

* Probar la aplicación mediante la creación, edición y eliminación de un contacto

### <a name="seed-the-database"></a>Inicializar la base de datos

Agregar el `SeedData` clase a la *datos* carpeta. Si ha descargado el ejemplo, puede copiar la *SeedData.cs* del archivo a la *datos* carpeta del proyecto de inicio.

[!code-csharp[Main](secure-data/samples/starter/Data/SeedData.cs)]

Agregue el código resaltado hasta el final de la `Configure` método en el *Startup.cs* archivo:

[!code-csharp[Main](secure-data/samples/starter/Startup.cs?name=Configure&highlight=28-)]

Compruebe que la aplicación había propagado la base de datos. El método de inicialización no se ejecuta si hay filas en la base de datos de contacto.

### <a name="create-a-class-used-in-the-tutorial"></a>Crear una clase usada en el tutorial

* Cree una carpeta denominada *autorización*.
* Copia la *Authorization\ContactOperations.cs* de archivos desde el proyecto completado para descargar o copie el código siguiente:

[!code-csharp[Main](secure-data/samples/final/Authorization/ContactOperations.cs)]

<a name=secure-data-add-resources-label></a>

### <a name="additional-resources"></a>Recursos adicionales

* [Laboratorio de autorización de ASP.NET Core](https://github.com/blowdart/AspNetAuthorizationWorkshop). Este laboratorio se explica con más detalle en las características de seguridad introducidas en este tutorial.
* [Autorización de ASP.NET Core: Simple, rol, basada en notificaciones y personalizado](index.md)
* [Autorización personalizada basada en directivas](policies.md)
