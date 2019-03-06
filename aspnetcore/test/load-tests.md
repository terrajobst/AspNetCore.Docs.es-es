---
title: Las pruebas de carga o esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Describe varios importantes herramientas y enfoques de pruebas de carga y las aplicaciones ASP.NET Core de prueba de carga.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 587df6e216943d3eeec779df4d0554dd0fc2fda0
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345433"
---
# <a name="load-and-stress-testing-aspnet-core"></a>Carga y esfuerzo de pruebas de ASP.NET Core

Pruebas de carga y pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable. Sus objetivos son distintos incluso comparten a menudo pruebas similares.

**Las pruebas de carga**: Comprueba si la aplicación puede controlar una carga de usuarios para un determinado escenario especificado sin dejar de satisfacer el objetivo de respuesta. La aplicación se ejecuta en condiciones normales.

**Las pruebas de esfuerzo**: Estabilidad de aplicación de pruebas cuando se ejecutan en condiciones extremas y a menudo un largo período de tiempo:

* Carga de usuarios elevado: picos o aumentando gradualmente.
* Recursos informáticos limitados.  

¿En situaciones de estrés, puede la aplicación de recuperarse de errores y volver correctamente al comportamiento esperado? En situaciones de estrés, la aplicación está *no* ejecutar en condiciones normales.

Visual Studio 2019 será la última versión de Visual Studio con las características de prueba de carga. Para los clientes que requieren herramientas de prueba de carga, se recomienda usar herramientas de prueba de carga alternativas, como Apache JMeter, Akamai CloudTest o Blazemeter. Para obtener más información, consulte el [notas de versión preliminar de Visual Studio 2019](/visualstudio/releases/2019/release-notes-preview#test-tools).

Finaliza la prueba de carga service en Azure DevOps en 2020. Para obtener más información, consulte [prueba final del servicio del ciclo de vida de carga basado en la nube](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permite a los usuarios crear, desarrollar y depurar las pruebas de carga y rendimiento web. Una opción está disponible para crear las pruebas mediante la grabación de acciones en el explorador web.

[Inicio rápido: Crear un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) se muestra cómo crear, configurar y ejecutar una prueba de carga de proyectos con Visual Studio 2017.

Para más información, consulte [Recursos adicionales](#add).

Las pruebas de carga pueden configurarse para ejecutar en local o ejecutar en la nube con Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Se pueden iniciar ejecuciones de pruebas de carga con el [planes de prueba de Azure DevOps](/azure/devops/test/load-test/index?view=vsts) service.

![](./load-tests/_static/azure-devops-load-test.png)

El servicio admite los siguientes tipos de formato de prueba:

- Prueba de Visual Studio: pruebas web creados en Visual Studio.
- Prueba basada en el almacenamiento HTTP: el tráfico HTTP capturado dentro del archivo se reproduce durante las pruebas.
- [Prueba basada en URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) : permite especificar las direcciones URL para la carga de prueba, tipos de solicitud, los encabezados y las cadenas de consulta. Ejecutar configuración de los parámetros como la duración, se puede configurar el modelo de carga, el número de usuarios, etc.
- [Apache JMeter](https://jmeter.apache.org/) probar.

## <a name="azure-portal"></a>Azure Portal

[Portal de Azure permite configurar y ejecutar las pruebas de carga de aplicaciones Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde la ficha rendimiento del servicio de aplicación de portal de Azure.

![](./load-tests/_static/azure-appservice-perf-test.png)

La prueba puede ser una prueba manual con una dirección URL especificada, o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.

![](./load-tests/_static/azure-appservice-perf-test-config.png)

Al final de la prueba, se generan los informes para mostrar las características de rendimiento de la aplicación. Estadísticas de ejemplo incluyen:

- Tiempo medio de respuesta
- Rendimiento máximo: solicitudes por segundo
- Porcentaje de errores

## <a name="third-party-tools"></a>Herramientas de terceros

En la lista siguiente contiene las herramientas de rendimiento web de terceros con varios conjuntos de características:

- [Apache JMeter](https://jmeter.apache.org/) : Serie destacada completa de herramientas de pruebas de carga. Subproceso enlazados: necesita un subproceso por cada usuario.
- [AB - servidor Apache HTTP herramienta de comparación de rendimiento](https://httpd.apache.org/docs/2.4/programs/ab.html)
- [Gatling](https://gatling.io/) : Herramienta de escritorio con un grabadores de GUI y de prueba. Mayor rendimiento que JMeter.
- [Locust.IO](https://locust.io/) : No está limitado por los subprocesos.

<a name="add"></a>
## <a name="additional-resources"></a>Recursos adicionales

[Serie de blogs de prueba de carga](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Charles Sterling. Con fecha, pero la mayoría de los temas siguen siendo pertinente.
