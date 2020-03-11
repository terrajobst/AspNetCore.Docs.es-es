---
title: Migre desde Microsoft. Extensions. Logging 2,1 a 2,2 o 3,0
author: pakrym
description: Obtenga información sobre cómo migrar una aplicación de non-ASP.NET Core que usa Microsoft. Extensions. Logging de 2,1 a 2,2 o 3,0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651881"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a>Migre desde Microsoft. Extensions. Logging 2,1 a 2,2 o 3,0

En este artículo se describen los pasos comunes para migrar una aplicación non-ASP.NET Core que usa `Microsoft.Extensions.Logging` de 2,1 a 2,2 o 3,0.

## <a name="21-to-22"></a>2.1 a 2.2

Cree manualmente `ServiceCollection` y llame a `AddLogging`.

ejemplo 2,1:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

ejemplo 2,2:

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a>de 2,1 a 3,0

En 3,0, use `LoggingFactory.Create`.

ejemplo 2,1:

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

ejemplo 3,0:

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a>Recursos adicionales

<xref:fundamentals/logging/index>