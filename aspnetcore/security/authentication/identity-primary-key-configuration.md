---
title: Configurar el tipo de datos de las claves principales de identidad
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="bf9e6-102">Configurar el tipo de datos de las claves principales de identidad</span><span class="sxs-lookup"><span data-stu-id="bf9e6-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="bf9e6-103">Identidad de ASP.NET Core le permite configurar fácilmente el tipo de datos que desee para las claves principales.</span><span class="sxs-lookup"><span data-stu-id="bf9e6-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="bf9e6-104">De forma predeterminada, tipo de datos de cadena de usos de identidad pero muy rápidamente puede invalidar este comportamiento.</span><span class="sxs-lookup"><span data-stu-id="bf9e6-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="bf9e6-105">Cómo</span><span class="sxs-lookup"><span data-stu-id="bf9e6-105">How to</span></span>

1.  <span data-ttu-id="bf9e6-106">El primer paso es implementar el modelo de identidad y reemplazar el tipo de cadena con el tipo de datos que desee.</span><span class="sxs-lookup"><span data-stu-id="bf9e6-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="bf9e6-107">Implementar el contexto de base de datos de identidad con los modelos y el tipo de datos que desee para las claves principales</span><span class="sxs-lookup"><span data-stu-id="bf9e6-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="bf9e6-108">Use los modelos y el tipo de datos que desee para las claves principales cuando se declara el servicio de identidad en la clase de inicio de la aplicación</span><span class="sxs-lookup"><span data-stu-id="bf9e6-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
