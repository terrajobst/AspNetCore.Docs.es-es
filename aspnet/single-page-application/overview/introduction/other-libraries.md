---
uid: single-page-application/overview/introduction/other-libraries
title: ¿Conoce una biblioteca distinta de Knockout? | Microsoft Docs
author: madskristensen
description: ¿Conoce una biblioteca distinta de Knockout?
ms.author: aspnetcontent
ms.date: 02/05/2013
ms.assetid: a8367c6d-ef94-4dff-a010-5eff9e6eea96
msc.legacyurl: /single-page-application/overview/introduction/other-libraries
msc.type: authoredcontent
ms.openlocfilehash: 69006df6a5de3290ac294a309756eb0a53baa975
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834599"
---
<a name="know-a-library-other-than-knockout"></a>¿Conoce una biblioteca distinta de Knockout?
====================
por [Mads Kristensen](https://github.com/madskristensen)

El [plantilla de aplicación de página única (SPA)](knockoutjs-template.md) es una excelente manera de empezar a escribir aplicaciones de página única. La plantilla utiliza [KnockoutJS](http://knockoutjs.com/) para enlazar datos de la aplicación a los elementos DOM.

Pero Knockout no es la única biblioteca de JavaScript para crear aplicaciones cliente enriquecida. Otras bibliotecas resolución desafíos similares de diferentes formas. Es preferible una biblioteca de uno u otro, por lo que hemos realizado varias plantillas creados por la Comunidad disponible para su descarga. Cada una de estas plantillas utiliza una combinación diferente de las bibliotecas de JavaScript de cliente.

Para instalar una plantilla creados por la Comunidad, visite uno de la plantilla de las páginas enumeran a continuación y haga clic en el botón de descarga. Las plantillas se proporcionan como archivos VSIX.

## <a name="backbonejs"></a>BackboneJS

[Plantilla SPA backbone.js](../templates/backbonejs-template.md). Esta plantilla proporciona un esqueleto inicial para desarrollar un [Backbone.js](http://backbonejs.org/) aplicación en ASP.NET MVC. De fábrica proporciona funcionalidad de inicio de sesión de usuario básico, incluido el restablecimiento de contraseña de registro, inicio de sesión de usuario y la confirmación del usuario con plantillas de correo electrónico básico.

## <a name="breezejs"></a>BreezeJS

[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa) es una biblioteca de código abierto para la administración de datos enriquecidos en un cliente de JavaScript. BREEZE encarga de realizar consultas, almacenamiento en caché, el seguimiento de cambios, validación y mucho más. Dos plantillas de la característica Breeze:

- El [Breeze/Knockout](../templates/breezeknockout-template.md) plantilla amplía la plantilla de SPA de Knockout, que muestra lo fácil puede compilar una aplicación de página única con Breeze para la administración de datos y KnockoutJS para el enlace de datos.
- El [Breeze/Angular](../templates/breezeangular-template.md) plantilla también extiende la plantilla de Knockout SPA con Breeze, pero utilizando el [AngularJS](http://angularjs.org) biblioteca para el enlace de datos, inserción de dependencias y la administración de la pantalla.

Además, el [plantilla de Hot SPA toalla](../templates/hottowel-template.md) usa BreezeJS.

## <a name="emberjs"></a>EmberJS

[Plantilla de EmberJS SPA](../templates/emberjs-template.md). Esta plantilla usa [Ember](http://emberjs.com/), una eficaz biblioteca de JavaScript de MVC que resuelve una amplia gama de desafíos para la creación de aplicaciones cliente enriquecidas.

La plantilla de SPA de Ember es una reimplementación de la plantilla de SPA de Knockout, mediante plantillas de EmberJS y Handlebars.

## <a name="hot-towel"></a>Hot toalla

[Plantilla de SPA toalla hot](../templates/hottowel-template.md). Esta plantilla se ofrece en varias bibliotecas de JavaScript, incluidos Breeze, Knockout, RequireJS y Twitter Bootstrap.

En comparación con las otras plantillas que se muestran aquí, el teample toalla activa proporciona una aplicación de más completa desde el que puede crear sus propios. Hay varios conceptos que tener en cuenta, pero una vez que entenderlas, esta plantilla puede ser simplemente lo busca. Si desea crear una SPA pero no se puede decidir dónde se inicia, use la toalla activo y en segundos, tendrá una SPA y todas las herramientas debe basarse en él.

## <a name="feature-table"></a>Tabla de características

Estas son las características proporcionadas por cada plantilla SPA:


|                        | ASP.NET SPA | Red troncal | BREEZE/Angular | BREEZE/KO |  Ember   | Hot toalla |
|------------------------|-------------|----------|----------------|-----------|----------|-----------|
|      Ejemplo "todo"       |  &#10003;   |          |    &#10003;    | &#10003;  | &#10003; |           |
|     Plantilla      |             | &#10003; |                |           |          | &#10003;  |
| Navegación e historial |             | &#10003; |    &#10003;    |           | &#10003; | &#10003;  |
|        Bibliotecas        |             |          |                |           |          |           |
|        Angular         |             |          |    &#10003;    |           |          |           |
|    &#8195;Red troncal     |             | &#10003; |                |           |          |           |
|         BREEZE         |             |          |    &#10003;    | &#10003;  |          | &#10003;  |
|        Durandal        |             |          |                |           |          | &#10003;  |
|         Ember          |             |          |                |           | &#10003; |           |
|        Knockout        |  &#10003;   |          |                | &#10003;  |          | &#10003;  |

