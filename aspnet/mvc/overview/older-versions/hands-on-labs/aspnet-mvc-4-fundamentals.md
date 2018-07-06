---
uid: mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
title: Aspectos básicos de ASP.NET MVC 4 | Microsoft Docs
author: rick-anderson
description: Este laboratorio práctico se basa en Store música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MV...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: b7dba543-73c3-4534-a9a0-ba70fa2c6a8a
msc.legacyurl: /mvc/overview/older-versions/hands-on-labs/aspnet-mvc-4-fundamentals
msc.type: authoredcontent
ms.openlocfilehash: 2b3f8916bdca1df0dd2855f02ae46f5e5d13311a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819976"
---
# <a name="aspnet-mvc-4-fundamentals"></a>Aspectos básicos de ASP.NET MVC 4

por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](https://aka.ms/webcamps-training-kit)

Este laboratorio práctico se basa en Store música MVC (Model View Controller), una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio. En todo el laboratorio, aprenderá la simplicidad, aún más potencia de usar estas tecnologías conjuntamente. Se iniciará con una aplicación sencilla y compilará hasta que haya una aplicación de Web de ASP.NET MVC 4 totalmente funcional.

Esta práctica funciona con ASP.NET MVC 4.

Si desea explorar la versión de ASP.NET MVC 3 de la aplicación del tutorial, puede encontrarlo en [MVC-música-Store](https://github.com/evilDave/MVC-Music-Store).

Esta práctica se da por supuesto que el desarrollador tiene experiencia en las tecnologías de desarrollo Web, como HTML y JavaScript.

> [!NOTE]
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [versiones de Microsoft de Web/WebCampTrainingKit](https://aka.ms/webcamps-training-kit). Está disponible en el proyecto específico para este laboratorio [aspectos básicos de ASP.NET MVC 4](https://github.com/Microsoft-Web/HOL-MVC4Fundamentals).

<a id="The_Music_Store_application"></a>
### <a name="the-music-store-application"></a>La aplicación Music Store

La aplicación web de Music Store que se generarán a lo largo de este laboratorio consta de tres partes principales: la compra, la desprotección y la administración. Los visitantes podrán examinar álbumes por género, agregar álbumes a su carro, revise su selección y, por último, continúe con la retirada para iniciar sesión y completar el pedido. Además, los administradores del almacén podrá administrar los álbumes disponibles, así como sus propiedades principales.

![Las pantallas de Music Store](aspnet-mvc-4-fundamentals/_static/image1.png "pantallas de Music Store")

*Pantallas de Music Store*

<a id="ASPNET_MVC_4_Essentials"></a>
### <a name="aspnet-mvc-4-essentials"></a>Fundamentos de ASP.NET MVC 4

Aplicación Music Store se compilará con **Model View Controller (MVC)**, un patrón de arquitectura que separa una aplicación en tres componentes principales:

- **Modelos**: objetos de modelo son las partes de la aplicación que implementan la lógica del dominio. A menudo, los objetos del modelo también recuperar y almacenan el estado del modelo en una base de datos.
- **Vistas:** las vistas son los componentes que muestran la interfaz de usuario (UI) de la aplicación. Normalmente, esta interfaz de usuario se crea a partir de los datos del modelo. Un ejemplo sería la vista de edición de álbumes que muestra los cuadros de texto y una lista desplegable según el estado actual de un objeto del álbum.
- **Controladores:** controladores son los componentes que controlan la interacción del usuario, manipulan el modelo y por último seleccionan una vista para representar la interfaz de usuario. En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.

El patrón de MVC le ayuda a crear aplicaciones que separan los diferentes aspectos de la aplicación (lógica de entrada, lógica de negocios y lógica de la interfaz de usuario), al tiempo que proporciona un acoplamiento vago entre estos elementos. Esta separación le ayuda a administrar la complejidad al compilar una aplicación, ya que permite al usuario centrarse en uno de los aspectos de la implementación a la vez. Además, el patrón MVC facilita probar las aplicaciones, también fomenta el uso de desarrollo controlado por pruebas (TDD) para crear aplicaciones.

El **ASP.NET MVC** framework proporciona una alternativa al modelo de formularios Web Forms ASP.NET para crear aplicaciones Web basadas en ASP.NET MVC. El **ASP.NET MVC** framework es un marco de presentación de poca que (al igual que con aplicaciones basadas en Web forms) se integra con las características de ASP.NET existentes, como páginas maestras y basada en pertenencia autenticación para obtener toda la potencia de la plataforma de .NET core. Esto es útil si ya está familiarizado con ASP.NET Web Forms porque todas las bibliotecas que ya usan también están disponibles en ASP.NET MVC 4.

Además, el acoplamiento flexible entre los tres componentes principales de una aplicación MVC también favorece el desarrollo paralelo. Por ejemplo, un desarrollador puede trabajar en la vista, un segundo desarrollador puede trabajar en la lógica del controlador y un desarrollador terceros puede centrarse en la lógica de negocios en el modelo.

<a id="Objectives"></a>

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear una aplicación MVC de ASP.NET desde cero basándose en el tutorial de aplicación de Music Store
- Agregar controladores para controlar las direcciones URL a la página principal del sitio y para explorar su funcionalidad principal
- Agregar una vista para personalizar el contenido que se muestra junto con su estilo
- Agregar clases de modelo que contiene y administra la lógica de datos y el dominio
- Usar el patrón de modelo de vista para pasar información de las acciones de controlador a las plantillas de vista
- Explore la nueva plantilla de ASP.NET MVC 4 para las aplicaciones de internet

<a id="Prerequisites"></a>

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Visual Studio 2012 Express para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo)

<a id="Setup"></a>

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Esta práctica se compone de los ejercicios siguientes:

1. [Ejercicio 1: Crear el proyecto de aplicación Web de Music Store tal ASP.NET MVC](#Exercise1)
2. [Ejercicio 2: Creación de un controlador](#Exercise2)
3. [Ejercicio 3: Pasar parámetros a un controlador](#Exercise3)
4. [Ejercicio 4: Crear una vista](#Exercise4)
5. [Ejercicio 5: Crear un modelo de vista](#Exercise5)
6. [Ejercicio 6: Uso de parámetros en la vista](#Exercise6)
7. [Ejercicio 7: Un paseo por la nueva plantilla de ASP.NET MVC 4](#Exercise7)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Creating_MusicStore_ASPNET_MVC_Web_Application_Project"></a>
### <a name="exercise-1-creating-musicstore-aspnet-mvc-web-application-project"></a>Ejercicio 1: Crear el proyecto de aplicación Web de Music Store tal ASP.NET MVC

En este ejercicio, obtendrá información sobre cómo crear una aplicación ASP.NET MVC en Visual Studio 2012 Express para Web, así como su organización de la carpeta principal. Además, obtendrá información sobre cómo agregar un nuevo controlador y que se muestre una cadena sencilla en la página principal de la aplicación.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_ASPNET_MVC_Web_Application_Project"></a>
#### <a name="task-1---creating-the-aspnet-mvc-web-application-project"></a>Tarea 1: crear el proyecto de aplicación Web de ASP.NET MVC

1. En esta tarea, creará un proyecto de aplicación de ASP.NET MVC vacío mediante la plantilla de MVC de Visual Studio. Iniciar **VS Express para Web**.
2. En el menú **Archivo**, haga clic en **Nuevo proyecto**.
3. En el **nuevo proyecto** cuadro de diálogo, seleccione el **aplicación Web de ASP.NET MVC 4** tipo, ubicado en proyecto **Visual C#,** **Web** plantilla lista.
4. Cambiar el **nombre** a *MvcMusicStore*.
5. Establecer la ubicación de la solución dentro de una nueva **comenzar** carpeta en la carpeta de origen de este ejercicio, por ejemplo **\Source\Ex01-CreatingMusicStoreProject\Begin [YOUR-HOL-PATH]**. Haga clic en **Aceptar**.

    ![Crear el cuadro de diálogo nuevo proyecto](aspnet-mvc-4-fundamentals/_static/image2.png "crear el cuadro de diálogo nuevo proyecto")

    *Crear el cuadro de diálogo nuevo proyecto*
6. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **básica** plantilla y asegúrese de que el **motor de vista** seleccionado es **Razor**. Haga clic en **Aceptar**.

    ![Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image3.png "nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4")

    *Nuevo cuadro de diálogo de proyecto de ASP.NET MVC 4*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Exploring_the_Solution_Structure"></a>
#### <a name="task-2---exploring-the-solution-structure"></a>Tarea 2: explorar la estructura de solución

El marco de ASP.NET MVC incluye una plantilla de proyecto de Visual Studio que le permite crear aplicaciones Web compatibles con el patrón de MVC. Esta plantilla crea una nueva aplicación Web de ASP.NET MVC con las carpetas necesarias, las plantillas de elemento y las entradas del archivo de configuración.

En esta tarea, examinará la estructura de solución para comprender los elementos que están implicados y sus relaciones. Las siguientes carpetas se incluyen en toda la aplicación de ASP.NET MVC, porque el marco de ASP.NET MVC predeterminada usa un &quot;Convención sobre configuración&quot; enfoque y hace algunas suposiciones predeterminadas basándose en la nomenclatura de carpeta convenciones.

1. Una vez creado el proyecto, revise la estructura de carpetas que se ha creado en el Explorador de soluciones en el lado derecho:

    ![Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image4.png "estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones")

    *Estructura de carpetas de MVC de ASP.NET en el Explorador de soluciones*

   1. **Los controladores**. Esta carpeta contiene las clases del controlador. En una aplicación basada en MVC, los controladores son responsables de controlar la interacción del usuario final, manipular el modelo y en última instancia, elija una vista para representar la interfaz de usuario.

       > [!NOTE]
       > El marco MVC requiere que los nombres de todos los controladores terminen con &quot;controlador&quot;-por ejemplo, HomeController, LoginController o ProductController.
   2. **Modelos**. Esta carpeta se proporciona para las clases que representan el modelo de aplicación para la aplicación Web de MVC. Normalmente, esto incluye código que define los objetos y la lógica para interactuar con el almacén de datos. Normalmente, los objetos del modelo real será en bibliotecas de clases independientes. Sin embargo, cuando se crea una nueva aplicación, se podría incluir clases y, a continuación, muévalos a bibliotecas de clases independientes en un momento posterior del ciclo de desarrollo.
   3. **Vistas**. Esta carpeta es la ubicación recomendada para las vistas, los componentes responsables de mostrar la interfaz de usuario de la aplicación. Las vistas usan archivos .aspx, .ascx, .cshtml y. master, además de otros archivos relacionados con la representación de vistas. Carpeta Views contiene una carpeta para cada controlador; la carpeta se denomina con el prefijo del nombre del controlador. Por ejemplo, si tiene un controlador denominado **HomeController**, la carpeta Views contiene una carpeta denominada Home. De forma predeterminada, cuando el marco de MVC de ASP.NET carga una vista, busca un archivo .aspx con el nombre de la vista solicitada en la carpeta Views\controllerName (**vistas [Nombredelcontrolador] [Action] .aspx**) o (**vistas [Nombredelcontrolador] [Action] .cshtml**) para las vistas de Razor.

      > [!NOTE]
      > Además de las carpetas enumeradas anteriormente, una aplicación Web MVC usa el **Global.asax** tiene como valor predeterminado de archivo para establecer el enrutamiento global de direcciones URL, y utiliza el **Web.config** archivo para configurar la aplicación.

<a id="Ex1Task3"></a>

<a id="Task_3_-_Adding_a_HomeController"></a>
#### <a name="task-3---adding-a-homecontroller"></a>Tarea 3: agregar un HomeController

En las aplicaciones de ASP.NET que no utilizan el marco de MVC, interacción del usuario se organiza alrededor de las páginas y en torno a generar y controlar eventos desde esas páginas. En cambio, interacción del usuario con aplicaciones ASP.NET MVC se organiza en torno a los controladores y sus métodos de acción.

Por otro lado, el marco de MVC de ASP.NET asigna las direcciones URL a las clases que se conocen como controladores. Los controladores de procesar las solicitudes entrantes, controlar la entrada del usuario y las interacciones, ejecutar la lógica de aplicación adecuada y determinar la respuesta enviada al cliente (Mostrar HTML, descargar un archivo, redirigir a una dirección URL diferente, etcetera.). En el caso de mostrar HTML, una clase de controlador llama normalmente a un componente de vista independiente para generar el marcado HTML para la solicitud. En una aplicación de MVC, la vista solo muestra información; el controlador controla y responde a la interacción y los datos que introducen los usuarios.

En esta tarea, agregará una clase de controlador que controlará las direcciones URL a la página principal del sitio de Music Store.

1. Haga clic en **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, **controlador** comando:

    ![Agregar un comando controlador](aspnet-mvc-4-fundamentals/_static/image5.png "agregar un comando de controlador")

    *Agregar controlador (comando)*
2. El **Agregar controlador** aparece el cuadro de diálogo. Asigne al controlador el *HomeController* y presione **agregar**.

    ![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image6.png "Agregar cuadro de diálogo de controlador")

    *Agregar cuadro de diálogo de controlador*
3. El archivo **HomeController.cs** se crea en el **controladores** carpeta. Para poder tener la **HomeController** devuelven una cadena en su acción del índice, reemplace el **índice** método con el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex1 HomeController índice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample1.cs)]

<a id="Ex1Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, se pruebe la aplicación en un explorador web.

1. Presione **F5** para ejecutar la aplicación. El proyecto se compila y se inicia de servidor Web de IIS local. El equipo local, servidor Web de IIS se abrirá automáticamente un explorador web que apunte a la dirección URL del servidor Web.

    ![Aplicación que se ejecuta en un explorador web](aspnet-mvc-4-fundamentals/_static/image7.png "aplicación que se ejecuta en un explorador web")

    *Aplicación que se ejecuta en un explorador web*

    > [!NOTE]
    > El servidor Web de IIS local se ejecutará el sitio Web en un número de puerto libre aleatorio. En la ilustración anterior, se está ejecutando en el sitio `http://localhost:50103/`, por lo que está utilizando el puerto 50103. El número de puerto puede variar.
2. Cierre el explorador.

<a id="Exercise2"></a>

<a id="Exercise_2_Creating_a_Controller"></a>
### <a name="exercise-2-creating-a-controller"></a>Ejercicio 2: Creación de un controlador

En este ejercicio, obtendrá información sobre cómo actualizar el controlador para implementar funciones sencillas de la aplicación Music Store. Ese controlador definirá métodos de acción para cada una de las solicitudes específicas siguientes:

- Una página de listado de los géneros de música en el Store música
- Una página de exploración que enumera todos los álbumes de música de un género determinado
- Una página de detalles que muestra información sobre un álbum de música específicos

Para el ámbito de este ejercicio, esas acciones simplemente devolverá una cadena en este momento.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Adding_a_New_StoreController_Class"></a>
#### <a name="task-1---adding-a-new-storecontroller-class"></a>Tarea 1: agregar una nueva clase StoreController

En esta tarea, agregará un nuevo controlador.

1. Si no está abierto, inicie **VS Express 2012 para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex02 CreatingAController\Begin**, seleccione **Begin.sln** y haga clic en **abierto**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Agregue el nuevo controlador. Para ello, haga clic en el **controladores** carpeta en el Explorador de soluciones, seleccione **agregar** y, a continuación, el **controlador** comando. Cambiar el **nombre del controlador** a *StoreController*y haga clic en **agregar**.

    ![Agregar cuadro de diálogo controlador](aspnet-mvc-4-fundamentals/_static/image8.png "Agregar cuadro de diálogo de controlador")

    *Agregar cuadro de diálogo de controlador*

<a id="Ex2Task2"></a>

<a id="Task_2_-_Modifying_the_StoreControllers_Actions"></a>
#### <a name="task-2---modifying-the-storecontrollers-actions"></a>Tarea 2: modificar las acciones de StoreController

En esta tarea, modificará los métodos del controlador que se denominan **acciones**. Las acciones son responsables de controlar las solicitudes de dirección URL y determinar el contenido que se debe enviar en el explorador o el usuario que invocó la dirección URL.

1. El **StoreController** clase ya tiene un **índice** método. Se usará más adelante en este laboratorio para implementar la página que enumera todos los géneros de la tienda de música. Por ahora, solo tiene que reemplazar el **índice** método con el código siguiente que devuelve una cadena &quot;Hola desde Store.Index()&quot;:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex2 StoreController índice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample2.cs)]
2. Agregar **examinar** y **detalles** métodos. Para ello, agregue el código siguiente a la **StoreController**:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex2 StoreController BrowseAndDetails*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample3.cs)]

<a id="Ex2Task3"></a>

<a id="Task_3_-_Running_the_Application"></a>
#### <a name="task-3---running-the-application"></a>Tarea 3: ejecutar la aplicación

En esta tarea, se pruebe la aplicación en un explorador web.

1. Presione **F5** para ejecutar la aplicación.
2. Se inicia el proyecto en el **inicio** página. Cambie la dirección URL para comprobar la implementación de la acción.

    1. **/ Store**. Verá  **&quot;Hola desde Store.Index()&quot;**.
    2. **/ Store/examinar**. Verá  **&quot;Hola desde Store.Browse()&quot;**.
    3. **/ Store/detalles**. Verá  **&quot;Hola desde Store.Details()&quot;**.

        ![Exploración StoreBrowse](aspnet-mvc-4-fundamentals/_static/image9.png "StoreBrowse de exploración")

        *Exploración /Store/Browse*
3. Cierre el explorador.

<a id="Exercise3"></a>

<a id="Exercise_3_Passing_parameters_to_a_Controller"></a>
### <a name="exercise-3-passing-parameters-to-a-controller"></a>Ejercicio 3: Pasar parámetros a un controlador

Hasta ahora, ha estado devolviendo las cadenas constantes desde los controladores. En este ejercicio obtendrá información sobre cómo pasar parámetros a un controlador utilizando la dirección URL y la cadena de consulta y, a continuación, cómo hacer que el método acciones responder con texto en el explorador.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Adding_Genre_Parameter_to_StoreController"></a>
#### <a name="task-1---adding-genre-parameter-to-storecontroller"></a>Tarea 1: Agregar parámetro de género StoreController

En esta tarea, utilizará el **querystring** enviar parámetros a la **examinar** método de acción en el **StoreController**.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex03 PassingParametersToAController\Begin**, seleccione **Begin.sln** y haga clic en **abierto**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra **StoreController** clase. Para ello, en el **el Explorador de soluciones**, expanda el **controladores** carpeta y haga doble clic en **StoreController.cs**.
4. Cambiar el **examinar** método, agregar un parámetro de cadena a la solicitud de un género concreto. ASP.NET MVC se automáticamente pasar cualquier cadena de consulta o parámetros con nombre de envío de formulario **género** a este método de acción cuando se invoca. Para ello, reemplace el **examinar** método con el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex3 StoreController BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample4.cs)]

> [!NOTE]
> ¿Usa el **HttpUtility.HtmlEncode** método de utilidad para impide que los usuarios de inyección de Javascript en la vista con un vínculo como   **/Store/examinar? Género =&lt;script&gt;Window.Location = "[http://hackersite.com](http://hackersite.com)'&lt;/script&gt;**.
> 
> Para obtener más información, visite [este artículo de msdn](https://msdn.microsoft.com/library/a2a4yykt(v=VS.80).aspx).

<a id="Ex3Task2"></a>

<a id="Task_2_-_Running_the_Application"></a>
#### <a name="task-2---running-the-application"></a>Tarea 2: ejecutar la aplicación

En esta tarea, va a probar la aplicación en un explorador web y usar el **género** parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. Se inicia el proyecto en el **inicio** página. ¿Cambiar la dirección URL de   */Store/examinar? Género = Disco* para comprobar que la acción recibe el parámetro de género.

    ![Exploración StoreBrowseGenre = Disco](aspnet-mvc-4-fundamentals/_static/image10.png "exploración StoreBrowseGenre = Disco")

    *¿Exploración /Store/Browse? Género = Disco*
3. Cierre el explorador.

<a id="Ex3Task3"></a>

<a id="Task_3_-_Adding_an_Id_Parameter_Embedded_in_the_URL"></a>
#### <a name="task-3---adding-an-id-parameter-embedded-in-the-url"></a>Tarea 3: agregar un parámetro de identificador incrustado en la dirección URL

En esta tarea, utilizará el **URL** para pasar un **Id. de** parámetro para el **detalles** método de acción de la **StoreController**. Predeterminado de ASP.NET MVC convención de enrutamiento es tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado **Id**. Si el método de acción tiene el parámetro denominado Id, a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro. En la dirección URL **Store/detalles/5**, **Id** se interpretará como **5**.

1. Cambio la **detalles** método de la **StoreController**, agregar un **int** parámetro llamado **id**. Para ello, reemplace el **detalles** método con el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex3 StoreController DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample5.cs)]

<a id="Ex3Task4"></a>

<a id="Task_4_-_Running_the_Application"></a>
#### <a name="task-4---running-the-application"></a>Tarea 4: ejecución de la aplicación

En esta tarea, va a probar la aplicación en un explorador web y usar el **Id** parámetro.

1. Presione **F5** para ejecutar la aplicación.
2. Se inicia el proyecto en el **inicio** página. Cambiar la dirección URL de */Store/Details/5* para comprobar que la acción recibe el parámetro id.

    ![Exploración StoreDetails5](aspnet-mvc-4-fundamentals/_static/image11.png "StoreDetails5 de exploración")

    *Exploración /Store/Details/5*

<a id="Exercise4"></a>

<a id="Exercise_4_Creating_a_View"></a>
### <a name="exercise-4-creating-a-view"></a>Ejercicio 4: Crear una vista

Hasta ahora ha sido devolver cadenas acciones de controlador. Aunque es una forma útil de comprender cómo funcionan los controladores, no es, cómo se compilan las aplicaciones Web reales. Las vistas son componentes que proporcionan un mejor enfoque para generar HTML al explorador con el uso de archivos de plantilla.

En este ejercicio obtendrá información sobre cómo agregar una página principal de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia del sitio y, por último, una plantilla de vista para habilitar HomeController devolver HTML.

<a id="Ex4Task1"></a>

<a id="Task_1_-_Modifying_the_file__layoutcshtml"></a>
#### <a name="task-1---modifying-the-file-layoutcshtml"></a>Tarea 1: modificación del archivo \_layout.cshtml

El archivo **~/Views/Shared/\_layout.cshtml** permite configurar una plantilla para HTML común usar a través de todo el sitio Web. En esta tarea agregará una página principal de diseño con un encabezado común con vínculos al área de página principal y Store.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex04 CreatingAView\Begin**, seleccione **Begin.sln** y haga clic en **abierto**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. El archivo  <strong>\_layout.cshtml</strong> contiene el diseño del contenedor HTML para todas las páginas del sitio. Incluye el <strong>&lt;html&gt;</strong> (elemento) para la respuesta HTML, así como el <strong>&lt;head&gt;</strong> y <strong>&lt;cuerpo&gt;</strong> elementos. <strong>@RenderBody()</strong> dentro del HTML body identificar regiones esa vista plantillas podrá rellenar con contenido dinámico.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample6.cshtml)]
4. Agregar un encabezado común con vínculos al área de página principal y Store de todas las páginas del sitio. Para ello, agregue el código siguiente &lt;cuerpo&gt; instrucción.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample7.cshtml)]
5. Incluir un elemento div para representar la sección de cuerpo de cada página. Reemplace  <strong>@RenderBody()</strong> con el siguiente código higlighted: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample8.cshtml)]

    > [!NOTE]
    > ¿Sabía que? Visual Studio 2012 tiene fragmentos de código que facilitan agregar el código usado frecuentemente en HTML, archivos de código y mucho más! Probarlo escribiendo **&lt;div&gt;** y presionando **ficha** dos veces para insertar una completa **div** etiqueta.

<a id="Ex4Task2"></a>

<a id="Task_2_-_Adding_CSS_Stylesheet"></a>
#### <a name="task-2---adding-css-stylesheet"></a>Tarea 2: Agregar hoja de estilos CSS

La plantilla proyecto vacío incluye un archivo CSS muy simplificado que solo incluye los estilos usados para mostrar formularios básicos y los mensajes de validación. Usará adicionales CSS e imágenes (potencialmente proporcionadas un diseñador) con el fin de mejorar la apariencia del sitio.

En esta tarea, agregará una hoja de estilos CSS para definir los estilos del sitio.

1. El archivo CSS y las imágenes se incluyen en el **Source\Assets\Content** carpeta de este laboratorio. Con el fin de agregarlos a la aplicación, arrastre su contenido desde un **Windows Explorer** ventana en la **el Explorador de soluciones** en Visual Studio Express para Web, como se muestra a continuación:

    ![Arrastrar el contenido de estilo](aspnet-mvc-4-fundamentals/_static/image12.png "arrastrar el contenido de estilo")

    *Arrastrar el contenido de estilo*
2. Un cuadro de diálogo de advertencia aparecerá, le pide confirmación reemplazar **Site.css** archivo y algunas imágenes existentes. Comprobar **aplicar a todos los elementos** y haga clic en **Sí**.

<a id="Ex4Task3"></a>

<a id="Task_3_-_Adding_a_View_Template"></a>
#### <a name="task-3---adding-a-view-template"></a>Tarea 3: adición de una plantilla de vista

En esta tarea, agregará una plantilla de vista para generar la respuesta HTML que va a usar la página principal de diseño y CSS que se agregan en este ejercicio.

1. Para usar una plantilla de vista al examinar la página principal del sitio, primero debe indicar que en lugar de devolver una cadena, el **HomeController índice** método devolverá un **vista**. Abra **HomeController** clase y cambie su **índice** método devuelva un **ActionResult**, y que devuelva **View()**.

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex4 HomeController índice*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample9.cs)]
2. Ahora, deberá agregar una plantilla de vista adecuada. Para ello, **haga** dentro de la **índice** método de acción y seleccione **agregar vista**. Esto le llevará a la **agregar vista** cuadro de diálogo.

    ![Agregar una vista desde dentro del método de índice](aspnet-mvc-4-fundamentals/_static/image13.png "agregar una vista desde dentro del método de índice")

    *Agregar una vista desde dentro del método de índice*
3. El **agregar vista** aparecerá el cuadro de diálogo para generar un archivo de plantilla de vista. De forma predeterminada, este cuadro de diálogo rellena previamente el nombre de la plantilla de vista para que coincida con el método de acción que va a usar. Ya ha usado el **agregar vista** menú contextual dentro el **índice** método de acción en HomeController, la **agregar vista** cuadro de diálogo tiene el índice como el nombre de la vista predeterminada. Haga clic en **Agregar**.

    ![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image14.png "diálogo Agregar vista")

    *Agregar cuadro de diálogo de vista*
4. Visual Studio genera un **Index.cshtml** ver plantilla dentro de la **Views\Home** carpeta y, a continuación, lo abre.

    ![Inicio de la vista de índice creada](aspnet-mvc-4-fundamentals/_static/image15.png "vista de índice de inicio creada")

    *Vista de índice principal creada*

    > [!NOTE]
    > nombre y la ubicación de la **Index.cshtml** archivo es relevante y sigue las convenciones de nomenclatura predeterminada ASP.NET MVC.
    > 
    > La carpeta \Views\**inicio** coincide con el nombre del controlador (**inicio** controlador). El nombre de la plantilla de vista (**índice**), coincide con el método de acción de controlador que se va a mostrar la vista.
    > 
    > De este modo, ASP.NET MVC evita tener que especificar explícitamente el nombre o la ubicación de una plantilla de vista cuando se usa esta convención de nomenclatura para devolver una vista.
5. La plantilla de vista generada se basa en el  **\_layout.cshtml** plantilla definida anteriormente. Actualizar la propiedad ViewBag.Title a **inicio**y cambiar el contenido principal en **se trata de la página principal**, tal y como se muestra en el código siguiente:

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample10.cshtml)]
6. Seleccione **MvcMusicStore** proyecto en el Explorador de soluciones y presione **F5** para ejecutar la aplicación.

<a id="Ex4Task4"></a>

<a id="Task_4_Verification"></a>
#### <a name="task-4-verification"></a>Tarea 4: verificación

Para comprobar que ha realizado correctamente todos los pasos en el ejercicio anterior, haga lo siguiente:

Con la aplicación se abre en un explorador, debe tener en cuenta:

1. Método de acción de índice de HomeController encuentra y muestra el **\Views\Home\Index.cshtml** ver plantilla, aunque el código llamado **devolver View()**, porque la plantilla de vista seguido el convención de nomenclatura estándar.
2. La página principal muestra el mensaje de bienvenida definido dentro de la **\Views\Home\Index.cshtml** plantilla de vista.
3. Está usando la página principal de la  **\_layout.cshtml** plantilla, por lo que se encuentra el mensaje de bienvenida del diseño del sitio estándar HTML.

    ![Mediante el LayoutPage definido y el estilo de vista de índice de inicio](aspnet-mvc-4-fundamentals/_static/image16.png "vista de índice de inicio con el estilo y LayoutPage definido")

    *Vista de índice principal utilizando el estilo y LayoutPage definido*

<a id="Exercise5"></a>

<a id="Exercise_5_Creating_a_View_Model"></a>
### <a name="exercise-5-creating-a-view-model"></a>Ejercicio 5: Crear un modelo de vista

Hasta ahora, realiza sus vistas Mostrar HTML codificados, pero, con el fin de crear aplicaciones web dinámicas, la plantilla de vista debería recibir información desde el controlador. Una técnica común que se usará para ese propósito es el **ViewModel** patrón, que permite que un controlador empaquetar toda la información necesaria para generar la respuesta HTML adecuada.

En este ejercicio, obtendrá información sobre cómo crear una clase ViewModel y agregar las propiedades necesarias: el número de géneros en el almacén y una lista de los géneros. También se actualizará el StoreController para usar el modelo de vista creada y, por último, creará una nueva plantilla de vista que muestra las propiedades mencionadas en la página.

<a id="Ex5Task1"></a>

<a id="Task_1_-_Creating_a_ViewModel_Class"></a>
#### <a name="task-1---creating-a-viewmodel-class"></a>Tarea 1: creación de una clase ViewModel

En esta tarea, creará una clase ViewModel que implementará el escenario de anuncio de género de Store.

1. Si no está abierto, inicie **VS Express para Web**.
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex05 CreatingAViewModel\Begin**, seleccione **Begin.sln** y haga clic en **abierto**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Crear un **ViewModels** carpeta que contendrá el modelo de vista. Para ello, haga clic en el nivel superior **MvcMusicStore** proyecto, seleccione **agregar** y, a continuación, **nueva carpeta**.

    ![Agregar una nueva carpeta](aspnet-mvc-4-fundamentals/_static/image17.png "agregando una nueva carpeta")

    *Agregar una nueva carpeta*
4. Nombre de la carpeta *ViewModels*.

    ![Carpeta ViewModels en el Explorador de soluciones](aspnet-mvc-4-fundamentals/_static/image18.png "carpeta ViewModels en el Explorador de soluciones")

    *Carpeta ViewModels en el Explorador de soluciones*
5. Crear un **ViewModel** clase. Para ello, haga doble clic en el **ViewModels** carpeta creado recientemente, seleccione **agregar** y, a continuación, **nuevo elemento**. En **código**, elija el **clase** de elemento y asigne el nombre *StoreIndexViewModel.cs*, a continuación, haga clic en **agregar**.

    ![Agregar una nueva clase](aspnet-mvc-4-fundamentals/_static/image19.png "agregando una nueva clase")

    *Agregar una nueva clase*

    ![Crear clase StoreIndexViewModel](aspnet-mvc-4-fundamentals/_static/image20.png "StoreIndexViewModel crear clase")

    *Crear clase StoreIndexViewModel*

<a id="Ex5Task2"></a>

<a id="Task_2_-_Adding_Properties_to_the_ViewModel_class"></a>
#### <a name="task-2---adding-properties-to-the-viewmodel-class"></a>Tarea 2: agregar propiedades a la clase ViewModel

Hay dos parámetros que se pasan desde el StoreController a la plantilla de vista para generar la respuesta esperada de HTML: el número de géneros en el almacén y una lista de los géneros.

En esta tarea, agregará esas 2 propiedades para el **StoreIndexViewModel** clase: **NumberOfGenres** (entero) y **géneros** (una lista de cadenas).

1. Agregar **NumberOfGenres** y **géneros** propiedades para el **StoreIndexViewModel** clase. Para ello, agregue las siguientes líneas de 2 a la definición de clase:

    (Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - Ex5 StoreIndexViewModel propiedades*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample11.cs)]

> [!NOTE]
> El **{get; configurar;}**  notación hace uso de C# característica propiedades implementadas automáticamente. Proporciona las ventajas de una propiedad sin necesidad de nosotros declarar un campo de respaldo.

<a id="Ex5Task3"></a>

<a id="Task_3_-_Updating_StoreController_to_use_the_StoreIndexViewModel"></a>
#### <a name="task-3---updating-storecontroller-to-use-the-storeindexviewmodel"></a>Tarea 3: actualización StoreController para usar el StoreIndexViewModel

El **StoreIndexViewModel** clase encapsula la información necesaria para pasar de **StoreController**del **índice** método a una plantilla de vista con el fin de generar una respuesta .

En esta tarea, actualizará la **StoreController** para usar el **StoreIndexViewModel**.

1. Abra **StoreController** clase.

    ![Abriendo la clase StoreController](aspnet-mvc-4-fundamentals/_static/image21.png "clase StoreController de apertura")

    *Clase StoreController de apertura*
2. Para poder usar el **StoreIndexViewModel** clase desde el **StoreController**, agregue el siguiente espacio de nombres en la parte superior de la **StoreController** código:

    (Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - StoreIndexViewModel Ex5 mediante ViewModels*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample12.cs)]
3. Cambiar el **StoreController**del **índice** método de acción para que se crea y rellena un **StoreIndexViewModel** de objetos y, a continuación, pasa a una plantilla de vista generar una respuesta HTML con él.

    > [!NOTE]
    > En el laboratorio &quot;acceso a datos y modelos de MVC de ASP.NET&quot; escribirá código que recupera la lista de géneros de almacén de una base de datos. En el código siguiente, creará un **lista** de géneros datos ficticios que rellenarán el **StoreIndexViewModel**.
    > 
    > Después de crear y configurar el **StoreIndexViewModel** objeto, se pasará como un argumento para el **vista** método. Esto indica que la plantilla de vista usará ese objeto para generar una respuesta HTML con él.
4. Reemplace el **índice** método con el código siguiente:

    (Código de fragmento de código - *ASP.NET MVC 4 Fundamentals - método Ex5 StoreController Index*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample13.cs)]

> [!NOTE]
> Si no está familiarizado con C#, se puede suponer que el uso **var** significa que el **viewModel** variable está enlazado en tiempo de ejecución. Que no es correcta, el compilador de C# usa según lo que asigne a la variable de la inferencia de tipos para determinar que **viewModel** es de tipo **StoreIndexViewModel**. Además, mediante la compilación local **viewModel** variable como un **StoreIndexViewModel** escriba get comprobación en tiempo de compilación y la compatibilidad con el editor de código de Visual Studio.

<a id="Ex5Task4"></a>

<a id="Task_4_-_Creating_a_View_Template_that_Uses_StoreIndexViewModel"></a>
#### <a name="task-4---creating-a-view-template-that-uses-storeindexviewmodel"></a>Tarea 4: creación de una plantilla de vista que usa StoreIndexViewModel

En esta tarea, creará una plantilla de vista que va a usar un objeto StoreIndexViewModel pasado desde el controlador para mostrar una lista de géneros.

1. Antes de crear la nueva plantilla de vista, vamos a compilar el proyecto para que el **Agregar cuadro de diálogo de vista** conoce la **StoreIndexViewModel** clase. Compile el proyecto seleccionando el **compilar** elemento de menú y, a continuación, **MvcMusicStore compilar**.

    ![Compilar el proyecto](aspnet-mvc-4-fundamentals/_static/image22.png "compilar el proyecto")

    *Compilar el proyecto*
2. Crear una nueva plantilla de vista. Para ello, haga clic en el **índice** método y seleccione **agregar vista**.

    ![Agregar una vista](aspnet-mvc-4-fundamentals/_static/image23.png "agregar una vista")

    *Agregar una vista*
3. Dado que el **Agregar cuadro de diálogo de vista** se invocó desde el **StoreController**, agregará la plantilla de vista de forma predeterminada en un **\Views\Store\Index.cshtml** archivo. Compruebe el **crear una fuertemente tipados-vista** casilla de verificación y, a continuación, seleccione **StoreIndexViewModel** como el **clase modelo**. Además, asegúrese de que el motor de vista seleccionado es **Razor**. Haga clic en **Agregar**.

    ![Agregar cuadro de diálogo Vista](aspnet-mvc-4-fundamentals/_static/image24.png "diálogo Agregar vista")

    *Agregar cuadro de diálogo de vista*

    El **\Views\Store\Index.cshtml** se crea y abre el archivo de plantilla de vista. Según la información proporcionada a la **agregar vista** cuadro de diálogo en el último paso, la vista de plantilla se espera que sea un **StoreIndexViewModel** instancia como los datos que se va a usar para generar una respuesta HTML. Observará que la plantilla hereda un `ViewPage<musicstore.viewmodels.storeindexviewmodel>` en C#.

<a id="Ex5Task5"></a>

<a id="Task_5_-_Updating_the_View_Template"></a>
#### <a name="task-5---updating-the-view-template"></a>Tarea 5: actualización de la plantilla de vista

En esta tarea, actualizará la plantilla de vista que se creó en la última tarea para recuperar el número de géneros y sus nombres dentro de la página.

> [!NOTE]
> Usará la sintaxis @ (suele denominarse &quot;fragmentos de código&quot;) para ejecutar código dentro de la plantilla de vista.

1. En el **Index.cshtml** archivo, dentro el **Store** carpeta, reemplace el código por lo siguiente:

[!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample14.cshtml)]

    > [!NOTE]
    > As soon as you finish typing the period after the word **Model**, Visual Studio's Intellisense will show a list of possible properties and methods to choose from.
    > 
    > ![](aspnet-mvc-4-fundamentals/_static/image25.png)
    > 
    > *Getting Model properties and methods with Visual Studio's IntelliSense*
    > 
    > The **Model** property references the **StoreIndexViewModel** object that the Controller passed to the View template. This means that you can access all of the data passed from the Controller to the View template via the **Model** property, and format it into an appropriate HTML response within the View template.
    > 
    > You can just select the **NumberOfGenres** property from the Intellisense list rather than typing it in and then it will auto-complete it by pressing the **tab key**.
2. Bucle a través de la lista de género en **StoreIndexViewModel** y crear un elemento HTML **&lt;ul&gt;** lista mediante un **foreach** bucle.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample15.cshtml)]
3. Presione **F5** para ejecutar la aplicación y examinar **/Store**. Verá la lista de géneros pasa el **StoreIndexViewModel** objeto desde el **StoreController** a la plantilla de vista.

    ![Vista que muestra una lista de géneros](aspnet-mvc-4-fundamentals/_static/image26.png "vista que muestra una lista de géneros")

    *Vista que muestra una lista de géneros*
4. Cierre el explorador.

<a id="Exercise6"></a>

<a id="Exercise_6_Using_Parameters_in_View"></a>
### <a name="exercise-6-using-parameters-in-view"></a>Ejercicio 6: Uso de parámetros en la vista

En el ejercicio 3 ha aprendido cómo pasar parámetros al controlador. En este ejercicio, obtendrá información sobre cómo usar estos parámetros en la plantilla de vista. Para ello, le presentaremos primero a las clases de modelo que le ayudará a administrar la lógica de datos y de dominio. Además, obtendrá información sobre cómo crear vínculos a páginas dentro de la aplicación de ASP.NET MVC sin preocuparse de cosas como las rutas de acceso de dirección URL de codificación.

<a id="Ex6Task1"></a>

<a id="Task_1_-_Adding_Model_Classes"></a>
#### <a name="task-1---adding-model-classes"></a>Tarea 1: agregar clases de modelo

A diferencia de ViewModel, que se crea para pasar información del controlador a la vista, se crean clases de modelo que contiene y administra la lógica de datos y el dominio. En esta tarea agregará dos clases de modelo para representar estos conceptos: **género** y **álbum**.

1. Si no está abierto, inicie **VS Express para Web**
2. En el **archivo** menú, elija **Abrir proyecto**. En el cuadro de diálogo Abrir proyecto, vaya a **Source\Ex06 UsingParametersInView\Begin**, seleccione **Begin.sln** y haga clic en **abierto**. Como alternativa, puede continuar con la solución que obtuvo después de completar el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Agregar un **género** clase de modelo. Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija el **clase** de elemento y asigne el nombre *Genre.cs*, a continuación, haga clic en **agregar**.

    ![Agregar una clase](aspnet-mvc-4-fundamentals/_static/image27.png "agregar una clase")

    *Agregar un nuevo elemento*

    ![Agregar clase de modelo de género](aspnet-mvc-4-fundamentals/_static/image28.png "Agregar clase de modelo de género")

    *Agregar clase de modelo de género*
4. Agregar un **nombre** propiedad a la clase de género. Para ello, agregue el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 género*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample16.cs)]
5. El siguiente procedimiento como antes, agregar un **álbum** clase. Para ello, haga clic en el **modelos** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija el **clase** de elemento y asigne el nombre *Album.cs*, a continuación, haga clic en **agregar**.
6. Agregue dos propiedades a la clase de álbum: **género** y **título**. Para ello, agregue el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - álbum Ex6*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample17.cs)]

<a id="Ex6Task2"></a>

<a id="Task_2_-_Adding_a_StoreBrowseViewModel"></a>
#### <a name="task-2---adding-a-storebrowseviewmodel"></a>Tarea 2: agregar un StoreBrowseViewModel

Un **StoreBrowseViewModel** se usará en esta tarea para mostrar los álbumes que coinciden con un género seleccionado. En esta tarea, creará esta clase y, a continuación, agregue dos propiedades para controlar la **género** y su **álbum**de la lista.

1. Agregar un **StoreBrowseViewModel** clase. Para ello, haga clic en el **ViewModels** carpeta en el **el Explorador de soluciones**, seleccione **agregar** y, a continuación, el **nuevo elemento** opción. En **código**, elija el **clase** de elemento y asigne el nombre *StoreBrowseViewModel.cs*, a continuación, haga clic en **agregar**.
2. Agregue una referencia a los modelos en **StoreBrowseViewModel** clase. Para ello, agregue el siguiente uso de espacio de nombres:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 UsingModel*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample18.cs)]
3. Agregue dos propiedades a **StoreBrowseViewModel** clase: **género** y **álbumes**. Para ello, agregue el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 ModelProperties*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample19.cs)]

> [!NOTE]
> ¿Qué es **lista&lt;álbum&gt;**  ?: está usando esta definición la **lista&lt;T&gt;**  tipo, donde **T** restringe el tipo de los elementos de este **lista** pertenecen, en este caso **álbum** (o cualquiera de sus descendientes).
> 
> Esta capacidad para diseñar clases y métodos que aplazan la especificación de uno o varios tipos hasta que la clase o método se declara y crea una instancia de código de cliente es una característica del lenguaje C# llamado **genéricos**.
> 
> **Lista&lt;T&gt;**  es el equivalente genérico de la **ArrayList** escriba y está disponible en el **System.Collections.Generic** espacio de nombres. Una de las ventajas de usar **genéricos** es que ya que se especifica el tipo, es necesario que se encargue de operaciones como convertir los elementos en la comprobación de tipo **álbum** como lo haría con un **ArrayList**.

<a id="Ex6Task3"></a>

<a id="Task_3_-_Using_the_New_ViewModel_in_the_StoreController"></a>
#### <a name="task-3---using-the-new-viewmodel-in-the-storecontroller"></a>Tarea 3: usar el nuevo modelo de vista en el StoreController

En esta tarea, modificará el **StoreController**del **examinar** y **detalles** métodos de acción para que use el nuevo **StoreBrowseViewModel** .

1. Agregue una referencia a la carpeta modelos en **StoreController** clase. Para ello, expanda el **controladores** carpeta en el **el Explorador de soluciones** y abra el **StoreController** clase. A continuación, agregue el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 UsingModelInController*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample20.cs)]
2. Reemplace el **examinar** método de acción que se usará el **StoreViewBrowseController** clase. Creará un género y dos nuevos objetos de álbumes con datos ficticios (en la práctica siguiente consumirá datos reales de una base de datos). Para ello, reemplace el **examinar** método con el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 BrowseMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample21.cs)]
3. Reemplace el **detalles** método de acción que se usará el **StoreViewBrowseController** clase. Se creará una nueva **álbum** objeto que debe devolverse a la **vista**. Para ello, reemplace el **detalles** método con el código siguiente:

    (Código de fragmento de código - *aspectos básicos de ASP.NET MVC 4 - Ex6 DetailsMethod*)

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample22.cs)]

<a id="Ex6Task4"></a>

<a id="Task_4_-_Adding_a_Browse_View_Template"></a>
#### <a name="task-4---adding-a-browse-view-template"></a>Tarea 4: agregar una plantilla de vista de exploración

En esta tarea, agregará un **examinar** vista para mostrar los álbumes que se encuentra un género concreto.

1. Antes de crear la nueva plantilla de vista, debe compilar el proyecto para que el **agregar vista** diálogo conoce la **ViewModel** clase se debe utilizar. Compile el proyecto seleccionando el **compilar** elemento de menú y, a continuación, **MvcMusicStore compilar**.
2. Agregar un **examinar** vista. Para ello, haga clic en el **examinar** método de acción de la **StoreController** y haga clic en **agregar vista**.
3. En el **agregar vista** diálogo cuadro, compruebe que el nombre de vista es **examinar**. Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **StoreBrowseViewModel** desde el **clase modelo** lista desplegable. Deje los demás campos con sus valores predeterminados. A continuación, haga clic en **Agregar**.

    ![Agregar una vista Examinar](aspnet-mvc-4-fundamentals/_static/image29.png "agregar una vista de exploración")

    *Agregar una vista de exploración*
4. Modificar el **Browse.cshtml** para mostrar información del género, obtener acceso a la **StoreBrowseViewModel** objeto que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample23.cshtml)]

<a id="Ex6Task5"></a>

<a id="Task_5_-_Running_the_Application"></a>
#### <a name="task-5---running-the-application"></a>Tarea 5: ejecución de la aplicación

En esta tarea, probará que la **examinar** método recupera álbumes desde el **examinar** acción del método.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. ¿Cambiar la dirección URL de   **/Store/examinar? Género = Disco** para comprobar que la acción devuelve dos álbumes.

    ![Exploración de álbumes Disco Store](aspnet-mvc-4-fundamentals/_static/image30.png "Store Disco álbumes de exploración")

    *Exploración álbumes de Disco de Store*

<a id="Ex6Task6"></a>

<a id="Task_6_-_Displaying_information_About_a_Specific_Album"></a>
#### <a name="task-6---displaying-information-about-a-specific-album"></a>Tarea 6: mostrar información sobre un álbum específico

En esta tarea, implementará el **Store/detalles** vista para mostrar información sobre un álbum específico. En este laboratorio práctico, todo lo que se va a mostrar sobre el álbum ya está incluido en el **vista** plantilla. Es así, en lugar de crear un **StoreDetailsViewModel** (clase), se usará el actual **StoreBrowseViewModel** plantilla pasándole el álbum.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Agregue un nuevo **detalles** ver para el **StoreController**del **detalles** método de acción. Para ello, haga clic en el **detalles** método en el **StoreController** clase y haga clic en **agregar vista**.
2. En el **agregar vista** cuadro de diálogo, compruebe que la **nombre de la vista** es **detalles**. Compruebe el **crear una vista fuertemente tipada** casilla y seleccione **álbum** desde el **clase modelo** lista desplegable. Deje los demás campos con sus valores predeterminados. A continuación, haga clic en **Agregar**. Esto creará y abrirá una **\Views\Store\Details.cshtml** archivo.

    ![Agregar una vista de detalles](aspnet-mvc-4-fundamentals/_static/image31.png "agregar una vista de detalles")

    *Agregar una vista de detalles*
3. Modificar el **Details.cshtml** archivo para mostrar información del álbum, obtener acceso a la **álbum** objeto que se pasa a la plantilla de vista. Para ello, reemplace el contenido por lo siguiente: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample24.cshtml)]

<a id="Ex6Task7"></a>

<a id="Task_7_-_Running_the_Application"></a>
#### <a name="task-7---running-the-application"></a>Tarea 7: ejecución de la aplicación

En esta tarea, probará que la **detalles** View recupera información del álbum desde la **detalla la acción** método.

1. Presione **F5** para ejecutar la aplicación.
2. Se inicia el proyecto en el **inicio** página. Cambiar la dirección URL de **/Store/Details/5** para comprobar la información del álbum.

    ![Exploración de detalle de álbumes](aspnet-mvc-4-fundamentals/_static/image32.png "detalle álbumes de exploración")

    *Examinar los detalles del álbum*

<a id="Ex6Task8"></a>

<a id="Task_8_-_Adding_Links_Between_Pages"></a>
#### <a name="task-8---adding-links-between-pages"></a>Tarea 8: agregar vínculos entre páginas

En esta tarea, agregará un vínculo en la vista de Store para tener un vínculo en todos los nombres de género correspondientes **/Store/examinar** dirección URL. Este modo, al hacer clic en uno de estos, por ejemplo **Disco**, se le remitirá a **/Store/examinar? genre = Disco** dirección URL.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Actualización de la **índice** página para agregar un vínculo a la **examinar** página. Para ello, en el **el Explorador de soluciones** expandir la **vistas** carpeta, el **Store** carpeta y haga doble clic en el **Index.cshtml** página.
2. Agregar un vínculo a la vista de exploración que indica el género seleccionado. Para ello, reemplace el código resaltado siguiente dentro de la **&lt;li&gt;** etiquetas: (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample25.cshtml)]

   > [!NOTE]
   > otro enfoque sería se vincula directamente a la página, con un código similar al siguiente:
   > 
   > &lt;un href =&quot;/Store/examinar? genre =@genreName&quot;&gt;@genreName&lt;/a&gt;
   > 
   > Aunque este enfoque funciona, depende de una cadena estática. Si posteriormente se cambia el nombre del controlador, tendrá que cambiar manualmente esta instrucción. Una alternativa mejor es usar un **aplicación auxiliar HTML** método. ASP.NET MVC incluye un método de aplicación auxiliar HTML que está disponible para estas tareas. El **Html.ActionLink()** método auxiliar facilita la compilación HTML **&lt;un&gt;** vínculos, asegurándose de que las rutas de acceso de dirección URL están correctamente codificado como URL.
   > 
   > Htlm.ActionLink tiene varias sobrecargas. En este ejercicio usará uno que toma tres parámetros:
   > 
   > 1. Texto del vínculo, que mostrará el nombre de género
   > 2. Nombre de acción de controlador (**examinar**)
   > 3. Los valores de parámetro, especificando el nombre de ruta (**género**) y el valor (**nombre de género**)

<a id="Ex6Task9"></a>

<a id="Task_9_-_Running_the_Application"></a>
#### <a name="task-9---running-the-application"></a>Tarea 9: ejecución de la aplicación

En esta tarea, realizará las pruebas que se muestra cada género con un vínculo a la correspondiente **/Store/examinar** dirección URL.

1. Presione **F5** para ejecutar la aplicación.
2. El proyecto se inicia en la página principal. Cambiar la dirección URL de **/Store** para comprobar que cada género se vincula a la correspondiente **/Store/examinar** dirección URL.

    ![Exploración de géneros con vínculos a la página Examinar](aspnet-mvc-4-fundamentals/_static/image33.png "géneros de exploración con vínculos a la página Examinar")

    *Géneros de exploración con vínculos a la página Examinar*

<a id="Ex6Task10"></a>

<a id="Task_10_-_Using_Dynamic_ViewModel_Collection_to_Pass_Values"></a>
#### <a name="task-10---using-dynamic-viewmodel-collection-to-pass-values"></a>Tarea 10: usar la colección de ViewModel dinámicos para pasar valores

En esta tarea, obtendrá información sobre un método sencillo y eficaz para pasar valores entre el controlador y la vista sin realizar ningún cambio en el modelo. ASP.NET MVC 4 proporciona la colección &quot;ViewModel&quot;, que se pueden asignar a cualquier valor dinámico y acceder a ellos dentro de los controladores y vistas también.

Ahora usará la colección dinámica ViewBag para pasar una lista de &quot; **estrellas géneros** &quot; desde el controlador a la vista. Tendrá acceso la vista de índice Store a **ViewModel** y mostrar la información.

1. Cierre el explorador si es necesario, para volver a la ventana de Visual Studio. Abra **StoreController.cs** y modificar **índice** método para crear una lista de estrellas géneros en ViewModel de la colección:

    [!code-csharp[Main](aspnet-mvc-4-fundamentals/samples/sample26.cs)]

    > [!NOTE]
    > También puede usar la sintaxis **ViewBag [&quot;Starred&quot;]** para tener acceso a las propiedades.
2. El icono de estrella **&quot;starred.png&quot;** se incluye en el **Source\Assets\Images** carpeta de este laboratorio. Para poder agregarlo a la aplicación, arrastre su contenido desde un **Windows Explorer** ventana en la **el Explorador de soluciones** en Visual Web Developer Express, como se muestra a continuación:

    ![Agregar imagen de estrella a la solución](aspnet-mvc-4-fundamentals/_static/image34.png "Agregar imagen de estrella a la solución")

    *Agregar imagen de estrella a la solución*
3. Abra la vista **Store/Index.cshtml** y modificar el contenido. Leerá el &quot;estrellas&quot; propiedad en el **ViewBag** colección y pregunte si es el nombre del género actual en la lista. En ese caso, se mostrará un icono de estrella directamente en el vínculo de género.
   (C#)

    [!code-cshtml[Main](aspnet-mvc-4-fundamentals/samples/sample27.cshtml)]

<a id="Ex6Task11"></a>

<a id="Task_11_-_Running_the_Application"></a>
#### <a name="task-11---running-the-application"></a>Tarea 11: ejecución de la aplicación

En esta tarea, realizará las pruebas que los géneros con estrellas mostrar un icono de estrella.

1. Presione **F5** para ejecutar la aplicación.
2. Se inicia el proyecto en el **inicio** página. Cambiar la dirección URL de **/Store** para comprobar que cada género destacado tiene la etiqueta respete a sí:

    ![Exploración de géneros con elementos con estrellas](aspnet-mvc-4-fundamentals/_static/image35.png "géneros de exploración con elementos con estrellas")

    *Exploración géneros con elementos con estrellas*

<a id="Exercise7"></a>

<a id="Exercise_7_A_lap_around_ASPNET_MVC_4_new_template"></a>
### <a name="exercise-7-a-lap-around-aspnet-mvc-4-new-template"></a>Ejercicio 7: Un paseo por la nueva plantilla de ASP.NET MVC 4

En este ejercicio, explorará las mejoras en las plantillas de proyecto de ASP.NET MVC 4, echar un vistazo las características más relevantes de la nueva plantilla.

<a id="Ex7Task1"></a>

<a id="Task_1_Exploring_the_ASPNET_MVC_4_Internet_Application_Template"></a>
#### <a name="task-1-exploring-the-aspnet-mvc-4-internet-application-template"></a>Tarea 1: Exploración de la plantilla de aplicación de Internet de ASP.NET MVC 4

1. Si no está abierto, inicie **VS Express para Web**
2. Seleccione el **archivo | Nuevo | Proyecto** comando de menú. En el **nuevo proyecto** cuadro de diálogo, seleccione el **Visual C# | Web** plantilla en el panel izquierdo del árbol y elija el **aplicación Web de ASP.NET MVC 4**. **Nombre** el proyecto *Music Store tal* y actualizar la **nombre de la solución** a *comenzar*, a continuación, seleccione una ubicación (o deje el valor predeterminado) y haga clic en **Aceptar** .

    ![Crear un nuevo proyecto de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image36.png "crear un nuevo proyecto de ASP.NET MVC 4")

    *Crear un nuevo proyecto de ASP.NET MVC 4*
3. En el **nuevo proyecto de ASP.NET MVC 4** cuadro de diálogo, seleccione el **aplicación de Internet** plantilla de proyecto y haga clic en **Aceptar**. Tenga en cuenta que puede seleccionar como el motor de vistas Razor o ASPX.

    ![Crear una nueva aplicación de Internet de ASP.NET MVC 4](aspnet-mvc-4-fundamentals/_static/image37.png "creando una nueva aplicación de Internet de ASP.NET MVC 4")

    *Crear una nueva aplicación de Internet de ASP.NET MVC 4*

    > [!NOTE]
    > Se ha introducido la sintaxis de Razor en ASP.NET MVC 3. Su objetivo es minimizar el número de caracteres y pulsaciones de teclas necesarias en un archivo, lo que permite un rápido y fluido de flujo de trabajo de codificación. Razor aprovecha existente C# / VB (u otros) habilidades lingüísticas y ofrece una sintaxis de marcado de plantilla que permite un flujo de trabajo de construcción de HTML impresionante.
4. Presione **F5** para ejecutar la solución y ver la plantilla renovada. Puede comprobar las características siguientes:

    1. **Plantillas de estilo moderno**

        Las plantillas que se han renovado, proporcionando más estilos de aspecto moderno.

        ![Las plantillas de ASP.NET MVC 4 se ha rediseñado](aspnet-mvc-4-fundamentals/_static/image38.png "vuelto a diseñar plantillas de ASP.NET MVC 4")

        *Plantillas de ASP.NET MVC 4 se ha rediseñado*
    2. **Procesamiento adaptable**

        Consulte Cambiar el tamaño de la ventana del explorador y observe cómo el diseño de página se adapta dinámicamente para el nuevo tamaño de ventana. Estas plantillas usan la técnica de procesamiento adaptable se presenten correctamente en las plataformas móviles y de escritorio sin ninguna personalización.

        ![Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente](aspnet-mvc-4-fundamentals/_static/image39.png "plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente")

        *Plantilla de proyecto de ASP.NET MVC 4 en tamaños de explorador diferente*
5. Cierre el explorador para detener al depurador y vuelva a Visual Studio.
6. Ahora pueden explorar la solución y consulte algunas de las nuevas características introducidas por ASP.NET MVC 4 en la plantilla de proyecto.

    ![ASP.NET MVC4-internet-de-plantilla de proyecto aplicación](aspnet-mvc-4-fundamentals/_static/image40.png "la plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4")

    *La plantilla de proyecto de aplicación de Internet de ASP.NET MVC 4*

   1. **Marcado de HTML5**

       Examinar vistas de la plantilla para averiguar el marcado nuevo tema, por ejemplo open **About.cshtml** ver dentro de **inicio** carpeta.

       ![Nueva plantilla, usando un marcado de Razor y HTML5](aspnet-mvc-4-fundamentals/_static/image41.png "nueva plantilla, usando un marcado de Razor y HTML5")

       *Nueva plantilla, usando un marcado de Razor y HTML5*
   2. **Bibliotecas de JavaScript incluidas**

      1. **jQuery**: jQuery simplifica atravesar de documento HTML, control de eventos, animar y las interacciones de Ajax.
      2. **jQuery UI**: esta biblioteca proporciona abstracciones de interacción de bajo nivel y animación, advanced efectos y temas widgets, creados a partir de la biblioteca JavaScript jQuery.

         > [!NOTE]
         > Puede obtener información acerca de jQuery y jQuery UI en [ [ http://docs.jquery.com/ ](http://docs.jquery.com/) ](http://docs.jquery.com/).
      3. **KnockoutJS**: plantilla predeterminada de las ASP.NET MVC 4 ahora incluye **KnockoutJS**, un marco MVVM JavaScript que le permite crear aplicaciones web enriquecidas y alta capacidad de respuesta con JavaScript y HTML. Al igual que en ASP.NET MVC 3, jQuery y bibliotecas de interfaz de usuario de jQuery también se incluyen en ASP.NET MVC 4.

          > [!NOTE]
          > Puede obtener más información acerca de la biblioteca KnockOutJS en este vínculo: [ http://learn.knockoutjs.com/ ](http://learn.knockoutjs.com/).
      4. **Modernizr**: esta biblioteca se ejecuta automáticamente, lo que su sitio compatible con los exploradores más antiguos cuando se usa tecnologías de HTML5 y CSS3.

          > [!NOTE]
          > Puede obtener más información acerca de la biblioteca Modernizr en este vínculo: [ http://www.modernizr.com/ ](http://www.modernizr.com/).
   3. **SimpleMembership, incluido en la solución**

       SimpleMembership se ha diseñado como un reemplazo para el sistema anterior de proveedor de roles de ASP.NET y la pertenencia. Tiene muchas características nuevas que facilitan al desarrollador de páginas web seguras en una manera más flexible.

       La plantilla de Internet ya ha configurado algunas cosas para integrar SimpleMembership, por ejemplo, AccountController está preparado para usar OAuthWebSecurity (para el registro de la cuenta de OAuth, inicio de sesión, administración, etc.) y la seguridad de la Web.

       ![SimpleMembership incluidos en la solución](aspnet-mvc-4-fundamentals/_static/image42.png "SimpleMembership incluidos en la solución")

       *SimpleMembership incluidos en la solución*

       > [!NOTE]
       > Obtenga más información sobre [OAuthWebSecurity](https://msdn.microsoft.com/library/jj158393(v=vs.111).aspx) en MSDN.

> [!NOTE]
> Además, puede implementar esta aplicación a los siguientes sitios Web Windows Azure [Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido los aspectos básicos de ASP.NET MVC:

- Los elementos principales de una aplicación MVC y cómo interactúan
- Cómo crear una aplicación ASP.NET MVC
- Cómo agregar y configurar los controladores para controlar los parámetros se pasa a través de la dirección URL y la cadena de consulta
- Cómo agregar una página principal de diseño para configurar una plantilla para el contenido HTML común, una hoja de estilos para mejorar la apariencia y funcionamiento y una plantilla de vista para mostrar el contenido HTML
- Cómo usar el patrón de ViewModel para pasar propiedades a la plantilla de vista para mostrar información dinámica
- Cómo usar los parámetros pasados a los controladores en la plantilla de vista
- Cómo agregar vínculos a páginas dentro de la aplicación de ASP.NET MVC
- Cómo agregar y utilizar propiedades dinámicas en una vista
- Las mejoras en las plantillas de proyecto de ASP.NET MVC 4

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con SDK de Windows Azure</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](aspnet-mvc-4-fundamentals/_static/image43.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](aspnet-mvc-4-fundamentals/_static/image44.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](aspnet-mvc-4-fundamentals/_static/image45.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](aspnet-mvc-4-fundamentals/_static/image46.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](aspnet-mvc-4-fundamentals/_static/image47.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy

En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de administración de Windows Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Windows Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-windows-azure-portal"></a>Tarea 1: crear un nuevo sitio Web de Windows Azure Portal

1. Vaya a la [Portal de administración de Windows Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Windows Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](aspnet-mvc-4-fundamentals/_static/image48.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal de administración de Azure de Windows*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](aspnet-mvc-4-fundamentals/_static/image49.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Un sitio Web de Windows Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en el sitio Web Windows Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](aspnet-mvc-4-fundamentals/_static/image50.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![Exploración al sitio web nuevo](aspnet-mvc-4-fundamentals/_static/image51.png "exploración al sitio web nuevo")

    *Exploración al sitio web nuevo*

    ![Sitio Web que se ejecuta](aspnet-mvc-4-fundamentals/_static/image52.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](aspnet-mvc-4-fundamentals/_static/image53.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un sitio Web de Windows Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web para sitios Web de Windows Azure.

    ![Descargando el sitio web de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image54.png "descargando el sitio web de perfil de publicación")

    *Descargando el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web a un sitios Web de Windows Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](aspnet-mvc-4-fundamentals/_static/image55.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Windows Azure en **bases de datos Sql** | **servidores** | **del servidor Panel**. Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas. No cree la base de datos, como se crearán en una etapa posterior.

    ![Panel de base de datos SQL Server](aspnet-mvc-4-fundamentals/_static/image56.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](aspnet-mvc-4-fundamentals/_static/image57.png) botón.

    ![Agregar dirección IP del cliente](aspnet-mvc-4-fundamentals/_static/image58.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](aspnet-mvc-4-fundamentals/_static/image59.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](aspnet-mvc-4-fundamentals/_static/image60.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](aspnet-mvc-4-fundamentals/_static/image61.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](aspnet-mvc-4-fundamentals/_static/image62.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de implementación Web](aspnet-mvc-4-fundamentals/_static/image63.png "configuración de implementación Web")

    *Configuración de implementación Web*
5. Configure la conexión de base de datos como sigue:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](aspnet-mvc-4-fundamentals/_static/image64.png "configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.

    ![Creación de la base de datos](aspnet-mvc-4-fundamentals/_static/image65.png "creación de la cadena de la base de datos")

    *Creación de la base de datos*
7. La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](aspnet-mvc-4-fundamentals/_static/image66.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **Preview** página, haga clic en **publicar**.

    ![Publicar la aplicación web](aspnet-mvc-4-fundamentals/_static/image67.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](aspnet-mvc-4-fundamentals/_static/image68.png "publica la aplicación en Windows Azure")

    *Publicada aplicación en Windows Azure*

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](aspnet-mvc-4-fundamentals/_static/image69.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](aspnet-mvc-4-fundamentals/_static/image70.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](aspnet-mvc-4-fundamentals/_static/image71.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](aspnet-mvc-4-fundamentals/_static/image72.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](aspnet-mvc-4-fundamentals/_static/image73.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](aspnet-mvc-4-fundamentals/_static/image74.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
