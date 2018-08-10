---
title: DevOps con ASP.NET Core y Azure
author: CamSoper
description: Una guía que proporciona guías de un extremo a otro sobre cómo crear una canalización de DevOps para una aplicación ASP.NET Core hospedada en Azure.
ms.author: casoper
ms.date: 08/07/2018
uid: azure/devops/index
ms.openlocfilehash: 09ca835e908e81c6f38f9430fb40638ba6dc3350
ms.sourcegitcommit: 29dfe436f54a27fbb4f6494bc639d16c75001fab
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/09/2018
ms.locfileid: "39722554"
---
# <a name="devops-with-aspnet-core-and-azure"></a>DevOps con ASP.NET Core y Azure

Le damos la bienvenida a la guía de Ciclo de vida de desarrollo de Azure para .NET. En esta guía le mostraremos los conceptos básicos de creación de un ciclo de vida de desarrollo en torno a Azure con herramientas y procesos de .NET. Cuando haya terminado con esta guía, podrá aprovechar las ventajas de una cadena de herramientas madura de DevOps.

## <a name="who-this-guide-is-for"></a>Destinatarios de esta guía

Debería ser un desarrollador de ASP.NET experimentado (nivel 200 o 300). No es necesario que tenga conocimientos de Azure, ya que está incluido en esta introducción. Esta guía también podría ser útil para ingenieros de DevOps, cuyo trabajo está más relacionado con las operación que con el desarrollo.

Esta guía está destinada a desarrolladores para Windows. Sin embargo, .NET Core es completamente compatible con Linux y macOS. Para adaptar esta guía para Linux o macOS, mire las llamadas en las que se indican las diferencias para Linux y macOS.

## <a name="what-this-guide-doesnt-cover"></a>Aspectos no tratados en esta guía

Esta guía está centrada en una experiencia de desarrollo continuo de un extremo a otro para desarrolladores de .NET. No es una guía exhaustiva de todo Azure y no se profundiza particularmente en API de .NET para servicios de Azure. El énfasis está en la integración, la implementación, la supervisión y la depuración continuas. Casi al final de la guía podrá ver recomendaciones para los pasos siguientes. En las sugerencias se incluyen servicios de plataformas de Azure que son útiles para desarrolladores de ASP.NET.

## <a name="whats-in-this-guide"></a>Qué se incluye en esta guía

### <a name="tools-and-downloadsxrefazuredevopstools-and-downloads"></a>[Herramientas y descargas](xref:azure/devops/tools-and-downloads)

Obtenga información sobre dónde adquirir las herramientas que se usan en esta guía.

### <a name="deploy-to-app-servicexrefazuredevopsdeploy-to-app-service"></a>[Implementación en App Service](xref:azure/devops/deploy-to-app-service)

Obtenga información sobre los distintos métodos para implementar una aplicación ASP.NET Core en Azure App Service.

### <a name="continuous-integration-and-deploymentxrefazuredevopscicd"></a>[Integración e implementación continuas](xref:azure/devops/cicd)

Cree una solución de implementación e integración continuas de un extremo a otro para su aplicación ASP.NET Core con GitHub, VSTS y Azure.

### <a name="monitor-and-debugxrefazuredevopsmonitor"></a>[Supervisión y depuración](xref:azure/devops/monitor)

Use las herramientas de Azure para supervisar la aplicación, solucionar problemas y ajustarla.

### <a name="next-stepsxrefazuredevopsnext-steps"></a>[Pasos siguientes](xref:azure/devops/next-steps)

Otras rutas de aprendizaje para los desarrolladores de ASP.NET Core que están aprendiendo sobre Azure.

## <a name="acknowledgments"></a>Agradecimientos

Gracias a todos los usuarios de la comunidad de .NET que han contribuido a esta guía con sugerencias útiles. También queremos mostrar nuestro agradecimiento específicamente a los siguientes miembros de la comunidad, que han contribuido a la revisión final de este contenido:

* [Sam Wronski](https://www.youtube.com/c/worldofzerodevelopment)
* [Jeffrey Palermo](https://twitter.com/jeffreypalermo)

## <a name="conclusion"></a>Conclusión

Esta guía le preparará para crear un ciclo de vida de desarrollo de integración continua alrededor de ASP.NET Core y Azure App Service.

## <a name="additional-reading"></a>Lecturas adicionales

* [¿Qué es la informática en la nube?](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [Ejemplos de informática en la nube](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [¿Qué es una IaaS?](https://azure.microsoft.com/overview/what-is-iaas/)
* [¿Qué es un PaaS?](https://azure.microsoft.com/overview/what-is-paas/)
