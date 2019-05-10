---
title: Migrar la configuración a ASP.NET Core
author: ardalis
description: Obtenga información sobre cómo migrar la configuración de un proyecto de MVC de ASP.NET a un proyecto de ASP.NET Core MVC.
ms.author: riande
ms.date: 10/14/2016
uid: migration/configuration
ms.openlocfilehash: c379f1f64dc5ab8aeb48055124e86e4e60d93785
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/27/2019
ms.locfileid: "64894932"
---
# <a name="migrate-configuration-to-aspnet-core"></a>Migrar la configuración a ASP.NET Core

Por [Steve Smith](https://ardalis.com/) y [Scott Addie](https://scottaddie.com)

En el artículo anterior, comenzamos a [migrar un proyecto de MVC de ASP.NET a ASP.NET Core MVC](xref:migration/mvc). En este artículo, nos migrar la configuración.

[Vea o descargue el código de ejemplo](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/configuration/samples) ([cómo descargarlo](xref:index#how-to-download-a-sample))

## <a name="setup-configuration"></a>Parámetros de configuración

ASP.NET Core ya no usa el *Global.asax* y *web.config* archivos que utilizan versiones anteriores de ASP.NET. En versiones anteriores de ASP.NET, la lógica de inicio de la aplicación se ha puesto en un `Application_StartUp` método dentro de *Global.asax*. Más adelante, en ASP.NET MVC, un *Startup.cs* archivo se incluyó en la raíz del proyecto; y, en el se llamó cuando se inicia la aplicación. ASP.NET Core ha adoptado este enfoque completamente mediante la colocación de toda la lógica de inicio en el *Startup.cs* archivo.

El *web.config* archivo también se ha sustituido en ASP.NET Core. Configuración de ahora se puede configurar, como parte del procedimiento de inicio de aplicación se describe en *Startup.cs*. Configuración todavía puede usar archivos XML, pero normalmente los proyectos de ASP.NET Core colocará los valores de configuración en un archivo con formato JSON, como *appsettings.json*. Sistema de configuración de ASP.NET Core también puede acceder fácilmente las variables de entorno, lo que pueden proporcionar un [ubicación más segura y sólida](xref:security/app-secrets) para valores específicos del entorno. Esto es especialmente cierto para los secretos, como las cadenas de conexión y las claves de API que no deben comprobarse en el control de código fuente. Consulte [configuración](xref:fundamentals/configuration/index) para obtener más información sobre la configuración en ASP.NET Core.

En este artículo, hemos empezado con el proyecto de ASP.NET Core parcialmente migrado desde [el artículo anterior](xref:migration/mvc). En la instalación de configuración, agregue el siguiente constructor y propiedad a la *Startup.cs* archivo se encuentra en la raíz del proyecto:

[!code-csharp[](configuration/samples/WebApp1/src/WebApp1/Startup.cs?range=11-16)]

Tenga en cuenta que en este momento, el *Startup.cs* archivo no se compilará, medida necesitamos agregar lo siguiente `using` instrucción:

```csharp
using Microsoft.Extensions.Configuration;
```

Agregar un *appsettings.json* archivo a la raíz del proyecto mediante la plantilla de elemento adecuado:

![Agregar AppSettings JSON](configuration/_static/add-appsettings-json.png)

## <a name="migrate-configuration-settings-from-webconfig"></a>Migrar la configuración de web.config

Nuestro proyecto de ASP.NET MVC incluye la cadena de conexión de base de datos necesarios en *web.config*, en el `<connectionStrings>` elemento. En nuestro proyecto de ASP.NET Core, vamos a almacenar esta información en el *appsettings.json* archivo. Abra *appsettings.json*y tenga en cuenta que ya incluye lo siguiente:

[!code-json[](../migration/configuration/samples/WebApp1/src/WebApp1/appsettings.json?highlight=4)]

En la línea resaltada descrita anteriormente, cambie el nombre de la base de datos **_CHANGE_ME** en el nombre de la base de datos.

## <a name="summary"></a>Resumen

ASP.NET Core coloca toda la lógica de inicio de la aplicación en un solo archivo, en el que los servicios necesarios y las dependencias pueden definirse y configuradas. Reemplaza el *web.config* archivo con una característica de configuración flexible que puede utilizar una variedad de formatos de archivo, como JSON, así como las variables de entorno.
