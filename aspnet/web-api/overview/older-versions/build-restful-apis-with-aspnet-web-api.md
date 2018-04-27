---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilar API RESTful con ASP.NET Web API | Documentos de Microsoft
author: rick-anderson
description: En los últimos años, se ha convertido en claro que HTTP no es solo para servir páginas HTML. También es una herramienta eficaz para la creación de las API Web, con una o pocas...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ded549109ca6e7ad806f1c3f53387766527e5a94
ms.sourcegitcommit: 01db73f2f7ac22b11ea48a947131d6176b0fe9ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/26/2018
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Compilar API RESTful con ASP.NET Web API
====================
Por [Web colonias equipo](https://twitter.com/webcamps)

> En los últimos años, se ha convertido en claro que HTTP no es solo para servir páginas HTML. También es una herramienta eficaz para la creación de las API Web, con unos cuantos verbos (GET, POST etc.) junto con algunos conceptos simples como *URI* y *encabezados*. ASP.NET Web API es un conjunto de componentes que simplifican la programación de HTTP. Dado que se basa en el tiempo de ejecución de ASP.NET MVC, Web API controla automáticamente los detalles de bajo nivel de transporte de HTTP. Al mismo tiempo, API Web de forma natural expone el modelo de programación HTTP. De hecho, es un objetivo de API Web *no* condensan y eliminan la realidad de HTTP. Como resultado, la API Web es flexible y fácil de extender. En este laboratorio práctico, utilizará la API Web para crear una API de REST simple para una aplicación de administrador de contactos. También generará un cliente para utilizar la API. El estilo arquitectónico REST ha demostrado para ser una manera eficaz de aprovechar HTTP - aunque ciertamente no es el enfoque solo es válido para HTTP. El Administrador de contactos expondrá la RESTful para enumerar, agregar y quitar contactos, entre otros. Esta práctica requiere conocimientos básicos de HTTP, REST y se supone que tiene conocimientos prácticos básicos de HTML, JavaScript y jQuery.
> 
> > [!NOTE]
> > El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ https://asp.net/web-api ](https://asp.net/web-api). Este sitio continuará proporcionando información de última hora, ejemplos y noticias relacionadas con la API Web, por lo que protegerlo con frecuencia si le gustaría profundizar en el arte de crear API personalizadas de Web disponible para prácticamente cualquier marco de desarrollo o dispositivo.
> > 
> > ASP.NET Web API, similar a ASP.NET MVC 4, tiene gran flexibilidad en cuanto a separar la capa de servicio desde los controladores de lo que le permite usar varios de los marcos de inyección de dependencia disponibles bastante fácil. Hay un buen ejemplo en MSDN que muestra cómo utilizar Ninject para la inyección de dependencia en un proyecto de ASP.NET Web API que puede descargar desde [aquí](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Implementar una API Web RESTful
- Llamar a la API desde un cliente HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio de prácticas, es necesario lo siguiente:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye el siguiente ejercicio:

1. [Ejercicio 1: Crear una API de Web de solo lectura](#Exercise1)
2. [Ejercicio 2: Crear una API Web de lectura/escritura](#Exercise2)
3. [Ejercicio 3: Utilizar la API Web desde un cliente HTML](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ejercicio 1: Crear una API de Web de solo lectura

En este ejercicio, implementará los métodos GET de solo lectura para el Administrador de contacto.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tarea 1: crear el proyecto de API

En esta tarea, utilizará las nuevas plantillas de proyecto web ASP.NET para crear una aplicación web de API Web.

1. Ejecutar **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.
2. Desde el **archivo** menú, seleccione **nuevo proyecto**. Seleccione el **Visual C# | Web** tipo de la vista de árbol del tipo de proyecto del proyecto y haga clic en el **aplicación Web de ASP.NET MVC 4** tipo de proyecto. Establezca el proyecto **nombre** a *ContactManager* y la **nombre de la solución** a *comenzar*, a continuación, haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web")

    *Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web*
3. En el cuadro de diálogo de tipo de proyecto de ASP.NET MVC 4, seleccione la **API Web** tipo de proyecto. Haga clic en **Aceptar**.

    ![Especifica el tipo de proyecto de Web API](build-restful-apis-with-aspnet-web-api/_static/image2.png "especifica el tipo de proyecto de Web API")

    *Especifica el tipo de proyecto de Web API*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tarea 2: crear los controladores de API póngase en contacto con el administrador

En esta tarea, creará las clases de controlador en el que residirá métodos de la API.

1. Elimine el archivo denominado **ValuesController.cs** en **controladores** carpeta del proyecto.
2. Haga clic en el **controladores** carpeta en el proyecto y seleccione **agregar | Controlador** en el menú contextual.

    ![Agregar un nuevo controlador para el proyecto](build-restful-apis-with-aspnet-web-api/_static/image3.png "agregando un nuevo controlador para el proyecto")

    *Agregar un nuevo controlador para el proyecto*
3. En el **Agregar controlador** cuadro de diálogo que aparece, seleccione **controlador de API en blanco** en el menú de la plantilla. Nombre de la clase de controlador **ContactController**. A continuación, haga clic en **agregar.**

    ![Mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web")

    *Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web*
4. Agregue el código siguiente a la **ContactController**.

    (Código de fragmento de código: *Web API laboratorio - Ex01 - obtener el método API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Presione **F5** para depurar la aplicación. Debe aparecer la página de inicio predeterminada para un proyecto de API Web.

    ![La página principal predeterminada de una aplicación de ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la página principal predeterminada de una aplicación de ASP.NET Web API")

    *La página principal predeterminada de una aplicación de ASP.NET Web API*
6. En la ventana de Internet Explorer, presione el **F12** clave para abrir el **Developer Tools** ventana. Haga clic en el **red** ficha y, a continuación, haga clic en el **Iniciar captura** botón para empezar a capturar el tráfico de red en la ventana.

    ![Abrir la pestaña red e iniciar captura de red](build-restful-apis-with-aspnet-web-api/_static/image6.png "abrir la pestaña red e iniciar captura de red")

    *Abrir la pestaña red e iniciar la captura de red*
7. Anexe la dirección URL en la barra de direcciones del explorador con **/api/contacto** y presione ENTRAR. Los detalles de transmisión se mostrarán en la ventana de captura de red. Tenga en cuenta que el tipo MIME de la respuesta es **application/json**. Esto muestra cómo el formato de salida predeterminado es JSON.

    ![Ver el resultado de la solicitud de API Web en la vista de red](build-restful-apis-with-aspnet-web-api/_static/image7.png "viendo la salida de la solicitud de API Web en la vista de red")

    *Ver el resultado de la solicitud de API Web en la vista de red*

    > [!NOTE]
    > Comportamiento predeterminado del Internet Explorer 10 en este momento será preguntar si el usuario le gustaría guardar o abrir la secuencia resultante de la llamada de API Web. El resultado será un archivo de texto que contiene el resultado JSON de la llamada de URL de la API de Web. No cancele el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de los desarrolladores.
8. Haga clic en el **vaya a la vista detallada** botón para ver más detalles acerca de la respuesta de esta llamada API.

    ![Cambie a la vista detallada](build-restful-apis-with-aspnet-web-api/_static/image8.png "cambie a la vista de detalles")

    *Cambie a la vista detallada*
9. Haga clic en el **cuerpo de respuesta** pestaña para ver el texto real de la respuesta JSON.

    ![Ver el JSON de salida texto en el monitor de red](build-restful-apis-with-aspnet-web-api/_static/image9.png "ver el JSON de salida texto en el monitor de red")

    *Ver el texto de salida JSON en el monitor de red*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tarea 3: crear los modelos de contacto y aumentar el controlador de contacto

En esta tarea, creará las clases de controlador en el que residirá métodos de la API.

1. Haga clic en el **modelos** carpeta y seleccione **agregar | Clase...**  en el menú contextual.

    ![Agregar un modelo nuevo a la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image10.png "agregar un modelo nuevo a la aplicación web")

    *Agregar un modelo nuevo a la aplicación web*
2. En el **Agregar nuevo elemento** cuadro de diálogo, denomine el nuevo archivo **Contact.cs** y haga clic en **agregar.**

    ![Crear el nuevo archivo de clase de contacto](build-restful-apis-with-aspnet-web-api/_static/image11.png "crear el nuevo archivo de clase de contacto")

    *Crear el nuevo archivo de clase de contacto*
3. Agregue el código resaltado siguiente a la **póngase en contacto con** clase.

    (Código de fragmento de código: *API laboratorio - Ex01 - contacto clase Web*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
~~~
4. En el **ContactController** de clases, seleccione la palabra **cadena** en la definición de método de la **obtener** (método) y escriba la palabra *póngase en contacto con*. Una vez que la palabra esté escrita en, aparecerá un indicador al principio de la palabra **póngase en contacto con**. Cualquiera mantenga presionada la tecla el **Ctrl** clave y presione la tecla de punto (.) o haga clic en el icono con el mouse para abrir el cuadro de diálogo de asistencia en el editor de código, para rellenar automáticamente el **con** la directiva para los modelos espacio de nombres.

    ![Utilización de la asistencia de Intellisense para las declaraciones de espacio de nombres](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Utilización de la asistencia de Intellisense para las declaraciones de espacio de nombres*
5. Modificar el código para el **obtener** método para que devuelva una matriz de instancias de modelo de contacto.

    (Código de fragmento de código: *devolver una lista de contactos de laboratorio de API Web - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Presione **F5** para depurar la aplicación web en el explorador. Para ver los cambios realizados en la salida de respuesta de la API, realice los pasos siguientes.

   1. Una vez que el explorador se abre, presione **F12** si las herramientas de desarrollo no están abiertas.
   2. Haga clic en el **red** ficha.
   3. Presione el **Iniciar captura** botón.
   4. Agregue el sufijo de URL **/api/póngase en contacto con** a la dirección URL en la barra de direcciones y presione la **ENTRAR** clave.
   5. Presione el **vaya a la vista detallada** botón.
   6. Seleccione el **cuerpo de respuesta** ficha. Debería ver una cadena JSON que representa la forma serializada de una matriz de instancias de contacto.

      ![JSON serializa la salida de una llamada al método de API Web complejo](build-restful-apis-with-aspnet-web-api/_static/image13.png "JSON serializa la salida de una llamada al método de API Web compleja")

      *Salida JSON serializado de una llamada al método de API Web compleja*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tarea 4: la funcionalidad de extracción en una capa de servicio

Esta tarea mostrará cómo extraer la funcionalidad en una capa de servicio para que sea fácil para los desarrolladores separar su funcionalidad de servicio de la capa de controlador, lo que permite la reutilización de los servicios que realmente realizan el trabajo.

1. Crear una nueva carpeta en la raíz de la solución y asígnele el nombre **Services**. Para ello, haga clic en **ContactManager** proyecto, seleccione **agregar** | **nueva carpeta**, asígnele el nombre *Services*.

    ![Crear carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image14.png "carpeta crear servicios")

    *Creando carpeta de servicios*
2. Haga clic en el **servicios** carpeta y seleccione **agregar | Clase...**  en el menú contextual.

    ![Agregar una nueva clase a la carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image15.png "agregar una nueva clase a la carpeta de servicios")

    *Agregar una nueva clase a la carpeta de servicios*
3. Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, la nueva clase el nombre **ContactRepository** y haga clic en **agregar**.

    ![Crear un archivo de clase para contener el código para el nivel de servicio de repositorio póngase en contacto con](build-restful-apis-with-aspnet-web-api/_static/image16.png "crear un archivo de clase para contener el código para el nivel de servicio de repositorio de contacto")

    *Crear un archivo de clase para contener el código para el nivel de servicio de repositorio de contacto*
4. Agregue un mediante la directiva a la **ContactRepository.cs** archivo para incluir el espacio de nombres de modelos.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
~~~
5. Agregue el código resaltado siguiente a la **ContactRepository.cs** archivo para implementar el método GetAllContacts.

    (Código de fragmento de código: *Web repositorio de contactos de laboratorio de API - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Abra la **ContactController.cs** archivo si no está ya abierto.
7. Agregue lo siguiente con la instrucción a la sección de declaración de espacio de nombres del archivo.


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
~~~
8. Agregue el código resaltado siguiente a la **ContactController.cs** clase para agregar un campo privado para representar la instancia del repositorio, para que utilice el resto de la clase pueden hacer los miembros de la implementación del servicio.

    (Código de fragmento de código: *API laboratorio - Ex01 - contacto controlador Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Cambiar el **obtener** método por lo que ofrece el uso de los servicios de repositorio de contactos.

    (Código de fragmento de código: *devolver una lista de contactos a través del repositorio de laboratorio de API Web - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Coloque un punto de interrupción en la **ContactController**del **obtener** definición de método.

   ![Agregar puntos de interrupción en el controlador de contacto](build-restful-apis-with-aspnet-web-api/_static/image17.png "agregar puntos de interrupción en el controlador de contacto")

   *Agregar puntos de interrupción en el controlador de contacto*
11. Presione **F5** para ejecutar la aplicación.
12. Cuando el explorador se abre, presione **F12** para abrir las herramientas de desarrollo.
13. Haga clic en el **red** ficha.
14. Haga clic en el **Iniciar captura** botón.
15. Anexe la dirección URL en la barra de direcciones con el sufijo **/api/póngase en contacto con** y presione **ENTRAR** para cargar el controlador de API.
16. Visual Studio 2012 se debería interrumpir una vez **obtener** método empieza a ejecutarse.

   ![Importantes dentro del método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "importantes dentro del método Get")

   *Separación dentro del método Get*
17. Presione **F5** para continuar.
18. Vuelva a Internet Explorer si aún no está en foco. Tenga en cuenta la ventana de captura de red.

    ![Ver en el Explorador de Internet que muestra los resultados de la llamada de API Web de red](build-restful-apis-with-aspnet-web-api/_static/image19.png "ver en el Explorador de Internet que muestra los resultados de la llamada de API Web de red")

    *Vista de red en Internet Explorer que muestra los resultados de la llamada de API Web*
19. Haga clic en el **vaya a la vista detallada** botón.
20. Haga clic en el **cuerpo de respuesta** ficha. Tenga en cuenta la salida JSON de la llamada de API y cómo representan los dos contactos recuperados por la capa de servicio.

    ![Ver la salida JSON a través de la API Web en la ventana de herramientas de desarrollador](build-restful-apis-with-aspnet-web-api/_static/image20.png "ver la salida JSON a través de la API Web en la ventana de herramientas de desarrollador")

    *Ver la salida JSON a través de la API Web en la ventana de herramientas de desarrollador*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ejercicio 2: Crear una API Web de lectura/escritura

En este ejercicio, implementará POST y PUT métodos para el Administrador de contacto habilitar con características de edición de datos.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tarea 1: abrir el proyecto de API Web

En esta tarea, preparará mejorar el proyecto de API Web creado en el ejercicio 1 para que pueda aceptar proporcionados por el usuario.

1. Ejecutar **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.
2. Abra la **comenzar** solución ubicado en **origen/Ex02-ReadWriteWebAPI/Begin/** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra la **Services/ContactRepository.cs** archivo.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tarea 2: agregar características de persistencia de los datos a la implementación del repositorio de contactos

En esta tarea, se aumentan la clase ContactRepository del proyecto de API Web que creó en el ejercicio 1 para que pueda conservar y Aceptar proporcionados por el usuario y nuevas instancias de contacto.

1. Agregue la siguiente constante para el **ContactRepository** clase para representar el nombre de la memoria caché elemento clave nombre del servidor web más adelante en este ejercicio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Agregue un constructor a la **ContactRepository** que contiene el código siguiente.

    (Código de fragmento de código: *Web Constructor de repositorio de contactos de laboratorio de API - Ex02 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modificar el código para el **GetAllContacts** método tal como se muestra a continuación.

    (Código de fragmento de código: *Web API laboratorio - Ex02 - obtener todos los contactos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > Este ejemplo es para fines ilustrativos y utilizará la caché del servidor web como un medio de almacenamiento, para que los valores se estén disponibles para varios clientes simultáneamente, en lugar de usar un mecanismo de almacenamiento de la sesión o una duración de almacenamiento de la solicitud. Se podría usar Entity Framework, el almacenamiento XML o cualquier otro tipo en lugar de la caché del servidor web.
4. Implemente un nuevo método denominado **SaveContact** a la **ContactRepository** clase para realizar el trabajo del proceso de guardar un contacto. El **SaveContact** método debe tomar un único **póngase en contacto con** valoran de parámetro y devuelva un valor booleano que indica éxito o error.

    (Código de fragmento de código: *Web API laboratorio - Ex02 - implementar el método SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ejercicio 3: Utilizar la API Web desde un cliente HTML

En este ejercicio, creará un cliente HTML para llamar a la API Web. Este cliente facilitará el intercambio de datos con la API de Web con JavaScript y mostrará los resultados en un explorador web mediante código HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tarea 1: modificar la vista de índice para proporcionar una interfaz gráfica de usuario para mostrar contactos

En esta tarea, modificará la vista de índice del valor predeterminado de la aplicación web para admitir el requisito de presentación de la lista de contactos existentes en un explorador HTML.

1. Abra **Visual Studio 2012 Express para Web** si no está ya abierto.
2. Abra la **comenzar** solución ubicado en **origen/Ex03-ConsumingWebAPI/Begin/** carpeta. En caso contrario, puede seguir usando el **final** solución obtenido siguiendo el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra la **Index.cshtml** archivo ubicado en **vistas/inicio** carpeta.
4. Reemplace el código HTML dentro del elemento div con el identificador **cuerpo** para que tenga un aspecto similar al código siguiente.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
~~~
5. Agregue el siguiente código de Javascript en la parte inferior del archivo para realizar la solicitud HTTP para la API Web.


~~~
[!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
~~~
6. Abra la **ContactController.cs** archivo si no está ya abierto.
7. Coloque un punto de interrupción en la **obtener** método de la **ContactController** clase.

    ![Colocar un punto de interrupción en el método Get del controlador API](build-restful-apis-with-aspnet-web-api/_static/image21.png "colocar un punto de interrupción en el método Get del controlador de API")

    *Colocar un punto de interrupción en el método Get del controlador de API*
8. Presione **F5** para ejecutar el proyecto. El explorador cargará el documento HTML.

    > [!NOTE]
    > Asegúrese de que va a examinar la dirección URL raíz de la aplicación.
9. Una vez que la página se carga y ejecuta el código JavaScript, se tendrán en cuenta el punto de interrupción y se detendrá la ejecución del código en el controlador.

    ![Depurar en las llamadas a API Web con VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "depurar en las llamadas a API Web con VS Express para Web")

    *Depurar en la llamada de API Web con Visual Studio 2012 Express para Web*
10. Quitar el punto de interrupción y presione **F5** o la barra de herramientas depuración **continuar** botón para continuar cargando la vista en el explorador. Una vez que finaliza la llamada de API Web debería ver los contactos devueltos por la API Web de llamadas mostrada como elementos de lista en el explorador.

    ![Resultados de la llamada de API que se muestra en el explorador como elementos de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "resultados de la llamada de API que se muestra en el explorador como elementos de lista")

    *Resultados de la llamada de API que se muestra en el explorador como elementos de lista*
11. Detenga la depuración.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tarea 2: modificar la vista de índice para proporcionar una interfaz gráfica de usuario para la creación de contactos

En esta tarea, continuará modificar la vista de índice de la aplicación de MVC. Un formulario se agregará a la página HTML que capturará proporcionados por el usuario y enviarlo a la API Web para crear un nuevo contacto y se creará un nuevo método de controlador de Web API para recopilar la fecha de la interfaz gráfica de usuario.

1. Abra la **ContactController.cs** archivo.
2. Agregue un nuevo método a la clase de controlador denominada **Post** tal como se muestra en el código siguiente.

    (Código de fragmento de código: *API laboratorio - Ex03 - Post método Web*)


~~~
[!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
~~~
3. Abra la **Index.cshtml** un archivo en Visual Studio si no está ya abierto.
4. Agregue el código HTML siguiente al archivo justo después de la lista sin ordenar que agregó en la tarea anterior.


~~~
[!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
~~~
5. En el elemento de secuencia de comandos en la parte inferior del documento, agregue el código resaltado siguiente para controlar los eventos de clic de botón, que registra los datos en la API Web mediante una llamada HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. En **ContactController.cs**, coloque un punto de interrupción en la **Post** método.
7. Presione **F5** para ejecutar la aplicación en el explorador.
8. Una vez que se carga la página en el explorador, escriba un nuevo nombre de contacto y el Id. y haga clic en el **guardar** botón.

    ![Cargar el documento de cliente HTML en el explorador](build-restful-apis-with-aspnet-web-api/_static/image24.png "cargar el documento de cliente HTML en el explorador")

    *Carga el documento HTML de cliente en el explorador*
9. Cuando la ventana del depurador se interrumpe la **Post** método, echar un vistazo a las propiedades de la **póngase en contacto con** parámetro. Los valores deben coincidir con los datos especificados en el formulario.

    ![El objeto de contacto que se envían a la API Web desde el cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "objeto póngase en contacto con el que se envían a la API Web desde el cliente")

    *El objeto de contacto que se envían a la API Web desde el cliente*
10. Paso a través del método en el depurador hasta el **respuesta** variable se ha creado. Tras la inspección en la **locales** ventana en el depurador, podrá ver que todas las propiedades se han establecido.

   ![La respuesta después de creación en el depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "la respuesta después de creación en el depurador")

   *La respuesta después de creación en el depurador*
11. Si presiona **F5** o haga clic en **continuar** en el depurador, se completa la solicitud. Una vez que cambie al explorador, el nuevo contacto se agregó a la lista de contactos almacenados por la **ContactRepository** implementación.

   ![El explorador refleja la creación correcta de la nueva instancia de contacto](build-restful-apis-with-aspnet-web-api/_static/image27.png "el explorador refleja la creación correcta de la nueva instancia de contacto")

   *El explorador refleja la creación correcta de la nueva instancia de contacto*

> [!NOTE]
> Además, puede implementar esta aplicación a Azure siguiente [Apéndice C: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Este laboratorio presenta para el nuevo marco de ASP.NET Web API y la implementación de REST API Web mediante el marco de trabajo. Desde aquí, puede crear un nuevo repositorio que facilita la persistencia de los datos con cualquier número de mecanismos y conecte ese servicio en lugar del simple proporcionada como un ejemplo en este laboratorio. Web API es compatible con un número de características adicionales, como habilitar la comunicación de los clientes de distintos de HTML escritos en cualquier lenguaje que admita HTTP y JSON o XML. La capacidad para hospedar una API Web fuera de una aplicación web típica también es posible, así como es la capacidad de crear sus propios formatos de serialización.

El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Este sitio continuará proporcionando información de última hora, ejemplos y noticias relacionadas con la API Web, por lo que protegerlo con frecuencia si le gustaría profundizar en el arte de crear API personalizadas de Web disponible para prácticamente cualquier marco de desarrollo o dispositivo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Para agregar un fragmento de código mediante el teclado (solo C#)

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

    ![Comience a escribir el nombre del fragmento](build-restful-apis-with-aspnet-web-api/_static/image29.png "comience a escribir el nombre del fragmento de código")

    *Comience a escribir el nombre del fragmento de código*

    ![Presione la tecla Tab para seleccionar el fragmento de código resaltada](build-restful-apis-with-aspnet-web-api/_static/image30.png "presione Tab para seleccionar el fragmento de código resaltada")

    *Presione la tecla Tab para seleccionar el fragmento de código resaltada*

    ![Vuelva a presionar Tab y el fragmento de código se expandirán](build-restful-apis-with-aspnet-web-api/_static/image31.png "vuelva a presionar Tab y el fragmento de código se expandirán")

    *Vuelva a presionar Tab y el fragmento de código se expandirán*

<a id="CodeSnippetUsingMouse"></a>

<a id="To_add_a_code_snippet_using_the_mouse_C_Visual_Basic_and_XML"></a>
### <a name="to-add-a-code-snippet-using-the-mouse-c-visual-basic-and-xml"></a>Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)

1. Haga clic en donde desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
3. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

    ![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](build-restful-apis-with-aspnet-web-api/_static/image32.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

    *Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

    ![Seleccione el fragmento de código relevante de la lista haciendo clic en él](build-restful-apis-with-aspnet-web-api/_static/image33.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

    *Seleccione el fragmento de código relevante de la lista haciendo clic en él*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con Azure SDK</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *Express de VS para icono Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice C: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: crear un nuevo sitio Web del Portal de Azure

1. Vaya a la [Portal de administración de Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](build-restful-apis-with-aspnet-web-api/_static/image42.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](build-restful-apis-with-aspnet-web-api/_static/image43.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](build-restful-apis-with-aspnet-web-api/_static/image44.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web en Azure.

    ![Descargar el sitio web de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image45.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image46.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Azure en **bases de datos Sql** | **servidores** | **panel del servidor**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](build-restful-apis-with-aspnet-web-api/_static/image47.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) botón.

    ![Agregar dirección IP del cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmar cambios*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](build-restful-apis-with-aspnet-web-api/_static/image51.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image52.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](build-restful-apis-with-aspnet-web-api/_static/image53.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configurar la cadena de conexión de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "configurar la cadena de conexión de destino")

     *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](build-restful-apis-with-aspnet-web-api/_static/image56.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "aplicación se publicó en Windows Azure")

    *Aplicación que se publica en Azure*
