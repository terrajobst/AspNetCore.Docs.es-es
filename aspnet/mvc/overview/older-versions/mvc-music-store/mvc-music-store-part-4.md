---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acceso a datos y modelos | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 4 cubre el acceso a datos y modelos.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 76671bbc7050d111b4d156c45584ba5aa4f1ea8f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="part-4-models-and-data-access"></a>Parte 4: Acceso a datos y modelos
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 4 cubre el acceso a datos y modelos.


Hasta ahora, hemos solo ha pasamos "datos ficticios" de nuestros controladores a nuestras plantillas de vista. Ahora estamos listos enlazar una base de datos real. En este tutorial abordaremos cómo usar SQL Server Compact Edition (también denominados SQL CE) como el motor de base de datos. SQL CE es un archivo gratuita e incrustada, en función de base de datos que no requiere ninguna instalación o configuración, lo que es realmente adecuado para el desarrollo local.

## <a name="database-access-with-entity-framework-code-first"></a>Acceso de base de datos con Entity Framework Code-First

Vamos a usar la compatibilidad de Entity Framework (EF) que se incluye en los proyectos de ASP.NET MVC 3 para consultar y actualizar la base de datos. EF es un objeto flexible relacional de asignación de API que permite a los desarrolladores para consultar y actualizar datos almacenados en una base de datos de una manera orientada a objetos de datos de (objetos ORM).

Versión 4 de Entity Framework admite un paradigma de desarrollo que se llama a código en primer lugar. Código primero le permite crear el objeto de modelo mediante la escritura de clases simples (también conocido como POCO "plain old" objetos de CLR) y puede incluso crear la base de datos sobre la marcha de las clases.

### <a name="changes-to-our-model-classes"></a>Cambios en las clases de modelo

Se aprovecha la característica de creación de la base de datos en Entity Framework de este tutorial. Antes de hacerlo, sin embargo, vamos a hacer algunos cambios menores en nuestras clases de modelo para agregar en algunas cosas que se usará más adelante.

#### <a name="adding-the-artist-model-classes"></a>Agregar las clases del modelo de intérprete

Nuestro álbumes se asociarán con intérpretes, por lo que vamos a agregar una clase de modelo simple para describir a un intérprete. Agregue una nueva clase en la carpeta de modelos denominada Artist.cs con el código que se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Actualizar las clases de modelo

Actualizar la clase de álbum tal y como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

A continuación, realice las siguientes actualizaciones a la clase de género.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Agregar la aplicación\_carpeta de datos

Vamos a agregar una aplicación\_directorio de datos a nuestro proyecto que contenga los archivos de base de datos de SQL Server Express. Aplicación\_datos están un directorio especial en ASP.NET que ya tiene los permisos de acceso de seguridad correctas para el acceso a la base de datos. En el menú proyecto, seleccione Agregar carpeta ASP.NET y, a continuación, aplicación\_datos.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Crear una cadena de conexión en el archivo web.config

Agregaremos unas cuantas líneas al archivo de configuración del sitio Web para que Entity Framework sepa cómo conectarse a nuestra base de datos. Haga doble clic en el archivo Web.config ubicado en la raíz del proyecto.

![](mvc-music-store-part-4/_static/image2.png)

Desplácese hasta el final de este archivo y agregar un &lt;connectionStrings&gt; sección directamente por encima de la última línea, tal y como se muestra a continuación.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Agregar una clase de contexto

Haga clic en la carpeta de modelos y agregue una nueva clase denominada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Esta clase se representan el contexto de base de datos de Entity Framework y se controlen nuestro crear, leer, actualizar y eliminar operaciones para nosotros. El código de esta clase se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

¡Eso es todo; no hay ninguna otra configuración, interfaces especiales etcetera. Si se extiende la clase base de DbContext, nuestra clase MusicStoreEntities es capaz de administrar nuestras operaciones de base de datos para que podamos. Ahora que tenemos enlazan, vamos a agregar algunas propiedades más a nuestro clases de modelo para aprovechar las ventajas de la parte de la información adicional en nuestra base de datos.

### <a name="adding-our-store-catalog-data"></a>Agregar los datos de catálogo del almacén

Se aprovechará una característica de Entity Framework que agrega datos de "inicialización" a una base de datos recién creada. Esto rellenará previamente nuestro catálogo de la tienda con una lista de géneros, intérpretes y álbumes. La descarga de MvcMusicStore Assets.zip - que incluye los archivos de diseño de sitio usados anteriormente en este tutorial - tiene un archivo de clase con estos datos de inicialización, ubicados en una carpeta con el nombre de código.

En el código / carpeta Models, busque el archivo SampleData.cs y colóquelo en la carpeta de modelos en el proyecto, tal y como se muestra a continuación.

![](mvc-music-store-part-4/_static/image4.png)

Ahora tenemos que agregar una línea de código y explíquenos esa clase SampleData en Entity Framework. Haga doble clic en el archivo Global.asax de la raíz del proyecto para abrirlo y agregue la siguiente línea a la parte superior de la aplicación\_Start (método).

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

En este punto, hemos completado el trabajo necesario configurar Entity Framework para el proyecto.

## <a name="querying-the-database"></a>Consultar la base de datos

Ahora vamos a actualizar nuestra StoreController para que en lugar de usar "ficticio datos" en su lugar, llama a nuestra base de datos para consultar toda su información. Comenzaremos declarando un campo en el **StoreController** que hospede una instancia de la clase MusicStoreEntities, denominada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Actualizar el índice de almacén para consultar la base de datos

La clase MusicStoreEntities se mantiene por Entity Framework y expone una propiedad de colección para cada tabla en la base de datos. Vamos a actualizar acción del índice de nuestro StoreController para recuperar todos los juegos en nuestra base de datos. Anteriormente se hizo esto al codificar los datos de cadena. Ahora podemos en su lugar, utilizar el contexto de Entity Framework Generes colección:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Ningún cambio tiene que haber a la plantilla de vista, ya que todavía nos estamos devolver el mismo StoreIndexViewModel se devuelve antes - que sólo se devuelve datos en directo de nuestra base de datos ahora.

Cuando se vuelva a ejecuta el proyecto y visita la dirección URL "/ Store", se verá ahora una lista de todos los juegos en nuestra base de datos:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Actualización de almacén de examinar y detalles para utilizar datos en directo

Con/almacén/examinar? género =*[algunas de género]* método de acción, estamos buscando un género por su nombre. Esperamos que solo un resultado, ya que no deberíamos nunca dos entradas para el mismo nombre de género, por lo que podemos usar la. Extensión de Single() en LINQ para consultar el objeto de género adecuado similar al siguiente (no escriba Esto todavía):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

El único método toma una expresión Lambda como un parámetro, que especifica que se desea un único objeto de género tal que su nombre coincide con el valor que hemos definido. En el caso anterior, vamos a cargar un único objeto de género con un valor de nombre que coincida con Disco.

Se podrá aprovechar las ventajas de una característica de Entity Framework que nos permite indicar otras entidades relacionadas que queremos cargar también cuando se recupera el objeto de género. Esta característica se denomina forma del resultado de consulta y nos permite reducir el número de veces que se necesita para tener acceso a la base de datos para recuperar todos los datos que necesitamos. Es deseable dar una captura previa de los álbumes de género recuperamos, por lo que actualizaremos nuestra consulta para incluir en Genres.Include("Albums") para indicar que deseamos así álbumes relacionados. Esto es más eficaz, ya que se recuperarán datos de nuestro género y el álbum en una solicitud de base de datos único.

Con las explicaciones fuera de la vista, este es nuestro acción de controlador actualizada examinar aspecto:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Ahora podemos actualizar el almacén de examinar la vista para mostrar los álbumes que están disponibles en cada género. Abra la plantilla de vista (se encuentra en /Views/Store/Browse.cshtml) y agregue una lista con viñetas de álbumes, tal y como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Ejecutar la aplicación y vaya a/almacén/examinar? género = muestra Jazz que nuestros resultados ahora son que se extrae de la base de datos, mostrar todos los álbumes en nuestro género seleccionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Nos aseguraremos de hacer los mismos cambie a nuestro/Store/detalles / [id] dirección URL y reemplace nuestros datos ficticios por una consulta de base de datos que carga un álbum cuyo identificador coincide con el valor del parámetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Ejecutar la aplicación y vaya a /Store/Details/1 muestran que nuestros resultados ahora provienen de la base de datos.

![](mvc-music-store-part-4/_static/image5.png)

Ahora que nuestra página de detalles del almacén está configurado para mostrar un álbum por el identificador del álbum, vamos a actualizar el **examinar** vista para vincular a la vista de detalles. Usaremos Html.ActionLink, exactamente igual que hicimos para crear un vínculo desde el índice de almacén para examinar el almacén al final de la sección anterior. El código fuente completo para la vista de exploración aparece debajo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Ahora podemos examinar desde nuestra página de almacén a una página de género, que enumera los álbumes disponibles, y haciendo clic en un álbum podemos ver detalles del álbum.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-3.md)
> [Siguiente](mvc-music-store-part-5.md)
