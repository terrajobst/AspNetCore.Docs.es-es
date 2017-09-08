---
title: Configurar el tipo de datos de las claves principales de identidad
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="3ebd5-102">Configurar el tipo de datos de las claves principales de identidad</span><span class="sxs-lookup"><span data-stu-id="3ebd5-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="3ebd5-103">Identidad de ASP.NET Core le permite configurar fácilmente el tipo de datos que desee para las claves principales.</span><span class="sxs-lookup"><span data-stu-id="3ebd5-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="3ebd5-104">De forma predeterminada, tipo de datos de cadena de usos de identidad pero muy rápidamente puede invalidar este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="3ebd5-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="3ebd5-105">Cómo</span><span class="sxs-lookup"><span data-stu-id="3ebd5-105">How to</span></span>

1.  <span data-ttu-id="3ebd5-106">El primer paso es implementar el modelo de identidad y reemplazar el tipo de cadena con el tipo de datos que desee.</span><span class="sxs-lookup"><span data-stu-id="3ebd5-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="3ebd5-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="3ebd5-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="3ebd5-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="3ebd5-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="3ebd5-109">Implementar el contexto de base de datos de identidad con los modelos y el tipo de datos que desee para las claves principales</span><span class="sxs-lookup"><span data-stu-id="3ebd5-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="3ebd5-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="3ebd5-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="3ebd5-111">Use los modelos y el tipo de datos que desee para las claves principales cuando se declara el servicio de identidad en la clase de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="3ebd5-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="3ebd5-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="3ebd5-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
