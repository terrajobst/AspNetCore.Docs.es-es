---
title: Migrar de Microsoft.Extensions.Logging 2.1 a 2.2 o 3.0
author: pakrym
description: Obtenga información sobre cómo migrar una aplicación que no son de ASP.NET Core que usa Microsoft.Extensions.Logging desde 2.1 a 2.2 o 3.0.
ms.author: pakrym
ms.custom: mvc
ms.date: 01/04/2019
uid: migration/logging-nonaspnetcore
ms.openlocfilehash: 2519ddc02cee5978483bcaef4341a52aad3ba2a6
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892462"
---
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="20535-103">Migrar de Microsoft.Extensions.Logging 2.1 a 2.2 o 3.0</span><span class="sxs-lookup"><span data-stu-id="20535-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="20535-104">En este artículo se describe los pasos comunes para migrar una aplicación que no son de ASP.NET Core que usa `Microsoft.Extensions.Logging` 2.1 a 2.2 o 3.0.</span><span class="sxs-lookup"><span data-stu-id="20535-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="20535-105">2.1 a 2.2</span><span class="sxs-lookup"><span data-stu-id="20535-105">2.1 to 2.2</span></span>

<span data-ttu-id="20535-106">Crear manualmente `ServiceCollection` y llamar a `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="20535-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="20535-107">ejemplo 2.1:</span><span class="sxs-lookup"><span data-stu-id="20535-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="20535-108">ejemplo 2.2:</span><span class="sxs-lookup"><span data-stu-id="20535-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="20535-109">2.1 a 3.0</span><span class="sxs-lookup"><span data-stu-id="20535-109">2.1 to 3.0</span></span>

<span data-ttu-id="20535-110">En 3.0, utilice `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="20535-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="20535-111">ejemplo 2.1:</span><span class="sxs-lookup"><span data-stu-id="20535-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="20535-112">ejemplo de 3.0:</span><span class="sxs-lookup"><span data-stu-id="20535-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="20535-113">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="20535-113">Additional resources</span></span>

<xref:fundamentals/logging/index>