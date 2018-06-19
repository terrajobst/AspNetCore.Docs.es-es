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
ms.locfileid: "30869145"
---
<a name="create-a-database"></a><span data-ttu-id="7f512-103">Crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="7f512-103">Create a Database</span></span>
====================
<span data-ttu-id="7f512-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="7f512-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="7f512-105">Descarga de PDF</span><span class="sxs-lookup"><span data-stu-id="7f512-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="7f512-106">Este es el paso 2 de una segunda ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que los recorridos de obtención de cómo generar una pequeña, pero completa, la aplicación web mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="7f512-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="7f512-107">Paso 2 muestra los pasos para crear la base de datos que contiene todos los de la cena y RSVP datos para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="7f512-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="7f512-108">Si usa ASP.NET MVC 3, se recomienda que siga el [obtener iniciado con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [tienda de música de MVC](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="7f512-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="7f512-109">NerdDinner paso 2: Crear la base de datos</span><span class="sxs-lookup"><span data-stu-id="7f512-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="7f512-110">Usaremos una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="7f512-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="7f512-111">Los pasos siguientes muestran la creación de la base de datos con la edición gratuita de SQL Server Express (que se puede instalar fácilmente con V2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="7f512-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="7f512-112">Todo el código que escribiremos funciona con SQL Server Express y la versión completa de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7f512-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="7f512-113">Crear una nueva base de datos SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="7f512-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="7f512-114">Se podrá comenzar con el botón secundario en nuestro proyecto web y, a continuación, seleccione la **Add -&gt;nuevo elemento** comando de menú:</span><span class="sxs-lookup"><span data-stu-id="7f512-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="7f512-115">Se abrirá el cuadro de diálogo de Visual Studio "Agregar nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="7f512-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="7f512-116">Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de elemento de "Base de datos de SQL Server":</span><span class="sxs-lookup"><span data-stu-id="7f512-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="7f512-117">Llamaremos a la base de datos de SQL Server Express que deseamos crear "NerdDinner.mdf" y pulse Aceptar.</span><span class="sxs-lookup"><span data-stu-id="7f512-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="7f512-118">Visual Studio le nos preguntará si desea agregar este archivo a nuestro \App\_directorio de datos (que es un directorio ya el programa de instalación con la lectura y escritura ACL de seguridad):</span><span class="sxs-lookup"><span data-stu-id="7f512-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="7f512-119">Haremos clic en "Sí" y la nueva base de datos se pueden crearse y agregarse en el Explorador de soluciones:</span><span class="sxs-lookup"><span data-stu-id="7f512-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="7f512-120">Crear tablas dentro de nuestra base de datos</span><span class="sxs-lookup"><span data-stu-id="7f512-120">Creating Tables within our Database</span></span>

<span data-ttu-id="7f512-121">Ahora tenemos una base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="7f512-121">We now have a new empty database.</span></span> <span data-ttu-id="7f512-122">Vamos a agregar algunas tablas a él.</span><span class="sxs-lookup"><span data-stu-id="7f512-122">Let's add some tables to it.</span></span>

<span data-ttu-id="7f512-123">Para ello, se desplazará a la ventana de la ficha "Explorador de servidores" dentro de Visual Studio, que permite administrar servidores y bases de datos.</span><span class="sxs-lookup"><span data-stu-id="7f512-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="7f512-124">Bases de datos de SQL Server Express almacenadas en el \App\_carpeta de datos de nuestra aplicación se mostrará automáticamente en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="7f512-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="7f512-125">Opcionalmente, podemos utilizar el icono "Conectar a base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (locales y remotos) a la lista como:</span><span class="sxs-lookup"><span data-stu-id="7f512-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="7f512-126">Agregará dos tablas a nuestra base de datos NerdDinner: uno para almacenar nuestra cenas y otro para realizar un seguimiento de aceptaciones RSVP a ellos.</span><span class="sxs-lookup"><span data-stu-id="7f512-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="7f512-127">Podemos crear nuevas tablas, haga doble clic en la carpeta de "Tablas" dentro de nuestra base de datos y elija el comando de menú "Agregar nueva tabla":</span><span class="sxs-lookup"><span data-stu-id="7f512-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="7f512-128">Esto se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla.</span><span class="sxs-lookup"><span data-stu-id="7f512-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="7f512-129">Para la tabla "Cenas" agregaremos 10 columnas de datos:</span><span class="sxs-lookup"><span data-stu-id="7f512-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="7f512-130">Queremos que la columna "DinnerID" para que sea una clave principal única para la tabla.</span><span class="sxs-lookup"><span data-stu-id="7f512-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="7f512-131">Podemos configurar esto, haga clic en la columna "DinnerID" y elija el elemento de menú "Establecer clave principal":</span><span class="sxs-lookup"><span data-stu-id="7f512-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="7f512-132">Además de realizar DinnerID una clave principal, también queremos configurar como una columna de "identidad" cuyo valor se incrementa automáticamente cuando se agregan nuevas filas de datos a la tabla (es decir, la primera fila insertada de cena tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etcetera).</span><span class="sxs-lookup"><span data-stu-id="7f512-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="7f512-133">Podemos hacer esto seleccionando la columna "DinnerID" y, a continuación, utilice el editor de "Propiedades de columna" para establecer la propiedad "(es la identidad)" en la columna en "Sí".</span><span class="sxs-lookup"><span data-stu-id="7f512-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="7f512-134">Se usará la identidad estándar de los valores predeterminados (empiezan en 1 y 1 en cada nueva fila de la cena de incrementar):</span><span class="sxs-lookup"><span data-stu-id="7f512-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="7f512-135">A continuación, se podrá guardar nuestra tabla escribiendo Ctrl-S o usando la **archivo -&gt;guardar** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="7f512-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="7f512-136">Se le pedirá que llame a la tabla.</span><span class="sxs-lookup"><span data-stu-id="7f512-136">This will prompt us to name the table.</span></span> <span data-ttu-id="7f512-137">Lo llamaremos "Cenas":</span><span class="sxs-lookup"><span data-stu-id="7f512-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="7f512-138">Nuestra nueva tabla cenas, a continuación, se mostrará dentro de nuestra base de datos en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="7f512-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="7f512-139">Comenzaremos, a continuación, repita los pasos anteriores y cree una tabla "RSVP".</span><span class="sxs-lookup"><span data-stu-id="7f512-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="7f512-140">Esta tabla con tener 3 columnas.</span><span class="sxs-lookup"><span data-stu-id="7f512-140">This table with have 3 columns.</span></span> <span data-ttu-id="7f512-141">Es la columna RsvpID como la clave principal de la instalación y también hacen una columna de identidad:</span><span class="sxs-lookup"><span data-stu-id="7f512-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="7f512-142">Se deberá guardarlo y asígnele el nombre "RSVP".</span><span class="sxs-lookup"><span data-stu-id="7f512-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="7f512-143">Cómo configurar una relación de clave externa entre tablas</span><span class="sxs-lookup"><span data-stu-id="7f512-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="7f512-144">Ahora tenemos dos tablas dentro de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f512-144">We now have two tables within our database.</span></span> <span data-ttu-id="7f512-145">El último paso de diseño de esquema será una relación "uno a varios" entre estas dos tablas: el programa de instalación para que podamos asociamos de cada fila de la cena con cero o más filas RSVP que se aplican a él.</span><span class="sxs-lookup"><span data-stu-id="7f512-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="7f512-146">Para hacer esto mediante la configuración de columna de la tabla RSVP "DinnerID" para que tenga una relación de clave externa a la columna "DinnerID" en la tabla "Cenas".</span><span class="sxs-lookup"><span data-stu-id="7f512-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="7f512-147">Para ello que se deberá abrir la tabla RSVP en el Diseñador de tablas haciendo doble clic en él en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="7f512-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="7f512-148">A continuación, seleccione la columna "DinnerID" dentro de él, menú contextual y elija el "Relationshps..."</span><span class="sxs-lookup"><span data-stu-id="7f512-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…"</span></span> <span data-ttu-id="7f512-149">comando de menú contextual:</span><span class="sxs-lookup"><span data-stu-id="7f512-149">context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="7f512-150">Se abrirá un cuadro de diálogo que podemos usar para las relaciones de instalación entre las tablas:</span><span class="sxs-lookup"><span data-stu-id="7f512-150">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="7f512-151">Se le haga clic en el botón "Agregar" para agregar una nueva relación en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="7f512-151">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="7f512-152">Una vez que se ha agregado una relación, se podrá expandir el nodo de la vista de árbol "Especificación de tablas y columnas" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haga clic en el botón "..." a la derecha de la misma:</span><span class="sxs-lookup"><span data-stu-id="7f512-152">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="7f512-153">Haga clic en el botón "..." se abrirá otro cuadro de diálogo que permite especificar qué tablas y columnas están implicadas en la relación, así como permiten el nombre de la relación.</span><span class="sxs-lookup"><span data-stu-id="7f512-153">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="7f512-154">Se cambia la tabla de clave principal para que sea "Cenas" y seleccione la columna "DinnerID" dentro de la tabla de cenas como clave principal.</span><span class="sxs-lookup"><span data-stu-id="7f512-154">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="7f512-155">Nuestra tabla RSVP estará en la tabla de clave externa y el protocolo RSVP. Columna de DinnerID será asociado como la clave externa:</span><span class="sxs-lookup"><span data-stu-id="7f512-155">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="7f512-156">Ahora cada fila de la tabla RSVP se asociará con una fila en la tabla de la cena.</span><span class="sxs-lookup"><span data-stu-id="7f512-156">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="7f512-157">SQL Server se mantiene la integridad referencial para que podamos – y evitar que nos agregar una nueva fila RSVP si no señala a una fila de la cena válida.</span><span class="sxs-lookup"><span data-stu-id="7f512-157">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="7f512-158">También nos impedirá de eliminación de una fila de la cena si hay todavía RSVP filas que hacen referencia a él.</span><span class="sxs-lookup"><span data-stu-id="7f512-158">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="7f512-159">Agregar datos a las tablas</span><span class="sxs-lookup"><span data-stu-id="7f512-159">Adding Data to our Tables</span></span>

<span data-ttu-id="7f512-160">Vamos a finalizar, agregue algunos datos de ejemplo con la tabla de cenas.</span><span class="sxs-lookup"><span data-stu-id="7f512-160">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="7f512-161">Podemos agregar datos a una tabla, haga clic en él en el Explorador de servidores y elegir el comando "Mostrar datos de tabla":</span><span class="sxs-lookup"><span data-stu-id="7f512-161">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="7f512-162">Vamos a agregar algunas filas de datos de la cena que podemos usar más adelante como empezar a implementar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="7f512-162">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="7f512-163">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="7f512-163">Next Step</span></span>

<span data-ttu-id="7f512-164">Hemos terminado de crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="7f512-164">We've finished creating our database.</span></span> <span data-ttu-id="7f512-165">Vamos a crear ahora las clases de modelo que podemos usar para consultar y actualizar.</span><span class="sxs-lookup"><span data-stu-id="7f512-165">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7f512-166">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="7f512-166">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
