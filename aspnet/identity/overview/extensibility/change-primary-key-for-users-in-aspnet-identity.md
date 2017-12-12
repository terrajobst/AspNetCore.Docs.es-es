---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Cambiar la clave principal para los usuarios en ASP.NET Identity | Documentos de Microsoft
author: tfitzmac
description: "En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena de la clave para las cuentas de usuario. Identidad de ASP.NET le permite cambiar el tipo de la..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a>Cambiar la clave principal para los usuarios de identidad de ASP.NET
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena de la clave para las cuentas de usuario. Identidad de ASP.NET le permite cambiar el tipo de la clave para cumplir los requisitos de datos. Por ejemplo, puede cambiar el tipo de la clave de una cadena en un entero.
> 
> Este tema muestra cómo comenzar con el valor predeterminado de aplicación web y cambiar la clave de cuenta de usuario a un entero. Puede utilizar las mismas modificaciones para implementar cualquier tipo de clave en el proyecto. Muestra cómo realizar estos cambios en la aplicación web predeterminada, pero podría aplicar modificaciones similares en una aplicación personalizada. Muestra los cambios necesarios cuando se trabaja con MVC o formularios Web Forms.
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - Visual Studio 2013 Update 2 (o posterior)
> - ASP.NET Identity 2.1 o posterior


Para llevar a cabo los pasos de este tutorial, debe tener Visual Studio 2013 Update 2 (o posterior) y una aplicación web creada desde la plantilla de aplicación Web ASP.NET. La plantilla cambiada en Update 3. En este tema se muestra cómo cambiar la plantilla en Update 2 y Update 3.

Este tema contiene las siguientes secciones:

- [Cambiar el tipo de la clave en la clase de usuario de identidad](#userclass)
- [Agregar clases de identidad personalizadas que utilizan el tipo de clave](#customclass)
- [Cambiar el Administrador de usuario y la clase de contexto para usar el tipo de clave](#context)
- [Cambiar la configuración de inicio para usar el tipo de clave](#startup)
- [En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave](#mvcupdate2)
- [Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave](#mvcupdate3)
- [Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave](#webformsupdate2)
- [Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave](#webformsupdate3)
- [Ejecutar la aplicación](#run)
- [Otros recursos](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a>Cambiar el tipo de la clave en la clase de usuario de identidad

En el proyecto creado a partir de la plantilla de aplicación Web ASP.NET, especifica que la clase ApplicationUser utiliza un entero para la clave para las cuentas de usuario. En IdentityModels.cs, cambie la clase ApplicationUser heredar de IdentityUser que tiene un tipo de **int** para el parámetro genérico TKey. También pasa los nombres de clase personalizada tres que aún no ha implementado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

Ha cambiado el tipo de la clave, pero, de forma predeterminada, el resto de la aplicación todavía se da por supuesto que la clave es una cadena. Debe indicar explícitamente el tipo de la clave en el código que supone una cadena.

En el **ApplicationUser** clase, cambie el **GenerateUserIdentityAsync** método incluir int, como se muestra en el código que aparece resaltado a continuación. Este cambio no es necesario para los proyectos de formularios Web Forms con la plantilla de Update 3.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a>Agregar clases de identidad personalizadas que utilizan el tipo de clave

Las demás clases de identidad, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, todavía se configuran para usar una clave de cadena. Crear nuevas versiones de estas clases que especifican un número entero para la clave. No es necesario proporcionar el código de implementación de estas clases, principalmente simplemente se establecer int como clave.

Agregue las siguientes clases en el archivo IdentityModels.cs.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a>Cambiar el Administrador de usuario y la clase de contexto para usar el tipo de clave

En IdentityModels.cs, cambie la definición de la **ApplicationDbContext** clase se debe utilizar la nueva personalizar clases y un **int** de la clave, como se muestra en el código que aparece resaltado.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

El parámetro ThrowIfV1Schema ya no es válido en el constructor. Cambie el constructor, por lo que no pasa un valor de ThrowIfV1Schema.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

Abra IdentityConfig.cs y cambie el **ApplicationUserManger** clase para usar el nuevo usuario de almacén de clase para conservar los datos y un **int** para la clave.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

En la plantilla de Update 3, debe cambiar la clase ApplicationSignInManager.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a>Cambiar la configuración de inicio para usar el tipo de clave

En Startup.Auth.cs, reemplace el código de OnValidateIdentity, como se indica a continuación. Tenga en cuenta que la definición de getUserIdCallback, analiza el valor de cadena en un entero.

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

Si el proyecto no reconoce la implementación genérica de la **GetUserId** método, puede que necesite actualizar el paquete NuGet de identidad de ASP.NET a la versión 2.1

Muchos de los cambios realizados en las clases de infraestructura usa ASP.NET Identity. Si intenta compilar el proyecto, observará una gran cantidad de errores. Afortunadamente, los errores restantes son similares. La clase de identidad espera un número entero para la clave, pero el controlador (o un formulario Web Forms) está pasando un valor de cadena. En cada caso, debe convertir de una cadena a y el entero mediante una llamada a **GetUserId&lt;int&gt;**. Puede trabajar con la lista de errores de compilación o seguir los cambios a continuación.

Los cambios restantes dependen del tipo de proyecto que está creando y actualización que ha instalado en Visual Studio. Puede ir directamente a la sección correspondiente a través de los siguientes vínculos

- [En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave](#mvcupdate2)
- [Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave](#mvcupdate3)
- [Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave](#webformsupdate2)
- [Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a>En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave

Abra el archivo AccountController.cs. Debe cambiar los métodos siguientes.

**ConfirmEmail** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

**Desasociar** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

**Manage(ManageUserViewModel)** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

**LinkLoginCallback** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

**RemoveAccountList** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

**HasPassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a>Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave

Abra el archivo AccountController.cs. Debe cambiar el método siguiente.

**ConfirmEmail** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

**SendCode** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

Abra el archivo ManageController.cs. Debe cambiar los métodos siguientes.

**Índice** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

**RemoveLogin** métodos

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

**AddPhoneNumber** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

**EnableTwoFactorAuthentication** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

**DisableTwoFactorAuthentication** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

**VerifyPhoneNumber** métodos

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

**RemovePhoneNumber** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

**ChangePassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

**SetPassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

**ManageLogins** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

**LinkLoginCallback** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

**HasPassword** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

**HasPhoneNumber** (método)

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a>Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave

Para los formularios Web con la actualización 2, debe cambiar las páginas siguientes.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a>Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave

Para los formularios Web con Update 3, debe cambiar las páginas siguientes.

**Confirm.aspx.CX**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

**RegisterExternalLogin.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

**Manage.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

**VerifyPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

**AddPhoneNumber.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

**ManagePassword.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

**ManageLogins.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

**TwoFactorAuthenticationSignIn.aspx.cs**

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a>Ejecutar la aplicación

Haya finalizado todos los cambios necesarios en la plantilla de aplicación Web predeterminada. Ejecute la aplicación y registrar un nuevo usuario. Después de registrar el usuario, observará que la tabla AspNetUsers tiene una columna de identificador que es un entero.

![nueva clave principal](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

Si previamente ha creado la identidad de ASP.NET tablas con una clave principal distinta, debe realizar algunos cambios adicionales. Si es posible, elimine la base de datos existente. La base de datos volverá a crear con el diseño correcto cuando se ejecuta la aplicación web y agregar un nuevo usuario. Si la eliminación no es posible ejecutar migraciones de code first para cambiar las tablas. Sin embargo, la nueva clave principal de entero no se configurará como una propiedad de identidad de SQL en la base de datos. Debe establecer manualmente la columna de identificador como una identidad.

<a id="other"></a>
## <a name="other-resources"></a>Otros recursos

- [Información general sobre proveedores de almacenamiento personalizado para identidades de ASP.NET](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [Migrar un sitio Web existente de pertenencia SQL a la identidad de ASP.NET](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [Migrar datos de proveedor Universal para la suscripción y perfiles de usuario para la identidad de ASP.NET](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- [Aplicación de ejemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) con la clave principal cambiado
