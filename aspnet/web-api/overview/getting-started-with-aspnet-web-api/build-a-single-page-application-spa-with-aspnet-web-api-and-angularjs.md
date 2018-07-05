---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'Laboratorio práctico: Crear una aplicación de página única (SPA) con ASP.NET Web API y Angular.js | Microsoft Docs'
author: rick-anderson
description: En las aplicaciones web tradicionales, el cliente (explorador) inicia la comunicación con el servidor mediante la solicitud de una página. El servidor, a continuación, procesa la solicitud...
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 174084d6923cf1fa445485b7c0dc639a240720a5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37370868"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>Laboratorio práctico: Crear una aplicación de página única (SPA) con ASP.NET Web API y Angular.js
====================
por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](http://aka.ms/webcamps-training-kit)

> En las aplicaciones web tradicionales, el cliente (explorador) inicia la comunicación con el servidor mediante la solicitud de una página. A continuación, el servidor procesa la solicitud y envía el HTML de la página al cliente. En las interacciones posteriores con la página, por ejemplo, el usuario navega a un vínculo o envía un formulario con datos, se envía una nueva solicitud al servidor y el flujo se inicia de nuevo: el servidor procesa la solicitud y envía una nueva página al explorador en respuesta a la nueva solicitud de acción Ed por el cliente.
> 
> En aplicaciones de página única (SPA) toda la página se carga en el explorador después de la solicitud inicial, pero las interacciones subsiguientes tendrán lugar a través de las solicitudes Ajax. Esto significa que el explorador tiene que actualizar solo la parte de la página que ha cambiado; No hay ninguna necesidad de volver a cargar la página completa. El enfoque SPA reduce el tiempo que tarda la aplicación para responder a las acciones del usuario, lo que resulta en una experiencia más fluida.
> 
> La arquitectura de una SPA implica ciertos desafíos que no están presentes en las aplicaciones web tradicionales. Sin embargo, apareciendo tecnologías como ASP.NET Web API, los marcos de JavaScript de AngularJS y nuevas funciones de estilo de CSS3 facilitan realmente diseñar y compilar aplicaciones spa.
> 
> En este laboratorio prácticos, aprovechará de esas tecnologías para implementar "geek" Quiz, un sitio Web de curiosidades basado en el concepto SPA. En primer lugar, implementará la capa de servicio con ASP.NET Web API para exponer los puntos de conexión necesarias para recuperar las preguntas del cuestionario y almacenen las respuestas. A continuación, creará una interfaz de usuario enriquecida y con capacidad de respuesta con los efectos de transformación de AngularJS y CSS3.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear un servicio ASP.NET Web API para enviar y recibir datos JSON
- Crear una IU dinámica con AngularJS
- Mejorar la experiencia de interfaz de usuario con las transformaciones de CSS3

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para poder ejecutar los ejercicios en este laboratorio práctico, deberá configurar primero el entorno.

1. Abra el Explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga doble clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que se configure su entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo Control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso de los fragmentos de código

En todo el documento de laboratorio, se le pedirá que inserte los bloques de código. Para su comodidad, la mayoría de este código se proporciona como fragmentos de código de Visual Studio, puede acceder desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.

> [!NOTE]
> Cada ejercicio viene acompañado por una solución inicial ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de los demás. Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio faltan en estos a partir de las soluciones y es posible que no funcione hasta que haya completado el ejercicio. En el código fuente para un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos descritos en el ejercicio correspondiente. Puede usar estas soluciones como instrucciones si necesita más ayuda mientras se trabaja a través de este laboratorio práctico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los ejercicios siguientes:

1. [Creación de una API Web](#Exercise1)
2. [Creación de una interfaz SPA](#Exercise2)

Tiempo estimado para completar esta práctica: **60 minutos**

> [!NOTE]
> Primera vez que inicie Visual Studio, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos de este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>Ejercicio 1: Creación de una API Web

Una de las partes principales de una SPA es la capa de servicio. Es responsable de procesar las llamadas Ajax enviadas por la interfaz de usuario y la devolución de datos en respuesta a esa llamada. Los datos recuperados deben presentarse en un formato legible para analizarse y consumido por el cliente.

El marco API Web es parte de la pila de ASP.NET y está diseñado para que sea fácil de implementar los servicios HTTP, por lo general, enviar y recibir datos con formato JSON o XML a través de una API de RESTful. En este ejercicio se creará el sitio Web para hospedar la aplicación de prueba "geek" y, a continuación, implementar el servicio de back-end para exponer y conservar los datos de prueba mediante ASP.NET Web API.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>Tarea 1: crear el proyecto inicial de prueba "geek"

En esta tarea se iniciará la creación de un nuevo proyecto de ASP.NET MVC con compatibilidad para ASP.NET Web API se basa en el **One ASP.NET** tipo que se incluye con Visual Studio del proyecto. **One ASP.NET** unifica todas las tecnologías de ASP.NET y le ofrece la opción de mezclar y combinar ellos según sea necesario. A continuación, agregará las clases de modelo de Entity Framework y el initializator de base de datos para insertar las preguntas de la prueba.

1. Abra **Visual Studio Express 2013 para Web** y seleccione **archivo | Nuevo proyecto...**  para iniciar una nueva solución.

    ![Crear un nuevo proyecto](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "crear un nuevo proyecto")

    *Crear un nuevo proyecto*
2. En el **nuevo proyecto** cuadro de diálogo, seleccione **aplicación Web ASP.NET** bajo el **Visual C# | Web** ficha. Asegúrese de que **.NET Framework 4.5** está seleccionada, asígnele el nombre *GeekQuiz*, elija un **ubicación** y haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de aplicación Web ASP.NET](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "crear un nuevo proyecto de aplicación Web ASP.NET")

    *Crear un nuevo proyecto de aplicación Web ASP.NET*
3. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **MVC** plantilla y seleccione el **API Web** opción. Además, asegúrese de que el **autenticación** opción está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla MVC, incluidos los componentes de la API Web*
4. En **el Explorador de soluciones**, haga clic en el **modelos** carpeta de la **GeekQuiz** del proyecto y seleccione **Add | Elemento existente...** .

    ![Agregar un elemento existente](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "agregando un elemento existente")

    *Agregar un elemento existente*
5. En el **Agregar elemento existente** diálogo cuadro, vaya a la **activos/origen/modelos** carpeta y seleccione todos los archivos. Haga clic en **Agregar**.

    ![Agregar los recursos de modelo](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "agregar los recursos de modelo")

    *Agregar los recursos de modelo*

    > [!NOTE]
    > Mediante la adición de estos archivos, va a agregar el modelo de datos, el contexto de base de datos de Entity Framework y el inicializador de base de datos para la aplicación de prueba "geek".
    > 
    > **Entity Framework (EF)** es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional. Puede aprender más acerca de Entity Framework [aquí](../../../entity-framework.md).
    > 
    > La siguiente es una descripción de las clases que acaba de agregar:
    > 
    > - **TriviaOption:** representa una opción única asociada con una pregunta de prueba
    > - **TriviaQuestion:** representa una pregunta cuestionario y expone las opciones asociadas a través de la **opciones** propiedad
    > - **TriviaAnswer:** representa la opción seleccionada por el usuario en respuesta a una pregunta de prueba
    > - **TriviaContext:** representa el contexto de base de datos de Entity Framework de la aplicación de prueba "geek". Esta clase se deriva de **DContext** y expone **DbSet** propiedades que representan las colecciones de las entidades que se ha descrito anteriormente.
    > - **TriviaDatabaseInitializer:** la implementación del inicializador de Entity Framework la **TriviaContext** clase que hereda de **CreateDatabaseIfNotExists**. Insertar las entidades es el comportamiento predeterminado de esta clase crear la base de datos solo si no existe, especificado en el **inicialización** método.
6. Abra el **Global.asax.cs** de archivo y agregue la siguiente instrucción using.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. Agregue el código siguiente al principio de la **aplicación\_iniciar** método para establecer el **TriviaDatabaseInitializer** como un inicializador de la base de datos.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. Modificar el **inicio** controlador para restringir el acceso a los usuarios autenticados. Para ello, abra el **HomeController.cs** archivo dentro de la **controladores** carpeta y agregue el **Authorize** atributo a la **HomeController**definición de clase.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > El **Authorize** filtrar comprueba si el usuario está autenticado. Si el usuario no está autenticado, devuelve código de estado HTTP 401 (no autorizado) sin invocar la acción. Puede aplicar el filtro globalmente, en el nivel de controlador, o en el nivel de acciones individuales.
9. Ahora va a personalizar el diseño de las páginas web y la personalización de marca. Para ello, abra el  **\_Layout.cshtml** archivo dentro de la **vistas | Compartido** carpeta y actualizar el contenido de la **&lt;título&gt;** elemento reemplazando *My ASP.NET Application* con *"geek" Quiz* .

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. En el mismo archivo, actualice la barra de navegación quitando el *sobre* y *póngase en contacto con* vínculos y cambiar el nombre de la *inicio* vincular a *reproducir*. Además, cambie el nombre de la *nombre de la aplicación* vincular a *"geek" Quiz*. El código HTML de la barra de navegación debe ser similar al código siguiente.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. Actualizar el pie de página de la página de diseño reemplazando *My ASP.NET Application* con *"geek" Quiz*. Para ello, reemplace el contenido de la **&lt;pie de página&gt;** elemento con el siguiente código resaltado.

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>Tarea 2: creación de la API de Web TriviaController

En la tarea anterior, ha creado la estructura inicial de la aplicación web de cuestionarios "geek". Ahora creará un servicio de API Web simple que interactúa con el modelo de datos de prueba y expone las siguientes acciones:

- **GET/api/curiosidades**: recupera la siguiente pregunta de la lista de prueba que deben responderse por el usuario autenticado.
- **POST/api/curiosidades**: almacena la respuesta de la prueba especificada por el usuario autenticado.

Utilizará las herramientas de Scaffolding de ASP.NET proporcionadas por Visual Studio para crear la línea base para la clase de controlador Web API.

1. Abra el **WebApiConfig.cs** archivo dentro de la **aplicación\_iniciar** carpeta. Este archivo define la configuración del servicio de API Web, similar a cómo se asignan las rutas a acciones del controlador Web API.
2. Agregue la siguiente instrucción using al principio del archivo.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. Agregue el código resaltado siguiente a la **registrar** método para configurar globalmente el formateador para los datos JSON recuperados por los métodos de acción de la API Web.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > El **CamelCasePropertyNamesContractResolver** convierte automáticamente los nombres de propiedad a *camel* caso, que es la convención general para los nombres de propiedad en JavaScript.
4. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **GeekQuiz** del proyecto y seleccione **Add | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo elemento con scaffold](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "crear un nuevo elemento con scaffolding")

    *Crear un nuevo elemento con scaffolding*
5. En el **agregar Scaffold** diálogo cuadro, asegúrese de que el **común** nodo está seleccionado en el panel izquierdo. A continuación, seleccione el **controlador Web API 2 - vacío** plantilla en el panel central y haga clic en **agregar**.

    ![Seleccione la plantilla vacía de controlador Web API 2](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "seleccionando la plantilla vacía de controlador Web API 2")

    *Seleccione la plantilla vacía de controlador Web API 2*

    > [!NOTE]
    > **Scaffolding de ASP.NET** es un marco de generación de código para aplicaciones Web ASP.NET. Visual Studio 2013 incluye generadores de código instalado previamente para los proyectos de MVC y Web API. Debe usar el scaffolding del proyecto cuando desee agregar rápidamente código que interactúe con modelos de datos con el fin de reducir la cantidad de tiempo necesario para desarrollar las operaciones de datos estándar.
    > 
    > El proceso de scaffolding también garantiza que todas las dependencias necesarias están instaladas en el proyecto. Por ejemplo, si se inicia con un proyecto ASP.NET vacío y, a continuación, usar el scaffolding para agregar un controlador Web API, los paquetes de API NuGet Web necesarios y las referencias se agregan al proyecto automáticamente.
6. En el **Agregar controlador** cuadro de diálogo, escriba *TriviaController* en el **nombre del controlador** cuadro de texto y haga clic en **agregar**.

    ![Agregar el controlador de curiosidades](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "adición del controlador de curiosidades")

    *Agregar el controlador de curiosidades*
7. El **TriviaController.cs** archivo, a continuación, se agrega a la **controladores** carpeta de la **GeekQuiz** proyecto, que contiene un valor vacío **TriviaController** clase. Agregue las siguientes instrucciones using al principio del archivo.

    (Código de fragmento de código - *TriviaControllerUsings AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. Agregue el código siguiente al principio de la **TriviaController** clase para definir, inicializar y eliminar el **TriviaContext** instancia en el controlador.

    (Código de fragmento de código - *TriviaControllerContext AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > El **Dispose** método **TriviaController** invoca el **Dispose** método de la **TriviaContext** instancia, lo que garantiza que todos los se liberan los recursos utilizados por el objeto de contexto cuando el **TriviaContext** instancia se elimina o se recopilan de elementos no utilizados. Esto incluye cerrar todas las conexiones de base de datos abiertas por Entity Framework.
9. Agregue el siguiente método auxiliar al final de la **TriviaController** clase. Este método recupera la siguiente pregunta de prueba de la base de datos que deben responderse por el usuario especificado.

    (Código de fragmento de código - *TriviaControllerNextQuestion AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. Agregue el siguiente **obtener** método de acción para el **TriviaController** clase. Llama este método de acción el **NextQuestionAsync** método auxiliar definido en el paso anterior para recuperar la siguiente pregunta para el usuario autenticado.

    (Código de fragmento de código - *TriviaControllerGetAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. Agregue el siguiente método auxiliar al final de la **TriviaController** clase. Este método almacena la respuesta especificada en la base de datos y devuelve un valor booleano que indica si la respuesta es correcta.

    (Código de fragmento de código - *TriviaControllerStoreAsync AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. Agregue el siguiente **Post** método de acción para el **TriviaController** clase. Este método de acción asocia la respuesta para el usuario autenticado y llama el **StoreAsync** método auxiliar. A continuación, envía una respuesta con el valor booleano devuelto por el método auxiliar.

    (Código de fragmento de código - *TriviaControllerPostAction AspNetWebApiSpa - Ex1 -*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. Modifique el controlador de Web API para restringir el acceso a los usuarios autenticados mediante la adición de la **Authorize** atributo a la **TriviaController** definición de clase.

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea comprobará que el servicio de API Web creado en la tarea anterior funciona según lo previsto. Va a usar el Explorador de Internet **herramientas de desarrollo F12** para capturar el tráfico de red e inspeccionar la respuesta completa desde el servicio de API Web.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. Presione **F5** para ejecutar la solución. El **iniciarla** página debería aparecer en el explorador.

    > [!NOTE]
    > Cuando se inicia la aplicación, se desencadena la ruta MVC predeterminada, que de forma predeterminada se asigna a la **índice** acción de la **HomeController** clase. Puesto que **HomeController** está restringido a los usuarios autenticados (Recuerde que representativos de esa clase con la **Authorize** atributo en el ejercicio 1) y no hay ningún usuario autenticado todavía, la aplicación redirige la solicitud original en el registro en la página.

    ![Ejecución de la solución](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "ejecución de la solución")

    *Ejecución de la solución*
2. Haga clic en **registrar** para crear un nuevo usuario.

    ![Registrar un nuevo usuario](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "registrar un nuevo usuario")

    *Registrar un nuevo usuario*
3. En el **registrar** , escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página registrar](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "página de registro")

    *Página de registro*
4. La aplicación registra la nueva cuenta y el usuario está autenticado y redirigido de nuevo a la página principal.

    ![Se autentica el usuario](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "usuario autenticado")

    *Se autentica el usuario*
5. En el explorador, presione **F12** para abrir el **Developer Tools** panel. Presione **CTRL + 4** o haga clic en el **red** icono y, a continuación, haga clic en botón de la flecha verde para empezar a capturar el tráfico de red.

    ![Iniciando la captura de red de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "captura de red iniciando la API Web")

    *Iniciando la captura de red de la API Web*
6. Anexar **api/curiosidades** a la dirección URL en la barra de direcciones del explorador. Ahora inspeccionará los detalles de la respuesta de la **obtener** los métodos de acción **TriviaController**.

    ![Recuperar los datos de la pregunta siguientes a través de Web API](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "recuperar los datos de la pregunta siguientes a través de Web API")

    *Recuperar los datos de la pregunta siguientes a través de Web API*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje el cuadro de diálogo Abrir con el fin de poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.
7. Ahora inspeccionará el cuerpo de la respuesta. Para ello, haga clic en el **detalles** pestaña y, a continuación, haga clic en **cuerpo de respuesta**. Puede comprobar que los datos descargados están un objeto con las propiedades **opciones** (que es una lista de **TriviaOption** objetos), **id** y **título** que corresponden a la **TriviaQuestion** clase.

    ![Ver el cuerpo de respuesta de la API Web](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "ver el cuerpo de respuesta de la API Web")

    *Cuerpo de respuesta de la API Web de visualización*
8. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>Ejercicio 2: Creación de la interfaz SPA

En este ejercicio primero creará la parte de front-end web de cuestionarios "geek", centrándose en la interacción de aplicaciones de página única mediante **AngularJS**. A continuación, mejorará la experiencia del usuario con CSS3 para realizar las animaciones y proporcionan un efecto visual de contexto al realizar la transición de una pregunta a la siguiente.

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>Tarea 1: creación de la interfaz SPA con AngularJS

En esta tarea usará **AngularJS** para implementar el cliente de la aplicación de prueba "geek". **AngularJS** es un marco de JavaScript de código abierto que aumenta las aplicaciones basadas en explorador con *Model-View-Controller* capacidad (MVC), facilita el desarrollo y pruebas.

Se iniciará mediante la instalación de AngularJS desde la consola de administrador de paquetes de Visual Studio. A continuación, creará el controlador para proporcionar el comportamiento de la aplicación de prueba "geek" y la vista para representar el preguntas y respuestas con el motor de plantillas de AngularJS.

> [!NOTE]
> Para obtener más información acerca de AngularJS, consulte [ [ http://angularjs.org/ ](http://angularjs.org/) ](http://angularjs.org/).


1. Abra **Visual Studio Express 2013 para Web** y abra el **GeekQuiz.sln** solución se encuentra en la **Begin/origen/Ex2-CreatingASPAInterface** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. Abra el **Package Manager Console** desde **herramientas** | **Administrador de paquetes de biblioteca**. Escriba el siguiente comando para instalar el **AngularJS.Core** paquete NuGet.

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. En **el Explorador de soluciones**, haga clic en el **Scripts** carpeta de la **GeekQuiz** del proyecto y seleccione **Add | Nueva carpeta**. Nombre de la carpeta **aplicación** y presione **ENTRAR**.
4. Haga clic en el **aplicación** carpeta que acaba de crea y seleccione **Add | Archivo JavaScript**.

    ![Crear un nuevo archivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *Crear un nuevo archivo JavaScript*
5. En el **especificar nombre para el elemento** cuadro de diálogo, escriba *cuestionario controlador* en el **nombre del elemento** cuadro de texto y haga clic en **Aceptar**.

    ![Nombre al nuevo archivo JavaScript](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *Nombre al nuevo archivo JavaScript*
6. En el **cuestionario controller.js** , agregue el siguiente código para declarar e inicializar AngularJS **QuizCtrl** controlador.

    (Código de fragmento de código - *AngularQuizController AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > La función de constructor de la **QuizCtrl** controlador espera un parámetro inyectables denominado **$scope**. El estado inicial del ámbito debe configurarse en la función constructora adjuntando propiedades para el **$scope** objeto. Las propiedades contienen el **modelo de vista**y podrá tener acceso a la plantilla cuando se registra el controlador.
    > 
    > El **QuizCtrl** controlador se define dentro de un módulo denominado **QuizApp**. Los módulos son unidades de trabajo que le permiten dividir la aplicación en componentes independientes. Las principales ventajas del uso de módulos es que el código es más fácil de entender y facilita las pruebas unitarias, reusabilidad y facilidad de mantenimiento.
7. Ahora agregará comportamiento para el ámbito para reaccionar ante los eventos desencadenados desde la vista. Agregue el código siguiente al final de la **QuizCtrl** controlador para definir el **nextQuestion** funcionando en el **$scope** objeto.

    (Código de fragmento de código - *AngularQuizControllerNextQuestion AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > Esta función recupera la siguiente pregunta de la **curiosidades** API Web creada en el ejercicio anterior y adjunta los datos de la pregunta a la **$scope** objeto.
8. Inserte el código siguiente al final de la **QuizCtrl** controlador para definir el **sendAnswer** funcionando en el **$scope** objeto.

    (Código de fragmento de código - *AngularQuizControllerSendAnswer AspNetWebApiSpa - Ex2 -*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > Esta función envía la respuesta seleccionada por el usuario para el **curiosidades** API Web y almacena el resultado: es decir, si la respuesta es correcta o no: en el **$scope** objeto.
    > 
    > El **nextQuestion** y **sendAnswer** funciones anteriores utilizan AngularJS **$http** objeto abstraer la comunicación con la API Web mediante el objeto XMLHttpRequest Objeto de JavaScript desde el explorador. AngularJS es compatible con otro servicio que aporta un mayor nivel de abstracción para realizar operaciones de CRUD en un recurso a través de las API de RESTful. AngularJS **$resource** objeto tiene métodos de acción que proporcionan un alto nivel comportamientos sin necesidad de interactuar con el **$http** objeto. Considere el uso de la **$resource** objeto en escenarios que requiere el modelo de CRUD (para más información, consulte el [$resource documentación](https://docs.angularjs.org/api/ngResource/service/$resource)).
9. El siguiente paso es crear la plantilla de AngularJS que define la vista de la prueba. Para ello, abra el **Index.cshtml** archivo dentro de la **vistas | Inicio** carpeta y reemplace el contenido con el código siguiente.

    (Código de fragmento de código - *GeekQuizView AspNetWebApiSpa - Ex2 -*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > La plantilla de AngularJS es una especificación declarativa que usa la información del modelo y el controlador para transformar el marcado estático en la vista dinámica que el usuario ve en el explorador. Los siguientes son ejemplos de AngularJS elementos y atributos del elemento que se pueden usar en una plantilla:
    > 
    > - El **ng-app** directiva indica a AngularJS el elemento DOM que representa el elemento raíz de la aplicación.
    > - El **ng-controller** directiva adjunta un controlador en el DOM en el punto donde se declara la directiva.
    > - La notación de llave **{{}}** denota los enlaces a las propiedades del ámbito definidos en el controlador.
    > - El **ng-click** directiva se usa para invocar las funciones definidas en el ámbito en respuesta a los clics del usuario.
10. Abra el **Site.css** archivo dentro de la **contenido** carpeta y agregue los siguientes estilos de resaltado al final del archivo para proporcionar una apariencia y funcionamiento de la vista de prueba.

    (Código de fragmento de código - *GeekQuizStyles AspNetWebApiSpa - Ex2 -*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea tendrá que ejecutar la solución con el nuevo usuario de interfaz que se han creado con AngularJS para responder a algunas de las preguntas de prueba.

1. Presione **F5** para ejecutar la solución.
2. Registrar una nueva cuenta de usuario. Para ello, siga los pasos de registro que se describe en el ejercicio 1, 3 de la tarea.

    > [!NOTE]
    > Si usa la solución del ejercicio anterior, puede iniciar sesión con la cuenta de usuario que creó antes.
3. El **inicio** debería aparecer página, que muestra la primera pregunta de la prueba. Responda a la pregunta, haga clic en una de las opciones. Esto desencadenará el **sendAnswer** función definida anteriormente, que envía la opción seleccionada a la **curiosidades** API Web.

    ![Responder a una pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "responder una pregunta")

    *Responder a una pregunta*
4. Después de hacer clic en uno de los botones, debería aparecer la respuesta. Haga clic en **siguiente pregunta** para mostrar la siguiente pregunta. Esto desencadenará el **nextQuestion** definida en el controlador de función.

    ![Solicitar la siguiente pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "que solicita la siguiente pregunta")

    *Solicitar la siguiente pregunta*
5. Debería aparecer la siguiente pregunta. Continuar a responder preguntas tantas veces como desee. Después de completar todas las preguntas que debe devolver a la primera pregunta.

    ![Otra pregunta](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "otra pregunta")

    *Siguiente pregunta*
6. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>Tarea 3: creación de una animación voltear mediante CSS3

En esta tarea usará las propiedades de CSS3 para realizar las animaciones mediante la adición de un efecto flip cuando se responde a una pregunta y cuando se recupera la siguiente pregunta.

1. En **el Explorador de soluciones**, haga clic en el **contenido** carpeta de la **GeekQuiz** del proyecto y seleccione **Add | Elemento existente...** .

    ![Agregar un elemento existente en la carpeta Content](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "agregando un elemento existente a la carpeta de contenido")

    *Agregar un elemento existente a la carpeta de contenido*
2. En el **Agregar elemento existente** diálogo cuadro, vaya a la **origen/activos** carpeta y seleccione **Flip.css**. Haga clic en **Agregar**.

    ![Agregar el archivo Flip.css desde activos](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "agregando el archivo Flip.css de recursos")

    *Agregar el archivo Flip.css de recursos*
3. Abra el **Flip.css** archivo que se acaba de agregar e inspeccionar su contenido.
4. Busque el **voltear transformación** comentario. Los estilos de comentario usar CSS **perspectiva** y **rotateY** transformaciones para generar un &quot;tarjeta flip&quot; efecto.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. Busque el **ocultar parte posterior del panel durante flip** comentario. El estilo de comentario oculta del lado posterior de las caras cuando tienes lejos del Visor estableciendo el **visibilidad de cara** propiedad CSS a *oculto*.

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. Abierto el **BundleConfig.cs** archivo dentro de la **aplicación\_iniciar** carpeta y agregue la referencia a la **Flip.css** de archivos en el **&quot;~/Content/css&quot;** agrupación de estilo

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. Presione **F5** para ejecutar la solución e inicie sesión con sus credenciales.
8. Responder a una pregunta, haga clic en una de las opciones. Tenga en cuenta el efecto flip al realizar la transición entre las vistas.

    ![Responder a una pregunta con el efecto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "responder una pregunta con el efecto flip")

    *Responder a una pregunta con el efecto flip*
9. Haga clic en **siguiente pregunta** para recuperar la siguiente pregunta. El efecto flip debería aparecer de nuevo.

    ![Recuperar la siguiente pregunta con el efecto flip](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "recuperar la siguiente pregunta con el efecto flip")

    *Recuperar la siguiente pregunta con el efecto flip*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Crear un controlador de ASP.NET Web API mediante Scaffolding de ASP.NET
- Implementar una acción de obtención de API Web para recuperar la siguiente pregunta de prueba
- Implementar una acción de publicación de Web API para almacenar las respuestas del cuestionario
- Instalar AngularJS desde la consola de administrador de paquetes de Visual Studio
- Implementar plantillas de AngularJS y controladores
- Use las transiciones de CSS3 para realizar efectos de animación
