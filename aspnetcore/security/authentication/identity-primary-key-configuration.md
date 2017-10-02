---
title: Configurar el tipo de datos de clave principal de identidad
author: AdrienTorris
description: "En este artículo se describe los pasos para configurar el tipo de datos que desee utilizado para la clave principal de ASP.NET Core Identity."
keywords: Clave principal de ASP.NET Core, identidad,
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 13291956ad858d465dfd46caa384df58f08e286b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/29/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="63776-104">Configurar el tipo de datos de clave principal de ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="63776-104">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="63776-105">Identidad de ASP.NET Core le permite configurar el tipo de datos que se utiliza para representar una clave principal.</span><span class="sxs-lookup"><span data-stu-id="63776-105">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="63776-106">Identidad utiliza el `string` tipo de datos de forma predeterminada.</span><span class="sxs-lookup"><span data-stu-id="63776-106">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="63776-107">Puede invalidar este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="63776-107">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="63776-108">Personalizar el tipo de datos de clave principal</span><span class="sxs-lookup"><span data-stu-id="63776-108">Customize the primary key data type</span></span>

1. <span data-ttu-id="63776-109">Crear una implementación personalizada de la [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) clase.</span><span class="sxs-lookup"><span data-stu-id="63776-109">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="63776-110">Representa el tipo que se usará para crear objetos de usuario.</span><span class="sxs-lookup"><span data-stu-id="63776-110">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="63776-111">En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="63776-111">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="63776-112">Crear una implementación personalizada de la [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) clase.</span><span class="sxs-lookup"><span data-stu-id="63776-112">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="63776-113">Representa el tipo que se usará para crear objetos de rol.</span><span class="sxs-lookup"><span data-stu-id="63776-113">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="63776-114">En el ejemplo siguiente, el valor predeterminado `string` tipo se reemplaza con `Guid`.</span><span class="sxs-lookup"><span data-stu-id="63776-114">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="63776-115">Cree una clase de contexto de base de datos personalizada.</span><span class="sxs-lookup"><span data-stu-id="63776-115">Create a custom database context class.</span></span> <span data-ttu-id="63776-116">Hereda de la clase de contexto de base de datos de Entity Framework usada para la identidad.</span><span class="sxs-lookup"><span data-stu-id="63776-116">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="63776-117">El `TUser` y `TRole` argumentos hacen referencia a las clases de usuario y el rol personalizadas creadas en el paso anterior, respectivamente.</span><span class="sxs-lookup"><span data-stu-id="63776-117">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="63776-118">El `Guid` se define el tipo de datos para la clave principal.</span><span class="sxs-lookup"><span data-stu-id="63776-118">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="63776-119">Registre la clase de contexto de base de datos personalizada al agregar el servicio de identidad en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="63776-119">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="63776-120">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="63776-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="63776-121">El `AddEntityFrameworkStores` método no aceptar un `TKey` argumento tal como se hacía en ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="63776-121">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="63776-122">Tipo de datos de la clave principal se deduce mediante el análisis de la `DbContext` objeto.</span><span class="sxs-lookup"><span data-stu-id="63776-122">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="63776-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="63776-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="63776-124">El `AddEntityFrameworkStores` método acepta un `TKey` argumento que indica el tipo de datos de la clave principal.</span><span class="sxs-lookup"><span data-stu-id="63776-124">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="63776-125">Probar los cambios</span><span class="sxs-lookup"><span data-stu-id="63776-125">Test the changes</span></span>

<span data-ttu-id="63776-126">Tras la finalización de los cambios de configuración, la propiedad que representa la clave principal refleja el nuevo tipo de datos.</span><span class="sxs-lookup"><span data-stu-id="63776-126">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="63776-127">En el ejemplo siguiente se muestra cómo obtener acceso a la propiedad en un controlador MVC.</span><span class="sxs-lookup"><span data-stu-id="63776-127">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
