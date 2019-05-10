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
# <a name="introduction-to-grpc-on-aspnet-core"></a>Introducción a gRPC en ASP.NET Core

Por [John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) es un marco de llamada a procedimiento remoto (RPC) de alto rendimiento e independiente del idioma. Para obtener más información sobre los aspectos básicos de gRPC, vea la [página de documentación de gRPC](https://grpc.io/docs/).

Las principales ventajas de gRPC son:
* Marco de RPC moderno, ligero y de alto rendimiento.
* Desarrollo de la API de primer contrato utilizando búferes de protocolo de forma predeterminada, lo que permite realizar implementaciones independientes del idioma.
* Dispone de herramientas para muchos idioma con la finalidad de generar clientes y servidores fuertemente tipados.
* Admite llamadas de transmisión en secuencias bidireccionales, de servidor y de cliente.
* Uso reducido de red con serialización binaria Protobuf.

Estas ventajas hacen que gRPC sea ideal para:
* Microservicios ligeros en los que la eficiencia sea fundamental.
* Sistemas políglotas en los que se requieran varios idiomas para el desarrollo.
* Servicios en tiempo real de punto a punto que necesitan controlar respuestas o solicitudes de transmisión en secuencias.

Mientras que C# se puede implementar desde la [página de gRPC](https://grpc.io/docs/quickstart/csharp.html) oficial, la implementación actual se basa en la biblioteca nativa escrita en C (gRPC [C-core](https://grpc.io/blog/grpc-stacks)). Estamos trabajando para proporcionar una nueva implementación totalmente administrada basada en el servidor HTTP de Kestrel y la pila de ASP.NET Core. En los siguientes documentos encontrará una introducción a la creación de servicios de gRPC con esta nueva implementación.

## <a name="additional-resources"></a>Recursos adicionales

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>