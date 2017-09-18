---
title: Configurar la identidad de ASP.NET Core
author: AdrienTorris
description: Comprender los valores predeterminados de ASP.NET Core Identity y configurar las distintas propiedades de identidad para usar valores personalizados.
keywords: "Autenticación de ASP.NET Core, identidad, seguridad"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2017
---
# <a name="configure-identity"></a><span data-ttu-id="5e835-104">Configurar la identidad</span><span class="sxs-lookup"><span data-stu-id="5e835-104">Configure Identity</span></span>

<span data-ttu-id="5e835-105">Identidad de ASP.NET Core tiene algunos comportamientos predeterminados que se pueden reemplazar fácilmente en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e835-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="5e835-106">Directiva de contraseñas</span><span class="sxs-lookup"><span data-stu-id="5e835-106">Passwords policy</span></span>

<span data-ttu-id="5e835-107">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas y dígitos.</span><span class="sxs-lookup"><span data-stu-id="5e835-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="5e835-108">También hay algunas otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="5e835-108">There are also some other restrictions.</span></span> <span data-ttu-id="5e835-109">Si desea simplificar las restricciones de contraseña, puede hacerlo en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="5e835-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="5e835-110">Configuración de cookies de la aplicación</span><span class="sxs-lookup"><span data-stu-id="5e835-110">Application's cookie settings</span></span>

<span data-ttu-id="5e835-111">Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en la clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="5e835-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="5e835-112">Bloqueo del usuario</span><span class="sxs-lookup"><span data-stu-id="5e835-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
