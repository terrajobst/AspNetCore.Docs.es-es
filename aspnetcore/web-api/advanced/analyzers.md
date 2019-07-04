---
title: Uso de analizadores de API web
author: pranavkm
description: Obtenga más información sobre los analizadores de API web en Microsoft.AspNetCore.Mvc.Api.Analyzers.
monikerRange: '>= aspnetcore-2.2'
ms.author: prkrishn
ms.custom: mvc
ms.date: 12/14/2018
uid: web-api/advanced/analyzers
ms.openlocfilehash: 2aaef738ab2a64f85cb85708f63d2375c04cacb5
ms.sourcegitcommit: 0b9e767a09beaaaa4301915cdda9ef69daaf3ff2
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2019
ms.locfileid: "67538566"
---
# <a name="use-web-api-analyzers"></a>Uso de analizadores de API web

ASP.NET Core 2.2 (y versiones posteriores) incluye el paquete NuGet [Microsoft.AspNetCore.Mvc.Api.Analyzers](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Api.Analyzers), que contiene analizadores para las API web. Los analizadores funcionan con los controladores anotados con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> y se compilan con [convenciones de la API](xref:web-api/advanced/conventions).

## <a name="package-installation"></a>Instalación del paquete

Se puede agregar `Microsoft.AspNetCore.Mvc.Api.Analyzers` con uno de los métodos siguientes:

### <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* En la ventana **Consola del Administrador de paquetes**:
  * Vaya a **Vista** > **Otras ventanas** > **Consola del Administrador de paquetes**.
  * Vaya al directorio en el que esté el archivo *ApiConventions.csproj*.
  * Ejecute el siguiente comando:

    ```powershell
    Install-Package Microsoft.AspNetCore.Mvc.Api.Analyzers
    ```

* En el cuadro de diálogo **Administrar paquetes NuGet**:
  * Haga clic con el botón derecho en el proyecto en el **Explorador de soluciones** > **Administrar paquetes NuGet**.
  * Establezca el **origen del paquete** en "nuget.org".
  * En el cuadro de búsqueda, escriba "Microsoft.AspNetCore.Mvc.Api.Analyzers".
  * Seleccione el paquete "Microsoft.AspNetCore.Mvc.Api.Analyzers" en la pestaña **Examinar** y haga clic en **Instalar**.

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio para Mac](#tab/visual-studio-mac)

* Haga clic con el botón derecho en la carpeta *Paquetes*, en **Panel de solución** > **Agregar paquetes...** .
* Establezca el menú desplegable **Origen** de la ventana **Agregar paquetes** en "nuget.org".
* En el cuadro de búsqueda, escriba "Microsoft.AspNetCore.Mvc.Api.Analyzers".
* Seleccione el paquete "Microsoft.AspNetCore.Mvc.Api.Analyzers" en el panel de resultados y haga clic en **Agregar paquete**.

### <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

Ejecute el siguiente comando en el **terminal integrado**:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

### <a name="net-core-clitabnetcore-cli"></a>[CLI de .NET Core](#tab/netcore-cli)

Ejecute el siguiente comando:

```console
dotnet add ApiConventions.csproj package Microsoft.AspNetCore.Mvc.Api.Analyzers
```

---

## <a name="analyzers-for-api-conventions"></a>Analizadores para las convenciones de API

Los documentos de Open API contienen los códigos de estado y los tipos de respuesta que puede devolver una acción. En ASP.NET Core MVC, los atributos como <xref:Microsoft.AspNetCore.Mvc.ProducesResponseTypeAttribute> y <xref:Microsoft.AspNetCore.Mvc.ProducesAttribute> se usan para documentar una acción. <xref:tutorials/web-api-help-pages-using-swagger> ofrece información más detallada sobre cómo documentar su API.

Uno de los analizadores del paquete inspecciona los controladores anotados con <xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute> e identifica las acciones que no documentan completamente sus respuestas. Considere el ejemplo siguiente:

[!code-csharp[](conventions/sample/Controllers/ContactsController.cs?name=missing404docs&highlight=9)]

La acción anterior documenta el tipo de valor devuelto correcto HTTP 200, pero no documenta el código de estado de error HTTP 404. El analizador informa de la documentación que falta para el código de estado 404 de HTTP como una advertencia. Se proporciona una opción para corregir el problema.

![advertencia notificada por el analizador](conventions/_static/Analyzer.gif)

## <a name="additional-resources"></a>Recursos adicionales

* <xref:web-api/advanced/conventions>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:web-api/index>
