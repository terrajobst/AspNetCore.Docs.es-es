---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
title: La integración continua y entrega continua (crear aplicaciones de nube reales con Azure) | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/12/2014
ms.assetid: eaece9f5-f80c-428b-b771-5db66d275b7d
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery
msc.type: authoredcontent
ms.openlocfilehash: 787aacfd843f5f72e567670d601fb036a2c474bc
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836962"
---
<a name="continuous-integration-and-continuous-delivery-building-real-world-cloud-apps-with-azure"></a>La integración continua y entrega continua (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Los dos primeros recomienda patrones de desarrollo de procesos eran [automatizar todo](automate-everything.md) y [Control de código fuente](source-control.md), y combina el tercer modelo de proceso. Integración continua (CI) significa que cada vez que un desarrollador protege en el código para el repositorio de origen, se desencadena automáticamente una compilación. Entrega continua (CD) lleva esto un paso adicional: después de una compilación y pruebas unitarias automatizadas son correctas, implementar automáticamente la aplicación en un entorno donde puede realizar más pruebas de profundidad.

La nube permite minimizar el costo de mantenimiento de un entorno de prueba porque solo paga por los recursos del entorno mientras usa ellos. El proceso de CD puede configurar el entorno de prueba cuando lo necesite, y puede dar de baja el entorno cuando haya terminado las pruebas.

## <a name="continuous-integration-and-continuous-delivery-workflow"></a>Flujo de trabajo de integración continua y entrega continua

Por lo general se recomienda que realice la entrega continua para el desarrollo y entornos de ensayo. La mayoría de los equipos, incluso en Microsoft, requieren un proceso de revisión y aprobación manual para la implementación de producción. Para una producción implementación que desea asegurarse de que se produce cuando personas clave en el equipo de desarrollo están disponibles para soporte técnico, o durante los períodos de poco tráfico. Pero no hay nada que evitan que automatice completamente sus entornos de desarrollo y pruebas para que todo desarrollador tiene que hacer es comprobar en un cambio y un entorno está configurado para las pruebas de aceptación.

El siguiente diagrama de [un Microsoft Patterns and Practices libro electrónico sobre la entrega continua](http://aka.ms/ReleasePipeline) ilustra un flujo de trabajo típica. Haga clic en la imagen para verla tamaño completo en su contexto original.

[![Flujo de trabajo de entrega continua](continuous-integration-and-continuous-delivery/_static/image1.png)](https://msdn.microsoft.com/library/dn449955.aspx)

## <a name="how-the-cloud-enables-cost-effective-ci-and-cd"></a>Cómo habilita la nube rentable CI y CD

Es fácil automatizar estos procesos en Azure. Porque todo lo que se está ejecutando en la nube, no tienes que comprar ni administrar servidores para las compilaciones o en sus entornos de pruebas. Y no tiene que esperar a un servidor que esté disponible para realizar las pruebas en el. Con cada compilación que lo hace, podría poner en marcha un entorno de prueba en Azure mediante el script de automatización, pruebas de aceptación de ejecución o más comprobaciones exhaustivas en él y, a continuación, cuando haya terminado simplemente Deshágase de él. Y si sólo ejecuta ese servidor durante 2 horas o 8 horas o un día, la cantidad de dinero que tiene que pagar por ello es mínima, ya que está pagando solo por el tiempo que se está ejecutando realmente una máquina. Por ejemplo, el entorno necesario para la corrección de aplicación básicamente cuesta aproximadamente 1 céntimos por hora si Subir un nivel desde el nivel gratuito. En el transcurso de un mes, si solo se ejecuta el entorno de una hora a la vez, el entorno de pruebas probablemente costaría menos de un café con leche que se compra en Starbucks.

## <a name="visual-studio-team-services-vsts"></a>Visual Studio Team Services (VSTS)

VSTS proporciona una serie de características para ayudarle con el desarrollo de aplicaciones de planeación de la implementación.

- Es compatible con Git (distribuido) y control de código fuente TFVC (centralizado).
- Ofrece un servicio de compilación elástica, lo que significa que dinámicamente crea los servidores de compilación cuando se necesitan y les lleva inactivo cuando hayan terminado. Automáticamente, puede iniciar una compilación cuando alguien protege cambios de código fuente y no tiene que haber asignar y pagar por sus propios servidores de compilación que se encuentran inactivos la mayor parte del tiempo. El servicio de compilación es gratuita, siempre y cuando no supere un determinado número de compilaciones. Si piensa hacer un gran volumen de compilaciones, puede pagar un poco de trabajo adicional para los servidores de compilación reservado.
- Admite la entrega continua a Azure.
- Es compatible con las pruebas de carga automatizada. Las pruebas de carga es fundamental para una aplicación de nube, pero a menudo la deformación hasta que sea demasiado tarde. Las pruebas de carga simulan un uso intensivo de una aplicación por miles de usuarios, lo que le permite encontrar cuellos de botella y mejore el rendimiento, antes de publicar la aplicación en producción.
- Admite la colaboración en equipo sala, lo que facilita la comunicación en tiempo real y la colaboración para pequeños equipos ágiles.
- Admite la administración de proyectos ágiles.


Para obtener más información sobre la integración continua y las características de entrega de VSTS, consulte [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

Si está buscando una administración de proyectos de llave en mano, colaboración en equipo y la solución de control de código fuente, consulte VSTS. El servicio es gratuito para 5 usuarios, y puede suscribirse en [Visual Studio Team Services](https://www.visualstudio.com/team-services/).

## <a name="summary"></a>Resumen

Los patrones de desarrollo de tres nube primera han sido acerca de cómo implementar un proceso de desarrollo repetible, confiable y predecible con el tiempo de ciclo baja. En el [siguiente capítulo](web-development-best-practices.md) empezamos a observar los patrones de arquitectura y codificación.

## <a name="resources"></a>Recursos

Para obtener más información, consulte [implementar una aplicación web en Azure App Service](https://azure.microsoft.com/documentation/articles/web-sites-deploy/).

Consulte también los siguientes recursos:

- [Creación de una canalización de versión con Team Foundation Server 2012](http://aka.ms/ReleasePipeline). Libro electrónico con laboratorios prácticos y código de ejemplo Microsoft Patterns and Practices, proporciona una introducción detallada para la entrega continua. Cubre el uso de Visual Studio Lab Management y Release Management para Visual Studio.
- [Las herramientas de ALM Rangers DevOps y orientación](https://aka.ms/vsarsolutions/). Los Rangers de ALM introdujo la solución complementaria de ejemplo de DevOps Workbench y la orientación práctica en colaboración con los patrones &amp; libro de prácticas *crear una canalización de versiones con TFS 2012*, como una manera excelente de comenzar aprender los conceptos de DevOps &amp; Release Management para TFS 2012 y a comprar. La guía muestra cómo compilar una sola vez e implementar en varios entornos.
- [Pruebas para entrega continua con Visual Studio 2012](https://msdn.microsoft.com/library/jj159345.aspx). Libro electrónico de Microsoft Patterns and Practices, se explica cómo integrar las pruebas automatizadas con entrega continua.
- [WindowsAzureDeploymentTracker](https://github.com/RyanTBerry/WindowsAzureDeploymentTracker). Código fuente de una herramienta diseñada para capturar una compilación de TFS (basado en una etiqueta), genérelo, empaquetarlo, permitir que otra persona en el rol de DevOps para configurar aspectos específicos del mismo e insertarla en Azure. La herramienta realiza un seguimiento del proceso de implementación con el fin de habilitar las operaciones de "restaurar" a una versión implementada anteriormente. La herramienta no tiene dependencias externas y puede funcionar independiente mediante las API de TFS y el SDK de Azure.
- [La entrega continua: Versiones de Software confiable a través de la compilación, prueba e implementación automatización](https://www.amazon.com/Continuous-Delivery-Deployment-Automation-Addison-Wesley/dp/0321601912/ref=sr_1_1?s=books&amp;ie=UTF8&amp;qid=1377126361). Libro de Jez Humble.
- [¡Liberarla! Diseñar e implementar Software para entornos de producción](https://www.amazon.com/Release-It-Production-Ready-Pragmatic-Programmers/dp/0978739213). Libro de Michael T. Nygard.

> [!div class="step-by-step"]
> [Anterior](source-control.md)
> [Siguiente](web-development-best-practices.md)
