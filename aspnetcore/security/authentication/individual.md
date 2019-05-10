---
title: Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales
author: rick-anderson
description: Descubra artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales.
ms.author: riande
ms.date: 11/30/2017
uid: security/authentication/individual
ms.openlocfilehash: f9c1be16386da935382275815bb5fd5c72894b1c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892532"
---
# <a name="articles-based-on-aspnet-core-projects-created-with-individual-user-accounts"></a>Artículos basados en los proyectos de ASP.NET Core creados con cuentas de usuario individuales

ASP.NET Core Identity se incluye en las plantillas de proyecto en Visual Studio con la opción "Cuentas de usuario individuales".

Las plantillas de autenticación están disponibles en la CLI de .NET Core con `-au Individual`:

::: moniker range=">= aspnetcore-2.1"

```console
dotnet new mvc -au Individual
dotnet new webapp -au Individual
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```console
dotnet new mvc -au Individual
dotnet new razor -au Individual
```

::: moniker-end

Consulte [este problema de GitHub](https://github.com/aspnet/AspNetCore/issues/5833) para la autenticación de API web.

<a name="no"></a>

## <a name="no-authentication"></a>Sin autenticación

La autenticación se especifica en la CLI de .NET Core con el `-au` opción. En Visual Studio, el **Cambiar autenticación** cuadro de diálogo está disponible para nuevas aplicaciones web. El valor predeterminado para nuevas aplicaciones web en Visual Studio es **sin autenticación**.

Proyectos creados con ninguna autenticación:

* No contienen páginas web y la interfaz de usuario para iniciar sesión y cierre la sesión.
* No contienen código de autenticación.

<a name="win"></a>

## <a name="windows-authentication"></a>Autenticación de Windows

Se especifica la autenticación de Windows para las nuevas aplicaciones web en la CLI de .NET Core con el `-au Windows` opción. En Visual Studio, el **Cambiar autenticación** cuadro de diálogo proporciona el **Windows autenticación** opciones.

Si selecciona la autenticación de Windows, la aplicación está configurada para usar el [módulo de autenticación de Windows IIS](xref:host-and-deploy/iis/modules). Autenticación de Windows está diseñada para sitios web de Intranet.

## <a name="additional-resources"></a>Recursos adicionales

Los artículos siguientes muestran cómo usar el código generado en las plantillas de ASP.NET Core que usan cuentas de usuario individuales:

* [Autenticación en dos fases con SMS](xref:security/authentication/2fa)
* [Confirmación de las cuentas y recuperación de contraseñas en ASP.NET Core](xref:security/authentication/accconfirm)
* [Crear una aplicación ASP.NET Core con datos de usuario protegidos por autorización](xref:security/authorization/secure-data)
