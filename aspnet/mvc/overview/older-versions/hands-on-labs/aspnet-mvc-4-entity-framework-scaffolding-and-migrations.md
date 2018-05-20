---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: Scaffolding de ASP.NET MVC 4 Entity Framework y migraciones | Documentos de Microsoft
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 42a12ee39223a06054382dbe9b4784196a706216
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>Las migraciones y Scaffolding de ASP.NET MVC 4 Entity Framework

Por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](https://aka.ms/webcamps-training-kit)

Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, deben tenerse en cuenta que muchos de la lógica para crear, actualizar, enumerar y quitar cualquier entidad de datos se repite entre la aplicación. No se mencione que, si el modelo tiene varias clases para manipular, es posible que pueda dedicar un tiempo considerable escribir los métodos de acción POST y GET para cada operación de entidad, así como cada una de las vistas.

En este laboratorio, aprenderá cómo utilizar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea de base de CRUD la aplicación (crear, leer, actualizar y eliminar). A partir de una clase de modelo simple y, sin escribir una sola línea de código, se creará un controlador que contendrá todas las operaciones CRUD, así como el todas las necesarias vistas. Después de compilar y ejecutar la solución más fácil, tendrá la base de datos de aplicación generado, junto con la lógica MVC y vistas para la manipulación de datos.

Además, obtendrá información sobre lo fácil que es usar migraciones de Entity Framework para realizar actualizaciones del modelo a lo largo de toda la aplicación. Migraciones de Entity Framework permiten modificar la base de datos después de que el modelo ha cambiado con pasos sencillos. Con todo esto en cuenta, podrá crear y mantener las aplicaciones web de forma más eficaz, sacar partido de las últimas características de ASP.NET MVC 4.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible desde en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [Entity Framework Scaffolding de ASP.NET MVC 4 y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar el scaffolding de ASP.NET para las operaciones CRUD en los controladores.
- Cambiar el modelo de base de datos usando migraciones de Entity Framework.

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

El siguiente ejercicio conforman este laboratorio práctico:

1. [Usar el Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework](#Exercise1)

> [!NOTE]
> Este ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar el ejercicio. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar en el ejercicio.


Tiempo estimado para completar esta práctica: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ejercicio 1: Usar el Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework

Scaffolding de ASP.NET MVC proporciona una manera rápida de generar las operaciones CRUD de una manera normalizada, crear la lógica necesaria que permite a las aplicaciones interactúan con el nivel de base de datos.

En este ejercicio, aprenderá a usar el scaffolding de ASP.NET MVC 4 con código en primer lugar para crear los métodos CRUD. A continuación, obtendrá información sobre cómo actualizar el modelo de aplicar los cambios en la base de datos mediante el uso de las migraciones de Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Proyecto de la tarea 1: crear una nueva versión de ASP.NET MVC 4 mediante Scaffolding

1. Si no está abierto, inicie **Visual Studio 2012**.
2. Seleccione **archivo | Nuevo proyecto**. En el icono nuevo proyecto del cuadro de diálogo, en la **Visual C# | Web** sección, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto a **MVC4andEFMigrations** y establezca la ubicación en **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** carpeta de este laboratorio. Establecer el **nombre de la solución** a **comenzar** y asegúrese de **crear directorio para la solución** está activada. Haga clic en **Aceptar**.

    ![Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4")

    *Nuevo cuadro de diálogo de proyecto de MVC de ASP.NET 4*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione la **aplicación de Internet** plantilla y asegúrese de que **Razor** está seleccionado **motor de vista**. Haga clic en **Aceptar** para crear el proyecto.

    ![Nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nueva aplicación de Internet de ASP.NET MVC 4")

    *Nueva aplicación de Internet de ASP.NET MVC 4*
4. En el Explorador de soluciones, haga clic en **modelos** y seleccione **agregar | Clase** para crear una persona de clases simple (POCO). Asígnele el nombre **persona** y haga clic en **Aceptar**.
5. Abra la clase de persona e inserte las siguientes propiedades.

    (Código de fragmento de código: *MVC de ASP.NET 4 y migraciones de Entity Framework - Ex1 persona propiedades*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Haga clic en **compilar | Compilar solución** para guardar los cambios y compile el proyecto.

    ![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilar la aplicación")

    *Compilación de la aplicación*
7. En el Explorador de soluciones, haga clic en la carpeta controllers y seleccione **agregar | Controlador**.
8. Nombre del controlador *PersonController* y completar la **opciones de Scaffolding** con los valores siguientes.

   1. En el **plantilla** lista desplegable, seleccione la **controlador de MVC con acciones de lectura/escritura y vistas, mediante Entity Framework** opción.
   2. En el **clase modelo** lista desplegable, seleccione la **persona** clase.
   3. En el **clase de contexto de datos** lista, seleccione  **&lt;nuevo contexto de datos... &gt;**. Elegir un nombre y haga clic en **Aceptar**.
   4. En el **vistas** lista desplegable lista, asegúrese de que **Razor** está seleccionada.

      ![Agregar el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "agregar el controlador de la persona con la técnica scaffolding")

      *Agregar el controlador de la persona con la técnica scaffolding*
9. Haga clic en **agregar** para crear el nuevo controlador de la persona con la técnica scaffolding. Ahora se ha generado las acciones de controlador, así como las vistas.

    ![Después de crear el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "después de crear el controlador de la persona con la técnica scaffolding")

    *Después de crear el controlador de la persona con la técnica scaffolding*
10. Abra **PersonController** clase. Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.

   ![En el controlador de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro de la persona")

   *En el controlador de persona*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarea 2: ejecutar la aplicación

En este momento, no se ha creado la base de datos. En esta tarea, ejecutará la aplicación por primera vez y probar las operaciones CRUD. Se creará la base de datos sobre la marcha con Code First.

1. Presione **F5** para ejecutar la aplicación.
2. En el explorador, agregue **/Person** a la dirección URL para abrir la página de la persona.

    ![Aplicación que se ejecuta por primera vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "aplicación ejecuta por primera vez")

    *Aplicación: primero ejecute*
3. Ahora podrá consultar las páginas de la persona y probar las operaciones CRUD.

    1. Haga clic en **crear nuevo** para agregar una nueva persona. Escriba un nombre y un apellido y haga clic en **crear**.

        ![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "agregar una nueva persona")

        *Agregar una nueva persona*
    2. En la lista de la persona, puede eliminar, editar o agregar elementos.

        ![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")

        *Lista de personas*
    3. Haga clic en **detalles** para abrir detalles de la persona.

        ![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalles de la persona")

        *Detalles de la persona*
4. Cierre el explorador y vuelva a Visual Studio. Tenga en cuenta que se ha creado el CRUD completa para la entidad person a lo largo de su aplicación - desde el modelo a las vistas, sin tener que escribir una sola línea de código.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarea 3: actualizar la base de datos usando migraciones de Entity Framework

En esta tarea se actualizará la base de datos usando migraciones de Entity Framework. Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.

1. Abra la consola de administrador de paquetes. Seleccione **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**.
2. En la consola de administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitar migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migraciones")

    *Habilitar las migraciones*

    El comando Enable-migración crea el **migraciones** carpeta, que contiene una secuencia de comandos para inicializar la base de datos.

    ![Carpeta Migrations](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "carpeta Migrations")

    *Carpeta de migraciones*
3. Abra la **archivo Configuration.cs que** archivos en la carpeta Migrations. Busque el constructor de clase y cambie la **AutomaticMigrationsEnabled** valor *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Abra la clase de persona y agregue un atributo para el segundo nombre de la persona. Con este nuevo atributo, va a cambiar el modelo.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seleccione **compilar | Compilar solución** en el menú para compilar la aplicación.

    ![Compilar la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilar la aplicación")

    *Compilar la aplicación*
6. En la consola de administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Este comando busca cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos en consecuencia.

    ![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "adición de un nombre de medio")

    *Adición de un nombre de medio*
7. (Opcional) Puede ejecutar el comando siguiente para generar un script SQL con la actualización diferencial. Esto le permitirá actualizar manualmente la base de datos (en este caso no es necesario), o aplicar los cambios en otras bases de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generar una secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generar un script SQL")

    *Generar un script SQL*

    ![Actualización de secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "actualización de secuencia de comandos SQL")

    *Actualización de secuencia de comandos SQL*
8. En la consola de administrador de paquetes, escriba el siguiente comando para actualizar la base de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Actualizar la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "actualizar la base de datos")

    *Actualizar la base de datos*

    Esto agregará la **MiddleName** columna en el **personas** tabla para que coincida con la definición actual de la **persona** clase.
9. Una vez que se actualiza la base de datos, haga clic en la carpeta de controlador y seleccione **agregar | Controlador** para agregar el controlador de persona nuevo (completada con los mismos valores). Esto actualizará los métodos existentes y las vistas de agregar el nuevo atributo.

    ![Agregar una actualización de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "agregar una actualización de controlador")

    *Actualizar el controlador*
10. Haga clic en **Agregar**. A continuación, seleccione los valores **sobrescribir PersonController.cs** y **sobrescribir vistas asociadas** y haga clic en **Aceptar**.

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Actualizar el controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4 - ejecutar la aplicación

1. Presione **F5** para ejecutar la aplicación.
2. Abra **/Person**. Tenga en cuenta que se conservan los datos, mientras que se ha agregado la columna apellido.

    ![Agrega el segundo apellido](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "agrega el segundo apellido")

    *Agrega el segundo apellido*
3. Si hace clic en **editar**, podrá agregar un segundo nombre a la persona actual.

    ![El segundo apellido edición](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edición del segundo nombre")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido sencillos pasos para crear operaciones CRUD con ASP.NET MVC 4 Scaffolding usando cualquier clase de modelo. A continuación, ha aprendido cómo realizar una actualización de extremo a extremo en su aplicación - desde la base de datos a las vistas - mediante el uso de las migraciones de Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
