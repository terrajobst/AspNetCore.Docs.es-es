---
uid: web-api/overview/data/using-web-api-with-entity-framework/part-10
title: "Publique la aplicación en Azure servicio de aplicaciones de Azure | Documentos de Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/16/2014
ms.topic: article
ms.assetid: 10fd812b-94d6-4967-be97-a31ce9c45e2c
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/data/using-web-api-with-entity-framework/part-10
msc.type: authoredcontent
ms.openlocfilehash: 08994815cb339800619caacdcb8d717e9986f9d5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="publish-the-app-to-azure-azure-app-service"></a>Publique la aplicación en Azure servicio de aplicaciones de Azure
====================
por [Mike Wasson](https://github.com/MikeWasson)

[Descargar el proyecto completado](https://github.com/MikeWasson/BookService)

Como último paso, publicará la aplicación en Azure. En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**.

![](part-10/_static/image1.png)

Haga clic en **publicar** , se invoca el **Publicar Web** cuadro de diálogo. Si ha activado **Host en la nube** cuando se crea por primera vez el proyecto y, a continuación, la conexión y configuración ya está configurada. En ese caso, simplemente haga clic en el **configuración** ficha y compruebe &quot;ejecutar Code First Migrations&quot;. (Si no activó **Host en la nube** al principio, a continuación, siga los pasos descritos en la [próxima sección](#new-website).)

[![](part-10/_static/image3.png)](part-10/_static/image2.png)

Para implementar la aplicación, haga clic en **publicar**. Puede ver el progreso de la publicación en el **Web publicar actividad** ventana. (Desde el **vista** menú, seleccione **otras ventanas**, a continuación, seleccione **Web publicar actividad**.)

![](part-10/_static/image4.png)

Cuando Visual Studio finaliza la implementación de la aplicación, el explorador predeterminado se abre automáticamente en la dirección URL del sitio Web implementado y ahora se ejecuta la aplicación que creó en la nube. La dirección URL en la barra de direcciones del explorador, se muestra que el sitio se está cargando desde Internet.

[![](part-10/_static/image6.png)](part-10/_static/image5.png)

<a id="new-website"></a>
## <a name="deploying-to-a-new-website"></a>Implementar en un sitio Web nuevo

Si no activó **Host en la nube** cuando creó el proyecto por primera vez, puede configurar una nueva aplicación web ahora. En el Explorador de soluciones, haga clic en el proyecto y seleccione **publicar**. Seleccione el **perfil** ficha y haga clic en **sitios Web de Microsoft Azure**. Si no están actualmente inició sesión Azure, se le pedirá que inicie sesión en.

[![](part-10/_static/image8.png)](part-10/_static/image7.png)

En el **sitios Web existentes** cuadro de diálogo, haga clic en **nuevo**.

![](part-10/_static/image9.png)

Escriba un nombre de sitio. Seleccione la suscripción de Azure y la región. En **servidor de base de datos**, seleccione **crear nuevo servidor**, o seleccione un servidor existente. Haga clic en **Crear**.

[![](part-10/_static/image11.png)](part-10/_static/image10.png)

Haga clic en el **configuración** ficha y compruebe &quot;ejecutar Code First Migrations&quot;. A continuación, haga clic en **publicar**.

>[!div class="step-by-step"]
[Anterior](part-9.md)
