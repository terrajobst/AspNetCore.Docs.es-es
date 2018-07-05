---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: Publicar la aplicación en Azure App Service de Azure | Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 0290b392c1b292d0f3cc080dbfa25ec6103b2751
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400809"
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publicar la aplicación en Azure App Service de Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargue el proyecto completado](https://github.com/MikeWasson/BookService)

Como último paso, publicará la aplicación en Azure. En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.

![](part-10/_static/image1.png)

Al hacer clic en **publicar** invoca el **publicación Web** cuadro de diálogo. Si ha activado **Host en la nube** cuando creó por primera vez el proyecto y, a continuación, la conexión y la configuración ya está configurada. En ese caso, simplemente haga clic en el **configuración** pestaña y compruebe &quot;ejecutar migraciones Code First&quot;. (Si no comprobara **Host en la nube** al principio, a continuación, siga los pasos descritos en la [siguiente sección](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implementar la aplicación, haga clic en **publicar**. Puede ver el progreso de la publicación en el **actividad de publicación Web** ventana. (Desde el **vista** menú, seleccione **Other Windows**, a continuación, seleccione **actividad de publicación Web**.)

![](part-10/_static/image4.png)

Cuando Visual Studio termine de implementar la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado, y ahora se ejecuta la aplicación que creó en la nube. La dirección URL en la barra de direcciones del explorador muestra que el sitio se está cargando desde Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implementación en un sitio Web nuevo

Si no comprobara **Host en la nube** cuando creó por primera vez el proyecto, puede configurar una nueva aplicación web ahora. En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**. Seleccione el **perfil** ficha y haga clic en **Microsoft Azure Websites**. Si actualmente no ha iniciado Azure, se le pedirá que inicie sesión.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

En el **sitios Web existentes** cuadro de diálogo, haga clic en **New**.

![](part-10/_static/image9.png)

Escriba un nombre de sitio. Seleccione su suscripción de Azure y la región. En **el servidor de base de datos**, seleccione **crear nuevo servidor**, o seleccione un servidor existente. Haga clic en **Crear**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Haga clic en el **configuración** pestaña y compruebe &quot;ejecutar migraciones Code First&quot;. A continuación, haga clic en **publicar**.

> [!div class="step-by-step"]
> [Anterior](part-9.md)
