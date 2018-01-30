---
title: "DI no compatible con escenarios para la protección de datos en ASP.NET Core"
author: rick-anderson
description: "Obtenga información acerca de cómo admitir escenarios de protección de datos que no se puede o no desea utilizar un servicio suministrado por inyección de dependencia."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: eccf914d20e04adbb113f17e262766ed2dd1a554
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>DI no compatible con escenarios para la protección de datos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

Suele ser el sistema de protección de datos de ASP.NET Core [agregado a un contenedor de servicio](xref:security/data-protection/consumer-apis/overview) y utilizado por los componentes dependientes a través de la inserción de dependencias (DI). Sin embargo, hay casos donde no es factible y deseadas, especialmente al importar el sistema en una aplicación existente.

Para admitir estos escenarios, el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete proporciona un tipo concreto, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), que ofrece una manera sencilla de usar la protección de datos sin tener que depender DI. El `DataProtectionProvider` escriba implementa [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Construir `DataProtectionProvider` sólo requiere proporcionar un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) instancia para indicar dónde se deben almacenar las claves criptográficas del proveedor, tal como se muestra en el ejemplo de código siguiente:

[!code-none[Main](non-di-scenarios/_static/nodisample1.cs)]

De forma predeterminada, la `DataProtectionProvider` tipo concreto no cifra el material de clave sin procesar antes la almacenarla en el sistema de archivos. Esto sirve para admitir escenarios donde los puntos de desarrollador para un recurso compartido de red y el sistema de protección de datos no pueden deducir automáticamente un mecanismo de cifrado de clave adecuado en rest.

Además, el `DataProtectionProvider` tipo concreto no [aislar las aplicaciones de](xref:security/data-protection/configuration/overview#per-application-isolation) de forma predeterminada. Todas las aplicaciones con el mismo directorio clave pueden compartir cargas siempre y cuando sus [finalidad parámetros](xref:security/data-protection/consumer-apis/purpose-strings) coincide con.

El [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) constructor acepta una devolución de llamada de configuración opcionales que puede usarse para ajustar los comportamientos del sistema. El ejemplo siguiente muestra el aislamiento de restauración con una llamada explícita a [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). El ejemplo también muestra la configuración del sistema para cifrar automáticamente las claves permanentes mediante DPAPI de Windows. Si el directorio apunta a un recurso compartido UNC, es recomendable que se va a distribuir un certificado compartido entre todos los equipos correspondientes como configurar el sistema para usar el cifrado basada en certificados con una llamada a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-none[Main](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Instancias de la `DataProtectionProvider` tipo concreto son caros de crear. Si una aplicación mantiene varias instancias de este tipo y si todo está usando el mismo directorio de almacenamiento de claves, puede degradar el rendimiento de la aplicación. Si usas el `DataProtectionProvider` tipo, se recomienda que cree este tipo una vez y volver a usarlo tanto como sea posible. El `DataProtectionProvider` tipo y todos [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) instancias creadas a partir de los son seguras para subprocesos para distintos llamadores.
