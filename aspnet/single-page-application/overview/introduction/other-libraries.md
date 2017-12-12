---
uid: single-page-application/overview/introduction/other-libraries
title: "¿Saber una biblioteca distinta de cobertura? | Microsoft Docs"
author: madskristensen
description: "¿Saber una biblioteca distinta de cobertura?"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/05/2013
ms.topic: article
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 5a863f50401a4e2bab3f772374b7fd178f6c6cdf
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="know-a-library-other-than-knockout"></a>¿Saber una biblioteca distinta de cobertura?
====================
por [Mads Kristensen](https://github.com/madskristensen)

El [plantilla de aplicación de página única (SPA)](knockoutjs-template.md) es una excelente manera de comenzar a escribir aplicaciones de la página. La plantilla utiliza [KnockoutJS](http://knockoutjs.com/) para enlazar los datos de la aplicación a elementos DOM.

Pero Knockout no es la única biblioteca de JavaScript para crear aplicaciones cliente enriquecida. Otras bibliotecas resolución desafíos similares de maneras diferentes. Puede que prefiera una uno biblioteca frente a otro, por lo que hemos realizado varias plantillas creados por la Comunidad disponible para su descarga. Cada una de estas plantillas utiliza una combinación diferente de las bibliotecas de JavaScript de cliente.

Para instalar una plantilla creados por la Comunidad, visite uno de la plantilla de páginas se enumeran a continuación y haga clic en el botón de descarga. Las plantillas se proporcionan como archivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Plantilla SPA backbone.js](../templates/backbonejs-template.md). Esta plantilla proporciona un esqueleto inicial para desarrollar un [Backbone.js](http://backbonejs.org/) aplicación de ASP.NET MVC. De fábrica proporciona funcionalidad de inicio de sesión de usuario básico, incluido el restablecimiento de contraseña de inicio de sesión, inicio de sesión de usuario y la confirmación de usuario con plantillas de correo electrónico básico.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) es una biblioteca de código abierto para la administración de datos enriquecidos en un cliente de JavaScript. Es sencilla controla la consulta, almacenamiento en caché, el seguimiento de cambios, validación y mucho más. Dos plantillas de características es sencilla:

- El [Facilísima/Knockout](../templates/breezeknockout-template.md) plantilla extiende la plantilla Knockout SPA, que muestra cómo fácilmente puede compilar una aplicación de página con sea sencillo KnockoutJS y la administración de datos de enlace de datos.
- El [Facilísima/Angular](../templates/breezeangular-template.md) plantilla también extiende la plantilla Knockout SPA con es sencilla, pero utilizando la [AngularJS](http://angularjs.org) biblioteca para enlace de datos, la inserción de dependencias y la administración de pantalla.

Además, el [plantilla activa SPA toallas](../templates/hottowel-template.md) utiliza BreezeJS.

## <a name="emberjs"></a>EmberJS

[Plantilla de SPA EmberJS](../templates/emberjs-template.md). Esta plantilla utiliza [Ember](http://emberjs.com/), una biblioteca de JavaScript de MVC eficaz que resuelve una amplia gama de desafíos para la creación de aplicaciones cliente enriquecidas.

La plantilla de SPA Ember es una implementación nueva de la plantilla de Knockout SPA, utilizando EmberJS y manillares de las plantillas.

## <a name="hot-towel"></a>Toallas activa

[Plantilla de toallas SPA activa](../templates/hottowel-template.md). Esta plantilla se introduce en varias bibliotecas de JavaScript, como es sencilla, Knockout, RequireJS y arranque de Twitter.

En comparación con las otras plantillas enumeradas aquí, el teample de toallas activa proporciona una aplicación más completa desde la que puede crear sus propios. Hay varios conceptos para tener en cuenta, pero una vez que comprenda ellos, esta plantilla puede ser simplemente lo está buscando. Si desea generar un SPA, pero no puede decidir dónde se inicia, use toallas activa y en segundos, tendrá un SPA y todas las herramientas debe basarse en él.

## <a name="feature-table"></a>Tabla de características

Estas son las características proporcionadas por cada plantilla SPA:

|  | ASP.NET SPA | Red troncal | Es sencilla/Angular | Es sencilla/KO | Ember | Toallas activa |
| --- | --- | --- | --- | --- | --- | --- |
| Ejemplo de lista de tareas | &#10003; |  | &#10003; | &#10003; | &#10003; |  |
| Plantilla |  | &#10003; |  |  |  | &#10003; |
| Navegación e historial |  | &#10003; | &#10003; |  | &#10003; | &#10003; |
| Bibliotecas |  |  |  |  |  |  |
| angular |  |  | &#10003; |  |  |  |
| &#8195; Red troncal |  | &#10003; |  |  |  |  |
| Es sencilla |  |  | &#10003; | &#10003; |  | &#10003; |
| durandal |  |  |  |  |  | &#10003; |
| Ember |  |  |  |  | &#10003; |  |
| knockout | &#10003; |  |  | &#10003; |  | &#10003; |
