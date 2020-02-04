---
title: Pruebas de carga y esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Obtenga información acerca de varias herramientas y enfoques importantes para las pruebas de carga y las pruebas de esfuerzo ASP.NET Core aplicaciones.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: 1fd77a767fb53b9276081dd712e13108094a0382
ms.sourcegitcommit: cb6015f737b6a93127016ab0f21b58e34b624ff3
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "77004297"
---
# <a name="aspnet-core-loadstress-testing"></a>Pruebas de carga y esfuerzo de ASP.NET Core

Las pruebas de carga y las pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable. Sus objetivos son diferentes, aunque a menudo compartan pruebas similares.

**Pruebas de carga** &ndash; comprobar si la aplicación puede controlar una carga específica de usuarios para un escenario determinado mientras sigue cumpliendo el objetivo de respuesta. La aplicación se ejecuta en condiciones normales.

Las **pruebas de esfuerzo** &ndash; la estabilidad de la aplicación de prueba cuando se ejecutan en condiciones extremas, a menudo durante un largo período de tiempo. Las pruebas colocan una carga elevada de usuarios, picos o aumento gradualmente de carga, en la aplicación o limitan los recursos informáticos de la aplicación.

Las pruebas de esfuerzo determinan si una aplicación con estrés puede recuperarse de un error y volver correctamente al comportamiento esperado. En stress, la aplicación no se ejecuta en condiciones normales.

Visual Studio 2019 es la última versión de Visual Studio con características de prueba de carga. Para los clientes que requieren herramientas de prueba de carga en el futuro, se recomienda usar herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter. Para obtener más información, vea las notas de la [versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).

## <a name="visual-studio-tools"></a>Visual Studio Tools

Visual Studio permite a los usuarios crear, desarrollar y depurar pruebas de carga y rendimiento web. Hay disponible una opción para crear pruebas grabando acciones en un explorador Web.

Para obtener información sobre cómo crear, configurar y ejecutar proyectos de prueba de carga con Visual Studio 2017, vea [Inicio rápido: crear un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).

Las pruebas de carga se pueden configurar para ejecutarse de forma local o ejecutarse en la nube con Azure DevOps.

## <a name="third-party-tools"></a>Herramientas de terceros

La lista siguiente contiene herramientas de rendimiento web de terceros con varios conjuntos de características:

* [Apache JMeter](https://jmeter.apache.org/)
* [ApacheBench (AB)](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [Gatling](https://gatling.io/)
* [K6](https://k6.io)
* [Locust](https://locust.io/)
* [Webpico de viento occidental](https://websurge.west-wind.com/)
* [Netling](https://github.com/hallatore/Netling)
* [Vegeta](https://github.com/tsenart/vegeta)

