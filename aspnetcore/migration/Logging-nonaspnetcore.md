---
title: Migrar de Microsoft.Extensions.Logging 2.1 a 2.2 o 3.0
author: pakrym
description: Obtenga información sobre cómo migrar una aplicación que no son de ASP.NET Core que usa Microsoft.Extensions.Logging desde 2.1 a 2.2 o 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099475"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="a708c-103">Migrar de Microsoft.Extensions.Logging 2.1 a 2.2 o 3.0</span><span class="sxs-lookup"><span data-stu-id="a708c-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="a708c-104">En este artículo se describe los pasos comunes para migrar una aplicación que no son de ASP.NET Core que usa `Microsoft.Extensions.Logging` 2.1 a 2.2 o 3.0.</span><span class="sxs-lookup"><span data-stu-id="a708c-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="a708c-105">2.1 a 2.2</span><span class="sxs-lookup"><span data-stu-id="a708c-105">2.1 to 2.2</span></span>

<span data-ttu-id="a708c-106">Crear manualmente `ServiceCollection` y llamar a `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="a708c-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="a708c-107">ejemplo 2.1:</span><span class="sxs-lookup"><span data-stu-id="a708c-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="a708c-108">ejemplo 2.2:</span><span class="sxs-lookup"><span data-stu-id="a708c-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="a708c-109">2.1 a 3.0</span><span class="sxs-lookup"><span data-stu-id="a708c-109">2.1 to 3.0</span></span>

<span data-ttu-id="a708c-110">En 3.0, utilice `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="a708c-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="a708c-111">ejemplo 2.1:</span><span class="sxs-lookup"><span data-stu-id="a708c-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="a708c-112">ejemplo de 3.0:</span><span class="sxs-lookup"><span data-stu-id="a708c-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="a708c-113">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a708c-113">Additional resources</span></span>

<xref:fundamentals/logging/index>