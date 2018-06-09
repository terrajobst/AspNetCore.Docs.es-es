---
uid: identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
title: Cambiar la clave principal para los usuarios en ASP.NET Identity | Documentos de Microsoft
author: tfitzmac
description: En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena de la clave para las cuentas de usuario. Identidad de ASP.NET le permite cambiar el tipo de la...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2014
ms.topic: article
ms.assetid: 44925849-5762-4504-a8cd-8f0cd06f6dc3
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /identity/overview/extensibility/change-primary-key-for-users-in-aspnet-identity
msc.type: authoredcontent
ms.openlocfilehash: 79812efb4de2461fad3765d6005bbd20393e62b2
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26498234"
---
<a name="change-primary-key-for-users-in-aspnet-identity"></a><span data-ttu-id="58599-104">Cambiar la clave principal para los usuarios de identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="58599-104">Change Primary Key for Users in ASP.NET Identity</span></span>
====================
<span data-ttu-id="58599-105">por [Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="58599-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="58599-106">En Visual Studio 2013, la aplicación web predeterminada utiliza un valor de cadena de la clave para las cuentas de usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-106">In Visual Studio 2013, the default web application uses a string value for the key for user accounts.</span></span> <span data-ttu-id="58599-107">Identidad de ASP.NET le permite cambiar el tipo de la clave para cumplir los requisitos de datos.</span><span class="sxs-lookup"><span data-stu-id="58599-107">ASP.NET Identity enables you to change the type of the key to meet your data requirements.</span></span> <span data-ttu-id="58599-108">Por ejemplo, puede cambiar el tipo de la clave de una cadena en un entero.</span><span class="sxs-lookup"><span data-stu-id="58599-108">For example, you can change the type of the key from a string to an integer.</span></span>
> 
> <span data-ttu-id="58599-109">Este tema muestra cómo comenzar con el valor predeterminado de aplicación web y cambiar la clave de cuenta de usuario a un entero.</span><span class="sxs-lookup"><span data-stu-id="58599-109">This topic shows how to start with the default web application and change the user account key to an integer.</span></span> <span data-ttu-id="58599-110">Puede utilizar las mismas modificaciones para implementar cualquier tipo de clave en el proyecto.</span><span class="sxs-lookup"><span data-stu-id="58599-110">You can use the same modifications to implement any type of key in your project.</span></span> <span data-ttu-id="58599-111">Muestra cómo realizar estos cambios en la aplicación web predeterminada, pero podría aplicar modificaciones similares en una aplicación personalizada.</span><span class="sxs-lookup"><span data-stu-id="58599-111">It shows how to make these changes in the default web application, but you could apply similar modifications to a customized application.</span></span> <span data-ttu-id="58599-112">Muestra los cambios necesarios cuando se trabaja con MVC o formularios Web Forms.</span><span class="sxs-lookup"><span data-stu-id="58599-112">It shows the changes needed when working with MVC or Web Forms.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="58599-113">Versiones de software que se usa en el tutorial</span><span class="sxs-lookup"><span data-stu-id="58599-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="58599-114">Visual Studio 2013 Update 2 (o posterior)</span><span class="sxs-lookup"><span data-stu-id="58599-114">Visual Studio 2013 with Update 2 (or later)</span></span>
> - <span data-ttu-id="58599-115">ASP.NET Identity 2.1 o posterior</span><span class="sxs-lookup"><span data-stu-id="58599-115">ASP.NET Identity 2.1 or later</span></span>


<span data-ttu-id="58599-116">Para llevar a cabo los pasos de este tutorial, debe tener Visual Studio 2013 Update 2 (o posterior) y una aplicación web creada desde la plantilla de aplicación Web ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="58599-116">To perform the steps in this tutorial, you must have Visual Studio 2013 Update 2 (or later), and a web application created from the ASP.NET Web Application template.</span></span> <span data-ttu-id="58599-117">La plantilla cambiada en Update 3.</span><span class="sxs-lookup"><span data-stu-id="58599-117">The template changed in Update 3.</span></span> <span data-ttu-id="58599-118">En este tema se muestra cómo cambiar la plantilla en Update 2 y Update 3.</span><span class="sxs-lookup"><span data-stu-id="58599-118">This topic shows how to change the template in Update 2 and Update 3.</span></span>

<span data-ttu-id="58599-119">Este tema contiene las siguientes secciones:</span><span class="sxs-lookup"><span data-stu-id="58599-119">This topic contains the following sections:</span></span>

- [<span data-ttu-id="58599-120">Cambiar el tipo de la clave en la clase de usuario de identidad</span><span class="sxs-lookup"><span data-stu-id="58599-120">Change the type of the key in the Identity user class</span></span>](#userclass)
- [<span data-ttu-id="58599-121">Agregar clases de identidad personalizadas que utilizan el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-121">Add customized Identity classes that use the key type</span></span>](#customclass)
- [<span data-ttu-id="58599-122">Cambiar el Administrador de usuario y la clase de contexto para usar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-122">Change the context class and user manager to use the key type</span></span>](#context)
- [<span data-ttu-id="58599-123">Cambiar la configuración de inicio para usar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-123">Change start-up configuration to use the key type</span></span>](#startup)
- [<span data-ttu-id="58599-124">En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-124">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="58599-125">Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-125">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="58599-126">Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-126">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="58599-127">Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-127">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)
- [<span data-ttu-id="58599-128">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="58599-128">Run application</span></span>](#run)
- [<span data-ttu-id="58599-129">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="58599-129">Other resources</span></span>](#other)

<a id="userclass"></a>
## <a name="change-the-type-of-the-key-in-the-identity-user-class"></a><span data-ttu-id="58599-130">Cambiar el tipo de la clave en la clase de usuario de identidad</span><span class="sxs-lookup"><span data-stu-id="58599-130">Change the type of the key in the Identity user class</span></span>

<span data-ttu-id="58599-131">En el proyecto creado a partir de la plantilla de aplicación Web ASP.NET, especifica que la clase ApplicationUser utiliza un entero para la clave para las cuentas de usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-131">In your project created from the ASP.NET Web Application template, specify that the ApplicationUser class uses an integer for the key for user accounts.</span></span> <span data-ttu-id="58599-132">En IdentityModels.cs, cambie la clase ApplicationUser heredar de IdentityUser que tiene un tipo de **int** para el parámetro genérico TKey.</span><span class="sxs-lookup"><span data-stu-id="58599-132">In IdentityModels.cs, change the ApplicationUser class to inherit from IdentityUser that has a type of **int** for the TKey generic parameter.</span></span> <span data-ttu-id="58599-133">También pasa los nombres de clase personalizada tres que aún no ha implementado.</span><span class="sxs-lookup"><span data-stu-id="58599-133">You also pass the names of three customized class which you have not implemented yet.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample1.cs?highlight=1-2)]

<span data-ttu-id="58599-134">Ha cambiado el tipo de la clave, pero, de forma predeterminada, el resto de la aplicación todavía se da por supuesto que la clave es una cadena.</span><span class="sxs-lookup"><span data-stu-id="58599-134">You have changed the type of the key, but, by default, the rest of the application still assumes the key is a string.</span></span> <span data-ttu-id="58599-135">Debe indicar explícitamente el tipo de la clave en el código que supone una cadena.</span><span class="sxs-lookup"><span data-stu-id="58599-135">You must explicitly indicate the type of the key in code that assumes a string.</span></span>

<span data-ttu-id="58599-136">En el **ApplicationUser** clase, cambie el **GenerateUserIdentityAsync** método incluir int, como se muestra en el código que aparece resaltado a continuación.</span><span class="sxs-lookup"><span data-stu-id="58599-136">In the **ApplicationUser** class, change the **GenerateUserIdentityAsync** method to include int, as shown in the highlighted code below.</span></span> <span data-ttu-id="58599-137">Este cambio no es necesario para los proyectos de formularios Web Forms con la plantilla de Update 3.</span><span class="sxs-lookup"><span data-stu-id="58599-137">This change is not necessary for Web Forms projects with the Update 3 template.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample2.cs?highlight=2)]

<a id="customclass"></a>
## <a name="add-customized-identity-classes-that-use-the-key-type"></a><span data-ttu-id="58599-138">Agregar clases de identidad personalizadas que utilizan el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-138">Add customized Identity classes that use the key type</span></span>

<span data-ttu-id="58599-139">Las demás clases de identidad, como IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, todavía se configuran para usar una clave de cadena.</span><span class="sxs-lookup"><span data-stu-id="58599-139">The other Identity classes, such as IdentityUserRole, IdentityUserClaim, IdentityUserLogin, IdentityRole, UserStore, RoleStore, are still set up to use a string key.</span></span> <span data-ttu-id="58599-140">Crear nuevas versiones de estas clases que especifican un número entero para la clave.</span><span class="sxs-lookup"><span data-stu-id="58599-140">Create new versions of these classes that specify an integer for the key.</span></span> <span data-ttu-id="58599-141">No es necesario proporcionar el código de implementación de estas clases, principalmente simplemente se establecer int como clave.</span><span class="sxs-lookup"><span data-stu-id="58599-141">You do not need to provide much implementation code in these classes, you are primarily just setting int as the key.</span></span>

<span data-ttu-id="58599-142">Agregue las siguientes clases en el archivo IdentityModels.cs.</span><span class="sxs-lookup"><span data-stu-id="58599-142">Add the following classes to your IdentityModels.cs file.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample3.cs)]

<a id="context"></a>
## <a name="change-the-context-class-and-user-manager-to-use-the-key-type"></a><span data-ttu-id="58599-143">Cambiar el Administrador de usuario y la clase de contexto para usar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-143">Change the context class and user manager to use the key type</span></span>

<span data-ttu-id="58599-144">En IdentityModels.cs, cambie la definición de la **ApplicationDbContext** clase se debe utilizar la nueva personalizar clases y un **int** de la clave, como se muestra en el código que aparece resaltado.</span><span class="sxs-lookup"><span data-stu-id="58599-144">In IdentityModels.cs, change the definition of the **ApplicationDbContext** class to use your new customized classes and an **int** for the key, as shown in the highlighted code.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample4.cs?highlight=1-2)]

<span data-ttu-id="58599-145">El parámetro ThrowIfV1Schema ya no es válido en el constructor.</span><span class="sxs-lookup"><span data-stu-id="58599-145">The ThrowIfV1Schema parameter is no longer valid in the constructor.</span></span> <span data-ttu-id="58599-146">Cambie el constructor, por lo que no pasa un valor de ThrowIfV1Schema.</span><span class="sxs-lookup"><span data-stu-id="58599-146">Change the constructor so it does not pass a ThrowIfV1Schema value.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample5.cs)]

<span data-ttu-id="58599-147">Abra IdentityConfig.cs y cambie el **ApplicationUserManger** clase para usar el nuevo usuario de almacén de clase para conservar los datos y un **int** para la clave.</span><span class="sxs-lookup"><span data-stu-id="58599-147">Open IdentityConfig.cs, and change the **ApplicationUserManger** class to use your new user store class for persisting data and an **int** for the key.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample6.cs?highlight=1,3,12,14,32,37,48)]

<span data-ttu-id="58599-148">En la plantilla de Update 3, debe cambiar la clase ApplicationSignInManager.</span><span class="sxs-lookup"><span data-stu-id="58599-148">In the Update 3 template, you must change the ApplicationSignInManager class.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample7.cs?highlight=1)]

<a id="startup"></a>
## <a name="change-start-up-configuration-to-use-the-key-type"></a><span data-ttu-id="58599-149">Cambiar la configuración de inicio para usar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-149">Change start-up configuration to use the key type</span></span>

<span data-ttu-id="58599-150">En Startup.Auth.cs, reemplace el código de OnValidateIdentity, como se indica a continuación.</span><span class="sxs-lookup"><span data-stu-id="58599-150">In Startup.Auth.cs, replace the OnValidateIdentity code, as highlighted below.</span></span> <span data-ttu-id="58599-151">Tenga en cuenta que la definición de getUserIdCallback, analiza el valor de cadena en un entero.</span><span class="sxs-lookup"><span data-stu-id="58599-151">Notice that the getUserIdCallback definition, parses the string value into an integer.</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample8.cs?highlight=7-12)]

<span data-ttu-id="58599-152">Si el proyecto no reconoce la implementación genérica de la **GetUserId** método, puede que necesite actualizar el paquete NuGet de identidad de ASP.NET a la versión 2.1</span><span class="sxs-lookup"><span data-stu-id="58599-152">If your project does not recognize the generic implementation of the **GetUserId** method, you may need to update the ASP.NET Identity NuGet package to version 2.1</span></span>

<span data-ttu-id="58599-153">Muchos de los cambios realizados en las clases de infraestructura usa ASP.NET Identity.</span><span class="sxs-lookup"><span data-stu-id="58599-153">You have made a lot of changes to the infrastructure classes used by ASP.NET Identity.</span></span> <span data-ttu-id="58599-154">Si intenta compilar el proyecto, observará una gran cantidad de errores.</span><span class="sxs-lookup"><span data-stu-id="58599-154">If you try compiling the project, you will notice a lot of errors.</span></span> <span data-ttu-id="58599-155">Afortunadamente, los errores restantes son similares.</span><span class="sxs-lookup"><span data-stu-id="58599-155">Fortunately, the remaining errors are all similar.</span></span> <span data-ttu-id="58599-156">La clase de identidad espera un número entero para la clave, pero el controlador (o un formulario Web Forms) está pasando un valor de cadena.</span><span class="sxs-lookup"><span data-stu-id="58599-156">The Identity class expects an integer for the key, but the controller (or Web Form) is passing a string value.</span></span> <span data-ttu-id="58599-157">En cada caso, debe convertir de una cadena a y el entero mediante una llamada a **GetUserId&lt;int&gt;**.</span><span class="sxs-lookup"><span data-stu-id="58599-157">In each case, you need to convert from a string to and integer by calling **GetUserId&lt;int&gt;**.</span></span> <span data-ttu-id="58599-158">Puede trabajar con la lista de errores de compilación o seguir los cambios a continuación.</span><span class="sxs-lookup"><span data-stu-id="58599-158">You can either work through the error list from compilation or follow the changes below.</span></span>

<span data-ttu-id="58599-159">Los cambios restantes dependen del tipo de proyecto que está creando y actualización que ha instalado en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="58599-159">The remaining changes depend on the type of project you are creating and which update you have installed in Visual Studio.</span></span> <span data-ttu-id="58599-160">Puede ir directamente a la sección correspondiente a través de los siguientes vínculos</span><span class="sxs-lookup"><span data-stu-id="58599-160">You can go directly to the relevant section through the following links</span></span>

- [<span data-ttu-id="58599-161">En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-161">For MVC with Update 2, change the AccountController to pass the key type</span></span>](#mvcupdate2)
- [<span data-ttu-id="58599-162">Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-162">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>](#mvcupdate3)
- [<span data-ttu-id="58599-163">Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-163">For Web Forms with Update 2, change Account pages to pass the key type</span></span>](#webformsupdate2)
- [<span data-ttu-id="58599-164">Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-164">For Web Forms with Update 3, change Account pages to pass the key type</span></span>](#webformsupdate3)

<a id="mvcupdate2"></a>
## <a name="for-mvc-with-update-2-change-the-accountcontroller-to-pass-the-key-type"></a><span data-ttu-id="58599-165">En el caso de MVC con Update 2, cambiar el AccountController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-165">For MVC with Update 2, change the AccountController to pass the key type</span></span>

<span data-ttu-id="58599-166">Abra el archivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="58599-166">Open the AccountController.cs file.</span></span> <span data-ttu-id="58599-167">Debe cambiar los métodos siguientes.</span><span class="sxs-lookup"><span data-stu-id="58599-167">You need to change the following methods.</span></span>

<span data-ttu-id="58599-168">**ConfirmEmail** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-168">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample9.cs?highlight=1,3)]

<span data-ttu-id="58599-169">**Desasociar** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-169">**Disassociate** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample10.cs?highlight=5,9)]

<span data-ttu-id="58599-170">**Manage(ManageUserViewModel)** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-170">**Manage(ManageUserViewModel)** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample11.cs?highlight=11,17,41)]

<span data-ttu-id="58599-171">**LinkLoginCallback** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-171">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample12.cs?highlight=10)]

<span data-ttu-id="58599-172">**RemoveAccountList** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-172">**RemoveAccountList** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample13.cs?highlight=3)]

<span data-ttu-id="58599-173">**HasPassword** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-173">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample14.cs?highlight=3)]

<span data-ttu-id="58599-174">Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-174">You can now [run the application](#run) and register a new user.</span></span>

<a id="mvcupdate3"></a>
## <a name="for-mvc-with-update-3-change-the-accountcontroller-and-managecontroller-to-pass-the-key-type"></a><span data-ttu-id="58599-175">Para MVC con Update 3, cambiar el AccountController y ManageController para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-175">For MVC with Update 3, change the AccountController and ManageController to pass the key type</span></span>

<span data-ttu-id="58599-176">Abra el archivo AccountController.cs.</span><span class="sxs-lookup"><span data-stu-id="58599-176">Open the AccountController.cs file.</span></span> <span data-ttu-id="58599-177">Debe cambiar el método siguiente.</span><span class="sxs-lookup"><span data-stu-id="58599-177">You need to change the following method.</span></span>

<span data-ttu-id="58599-178">**ConfirmEmail** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-178">**ConfirmEmail** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample15.cs?highlight=1,3)]

<span data-ttu-id="58599-179">**SendCode** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-179">**SendCode** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample16.cs?highlight=4)]

<span data-ttu-id="58599-180">Abra el archivo ManageController.cs.</span><span class="sxs-lookup"><span data-stu-id="58599-180">Open the ManageController.cs file.</span></span> <span data-ttu-id="58599-181">Debe cambiar los métodos siguientes.</span><span class="sxs-lookup"><span data-stu-id="58599-181">You need to change the following methods.</span></span>

<span data-ttu-id="58599-182">**Índice** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-182">**Index** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample17.cs?highlight=15-17)]

<span data-ttu-id="58599-183">**RemoveLogin** métodos</span><span class="sxs-lookup"><span data-stu-id="58599-183">**RemoveLogin** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample18.cs?highlight=3,13,17)]

<span data-ttu-id="58599-184">**AddPhoneNumber** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-184">**AddPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample19.cs?highlight=9)]

<span data-ttu-id="58599-185">**EnableTwoFactorAuthentication** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-185">**EnableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample20.cs?highlight=3-4)]

<span data-ttu-id="58599-186">**DisableTwoFactorAuthentication** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-186">**DisableTwoFactorAuthentication** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample21.cs?highlight=3-4)]

<span data-ttu-id="58599-187">**VerifyPhoneNumber** métodos</span><span class="sxs-lookup"><span data-stu-id="58599-187">**VerifyPhoneNumber** methods</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample22.cs?highlight=4,18,21)]

<span data-ttu-id="58599-188">**RemovePhoneNumber** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-188">**RemovePhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample23.cs?highlight=3,8)]

<span data-ttu-id="58599-189">**ChangePassword** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-189">**ChangePassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample24.cs?highlight=10,13)]

<span data-ttu-id="58599-190">**SetPassword** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-190">**SetPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample25.cs?highlight=5,8)]

<span data-ttu-id="58599-191">**ManageLogins** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-191">**ManageLogins** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample26.cs?highlight=7,12)]

<span data-ttu-id="58599-192">**LinkLoginCallback** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-192">**LinkLoginCallback** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample27.cs?highlight=8)]

<span data-ttu-id="58599-193">**HasPassword** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-193">**HasPassword** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample28.cs?highlight=3)]

<span data-ttu-id="58599-194">**HasPhoneNumber** (método)</span><span class="sxs-lookup"><span data-stu-id="58599-194">**HasPhoneNumber** method</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample29.cs?highlight=3)]

<span data-ttu-id="58599-195">Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-195">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate2"></a>
## <a name="for-web-forms-with-update-2-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="58599-196">Para los formularios Web con la actualización 2, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-196">For Web Forms with Update 2, change Account pages to pass the key type</span></span>

<span data-ttu-id="58599-197">Para los formularios Web con la actualización 2, debe cambiar las páginas siguientes.</span><span class="sxs-lookup"><span data-stu-id="58599-197">For Web Forms with Update 2, you need to change the following pages.</span></span>

<span data-ttu-id="58599-198">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="58599-198">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample30.cs?highlight=8)]

<span data-ttu-id="58599-199">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-199">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample31.cs?highlight=36)]

<span data-ttu-id="58599-200">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-200">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample32.cs?highlight=3,22,47,52,69,85,93,98)]

<span data-ttu-id="58599-201">Ahora puede [ejecutar la aplicación](#run) y registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-201">You can now [run the application](#run) and register a new user.</span></span>

<a id="webformsupdate3"></a>
## <a name="for-web-forms-with-update-3-change-account-pages-to-pass-the-key-type"></a><span data-ttu-id="58599-202">Para los formularios Web con Update 3, cambiar las páginas de la cuenta para pasar el tipo de clave</span><span class="sxs-lookup"><span data-stu-id="58599-202">For Web Forms with Update 3, change Account pages to pass the key type</span></span>

<span data-ttu-id="58599-203">Para los formularios Web con Update 3, debe cambiar las páginas siguientes.</span><span class="sxs-lookup"><span data-stu-id="58599-203">For Web Forms with Update 3, you need to change the following pages.</span></span>

<span data-ttu-id="58599-204">**Confirm.aspx.CX**</span><span class="sxs-lookup"><span data-stu-id="58599-204">**Confirm.aspx.cx**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample33.cs?highlight=8)]

<span data-ttu-id="58599-205">**RegisterExternalLogin.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-205">**RegisterExternalLogin.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample34.cs?highlight=36)]

<span data-ttu-id="58599-206">**Manage.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-206">**Manage.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample35.cs?highlight=11,27,32,34,82,87,99,108)]

<span data-ttu-id="58599-207">**VerifyPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-207">**VerifyPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample36.cs?highlight=8,23,27)]

<span data-ttu-id="58599-208">**AddPhoneNumber.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-208">**AddPhoneNumber.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample37.cs?highlight=7)]

<span data-ttu-id="58599-209">**ManagePassword.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-209">**ManagePassword.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample38.cs?highlight=11,47,50,68)]

<span data-ttu-id="58599-210">**ManageLogins.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-210">**ManageLogins.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample39.cs?highlight=16,23,32,41,45)]

<span data-ttu-id="58599-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span><span class="sxs-lookup"><span data-stu-id="58599-211">**TwoFactorAuthenticationSignIn.aspx.cs**</span></span>

[!code-csharp[Main](change-primary-key-for-users-in-aspnet-identity/samples/sample40.cs?highlight=14-15,29,53)]

<a id="run"></a>
## <a name="run-application"></a><span data-ttu-id="58599-212">Ejecutar la aplicación</span><span class="sxs-lookup"><span data-stu-id="58599-212">Run application</span></span>

<span data-ttu-id="58599-213">Haya finalizado todos los cambios necesarios en la plantilla de aplicación Web predeterminada.</span><span class="sxs-lookup"><span data-stu-id="58599-213">You have finished all of the required changes to the default Web Application template.</span></span> <span data-ttu-id="58599-214">Ejecute la aplicación y registrar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-214">Run the application and register a new user.</span></span> <span data-ttu-id="58599-215">Después de registrar el usuario, observará que la tabla AspNetUsers tiene una columna de identificador que es un entero.</span><span class="sxs-lookup"><span data-stu-id="58599-215">After registering the user, you will notice that the AspNetUsers table has an Id column that is an integer.</span></span>

![nueva clave principal](change-primary-key-for-users-in-aspnet-identity/_static/image1.png)

<span data-ttu-id="58599-217">Si previamente ha creado la identidad de ASP.NET tablas con una clave principal distinta, debe realizar algunos cambios adicionales.</span><span class="sxs-lookup"><span data-stu-id="58599-217">If you have previously created the ASP.NET Identity tables with a different primary key, you need to make some additional changes.</span></span> <span data-ttu-id="58599-218">Si es posible, elimine la base de datos existente.</span><span class="sxs-lookup"><span data-stu-id="58599-218">If possible, just delete the existing database.</span></span> <span data-ttu-id="58599-219">La base de datos volverá a crear con el diseño correcto cuando se ejecuta la aplicación web y agregar un nuevo usuario.</span><span class="sxs-lookup"><span data-stu-id="58599-219">The database will be re-created with the correct design when you run the web application and add a new user.</span></span> <span data-ttu-id="58599-220">Si la eliminación no es posible ejecutar migraciones de code first para cambiar las tablas.</span><span class="sxs-lookup"><span data-stu-id="58599-220">If deletion is not possible, run code first migrations to change the tables.</span></span> <span data-ttu-id="58599-221">Sin embargo, la nueva clave principal de entero no se configurará como una propiedad de identidad de SQL en la base de datos.</span><span class="sxs-lookup"><span data-stu-id="58599-221">However, the new integer primary key will not be set up as a SQL IDENTITY property in the database.</span></span> <span data-ttu-id="58599-222">Debe establecer manualmente la columna de identificador como una identidad.</span><span class="sxs-lookup"><span data-stu-id="58599-222">You must manually set the Id column as an IDENTITY.</span></span>

<a id="other"></a>
## <a name="other-resources"></a><span data-ttu-id="58599-223">Otros recursos</span><span class="sxs-lookup"><span data-stu-id="58599-223">Other resources</span></span>

- [<span data-ttu-id="58599-224">Información general sobre los proveedores de almacenamiento personalizado para ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="58599-224">Overview of Custom Storage Providers for ASP.NET Identity</span></span>](overview-of-custom-storage-providers-for-aspnet-identity.md)
- [<span data-ttu-id="58599-225">Migrar un sitio web existente desde la pertenencia de SQL a ASP.NET Identity</span><span class="sxs-lookup"><span data-stu-id="58599-225">Migrating an Existing Website from SQL Membership to ASP.NET Identity</span></span>](../migrations/migrating-an-existing-website-from-sql-membership-to-aspnet-identity.md)
- [<span data-ttu-id="58599-226">Migrar datos de proveedor Universal para la suscripción y perfiles de usuario para la identidad de ASP.NET</span><span class="sxs-lookup"><span data-stu-id="58599-226">Migrating Universal Provider Data for Membership and User Profiles to ASP.NET Identity</span></span>](../migrations/migrating-universal-provider-data-for-membership-and-user-profiles-to-aspnet-identity.md)
- <span data-ttu-id="58599-227">[Aplicación de ejemplo](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) con la clave principal cambiado</span><span class="sxs-lookup"><span data-stu-id="58599-227">[Sample application](https://aspnet.codeplex.com/SourceControl/latest#Samples/Identity/ChangePK/readme.txt) with changed primary key</span></span>
