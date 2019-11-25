---
title: Pruebas de carga y esfuerzo de ASP.NET Core
author: Jeremy-Meng
description: Obtenga información acerca de varias herramientas y enfoques importantes para las pruebas de carga y las pruebas de esfuerzo ASP.NET Core aplicaciones.
ms.author: riande
ms.custom: mvc
ms.date: 4/05/2019
uid: test/loadtests
ms.openlocfilehash: edaa9e873e8e489f0c560c1736f81358ca1720d0
ms.sourcegitcommit: f40c9311058c9b1add4ec043ddc5629384af6c56
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2019
ms.locfileid: "74289021"
---
# <a name="aspnet-core-loadstress-testing"></a><span data-ttu-id="2b18a-103">Pruebas de carga y esfuerzo de ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2b18a-103">ASP.NET Core load/stress testing</span></span>

<span data-ttu-id="2b18a-104">Las pruebas de carga y las pruebas de esfuerzo son importantes para asegurarse de que una aplicación web es eficaz y escalable.</span><span class="sxs-lookup"><span data-stu-id="2b18a-104">Load testing and stress testing are important to ensure a web app is performant and scalable.</span></span> <span data-ttu-id="2b18a-105">Sus objetivos son diferentes, aunque a menudo compartan pruebas similares.</span><span class="sxs-lookup"><span data-stu-id="2b18a-105">Their goals are different even though they often share similar tests.</span></span>

<span data-ttu-id="2b18a-106">**Pruebas de carga** &ndash; comprobar si la aplicación puede controlar una carga específica de usuarios para un escenario determinado mientras sigue cumpliendo el objetivo de respuesta.</span><span class="sxs-lookup"><span data-stu-id="2b18a-106">**Load tests** &ndash; Test whether the app can handle a specified load of users for a certain scenario while still satisfying the response goal.</span></span> <span data-ttu-id="2b18a-107">La aplicación se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="2b18a-107">The app is run under normal conditions.</span></span>

<span data-ttu-id="2b18a-108">Las **pruebas de esfuerzo** &ndash; la estabilidad de la aplicación de prueba cuando se ejecutan en condiciones extremas, a menudo durante un largo período de tiempo.</span><span class="sxs-lookup"><span data-stu-id="2b18a-108">**Stress tests** &ndash; Test app stability when running under extreme conditions, often for a long period of time.</span></span> <span data-ttu-id="2b18a-109">Las pruebas colocan una carga elevada de usuarios, picos o aumento gradualmente de carga, en la aplicación o limitan los recursos informáticos de la aplicación.</span><span class="sxs-lookup"><span data-stu-id="2b18a-109">The tests place high user load, either spikes or gradually increasing load, on the app, or they limit the app's computing resources.</span></span>

<span data-ttu-id="2b18a-110">Las pruebas de esfuerzo determinan si una aplicación con estrés puede recuperarse de un error y volver correctamente al comportamiento esperado.</span><span class="sxs-lookup"><span data-stu-id="2b18a-110">Stress tests determine if an app under stress can recover from failure and gracefully return to expected behavior.</span></span> <span data-ttu-id="2b18a-111">En stress, la aplicación no se ejecuta en condiciones normales.</span><span class="sxs-lookup"><span data-stu-id="2b18a-111">Under stress, the app isn't run under normal conditions.</span></span>

<span data-ttu-id="2b18a-112">Visual Studio 2019 es la última versión de Visual Studio con características de prueba de carga.</span><span class="sxs-lookup"><span data-stu-id="2b18a-112">Visual Studio 2019 is the last version of Visual Studio with load test features.</span></span> <span data-ttu-id="2b18a-113">Para los clientes que requieren herramientas de prueba de carga en el futuro, se recomienda usar herramientas alternativas, como Apache JMeter, Akamai CloudTest y BlazeMeter.</span><span class="sxs-lookup"><span data-stu-id="2b18a-113">For customers requiring load testing tools in the future, we recommend alternate tools, such as Apache JMeter, Akamai CloudTest, and BlazeMeter.</span></span> <span data-ttu-id="2b18a-114">Para obtener más información, vea las notas de la [versión de Visual Studio 2019](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span><span class="sxs-lookup"><span data-stu-id="2b18a-114">For more information, see the [Visual Studio 2019 Release Notes](/visualstudio/releases/2019/release-notes-v16.0#test-tools).</span></span>

## <a name="visual-studio-tools"></a><span data-ttu-id="2b18a-115">Visual Studio Tools</span><span class="sxs-lookup"><span data-stu-id="2b18a-115">Visual Studio tools</span></span>

<span data-ttu-id="2b18a-116">Visual Studio permite a los usuarios crear, desarrollar y depurar pruebas de carga y rendimiento web.</span><span class="sxs-lookup"><span data-stu-id="2b18a-116">Visual Studio allows users to create, develop, and debug web performance and load tests.</span></span> <span data-ttu-id="2b18a-117">Hay disponible una opción para crear pruebas grabando acciones en un explorador Web.</span><span class="sxs-lookup"><span data-stu-id="2b18a-117">An option is available to create tests by recording actions in a web browser.</span></span>

<span data-ttu-id="2b18a-118">Para obtener información sobre cómo crear, configurar y ejecutar proyectos de prueba de carga con Visual Studio 2017, vea [Inicio rápido: crear un proyecto de prueba de carga](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span><span class="sxs-lookup"><span data-stu-id="2b18a-118">For information on how to create, configure, and run a load test projects using Visual Studio 2017, see [Quickstart: Create a load test project](/visualstudio/test/quickstart-create-a-load-test-project?view=vs-2017).</span></span>

<span data-ttu-id="2b18a-119">Las pruebas de carga se pueden configurar para ejecutarse de forma local o ejecutarse en la nube con Azure DevOps.</span><span class="sxs-lookup"><span data-stu-id="2b18a-119">Load tests can be configured to run on-premise or run in the cloud using Azure DevOps.</span></span>

## <a name="third-party-tools"></a><span data-ttu-id="2b18a-120">Herramientas de terceros</span><span class="sxs-lookup"><span data-stu-id="2b18a-120">Third-party tools</span></span>

<span data-ttu-id="2b18a-121">La lista siguiente contiene herramientas de rendimiento web de terceros con varios conjuntos de características:</span><span class="sxs-lookup"><span data-stu-id="2b18a-121">The following list contains third-party web performance tools with various feature sets:</span></span>

* [<span data-ttu-id="2b18a-122">Apache JMeter</span><span class="sxs-lookup"><span data-stu-id="2b18a-122">Apache JMeter</span></span>](https://jmeter.apache.org/)
* [<span data-ttu-id="2b18a-123">ApacheBench (AB)</span><span class="sxs-lookup"><span data-stu-id="2b18a-123">ApacheBench (ab)</span></span>](https://httpd.apache.org/docs/2.4/programs/ab.html)
* [<span data-ttu-id="2b18a-124">Gatling</span><span class="sxs-lookup"><span data-stu-id="2b18a-124">Gatling</span></span>](https://gatling.io/)
* [<span data-ttu-id="2b18a-125">Locust</span><span class="sxs-lookup"><span data-stu-id="2b18a-125">Locust</span></span>](https://locust.io/)
* [<span data-ttu-id="2b18a-126">Webpico de viento occidental</span><span class="sxs-lookup"><span data-stu-id="2b18a-126">West Wind WebSurge</span></span>](https://websurge.west-wind.com/)
* [<span data-ttu-id="2b18a-127">Netling</span><span class="sxs-lookup"><span data-stu-id="2b18a-127">Netling</span></span>](https://github.com/hallatore/Netling)
* [<span data-ttu-id="2b18a-128">Vegeta</span><span class="sxs-lookup"><span data-stu-id="2b18a-128">Vegeta</span></span>](https://github.com/tsenart/vegeta)
