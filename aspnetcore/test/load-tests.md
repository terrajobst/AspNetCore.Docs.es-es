---
title: Las pruebas de carga o esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Obtenga información sobre varios importantes herramientas y enfoques de pruebas de carga y las aplicaciones ASP.NET Core de prueba de carga.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: test/loadtests
ms.openlocfilehash: 0a8449ea2c9df0f2ac93058f03af0a1a2aa66508
ms.sourcegitcommit: 78339e9891c8676db01a6e81e9cb0cdaa280162f
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/17/2019
ms.locfileid: "59068188"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="d3df4-103">Las pruebas de carga o esfuerzo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3df4-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="d3df4-104">Pruebas de carga y pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable.</span><span class="sxs-lookup"><span data-stu-id="d3df4-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="d3df4-105">Sus objetivos son diferentes, aunque a menudo comparten pruebas similares.</span><span class="sxs-lookup"><span data-stu-id="d3df4-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="d3df4-106">**Las pruebas de carga** &ndash; comprobar si la aplicación puede controlar una carga de usuarios para un determinado escenario especificado sin dejar de satisfacer el objetivo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="d3df4-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="d3df4-107">La aplicación se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="d3df4-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="d3df4-108">**Las pruebas de esfuerzo** &ndash; probar la estabilidad de la aplicación cuando se ejecuta en condiciones extremas, con frecuencia durante un largo período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="d3df4-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="d3df4-109">Las pruebas colocan gran carga de usuarios, los picos o incrementar gradualmente la carga, en la aplicación o limitan los recursos informáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d3df4-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="d3df4-110">Las pruebas de esfuerzo determinan si una aplicación en situaciones de estrés puede recuperarse de errores y volver correctamente al comportamiento esperado.</span><span class="sxs-lookup"><span data-stu-id="d3df4-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="d3df4-111">En situaciones de estrés, la aplicación no se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="d3df4-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="d3df4-112">2019 de Visual Studio es la última versión de Visual Studio con las funciones de prueba de carga.</span><span class="sxs-lookup"><span data-stu-id="d3df4-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="d3df4-113">Para los clientes que requieren en el futuro de las herramientas de prueba de carga, se recomienda herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="d3df4-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="d3df4-114">Para obtener más información, consulte el [notas de la versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes#test-tools).</span><span class="sxs-lookup"><span data-stu-id="d3df4-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes#test-tools).</span></span>

<span data-ttu-id="d3df4-115">Finaliza la prueba de carga service en Azure DevOps en 2020.</span><span class="sxs-lookup"><span data-stu-id="d3df4-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="d3df4-116">Para obtener más información, consulte [prueba final del servicio del ciclo de vida de carga basado en la nube](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="d3df4-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="d3df4-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="d3df4-117">Visual Studio tools</span></span>

<span data-ttu-id="d3df4-118">Visual Studio permite a los usuarios crear, desarrollar y depurar las pruebas de carga y rendimiento web.</span><span class="sxs-lookup"><span data-stu-id="d3df4-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="d3df4-119">Una opción está disponible para crear las pruebas mediante la grabación de acciones en un explorador web.</span><span class="sxs-lookup"><span data-stu-id="d3df4-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="d3df4-120">Para obtener información sobre cómo crear, configurar y ejecutar una prueba de carga de proyectos con Visual Studio 2017, consulte [inicio rápido: Creación de un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="d3df4-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span> <span data-ttu-id="d3df4-121">Para obtener más información, consulte el [recursos adicionales](#additional-resources) sección.</span><span class="sxs-lookup"><span data-stu-id="d3df4-121">For more information, see the [Additional resources](#additional-resources) section.</span></span>

<span data-ttu-id="d3df4-122">Las pruebas de carga pueden configurarse para ejecutarse de forma local o en ejecución en la nube con Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="d3df4-122">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="d3df4-123">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="d3df4-123">Azure DevOps</span></span>

<span data-ttu-id="d3df4-124">Se pueden iniciar ejecuciones de pruebas de carga con el [planes de prueba de Azure DevOps](/azure/devops/test/load-test/index?view=vsts) service.</span><span class="sxs-lookup"><span data-stu-id="d3df4-124">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Página de aterrizaje de pruebas de carga de Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="d3df4-126">El servicio admite los siguientes formatos de prueba:</span><span class="sxs-lookup"><span data-stu-id="d3df4-126">The service supports the following test formats:</span></span>

* <span data-ttu-id="d3df4-127">Visual Studio &ndash; prueba Web creado en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d3df4-127">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="d3df4-128">Archivo HTTP &ndash; tráfico HTTP capturado dentro del archivo se reproduce durante las pruebas.</span><span class="sxs-lookup"><span data-stu-id="d3df4-128">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="d3df4-129">[Basado en URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; permite especificar direcciones URL para la carga de prueba, tipos de solicitud, los encabezados y las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="d3df4-129">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="d3df4-130">Ejecutar configuración de los parámetros como la duración, modelo de carga y el número de usuarios pueden configurarse.</span><span class="sxs-lookup"><span data-stu-id="d3df4-130">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="d3df4-131">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="d3df4-131">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="d3df4-132">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3df4-132">Azure portal</span></span>

<span data-ttu-id="d3df4-133">[Portal de Azure permite configurar y ejecutar las pruebas de carga de aplicaciones web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde el **rendimiento** ficha de App Service en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="d3df4-133">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure App Service en Azure portal](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="d3df4-135">La prueba puede ser una prueba manual con una dirección URL especificada o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="d3df4-135">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nueva página de prueba de rendimiento en Azure portal](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="d3df4-137">Al final de la prueba, informes generados muestran las características de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="d3df4-137">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="d3df4-138">Estadísticas de ejemplo incluyen:</span><span class="sxs-lookup"><span data-stu-id="d3df4-138">Example statistics include:</span></span>

* <span data-ttu-id="d3df4-139">Tiempo medio de respuesta</span><span class="sxs-lookup"><span data-stu-id="d3df4-139">Average response time</span></span>
* <span data-ttu-id="d3df4-140">Rendimiento máximo: solicitudes por segundo</span><span class="sxs-lookup"><span data-stu-id="d3df4-140">Max throughput: requests per second</span></span>
* <span data-ttu-id="d3df4-141">Porcentaje de errores</span><span class="sxs-lookup"><span data-stu-id="d3df4-141">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="d3df4-142">Herramientas de terceros</span><span class="sxs-lookup"><span data-stu-id="d3df4-142">Third-party tools</span></span>

<span data-ttu-id="d3df4-143">En la lista siguiente contiene las herramientas de rendimiento web de terceros con varios conjuntos de características:</span><span class="sxs-lookup"><span data-stu-id="d3df4-143">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="d3df4-144">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="d3df4-144">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="d3df4-145">ApacheBench (ab)</span><span class="sxs-lookup"><span data-stu-id="d3df4-145">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="d3df4-146">Gatling</span><span class="sxs-lookup"><span data-stu-id="d3df4-146">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="d3df4-147">Algarrobas</span><span class="sxs-lookup"><span data-stu-id="d3df4-147">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="d3df4-148">West Wind WebSurge</span><span class="sxs-lookup"><span data-stu-id="d3df4-148">West Wind WebSurge</span></span>](http://websurge.west-wind.com/)
* [<span data-ttu-id="d3df4-149">Netling</span><span class="sxs-lookup"><span data-stu-id="d3df4-149">Netling</span></span>](https://github.com/hallatore/Netling)

## <a name="additional-resources"></a><span data-ttu-id="d3df4-150">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="d3df4-150">Additional resources</span></span>

* [<span data-ttu-id="d3df4-151">Serie de blogs de prueba de carga</span><span class="sxs-lookup"><span data-stu-id="d3df4-151">Load Test blog series</span></span>](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/)
