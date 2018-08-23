---
uid: mvc/overview/older-versions-1/nerddinner/create-a-database
title: Crear una base de datos | Microsoft Docs
author: microsoft
description: Paso 2 muestra los pasos para crear la base de datos que contienen todas la cena y confirme su asistencia datos para nuestra aplicación NerdDinner.
ms.author: riande
ms.date: 07/27/2010
ms.assetid: 983f3ffa-08b8-4868-b8c9-aa34593fc683
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/create-a-database
msc.type: authoredcontent
ms.openlocfilehash: b6ab0740f251889f0fa0561809cac2bbe79bcb3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836653"
---
<a name="create-a-database"></a><span data-ttu-id="1168f-103">Crear una base de datos</span><span class="sxs-lookup"><span data-stu-id="1168f-103">Create a Database</span></span>
====================
<span data-ttu-id="1168f-104">por [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="1168f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="1168f-105">Descargar PDF</span><span class="sxs-lookup"><span data-stu-id="1168f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="1168f-106">Este es el paso 2 de manera gratuita ["" la aplicación NerdDinner](introducing-the-nerddinner-tutorial.md) que le guía a través cómo compilar un pequeño, pero completa, aplicación web mediante ASP.NET MVC 1.</span><span class="sxs-lookup"><span data-stu-id="1168f-106">This is step 2 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="1168f-107">Paso 2 muestra los pasos para crear la base de datos que contienen todas la cena y confirme su asistencia datos para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="1168f-107">Step 2 shows the steps to create the database holding all of the dinner and RSVP data for our NerdDinner application.</span></span>
> 
> <span data-ttu-id="1168f-108">Si usa ASP.NET MVC 3, se recomienda que siga el [Introducción a trabajar con MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) o [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutoriales.</span><span class="sxs-lookup"><span data-stu-id="1168f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-2-creating-the-database"></a><span data-ttu-id="1168f-109">NerdDinner paso 2: Creación de la base de datos</span><span class="sxs-lookup"><span data-stu-id="1168f-109">NerdDinner Step 2: Creating the Database</span></span>

<span data-ttu-id="1168f-110">Vamos a usar una base de datos para almacenar todos los datos de la cena y RSVP para nuestra aplicación NerdDinner.</span><span class="sxs-lookup"><span data-stu-id="1168f-110">We'll be using a database to store all of the Dinner and RSVP data for our NerdDinner application.</span></span>

<span data-ttu-id="1168f-111">Los pasos siguientes muestran la creación de la base de datos mediante la edición gratuita de SQL Server Express (que se puede instalar fácilmente mediante V2 de la [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span><span class="sxs-lookup"><span data-stu-id="1168f-111">The steps below show creating the database using the free SQL Server Express edition (which you can easily install using V2 of the [Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)).</span></span> <span data-ttu-id="1168f-112">Todo el código que escribiremos funciona con SQL Server Express y la versión completa de SQL Server.</span><span class="sxs-lookup"><span data-stu-id="1168f-112">All of the code we'll write works with both SQL Server Express and the full SQL Server.</span></span>

### <a name="creating-a-new-sql-server-express-database"></a><span data-ttu-id="1168f-113">Crear una nueva base de datos SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="1168f-113">Creating a new SQL Server Express database</span></span>

<span data-ttu-id="1168f-114">Nos comenzar con el botón secundario en el proyecto web y, a continuación, seleccione el **Add -&gt;nuevo elemento** comando de menú:</span><span class="sxs-lookup"><span data-stu-id="1168f-114">We'll begin by right-clicking on our web project, and then select the **Add-&gt;New Item** menu command:</span></span>

![](create-a-database/_static/image1.png)

<span data-ttu-id="1168f-115">Se abrirá el cuadro de diálogo de Visual Studio "Agregar nuevo elemento".</span><span class="sxs-lookup"><span data-stu-id="1168f-115">This will bring up Visual Studio's "Add New Item" dialog.</span></span> <span data-ttu-id="1168f-116">Se podrá filtrar por la categoría de "Datos" y seleccione la plantilla de elemento de "Base de datos de SQL Server":</span><span class="sxs-lookup"><span data-stu-id="1168f-116">We'll filter by the "Data" category and select the "SQL Server Database" item template:</span></span>

![](create-a-database/_static/image2.png)

<span data-ttu-id="1168f-117">Llamaremos a la base de datos de SQL Server Express que deseamos crear "NerdDinner.mdf" y presione Aceptar.</span><span class="sxs-lookup"><span data-stu-id="1168f-117">We'll name the SQL Server Express database we want to create "NerdDinner.mdf" and hit ok.</span></span> <span data-ttu-id="1168f-118">Visual Studio pedirá nos si deseamos agregar este archivo a nuestro \App\_directorio de datos (que es ya un directorio de instalación con la lectura y escritura listas ACL de seguridad):</span><span class="sxs-lookup"><span data-stu-id="1168f-118">Visual Studio will then ask us if we want to add this file to our \App\_Data directory (which is a directory already setup with both read and write security ACLs):</span></span>

![](create-a-database/_static/image3.png)

<span data-ttu-id="1168f-119">Haremos clic en "Sí", y nuestra nueva base de datos se crea y se agregará en el Explorador de soluciones:</span><span class="sxs-lookup"><span data-stu-id="1168f-119">We'll click "Yes" and our new database will be created and added to our Solution Explorer:</span></span>

![](create-a-database/_static/image4.png)

### <a name="creating-tables-within-our-database"></a><span data-ttu-id="1168f-120">Creación de tablas dentro de nuestra base de datos</span><span class="sxs-lookup"><span data-stu-id="1168f-120">Creating Tables within our Database</span></span>

<span data-ttu-id="1168f-121">Ahora tenemos una nueva base de datos vacía.</span><span class="sxs-lookup"><span data-stu-id="1168f-121">We now have a new empty database.</span></span> <span data-ttu-id="1168f-122">Vamos a agregar algunas tablas a él.</span><span class="sxs-lookup"><span data-stu-id="1168f-122">Let's add some tables to it.</span></span>

<span data-ttu-id="1168f-123">Para ello, se navegará a la ventana de la pestaña "Explorador de servidores" dentro de Visual Studio, que nos permite administrar servidores y bases de datos.</span><span class="sxs-lookup"><span data-stu-id="1168f-123">To do this we'll navigate to the "Server Explorer" tab window within Visual Studio, which enables us to manage databases and servers.</span></span> <span data-ttu-id="1168f-124">Bases de datos de SQL Server Express almacenados en el \App\_carpeta de datos de nuestra aplicación se mostrará automáticamente en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="1168f-124">SQL Server Express databases stored in the \App\_Data folder of our application will automatically show up within the Server Explorer.</span></span> <span data-ttu-id="1168f-125">Opcionalmente, podemos usar el icono "Conectar a base de datos" en la parte superior de la ventana "Explorador de servidores" para agregar más bases de datos de SQL Server (locales y remotos) a la lista, así:</span><span class="sxs-lookup"><span data-stu-id="1168f-125">We can optionally use the "Connect to Database" icon on the top of the "Server Explorer" window to add additional SQL Server databases (both local and remote) to the list as well:</span></span>

![](create-a-database/_static/image5.png)

<span data-ttu-id="1168f-126">Agregaremos dos tablas a nuestra base de datos de NerdDinner: uno para almacenar nuestras Dinners y otro para realizar un seguimiento de aceptaciones RSVP a ellos.</span><span class="sxs-lookup"><span data-stu-id="1168f-126">We will add two tables to our NerdDinner database – one to store our Dinners, and the other to track RSVP acceptances to them.</span></span> <span data-ttu-id="1168f-127">Podemos crear nuevas tablas, con el botón secundario en la carpeta "Tablas" dentro de nuestra base de datos y elija el comando de menú "Agregar nueva tabla":</span><span class="sxs-lookup"><span data-stu-id="1168f-127">We can create new tables by right-clicking on the "Tables" folder within our database and choosing the "Add New Table" menu command:</span></span>

![](create-a-database/_static/image6.png)

<span data-ttu-id="1168f-128">Se abrirá un diseñador de tablas que nos permite configurar el esquema de la tabla.</span><span class="sxs-lookup"><span data-stu-id="1168f-128">This will open up a table designer that allows us to configure the schema of our table.</span></span> <span data-ttu-id="1168f-129">La tabla "Dinners" agregamos 10 columnas de datos:</span><span class="sxs-lookup"><span data-stu-id="1168f-129">For our "Dinners" table we will add 10 columns of data:</span></span>

![](create-a-database/_static/image7.png)

<span data-ttu-id="1168f-130">Queremos la columna "DinnerID" sea una clave principal única para la tabla.</span><span class="sxs-lookup"><span data-stu-id="1168f-130">We want the "DinnerID" column to be a unique primary key for the table.</span></span> <span data-ttu-id="1168f-131">Podemos configurar esto, el botón secundario en la columna "DinnerID" y elija el elemento de menú "Establecer clave principal":</span><span class="sxs-lookup"><span data-stu-id="1168f-131">We can configure this by right-clicking on the "DinnerID" column and choosing the "Set Primary Key" menu item:</span></span>

![](create-a-database/_static/image8.png)

<span data-ttu-id="1168f-132">Además de hacer DinnerID una clave principal, que también querremos configurarla como una columna de "identidad" cuyo valor se incrementa automáticamente cuando se agregan nuevas filas de datos a la tabla (es decir, la primera fila de la cena insertada tendrá un DinnerID de 1, la segunda fila insertada tendrá un DinnerID de 2, etcetera).</span><span class="sxs-lookup"><span data-stu-id="1168f-132">In addition to making DinnerID a primary key, we also want configure it as an "identity" column whose value is automatically incremented as new rows of data are added to the table (meaning the first inserted Dinner row will have a DinnerID of 1, the second inserted row will have a DinnerID of 2, etc).</span></span>

<span data-ttu-id="1168f-133">Podemos hacer esto seleccionando la columna "DinnerID" y, a continuación, utilice el editor de "Propiedades de columna" para establecer la propiedad "(es la identidad)" en la columna en "Sí".</span><span class="sxs-lookup"><span data-stu-id="1168f-133">We can do this by selecting the "DinnerID" column and then use the "Column Properties" editor to set the "(Is Identity)" property on the column to "Yes".</span></span> <span data-ttu-id="1168f-134">Usamos los valores predeterminados de la identidad estándar (empiezan en 1 y se incrementan 1 en cada nueva fila de la cena):</span><span class="sxs-lookup"><span data-stu-id="1168f-134">We will use the standard identity defaults (start at 1 and increment 1 on each new Dinner row):</span></span>

![](create-a-database/_static/image9.png)

<span data-ttu-id="1168f-135">A continuación, guardaremos nuestra tabla escribiendo Ctrl-S o usando el **archivo -&gt;guardar** comando de menú.</span><span class="sxs-lookup"><span data-stu-id="1168f-135">We'll then save our table by typing Ctrl-S or by using the **File-&gt;Save** menu command.</span></span> <span data-ttu-id="1168f-136">Esto le solicitará que el nombre de la tabla.</span><span class="sxs-lookup"><span data-stu-id="1168f-136">This will prompt us to name the table.</span></span> <span data-ttu-id="1168f-137">Lo llamaremos "Dinners":</span><span class="sxs-lookup"><span data-stu-id="1168f-137">We'll name it "Dinners":</span></span>

![](create-a-database/_static/image10.png)

<span data-ttu-id="1168f-138">Nuestra nueva tabla de instancias Dinners, a continuación, se mostrará dentro de nuestra base de datos en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="1168f-138">Our new Dinners table will then show up within our database in the server explorer.</span></span>

<span data-ttu-id="1168f-139">A continuación, se deberá repetir los pasos anteriores y crear una tabla de "RSVP".</span><span class="sxs-lookup"><span data-stu-id="1168f-139">We'll then repeat the above steps and create a "RSVP" table.</span></span> <span data-ttu-id="1168f-140">Esta tabla con tiene 3 columnas.</span><span class="sxs-lookup"><span data-stu-id="1168f-140">This table with have 3 columns.</span></span> <span data-ttu-id="1168f-141">Se programa de instalación de la columna RsvpID como clave principal y también hacen que sea en una columna de identidad:</span><span class="sxs-lookup"><span data-stu-id="1168f-141">We will setup the RsvpID column as the primary key, and also make it an identity column:</span></span>

![](create-a-database/_static/image11.png)

<span data-ttu-id="1168f-142">Se deberá guardarlo y asígnele el nombre "RSVP".</span><span class="sxs-lookup"><span data-stu-id="1168f-142">We'll save it and give it the name "RSVP".</span></span>

### <a name="setting-up-a-foreign-key-relationship-between-tables"></a><span data-ttu-id="1168f-143">Configurar una relación de clave externa entre tablas</span><span class="sxs-lookup"><span data-stu-id="1168f-143">Setting up a Foreign Key Relationship between Tables</span></span>

<span data-ttu-id="1168f-144">Ahora tenemos dos tablas dentro de nuestra base de datos.</span><span class="sxs-lookup"><span data-stu-id="1168f-144">We now have two tables within our database.</span></span> <span data-ttu-id="1168f-145">El último paso de diseño de esquema será configurar una relación "uno a varios" entre estas dos tablas: para que podamos asociamos cada fila de la cena con cero o más filas RSVP que se aplican a él.</span><span class="sxs-lookup"><span data-stu-id="1168f-145">Our last schema design step will be to setup a "one-to-many" relationship between these two tables – so that we can associate each Dinner row with zero or more RSVP rows that apply to it.</span></span> <span data-ttu-id="1168f-146">Para hacer esto mediante la configuración de la columna de la tabla RSVP "DinnerID" para que tenga una relación de clave externa a la columna "DinnerID" en la tabla "Dinners".</span><span class="sxs-lookup"><span data-stu-id="1168f-146">We will do this by configuring the RSVP table's "DinnerID" column to have a foreign-key relationship to the "DinnerID" column in the "Dinners" table.</span></span>

<span data-ttu-id="1168f-147">Para ello que se abrirá el Diseñador de tablas la tabla de RSVP haciendo doble clic en el Explorador de servidores.</span><span class="sxs-lookup"><span data-stu-id="1168f-147">To do this we'll open up the RSVP table within the table designer by double-clicking it in the server explorer.</span></span> <span data-ttu-id="1168f-148">A continuación, seleccione la columna "DinnerID" dentro de ella, con el botón secundario y elija el comando "Relationshps..." del menú contextual:</span><span class="sxs-lookup"><span data-stu-id="1168f-148">We'll then select the "DinnerID" column within it, right-click, and choose the "Relationshps…" context menu command:</span></span>

![](create-a-database/_static/image12.png)

<span data-ttu-id="1168f-149">Se abrirá un cuadro de diálogo que podemos usar en las relaciones de programa de instalación entre las tablas:</span><span class="sxs-lookup"><span data-stu-id="1168f-149">This will bring up a dialog that we can use to setup relationships between tables:</span></span>

![](create-a-database/_static/image13.png)

<span data-ttu-id="1168f-150">Haremos clic en el botón "Agregar" para agregar una nueva relación en el cuadro de diálogo.</span><span class="sxs-lookup"><span data-stu-id="1168f-150">We'll click the "Add" button to add a new relationship to the dialog.</span></span> <span data-ttu-id="1168f-151">Una vez que se ha agregado una relación, deberá expandir el nodo de la vista de árbol "Especificación de tablas y columnas" dentro de la cuadrícula de propiedades a la derecha del cuadro de diálogo y, a continuación, haga clic en el botón "..." a la derecha de la misma:</span><span class="sxs-lookup"><span data-stu-id="1168f-151">Once a relationship has been added, we'll expand the "Tables and Column Specification" tree-view node within the property grid to the right of the dialog, and then click the "…" button to the right of it:</span></span>

![](create-a-database/_static/image14.png)

<span data-ttu-id="1168f-152">Al hacer clic en el botón "..." se abrirá otro cuadro de diálogo que nos permite especificar qué tablas y columnas están implicadas en la relación, así como nos permiten un nombre de la relación.</span><span class="sxs-lookup"><span data-stu-id="1168f-152">Clicking the "…" button will bring up another dialog that allows us to specify which tables and columns are involved in the relationship, as well as allow us to name the relationship.</span></span>

<span data-ttu-id="1168f-153">Se cambia la tabla de clave principal para que sea "Dinners" y seleccione la columna "DinnerID" dentro de la tabla de instancias Dinners como clave principal.</span><span class="sxs-lookup"><span data-stu-id="1168f-153">We will change the Primary Key Table to be "Dinners", and select the "DinnerID" column within the Dinners table as the primary key.</span></span> <span data-ttu-id="1168f-154">Nuestra tabla RSVP será la tabla de clave externa y la confirmación de asistencia. Columna DinnerID se asociarán como la clave externa:</span><span class="sxs-lookup"><span data-stu-id="1168f-154">Our RSVP table will be the foreign-key table, and the RSVP.DinnerID column will be associated as the foreign-key:</span></span>

![](create-a-database/_static/image15.png)

<span data-ttu-id="1168f-155">Ahora cada fila de la tabla RSVP se asociará con una fila en la tabla de la cena.</span><span class="sxs-lookup"><span data-stu-id="1168f-155">Now each row in the RSVP table will be associated with a row in the Dinner table.</span></span> <span data-ttu-id="1168f-156">Mantener la integridad referencial para nosotros: SQL Server y nosotros impedir agregar una nueva fila RSVP si no apunta a una fila válida de la cena.</span><span class="sxs-lookup"><span data-stu-id="1168f-156">SQL Server will maintain referential integrity for us – and prevent us from adding a new RSVP row if it does not point to a valid Dinner row.</span></span> <span data-ttu-id="1168f-157">También nos impedirá de eliminación de una fila de la cena si no hay todavía RSVP filas que hacen referencia a él.</span><span class="sxs-lookup"><span data-stu-id="1168f-157">It will also prevent us from deleting a Dinner row if there are still RSVP rows referring to it.</span></span>

### <a name="adding-data-to-our-tables"></a><span data-ttu-id="1168f-158">Agregar datos a las tablas</span><span class="sxs-lookup"><span data-stu-id="1168f-158">Adding Data to our Tables</span></span>

<span data-ttu-id="1168f-159">Vamos a terminar mediante la adición de algunos datos de ejemplo con la tabla de instancias Dinners.</span><span class="sxs-lookup"><span data-stu-id="1168f-159">Let's finish by adding some sample data to our Dinners table.</span></span> <span data-ttu-id="1168f-160">Podemos agregar datos a una tabla, con el botón secundario en él en el Explorador de servidores y elija el comando "Mostrar datos de tabla":</span><span class="sxs-lookup"><span data-stu-id="1168f-160">We can add data to a table by right-clicking on it within the Server Explorer and choosing the "Show Table Data" command:</span></span>

![](create-a-database/_static/image16.png)

<span data-ttu-id="1168f-161">Vamos a agregar algunas filas de datos de la cena que podemos usar más adelante cuando empezamos a implementar la aplicación:</span><span class="sxs-lookup"><span data-stu-id="1168f-161">We'll add a few rows of Dinner data that we can use later as we start implementing the application:</span></span>

![](create-a-database/_static/image17.png)

### <a name="next-step"></a><span data-ttu-id="1168f-162">Paso siguiente</span><span class="sxs-lookup"><span data-stu-id="1168f-162">Next Step</span></span>

<span data-ttu-id="1168f-163">Hemos terminado de crear la base de datos.</span><span class="sxs-lookup"><span data-stu-id="1168f-163">We've finished creating our database.</span></span> <span data-ttu-id="1168f-164">Vamos a crear ahora las clases de modelo que podemos usar para consultar y actualizarlo.</span><span class="sxs-lookup"><span data-stu-id="1168f-164">Let's now create model classes that we can use to query and update it.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1168f-165">[Anterior](create-a-new-aspnet-mvc-project.md)
> [Siguiente](build-a-model-with-business-rule-validations.md)</span><span class="sxs-lookup"><span data-stu-id="1168f-165">[Previous](create-a-new-aspnet-mvc-project.md)
[Next](build-a-model-with-business-rule-validations.md)</span></span>
