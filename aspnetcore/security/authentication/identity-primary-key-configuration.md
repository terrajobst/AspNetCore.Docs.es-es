---
title: Configurar el tipo de datos de las claves principales de identidad
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Configurar el tipo de datos de las claves principales de identidad

Identidad de ASP.NET Core le permite configurar fácilmente el tipo de datos que desee para las claves principales. De forma predeterminada, tipo de datos de cadena de usos de identidad pero muy rápidamente puede invalidar este comportamiento.

## <a name="how-to"></a>Cómo

1.  El primer paso es implementar el modelo de identidad y reemplazar el tipo de cadena con el tipo de datos que desee.

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Implementar el contexto de base de datos de identidad con los modelos y el tipo de datos que desee para las claves principales

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  Use los modelos y el tipo de datos que desee para las claves principales cuando se declara el servicio de identidad en la clase de inicio de la aplicación

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
