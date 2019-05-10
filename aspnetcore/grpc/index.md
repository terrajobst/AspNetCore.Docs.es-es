---
title: Introducción a gRPC en ASP.NET Core
author: juntaoluo
description: Obtenga información sobre los servicios gRPC con el servidor de Kestrel y la pila de ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085557"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a><span data-ttu-id="a3b23-103">Introducción a gRPC en ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a3b23-103">Introduction to gRPC on ASP.NET Core</span></span>

<span data-ttu-id="a3b23-104">Por [John Luo](https://github.com/juntaoluo)</span><span class="sxs-lookup"><span data-stu-id="a3b23-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="a3b23-105">[gRPC](https://grpc.io/docs/guides/) es un marco de llamada a procedimiento remoto (RPC) de alto rendimiento e independiente del idioma.</span><span class="sxs-lookup"><span data-stu-id="a3b23-105">[gRPC](https://grpc.io/docs/guides/) is a language agnostic, high-performance Remote Procedure Call (RPC) framework.</span></span> <span data-ttu-id="a3b23-106">Para obtener más información sobre los aspectos básicos de gRPC, vea la [página de documentación de gRPC](https://grpc.io/docs/).</span><span class="sxs-lookup"><span data-stu-id="a3b23-106">For more on gRPC fundamentals, see the [gRPC documentation page](https://grpc.io/docs/).</span></span>

<span data-ttu-id="a3b23-107">Las principales ventajas de gRPC son:</span><span class="sxs-lookup"><span data-stu-id="a3b23-107">The main benefits of gRPC are:</span></span>
* <span data-ttu-id="a3b23-108">Marco de RPC moderno, ligero y de alto rendimiento.</span><span class="sxs-lookup"><span data-stu-id="a3b23-108">Modern high-performance lightweight RPC framework.</span></span>
* <span data-ttu-id="a3b23-109">Desarrollo de la API de primer contrato utilizando búferes de protocolo de forma predeterminada, lo que permite realizar implementaciones independientes del idioma.</span><span class="sxs-lookup"><span data-stu-id="a3b23-109">Contract-first API development, using Protocol Buffers by default, allowing for language agnostic implementations.</span></span>
* <span data-ttu-id="a3b23-110">Dispone de herramientas para muchos idioma con la finalidad de generar clientes y servidores fuertemente tipados.</span><span class="sxs-lookup"><span data-stu-id="a3b23-110">Tooling available for many languages to generate strongly-typed servers and clients.</span></span>
* <span data-ttu-id="a3b23-111">Admite llamadas de transmisión en secuencias bidireccionales, de servidor y de cliente.</span><span class="sxs-lookup"><span data-stu-id="a3b23-111">Supports client, server, and bi-directional streaming calls.</span></span>
* <span data-ttu-id="a3b23-112">Uso reducido de red con serialización binaria Protobuf.</span><span class="sxs-lookup"><span data-stu-id="a3b23-112">Reduced network usage with Protobuf binary serialization.</span></span>

<span data-ttu-id="a3b23-113">Estas ventajas hacen que gRPC sea ideal para:</span><span class="sxs-lookup"><span data-stu-id="a3b23-113">These benefits make gRPC ideal for:</span></span>
* <span data-ttu-id="a3b23-114">Microservicios ligeros en los que la eficiencia sea fundamental.</span><span class="sxs-lookup"><span data-stu-id="a3b23-114">Lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="a3b23-115">Sistemas políglotas en los que se requieran varios idiomas para el desarrollo.</span><span class="sxs-lookup"><span data-stu-id="a3b23-115">Polyglot systems where multiple languages are required for development.</span></span>
* <span data-ttu-id="a3b23-116">Servicios en tiempo real de punto a punto que necesitan controlar respuestas o solicitudes de transmisión en secuencias.</span><span class="sxs-lookup"><span data-stu-id="a3b23-116">Point-to-point real-time services that need to handle streaming requests or responses.</span></span>

<span data-ttu-id="a3b23-117">Mientras que C# se puede implementar desde la [página de gRPC](https://grpc.io/docs/quickstart/csharp.html) oficial, la implementación actual se basa en la biblioteca nativa escrita en C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span><span class="sxs-lookup"><span data-stu-id="a3b23-117">While a C# implementation is currently available on the official [gRPC page](https://grpc.io/docs/quickstart/csharp.html), the current implementation relies on the native library written in C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)).</span></span> <span data-ttu-id="a3b23-118">Estamos trabajando para proporcionar una nueva implementación totalmente administrada basada en el servidor HTTP de Kestrel y la pila de ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a3b23-118">Work is currently in progress to provide a new implementation based on the Kestrel HTTP server and the ASP.NET Core stack that is fully managed.</span></span> <span data-ttu-id="a3b23-119">En los siguientes documentos encontrará una introducción a la creación de servicios de gRPC con esta nueva implementación.</span><span class="sxs-lookup"><span data-stu-id="a3b23-119">The following documents provide an introduction to building gRPC services with this new implementation.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a3b23-120">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="a3b23-120">Additional resources</span></span>

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>