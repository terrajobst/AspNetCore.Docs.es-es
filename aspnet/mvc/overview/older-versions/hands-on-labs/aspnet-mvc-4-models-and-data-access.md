---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
title: Acceso a datos y modelos de ASP.NET MVC 4 | Documentos de Microsoft
author: rick-anderson
description: 'Nota: Este laboratorio práctico se supone que tener conocimientos básicos de ASP.NET MVC. Si no ha usado ASP.NET MVC antes, le recomendamos que repase ASP.NET MVC 4...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 634ea84b-f904-4afe-b71b-49cccef4d9cc
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-models-and-data-access
msc.type: authoredcontent
ms.openlocfilehash: 88b3316b116962dd35031f4b971dbfe31ed0e010
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
ms.locfileid: "34306743"
---
# <a name="aspnet-mvc-4-models-and-data-access"></a>Acceso a datos y modelos de ASP.NET MVC 4

Por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico se da por supuesto que tiene conocimientos básicos de **ASP.NET MVC**. Si no ha usado **ASP.NET MVC** antes, le recomendamos que repase **Fundamentos de ASP.NET MVC 4** laboratorio práctico.

Esta práctica le guiará a través de las mejoras y nuevas características descritos anteriormente aplicando cambios menores a una aplicación Web de ejemplo proporcionada en la carpeta de origen.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [versiones de Microsoft-Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [acceso a datos y modelos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4ModelsAndDataAccess).

En **Fundamentos de MVC de ASP.NET** los laboratorios de prácticas, que se ha pasando datos codificados de forma rígida desde los controladores para las plantillas de vista. Sin embargo, para crear una aplicación Web real, debe usar una base de datos real.

Este laboratorio práctico le mostrará cómo usar un motor de base de datos con el fin de almacenar y recuperar los datos necesarios para la aplicación de tienda de música. Para hacerlo, iniciará con una base de datos existente y crear el Entity Data Model a partir de él. A lo largo de este laboratorio, cumplirá el **Database First** enfoque, así como la **Code First** enfoque.

Sin embargo, también puede usar el **Model First** enfocar, crear el mismo modelo de uso de las herramientas y, a continuación, generar la base de datos del mismo.

![Primer frente a base de datos. Modelo primero](aspnet-mvc-4-models-and-data-access/_static/image1.png "frente a Database First. En primer lugar del modelo")

*Primer frente a base de datos. En primer lugar del modelo*

Después de generar el modelo, hará que los ajustes adecuados en el StoreController para proporcionar las vistas del almacén con los datos tomados de la base de datos, en lugar de utilizar datos codificados de forma rígida. No necesitará realizar cualquier cambio en las plantillas de vista porque el StoreController devolverá el mismo ViewModels a las plantillas de vista, aunque esta vez los datos depende de la base de datos.

**El primer enfoque de código**

El enfoque de Code First nos permite definir el modelo desde el código sin necesidad de generar las clases que normalmente están acopladas con el marco de trabajo.

En el código en primer lugar, se definen los objetos del modelo con POCOs, &quot;objetos de CLR antiguos sin formato&quot;. POCOs son simples clases sin formato que no tienen herencia y no implementan interfaces. Podemos generar automáticamente la base de datos de ellos, o podemos usar una base de datos existente y generar la asignación de la clase desde el código.

Las ventajas de utilizar este enfoque es que el modelo sigue siendo independiente desde el marco de persistencia (en este caso, Entity Framework), como las clases de POCOs no están acopladas con el marco de trabajo de asignación.

> [!NOTE]
> Este laboratorio se basa en ASP.NET MVC 4 y una versión de la aplicación de ejemplo de la tienda de música personalizada y reducirse para ajustarse solo las características mostradas en este laboratorio práctico.
> 
> Si desea explorar todo el **tienda de música** aplicación del tutorial puede encontrarlo en [tienda de música de MVC](https://github.com/evilDave/MVC-Music-Store).


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

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

* * *

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico se compone de los ejercicios siguientes:

1. [Ejercicio 1: Agregar una base de datos](#Exercise1)
2. [Ejercicio 2: Crear una base de datos mediante Code First](#Exercise2)
3. [Ejercicio 3: Consultar la base de datos con parámetros](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **35 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Adding_a_Database"></a>
### <a name="exercise-1-adding-a-database"></a>Ejercicio 1: Agregar una base de datos

En este ejercicio, obtendrá información sobre cómo agregar una base de datos con las tablas de la aplicación de MusicStore a la solución para consumir los datos. Una vez que la base de datos se genera con el modelo y agrega a la solución, se modificará la clase StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados de forma rígida.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Adding_a_Database"></a>
#### <a name="task-1---adding-a-database"></a>Tarea 1: agregar una base de datos

En esta tarea, agregará una base de datos ya se ha creado con las tablas principales de la aplicación MusicStore a la solución.

1. Abra la **comenzar** solución ubicado en **origen/Ex1-AddingADatabaseDBFirst/Begin/** carpeta.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Agregar **MvcMusicStore** archivo de base de datos. En este laboratorio práctico, va a usar una base de datos ya se ha creado denominada **MvcMusicStore.mdf**. Para ello, haga clic en **aplicación\_datos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**. Vaya a **\Source\Assets** y seleccione la **MvcMusicStore.mdf** archivo.

    ![Agregar un elemento existente](aspnet-mvc-4-models-and-data-access/_static/image2.png "agregar un elemento existente")

    *Agregar un elemento existente*

    ![Archivo de base de datos de MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image3.png "MvcMusicStore.mdf archivo de base de datos")

    *Archivo de base de datos de MvcMusicStore.mdf*

    La base de datos se ha agregado al proyecto. Incluso cuando la base de datos se encuentra dentro de la solución, puede consultar y actualizarla a medida que se hospede en un servidor de base de datos diferente.

    ![Base de datos de MvcMusicStore en el Explorador de soluciones](aspnet-mvc-4-models-and-data-access/_static/image4.png "MvcMusicStore base de datos en el Explorador de soluciones")

    *Base de datos de MvcMusicStore en el Explorador de soluciones*
3. Compruebe la conexión a la base de datos. Para ello, haga doble clic en **MvcMusicStore.mdf** para establecer una conexión.

    ![Conectarse a MvcMusicStore.mdf](aspnet-mvc-4-models-and-data-access/_static/image5.png "conectarse a MvcMusicStore.mdf")

    *Conectarse a MvcMusicStore.mdf*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_a_Data_Model"></a>
#### <a name="task-2---creating-a-data-model"></a>Tarea 2: crear un modelo de datos

En esta tarea, creará un modelo de datos para interactuar con la base de datos que agregó en la tarea anterior.

1. Crear un modelo de datos que representa la base de datos. Para ello, en el menú contextual del explorador de soluciones la **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **nuevo elemento**. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione la **datos** plantilla y, a continuación, el **ADO.NET Entity Data Model** elemento. Cambie el nombre del modelo de datos a **StoreDB.edmx** y haga clic en **agregar**.

    ![Agregar StoreDB ADO.NET Entity Data Model](aspnet-mvc-4-models-and-data-access/_static/image6.png "agregar StoreDB ADO.NET Entity Data Model")

    *Agregar StoreDB ADO.NET Entity Data Model*
2. El **Entity Data Model Wizard** aparecerá. Este asistente le guiará a través de la creación de la capa del modelo. Puesto que el modelo se debe crear en función de la recentyl de base de datos existente agregado, seleccione **generar desde la base de datos** y haga clic en **siguiente**.

    ![Elegir el contenido del modelo](aspnet-mvc-4-models-and-data-access/_static/image7.png "elegir el contenido del modelo")

    *Elegir el contenido del modelo*
3. Puesto que está generando un modelo de una base de datos, debe especificar la conexión a la utilice. Haga clic en **nueva conexión**.
4. Seleccione **archivo de base de datos de Microsoft SQL Server** y haga clic en **continuar**.

    ![Elegir origen de datos](aspnet-mvc-4-models-and-data-access/_static/image8.png "Elegir origen de datos")

    *Elija el cuadro de diálogo de origen de datos*
5. Haga clic en **examinar** y seleccione la base de datos **MvcMusicStore.mdf** ubicado en el **aplicación\_datos** carpeta y haga clic en **Aceptar**.

    ![Propiedades de conexión](aspnet-mvc-4-models-and-data-access/_static/image9.png "propiedades de conexión")

    *Propiedades de conexión*
6. Debe tener el mismo nombre que la cadena de conexión de entidad, por lo que cambie su nombre a la clase generada **MusicStoreEntities** y haga clic en **siguiente**.

    ![Elegir la conexión de datos](aspnet-mvc-4-models-and-data-access/_static/image10.png "elegir la conexión de datos")

    *Elegir la conexión de datos*
7. Elija los objetos de base de datos que usará. Como el modelo de entidad utilizará las tablas de la base de datos, seleccione la **tablas** opción y asegúrese de que el **incluir columnas de clave externa en el modelo** y **poner en plural o en singular los generados nombres de objeto** opciones también están seleccionadas. Cambiar el Namespace de modelo para **MvcMusicStore.Model** y haga clic en **finalizar**.

    ![Elegir los objetos de base de datos](aspnet-mvc-4-models-and-data-access/_static/image11.png "elegir los objetos de base de datos")

    *Elegir los objetos de base de datos*

    > [!NOTE]
    > Si se muestra un cuadro de diálogo de advertencia de seguridad, haga clic en **Aceptar** para ejecutar la plantilla y generar las clases para las entidades del modelo.
8. Un diagrama de la entidad para la base de datos aparecerá mientras se creará una clase independiente que se asigna cada tabla a la base de datos. Por ejemplo, el **álbumes** tabla se representará mediante un **álbum** (clase), donde cada columna de la tabla se asignará a una propiedad de clase. Esto le permitirá consultar y trabajar con objetos que representan las filas de la base de datos.

    ![Diagrama de la entidad](aspnet-mvc-4-models-and-data-access/_static/image12.png "diagrama de entidad")

    *Diagrama de entidad*

    > [!NOTE]
    > Las plantillas T4 (.tt) ejecutan el código para generar las clases de entidades y las clases existentes, sobrescribirán con el mismo nombre. En este ejemplo, las clases de &quot;álbum&quot;, &quot;género&quot; y &quot;intérprete&quot; se sobrescriben con el código generado.


<a id="Ex1Task3"></a>

<a id="Task_3_-_Building_the_Application"></a>
#### <a name="task-3---building-the-application"></a>Tarea 3: creación de la aplicación

En esta tarea, va a comprobar que, aunque la generación del modelo se ha quitado el **álbum**, **género** y **intérprete** clases del modelo, el proyecto se compila correctamente mediante el uso de las nuevas clases de modelo de datos.

1. Compile el proyecto seleccionando la **generar** elemento de menú y, a continuación, **MvcMusicStore generar**.

    ![Compilar el proyecto](aspnet-mvc-4-models-and-data-access/_static/image13.png "compilar el proyecto")

    *Compilar el proyecto*
2. El proyecto se compila correctamente. ¿Por qué todavía funciona? Funciona porque las tablas de base de datos tienen campos que incluyan las propiedades que se usaba en las clases quitadas **álbum** y **género**.

    ![Compilaciones se realizó correctamente](aspnet-mvc-4-models-and-data-access/_static/image14.png "compilaciones se realizó correctamente")

    *Compilaciones se realizó correctamente*
3. Aunque el diseñador muestra las entidades en un formato de diagrama, realmente son clases de C#. Expanda el **StoreDB.edmx** nodo en el Explorador de soluciones y, a continuación, **StoreDB.tt**, verá las nuevas entidades generadas.

    ![Los archivos generados](aspnet-mvc-4-models-and-data-access/_static/image15.png "los archivos generados")

    *Archivos generados*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarea 4: consultar la base de datos

En esta tarea, actualizará la clase StoreController de modo que, en lugar de utilizar datos codificados de forma rígida, consultará la base de datos para recuperar la información.

1. Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para que contenga una instancia de la **MusicStoreEntities** (clase), denominado **storeDB**:

    (Código de fragmento de código: *modelos y acceso a datos - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample1.cs)]
2. El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla de la base de datos. Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.

    (Código de fragmento de código: *modelos y acceso a datos - Ex1 almacén examinar*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample2.cs)]

    > [!NOTE]
    > Usa una funcionalidad de .NET denominada **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutar el código en la base de datos y devolver objetos que se puede programar con.
    > 
    > Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/library/bb397926&amp;#040;v=vs.110&amp;#041;.aspx).
3. Actualización **índice** método de acción para recuperar todos los géneros.

    (Código de fragmento de código: *modelos y acceso a datos - índice de almacén Ex1*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample3.cs)]
4. Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.

    (Código de fragmento de código: *modelos y acceso a datos - Ex1 almacén GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample4.cs)]

<a id="Ex1Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que el géneros almacenados en la base de datos en lugar de los codificados de forma rígida que mostrará ahora en la página de índice de almacén. No es necesario cambiar la plantilla de vista porque el **StoreController** devuelve las mismas entidades como antes, aunque esta vez los datos depende de la base de datos.

1. Volver a generar la solución y presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Compruebe que el menú de **géneros** ya no es una lista codificada, y que los datos se recuperan directamente desde la base de datos.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image16.png)

    *Géneros exploración desde la base de datos*
3. Ahora, vaya a cualquier género y comprobar que los álbumes que se rellenan de base de datos.

    ![Exploración álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image17.png "álbumes de exploración de la base de datos")

    *Exploración álbumes de la base de datos*

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Database_Using_Code_First"></a>
### <a name="exercise-2-creating-a-database-using-code-first"></a>Ejercicio 2: Crear una base de datos utilizando código en primer lugar

En este ejercicio, obtendrá información sobre cómo usar el enfoque de Code First para crear una base de datos con las tablas de la aplicación MusicStore y cómo obtener acceso a sus datos.

Una vez que se genera el modelo, se modificará el StoreController para proporcionar la plantilla de vista con los datos tomados de la base de datos, en lugar de usar valores codificados.

> [!NOTE]
> Si se ha completado el ejercicio 1 y ya ha trabajado con el enfoque Database First, ahora obtendrá información sobre cómo obtener los mismos resultados con un proceso diferente. Las tareas que están en común con el ejercicio 1 se han marcado para facilitar su lectura. Si no se han completado el ejercicio 1, pero le gustaría obtener información sobre el enfoque de Code First, puede iniciar desde este ejercicio y obtener una cobertura total del tema.


<a id="Ex2Task1"></a>

<a id="Task_1_-_Populating_Sample_Data"></a>
#### <a name="task-1---populating-sample-data"></a>Tarea 1: rellenar de los datos de ejemplo

En esta tarea, rellenará la base de datos con datos de ejemplo cuando se crea desde el principio mediante Code First.

1. Abra la **comenzar** solución ubicado en **origen/Ex2-CreatingADatabaseCodeFirst/Begin/** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Agregar el **SampleData.cs** del archivo a la **modelos** carpeta. Para ello, haga clic en **modelos** carpeta, seleccione **agregar** y, a continuación, haga clic en **elemento existente**. Vaya a **\Source\Assets** y seleccione la **SampleData.cs** archivo.

    ![Datos de ejemplo rellenan código](aspnet-mvc-4-models-and-data-access/_static/image18.png "código de rellenar de datos de ejemplo")

    *Código de rellenar de datos de ejemplo*
3. Abra la **Global.asax.cs** de archivos y agregue el siguiente *con* instrucciones.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 Global Asax instrucciones using*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample5.cs)]
4. En el **aplicación\_Start()** método agregue la línea siguiente para establecer el inicializador de base de datos.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 Global Asax SetInitializer*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample6.cs)]

<a id="Ex2Task2"></a>

<a id="Task_2_-_Configuring_the_connection_to_the_Database"></a>
#### <a name="task-2---configuring-the-connection-to-the-database"></a>Tarea 2: configurar la conexión a la base de datos

Ahora que ya ha agregado una base de datos para el proyecto, se escribirá el **Web.config** la cadena de conexión de archivos.

1. Agregar una cadena de conexión en **Web.config**. Para ello, abra **Web.config** en la raíz del proyecto y reemplace la cadena de conexión denominado DefaultConnection con esta línea en el **&lt;connectionStrings&gt;** sección:

    ![Ubicación del archivo Web.config](aspnet-mvc-4-models-and-data-access/_static/image19.png "ubicación del archivo Web.config")

    *ubicación del archivo Web.config*

    [!code-xml[Main](aspnet-mvc-4-models-and-data-access/samples/sample7.xml)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Working_with_the_Model"></a>
#### <a name="task-3---working-with-the-model"></a>Tarea 3: trabajar con el modelo

Ahora que ya ha configurado la conexión a la base de datos, vinculará el modelo con las tablas de base de datos. En esta tarea, creará una clase que se vinculará a la base de datos con Code First. Recuerde que hay una clase de modelo POCO existente que se deben modificar.

> [!NOTE]
> Si completó el ejercicio 1, se tenga en cuenta que este paso se realiza mediante un asistente. Haciendo Code First, creará manualmente las clases que se vinculará a las entidades de datos.

1. Abra la clase de modelo POCO **género** de **modelos** carpeta del proyecto e incluir un identificador. Usar una propiedad de tipo int con el nombre **GenreId**.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 código primera género*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample8.cs)]

    > [!NOTE]
    > Para trabajar con las convenciones de Code First, el género de clase debe tener una propiedad de clave principal que se detectarán automáticamente.
    > 
    > Puede leer más acerca de las convenciones de primer código de esta [artículo de msdn](https://msdn.microsoft.com/library/hh161541&amp;#040;v=vs.103&amp;#041;.aspx).
2. Ahora, abra la clase del modelo POCO **álbum** de **modelos** carpeta del proyecto y se incluye las claves externas, cree propiedades con los nombres **GenreId** y  **ArtistId**. Esta clase ya tiene la **GenreId** para la clave principal.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 código primer álbum*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample9.cs)]
3. Abra la clase de modelo POCO **intérprete** e incluya el **ArtistId** propiedad.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 código primer intérprete*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample10.cs)]
4. Haga clic en el **modelos** carpeta del proyecto y seleccione **agregar | Clase**. Asignar nombre al archivo **MusicStoreEntities.cs**. A continuación, haga clic en **agregar.**

    ![Agregar una clase](aspnet-mvc-4-models-and-data-access/_static/image20.png "agregar una clase")

    *Agregar un nuevo elemento*

    ![Agregar una clase2](aspnet-mvc-4-models-and-data-access/_static/image21.png "agregar una clase2")

    *Agregar una clase*
5. Abra la clase que se acaba de crear, **MusicStoreEntities.cs**e incluyen los espacios de nombres **System.Data.Entity** y **System.Data.Entity.Infrastructure**.

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample11.cs)]
6. Reemplace la declaración de clase para extender la **DbContext** clase: declarar una variable public **DBSet** e invalide **OnModelCreating** método. Después de este paso, obtendrá una clase de dominio que se vinculará el modelo con Entity Framework. Para ello, reemplace el código de clase por lo siguiente:

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 código primera MusicStoreEntities*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample12.cs)]

> [!NOTE]
> Con Entity Framework **DbContext** y **DBSet** podrá consultar la clase POCO género. Extendiendo **OnModelCreating** método, se especifica en el **código** cómo género se asignarán a una tabla de base de datos. Puede encontrar más información sobre DBContext y DBSet en este artículo de msdn: [vínculo](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.103).aspx)

<a id="Ex2Task4"></a>

<a id="Task_4_-_Querying_the_Database"></a>
#### <a name="task-4---querying-the-database"></a>Tarea 4: consultar la base de datos

En esta tarea, actualizará la clase StoreController para que, en lugar de utilizar datos codificados de forma rígida, lo recuperará de la base de datos.

> [!NOTE]
> Esta tarea consiste en común con el ejercicio 1.
> 
> Si completó ejercicio 1 puede observar estos pasos son los mismos en ambos enfoques (primero la base de datos o Code first). Son diferentes en la manera en que los datos se vinculan con el modelo, pero el acceso a las entidades de datos todavía es transparente desde el controlador.


1. Abra **Controllers\StoreController.cs** y agregue el siguiente campo a la clase para que contenga una instancia de la **MusicStoreEntities** (clase), denominado **storeDB**:

    (Código de fragmento de código: *modelos y acceso a datos - Ex1 storeDB*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample13.cs)]
2. El **MusicStoreEntities** clase expone una propiedad de colección para cada tabla de la base de datos. Actualización **examinar** método de acción para recuperar un género con todos los **álbumes**.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 almacén examinar*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample14.cs)]

    > [!NOTE]
    > Usa una funcionalidad de .NET denominada **LINQ** (language integrated query) para escribir expresiones de consulta fuertemente tipada en estas colecciones - que ejecutar el código en la base de datos y devolver objetos que se puede programar con.
    > 
    > Para obtener más información acerca de LINQ, visite la [sitio msdn](https://msdn.microsoft.com/library/bb397926(v=vs.110).aspx).
3. Actualización **índice** método de acción para recuperar todos los géneros.

    (Código de fragmento de código: *modelos y acceso a datos - índice de almacén Ex2*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample15.cs)]
4. Actualización **índice** método de acción para recuperar todos los géneros y transformar la colección a una lista.

    (Código de fragmento de código: *modelos y acceso a datos - Ex2 almacén GenreMenu*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample16.cs)]

<a id="Ex2Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecutar la aplicación

En esta tarea, comprobará que el géneros almacenados en la base de datos en lugar de los codificados de forma rígida que mostrará ahora en la página de índice de almacén. No es necesario cambiar la plantilla de vista porque el **StoreController** está devolviendo los mismos **StoreIndexViewModel** como antes, pero esta vez los datos depende de la base de datos.

1. Volver a generar la solución y presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Compruebe que el menú de **géneros** ya no es una lista codificada, y que los datos se recuperan directamente desde la base de datos.

    ![BrowsingGenresFromDataBase](aspnet-mvc-4-models-and-data-access/_static/image22.png)

    *Géneros exploración desde la base de datos*
3. Ahora, vaya a cualquier género y comprobar que los álbumes que se rellenan de base de datos.

    ![Exploración álbumes de la base de datos](aspnet-mvc-4-models-and-data-access/_static/image23.png "álbumes de exploración de la base de datos")

    *Exploración álbumes de la base de datos*

<a id="Exercise3"></a>

<a id="Exercise_3_Querying_the_Database_with_Parameters"></a>
### <a name="exercise-3-querying-the-database-with-parameters"></a>Ejercicio 3: Consultar la base de datos con parámetros

En este ejercicio, obtendrá información sobre cómo consultar la base de datos con parámetros y cómo usar la forma del resultado de consulta, una característica que reduce el número base de datos tiene acceso a recuperación de datos de una manera más eficaz.

> [!NOTE]
> Para obtener más información sobre la forma de resultado de consulta, visite el siguiente [artículo de msdn](https://msdn.microsoft.com/library/bb896272&amp;#040;v=vs.100&amp;#041;.aspx).

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_StoreController_to_Retrieve_Albums_from_Database"></a>
#### <a name="task-1---modifying-storecontroller-to-retrieve-albums-from-database"></a>Tarea 1: modificar StoreController para recuperar los álbumes de base de datos

En esta tarea, cambiará la **StoreController** clase para tener acceso a la base de datos para recuperar los álbumes de un género específico.

1. Abra la **comenzar** solución se encuentra en la **Source\Ex3 QueryingTheDatabaseWithParametersCodeFirst\Begin** carpeta si desea usar el enfoque de Code First o **Source\ Ex3 QueryingTheDatabaseWithParametersDBFirst\Begin** carpeta si desea usar el enfoque basado principalmente en la base de datos. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la **StoreController** clase para cambiar la **examinar** método de acción. Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.
3. Cambiar el **examinar** método de acción para recuperar los álbumes de un género específico. Para ello, reemplace el código siguiente:

    (Código de fragmento de código: *modelos y acceso a datos - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample17.cs)]

> [!NOTE]
> Para rellenar una colección de la entidad, debe usar el **Include** método para especificar que van a recuperar los álbumes demasiado. Puede usar el. **Single()** extensión de LINQ porque en este caso se espera solo un género para un álbum. El **Single()** método toma una expresión Lambda como un parámetro, que en este caso especifica un único objeto de género tal que su nombre coincide con el valor definido.
> 
> Se aprovechará una característica que le permite indicar otras entidades relacionadas que se desea cargar también cuando se recupere el objeto de género. Esta característica se denomina **forma de resultado de consulta**y permite reducir el número de veces que sea necesario para tener acceso a la base de datos para recuperar información. En este escenario, desea realizar una captura previa de los álbumes para el género que recupera.
> 
> La consulta incluye **Genres.Include (&quot;álbumes&quot;)** para indicar que desea que también álbumes relacionados. Esto provocará en una aplicación más eficaz, ya que se recuperarán datos de género y álbum en una solicitud de base de datos único.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecutar la aplicación

En esta tarea, ejecutará la aplicación y recuperar los álbumes de un género específico de la base de datos.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/almacén/examinar? género = Pop** para comprobar que se están recuperando los resultados de la base de datos.

    ![Explorar por género](aspnet-mvc-4-models-and-data-access/_static/image24.png "explorar por género")

    *Exploración/almacén/examinar? género = Pop*

<a id="Ex3Task3"></a>

<a id="Task_3_-_Accessing_Albums_by_Id"></a>
#### <a name="task-3---accessing-albums-by-id"></a>Tarea 3: obtener acceso a los álbumes por Id.

En esta tarea, se repetirá el procedimiento anterior para obtener álbumes por su identificador.

1. Cierre el explorador si es necesario, para volver a Visual Studio. Abra la **StoreController** clase para cambiar la **detalles** método de acción. Para ello, en el **el Explorador de soluciones**, expanda la **controladores** carpeta y haga doble clic en **StoreController.cs**.
2. Cambiar el **detalles** método de acción para recuperar los detalles de álbumes en función de sus **Id. de**. Para ello, reemplace el código siguiente:

    (Código de fragmento de código: *modelos y acceso a datos - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-models-and-data-access/samples/sample18.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecutar la aplicación

En esta tarea, ejecutará la aplicación en un explorador web y obtener detalles del álbum por su identificador.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/Store/Details/51** o examinar los géneros y seleccione un álbum para comprobar que se están recuperando los resultados de la base de datos.

    ![Detalles de exploración](aspnet-mvc-4-models-and-data-access/_static/image25.png "detalles de exploración")

    *Exploración /Store/Details/51*

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).

* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Por completar este laboratorio práctico ha aprendido los conceptos básicos del acceso a datos y modelos de ASP.NET MVC, usando la **Database First** enfoque, así como la **Code First** enfoque:

- Cómo agregar una base de datos a la solución para consumir los datos
- Cómo actualizar los controladores para proporcionar las plantillas de vista con los datos tomados de la base de datos en lugar de codificado de forma rígida
- Cómo consultar la base de datos con parámetros
- Cómo usar la consulta de resultados forma, una característica que reduce el número de accesos de base de datos, recuperar datos de una manera más eficaz
- Cómo usar métodos Database First o Code First en Entity Framework de Microsoft para vincular la base de datos con el modelo

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-models-and-data-access/_static/image26.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-models-and-data-access/_static/image27.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-models-and-data-access/_static/image28.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](aspnet-mvc-4-models-and-data-access/_static/image29.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](aspnet-mvc-4-models-and-data-access/_static/image30.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web del Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde las ventanas de Portal de Azure

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-models-and-data-access/_static/image31.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](aspnet-mvc-4-models-and-data-access/_static/image32.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada al Windows Azure sitio Web desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-models-and-data-access/_static/image33.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](aspnet-mvc-4-models-and-data-access/_static/image34.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](aspnet-mvc-4-models-and-data-access/_static/image35.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](aspnet-mvc-4-models-and-data-access/_static/image36.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web a los sitios Web de Windows Azure.

    ![Descargar el sitio web de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image37.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image38.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](aspnet-mvc-4-models-and-data-access/_static/image39.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-models-and-data-access/_static/image40.png) botón.

    ![Agregar dirección IP del cliente](aspnet-mvc-4-models-and-data-access/_static/image41.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-models-and-data-access/_static/image42.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-models-and-data-access/_static/image43.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-models-and-data-access/_static/image44.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](aspnet-mvc-4-models-and-data-access/_static/image45.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](aspnet-mvc-4-models-and-data-access/_static/image46.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configurar la cadena de conexión de destino](aspnet-mvc-4-models-and-data-access/_static/image47.png "configurar la cadena de conexión de destino")

     *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](aspnet-mvc-4-models-and-data-access/_static/image48.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-models-and-data-access/_static/image49.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](aspnet-mvc-4-models-and-data-access/_static/image50.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-models-and-data-access/_static/image51.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-models-and-data-access/_static/image52.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-models-and-data-access/_static/image53.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-models-and-data-access/_static/image54.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-models-and-data-access/_static/image55.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](aspnet-mvc-4-models-and-data-access/_static/image56.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
