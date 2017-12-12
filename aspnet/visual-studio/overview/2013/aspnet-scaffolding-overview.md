---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding de ASP.NET en Visual Studio 2013 | Documentos de Microsoft
author: tfitzmac
description: "Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/09/2014
ms.topic: article
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: 425983c1ffff6369276f0723a9947a411a4617eb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding de ASP.NET en Visual Studio 2013
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.


## <a name="overview"></a>Información general

La técnica de Scaffolding de ASP.NET es un marco de trabajo de generación de código para aplicaciones Web ASP.NET. Visual Studio 2013 incluye generadores de código previamente instalados para los proyectos de MVC y API Web. Agregar scaffolding al proyecto cuando desee agregar rápidamente código que interactúe con modelos de datos. Mediante scaffolding puede reducir la cantidad de tiempo para desarrollar las operaciones de datos estándar en el proyecto.

De forma predeterminada, Visual Studio 2013 no admite la generación de código para un proyecto de formularios Web Forms, pero puede usar scaffolding con formularios Web Forms agregando las dependencias de MVC al proyecto o instalar una extensión. A continuación se muestran ambos enfoques.

Visual Studio 2013 Update 2 (actualmente RC) proporciona la capacidad para extender el Scaffolding de ASP.NET para cumplir los requisitos de su escenario. Con esta funcionalidad, puede crear una plantilla personalizada de scaffolding y agregarlo al cuadro de diálogo Agregar nuevo scaffolding. Dentro de la plantilla personalizada, especifique el código que se genera cuando se agrega un elemento con scaffolding. Para obtener más información, consulte [crear un Scaffolder personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Requisitos previos

Para usar la técnica Scaffolding de ASP.NET, debe tener:

- Microsoft Visual Studio 2013
- Herramientas de desarrollador Web (parte de la instalación predeterminada de Visual Studio 2013)
- Marcos de trabajo ASP.NET Web y herramientas de 2013 (parte de la instalación predeterminada de Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Agregar un elemento con scaffolding para MVC o Web API

Para agregar un scaffold, haga clic en el proyecto o una carpeta dentro del proyecto y seleccione **agregar** : **nuevo elemento de scaffolding**, tal y como se muestra en la siguiente imagen.

![Agregar elemento de scaffolding](aspnet-scaffolding-overview/_static/image1.png)

Desde el **agregar scaffolding** ventana, seleccione el tipo de scaffolding para agregar.

![Seleccione el tipo de scaffolding](aspnet-scaffolding-overview/_static/image2.png)

El **Agregar controlador** ventana le ofrece la oportunidad de seleccionar opciones para generar el controlador, incluido si desea usar las nuevas características asincrónicas de Entity Framework 6.

![Agregar controlador](aspnet-scaffolding-overview/_static/image3.png)

Las clases relevantes y las páginas se crean para su escenario. Por ejemplo, la siguiente imagen muestra el controlador MVC y vistas que se crearon a través de la técnica scaffolding para una clase de modelo denominada películas.

![Los archivos creados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Agregar un elemento con scaffolding a formularios Web Forms

Para agregar scaffolding que genera el código de formularios Web Forms, debe instalar una extensión de Visual Studio o agregar las dependencias de MVC. A continuación se muestran ambos enfoques, pero sólo necesite realizar uno de estos métodos.

### <a name="web-forms-scaffolding-extension"></a>Formularios Web Forms Scaffolding extensión

Puede instalar una extensión de Visual Studio que le permiten usar la técnica scaffolding con un proyecto de formularios Web Forms. En Visual Studio, seleccione **herramientas** y, a continuación, **extensiones y actualizaciones**. En este cuadro de diálogo Buscar en la Galería de Visual Studio para **Scaffolding de Web Forms**.

![instalar formularios web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

Para obtener más información, consulte [Scaffolding de Web Forms](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependencias de MVC

Para agregar las dependencias de MVC, seleccione **agregar** - **nuevo elemento de scaffolding**. En la ventana Agregar scaffolding, seleccione **las dependencias de MVC**, tal y como se muestra a continuación.

![agregar las dependencias de MVC](aspnet-scaffolding-overview/_static/image6.png)

Hay dos opciones de scaffolding MVC; Mínimo y completa. Si selecciona mínimo, solo los paquetes de NuGet y referencias para ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC. Para utilizar fácilmente la técnica scaffolding, seleccione dependencias completas.

![Seleccione dependencias completas](aspnet-scaffolding-overview/_static/image7.png)

Después de agregar las dependencias, verá un **readme.txt** archivo. Siga cuidadosamente las instrucciones de este archivo para asegurarse de que el proyecto funciona correctamente.

Cuando se hayan completado los pasos descritos en el archivo readme.txt, puede agregar un nuevo elemento con scaffolding tal y como se muestra en la sección anterior sobre MVC y API Web. Las vistas de generados automáticamente y el controlador funcionará correctamente dentro del proyecto.

## <a name="tutorials"></a>Tutoriales

Para crear un scaffolder personalizado, consulte [crear un Scaffolder personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar los archivos generados, vea [cómo personalizar los archivos generados desde el cuadro de diálogo nuevo elemento de scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obtener un ejemplo del uso de la técnica scaffolding con **desarrollo Database First**, consulte [EF Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obtener un ejemplo del uso de la técnica scaffolding en una **MVC** proyecto, vea [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener un ejemplo del uso de la técnica scaffolding en una **API Web** proyecto, vea [crear una API de REST con el atributo de enrutamiento de Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
