---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio de prácticas: Uno ASP.NET: integrar formularios ASP.NET Web Forms, MVC y API Web | Documentos de Microsoft'
author: rick-anderson
description: ASP.NET es un marco para la creación de sitios Web, aplicaciones y servicios mediante tecnologías especializadas como MVC, Web API y otros. Con la expansión h ASP.NET...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 55109723e566a9f7c66c1a59414377b05dbec760
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio de prácticas: Uno ASP.NET: integrar formularios ASP.NET Web Forms, MVC y API Web
====================
por [Web colonias equipo](https://twitter.com/webcamps)

[Descargar el Kit de aprendizaje de colonias de Web](http://aka.ms/webcamps-training-kit)

> ASP.NET es un marco para la creación de sitios Web, aplicaciones y servicios mediante tecnologías especializadas como MVC, Web API y otros. Con la expansión de ASP.NET ha visto desde su creación y la expresa necesita tener estas tecnologías integradas, ha habido recientes esfuerzos de trabajo hacia **One ASP.NET**.
> 
> Visual Studio 2013 presenta un nuevo sistema de proyectos unificada que permite compilar una aplicación y utilizar todas las tecnologías ASP.NET en un proyecto. Esta característica elimina la necesidad de elegir una tecnología al principio de un proyecto y stick con él y en su lugar, recomienda el uso de varios marcos de trabajo ASP.NET dentro de un proyecto.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear un sitio Web basado en la **One ASP.NET** tipo de proyecto
- Usar diferentes **ASP.NET** marcos como **MVC** y **API Web** en el mismo proyecto
- Identificar los componentes principales de un **ASP.NET** aplicación
- Aprovechar las ventajas de la **ASP.NET Scaffolding** framework para crear automáticamente los controladores y vistas para llevar a cabo operaciones CRUD en función de las clases de modelo
- Exponer el mismo conjunto de información de máquina y legible para las personas formatos usando la herramienta adecuada para cada trabajo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Para completar este laboratorio de prácticas, es necesario lo siguiente:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [Crear un nuevo proyecto de formularios Web](#Exercise1)
2. [Crear un controlador MVC con Scaffolding](#Exercise2)
3. [Creación de un controlador de API Web mediante Scaffolding](#Exercise3)

Tiempo estimado para completar esta práctica: **60 minutos**

> [!NOTE]
> Cuando se inicia Visual Studio por primera vez, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos descritos en este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ejercicio 1: Crear un nuevo proyecto de formularios Web

En este ejercicio se creará un nuevo sitio de formularios Web Forms en Visual Studio 2013 usando la **One ASP.NET** unificado experiencia en el proyecto, lo que le permitirá integrar fácilmente los componentes de formularios Web Forms, MVC y API Web en la misma aplicación. Podrá explorar la solución generada e identificar sus partes, y por último, verá el sitio Web en acción.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tarea 1: crear un sitio nuevo con una experiencia de ASP.NET

En esta tarea se iniciará crea un nuevo sitio Web en Visual Studio basado en la **One ASP.NET** tipo de proyecto. **Uno ASP.NET** unifica todas las tecnologías ASP.NET y ofrece la opción de mezclar y hacerlos coincidir según sea necesario. A continuación, reconocerá los diferentes componentes de formularios Web Forms, MVC y API Web que se encuentran en paralelo dentro de la aplicación.

1. Abra **Visual Studio Express 2013 para Web** y seleccione **archivo | Nuevo proyecto...**  para iniciar una nueva solución.

    ![Crear un proyecto nuevo](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Crear un nuevo proyecto*
2. En el **nuevo proyecto** cuadro de diálogo, seleccione **aplicación Web ASP.NET** en el **Visual C# | Web** pestaña y asegúrese de que **.NET Framework 4.5** está seleccionada. Denomine el proyecto *MyHybridSite*, elija un **ubicación** y haga clic en **Aceptar**.

    ![Nuevo proyecto de aplicación Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Crear un nuevo proyecto de aplicación Web ASP.NET*
3. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione la **formularios Web Forms** plantilla y seleccione el **MVC** y **API Web** opciones. Además, asegúrese de que el **autenticación** opción está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de API Web y MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de API Web y MVC*
4. Ahora puede explorar la estructura de la solución generada.

    ![Explorar la solución generada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Explorar la solución generada*

    1. **Cuenta:** esta carpeta contiene las páginas de formularios Web Forms para registrar, inicie sesión en y administrar cuentas de usuario de la aplicación. Esta carpeta se agrega cuando el **cuentas de usuario individuales** se selecciona la opción de autenticación durante la configuración de la plantilla de proyecto de formularios Web Forms.
    2. **Modelos:** esta carpeta contiene las clases que representan los datos de aplicación.
    3. **Controladores de** y **vistas**: estas carpetas son necesarias para la **ASP.NET MVC** y **ASP.NET Web API** componentes. Explorará las tecnologías MVC y API Web en los ejercicios siguientes.
    4. El **Default.aspx**, **Contact.aspx** y **About.aspx** archivos son páginas de formularios Web Forms predefinidas que puede usar como punto de partida para crear las páginas específicas de su aplicación. La lógica de programación de dichos archivos reside en un archivo independiente que se conoce como el &quot;código subyacente&quot; archivo, que tiene un &quot;. aspx.vb&quot; o &quot;. aspx.cs&quot; extensión (en función de la lenguaje utilizado). La lógica de código subyacente se ejecuta en el servidor y genera dinámicamente la salida HTML de la página.
    5. El **Site.Master** y **Site.Mobile.Master** páginas definen la apariencia y el comportamiento estándar de todas las páginas en la aplicación.
5. Haga doble clic en el **Default.aspx** archivo para explorar el contenido de la página.

    ![Explorar la página Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Explorar la página Default.aspx*

    > [!NOTE]
    > El **página** la directiva en la parte superior del archivo define los atributos de la página de formularios Web Forms. Por ejemplo, el **MasterPageFile** atributo especifica la ruta de acceso al maestro de página: en este caso, el *Site.Master* página y la **Inherits** atributo define el clase de código subyacente de la página para heredar. Esta clase se encuentra en el archivo determinado por la **código subyacente** atributo.
    > 
    > El **asp: Content** control mantiene el contenido real de la página (texto, marcado y controles) y se asigna a un **asp: ContentPlaceHolder** control en la página maestra. En este caso, el contenido de la página se representará dentro de la *MainContent* control definido en el *Site.Master* página.
6. Expanda el **aplicación\_iniciar** carpeta y observe el **WebApiConfig.cs** archivo. Visual Studio incluye ese archivo en la solución generada porque incluye API Web al configurar el proyecto con la plantilla de One ASP.NET.
7. Abra la **WebApiConfig.cs** archivo. En el *WebApiConfig* clase encontrará la configuración asociada a la API Web, que se asigna HTTP enruta a **controladores de API Web**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Abra la **RouteConfig.cs** archivo. Dentro de la *RegisterRoutes* encontrará la configuración asociada a MVC, que se asigna la ruta HTTP para el método **controladores MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecutar la solución

En esta tarea se ejecute la solución generada, explorar la aplicación y algunas de sus características, como la reescritura de direcciones URL y la autenticación integrada.

1. Para ejecutar la solución, presione **F5** o haga clic en el **iniciar** situado en la barra de herramientas. Debe abrir la página de inicio de la aplicación en el explorador.

    ![Ejecución de la solución](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Compruebe que se está llamando a las páginas de formularios Web Forms. Para ello, anexe **/contact.aspx** a la dirección URL en la barra de direcciones y presione **ENTRAR**.

    ![Direcciones URL descriptivas](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Direcciones URL descriptivas*

    > [!NOTE]
    > Como puede ver, la dirección URL cambia a **o póngase en contacto con**. A partir de **ASP.NET 4**, capacidades de enrutamiento de dirección URL se agregaron a formularios Web Forms, por lo que puede escribir las direcciones URL como *[http://www.mysite.com/products/software](http://www.mysite.com/products/software)* en lugar de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Para obtener más información, consulte [enrutamiento de direcciones URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Ahora explorará el flujo de autenticación que se integra en la aplicación. Para ello, haga clic en **registrar** en la esquina superior derecha de la página.

    ![Registrar un nuevo usuario](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrar un nuevo usuario*
4. En el **registrar** página, escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página registrar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Página registrar*
5. La aplicación registra la nueva cuenta, y el usuario está autenticado.

    ![Usuario autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Usuario autenticado*
6. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ejercicio 2: Crear un controlador MVC con Scaffolding

En este ejercicio tendrá aprovechar el marco de trabajo de ASP.NET Scaffolding proporcionada por Visual Studio para crear un controlador de ASP.NET MVC 5 con acciones y vistas de Razor para realizar operaciones CRUD, sin escribir una sola línea de código. El proceso de scaffolding Utilice Entity Framework Code First para generar el contexto de datos y el esquema de base de datos en la base de datos SQL.

**Acerca de Entity Framework Code First**

Entity Framework (EF) es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional.

El flujo de trabajo de modelado Entity Framework Code First le permite utilizar sus propias clases de dominio para representar el modelo que dependa de EF al realizar la consulta, las funciones de seguimiento de cambios y actualizaciones. Mediante el flujo de trabajo de desarrollo Code First, es necesario iniciar la aplicación mediante la creación de una base de datos o especificar un esquema. En su lugar, puede escribir las clases de .NET estándar que definen los objetos de modelo de dominio más adecuados para su aplicación y Entity Framework creará la base de datos por usted.

> [!NOTE]
> Puede aprender más acerca de Entity Framework [aquí](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tarea 1: crear un nuevo modelo

Ahora va a definir un **persona** (clase), que será el modelo utilizado por el proceso de scaffolding para crear el controlador MVC y las vistas. Primero debe crear un **persona** clase de modelo y las operaciones CRUD en el controlador se crearán automáticamente con características de scaffolding.

1. Abra **Visual Studio Express 2013 para Web** y **MyHybridSite.sln** soluciones se encuentran en la **origen/Ex2-MvcScaffolding/Begin** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **el Explorador de soluciones**, haga clic en el **modelos** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Clase...** .

    ![Agregar la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Agregar la clase de modelo de persona*
3. En el **Agregar nuevo elemento** cuadro de diálogo, el nombre del archivo *Person.cs* y haga clic en **agregar**.

    ![Crear la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Crear la clase de modelo de persona*
4. Reemplace el contenido de la **Person.cs** archivo con el código siguiente. Presione **CTRL + S** para guardar los cambios.

    (Código de fragmento de código: *PersonClass BringingTogetherOneAspNet - Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. En **el Explorador de soluciones**, haga clic en el **MyHybridSite** de proyecto y seleccione **generar**, o bien presione **CTRL + MAYÚS + B** para compilar el proyecto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tarea 2: crear un controlador MVC

Ahora que el **persona** se crea un modelo, se utilizará el scaffolding de ASP.NET MVC con Entity Framework para crear las acciones del controlador CRUD y vistas para **persona**.

1. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Crear un nuevo controlador de scaffolding*
2. En el **agregar scaffolding** cuadro de diálogo, seleccione **controlador de MVC 5 con vistas, mediante Entity Framework** y, a continuación, haga clic en **agregar.**

    ![Seleccionar controlador de MVC 5 con vistas y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Seleccionar controlador de MVC 5 con vistas y Entity Framework*
3. Establecer *MvcPersonController* como el **nombre de controlador**, seleccione la **utilizar las acciones de controlador asincrónico** opción y seleccione **persona (MyHybridSite.Models)**  como el **clase modelo**.

    ![Agregar un controlador de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Agregar un controlador de MVC con scaffolding*
4. En **clase de contexto de datos**, haga clic en **nuevo contexto de datos...** .

    ![Crear un nuevo contexto de datos](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Crear un nuevo contexto de datos*
5. En el **nuevo contexto de datos** nombre el nuevo contexto de datos de cuadro de diálogo *PersonContext* y haga clic en **agregar**.

    ![Crear la nueva PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Crear el nuevo tipo de PersonContext*
6. Haga clic en **agregar** para crear el nuevo controlador de **persona** con la técnica scaffolding. Visual Studio, a continuación, generará las acciones de controlador, el contexto de datos de la persona y las vistas de Razor.

    ![Después de crear el controlador MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Después de crear el controlador MVC con scaffolding*
7. Abra la **MvcPersonController.cs** un archivo en el **controladores** carpeta. Tenga en cuenta que los métodos de acción CRUD se han generado automáticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Si selecciona el **utilizar las acciones de controlador asincrónico** opciones de casilla de verificación de la técnica scaffolding en los pasos anteriores, Visual Studio genera los métodos de acción asincrónicos para todas las acciones que implican el acceso en el contexto de datos de la persona. Se recomienda utilizar métodos de acción asincrónicos de larga duración, no con la CPU solicitudes para evitar el bloqueo en el servidor Web realizar trabajo mientras se procesa la solicitud.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecutar la solución

En esta tarea, se ejecutará la solución para comprobar que las vistas de **persona** funcionan del modo esperado. Se agregará una nueva persona para comprobar que se guardó correctamente para la base de datos.

1. Presione **F5** para ejecutar la solución.
2. Vaya a **/MvcPerson**. Debería aparecer la vista con scaffolding que muestra la lista de usuarios.
3. Haga clic en **crear nuevo** para agregar una nueva persona.

    ![Navegar a las vistas MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Navegar a las vistas MVC con scaffolding*
4. En el **crear** ver, proporcione un **nombre** y un **Age** la persona y haga clic en **crear**.

    ![Agregar una nueva persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Agregar una nueva persona*
5. La persona nueva se agrega a la lista. En la lista de elementos, haga clic en **detalles** para mostrar la vista de detalles de la persona. A continuación, en la **detalles** ver, haga clic en **volver a la lista** para volver a la vista de lista.

    ![Vista de detalles de la persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Vista de detalles de la persona*
6. Haga clic en el **eliminar** vínculo Eliminar de la persona. En el **eliminar** ver, haga clic en **eliminar** para confirmar la operación.

    ![Eliminación de una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Eliminación de una persona*
7. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ejercicio 3: Crear un controlador de API Web mediante Scaffolding

El marco Web API forma parte de la pila de ASP.NET y diseñado para facilitar la implementación de los servicios HTTP, por lo general, enviar y recibir datos con formato JSON o XML a través de la API de REST.

En este ejercicio, se usará Scaffolding de ASP.NET nuevo para generar un controlador de Web API. Utilizará las mismas **persona** y **PersonContext** clases desde el ejercicio anterior para proporcionar los mismos datos de la persona en formato JSON. Verá cómo puede exponer los mismos recursos de maneras diferentes dentro de la misma aplicación de ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tarea 1: creación de un controlador de API Web

En esta tarea se creará una nueva **controlador de API Web** que va a exponer los datos de la persona en un formato de equipo puede consumir como JSON.

1. Si aún no está abierto, abra **Visual Studio Express 2013 para Web** y abra el **MyHybridSite.sln** soluciones se encuentran en la **origen/Ex3-WebAPI/Begin** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.

    > [!NOTE]
    > Si empieza con la solución de inicio de ejercicio 3, presione **CTRL + MAYÚS + B** para compilar la solución.
2. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** de proyecto y seleccione **agregar | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Crear un nuevo controlador con scaffolding*
3. En el **agregar scaffolding** cuadro de diálogo, seleccione **API Web** en el panel izquierdo, a continuación, **Web API 2 controlador con acciones que usan Entity Framework** en el panel central y, a continuación, haga clic en  **Agregar.**

    ![Seleccionar controlador de Web API 2 con acciones y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "seleccionar controlador de Web API 2 con acciones y Entity Framework")

    *Seleccionar controlador de Web API 2 con acciones y Entity Framework*
4. Establecer *ApiPersonController* como el **nombre de controlador**, seleccione la **utilizar las acciones de controlador asincrónico** opción y seleccione **persona (MyHybridSite.Models)**  y **PersonContext (MyHybridSite.Models)** como el **modelo** y **contexto de datos** clases respectivamente. A continuación, haga clic en **Agregar**.

    ![Cómo agregar un controlador de API Web con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "agregando un controlador de Web API con la técnica scaffolding")

    *Agregar un controlador de Web API con la técnica scaffolding*
5. Visual Studio, a continuación, generará el **ApiPersonController** clase con las cuatro acciones CRUD para trabajar con los datos.

    ![Después de crear el controlador de Web API con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "después de crear el controlador de Web API con la técnica scaffolding")

    *Después de crear el controlador de Web API con la técnica scaffolding*
6. Abra la **ApiPersonController.cs** archivo e inspeccione el *GetPeople* método de acción. Este método consulta el campo de base de datos de **PersonContext** tipo con el fin de obtener datos de las personas.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Ahora observe el comentario encima de la definición de método. Proporciona el URI que expone esta acción que se va a utilizar en la siguiente tarea.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > De forma predeterminada, la API Web está configurada para detectar las consultas para el */API* ruta de acceso para evitar conflictos con los controladores MVC. Si necesita cambiar esta configuración, consulte [enrutamiento de ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecutar la solución

En esta tarea utilizará el Explorador de Internet **herramientas de desarrollo F12** para inspeccionar la respuesta completa desde el controlador de API Web. Verá cómo puede capturar el tráfico de red para obtener más información sobre los datos de aplicación.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> El **herramientas de desarrollo F12** tiene un amplio conjunto de funcionalidad que no se trata en este laboratorio prácticas. Si desea obtener más información, consulte [mediante las herramientas de desarrollo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Presione **F5** para ejecutar la solución.

    > [!NOTE]
    > Para seguir esta tarea correctamente, la aplicación necesita que los datos. Si la base de datos está vacía, puede volver a la tarea 3 en el ejercicio 2 y siga los pasos sobre cómo crear a una nueva persona mediante las vistas MVC.
2. En el explorador, presione **F12** para abrir el **Developer Tools** panel. Presione **CTRL** + **4** o haga clic en el **red** icono y, a continuación, haga clic en la flecha verde botón para empezar a capturar el tráfico de red.

    ![Iniciar la captura de red de la API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "captura de red iniciando Web API")

    *Iniciar la captura de red de la API de Web*
3. Anexar **api/ApiPerson** a la dirección URL en la barra de direcciones del explorador. Ahora inspeccionará los detalles de la respuesta de la **ApiPersonController**.

    ![Recuperar datos de la persona a través de Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "recuperar datos de la persona a través de Web API")

    *Recuperar datos de la persona a través de Web API*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje el cuadro de diálogo abierto para poder ver el contenido de la respuesta a través de la ventana de herramientas de los desarrolladores.
4. Ahora inspeccionará el cuerpo de la respuesta. Para ello, haga clic en el **detalles** ficha y, a continuación, haga clic en **cuerpo de respuesta**. Puede comprobar que los datos descargados están una lista de objetos con las propiedades **identificador**, **nombre** y **Age** que corresponden a la **persona** clase.

    ![Ver el cuerpo de respuesta de la API de Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "ver cuerpo de respuesta de la API de Web")

    *Cuerpo de respuesta mediante una API Web de visualización*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tarea 3: adición de páginas de Ayuda de API de Web

Cuando se crea una API Web, resulta útil crear una página de ayuda para que otros desarrolladores sepan cómo llamar a la API. Puede crear y actualizar manualmente las páginas de documentación, pero es mejor para evitar tener que realizar trabajo de mantenimiento genere automáticamente. En esta tarea usará un paquete de Nuget para generar automáticamente las páginas de Ayuda de API Web a la solución.

1. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**y, a continuación, haga clic en **Package Manager Console**.
2. En el **Package Manager Console** ventana, ejecute el comando siguiente:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > El **Microsoft.AspNet.WebApi.HelpPage** paquete instala los ensamblados necesarios y agrega las vistas de MVC para las páginas de ayuda en la **áreas/HelpPage** carpeta.

    ![Área de HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage área")

    *Área de HelpPage*
3. De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación. Puede utilizar comentarios de documentación XML para crear la documentación. Para habilitar esta característica, abra la **HelpPageConfig.cs** archivo se encuentra en la **áreas/HelpPage/aplicación\_iniciar** carpeta y elimine la línea siguiente:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. En **el Explorador de soluciones**, haga clic en el proyecto **MyHybridSite**, seleccione **propiedades** y haga clic en el **generar** ficha.

    ![Pestaña compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "crear sección")

    *Generar (ficha)*
5. En **salida**, seleccione **archivo de documentación XML**. En el cuadro de edición, escriba **aplicación\_Data/XmlDocument.xml**.

    ![Salida de la sección en la ficha generar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sección en la pestaña de la compilación de salida")

    *Sección de salida en la ficha generar*
6. Presione **CTRL** + **S** para guardar los cambios.
7. Abra la **ApiPersonController.cs** de archivos desde el **controladores** carpeta.
8. Escriba una nueva línea entre la *GetPeople* firma del método y la */ / obtener api/ApiPerson* comentar y, a continuación, escriba tres barras diagonales.

    > [!NOTE]
    > Visual Studio inserta automáticamente los elementos XML que definen la documentación del método.
9. Agregar un texto de resumen y el valor devuelto para la *GetPeople* método. Debe ser similar al siguiente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Presione **F5** para ejecutar la solución.
11. Anexar **/help** a la dirección URL en la barra de direcciones para ir a la página de ayuda.

    ![Página de Ayuda de ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "página de Ayuda de ASP.NET Web API")

    *Página de Ayuda de ASP.NET Web API*

    > [!NOTE]
    > El contenido de la página principal es una tabla de API, agrupadas por el controlador. Las entradas de tabla se generan de forma dinámica, mediante la **IApiExplorer** interfaz. Si agrega o actualiza un controlador de API, la tabla se actualizarán automáticamente la próxima vez que compile la aplicación.
    > 
    > El **API** columna muestra el método HTTP y el URI relativo. El **descripción** columna contiene información que se ha extraído de la documentación del método.
12. Tenga en cuenta que la descripción que ha agregado la definición de método se muestra en la columna Descripción.

    ![Descripción del método de API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descripción del método de API")

    *Descripción del método de API*
13. Haga clic en uno de los métodos de API para navegar a una página con información más detallada, incluidos los cuerpos de respuesta de ejemplo.

    ![Página de información de detalle](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "página de información detallada")

    *Página de información detallada*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Cree una nueva aplicación Web con una experiencia de ASP.NET en Visual Studio 2013
- Integrar diversas tecnologías ASP.NET en un solo proyecto
- Generar controladores MVC y vistas de las clases de modelo mediante Scaffolding de ASP.NET
- Generar controladores de API Web que usan características como la programación asincrónica y acceso a los datos a través de Entity Framework
- Generar automáticamente las páginas de Ayuda de Web API para los controladores
