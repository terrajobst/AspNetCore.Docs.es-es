---
title: Configurar el tipo de datos de clave principal de identidad en ASP.NET Core
author: AdrienTorris
description: Obtenga información acerca de los pasos para configurar el tipo de datos que desee utilizado para la clave principal de ASP.NET Core Identity.
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274361"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="d8344-103">Configurar el tipo de datos de clave principal de identidad en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d8344-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="d8344-104">Identidad de ASP.NET Core le permite configurar el tipo de datos que se utiliza para representar una clave principal.</span><span class="sxs-lookup"><span data-stu-id="d8344-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="d8344-105">Identidad utiliza el `string` tipo de datos de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="d8344-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="d8344-106">Puede invalidar este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="d8344-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="d8344-107">Personalizar el tipo de datos de clave principal</span><span class="sxs-lookup"><span data-stu-id="d8344-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="d8344-108">Crear una implementación personalizada de la [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) clase.</span><span class="sxs-lookup"><span data-stu-id="d8344-108">Create a custom implementation of the [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="d8344-109">Representa el tipo que se usará para crear objetos de usuario.</span><span class="sxs-lookup"><span data-stu-id="d8344-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="d8344-110">En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="d8344-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="d8344-111">Crear una implementación personalizada de la [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) clase.</span><span class="sxs-lookup"><span data-stu-id="d8344-111">Create a custom implementation of the [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="d8344-112">Representa el tipo que se usará para crear objetos de rol.</span><span class="sxs-lookup"><span data-stu-id="d8344-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="d8344-113">En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="d8344-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="d8344-114">Cree una clase de contexto de base de datos personalizada.</span><span class="sxs-lookup"><span data-stu-id="d8344-114">Create a custom database context class.</span></span> <span data-ttu-id="d8344-115">Hereda de la clase de contexto de base de datos de Entity Framework usada para la identidad.</span><span class="sxs-lookup"><span data-stu-id="d8344-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="d8344-116">El `TUser` y `TRole` argumentos hacen referencia a las clases de usuario y el rol personalizadas creadas en el paso anterior, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="d8344-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="d8344-117">El `Guid` se define el tipo de datos para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="d8344-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="d8344-118">Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d8344-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d8344-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d8344-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   <span data-ttu-id="d8344-120">El `AddEntityFrameworkStores` método no aceptar un `TKey` argumento tal como se hacía en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d8344-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="d8344-121">Tipo de datos de la clave principal se deduce mediante el análisis de la `DbContext` objeto.</span><span class="sxs-lookup"><span data-stu-id="d8344-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d8344-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d8344-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   <span data-ttu-id="d8344-123">El `AddEntityFrameworkStores` método acepta un `TKey` argumento que indica el tipo de datos de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="d8344-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a><span data-ttu-id="d8344-124">Probar los cambios</span><span class="sxs-lookup"><span data-stu-id="d8344-124">Test the changes</span></span>

<span data-ttu-id="d8344-125">Tras la finalización de los cambios de configuración, la propiedad que representa la clave principal refleja el nuevo tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="d8344-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="d8344-126">En el ejemplo siguiente se muestra cómo obtener acceso a la propiedad en un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="d8344-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
