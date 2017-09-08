---
title: Configurar la identidad
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="8cc43-102">Configurar la identidad</span><span class="sxs-lookup"><span data-stu-id="8cc43-102">Configure Identity</span></span>

<span data-ttu-id="8cc43-103">Identidad de ASP.NET Core tiene algunos comportamientos predeterminados que se pueden reemplazar fácilmente en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8cc43-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="8cc43-104">Directiva de contraseñas</span><span class="sxs-lookup"><span data-stu-id="8cc43-104">Passwords policy</span></span>

<span data-ttu-id="8cc43-105">De forma predeterminada, identidad requiere que las contraseñas contengan un carácter en mayúsculas, caracteres en minúsculas y dígitos.</span><span class="sxs-lookup"><span data-stu-id="8cc43-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="8cc43-106">También hay algunas otras restricciones.</span><span class="sxs-lookup"><span data-stu-id="8cc43-106">There are also some other restrictions.</span></span> <span data-ttu-id="8cc43-107">Si desea simplificar las restricciones de contraseña, puede hacerlo en la clase de inicio de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="8cc43-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="8cc43-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="8cc43-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="8cc43-109">Configuración de cookies de la aplicación</span><span class="sxs-lookup"><span data-stu-id="8cc43-109">Application's cookie settings</span></span>

<span data-ttu-id="8cc43-110">Al igual que la directiva de contraseñas, todos los valores de cookie de la aplicación pueden cambiarse en la clase de inicio.</span><span class="sxs-lookup"><span data-stu-id="8cc43-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="8cc43-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="8cc43-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="8cc43-112">Bloqueo del usuario</span><span class="sxs-lookup"><span data-stu-id="8cc43-112">User's lockout</span></span>

<span data-ttu-id="8cc43-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="8cc43-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
