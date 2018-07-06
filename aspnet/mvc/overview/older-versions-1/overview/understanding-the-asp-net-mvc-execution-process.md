---
uid: mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
title: Descripción del proceso de ejecución de ASP.NET MVC | Microsoft Docs
author: microsoft
description: Obtenga información sobre cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso.
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: d1608db3-660d-4079-8c15-f452ff01f1db
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-the-asp-net-mvc-execution-process
msc.type: authoredcontent
ms.openlocfilehash: 34699c314eb862e91ecba8831c5092af9d47dc70
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37823740"
---
<a name="understanding-the-aspnet-mvc-execution-process"></a>Descripción del proceso de ejecución de ASP.NET MVC
====================
por [Microsoft](https://github.com/microsoft)

> Obtenga información sobre cómo el marco de MVC de ASP.NET procesa una solicitud de explorador paso a paso.


Las solicitudes a una aplicación Web basada en ASP.NET MVC que primero pasan por la **UrlRoutingModule** objeto, que es un módulo HTTP. Este módulo analiza la solicitud y realiza la selección de la ruta. El **UrlRoutingModule** objeto selecciona el primer objeto de ruta que coincida con la solicitud actual. (Un objeto de ruta es una clase que implementa **RouteBase**, y normalmente es una instancia de la **ruta** clase.) Si ninguna ruta coincide, el **UrlRoutingModule** objeto no hace nada y permite que la solicitud recurrir a la solicitud ASP.NET o IIS normal al procesamiento.

Desde seleccionado **ruta** objeto, el **UrlRoutingModule** objeto obtiene la **IRouteHandler** objeto que está asociado el **ruta**objeto. Normalmente, en una aplicación MVC, esto será una instancia de **MvcRouteHandler**. El **IRouteHandler** instancia crea un **IHttpHandler** objeto y lo pasa la **IHttpContext** objeto. De forma predeterminada, el **IHttpHandler** instancia para MVC es el **MvcHandler** objeto. El **MvcHandler** objeto, a continuación, selecciona el controlador que en última instancia controlará la solicitud.

> [!NOTE]
> Cuando se ejecuta una aplicación Web de MVC de ASP.NET en IIS 7.0, extensión de nombre de archivo no es necesaria para los proyectos de MVC. Sin embargo, en IIS 6.0, el controlador requiere que asigne la extensión de nombre de archivo .mvc a la DLL de ISAPI de ASP.NET.


El módulo y el controlador son los puntos de entrada para el marco de MVC de ASP.NET. Se realizan las siguientes acciones:

- Seleccionar el controlador adecuado en una aplicación Web MVC.
- Obtener una instancia de un controlador específico.
- Llamar a del controlador **Execute** método.

A continuación enumeran las fases de ejecución para un proyecto Web de MVC:

- Recibir la primera solicitud para la aplicación 

    - En el archivo Global.asax, **ruta** se agregan objetos a la **RouteTable** objeto.
- Realizar el enrutamiento 

    - El **UrlRoutingModule** módulo usa la primera coincidencia **ruta** objeto en el **RouteTable** colección para crear el **RouteData** objeto que usa entonces para crear un **RequestContext** (**IHttpContext**) objetos.
- Crear el controlador de solicitudes de MVC 

    - El **MvcRouteHandler** objeto crea una instancia de la **MvcHandler** clase y le pasa el **RequestContext** instancia.
- Crear el controlador 

    - El **MvcHandler** de objeto usa la **RequestContext** instancia para identificar el **IControllerFactory** objeto (normalmente una instancia de la  **DefaultControllerFactory** clase) para crear la instancia del controlador.
- Controlador Execute: el **MvcHandler** instancia invoca el controlador s **Execute** método. |
- Invocar acción 

    - La mayoría de los controladores hereda de la **controlador** clase base. Para los controladores que lo haga, la **ControllerActionInvoker** objeto que está asociado con el controlador determina qué método de acción de la clase de controlador para llamar a y, a continuación, llama a ese método.
- Resultado de ejecución 

    - Un método de acción típico podría recibir entradas del usuario, preparar los datos de la respuesta adecuada y, a continuación, ejecute el resultado devolviendo un tipo de resultado. Los tipos de resultado integrados que se pueden ejecutar incluyen los siguientes: **ViewResult** (que presenta una vista y es el tipo de resultado utilizado con mayor frecuencia), **RedirectToRouteResult**,  **RedirectResult**, **ContentResult**, **JsonResult**, y **EmptyResult**.
