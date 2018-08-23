---
uid: visual-studio/overview/2013/aspnet-scaffolding-overview
title: Scaffolding de ASP.NET en Visual Studio 2013 | Microsoft Docs
author: tfitzmac
description: Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.
ms.author: riande
ms.date: 04/09/2014
ms.assetid: a41ec9d4-8287-4f31-9e2a-460e7b7f04be
msc.legacyurl: /visual-studio/overview/2013/aspnet-scaffolding-overview
msc.type: authoredcontent
ms.openlocfilehash: c554f592e57f2e6017f7fcfcc9b4c98051e21b37
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837292"
---
<a name="aspnet-scaffolding-in-visual-studio-2013"></a>Scaffolding de ASP.NET en Visual Studio 2013
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Scaffolding de ASP.NET es una característica nueva que se incluye en Visual Studio 2013.


## <a name="overview"></a>Información general

Scaffolding de ASP.NET es un marco de generación de código para aplicaciones Web ASP.NET. Visual Studio 2013 incluye generadores de código instalado previamente para los proyectos de MVC y Web API. Agregar scaffolding al proyecto cuando desea agregar rápidamente código que interactúe con modelos de datos. Usar scaffolding puede reducir la cantidad de tiempo para desarrollar las operaciones de datos estándar en el proyecto.

De forma predeterminada, Visual Studio 2013 no admite la generación de código para un proyecto de formularios Web Forms, pero puede usar con formularios Web Forms scaffolding agregando las dependencias MVC al proyecto o instalar una extensión. A continuación se muestran ambos enfoques.

Visual Studio 2013 Update 2 (actualmente RC) proporciona la capacidad para ampliar el Scaffolding de ASP.NET para cumplir los requisitos de su escenario. Con esta funcionalidad, puede crear una plantilla de scaffolding personalizado y agregarlo al cuadro de diálogo Agregar Scaffold de nuevo. Dentro de la plantilla personalizada, especifique el código que se genera cuando se agrega un elemento con scaffolding. Para obtener más información, consulte [creación de un proveedor de scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

## <a name="prerequisites"></a>Requisitos previos

Para usar el Scaffolding de ASP.NET, debe tener:

- Microsoft Visual Studio 2013
- Herramientas de desarrollo Web (parte de la instalación predeterminada de Visual Studio 2013)
- Marcos Web ASP.NET y herramientas 2013 (parte de la instalación predeterminada de Visual Studio 2013)

## <a name="add-a-scaffolded-item-to-mvc-or-web-api"></a>Agregar un elemento con scaffold a MVC o Web API

Para agregar una scaffold, haga clic en el proyecto o una carpeta dentro del proyecto y seleccione **agregar** – **nuevo elemento de scaffolding**, tal y como se muestra en la siguiente imagen.

![Agregar elemento de scaffolding](aspnet-scaffolding-overview/_static/image1.png)

Desde el **agregar Scaffold** ventana, seleccione el tipo de scaffold a agregar.

![Seleccione el tipo de scaffold](aspnet-scaffolding-overview/_static/image2.png)

El **Agregar controlador** ventana le ofrece la oportunidad de seleccionar opciones para generar el controlador, incluso si desea usar las nuevas características asincrónicas de Entity Framework 6.

![Agregar controlador](aspnet-scaffolding-overview/_static/image3.png)

Las clases pertinentes y las páginas se crean para su escenario. Por ejemplo, la siguiente imagen muestra el controlador de MVC y las vistas que se crearon mediante scaffolding para una clase de modelo denominada películas.

![Los archivos creados](aspnet-scaffolding-overview/_static/image4.png)

## <a name="add-a-scaffolded-item-to-web-forms"></a>Agregar un elemento con scaffolding para formularios Web Forms

Para agregar scaffolding que genera el código de formularios Web Forms, debe instalar una extensión para Visual Studio, o bien agregar dependencias de MVC. A continuación se muestran ambos enfoques, pero solo necesita realizar uno de estos enfoques.

### <a name="web-forms-scaffolding-extension"></a>Formularios Web Forms Scaffolding de extensión

Puede instalar una extensión de Visual Studio que le permiten usar la técnica scaffolding con un proyecto de formularios Web Forms. En Visual Studio, seleccione **herramientas** y, a continuación, **extensiones y actualizaciones**. En este cuadro de diálogo Buscar en la Galería de Visual Studio para **Web Forms Scaffolding**.

![instalar los formularios web forms scaffolding](aspnet-scaffolding-overview/_static/image5.png)

Para obtener más información, consulte [Web Forms Scaffolding](https://go.microsoft.com/fwlink/p/?LinkId=396478).

### <a name="mvc-dependencies"></a>Dependencias de MVC

Para agregar dependencias de MVC, seleccione **agregar** - **nuevo elemento de scaffolding**. En la ventana Agregar Scaffold, seleccione **dependencias de MVC**, tal y como se muestra a continuación.

![Agregar dependencias de MVC](aspnet-scaffolding-overview/_static/image6.png)

Hay dos opciones para scaffolding de MVC; Mínimo y completa. Si selecciona mínima, sólo los paquetes de NuGet y referencias de ASP.NET MVC se agregan al proyecto. Si selecciona la opción completa, se agregan las dependencias mínimas, así como los archivos de contenido necesarios para un proyecto de MVC. Para utilizar fácilmente la técnica scaffolding, seleccione dependencias completas.

![Seleccione dependencias completas](aspnet-scaffolding-overview/_static/image7.png)

Después de agregar las dependencias, verá un **readme.txt** archivo. Siga cuidadosamente las instrucciones de este archivo para asegurarse de que el proyecto funcione correctamente.

Cuando haya completado los pasos descritos en el archivo readme.txt, puede agregar un nuevo elemento con scaffolding, tal como se muestra en la sección anterior sobre MVC y Web API. Las vistas de genera automáticamente y el controlador funcionará correctamente dentro del proyecto.

## <a name="tutorials"></a>Tutoriales

Para crear un proveedor de scaffolding personalizado, consulte [creación de un proveedor de scaffolding personalizado para Visual Studio](https://go.microsoft.com/fwlink/p/?LinkId=395029).

Para personalizar los archivos generados, vea [cómo personalizar los archivos generados en el cuadro de diálogo nuevo elemento de scaffolding](https://blogs.msdn.com/b/webdev/archive/2013/12/26/how-to-customize-the-generated-files-from-the-new-scaffolded-item-dialog.aspx).

Para obtener un ejemplo del uso de la técnica scaffolding con **desarrollo Database First**, consulte [EF Database First con ASP.NET MVC](../../../mvc/overview/getting-started/database-first-development/setting-up-database.md).

Para obtener un ejemplo del uso de la técnica scaffolding en un **MVC** de proyecto, consulte [Introducción a ASP.NET MVC 5](../../../mvc/overview/getting-started/introduction/getting-started.md).

Para obtener un ejemplo del uso de la técnica scaffolding en un **API Web** de proyecto, consulte [crear una API de REST con enrutamiento mediante atributos en Web API 2](../../../web-api/overview/web-api-routing-and-actions/create-a-rest-api-with-attribute-routing.md).
