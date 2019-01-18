---
ms.openlocfilehash: a7be92adffc06ac0f25b84ea15d1a8c4896f4f9d
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/18/2019
ms.locfileid: "54397085"
---
<!--Don't update this for 2.2, use the 2.2 version --> La llamada a [AddIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) configura las opciones de esquema predeterminadas. El [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) conjuntos de sobrecarga el [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) propiedad. El [AddAuthentication (acción&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sobrecarga permite configurar las opciones de autenticación, que se pueden usar para configurar los esquemas de autenticación predeterminado para distintos fines. Las llamadas subsiguientes a `AddAuthentication` invalidación configurado previamente [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) propiedades.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) métodos de extensión que registra un controlador de autenticación sólo se llama una vez por cada esquema de autenticación. Existen sobrecargas que permiten configurar las propiedades del esquema, el nombre de esquema y nombre para mostrar.
