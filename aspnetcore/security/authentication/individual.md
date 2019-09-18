---
title: Artículos basados en proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Descubra artículos basados en ASP.NET Core proyectos creados con cuentas de usuario individuales.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: cf548417268a8587787471b9ed91c0ed109fbee9
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 09/18/2019
ms.locfileid: "71080701"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artículos basados en proyectos de ASP.NET Core creados con cuentas de usuario individuales

ASP.NET Core identidad se incluye en las plantillas de proyecto de Visual Studio con la opción "cuentas de usuario individuales".

Las plantillas de autenticación están disponibles en CLI de .NET Core `-au Individual`con:

::: moniker range=">= aspnetcore-2.1"

```dotnetcli
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```dotnetcli
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Consulte [este problema de github para la](https://github.com/aspnet/AspNetCore/issues/5833) autenticación de API Web.

<a name="no"></a>

## <a name="no-authentication"></a>Sin autenticación

La autenticación se especifica en el CLI de .net Core con `-au` la opción. En Visual Studio, el cuadro de diálogo **cambiar autenticación** está disponible para las nuevas aplicaciones Web. El valor predeterminado para las nuevas aplicaciones web en Visual Studio **no es autenticación**.

Proyectos creados sin autenticación:

* No contenga páginas web y la interfaz de usuario para iniciar sesión y cerrar sesión.
* No contenga código de autenticación.

<a name="win"></a>

## <a name="windows-authentication"></a>Autenticación de Windows

La autenticación de Windows se especifica para las nuevas aplicaciones web en el `-au Windows` CLI de .net Core con la opción. En Visual Studio, el cuadro de diálogo **cambiar autenticación** proporciona las opciones de **autenticación de Windows** .

Si se selecciona autenticación de Windows, la aplicación se configura para usar el [módulo IIS de autenticación de Windows](xref:host-and-deploy/iis/modules). La autenticación de Windows está pensada para sitios web de la intranet.

## <a name="additional-resources"></a>Recursos adicionales

En los artículos siguientes se muestra cómo usar el código generado en ASP.NET Core plantillas que utilizan cuentas de usuario individuales:

* [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
* [Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core](xref:security/authentication/accconfirm)
* [Creación de una aplicación ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
