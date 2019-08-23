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
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="a8ca0-103">Pruebas de carga y esfuerzo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8ca0-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="a8ca0-104">Las pruebas de carga y las pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="a8ca0-105">Sus objetivos son diferentes, aunque a menudo compartan pruebas similares.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="a8ca0-106">**Pruebas de carga** &ndash; Compruebe si la aplicación puede controlar una carga específica de usuarios para un escenario determinado mientras sigue cumpliendo el objetivo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="a8ca0-107">La aplicación se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="a8ca0-108">**Pruebas de esfuerzo** &ndash; Pruebe la estabilidad de la aplicación cuando se ejecute en condiciones extremas, a menudo durante un largo período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="a8ca0-109">Las pruebas colocan una carga elevada de usuarios, picos o aumento gradualmente de carga, en la aplicación o limitan los recursos informáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="a8ca0-110">Las pruebas de esfuerzo determinan si una aplicación con estrés puede recuperarse de un error y volver correctamente al comportamiento esperado.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="a8ca0-111">En stress, la aplicación no se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="a8ca0-112">Visual Studio 2019 es la última versión de Visual Studio con características de prueba de carga.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="a8ca0-113">Para los clientes que requieren herramientas de prueba de carga en el futuro, se recomienda usar herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="a8ca0-114">Para obtener más información, vea las notas de la [versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="a8ca0-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

<span data-ttu-id="a8ca0-115">El servicio de pruebas de carga de Azure DevOps está terminando en 2020.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-115">The load testing service in Azure DevOps is ending in 2020.</span></span> <span data-ttu-id="a8ca0-116">Para obtener más información, vea [final de la vida del servicio de pruebas de carga basadas en la nube](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span><span class="sxs-lookup"><span data-stu-id="a8ca0-116">For more information, see [Cloud-based load testing service end of life](https://devblogs.microsoft.com/devops/cloud-based-load-testing-service-eol/).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="a8ca0-117">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="a8ca0-117">Visual Studio tools</span></span>

<span data-ttu-id="a8ca0-118">Visual Studio permite a los usuarios crear, desarrollar y depurar pruebas de carga y rendimiento web.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-118">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="a8ca0-119">Hay disponible una opción para crear pruebas grabando acciones en un explorador Web.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-119">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="a8ca0-120">Para obtener información sobre cómo crear, configurar y ejecutar proyectos de prueba de carga con Visual Studio 2017, consulte [Inicio rápido: Creación de un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="a8ca0-120">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="a8ca0-121">Las pruebas de carga se pueden configurar para ejecutarse de forma local o ejecutarse en la nube con Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-121">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="azure-devops"></a><span data-ttu-id="a8ca0-122">Azure DevOps</span><span class="sxs-lookup"><span data-stu-id="a8ca0-122">Azure DevOps</span></span>

<span data-ttu-id="a8ca0-123">Las ejecuciones de pruebas de carga se pueden iniciar mediante el servicio [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) .</span><span class="sxs-lookup"><span data-stu-id="a8ca0-123">Load test runs can be started using the [Azure DevOps Test Plans](/azure/devops/test/load-test/index?view=vsts) service.</span></span>

![Página de aterrizaje de pruebas de carga de Azure DevOps](./load-tests/_static/azure-devops-load-test.png)

<span data-ttu-id="a8ca0-125">El servicio admite los siguientes formatos de prueba:</span><span class="sxs-lookup"><span data-stu-id="a8ca0-125">The service supports the following test formats:</span></span>

* <span data-ttu-id="a8ca0-126">Prueba Web &ndash; de Visual Studio creada en Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-126">Visual Studio &ndash; Web test created in Visual Studio.</span></span>
* <span data-ttu-id="a8ca0-127">El archivo &ndash; http capturado el tráfico http dentro del archivo se reproduce durante las pruebas.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-127">HTTP Archive &ndash; Captured HTTP traffic inside archive is replayed during testing.</span></span>
* <span data-ttu-id="a8ca0-128">[Basado en dirección URL](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Permite especificar direcciones URL para la prueba de carga, los tipos de solicitud, los encabezados y las cadenas de consulta.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-128">[URL-based](/azure/devops/test/load-test/get-started-simple-cloud-load-test?view=vsts) &ndash; Allows specifying URLs to load test, request types, headers, and query strings.</span></span> <span data-ttu-id="a8ca0-129">Se pueden configurar parámetros de configuración de ejecución como la duración, el modelo de carga y el número de usuarios.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-129">Run setting parameters such as duration, load pattern, and number of users can be configured.</span></span>
* <span data-ttu-id="a8ca0-130">[Apache JMeter](https://jmeter.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a8ca0-130">[Apache JMeter](https://jmeter.apache.org/).</span></span>

## <a name="azure-portal"></a><span data-ttu-id="a8ca0-131">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a8ca0-131">Azure portal</span></span>

<span data-ttu-id="a8ca0-132">[Azure portal permite configurar y ejecutar pruebas de carga de aplicaciones web](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directamente desde la pestaña **rendimiento** del App Service en Azure portal.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-132">[Azure portal allows setting up and running load testing of web apps](/azure/devops/test/load-test/app-service-web-app-performance-test?view=vsts) directly from the **Performance** tab of the App Service in Azure portal.</span></span>

![Azure App Service en Azure Portal](./load-tests/_static/azure-appservice-perf-test.png)

<span data-ttu-id="a8ca0-134">La prueba puede ser una prueba manual con una dirección URL especificada o un archivo de prueba Web de Visual Studio, que puede probar varias direcciones URL.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-134">The test can be a manual test with a specified URL or a Visual Studio Web Test file, which can test multiple URLs.</span></span>

![Nueva página de prueba de rendimiento en Azure Portal](./load-tests/_static/azure-appservice-perf-test-config.png)

<span data-ttu-id="a8ca0-136">Al final de la prueba, los informes generados muestran las características de rendimiento de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="a8ca0-136">At end of the test, generated reports show the performance characteristics of the app.</span></span> <span data-ttu-id="a8ca0-137">Las estadísticas de ejemplo incluyen:</span><span class="sxs-lookup"><span data-stu-id="a8ca0-137">Example statistics include:</span></span>

* <span data-ttu-id="a8ca0-138">Tiempo medio de respuesta</span><span class="sxs-lookup"><span data-stu-id="a8ca0-138">Average response time</span></span>
* <span data-ttu-id="a8ca0-139">Rendimiento máximo: solicitudes por segundo</span><span class="sxs-lookup"><span data-stu-id="a8ca0-139">Max throughput: requests per second</span></span>
* <span data-ttu-id="a8ca0-140">Porcentaje de errores</span><span class="sxs-lookup"><span data-stu-id="a8ca0-140">Failure percentage</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="a8ca0-141">Herramientas de terceros</span><span class="sxs-lookup"><span data-stu-id="a8ca0-141">Third-party tools</span></span>

<span data-ttu-id="a8ca0-142">La lista siguiente contiene herramientas de rendimiento web de terceros con varios conjuntos de características:</span><span class="sxs-lookup"><span data-stu-id="a8ca0-142">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="a8ca0-143">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="a8ca0-143">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="a8ca0-144">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="a8ca0-144">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="a8ca0-145">Gatling</span><span class="sxs-lookup"><span data-stu-id="a8ca0-145">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="a8ca0-146">Locust</span><span class="sxs-lookup"><span data-stu-id="a8ca0-146">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="a8ca0-147">Webpico de viento occidental</span><span class="sxs-lookup"><span data-stu-id="a8ca0-147">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="a8ca0-148">Netling</span><span class="sxs-lookup"><span data-stu-id="a8ca0-148">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="a8ca0-149">Vegeta</span><span class="sxs-lookup"><span data-stu-id="a8ca0-149">Vegeta</span></span>](https://github.com/tsenart/vegeta)
