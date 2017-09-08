---
title: DI no es compatible con escenarios
author: rick-anderson
description: 
keywords: "Núcleo de ASP.NET,"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a7d8a962-80ff-48e3-96f6-8472b7ba2df9
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 54a930c26f9f48ea0e6f7865e2927bcde0f4d6c0
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/11/2017
---
# <a name="non-di-aware-scenarios"></a>DI no es compatible con escenarios

El sistema de protección de datos normalmente está diseñado [va a agregar a un contenedor de servicios](../consumer-apis/overview.md) y proporcionar a sus componentes dependientes a través de un mecanismo de DI. Sin embargo, puede haber algunos casos donde esto no es factible, especialmente al importar el sistema en una aplicación existente.

Para admitir estos escenarios el paquete Microsoft.AspNetCore.DataProtection.Extensions proporciona un tipo concreto DataProtectionProvider que ofrece una manera sencilla de usar el sistema de protección de datos sin tener que pasar a través de las rutas de código DI específica. El propio tipo implementa IDataProtectionProvider, y la creación, es tan fácil como proporcionar un DirectoryInfo dónde se deben almacenar las claves criptográficas de este proveedor.

Por ejemplo:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

>[!WARNING]
> De forma predeterminada el DataProtectionProvider tipo concreto no cifra material de clave sin procesar antes la almacenarla en el sistema de archivos. Esto sirve para admitir escenarios donde se comparten los puntos de desarrollador a una red, en cuyo caso el sistema de protección de datos no puede deducir automáticamente un mecanismo de cifrado de clave adecuado en rest.
>
>Además, no el tipo concreto de DataProtectionProvider [aislar las aplicaciones](overview.md#data-protection-configuration-per-app-isolation) de forma predeterminada, por lo que todas las aplicaciones que apunta en el mismo directorio clave pueden compartir cargas siempre que coincidan con sus parámetros de uso.

El desarrollador de aplicaciones puede resolver ambos si lo desea. El constructor DataProtectionProvider acepta un [devolución de llamada de configuración opcional](overview.md#data-protection-configuration-callback) que se puede utilizar para ajustar los comportamientos del sistema. El ejemplo siguiente muestra la restauración aislamiento a través de una llamada explícita a SetApplicationName, y también se muestra la configuración del sistema para cifrar automáticamente las claves permanentes mediante DPAPI de Windows. Si el directorio apunta a un recurso compartido UNC, es recomendable que se va a distribuir un certificado compartido entre todos los equipos correspondientes como configurar el sistema para usar cifrado basado en certificados en su lugar mediante una llamada a [ProtectKeysWithCertificate](overview.md#configuring-x509-certificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

>[!TIP]
> Las instancias de un tipo concreto de DataProtectionProvider son caras de crear. Si una aplicación mantiene varias instancias de este tipo y si está señala en el mismo directorio de almacenamiento de claves, el rendimiento de la aplicación puede verse afectado. El uso previsto es que el desarrollador de aplicaciones crear instancias de este tipo una vez tenga que volver a usar esta referencia única tanto como sea posible. El tipo de DataProtectionProvider y todas las instancias de IDataProtector creadas a partir de él son seguras para subprocesos para distintos llamadores.
