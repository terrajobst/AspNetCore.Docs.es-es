---
uid: web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
title: La solución Contact Manager | Microsoft Docs
author: jrjlee
description: Esta serie de tutoriales usa una solución de ejemplo&#x2014;la solución Contact Manager&#x2014;para representar una aplicación de escala empresarial con una redistribución realista...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 4d8c8d19-055b-4b70-9ee1-f748f0db3a01
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/web-deployment-in-the-enterprise/the-contact-manager-solution
msc.type: authoredcontent
ms.openlocfilehash: 8187766190da43ded52359892601f8129b9940ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388113"
---
<a name="the-contact-manager-solution"></a>La solución Contact Manager
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Esto [serie de tutoriales](web-deployment-in-the-enterprise.md) usa una solución de ejemplo&#x2014;la solución Contact Manager&#x2014;para representar una aplicación de escala empresarial con un nivel de complejidad realista. En este tema presenta la solución Contact Manager, se describe los componentes claves de la solución e identifica los desafíos en la implementación de este tipo de aplicación en diferentes plataformas de destino en un entorno empresarial.
> 
> Cuando se trabaja con los temas en estos tutoriales, puede usar la solución Contact Manager como una implementación de referencia que se muestra cómo puede satisfacer los desafíos específicos en escenarios de implementación de la empresa. El tema siguiente, [configuración de la solución de Contact Manager](setting-up-the-contact-manager-solution.md), se describe cómo descargar y ejecutar la solución en la estación de trabajo de desarrollador.


## <a name="solution-overview"></a>Introducción a la solución

La solución Contact Manager consta de cuatro proyectos individuales:

![](the-contact-manager-solution/_static/image1.png)

- **ContactManager.Mvc**. Se trata de un proyecto de aplicación web de ASP.NET MVC 3 que representa el punto de entrada para la solución. Ofrece algunas características de aplicación web básica, como proporcionar a los usuarios la capacidad de crear y ver los detalles de contacto. La aplicación se basa en un servicio de Windows Communication Foundation (WCF) para administrar los contactos y una base de datos de servicios de aplicación de ASP.NET para administrar la autenticación y autorización.
- **ContactManager.Database**. Se trata de un proyecto de base de datos de Visual Studio. El proyecto define el esquema de una base de datos que almacena los detalles de contacto.
- **ContactManager.Service**. Se trata de un proyecto de servicio web WCF. La expone de servicio WCF, un punto de conexión que permite a los llamadores realizar crear, recuperar, actualizar y eliminar operaciones (CRUD) en el **ContactManager** base de datos. El servicio se basa en el **ContactManager** base de datos y el **ContactManager.Common.dll** ensamblado.
- **ContactManager.Common**. Se trata de un proyecto de biblioteca de clases. El servicio de WCF se basa en tipos definidos en este ensamblado.

La solución también incluye una carpeta de soluciones con el nombre de publicación. Este archivo contiene varios archivos de proyecto personalizadas y archivos de comandos que muestran cómo puede controlar y manipular el proceso de compilación e implementación. Se tratan con más detalle más adelante en este tutorial.

En un nivel conceptual, los componentes de la solución encajan similar al siguiente:

![](the-contact-manager-solution/_static/image2.png)

> [!NOTE]
> Mientras que la aplicación web de ASP.NET MVC 3 utiliza el proveedor de pertenencia ASP.NET, todas las páginas dentro de la aplicación web permiten acceso anónimo. Esto claramente no es una configuración realista. Sin embargo, la solución se configura en este modo para que sea más fácil de implementar y probar la solución sin necesidad de configurar los roles y las cuentas de usuario.


## <a name="deployment-challenges"></a>Desafíos de la implementación

La solución Contact Manager muestra varios desafíos de implementación que son comunes a muchos escenarios de implementación de enterprise:

- La solución consta de varios proyectos dependientes. Deberá implementar estos proyectos al mismo tiempo.
- Las cadenas de conexión y puntos de conexión de servicio deben actualizarse para cada entorno y, en muchos casos esta información no estará disponible para el desarrollador.
- Cuando se implementa el **ContactManager** base de datos a los entornos de ensayo y producción, tiene que conservar los datos existentes en las implementaciones posteriores.
- Al implementar la base de datos de servicios de aplicación de ASP.NET, deberá implementar algunos datos de configuración, pero omitir los datos de la cuenta de usuario.
- Los proyectos incluyen algunos archivos y carpetas que no se deben implementar. Tendrá que excluir estos archivos y carpetas desde el proceso de implementación.
- La solución debe admitir una implementación automatizada desde un servidor de compilación de Team Foundation Server (TFS).

## <a name="conclusion"></a>Conclusión

En este tema se proporciona una descripción general de la solución Contact Manager y se identifica algunos de los desafíos inherentes de implementación que son comunes a muchos escenarios de implementación de empresa. Los temas restantes en este tutorial describen algunas de las técnicas que puede usar para superar estos retos.

El tema siguiente, [configuración de la solución de Contact Manager](setting-up-the-contact-manager-solution.md), se describe cómo descargar y ejecutar la solución en la estación de trabajo de desarrollador.

> [!div class="step-by-step"]
> [Anterior](web-deployment-in-the-enterprise.md)
> [Siguiente](setting-up-the-contact-manager-solution.md)
