---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
title: 'Parte 4: Acceso a datos y modelos | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 4 trata los modelos y acceso a datos.
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: ab55ca81-ab9b-44a0-8700-dc6da2599335
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-4
msc.type: authoredcontent
ms.openlocfilehash: 023350e882afe049ce3800921825b1b2bec8e415
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818958"
---
<a name="part-4-models-and-data-access"></a>Parte 4: Acceso a datos y modelos
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.
> 
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 4 trata los modelos y acceso a datos.


Hasta ahora, nos hemos simplemente ha en el paso "datos ficticios" de nuestros controladores a nuestras plantillas de vista. Ahora estamos listos enlazar una base de datos real. En este tutorial se tratarán cómo usar SQL Server Compact Edition (a menudo denominado SQL CE) como nuestro motor de base de datos. SQL CE es un archivo gratuita e incrustada, en función de base de datos que no requiere ninguna instalación o configuración, lo que es realmente adecuado para el desarrollo local.

## <a name="database-access-with-entity-framework-code-first"></a>Acceso de la base de datos con Entity Framework Code First

Vamos a usar la compatibilidad con Entity Framework (EF) que se incluye en los proyectos de ASP.NET MVC 3 para consultar y actualizar la base de datos. EF es un objeto flexible relacional de asignación de API que permite a los desarrolladores para consultar y actualizar datos almacenados en una base de datos de una manera orientada a objetos de datos de (objetos ORM).

Versión 4 de Entity Framework admite un paradigma de desarrollo llamado code first. Code first le permite crear el objeto de modelo escribiendo clases simples (también conocido como POCO desde los objetos CLR "antiguos") y, incluso puede crear la base de datos sobre la marcha a partir de clases.

### <a name="changes-to-our-model-classes"></a>Cambios realizados en nuestras clases de modelo

Utilizaremos la función de creación de la base de datos en Entity Framework en este tutorial. Antes de hacerlo, sin embargo, vamos a hacer algunos cambios menores en nuestras clases de modelo para agregar en algunas cosas que usaremos más adelante.

#### <a name="adding-the-artist-model-classes"></a>Agregar las clases de modelo del intérprete

Nuestro álbumes se asociará con intérpretes, así que agregaremos una clase de modelo simple para describir a un artista. Agregue una nueva clase a la carpeta Models denominada Artist.cs con el código que se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample1.cs)]

#### <a name="updating-our-model-classes"></a>Actualización de nuestras clases de modelo

Actualización de la clase álbum tal como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample2.cs)]

A continuación, realice las siguientes actualizaciones a la clase de género.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample3.cs)]

### <a name="adding-the-appdata-folder"></a>Agregar la aplicación\_carpeta de datos

Vamos a agregar una aplicación\_directorio de datos a nuestro proyecto que contenga los archivos de base de datos de SQL Server Express. Aplicación\_datos están un directorio especial en ASP.NET que ya tiene los permisos de acceso de seguridad correctas para el acceso a la base de datos. En el menú proyecto, seleccione Agregar carpeta ASP.NET y, a continuación, aplicación\_datos.

![](mvc-music-store-part-4/_static/image1.png)

### <a name="creating-a-connection-string-in-the-webconfig-file"></a>Creación de una cadena de conexión en el archivo web.config

Agregaremos unas cuantas líneas al archivo de configuración del sitio Web para que sepa cómo conectarse a nuestra base de datos que Entity Framework. Haga doble clic en el archivo Web.config ubicado en la raíz del proyecto.

![](mvc-music-store-part-4/_static/image2.png)

Desplácese hasta el final de este archivo y agregue un &lt;connectionStrings&gt; sección directamente encima de la última línea, como se muestra a continuación.

[!code-xml[Main](mvc-music-store-part-4/samples/sample4.xml)]

### <a name="adding-a-context-class"></a>Agregar una clase de contexto

Haga clic en la carpeta Models y agregue una nueva clase denominada MusicStoreEntities.cs.

![](mvc-music-store-part-4/_static/image3.png)

Esta clase se representan el contexto de base de datos de Entity Framework y se controlan nuestro crear, leer, actualizar y eliminar operaciones para nosotros. El código para esta clase se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample5.cs)]

Eso es todo: no hay ningún otro configuración, interfaces especiales, etcetera. Mediante la extensión de la clase base DbContext, nuestra clase MusicStoreEntities es capaz de controlar nuestras operaciones de base de datos para nosotros. Ahora que tenemos enlazarse, vamos a agregar más propiedades para nuestras clases de modelo para aprovechar las ventajas de la parte de la información adicional en nuestra base de datos.

### <a name="adding-our-store-catalog-data"></a>Adición de nuestros datos de catálogo del almacén

Nos sacarán provecho de una característica en Entity Framework que agrega datos de "inicialización" a una base de datos recién creada. Esto rellenará automáticamente con una lista de géneros, intérpretes y álbumes nuestro catálogo de la tienda. La descarga de MvcMusicStore Assets.zip - que incluye los archivos de diseño del sitio usados anteriormente en este tutorial: tiene un archivo de clase con estos datos de inicialización, ubicados en una carpeta con el nombre de código.

Dentro del código o la carpeta Models, busque el archivo SampleData.cs y colóquelo en la carpeta de modelos en el proyecto, como se muestra a continuación.

![](mvc-music-store-part-4/_static/image4.png)

Ahora tenemos que agregar una línea de código para indicar a Entity Framework acerca de esa clase SampleData. Haga doble clic en el archivo Global.asax en la raíz del proyecto para abrirlo y agregue la siguiente línea a la parte superior de la aplicación\_Start (método).

[!code-csharp[Main](mvc-music-store-part-4/samples/sample6.cs)]

En este momento, hemos completado el trabajo necesario configurar Entity Framework para nuestro proyecto.

## <a name="querying-the-database"></a>Consultar la base de datos

Ahora vamos a actualizar nuestro StoreController para que en lugar de usar "ficticio datos" en su lugar, llama a nuestra base de datos para consultar toda su información. Comenzaremos declarando un campo en el **StoreController** que hospede una instancia de la clase MusicStoreEntities, denominada storeDB:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample7.cs)]

### <a name="updating-the-store-index-to-query-the-database"></a>Actualizar el índice de Store para consultar la base de datos

La clase MusicStoreEntities se mantiene por Entity Framework y expone una propiedad de colección para cada tabla en nuestra base de datos. Vamos a actualizar acción del índice de nuestro StoreController para recuperar todos los géneros en nuestra base de datos. Anteriormente se hizo esto al codificar los datos de cadena. Ahora podemos en su lugar, simplemente usamos el contexto de Entity Framework Generes colección:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample8.cs)]

Ningún cambio debe ocurrir a nuestra plantilla de vista, ya que todavía devuelve el mismo StoreIndexViewModel nos devuelve antes - simplemente devuelve datos en directo desde nuestra base de datos ahora.

Cuando se vuelva a ejecuta el proyecto y visite la dirección URL "/ Store", Ahora veremos una lista de todos los géneros en nuestra base de datos:

![](mvc-music-store-part-4/_static/image1.jpg)

### <a name="updating-store-browse-and-details-to-use-live-data"></a>Actualización de examinar Store y los detalles para utilizar datos en directo

Con/Store/examinar? genre =*[some y género]* método de acción, estamos buscando un género por su nombre. Solo se espera un resultado, porque no alguna vez deberíamos dos entradas para el mismo nombre de género, por lo que podemos usar el. Extensión de Single() en LINQ para consultar el objeto adecuado de género similar al siguiente (no escriba Esto todavía):

[!code-csharp[Main](mvc-music-store-part-4/samples/sample9.cs)]

El único método toma una expresión Lambda como un parámetro, que especifica que se desea un único objeto género tal que su nombre coincide con el valor que hemos definido. En el caso anterior, estamos cargando un único objeto género con un valor de nombre que coincida con Disco.

Se podrán aprovechar las ventajas de una característica de Entity Framework que nos permite indicar otras entidades relacionadas que queremos cargar también cuando se recupera el objeto de género. Esta característica se llama a la forma de resultado de consulta y nos permite reducir el número de veces que se necesita para tener acceso a la base de datos para recuperar toda la información que necesitamos. Queremos una captura previa de los álbumes de género recuperamos, por lo que actualizaremos nuestra consulta para incluir en Genres.Include("Albums") para indicar que queremos también álbumes relacionados. Esto es más eficaz, ya que recuperará los datos de nuestros género y el álbum en una solicitud de base de datos única.

Con las explicaciones de la vista, aquí es el aspecto de la acción de controlador examinar actualizada:

[!code-csharp[Main](mvc-music-store-part-4/samples/sample10.cs)]

Ahora podemos actualizar la vista para examinar Store para mostrar los álbumes que están disponibles en cada género. Abra la plantilla de vista (se encuentra en /Views/Store/Browse.cshtml) y agregue una lista con viñetas de álbumes, tal como se muestra a continuación.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample11.cshtml)]

Ejecutar la aplicación y vaya a/Store/examinar? genre = muestra Jazz que nuestros resultados ahora provienen de la base de datos, mostrar todos los álbumes en nuestra género seleccionado.

![](mvc-music-store-part-4/_static/image2.jpg)

Nos aseguraremos de hacer el mismo cambia a nuestro/Store/detalles / [id] dirección URL y reemplace nuestros datos ficticios con una consulta de base de datos que carga un álbum cuyo identificador coincide con el valor del parámetro.

[!code-csharp[Main](mvc-music-store-part-4/samples/sample12.cs)]

Ejecutar la aplicación y vaya a /Store/Details/1 muestran que nuestros resultados ahora provienen de la base de datos.

![](mvc-music-store-part-4/_static/image5.png)

Ahora que nuestra página de detalles de Store se ha configurado para mostrar un álbum por el identificador del álbum, vamos a actualizar el **examinar** vista para vincular a la vista de detalles. Usaremos Html.ActionLink, exactamente igual que hicimos para vincular de índice Store para examinar Store al final de la sección anterior. El código fuente completo para la vista de examinar aparece debajo.

[!code-cshtml[Main](mvc-music-store-part-4/samples/sample13.cshtml)]

Ahora podemos examinar desde nuestra página Store a una página de género, que enumera los álbumes disponibles, y haciendo clic en un álbum, podemos ver los detalles de ese álbum.

![](mvc-music-store-part-4/_static/image6.png)

> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-3.md)
> [Siguiente](mvc-music-store-part-5.md)
