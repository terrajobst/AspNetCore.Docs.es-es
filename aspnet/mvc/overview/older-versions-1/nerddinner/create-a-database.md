---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Crear una base de datos | Microsoft Docs
author: microsoft
description: Paso 2 muestra los pasos para crear la base de datos que contienen todas la cena y confirme su asistencia datos para nuestra aplicación NerdDinner.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: 2a2ed8c91aa3ec0da63cd8e8686ff601f6e66374
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818971"
---
<a name="create-a-database"></a>Crear una base de datos
====================
por [Microsoft](https://github.com/microsoft)

[Descargar PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> Este es el paso 2 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.
> 
> Paso 2 muestra los pasos para crear la base de datos que contienen todas la cena y confirme su asistencia datos para nuestra aplicación NerdDinner.
> 
> Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.


## <a name="nerddinner-step-2-creating-the-database"></a>NerdDinner paso 2: Creación de la base de datos

Vamos a usar una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.

Los pasos siguientes muestran la creación de la base de datos mediante la edición gratuita de SQL Server Express (que se puede instalar fácilmente mediante V2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)). Todo el código que escribiremos funciona con SQL Server Express y la versión completa de SQL Server.

### <a name="creating-a-new-sql-server-express-database"></a>Crear una nueva base de datos SQL Server Express

Nos comenzar con el botón secundario en el proyecto web y, a continuación, seleccione el **Add -&gt;nuevo elemento** comando de menú:

![](create-a-database/_static/image1.png)

Se abrirá el cuadro de diálogo de Visual Studio "Agregar nuevo elemento". Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de elemento de "Base de datos de SQL Server":

![](create-a-database/_static/image2.png)

Llamaremos a la base de datos de SQL Server Express que deseamos crear "NerdDinner.mdf" y presione Aceptar. Visual Studio pedirá nos si deseamos agregar este archivo a nuestro \App\_directorio de datos (que es ya un directorio de instalación con la lectura y escritura listas ACL de seguridad):

![](create-a-database/_static/image3.png)

Haremos clic en "Sí", y nuestra nueva base de datos se crea y se agregará en el Explorador de soluciones:

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a>Creación de tablas dentro de nuestra base de datos

Ahora tenemos una nueva base de datos vacía. Vamos a agregar algunas tablas a él.

Para ello, se navegará a la ventana de la pestaña "Explorador de servidores" dentro de Visual Studio, que nos permite administrar servidores y bases de datos. Bases de datos de SQL Server Express almacenados en el \App\_carpeta de datos de nuestra aplicación se mostrará automáticamente en el Explorador de servidores. Opcionalmente, podemos usar el icono "Conectar a base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (locales y remotos) a la lista, así:

![](create-a-database/_static/image5.png)

Agregaremos dos tablas a nuestra base de datos de NerdDinner: uno para almacenar nuestras Dinners y otro para realizar un seguimiento de aceptaciones RSVP a ellos. Podemos crear nuevas tablas, con el botón secundario en la carpeta "Tablas" dentro de nuestra base de datos y elija el comando de menú "Agregar nueva tabla":

![](create-a-database/_static/image6.png)

Se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla. La tabla "Dinners" agregamos 10 columnas de datos:

![](create-a-database/_static/image7.png)

Queremos la columna "DinnerID" sea una clave principal única para la tabla. Podemos configurar esto, el botón secundario en la columna "DinnerID" y elija el elemento de menú "Establecer clave principal":

![](create-a-database/_static/image8.png)

Además de hacer DinnerID una clave principal, que también querremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente cuando se agregan nuevas filas de datos a la tabla (es decir, la primera fila de la cena insertada tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etcetera).

Podemos hacer esto seleccionando la columna "DinnerID" y, a continuación, utilice el editor de "Propiedades de columna" para establecer la propiedad "(es la identidad)" en la columna en "Sí". Usamos los valores predeterminados de la identidad estándar (empiezan en 1 y se incrementan 1 en cada nueva fila de la cena):

![](create-a-database/_static/image9.png)

A continuación, guardaremos nuestra tabla escribiendo Ctrl-S o usando el **archivo -&gt;guardar** comando de menú. Esto le solicitará que el nombre de la tabla. Lo llamaremos "Dinners":

![](create-a-database/_static/image10.png)

Nuestra nueva tabla de instancias Dinners, a continuación, se mostrará dentro de nuestra base de datos en el Explorador de servidores.

A continuación, se deberá repetir los pasos anteriores y crear una tabla de "RSVP". Esta tabla con tiene 3 columnas. Se programa de instalación de la columna RsvpID como clave principal y también hacen que sea en una columna de identidad:

![](create-a-database/_static/image11.png)

Se deberá guardarlo y asígnele el nombre "RSVP".

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a>Configurar una relación de clave externa entre tablas

Ahora tenemos dos tablas dentro de nuestra base de datos. El último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas: para que podamos asociamos cada fila de la cena con cero o más filas RSVP que se aplican a él. Para hacer esto mediante la configuración de la columna de la tabla RSVP "DinnerID" para que tenga una relación de clave externa a la columna "DinnerID" en la tabla "Dinners".

Para ello que se abrirá el Diseñador de tablas la tabla de RSVP haciendo doble clic en el Explorador de servidores. A continuación, seleccione la columna "DinnerID" dentro de ella, con el botón secundario y elija el comando "Relationshps..." del menú contextual:

![](create-a-database/_static/image12.png)

Se abrirá un cuadro de diálogo que podemos usar en las relaciones de programa de instalación entre las tablas:

![](create-a-database/_static/image13.png)

Haremos clic en el botón "Agregar" para agregar una nueva relación en el cuadro de diálogo. Una vez que se ha agregado una relación, deberá expandir el nodo de la vista de árbol "Especificación de tablas y columnas" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haga clic en el botón "..." a la derecha de la misma:

![](create-a-database/_static/image14.png)

Al hacer clic en el botón "..." se abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas están implicadas en la relación, así como nos permiten un nombre de la relación.

Se cambia la tabla de clave principal para que sea "Dinners" y seleccione la columna "DinnerID" dentro de la tabla de instancias Dinners como clave principal. Nuestra tabla RSVP será la tabla de clave externa y la confirmación de asistencia. Columna DinnerID se asociarán como la clave externa:

![](create-a-database/_static/image15.png)

Ahora cada fila de la tabla RSVP se asociará con una fila en la tabla de la cena. Mantener la integridad referencial para nosotros: SQL Server y nosotros impedir agregar una nueva fila RSVP si no apunta a una fila válida de la cena. También nos impedirá de eliminación de una fila de la cena si no hay todavía RSVP filas que hacen referencia a él.

### <a name="adding-data-to-our-tables"></a>Agregar datos a las tablas

Vamos a terminar mediante la adición de algunos datos de ejemplo con la tabla de instancias Dinners. Podemos agregar datos a una tabla, con el botón secundario en él en el Explorador de servidores y elija el comando "Mostrar datos de tabla":

![](create-a-database/_static/image16.png)

Vamos a agregar algunas filas de datos de la cena que podemos usar más adelante cuando empezamos a implementar la aplicación:

![](create-a-database/_static/image17.png)

### <a name="next-step"></a>Paso siguiente

Hemos terminado de crear la base de datos. Vamos a crear ahora las clases de modelo que podemos usar para consultar y actualizarlo.

> [!div class="step-by-step"]
> [Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)
