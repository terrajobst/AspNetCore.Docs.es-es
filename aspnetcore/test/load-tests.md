---
title: Las pruebas de carga o esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Describe varios importantes herramientas y enfoques de pruebas de carga y las aplicaciones ASP.NET Core de prueba de carga.
ms.author: riande
ms.custom: mvc
ms.date: 01/04/2019
uid: test/loadtests
ms.openlocfilehash: 0a53405cba19435a74b398ba42a05456c50bdc72
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099485"
---
# <a name="load-and-stress-testing-aspnet-core"></a><span data-ttu-id="17f0a-103">Carga y esfuerzo de pruebas de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="17f0a-103">Load and stress testing ASP.NET Core</span></span>

<span data-ttu-id="17f0a-104">Pruebas de carga y pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable.</span><span class="sxs-lookup"><span data-stu-id="17f0a-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="17f0a-105">Sus objetivos son distintos incluso comparten a menudo pruebas similares.</span><span class="sxs-lookup"><span data-stu-id="17f0a-105">Their goals are different even they often share similar tests.</span></span>

<span data-ttu-id="17f0a-106">**Las pruebas de carga**: Comprueba si la aplicación puede controlar una carga de usuarios para un determinado escenario especificado sin dejar de satisfacer el objetivo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="17f0a-106">**Load tests**: Tests whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="17f0a-107">La aplicación se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="17f0a-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="17f0a-108">**Las pruebas de esfuerzo**: Estabilidad de aplicación de pruebas cuando se ejecutan en condiciones extremas y a menudo un largo período de tiempo:</span><span class="sxs-lookup"><span data-stu-id="17f0a-108">**Stress tests**: Tests app stability when running under extreme conditions and often a long period of time:</span></span>

* <span data-ttu-id="17f0a-109">Carga de usuarios elevado: picos o aumentando gradualmente.</span><span class="sxs-lookup"><span data-stu-id="17f0a-109">High user load – either spikes or gradually increasing.</span></span>
* <span data-ttu-id="17f0a-110">Recursos informáticos limitados.</span><span class="sxs-lookup"><span data-stu-id="17f0a-110">Limited computing resources.</span></span>  

<span data-ttu-id="17f0a-111">¿En situaciones de estrés, puede la aplicación de recuperarse de errores y volver correctamente al comportamiento esperado?</span><span class="sxs-lookup"><span data-stu-id="17f0a-111">Under stress, can the app recover from failure and gracefully return to expected behavior?</span></span> <span data-ttu-id="17f0a-112">La aplicación se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="17f0a-112">The app is run under normal conditions.</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="17f0a-113">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="17f0a-113">Visual Studio Tools</span></span>

<span data-ttu-id="17f0a-114">Visual Studio permite a los usuarios crear, desarrollar y depurar las pruebas de carga y rendimiento web.</span><span class="sxs-lookup"><span data-stu-id="17f0a-114">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="17f0a-115">Una opción está disponible para crear las pruebas mediante la grabación de acciones en el explorador web.</span><span class="sxs-lookup"><span data-stu-id="17f0a-115">An option is available to create tests by recording actions in web browser.</span></span>

<span data-ttu-id="17f0a-116">[Inicio rápido: Crear un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) se muestra cómo crear, configurar y ejecutar una prueba de carga de proyectos con Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="17f0a-116">[Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017) shows how to create, configure, and run a load test projects using Visual Studio 2017.</span></span>

<span data-ttu-id="17f0a-117">Para más información, consulte [Recursos adicionales](#add).</span><span class="sxs-lookup"><span data-stu-id="17f0a-117">See [Additional Resources](#add) for more information.</span></span>

<span data-ttu-id="17f0a-118">Las pruebas de carga pueden configurarse para ejecutar en local o ejecutar en la nube con Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="17f0a-118">Load tests can be configured to run in on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="17f0a-119">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="17f0a-119">Azure DevOps</span></span>

<span data-ttu-id="17f0a-120">Se pueden iniciar ejecuciones de pruebas de carga con el [planes de prueba de Azure DevOps](/azure/devops/test/load-test/index?view=vsts) service.</span><span class="sxs-lookup"><span data-stu-id="17f0a-120">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="17f0a-121">El servicio admite los siguientes tipos de formato de prueba:</span><span class="sxs-lookup"><span data-stu-id="17f0a-121">The service supports the following types of test format:</span></span>

- <span data-ttu-id="17f0a-122">Prueba de Visual Studio: pruebas web creados en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="17f0a-122">Visual Studio test – web test created in Visual Studio.</span></span>
- <span data-ttu-id="17f0a-123">Prueba basada en el almacenamiento HTTP: el tráfico HTTP capturado dentro del archivo se reproduce durante las pruebas.</span><span class="sxs-lookup"><span data-stu-id="17f0a-123">HTTP Archive-based test – captured HTTP traffic inside archive is replayed during testing.</span></span>
- <span data-ttu-id="17f0a-124">[Prueba basada en URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) : permite especificar las direcciones URL para la carga de prueba, tipos de solicitud, los encabezados y las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="17f0a-124">[URL-based test](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) – allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="17f0a-125">Ejecutar configuración de los parámetros como la duración, se puede configurar el modelo de carga, el número de usuarios, etc.</span><span class="sxs-lookup"><span data-stu-id="17f0a-125">Run setting parameters such as duration, load pattern, number of users, etc., can be configured.</span></span>
- <span data-ttu-id="17f0a-126">[Apache JMeter](https://jmeter.apache.org/) probar.</span><span class="sxs-lookup"><span data-stu-id="17f0a-126">[Apache JMeter](https://jmeter.apache.org/) test.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="17f0a-127">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="17f0a-127">Azure portal</span></span>

<span data-ttu-id="17f0a-128">[Portal de Azure permite configurar y ejecutar las pruebas de carga de aplicaciones Web,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde la ficha rendimiento del servicio de aplicación de portal de Azure.</span><span class="sxs-lookup"><span data-stu-id="17f0a-128">[Azure portal allows setting up and running load testing of Web Apps,](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the Performance tab of the App Service in Azure portal.</span></span>

![](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="17f0a-129">La prueba puede ser una prueba manual con una dirección URL especificada, o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="17f0a-129">The test can be a manual test with a specified URL, or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="17f0a-130">Al final de la prueba, se generan los informes para mostrar las características de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="17f0a-130">At end of the test, reports are generated to show the performance characteristics of the app.</span></span> <span data-ttu-id="17f0a-131">Estadísticas de ejemplo incluyen:</span><span class="sxs-lookup"><span data-stu-id="17f0a-131">Example statistics include:</span></span>

- <span data-ttu-id="17f0a-132">Tiempo medio de respuesta</span><span class="sxs-lookup"><span data-stu-id="17f0a-132">Average response time</span></span>
- <span data-ttu-id="17f0a-133">Rendimiento máximo: solicitudes por segundo</span><span class="sxs-lookup"><span data-stu-id="17f0a-133">Max throughput: requests per second</span></span>
- <span data-ttu-id="17f0a-134">Porcentaje de errores</span><span class="sxs-lookup"><span data-stu-id="17f0a-134">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="17f0a-135">Herramientas de terceros</span><span class="sxs-lookup"><span data-stu-id="17f0a-135">Third-party Tools</span></span>

<span data-ttu-id="17f0a-136">En la lista siguiente contiene las herramientas de rendimiento web de terceros con varios conjuntos de características:</span><span class="sxs-lookup"><span data-stu-id="17f0a-136">The following list contains third-party web performance tools with various feature sets:</span></span>

- <span data-ttu-id="17f0a-137">[Apache JMeter](https://jmeter.apache.org/) : Serie destacada completa de herramientas de pruebas de carga.</span><span class="sxs-lookup"><span data-stu-id="17f0a-137">[Apache JMeter](https://jmeter.apache.org/) : Full featured suite of load testing tools.</span></span> <span data-ttu-id="17f0a-138">Subproceso enlazados: necesita un subproceso por cada usuario.</span><span class="sxs-lookup"><span data-stu-id="17f0a-138">Thread-bound: need one thread per user.</span></span>
- [<span data-ttu-id="17f0a-139">AB - servidor Apache HTTP herramienta de comparación de rendimiento</span><span class="sxs-lookup"><span data-stu-id="17f0a-139">ab - Apache HTTP server benchmarking tool</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
- <span data-ttu-id="17f0a-140">[Gatling](https://gatling.io/) : Herramienta de escritorio con un grabadores de GUI y de prueba.</span><span class="sxs-lookup"><span data-stu-id="17f0a-140">[Gatling](https://gatling.io/) : Desktop tool with a GUI and test recorders.</span></span> <span data-ttu-id="17f0a-141">Mayor rendimiento que JMeter.</span><span class="sxs-lookup"><span data-stu-id="17f0a-141">More performant than JMeter.</span></span>
- <span data-ttu-id="17f0a-142">[Locust.IO](https://locust.io/) : No está limitado por los subprocesos.</span><span class="sxs-lookup"><span data-stu-id="17f0a-142">[Locust.io](https://locust.io/) : Not bounded by threads.</span></span>

<a name="add"></a>
## <a name="additional-resources"></a><span data-ttu-id="17f0a-143">Recursos adicionales</span><span class="sxs-lookup"><span data-stu-id="17f0a-143">Additional Resources</span></span>

<span data-ttu-id="17f0a-144">[Serie de blogs de prueba de carga](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) Charles Sterling.</span><span class="sxs-lookup"><span data-stu-id="17f0a-144">[Load Test blog series](https://blogs.msdn.microsoft.com/charles_sterling/2015/06/01/load-test-series-part-i-creating-web-performance-tests-for-a-load-test/) by Charles Sterling.</span></span> <span data-ttu-id="17f0a-145">Con fecha, pero la mayoría de los temas siguen siendo pertinente.</span><span class="sxs-lookup"><span data-stu-id="17f0a-145">Dated but most of the topics are still relevant.</span></span>