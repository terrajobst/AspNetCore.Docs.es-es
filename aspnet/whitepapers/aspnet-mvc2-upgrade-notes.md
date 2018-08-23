---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2 | Microsoft Docs
author: rick-anderson
description: Este documento explica cómo actualizar manualmente y con un Asistente para una aplicación ASP.NET MVC 1.0 a ASP.NET MVC 2. Este documento también está disponible para d...
ms.author: riande
ms.date: 04/08/2010
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: 3de69df7e80037de35c2609232f4574bc9d03c80
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835973"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>Actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2
====================
> Este documento explica cómo actualizar manualmente y con un Asistente para una aplicación ASP.NET MVC 1.0 a ASP.NET MVC 2. Este documento también está disponible para [descargar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>Introducción

ASP.NET MVC 2 se puede instalar en paralelo con ASP.NET MVC 1.0 en el mismo servidor. Esto proporciona flexibilidad a los desarrolladores de aplicaciones en elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2.

Visual Studio 2010 incluye a un asistente que las actualizaciones de los proyectos de ASP.NET MVC 1.0 existentes creados con Visual Studio 2008 para ASP.NET MVC 2. Se inicia el Asistente para actualización, abra un proyecto de ASP.NET MVC 1.0 en Visual Studio 2010.

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Actualizar el Asistente para ASP.NET MVC 1.0 en Visual Studio 2008 SP1

Para actualizar una aplicación de ASP.NET MVC 1.0 a ASP.NET MVC 2 en Visual Studio 2008 SP1, use la aplicación MvcAppConverter (no compatible). Puede descargar esta aplicación desde la dirección URL siguiente:

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>Actualizar manualmente un proyecto de ASP.NET MVC 1.0

Para actualizar manualmente una aplicación existente de ASP.NET MVC 1.0 a la versión 2, siga estos pasos:

1. Realice una copia de seguridad del proyecto existente.
2. En un editor de texto, abra el archivo de proyecto (el archivo con la extensión de archivo .csproj o .vbproj) y busque el elemento ProjectTypeGuid. Como el valor de ese elemento, reemplace el GUID {603c0e0b-db56-11dc-be95-000d561079b0} con {F85E285D-A4E0-4152-9332-AB1D724D3325}. Cuando haya terminado, el valor de ese elemento debe ser como sigue: 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. En la carpeta raíz de aplicación Web, edite el archivo Web.config. Busque System.Web.Mvc, versión = 1.0.0.0 y reemplace todas las instancias con System.Web.Mvc, versión = 2.0.0.0.
4. Repita el paso anterior para el archivo Web.config ubicado en la carpeta Views.
5. Abra el proyecto con Visual Studio y en **el Explorador de soluciones**, expanda el **referencias** nodo. Elimine la referencia a System.Web.Mvc (que apunta al ensamblado de la versión 1.0). Agregue una referencia a System.Web.Mvc (v2.0.0.0).
6. Agregue el siguiente elemento bindingRedirect al archivo Web.config en la raíz de la aplicación en la sección de configuración:   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. Cree una nueva aplicación vacía de ASP.NET MVC 2. Copie los archivos de la carpeta de Scripts de la nueva aplicación en la carpeta de Scripts de la aplicación existente.
8. Actualizar la existente es decir™ archivo CSS de s con las definiciones de estilos CSS en el archivo Site.css.
9. Compile la aplicación y ejecutarla. Si se produce algún error, consulte la sección cambios importantes de la [Novedades de ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) página.
