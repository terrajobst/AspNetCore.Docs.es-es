---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: Integración continua y la entrega continua (creación de aplicaciones de nube reales con Azure) | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 4d482aaa0d25d6e6baaf196df4b4bb9335408e46
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>Integración continua y la entrega continua (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Las dos primeras recomienda patrones de procesos de desarrollo estaban [automatizar todo](automate-everything.md) y [Control de código fuente](source-control.md), y combina el tercer modelo de proceso. Integración continua (CI) significa que cada vez que un desarrollador protege en el código en el repositorio de origen, se activará automáticamente una compilación. La entrega continua (CD) toma un paso adicional: después de una compilación y pruebas unitarias automatizadas son correctas, implementar automáticamente la aplicación en un entorno donde se pueden realizar más pruebas detallada.

La nube permite minimizar el costo de mantenimiento de un entorno de prueba porque solo se paga por los recursos del entorno mientras utilizas ellos. El proceso de CD puede configurar el entorno de prueba cuando lo necesite y el entorno puede desconectarla cuando haya terminado pruebas.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flujo de trabajo de integración continua y la entrega continua

Por lo general se recomienda que realice la entrega continua para su desarrollo y entornos de ensayo. La mayoría de los equipos, incluso en Microsoft, requiere un proceso de revisión y aprobación manual de implementación de producción. Para una producción implementación que desea asegurarse de que ocurre cuando personas claves en el equipo de desarrollo están disponibles para la compatibilidad con, o durante los períodos de poco tráfico. Pero no hay nada que evitan que automatice completamente los entornos de desarrollo y prueba para que todo un desarrollador tiene que hacer es comprobar en un cambio y un entorno está configurado para las pruebas de aceptación.

El siguiente diagrama de [un Microsoft Patterns and Practices libros electrónicos sobre la entrega continua](http://aka.ms/ReleasePipeline) muestra un flujo de trabajo típico. Haga clic en la imagen para verla tamaño completo en su contexto original.

[![Flujo de trabajo de la entrega continua](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Forma en que la nube permite rentable CI y CD

Es fácil automatizar estos procesos en Azure. Como estás ejecutando todo en la nube, no tienes que comprar o administrar los servidores para los entornos de prueba o de las compilaciones. Y no tendrá que esperar para que estén disponibles para realizar las pruebas en un servidor de. Con cada compilación que lo hace, podría poner en marcha un entorno de prueba en Azure mediante el script de automatización, pruebas de aceptación de ejecución o más comprobaciones exhaustivas en él y, a continuación, cuando haya terminado simplemente caerlo. Y si solo ejecuta ese servidor para 2 horas, 8 horas o un día, la cantidad de dinero que tendrá que pagar por él es mínima, ya que solo se cobran durante el tiempo que se está ejecutando realmente una máquina. Por ejemplo, el entorno necesario para la corrección de aplicación básicamente cuesta aproximadamente 1 céntimos por hora si Subir un nivel desde el nivel gratis. En el transcurso de un mes, si solo se ejecuta el entorno de la hora a la vez, su entorno de pruebas haría probablemente costo es menor que un leche que se compra en Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS proporciona una serie de características para ayudarle a desarrollo de aplicaciones de planeación de la implementación.

- Es compatible con Git (distribuidos) y control de código fuente TFVC (centralizado).
- Ofrece un servicio de compilación flexible, lo que significa crea servidores de compilación cuando se necesitan dinámicamente y los deja hacia abajo cuando haya terminado. Automáticamente, puede comenzar una compilación cuando alguien Proteja los cambios de código de origen y no tiene que haber asignar y paga por sus propios servidores de compilación que se encuentran inactivos la mayor parte del tiempo. El servicio de compilación es gratuito, siempre y cuando no se supera un cierto número de compilaciones. Si piensa hacer un gran volumen de las compilaciones, puede pagar un poco de trabajo adicional para los servidores de compilación reservada.
- Admite entrega continuada para Azure.
- Admite la prueba de carga automatizada. Pruebas de carga es fundamental para una aplicación de nube, pero a menudo se dejó hasta que sea demasiado tarde. Pruebas de carga simulan un uso intensivo de una aplicación por miles de usuarios, lo que le permite encontrar cuellos de botella y mejorar el rendimiento, antes de publicar la aplicación en producción.
- Admite la colaboración de salón de equipo, que facilita la comunicación en tiempo real y la colaboración para pequeños equipos ágiles.
- Admite la administración de proyectos de agile.


Para obtener más información sobre la integración continua y características de entrega de VSTS, consulte [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Si está buscando una administración de proyectos de preparada, colaboración en equipo y la solución de control de origen, consulte VSTS. El servicio es gratuito para 5 usuarios y puede registrarse para él en [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Resumen

Los modelos de desarrollo de tres nube primera han sido sobre cómo implementar un proceso de desarrollo repetible, confiable y predecible con tiempo de ciclo de baja. En el [siguiente capítulo](web-development-best-practices.md) , empezamos a mirar los patrones arquitectónicos y codificación.

## <a name="resources"></a>Recursos

Para obtener más información, consulte [implementar una aplicación web en el servicio de aplicación de Azure](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Vea también los siguientes recursos:

- [Creación de una canalización de versiones con Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Laboratorios prácticos de libros electrónicos y código de ejemplo por Microsoft Patterns and Practices, proporciona una introducción detallada a la entrega continua. Cubre el uso de Visual Studio Lab Management y administración de versión de Visual Studio.
- [Instrucciones y herramientas de ALM Rangers DevOps](https://aka.ms/vsarsolutions/). ALM Rangers introdujo la solución complementaria de ejemplo de DevOps Workbench y directrices prácticas en colaboración con los patrones &amp; libreta de prácticas *crear una canalización de versiones con TFS 2012*, como una excelente manera de iniciar aprender los conceptos de DevOps &amp; Release Management para TFS 2012 y para iniciar el neumático. La guía muestra cómo compilar una vez e implementarla en varios entornos.
- [Pruebas para la entrega continua con Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Libro electrónico por Microsoft Patterns and Practices, explica cómo integrar las pruebas automatizadas con la entrega continua.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código fuente de una herramienta diseñada para capturar una compilación de TFS (según una etiqueta), genérelo, empaquetarlo, permitir que otra persona en el rol de DevOps para configurar aspectos específicos del mismo e insertar en Azure. La herramienta realiza un seguimiento del proceso de implementación con el fin de permitir que las operaciones "restaurar" a una versión implementada anteriormente. La herramienta no tiene dependencias externas y puede funcionar independiente con las API de TFS y el SDK de Azure.
- [La entrega continua: Software confiable libera a través de la compilación, prueba y automatización de la implementación](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Libro de Jez modesto.
- [Liberarlo. Diseñar e implementar Software para entornos de producción](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Libro de Michael T. Nygard.

> [!div class="step-by-step"]
> [Anterior](source-control.md)
> [Siguiente](web-development-best-practices.md)
