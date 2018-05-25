---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratorio de prácticas: Compilar una aplicación de una página (SPA) con ASP.NET Web API y Angular.js | Documentos de Microsoft'
author: rick-anderson
description: En las aplicaciones web tradicionales, el cliente (explorador) inicia la comunicación con el servidor al que solicita una página. El servidor, a continuación, procesa la solicitud...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratorio de prácticas: Compilar una aplicación de una página (SPA) con ASP.NET Web API y Angular.js
====================
por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](http://aka.ms/webcamps-training-kit)

> En las aplicaciones web tradicionales, el cliente (explorador) inicia la comunicación con el servidor al que solicita una página. A continuación, el servidor procesa la solicitud y envía el código HTML de la página al cliente. En las interacciones subsiguientes con la página: por ejemplo, el usuario navega a un vínculo o envía un formulario con los datos: una nueva solicitud se envía al servidor y el flujo se inicia de nuevo: el servidor procesa la solicitud y envía una nueva página en el explorador en respuesta a la nueva solicitud de acción Ed por el cliente.
> 
> Aplicaciones de página (SPAs) toda la página se carga en el explorador después de la solicitud inicial, pero las interacciones subsiguientes tienen lugar a través de solicitudes de Ajax. Esto significa que el explorador tiene que actualizar solo la parte de la página que ha cambiado. no es necesario volver a cargar la página completa. El enfoque SPA reduce el tiempo que tarda la aplicación para responder a las acciones del usuario, lo que produce una experiencia más fluida.
> 
> La arquitectura de un SPA implica ciertos desafíos que no están presentes en las aplicaciones web tradicionales. Sin embargo, nuevas tecnologías, como ASP.NET Web API, marcos de JavaScript como AngularJS y nuevas características de estilo proporcionadas por CSS3 facilitan realmente diseñar y compilar SPAs.
> 
> En este laboratorio de mano, aprovechará de esas tecnologías para implementar el experto en juego de prueba matemática, un sitio Web de curiosidades basado en el concepto SPA. En primer lugar, implemente el nivel de servicio con ASP.NET Web API para exponer los puntos de conexión necesarias para recuperar las preguntas de prueba y almacenar las respuestas. A continuación, creará una interfaz de usuario enriquecida y la capacidad de respuesta con AngularJS y CSS3 efectos de transformación.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crea un servicio ASP.NET Web API para enviar y recibir datos JSON
- Crear una interfaz de usuario siga respondiendo con AngularJS
- Mejorar la experiencia de interfaz de usuario con las transformaciones de CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio de prácticas, es necesario lo siguiente:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para ejecutar los ejercicios en este laboratorio práctico, debe configurar primero el entorno.

1. Abra el Explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga doble clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que configure el entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo de Control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha activado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso de los fragmentos de código

En este documento de laboratorio, le indicará que insertar bloques de código. Para su comodidad, la mayor parte de este código se proporciona como fragmentos de código de Visual Studio, puede tener acceso desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.

> [!NOTE]
> Cada ejercicio viene acompañado por una solución de inicia ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de las otras. Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio se encuentran desde estas a partir de soluciones y no pueden funcionar hasta que haya completado el ejercicio. En el código fuente de un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos en el ejercicio correspondiente. Puede usar estas soluciones como guía si necesita ayuda adicional cuando se trabaja a través de este laboratorio práctico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Crear una API Web](#Exercise1)
2. [Crear una interfaz SPA](#Exercise2)

Tiempo estimado para completar esta práctica: **60 minutos**

> [!NOTE]
> Cuando se inicia Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos descritos en este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ejercicio 1: Crear una API Web

Una de las partes principales de un SPA es el nivel de servicio. Es responsable de procesar las llamadas de Ajax enviadas por la interfaz de usuario y la devolución de datos en respuesta a esa llamada. Los datos recuperados deben presentarse en un formato legible por máquina para analizarse y utilizado por el cliente.

El marco Web API forma parte de la pila de ASP.NET y está diseñado para que sea fácil de implementar servicios HTTP, por lo general, enviar y recibir datos con formato JSON o XML a través de la API de REST. En este ejercicio se creará el sitio Web para hospedar la aplicación de prueba de experto en y, a continuación, implementar el servicio back-end para exponer y conservar los datos de prueba mediante ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tarea 1: crear el proyecto inicial para el experto en prueba

En esta tarea se comenzará a crear un nuevo proyecto de MVC de ASP.NET con compatibilidad para ASP.NET Web API se basa en el **One ASP.NET** proyecto tipo que se incluye con Visual Studio. **Uno ASP.NET** unifica todas las tecnologías ASP.NET y ofrece la opción de mezclar y hacerlos coincidir según sea necesario. A continuación, agregará clases del modelo de Entity Framework y el initializator de base de datos para insertar las preguntas de prueba.

1. Abra **Visual Studio Express 2013 para Web** y seleccione **archivo | Nuevo proyecto...**  para iniciar una nueva solución.

    ![Crear un nuevo proyecto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "crear un nuevo proyecto")

    *Crear un nuevo proyecto*
2. En el **nuevo proyecto** cuadro de diálogo, seleccione **aplicación Web ASP.NET** en el **Visual C# | Web** ficha. Asegúrese de que **.NET Framework 4.5** está seleccionado, asígnele el nombre *GeekQuiz*, elija un **ubicación** y haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de aplicación Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "crear un nuevo proyecto de aplicación Web ASP.NET")

    *Crear un nuevo proyecto de aplicación Web ASP.NET*
3. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **MVC** plantilla y seleccione el **API Web** opción. Además, asegúrese de que el **autenticación** opción está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API de Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API de Web*
4. En **el Explorador de soluciones**, haga clic en el **modelos** carpeta de la **GeekQuiz** de proyecto y seleccione **agregar | Elemento existente...** .

    ![Agregar un elemento existente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "agregar un elemento existente")

    *Agregar un elemento existente*
5. En el **Agregar elemento existente** diálogo cuadro, vaya a la **activos/modelos/fuente** carpeta y seleccione todos los archivos. Haga clic en **Agregar**.

    ![Agregar los activos de modelo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "agregando los activos de modelo")

    *Agregar los activos de modelo*

    > [!NOTE]
    > Al agregar estos archivos, se agrega el modelo de datos, el contexto de base de datos de Entity Framework y el inicializador de base de datos para la aplicación de prueba de experto en.
    > 
    > **Entity Framework (EF)** es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional. Puede aprender más acerca de Entity Framework [aquí](../../../entity-framework.md).
    > 
    > La siguiente es una descripción de las clases que acaba de agregar:
    > 
    > - **TriviaOption:** representa una opción única asociada a una pregunta de prueba
    > - **TriviaQuestion:** representa una pregunta de prueba y expone las opciones asociadas a través de la **opciones** propiedad
    > - **TriviaAnswer:** representa la opción seleccionada por el usuario en respuesta a una pregunta de prueba
    > - **TriviaContext:** representa el contexto de base de datos de Entity Framework de la aplicación experto en prueba. Esta clase se deriva de **DContext** y expone **DbSet** propiedades que representan colecciones de las entidades que se ha descrito anteriormente.
    > - **TriviaDatabaseInitializer:** la implementación del inicializador de Entity Framework para la **TriviaContext** clase que hereda de **CreateDatabaseIfNotExists**. El comportamiento predeterminado de esta clase crear la base de datos solamente si no existe, insertar las entidades especificada en el **inicialización** método.
6. Abierto el **Global.asax.cs** archivo y agregue la siguiente instrucción using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Agregue el código siguiente al principio de la **aplicación\_iniciar** método para establecer el **TriviaDatabaseInitializer** como el inicializador de base de datos.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificar el **inicio** controlador para restringir el acceso a los usuarios autenticados. Para ello, abra el **HomeController.cs** archivo dentro de la **controladores** carpeta y agregue el **Authorize** atribuir a la **HomeController**definición de clase.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > El **Authorize** filtrar comprueba si el usuario está autenticado. Si el usuario no está autenticado, devuelve código de estado HTTP 401 (no autorizado) sin invocar la acción. Puede aplicar el filtro globalmente, en el nivel de controlador, o en el nivel de acciones individuales.
9. Ahora va a personalizar el diseño de las páginas web y la personalización de marca. Para ello, abra el  **\_Layout.cshtml** archivo dentro de la **vistas | Compartido** carpeta y actualizar el contenido de la **&lt;título&gt;** elemento reemplazando *mi aplicación ASP.NET* con *experto en prueba* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. En el mismo archivo, actualice la barra de navegación mediante la eliminación de la *sobre* y *póngase en contacto con* vínculos y cambiar el nombre de la *inicio* vincular a *reproducir*. Además, cambie el nombre de la *nombre de la aplicación* vincular a *cuestionario experto en*. El código HTML de la barra de navegación debe ser similar al código siguiente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Actualice el pie de página de la página de diseño reemplazando *mi aplicación ASP.NET* con *cuestionario experto en*. Para ello, reemplace el contenido de la **&lt;pie de página&gt;** elemento con el código que aparece resaltado.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tarea 2: creación de la API de Web TriviaController

En la tarea anterior, creó la estructura inicial de la aplicación web experto en prueba. Ahora creará un servicio de API Web simple que interactúa con el modelo de datos de prueba y expone las siguientes acciones:

- **GET/api/curiosidades**: recupera la siguiente pregunta de la lista de prueba para responder al usuario autenticado.
- **POST/api/curiosidades**: almacena la respuesta de la prueba especificada por el usuario autenticado.

Utilizará las herramientas de ASP.NET Scaffolding proporcionadas por Visual Studio para crear la línea de base para la clase de controlador de API Web.

1. Abra la **WebApiConfig.cs** archivo dentro de la **aplicación\_iniciar** carpeta. Este archivo define la configuración del servicio de API Web, como cómo se asignan las rutas a las acciones del controlador de API Web.
2. Agregue la siguiente instrucción using al principio del archivo.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Agregue el código resaltado siguiente a la **registrar** método para configurar globalmente el formateador para los datos JSON recuperados por los métodos de acción de la API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > El **CamelCasePropertyNamesContractResolver** convierte automáticamente los nombres de propiedades para *camel* caso, que es la convención general para los nombres de propiedad en JavaScript.
4. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **GeekQuiz** de proyecto y seleccione **agregar | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo elemento con scaffolding](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "crear un nuevo elemento con scaffolding")

    *Crear un nuevo elemento con scaffolding*
5. En el **agregar scaffolding** diálogo cuadro, asegúrese de que el **común** nodo está seleccionado en el panel izquierdo. A continuación, seleccione la **Web API 2 controlador - vacío** plantilla en el panel central y haga clic en **agregar**.

    ![Al seleccionar la plantilla vacía de controlador Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "seleccionando la plantilla vacía de controlador Web API 2")

    *Al seleccionar la plantilla vacía de controlador Web API 2*

    > [!NOTE]
    > **ASP.NET Scaffolding** es un marco de trabajo de generación de código para aplicaciones Web ASP.NET. Visual Studio 2013 incluye generadores de código previamente instalados para los proyectos de MVC y API Web. Debe usar scaffolding en el proyecto cuando desee agregar rápidamente código que interactúe con modelos de datos con el fin de reducir la cantidad de tiempo necesario para desarrollar las operaciones de datos estándar.
    > 
    > El proceso de scaffolding también garantiza que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si inicia con un proyecto ASP.NET vacío y, a continuación, usar el scaffolding para agregar un controlador de API Web, los paquetes de NuGet para la API Web necesarios y las referencias se agregan al proyecto automáticamente.
6. En el **Agregar controlador** cuadro de diálogo, escriba *TriviaController* en el **nombre de controlador** cuadro de texto y haga clic en **agregar**.

    ![Agregar el controlador curiosidades](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "agregar el controlador curiosidades")

    *Agregar el controlador curiosidades*
7. El **TriviaController.cs** , a continuación, se agrega a la **controladores** carpeta de la **GeekQuiz** proyecto, que contiene vacío **TriviaController** clase. Agregue las siguientes instrucciones using al principio del archivo.

    (Código de fragmento de código: *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Agregue el código siguiente al principio de la **TriviaController** clase para definir, inicializar y eliminar el **TriviaContext** instancia en el controlador.

    (Código de fragmento de código: *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > El **Dispose** método **TriviaController** , se invoca el **Dispose** método de la **TriviaContext** instancia, lo que garantiza que todos los se liberan los recursos utilizados por el objeto de contexto cuando las **TriviaContext** instancia se elimina o se recopilan de elementos no utilizados. Esto incluye cerrar todas las conexiones de base de datos abiertas por Entity Framework.
9. Agregue el siguiente método auxiliar al final de la **TriviaController** clase. Este método recupera la siguiente pregunta de prueba de la base de datos a los que pueden responder por el usuario especificado.

    (Código de fragmento de código: *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Agregue el siguiente **obtener** método de acción para el **TriviaController** clase. Llama a este método de acción el **NextQuestionAsync** método auxiliar definido en el paso anterior para recuperar la siguiente pregunta del usuario autenticado.

    (Código de fragmento de código: *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Agregue el siguiente método auxiliar al final de la **TriviaController** clase. Este método almacena la respuesta especificada en la base de datos y devuelve un valor booleano que indica si la respuesta es correcta o no.

    (Código de fragmento de código: *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Agregue el siguiente **Post** método de acción para el **TriviaController** clase. Este método de acción asocia la respuesta para el usuario autenticado y llama el **StoreAsync** método auxiliar. A continuación, envía una respuesta con el valor booleano devuelto por el método auxiliar.

    (Código de fragmento de código: *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifique el controlador de Web API para restringir el acceso a los usuarios autenticados mediante la adición de la **Authorize** atribuir a la **TriviaController** definición de clase.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecutar la solución

En esta tarea comprobará que el servicio de API Web creado en la tarea anterior funciona según lo previsto. Va a usar el Explorador de Internet **herramientas de desarrollo F12** para capturar el tráfico de red e inspeccionar la respuesta completa desde el servicio de API Web.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Presione **F5** para ejecutar la solución. El **sesión** debería aparecer la página en el explorador.

    > [!NOTE]
    > Cuando se inicia la aplicación, se desencadena la ruta MVC predeterminada, que de forma predeterminada se asigna a la **índice** acción de la **HomeController** clase. Puesto que **HomeController** está restringido a los usuarios autenticados (Recuerde que decora esa clase con la **Authorize** atributo en el ejercicio 1) y no hay ningún usuario autenticado aún, la aplicación redirige la solicitud original a la página de inicio de sesión.

    ![Ejecución de la solución](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "ejecutar la solución")

    *Ejecución de la solución*
2. Haga clic en **registrar** para crear un nuevo usuario.

    ![Registrar un nuevo usuario](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "registrar un nuevo usuario")

    *Registrar un nuevo usuario*
3. En el **registrar** página, escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página registrar](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "página de registro")

    *Página registrar*
4. La aplicación registra la nueva cuenta y el usuario está autenticado y redirige a la página principal.

    ![Se autentica el usuario](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "usuario autenticado")

    *Se autentica el usuario*
5. En el explorador, presione **F12** para abrir el **Developer Tools** panel. Presione **CTRL + 4** o haga clic en el **red** icono y, a continuación, haga clic en la flecha verde botón para empezar a capturar el tráfico de red.

    ![Iniciar la captura de red de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "captura de red iniciando Web API")

    *Iniciar la captura de red de la API de Web*
6. Anexar **api/curiosidades** a la dirección URL en la barra de direcciones del explorador. Ahora inspeccionará los detalles de la respuesta de la **obtener** métodos de acción de **TriviaController**.

    ![Recuperar los datos de pregunta siguientes a través de Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "recuperar los datos de pregunta siguientes a través de Web API")

    *Recuperar los datos de pregunta siguientes a través de Web API*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje el cuadro de diálogo abierto para poder ver el contenido de la respuesta a través de la ventana de herramientas de los desarrolladores.
7. Ahora inspeccionará el cuerpo de la respuesta. Para ello, haga clic en el **detalles** ficha y, a continuación, haga clic en **cuerpo de respuesta**. Puede comprobar que los datos descargados están un objeto con las propiedades **opciones** (que es una lista de **TriviaOption** objetos), **identificador** y **título** que corresponden a la **TriviaQuestion** clase.

    ![Ver el cuerpo de respuesta mediante una API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "ver el cuerpo de respuesta de la API de Web")

    *Cuerpo de respuesta mediante una API Web de visualización*
8. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ejercicio 2: Creación de la interfaz SPA

En este ejercicio primero creará la parte de front-end web de juego de prueba matemática experto en, centrándose en la interacción de página única aplicación que utiliza **AngularJS**. A continuación, mejorará la experiencia del usuario con CSS3 para realizar animaciones enriquecidas y proporcionan un efecto visual del contexto de cambio al realizar la transición de una pregunta a la siguiente.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tarea 1: creación de la interfaz SPA con AngularJS

En esta tarea usará **AngularJS** para implementar el cliente de la aplicación de prueba de experto en. **AngularJS** es un marco de JavaScript de código abierto que aumenta las aplicaciones basadas en explorador con *Model-View-Controller* capacidad (MVC), facilitar el desarrollo y pruebas.

Se iniciará mediante la instalación de AngularJS desde la consola de administrador de paquetes de Visual Studio. A continuación, creará el controlador para proporcionar el comportamiento de la aplicación de prueba de experto en la vista para representar las preguntas de prueba y respuestas con el motor de plantillas de AngularJS.

> [!NOTE]
> Para obtener más información acerca de AngularJS, consulte [ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/).


1. Abra **Visual Studio Express 2013 para Web** y abra el **GeekQuiz.sln** soluciones se encuentran en la **origen/Ex2-CreatingASPAInterface/Begin** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. Abra la **consola de administrador de paquetes** de **herramientas** | **Administrador de paquetes de biblioteca**. Escriba el siguiente comando para instalar el **AngularJS.Core** paquete NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. En **el Explorador de soluciones**, haga clic en el **Scripts** carpeta de la **GeekQuiz** de proyecto y seleccione **agregar | Nueva carpeta**. Nombre de la carpeta **aplicación** y presione **ENTRAR**.
4. Haga clic en el **aplicación** carpeta que ha creado y seleccione **agregar | Archivo JavaScript**.

    ![Crear un nuevo archivo de JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Crear un nuevo archivo de JavaScript*
5. En el **especificar nombre para el elemento** cuadro de diálogo, escriba *controlador de prueba* en el **nombre del elemento** cuadro de texto y haga clic en **Aceptar**.

    ![Nombre al nuevo archivo de JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nombre al nuevo archivo de JavaScript*
6. En el **prueba controller.js** , agregue el código siguiente para declarar e inicializar AngularJS **QuizCtrl** controlador.

    (Código de fragmento de código: *AngularQuizController AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La función constructora de la **QuizCtrl** controlador espera un parámetro inyectables denominado **$scope**. El estado inicial del ámbito se debe configurar en la función constructora adjuntando propiedades para la **$scope** objeto. Las propiedades contienen la **modelo de vista**y podrá obtener acceso a la plantilla cuando se registra el controlador.
    > 
    > El **QuizCtrl** controlador se define dentro de un módulo denominado **QuizApp**. Los módulos son unidades de trabajo que le permiten dividir la aplicación en componentes independientes. Las principales ventajas del uso de módulos es que el código sea más fácil de entender y facilita las pruebas unitarias, reutilización y mantenimiento.
7. Ahora agregará comportamiento para el ámbito para reaccionar a los eventos desencadenados a partir de la vista. Agregue el código siguiente al final de la **QuizCtrl** controlador para definir la **nextQuestion** funcionando en el **$scope** objeto.

    (Código de fragmento de código: *AngularQuizControllerNextQuestion AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Esta función recupera la siguiente pregunta de la **curiosidades** API Web creada en el ejercicio anterior y los datos de la pregunta que se adjunta el **$scope** objeto.
8. Inserte el siguiente código al final de la **QuizCtrl** controlador para definir la **sendAnswer** funcionando en el **$scope** objeto.

    (Código de fragmento de código: *AngularQuizControllerSendAnswer AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Esta función envía la respuesta seleccionada por el usuario para la **curiosidades** API Web y almacena el resultado, es decir, si la respuesta es correcta o no: en el **$scope** objeto.
    > 
    > El **nextQuestion** y **sendAnswer** funciones anteriores utilizan AngularJS **$http** objeto abstraer la comunicación con la API Web mediante el objeto XMLHttpRequest Objeto de JavaScript desde el explorador. AngularJS es compatible con otro servicio que ofrece un mayor nivel de abstracción para realizar operaciones CRUD en un recurso a través de API RESTful. AngularJS **$resource** objeto tiene métodos de acción que proporcionan los comportamientos de alto nivel sin necesidad de interactuar con el **$http** objeto. Considere el uso de la **$resource** objeto en escenarios que requiere el modelo CRUD (fore información, consulte el [$resource documentación](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. El siguiente paso es crear la plantilla de AngularJS que define la vista para la prueba. Para ello, abra el **Index.cshtml** archivo dentro de la **vistas | Inicio** carpeta y reemplace el contenido con el código siguiente.

    (Código de fragmento de código: *GeekQuizView AspNetWebApiSpa - Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > La plantilla de AngularJS es una especificación declarativa que utiliza información del modelo y el controlador para transformar marcado estático en la vista dinámica que el usuario ve en el explorador. Los siguientes son ejemplos de AngularJS elementos y atributos del elemento que se pueden usar en una plantilla:
    > 
    > - El **ng aplicación** directiva indica a AngularJS el elemento DOM que representa el elemento raíz de la aplicación.
    > - El **ng controlador** directiva asocia un controlador en el DOM en el punto donde se declara la directiva.
    > - La notación de llave **{{}}** denota enlaces a las propiedades del ámbito definidos en el controlador.
    > - El **ng clic** directiva se utiliza para invocar las funciones definidas en el ámbito en respuesta a los clics del usuario.
10. Abra la **Site.css** archivo dentro de la **contenido** carpeta y agregue los siguientes estilos de resaltado al final del archivo para proporcionar una apariencia y funcionamiento de la vista de prueba.

    (Código de fragmento de código: *GeekQuizStyles AspNetWebApiSpa - Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecutar la solución

En esta tarea se ejecutará la solución con el nuevo usuario de interfaz que se han creado con AngularJS para responder algunas de las preguntas de prueba.

1. Presione **F5** para ejecutar la solución.
2. Registrar una nueva cuenta de usuario. Para ello, siga los pasos de registro descritos en el ejercicio 1, tarea 3.

    > [!NOTE]
    > Si utilizas la solución desde el ejercicio anterior, puede iniciar sesión con la cuenta de usuario que creó antes.
3. El **inicio** debería aparecer página, que muestra la primera pregunta de la prueba. Responda a la pregunta, haga clic en una de las opciones. Esto desencadenará la **sendAnswer** función definida anteriormente, que envía la opción seleccionada a la **curiosidades** API Web.

    ![Responder a una pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "responder a una pregunta")

    *Responder a una pregunta*
4. Después de hacer clic en uno de los botones, debería aparecer la respuesta. Haga clic en **siguiente pregunta** para mostrar la siguiente pregunta. Esto desencadenará la **nextQuestion** función definida en el controlador.

    ![Solicitar la siguiente pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "que solicita la siguiente pregunta")

    *Solicitar la siguiente pregunta*
5. Debe aparecer la siguiente pregunta. Continuar a responder las preguntas tantas veces como desee. Después de completar todas las preguntas que debe devolver a la primera pregunta.

    ![Otra cuestión](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "otra pregunta")

    *Siguiente pregunta*
6. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tarea 3: crear una animación voltear mediante CSS3

En esta tarea usará CSS3 propiedades para realizar animaciones enriquecidas agregando un efecto flip cuando se responde a una pregunta y cuando se recupera la siguiente pregunta.

1. En **el Explorador de soluciones**, haga clic en el **contenido** carpeta de la **GeekQuiz** de proyecto y seleccione **agregar | Elemento existente...** .

    ![Agregar un elemento existente a la carpeta de contenido](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "agregar un elemento existente a la carpeta de contenido")

    *Agregar un elemento existente a la carpeta de contenido*
2. En el **Agregar elemento existente** diálogo cuadro, vaya a la **origen/activos** carpeta y seleccione **Flip.css**. Haga clic en **Agregar**.

    ![Agregar el archivo Flip.css de activos](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "agregar el archivo Flip.css de activos")

    *Agregar el archivo Flip.css de activos*
3. Abra la **Flip.css** archivo que se acaba de agregar e inspeccionar su contenido.
4. Busque la **voltear transformación** comentario. Los estilos por debajo del comentario usen la CSS **perspectiva** y **rotateY** transformaciones para generar un &quot;tarjeta flip&quot; efecto.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Busque la **ocultar parte posterior del panel durante flip** comentario. El estilo de comentario oculta el lado posterior de las caras cuando que se encuentran visibles desde el Visor estableciendo la **visibilidad del lado posterior** propiedad CSS en *oculto*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Abra el **BundleConfig.cs** archivo dentro de la **aplicación\_iniciar** carpeta y agregar la referencia al **Flip.css** en el archivo la **&quot;~/Content/css&quot;** agrupación de estilo

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Presione **F5** para ejecutar la solución e inicie sesión con sus credenciales.
8. Responder a una pregunta, haga clic en una de las opciones. Tenga en cuenta el efecto flip al realizar la transición entre las vistas.

    ![Responder a una pregunta con el efecto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "responder a una pregunta con el efecto flip")

    *Responder a una pregunta con el efecto flip*
9. Haga clic en **siguiente pregunta** para recuperar la siguiente pregunta. El efecto flip debería aparecer de nuevo.

    ![Recuperar la siguiente pregunta con el efecto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "recuperar la siguiente pregunta con el efecto flip")

    *Recuperar la siguiente pregunta con el efecto flip*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Crear un controlador de ASP.NET Web API mediante Scaffolding de ASP.NET
- Implementar una acción de Web API Get para recuperar la siguiente pregunta de prueba
- Implementar una acción posterior de la API de Web para almacenar las respuestas de la prueba
- Instalar AngularJS desde la consola de administrador de paquetes de Visual Studio
- Implemente AngularJS plantillas y controladores
- Para llevar a cabo efectos de animación de transiciones CSS3 de uso
