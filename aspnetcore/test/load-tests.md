---
title: Las pruebas de carga o esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Obtenga información sobre varios importantes herramientas y enfoques de pruebas de carga y las aplicaciones ASP.NET Core de prueba de carga.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 3c21da6c799bc3080a1a16cb62ae4535b8890a1b
ms.sourcegitcommit: bee530454ae2b3c25dc7ffebf93536f479a14460
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/10/2019
ms.locfileid: "67724490"
---
# <a name="aspnet-core-loadstress-testing"></a>Las pruebas de carga o esfuerzo de ASP.NET Core

Pruebas de carga y pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable. Sus objetivos son diferentes, aunque a menudo comparten pruebas similares.

**Las pruebas de carga** &ndash; comprobar si la aplicación puede controlar una carga de usuarios para un determinado escenario especificado sin dejar de satisfacer el objetivo de respuesta. La aplicación se ejecuta en condiciones normales.

**Las pruebas de esfuerzo** &ndash; probar la estabilidad de la aplicación cuando se ejecuta en condiciones extremas, con frecuencia durante un largo período de tiempo. Las pruebas colocan gran carga de usuarios, los picos o incrementar gradualmente la carga, en la aplicación o limitan los recursos informáticos de la aplicación.

Las pruebas de esfuerzo determinan si una aplicación en situaciones de estrés puede recuperarse de errores y volver correctamente al comportamiento esperado. En situaciones de estrés, la aplicación no se ejecuta en condiciones normales.

2019 de Visual Studio es la última versión de Visual Studio con las funciones de prueba de carga. Para los clientes que requieren en el futuro de las herramientas de prueba de carga, se recomienda herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter. Para obtener más información, consulte el [notas de la versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).

Finaliza la prueba de carga service en Azure DevOps en 2020. Para obtener más información, consulte [prueba final del servicio del ciclo de vida de carga basado en la nube](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permite a los usuarios crear, desarrollar y depurar las pruebas de carga y rendimiento web. Una opción está disponible para crear las pruebas mediante la grabación de acciones en un explorador web.

Para obtener información sobre cómo crear, configurar y ejecutar una prueba de carga de proyectos con Visual Studio 2017, consulte [inicio rápido: Creación de un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017). Para obtener más información, consulte el [recursos adicionales](#additional-resources) sección.

Las pruebas de carga pueden configurarse para ejecutarse de forma local o en ejecución en la nube con Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Se pueden iniciar ejecuciones de pruebas de carga con el [planes de prueba de Azure DevOps](/azure/devops/test/load-test/index?view=vsts) service.

![Página de aterrizaje de pruebas de carga de Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

El servicio admite los siguientes formatos de prueba:

* Visual Studio &ndash; prueba Web creado en Visual Studio.
* Archivo HTTP &ndash; tráfico HTTP capturado dentro del archivo se reproduce durante las pruebas.
* [Basado en URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; permite especificar direcciones URL para la carga de prueba, tipos de solicitud, los encabezados y las cadenas de consulta. Ejecutar configuración de los parámetros como la duración, modelo de carga y el número de usuarios pueden configurarse.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure Portal

[Portal de Azure permite configurar y ejecutar las pruebas de carga de aplicaciones web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde el **rendimiento** ficha de App Service en Azure portal.

![Azure App Service en Azure portal](./load-tests/_static/azure-appservice-perf-test.png)

La prueba puede ser una prueba manual con una dirección URL especificada o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.

![Nueva página de prueba de rendimiento en Azure portal](./load-tests/_static/azure-appservice-perf-test-config.png)

Al final de la prueba, informes generados muestran las características de rendimiento de la aplicación. Estadísticas de ejemplo incluyen:

* Tiempo medio de respuesta
* Rendimiento máximo: solicitudes por segundo
* Porcentaje de errores

## <a name="third-party-tools"></a>Herramientas de terceros

En la lista siguiente contiene las herramientas de rendimiento web de terceros con varios conjuntos de características:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (ab)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Algarrobas](https://locust.io/)
* [West Wind WebSurge](http://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)

## <a name="additional-resources"></a>Recursos adicionales

* [Serie de blogs de prueba de carga](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
