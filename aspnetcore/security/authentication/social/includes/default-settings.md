---
ms.openlocfilehash: 2fe12027e7a5233cf01e6c412f7ee536d479facd
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665778"
---
<!--Don't update this for 2.2, use the 2.2 version --> La llamada a [AddDefaultIdentity](/dotnet/api/microsoft.extensions.dependencyinjection.identityservicecollectionuiextensions.adddefaultidentity) configura las opciones de esquema predeterminadas. El [AddAuthentication(String)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_String_) conjuntos de sobrecarga el [DefaultScheme](/dotnet/api/microsoft.aspnetcore.authentication.authenticationoptions.defaultscheme) propiedad. El [AddAuthentication (acción&lt;AuthenticationOptions&gt;)](/dotnet/api/microsoft.extensions.dependencyinjection.authenticationservicecollectionextensions.addauthentication#Microsoft_Extensions_DependencyInjection_AuthenticationServiceCollectionExtensions_AddAuthentication_Microsoft_Extensions_DependencyInjection_IServiceCollection_System_Action_Microsoft_AspNetCore_Authentication_AuthenticationOptions__) sobrecarga permite configurar las opciones de autenticación, que se pueden usar para configurar los esquemas de autenticación predeterminado para distintos fines. Las llamadas subsiguientes a `AddAuthentication` invalidación configurado previamente [AuthenticationOptions](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions) propiedades.

[AuthenticationBuilder](/dotnet/api/microsoft.aspnetcore.authentication.authenticationbuilder) métodos de extensión que registra un controlador de autenticación sólo se llama una vez por cada esquema de autenticación. Existen sobrecargas que permiten configurar las propiedades del esquema, el nombre de esquema y nombre para mostrar.
