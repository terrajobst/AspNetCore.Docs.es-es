---
title: Pruebas de carga y esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Obtenga información acerca de varias herramientas y enfoques importantes para las pruebas de carga y las pruebas de esfuerzo ASP.NET Core aplicaciones.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 7a9dfc1fedf747ab26daa573b61ed01c31709058
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975240"
---
# <a name="aspnet-core-loadstress-testing"></a>Pruebas de carga y esfuerzo de ASP.NET Core

Las pruebas de carga y las pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable. Sus objetivos son diferentes, aunque a menudo compartan pruebas similares.

**Pruebas de carga** &ndash; Compruebe si la aplicación puede controlar una carga específica de usuarios para un escenario determinado mientras sigue cumpliendo el objetivo de respuesta. La aplicación se ejecuta en condiciones normales.

**Pruebas de esfuerzo** &ndash; Pruebe la estabilidad de la aplicación cuando se ejecute en condiciones extremas, a menudo durante un largo período de tiempo. Las pruebas colocan una carga elevada de usuarios, picos o aumento gradualmente de carga, en la aplicación o limitan los recursos informáticos de la aplicación.

Las pruebas de esfuerzo determinan si una aplicación con estrés puede recuperarse de un error y volver correctamente al comportamiento esperado. En stress, la aplicación no se ejecuta en condiciones normales.

Visual Studio 2019 es la última versión de Visual Studio con características de prueba de carga. Para los clientes que requieren herramientas de prueba de carga en el futuro, se recomienda usar herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter. Para obtener más información, vea las notas de la [versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

El servicio de pruebas de carga de Azure DevOps está terminando en 2020. Para obtener más información, vea [final de la vida del servicio de pruebas de carga basadas en la nube](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permite a los usuarios crear, desarrollar y depurar pruebas de carga y rendimiento web. Hay disponible una opción para crear pruebas grabando acciones en un explorador Web.

Para obtener información sobre cómo crear, configurar y ejecutar proyectos de prueba de carga con Visual Studio 2017, consulte [Inicio rápido: Creación de un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

Las pruebas de carga se pueden configurar para ejecutarse de forma local o ejecutarse en la nube con Azure DevOps.

## <a name="azure-devops"></a>Azure DevOps

Las ejecuciones de pruebas de carga se pueden iniciar mediante el servicio [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) .

![Página de aterrizaje de pruebas de carga de Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

El servicio admite los siguientes formatos de prueba:

* Prueba Web &ndash; de Visual Studio creada en Visual Studio.
* El archivo &ndash; http capturado el tráfico http dentro del archivo se reproduce durante las pruebas.
* [Basado en dirección URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Permite especificar direcciones URL para la prueba de carga, los tipos de solicitud, los encabezados y las cadenas de consulta. Se pueden configurar parámetros de configuración de ejecución como la duración, el modelo de carga y el número de usuarios.
* [Apache JMeter](https://jmeter.apache.org/).

## <a name="azure-portal"></a>Azure Portal

[Azure portal permite configurar y ejecutar pruebas de carga de aplicaciones web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde la pestaña **rendimiento** del App Service en Azure portal.

![Azure App Service en Azure Portal](./load-tests/_static/azure-appservice-perf-test.png)

La prueba puede ser una prueba manual con una dirección URL especificada o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.

![Nueva página de prueba de rendimiento en Azure Portal](./load-tests/_static/azure-appservice-perf-test-config.png)

Al final de la prueba, los informes generados muestran las características de rendimiento de la aplicación. Las estadísticas de ejemplo incluyen:

* Tiempo medio de respuesta
* Rendimiento máximo: solicitudes por segundo
* Porcentaje de errores

## <a name="third-party-tools"></a>Herramientas de terceros

La lista siguiente contiene herramientas de rendimiento web de terceros con varios conjuntos de características:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [Locust](https://locust.io/)
* [Webpico de viento occidental](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)
