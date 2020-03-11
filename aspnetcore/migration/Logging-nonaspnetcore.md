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
# <a name="migrate-from-microsoftextensionslogging-21-to-22-or-30"></a><span data-ttu-id="f3984-103">Migre desde Microsoft. Extensions. Logging 2,1 a 2,2 o 3,0</span><span class="sxs-lookup"><span data-stu-id="f3984-103">Migrate from Microsoft.Extensions.Logging 2.1 to 2.2 or 3.0</span></span>

<span data-ttu-id="f3984-104">En este artículo se describen los pasos comunes para migrar una aplicación non-ASP.NET Core que usa `Microsoft.Extensions.Logging` de 2,1 a 2,2 o 3,0.</span><span class="sxs-lookup"><span data-stu-id="f3984-104">This article outlines the common steps for migrating a non-ASP.NET Core application that uses `Microsoft.Extensions.Logging` from 2.1 to 2.2 or 3.0.</span></span>

## <a name="21-to-22"></a><span data-ttu-id="f3984-105">2.1 a 2.2</span><span class="sxs-lookup"><span data-stu-id="f3984-105">2.1 to 2.2</span></span>

<span data-ttu-id="f3984-106">Cree manualmente `ServiceCollection` y llame a `AddLogging`.</span><span class="sxs-lookup"><span data-stu-id="f3984-106">Manually create `ServiceCollection` and call `AddLogging`.</span></span>

<span data-ttu-id="f3984-107">ejemplo 2,1:</span><span class="sxs-lookup"><span data-stu-id="f3984-107">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="f3984-108">ejemplo 2,2:</span><span class="sxs-lookup"><span data-stu-id="f3984-108">2.2 example:</span></span>

```csharp
var serviceCollection = new ServiceCollection();
serviceCollection.AddLogging(builder => builder.AddConsole());

using (var serviceProvider = serviceCollection.BuildServiceProvider())
using (var loggerFactory = serviceProvider.GetService<ILoggerFactory>())
{
    // use loggerFactory
}
```

## <a name="21-to-30"></a><span data-ttu-id="f3984-109">de 2,1 a 3,0</span><span class="sxs-lookup"><span data-stu-id="f3984-109">2.1 to 3.0</span></span>

<span data-ttu-id="f3984-110">En 3,0, use `LoggingFactory.Create`.</span><span class="sxs-lookup"><span data-stu-id="f3984-110">In 3.0, use `LoggingFactory.Create`.</span></span>

<span data-ttu-id="f3984-111">ejemplo 2,1:</span><span class="sxs-lookup"><span data-stu-id="f3984-111">2.1 example:</span></span>

```csharp
using (var loggerFactory = new LoggerFactory())
{
    loggerFactory.AddConsole();

    // use loggerFactory
}
```

<span data-ttu-id="f3984-112">ejemplo 3,0:</span><span class="sxs-lookup"><span data-stu-id="f3984-112">3.0 example:</span></span>

```csharp
using (var loggerFactory = LoggerFactory.Create(builder => builder.AddConsole()))
{
    // use loggerFactory
}
```

## <a name="additional-resources"></a><span data-ttu-id="f3984-113">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="f3984-113">Additional resources</span></span>

<xref:fundamentals/logging/index>