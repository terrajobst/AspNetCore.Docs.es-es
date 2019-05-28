---
title: Hospedaje e implementación de Blazor
author: guardrex
description: Descubra cómo hospedar e implementar aplicaciones de Blazor.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 05/23/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 5def0356d13975211dd234f6a6a9f5a993d003b7
ms.sourcegitcommit: e1623d8279b27ff83d8ad67a1e7ef439259decdf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 05/25/2019
ms.locfileid: "66223179"
---
# <a name="host-and-deploy-blazor"></a>Hospedaje e implementación de Blazor

Por [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com) y [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>Publicar la aplicación

Las aplicaciones se publican para implementación en la configuración de versión.

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. Seleccione **Compilar** > **Publicar {aplicación}** en la barra de navegación.
1. Seleccione el *destino de publicación*. Para publicar localmente, seleccione **Carpeta**.
1. Acepte la ubicación predeterminada del campo **Elegir una carpeta** o especifique una ubicación diferente. Seleccione el botón **Publicar**.


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code y CLI de .NET Core](#tab/visual-studio-code+netcore-cli)

Use el comando [dotnet publish](/dotnet/core/tools/dotnet-publish) para publicar la aplicación con una configuración de versión:

```console
dotnet publish -c Release
```

---

Al publicar la aplicación se desencadena una [restauración](/dotnet/core/tools/dotnet-restore) de las dependencias del proyecto y se [compila](/dotnet/core/tools/dotnet-build) el proyecto antes de crear los recursos para la implementación. Como parte del proceso de compilación, se quitan los ensamblados y métodos que no se usan para reducir los tiempos de carga y el tamaño de descarga de la aplicación.

Una aplicación cliente de Blazor se publica en la carpeta */bin/Release/{TARGET FRAMEWORK}/dist*. Una aplicación de servidor de Blazor se publica en la carpeta */bin/Release/{TARGET FRAMEWORK}/publish*.

Los recursos de la carpeta se implementan en el servidor web. La implementación puede ser un proceso manual o automatizado, en función de las herramientas de desarrollo que se usen.

## <a name="deployment"></a>Implementación

Para una guía sobre la implementación, consulte los temas siguientes:

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>Hospedaje sin servidor de Blazor con Azure Storage

Las aplicaciones de cliente de Blazor pueden obtenerse de [Azure Storage](https://azure.microsoft.com/services/storage/) como contenido estático directamente desde un contenedor de almacenamiento.

Para más información, consulte [Hospedaje e implementación de Blazor en el cliente (implementación independiente): Azure Storage](xref:host-and-deploy/blazor/client-side#azure-storage).
