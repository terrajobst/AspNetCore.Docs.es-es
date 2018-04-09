---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Crear una base de datos | Documentos de Microsoft
author: microsoft
description: Paso 2 muestra los pasos para crear la base de datos que contiene todos los de la cena y RSVP datos para nuestra aplicación NerdDinner.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: ba28d671bf13ec54b83b876462e2c23f90310037
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="create-a-database"></a>Crear una base de datos
====================
por [Microsoft](https://github.com/microsoft)

[Descarga de PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 2 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.
> 
> Paso 2 muestra los pasos para crear la base de datos que contiene todos los de la cena y RSVP datos para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner paso 2: Crear la base de datos

Usaremos una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.

Los pasos siguientes muestran la creación de la base de datos con la edición gratuita de SQL Server Express (que se puede instalar fácilmente con V2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Todo el código que escribiremos funciona con SQL Server Express y la versión completa de SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Crear una nueva base de datos SQL Server Express

Se podrá comenzar con el botón secundario en nuestro proyecto web y, a continuación, seleccione la **Add -&gt;nuevo elemento** comando de menú:

![](create-a-database/_static/image1.png)

Se abrirá el cuadro de diálogo de Visual Studio "Agregar nuevo elemento". Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de elemento de "Base de datos de SQL Server":

![](create-a-database/_static/image2.png)

Llamaremos a la base de datos de SQL Server Express que deseamos crear "NerdDinner.mdf" y pulse Aceptar. Visual Studio le nos preguntará si desea agregar este archivo a nuestro \App\_directorio de datos (que es un directorio ya el programa de instalación con la lectura y escritura ACL de seguridad):

![](create-a-database/_static/image3.png)

Haremos clic en "Sí" y la nueva base de datos se pueden crearse y agregarse en el Explorador de soluciones:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Crear tablas dentro de nuestra base de datos

Ahora tenemos una base de datos vacía. Vamos a agregar algunas tablas a él.

Para ello, se desplazará a la ventana de la ficha "Explorador de servidores" dentro de Visual Studio, que permite administrar servidores y bases de datos. Bases de datos de SQL Server Express almacenadas en el \App\_carpeta de datos de nuestra aplicación se mostrará automáticamente en el Explorador de servidores. Opcionalmente, podemos utilizar el icono "Conectar a base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (locales y remotos) a la lista como:

![](create-a-database/_static/image5.png)

Agregará dos tablas a nuestra base de datos NerdDinner: uno para almacenar nuestra cenas y otro para realizar un seguimiento de aceptaciones RSVP a ellos. Podemos crear nuevas tablas, haga doble clic en la carpeta de "Tablas" dentro de nuestra base de datos y elija el comando de menú "Agregar nueva tabla":

![](create-a-database/_static/image6.png)

Esto se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla. Para la tabla "Cenas" agregaremos 10 columnas de datos:

![](create-a-database/_static/image7.png)

Queremos que la columna "DinnerID" para que sea una clave principal única para la tabla. Podemos configurar esto, haga clic en la columna "DinnerID" y elija el elemento de menú "Establecer clave principal":

![](create-a-database/_static/image8.png)

Además de realizar DinnerID una clave principal, también queremos configurar como una columna de "identidad" cuyo valor se incrementa automáticamente cuando se agregan nuevas filas de datos a la tabla (es decir, la primera fila insertada de cena tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etcetera).

Podemos hacer esto seleccionando la columna "DinnerID" y, a continuación, utilice el editor de "Propiedades de columna" para establecer la propiedad "(es la identidad)" en la columna en "Sí". Se usará la identidad estándar de los valores predeterminados (empiezan en 1 y 1 en cada nueva fila de la cena de incrementar):

![](create-a-database/_static/image9.png)

A continuación, se podrá guardar nuestra tabla escribiendo Ctrl-S o usando la **archivo -&gt;guardar** comando de menú. Se le pedirá que llame a la tabla. Lo llamaremos "Cenas":

![](create-a-database/_static/image10.png)

Nuestra nueva tabla cenas, a continuación, se mostrará dentro de nuestra base de datos en el Explorador de servidores.

Comenzaremos, a continuación, repita los pasos anteriores y cree una tabla "RSVP". Esta tabla con tener 3 columnas. Es la columna RsvpID como la clave principal de la instalación y también hacen una columna de identidad:

![](create-a-database/_static/image11.png)

Se deberá guardarlo y asígnele el nombre "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Cómo configurar una relación de clave externa entre tablas

Ahora tenemos dos tablas dentro de nuestra base de datos. El último paso de diseño de esquema será una relación "uno a varios" entre estas dos tablas: el programa de instalación para que podamos asociamos de cada fila de la cena con cero o más filas RSVP que se aplican a él. Para hacer esto mediante la configuración de columna de la tabla RSVP "DinnerID" para que tenga una relación de clave externa a la columna "DinnerID" en la tabla "Cenas".

Para ello que se deberá abrir la tabla RSVP en el Diseñador de tablas haciendo doble clic en él en el Explorador de servidores. A continuación, seleccione la columna "DinnerID" dentro de él, menú contextual y elija el "Relationshps..." comando de menú contextual:

![](create-a-database/_static/image12.png)

Se abrirá un cuadro de diálogo que podemos usar para las relaciones de instalación entre las tablas:

![](create-a-database/_static/image13.png)

Se le haga clic en el botón "Agregar" para agregar una nueva relación en el cuadro de diálogo. Una vez que se ha agregado una relación, se podrá expandir el nodo de la vista de árbol "Especificación de tablas y columnas" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haga clic en el botón "..." a la derecha de la misma:

![](create-a-database/_static/image14.png)

Haga clic en el botón "..." se abrirá otro cuadro de diálogo que permite especificar qué tablas y columnas están implicadas en la relación, así como permiten el nombre de la relación.

Se cambia la tabla de clave principal para que sea "Cenas" y seleccione la columna "DinnerID" dentro de la tabla de cenas como clave principal. Nuestra tabla RSVP estará en la tabla de clave externa y el protocolo RSVP. Columna de DinnerID será asociado como la clave externa:

![](create-a-database/_static/image15.png)

Ahora cada fila de la tabla RSVP se asociará con una fila en la tabla de la cena. SQL Server se mantiene la integridad referencial para que podamos – y evitar que nos agregar una nueva fila RSVP si no señala a una fila de la cena válida. También nos impedirá de eliminación de una fila de la cena si hay todavía RSVP filas que hacen referencia a él.

### <a name="adding-data-to-our-tables"></a>Agregar datos a las tablas

Vamos a finalizar, agregue algunos datos de ejemplo con la tabla de cenas. Podemos agregar datos a una tabla, haga clic en él en el Explorador de servidores y elegir el comando "Mostrar datos de tabla":

![](create-a-database/_static/image16.png)

Vamos a agregar algunas filas de datos de la cena que podemos usar más adelante como empezar a implementar la aplicación:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Paso siguiente

Hemos terminado de crear la base de datos. Vamos a crear ahora las clases de modelo que podemos usar para consultar y actualizar.

> [!div class="step-by-step"]
> [Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)
