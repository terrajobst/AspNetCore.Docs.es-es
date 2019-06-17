---
title: Escenarios compatibles con DI no para la protección de datos en ASP.NET Core
author: rick-anderson
description: Obtenga información sobre cómo admitir escenarios de protección de datos donde no se puede o no desea utilizar un servicio proporcionado por la inserción de dependencias.
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/configuration/non-di-scenarios
ms.openlocfilehash: 62280a9f911b003383cbe348b9b62942766a2b99
ms.sourcegitcommit: f5762967df3be8b8c868229e679301f2f7954679
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/13/2019
ms.locfileid: "67048230"
---
# <a name="non-di-aware-scenarios-for-data-protection-in-aspnet-core"></a>Escenarios compatibles con DI no para la protección de datos en ASP.NET Core

Por [Rick Anderson](https://twitter.com/RickAndMSFT)

El sistema de protección de datos de ASP.NET Core es normalmente [agregado a un contenedor de servicios](xref:security/data-protection/consumer-apis/overview) y consumo de los componentes dependientes a través de la inserción de dependencias (DI). Sin embargo, hay casos donde esto no es viable o deseado, especialmente al importar el sistema en una aplicación existente.

Para admitir estos escenarios, el [Microsoft.AspNetCore.DataProtection.Extensions](https://www.nuget.org/packages/Microsoft.AspNetCore.DataProtection.Extensions/) paquete proporciona un tipo concreto, [DataProtectionProvider](/dotnet/api/Microsoft.AspNetCore.DataProtection.DataProtectionProvider), que ofrece una manera sencilla de usar protección de datos sin tener que depender de DI. El `DataProtectionProvider` escriba implementa [IDataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotectionprovider). Construir `DataProtectionProvider` sólo requiere proporcionar un [DirectoryInfo](/dotnet/api/system.io.directoryinfo) instancia para indicar dónde se deben almacenar las claves criptográficas del proveedor, tal como se muestra en el ejemplo de código siguiente:

[!code-csharp[](non-di-scenarios/_static/nodisample1.cs)]

De forma predeterminada, el `DataProtectionProvider` tipo concreto no cifra el material de clave sin procesar guardarlos antes en el sistema de archivos. Esto es para admitir escenarios donde los puntos de desarrollador a un recurso compartido de red y el sistema de protección de datos no pueden deducir automáticamente un mecanismo de cifrado de claves en reposo adecuado.

Además, el `DataProtectionProvider` tipo concreto no [aislar aplicaciones](xref:security/data-protection/configuration/overview#per-application-isolation) de forma predeterminada. Todas las aplicaciones con el mismo directorio clave pueden compartir las cargas siempre y cuando sus [finalidad parámetros](xref:security/data-protection/consumer-apis/purpose-strings) coinciden.

El [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionprovider) constructor acepta una devolución de llamada de configuración opcional que puede usarse para ajustar los comportamientos del sistema. El ejemplo siguiente muestra la restauración aislamiento con una llamada explícita a [SetApplicationName](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.setapplicationname). El ejemplo también muestra la configuración del sistema para cifrar automáticamente claves persistentes mediante DPAPI de Windows. Si el directorio señala a un recurso compartido UNC, es posible que desee distribuir un certificado compartido entre todos los equipos correspondientes y configurar el sistema para usar cifrado basada en certificados con una llamada a [ProtectKeysWithCertificate](/dotnet/api/microsoft.aspnetcore.dataprotection.dataprotectionbuilderextensions.protectkeyswithcertificate).

[!code-csharp[](non-di-scenarios/_static/nodisample2.cs)]

> [!TIP]
> Las instancias de la `DataProtectionProvider` son caros de crear un tipo concreto. Si una aplicación mantiene varias instancias de este tipo y todos usan el mismo directorio de almacenamiento de claves, podría degradar el rendimiento de la aplicación. Si usas el `DataProtectionProvider` tipo, se recomienda que cree este tipo una vez y volver a usarlo tanto como sea posible. El `DataProtectionProvider` tipo y todos [IDataProtector](/dotnet/api/microsoft.aspnetcore.dataprotection.idataprotector) creadas a partir de instancias son seguras para subprocesos para varios de los llamadores.
