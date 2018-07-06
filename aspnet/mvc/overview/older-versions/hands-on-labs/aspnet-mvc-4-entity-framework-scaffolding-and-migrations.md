---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
title: ASP.NET MVC 4 Entity Framework Scaffolding y migraciones | Microsoft Docs
author: rick-anderson
description: Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 093c1362-f10b-407c-a708-be370f4b62b0
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-entity-framework-scaffolding-and-migrations
msc.type: authoredcontent
ms.openlocfilehash: 31f593f294c4865f621a8556cb43d0d9c42f2660
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37814127"
---
# <a name="aspnet-mvc-4-entity-framework-scaffolding-and-migrations"></a>ASP.NET MVC 4 Entity Framework Scaffolding y migraciones

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

Si está familiarizado con los métodos de controlador de ASP.NET MVC 4 o ha completado la &quot;aplicaciones auxiliares, formularios y la validación&quot; laboratorio práctico, debe tener en cuenta que muchos de la lógica para crear, actualizar, enumerar y quitar cualquier entidad de datos se repite entre la aplicación. Para que no se menciona, si el modelo tiene varias clases para manipular, será probablemente dedicar un tiempo considerable a escribir los métodos de acción POST y GET para cada operación de entidad, así como cada una de las vistas.

En este laboratorio obtendrá información sobre cómo usar el scaffolding de ASP.NET MVC 4 para generar automáticamente la línea de base de CRUD la aplicación (creación, lectura, actualización y eliminación). A partir de una clase de modelo simple y sin necesidad de escribir una sola línea de código, se creará un controlador que contendrá todas las operaciones de CRUD, así como la todas las necesarias vistas. Después de compilar y ejecutar la solución más sencilla, tendrá la base de datos de aplicación generado, junto con la lógica MVC y vistas para la manipulación de datos.

Además, obtendrá información sobre lo fácil que es usar migraciones de Entity Framework para realizar actualizaciones del modelo a lo largo de toda la aplicación. Migraciones de Entity Framework le permitirá modificar la base de datos después de que el modelo ha cambiado con pasos sencillos. Con todo esto en mente, podrá crear y mantener las aplicaciones web de forma más eficaz, que aprovecha las características más recientes de ASP.NET MVC 4.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web campamentos, disponible desde en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [Entity Framework Scaffolding de ASP.NET MVC 4 y migraciones](https://github.com/Microsoft-Web/HOL-EntityFrameworkScaffoldingAndMigrations).

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar el scaffolding de ASP.NET para las operaciones CRUD en los controladores.
- Cambiar el modelo de base de datos mediante migraciones de Entity Framework.

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

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice B: Using Code Snippets](#AppendixB)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

El ejercicio siguiente conforman este laboratorio práctico:

1. [Uso de Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework](#Exercise1)

> [!NOTE]
> Este ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar el ejercicio. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar por el ejercicio.


Tiempo estimado para completar esta práctica: **30 minutos**

<a id="Exercise1"></a>

<a id="Exercise_1_Using_ASPNET_MVC_4_Scaffolding_with_Entity_Framework_Migrations"></a>
### <a name="exercise-1-using-aspnet-mvc-4-scaffolding-with-entity-framework-migrations"></a>Ejercicio 1: Uso de Scaffolding de ASP.NET MVC 4 con migraciones de Entity Framework

Scaffolding de ASP.NET MVC proporciona una forma rápida para generar las operaciones CRUD de manera estándar para crear la lógica necesaria que permite a las aplicaciones interactúan con la capa de base de datos.

En este ejercicio, obtendrá información sobre cómo usar el scaffolding de ASP.NET MVC 4 con código en primer lugar para crear los métodos CRUD. A continuación, obtendrá información sobre cómo actualizar el modelo de aplicar los cambios en la base de datos mediante el uso de migraciones de Entity Framework.

<a id="Ex1Task1"></a>

<a id="Task_1-_Creating_a_new_ASPNET_MVC_4_project_using_Scaffolding"></a>
#### <a name="task-1--creating-a-new-aspnet-mvc-4-project-using-scaffolding"></a>Proyecto de la tarea 1: crear una nueva versión de ASP.NET MVC 4 mediante Scaffolding

1. Si no está abierto, inicie **Visual Studio 2012**.
2. Seleccione **archivo | Nuevo proyecto**. En el nuevo proyecto de cuadro de diálogo, en el **Visual C# | Web** sección, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto a **MVC4andEFMigrations** y establezca la ubicación en **Source\Ex1 UsingMVC4ScaffoldingEFMigrations** carpeta de este laboratorio. Establecer el **nombre de la solución** a **comenzar** y asegúrese de **crear directorio para la solución** está activada. Haga clic en **Aceptar**.

    ![Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image1.png "nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4")

    *Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla y asegúrese de que **Razor** está seleccionado **delmotordevistas**. Haga clic en **Aceptar** para crear el proyecto.

    ![Nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image2.png "nueva aplicación de Internet de ASP.NET MVC 4")

    *Nueva aplicación de Internet de ASP.NET MVC 4*
4. En el Explorador de soluciones, haga clic en **modelos** y seleccione **Add | Clase** para crear una persona de la clase simple (POCO). Asígnele el nombre **persona** y haga clic en **Aceptar**.
5. Abra la clase Person e inserte las siguientes propiedades.

    (Código de fragmento de código - *ASP.NET MVC 4 y migraciones de Entity Framework - propiedades de persona Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample1.cs)]
6. Haga clic en **compilar | Compilar solución** para guardar los cambios y compile el proyecto.

    ![Creación de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image3.png "compilar la aplicación")

    *Compilación de la aplicación*
7. En el Explorador de soluciones, haga clic en la carpeta controllers y seleccione **Add | Controlador**.
8. Asigne al controlador el *PersonController* y complete el **opciones de Scaffolding** con los valores siguientes.

   1. En el **plantilla** lista desplegable, seleccione el **controlador de MVC con acciones de lectura/escritura y vistas que usan Entity Framework** opción.
   2. En el **clase modelo** lista desplegable, seleccione el **persona** clase.
   3. En el **clase de contexto de datos** lista, seleccione  **&lt;nuevo contexto de datos... &gt;**. Elegir un nombre y haga clic en **Aceptar**.
   4. En el **vistas** desplegable lista, asegúrese de que **Razor** está seleccionada.

      ![Agregar el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image4.png "adición del controlador de la persona con la técnica scaffolding")

      *Agregar el controlador de la persona con la técnica scaffolding*
9. Haga clic en **agregar** para crear el nuevo controlador para la persona con la técnica scaffolding. Ahora se ha generado las acciones de controlador, así como las vistas.

    ![Después de crear el controlador de la persona con la técnica scaffolding](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image5.png "después de crear el controlador de la persona con la técnica scaffolding")

    *Después de crear el controlador de la persona con la técnica scaffolding*
10. Abra **PersonController** clase. Tenga en cuenta que los métodos de acción CRUD completos se han generado automáticamente.

   ![En el controlador de persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image6.png "controlador dentro de la persona")

   *En el controlador de persona*

<a id="Ex1Task2"></a>

<a id="Task_2-_Running_the_application"></a>
#### <a name="task-2--running-the-application"></a>Tarea 2: ejecutar la aplicación

En este momento, no se ha creado la base de datos. En esta tarea, ejecutará la aplicación por primera vez y probar las operaciones CRUD. Se creará la base de datos sobre la marcha con Code First.

1. Presione **F5** para ejecutar la aplicación.
2. En el explorador, agregue **/Person** a la dirección URL para abrir la página de la persona.

    ![Aplicación que se ejecuta por primera vez](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image7.png "primero la ejecución de la aplicación")

    *Aplicación: primero ejecute*
3. Ahora explorará las páginas de la persona y probar las operaciones CRUD.

    1. Haga clic en **crear nuevo** para agregar una nueva persona. Escriba un nombre y un apellido y haga clic en **crear**.

        ![Agregar una nueva persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image8.png "agregando una nueva persona")

        *Agregar una nueva persona*
    2. En la lista de la persona, puede eliminar, editar o agregar elementos.

        ![lista de personas](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image9.png "lista de personas")

        *Lista de personas*
    3. Haga clic en **detalles** para abrir los detalles de la persona.

        ![Detalles de la persona](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image10.png "detalles de la persona.")

        *Detalles de la persona.*
4. Cierre el explorador y vuelva a Visual Studio. Tenga en cuenta que ha creado el CRUD completo para la entidad person a lo largo de la aplicación - desde el modelo a las vistas, sin tener que escribir una sola línea de código.

<a id="Ex1Task3"></a>

<a id="Task_3-_Updating_the_database_using_Entity_Framework_Migrations"></a>
#### <a name="task-3--updating-the-database-using-entity-framework-migrations"></a>Tarea 3: actualizando la base de datos mediante migraciones de Entity Framework

En esta tarea se actualizará la base de datos mediante migraciones de Entity Framework. Descubrirá lo fácil que es cambiar el modelo y reflejar los cambios en las bases de datos mediante la característica de migraciones de Entity Framework.

1. Abra la consola de administrador de paquetes. Seleccione **herramientas | Administrador de paquetes de biblioteca | Consola de administrador de paquetes**.
2. En la consola de administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample2.ps1)]

    ![Habilitar migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image11.png "habilitar migraciones")

    *Habilitación de migraciones*

    El comando de habilitación de la migración crea el **migraciones** carpeta, que contiene una secuencia de comandos para inicializar la base de datos.

    ![Carpeta de migraciones](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image12.png "carpeta migraciones")

    *Carpeta de migraciones*
3. Abra el **Configuration.cs** archivo en la carpeta de migraciones. Busque el constructor de clase y cambie el **AutomaticMigrationsEnabled** valor *true*.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample3.cs)]
4. Abra la clase Person y agregue un atributo para el nombre del medio. Con este nuevo atributo, va a cambiar el modelo.

    [!code-csharp[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample4.cs)]
5. Seleccione **compilar | Compilar solución** en el menú para compilar la aplicación.

    ![Creación de la aplicación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image13.png "compilar la aplicación")

    *Compilar la aplicación*
6. En la consola de administrador de paquetes, escriba el siguiente comando:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample5.ps1)]

    Este comando busca cambios en los objetos de datos y, a continuación, agregará los comandos necesarios para modificar la base de datos en consecuencia.

    ![Agregar un segundo nombre](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image14.png "agregando un segundo nombre")

    *Agregar un segundo nombre*
7. (Opcional) Puede ejecutar el siguiente comando para generar un script SQL con la actualización diferencial. Esto le permitirá actualizar manualmente la base de datos (en este caso no es necesario), o aplicar los cambios en otras bases de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample6.ps1)]

    ![Generar un script SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image15.png "generar un script SQL")

    *Generar un script SQL*

    ![Actualización de secuencia de comandos SQL](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image16.png "actualización de secuencia de comandos SQL")

    *Actualización de secuencia de comandos SQL*
8. En la consola de administrador de paquetes, escriba el siguiente comando para actualizar la base de datos:

    PMC

    [!code-powershell[Main](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/samples/sample7.ps1)]

    ![Actualizando la base de datos](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image17.png "actualizar la base de datos")

    *Actualizando la base de datos*

    Esto agregará la **MiddleName** columna en el **personas** tabla para que coincida con la definición actual de la **persona** clase.
9. Una vez que se actualiza la base de datos, haga clic en la carpeta de controlador y seleccione **Add | Controlador** para agregar el controlador de persona nuevo (terminar con los mismos valores). Esto actualizará los métodos existentes y agregar el nuevo atributo de vistas.

    ![Adición de una actualización del controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image18.png "adición de una actualización del controlador")

    *Actualizar el controlador*
10. Haga clic en **Agregar**. A continuación, seleccione los valores **sobrescribir PersonController.cs** y **sobrescritura vistas asociadas** y haga clic en **Aceptar**.

   ![Agregar una sobrescritura de controlador](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image19.png)

   *Actualizar el controlador*

<a id="Ex1Task4"></a>

<a id="Task4-_Running_the_application"></a>
#### <a name="task4--running-the-application"></a>Task4: ejecución de la aplicación

1. Presione **F5** para ejecutar la aplicación.
2. Abra **/Person**. Tenga en cuenta que se conservan los datos, mientras que se ha agregado la columna middle name.

    ![El segundo apellido agregado](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image20.png "segundo nombre agregado")

    *Segundo apellido agregado*
3. Si hace clic en **editar**, podrá agregar un segundo nombre a la persona actual.

    ![El segundo apellido edition](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image21.png "edition del segundo nombre")

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido sencillos pasos para crear operaciones CRUD con Scaffolding de ASP.NET MVC 4 mediante cualquier clase de modelo. A continuación, ha aprendido a realizar una actualización de extremo a extremo en su aplicación - desde la base de datos a las vistas - mediante migraciones de Entity Framework.

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/? linkid = 9810169](https://go.microsoft.com/?linkid=9810169)](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image22.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image23.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image24.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image25.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image26.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Using_Code_Snippets"></a>
## <a name="appendix-b-using-code-snippets"></a>Apéndice B: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image27.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image28.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image29.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image30.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image31.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-entity-framework-scaffolding-and-migrations/_static/image32.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
