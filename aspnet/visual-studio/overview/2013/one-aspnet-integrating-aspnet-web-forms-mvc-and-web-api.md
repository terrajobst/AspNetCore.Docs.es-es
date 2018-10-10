---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'Laboratorio práctico: One ASP.NET: integrar formularios Web Forms ASP.NET, MVC y API Web | Microsoft Docs'
author: rick-anderson
description: ASP.NET es un marco para crear sitios Web, aplicaciones y servicios con tecnologías especializadas como MVC, Web API y otros. Con la expansión h ASP.NET...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: c18911680b59448cd67190f71e951a3fcf3d0478
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912740"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>Laboratorio práctico: One ASP.NET: integrar formularios Web Forms ASP.NET, MVC y Web API
====================
por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](http://aka.ms/webcamps-training-kit)

> ASP.NET es un marco para crear sitios Web, aplicaciones y servicios con tecnologías especializadas como MVC, Web API y otros. Con la expansión de ASP.NET ha visto desde su creación y el expresado debe tener estas tecnologías integradas, ha habido esfuerzos recientes en trabajar hacia **One ASP.NET**.
> 
> Visual Studio 2013 presenta un nuevo sistema de proyecto unificado que le permite crear una aplicación y usar todas las tecnologías ASP.NET en un proyecto. Esta característica elimina la necesidad de elegir una tecnología al principio de un proyecto y stick con él y en su lugar, recomienda el uso de varios marcos ASP.NET dentro de un proyecto.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Crear un sitio Web basado en la **One ASP.NET** tipo de proyecto
- Usar diferentes **ASP.NET** marcos como **MVC** y **API Web** en el mismo proyecto
- Identificar los componentes principales de un **ASP.NET** aplicación
- Aproveche la **Scaffolding de ASP.NET** framework para crear automáticamente los controladores y vistas para realizar operaciones CRUD en función de las clases de modelo
- Exponer el mismo conjunto de información en formatos máquina - y legible mediante la herramienta adecuada para cada trabajo

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [Crear un nuevo proyecto de formularios Web](#Exercise1)
2. [Creación de un controlador MVC con Scaffolding](#Exercise2)
3. [Creación de un controlador de API Web mediante Scaffolding](#Exercise3)

Tiempo estimado para completar esta práctica: **60 minutos**

> [!NOTE]
> Primera vez que inicie Visual Studio, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos de este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>Ejercicio 1: Crear un nuevo proyecto de Web Forms

En este ejercicio creará un nuevo sitio de formularios Web Forms en Visual Studio 2013 usando la **One ASP.NET** unificada experiencia en el proyecto, que le permitirá integrar fácilmente los componentes de formularios Web Forms, MVC y Web API en la misma aplicación. A continuación, explorará la solución generada e identificar sus partes, y, por último, verá el sitio Web en acción.

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>Tarea 1: crear un nuevo sitio mediante una experiencia de ASP.NET

En esta tarea iniciará la creación de un nuevo sitio Web en Visual Studio según la **One ASP.NET** tipo de proyecto. **One ASP.NET** unifica todas las tecnologías de ASP.NET y le ofrece la opción de mezclar y combinar ellos según sea necesario. A continuación, reconocerá los distintos componentes de formularios Web Forms, MVC y API Web que se encuentran en paralelo dentro de la aplicación.

1. Abra **Visual Studio Express 2013 para Web** y seleccione **archivo | Nuevo proyecto...**  para iniciar una nueva solución.

    ![Crear un proyecto nuevo](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *Crear un nuevo proyecto*
2. En el **nuevo proyecto** cuadro de diálogo, seleccione **aplicación Web ASP.NET** bajo el **Visual C# | Web** pestaña y asegúrese de que **.NET Framework 4.5** está seleccionada. Denomine el proyecto *MyHybridSite*, elija un **ubicación** y haga clic en **Aceptar**.

    ![Nuevo proyecto de aplicación Web ASP.NET](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *Crear un nuevo proyecto de aplicación Web ASP.NET*
3. En el **nuevo proyecto ASP.NET** cuadro de diálogo, seleccione el **formularios Web Forms** plantilla y seleccione el **MVC** y **API Web** opciones. Además, asegúrese de que el **autenticación** opción está establecida en **cuentas de usuario individuales**. Haga clic en **Aceptar** para continuar.

    ![Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de Web API y MVC](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Crear un nuevo proyecto con la plantilla de formularios Web Forms, incluidos los componentes de Web API y MVC*
4. Ahora puede explorar la estructura de la solución generada.

    ![Exploración de la solución generada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *Exploración de la solución generada*

    1. **Cuenta:** esta carpeta contiene las páginas de formulario Web Forms para registrarse, inicie sesión en y administrar cuentas de usuario de la aplicación. Esta carpeta se agrega cuando el **cuentas de usuario individuales** está seleccionada la opción de autenticación durante la configuración de la plantilla de proyecto de formularios Web Forms.
    2. **Modelos:** esta carpeta contiene las clases que representan los datos de aplicación.
    3. **Los controladores** y **vistas**: estas carpetas son necesarias para la **ASP.NET MVC** y **ASP.NET Web API** componentes. Explorará las tecnologías MVC y Web API en los ejercicios siguientes.
    4. El **Default.aspx**, **Contact.aspx** y **About.aspx** archivos son páginas de formularios Web predefinidas que puede usar como punto de partida para compilar las páginas específicas de su aplicación. La lógica de programación de dichos archivos reside en un archivo independiente que se conoce como el &quot;código&quot; archivo, que tiene un &quot;. aspx.vb&quot; o &quot;. aspx.cs&quot; extensión (en función de la lenguaje usado). La lógica de código subyacente se ejecuta en el servidor y genera dinámicamente la salida HTML de la página.
    5. El **Site.Master** y **Site.Mobile.Master** páginas definen la apariencia y el comportamiento estándar de todas las páginas en la aplicación.
5. Haga doble clic en el **Default.aspx** archivo para explorar el contenido de la página.

    ![Exploración de la página Default.aspx](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Exploración de la página Default.aspx*

    > [!NOTE]
    > El **página** directiva en la parte superior del archivo define los atributos de la página de formularios Web Forms. Por ejemplo, el **MasterPageFile** atributo especifica la ruta de acceso al maestro de página: en este caso, el *Site.Master* página - y el **Inherits** atributo define el clase de código subyacente para la página hereda. Esta clase se encuentra en el archivo determinado por la **CodeBehind** atributo.
    > 
    > El **asp: Content** control incluye el contenido real de la página (texto, marcado y controles) y se asigna a un **asp: ContentPlaceHolder** control en la página maestra. En este caso, el contenido de la página se representará dentro de la *MainContent* control definido en el *Site.Master* página.
6. Expanda el **aplicación\_iniciar** carpeta y observe el **WebApiConfig.cs** archivo. Visual Studio incluye ese archivo en la solución generada porque incluye API Web al configurar el proyecto con la plantilla de One ASP.NET.
7. Abra el **WebApiConfig.cs** archivo. En el *WebApiConfig* clase encontrará la configuración asociada con Web API, que se asigna a HTTP que se enruta a **controladores Web API**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. Abra el **RouteConfig.cs** archivo. Dentro de la *RegisterRoutes* encontrará la configuración asociada con MVC, que asigna las rutas HTTP para el método **controladores MVC**.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea se ejecute la solución generada, explore la aplicación y algunas de sus características, como la reescritura de URL y la autenticación integrada.

1. Para ejecutar la solución, presione **F5** o haga clic en el **iniciar** situado en la barra de herramientas. Debe abrir la página de inicio de la aplicación en el explorador.

    ![Ejecución de la solución](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Compruebe que se invocan las páginas de formularios Web Forms. Para ello, anexe **/contact.aspx** a la dirección URL en la barra de direcciones y presione **ENTRAR**.

    ![Direcciones URL descriptivas](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *Direcciones URL descriptivas*

    > [!NOTE]
    > Como puede ver, la dirección URL cambia a **o póngase en contacto con**. A partir de **ASP.NET 4**, funcionalidades de enrutamiento de URL se agregaron a formularios Web Forms, por lo que puede escribir direcciones URL como *[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* en lugar de  *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. Para obtener más información, consulte [enrutamiento de direcciones URL](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md).
3. Ahora, explorará el flujo de autenticación integrado en la aplicación. Para ello, haga clic en **registrar** en la esquina superior derecha de la página.

    ![Registrar un nuevo usuario](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *Registrar un nuevo usuario*
4. En el **registrar** , escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.

    ![Página de registro](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *Página de registro*
5. La aplicación registra la nueva cuenta y el usuario está autenticado.

    ![Usuario autenticado](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *Usuario autenticado*
6. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>Ejercicio 2: Creación de un controlador MVC con Scaffolding

En este ejercicio se aprovechará el marco de ASP.NET Scaffolding proporcionado por Visual Studio para crear un controlador de ASP.NET MVC 5 con acciones y vistas de Razor para realizar operaciones de CRUD, sin necesidad de escribir una sola línea de código. El proceso de scaffolding usará Entity Framework Code First para generar el contexto de datos y el esquema de base de datos en la base de datos SQL.

**Acerca de Entity Framework Code First**

Entity Framework (EF) es un asignador relacional de objetos (ORM) que le permite crear aplicaciones de acceso a datos programando con un modelo de aplicación conceptual en lugar de programar directamente con un esquema de almacenamiento relacional.

El flujo de trabajo de modelado de Entity Framework Code First le permite usar sus propias clases de dominio para representar el modelo de EF se basa en al realizar consultas, funciones de seguimiento de cambios y actualización. Mediante el flujo de trabajo de desarrollo Code First, es necesario iniciar la aplicación mediante la creación de una base de datos o especificar un esquema. En su lugar, puede escribir clases de .NET estándar que definen los objetos de modelo de dominio más adecuados para su aplicación, y Entity Framework creará la base de datos por usted.

> [!NOTE]
> Puede aprender más acerca de Entity Framework [aquí](../../../entity-framework.md).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>Tarea 1: crear un nuevo modelo

Ahora va a definir un **persona** (clase), que será el modelo utilizado por el proceso de scaffolding para crear el controlador MVC y las vistas. Primero debe crear un **persona** clase de modelo y las operaciones CRUD en el controlador se creará automáticamente con las características de scaffolding.

1. Abrir **Visual Studio Express 2013 para Web** y **MyHybridSite.sln** solución se encuentra en la **Begin/origen/Ex2-MvcScaffolding** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.
2. En **el Explorador de soluciones**, haga clic en el **modelos** carpeta de la **MyHybridSite** del proyecto y seleccione **Add | Clase...** .

    ![Adición de la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Adición de la clase de modelo de persona*
3. En el **Agregar nuevo elemento** cuadro de diálogo, el nombre del archivo *Person.cs* y haga clic en **agregar**.

    ![Creación de la clase de modelo de persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Creación de la clase de modelo de persona*
4. Reemplace el contenido de la **Person.cs** archivo con el código siguiente. Presione **CTRL + S** para guardar los cambios.

    (Código de fragmento de código - *PersonClass BringingTogetherOneAspNet - Ex2 -*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. En **el Explorador de soluciones**, haga clic en el **MyHybridSite** del proyecto y seleccione **compilar**, o bien presione **CTRL + MAYÚS + B** para compilar el proyecto.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>Tarea 2: creación de un controlador MVC

Ahora que la **persona** se crea un modelo, usará el scaffolding de ASP.NET MVC con Entity Framework para crear las acciones de controlador CRUD y vistas para **persona**.

1. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** del proyecto y seleccione **Add | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *Crear un nuevo controlador de scaffolding*
2. En el **agregar Scaffold** cuadro de diálogo, seleccione **controlador MVC 5 con vistas, usando Entity Framework** y, a continuación, haga clic en **agregar.**

    ![Seleccionar controlador MVC 5 con vistas y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *Seleccionar controlador MVC 5 con vistas y Entity Framework*
3. Establecer *MvcPersonController* como el **nombre del controlador**, seleccione el **usar acciones de controlador asincrónicas** opción y seleccione **persona (MyHybridSite.Models)**  como el **clase modelo**.

    ![Agregar un controlador MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *Agregar un controlador MVC con scaffolding*
4. En **clase de contexto de datos**, haga clic en **nuevo contexto de datos...** .

    ![Crear un nuevo contexto de datos](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *Crear un nuevo contexto de datos*
5. En el **nuevo contexto de datos** nombre nuevo contexto de datos de cuadro de diálogo *PersonContext* y haga clic en **agregar**.

    ![Crear el nuevo PersonContext](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *Crear el nuevo tipo PersonContext*
6. Haga clic en **agregar** para crear el nuevo controlador para **persona** con la técnica scaffolding. Visual Studio, a continuación, generará las acciones de controlador, el contexto de datos de la persona y las vistas de Razor.

    ![Después de crear el controlador de MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *Después de crear el controlador de MVC con scaffolding*
7. Abra el **MvcPersonController.cs** de archivos en el **controladores** carpeta. Tenga en cuenta que los métodos de acción CRUD se han generado automáticamente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > Si selecciona el **usar acciones de controlador asincrónicas** opciones de casilla de verificación de la técnica scaffolding en los pasos anteriores, Visual Studio genera los métodos de acción asincrónicos para todas las acciones que implican el acceso en el contexto de datos de la persona. Se recomienda que use métodos de acción asincrónicos de ejecución prolongada, relacionadas con la CPU no las solicitudes para evitar el bloqueo en el servidor Web de realizar el trabajo mientras se procesa la solicitud.

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea, se ejecutará la solución para comprobar que las vistas de **persona** funcionan del modo esperado. Agregará una nueva persona para comprobar que se guardó correctamente para la base de datos.

1. Presione **F5** para ejecutar la solución.
2. Vaya a **/MvcPerson**. Debería aparecer la vista con scaffold que muestra la lista de personas.
3. Haga clic en **crear nuevo** para agregar una nueva persona.

    ![Vaya a las vistas MVC con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *Vaya a las vistas MVC con scaffolding*
4. En el **crear** ver, proporcione un **nombre** y un **Age** la persona y haga clic en **crear**.

    ![Agregar una nueva persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *Agregar una nueva persona*
5. La persona nueva se agrega a la lista. En la lista de elementos, haga clic en **detalles** para mostrar la vista de detalles de la persona. A continuación, en el **detalles** ver, haga clic en **volver a la lista** para volver a la vista de lista.

    ![Vista de detalles de la persona.](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *Vista de detalles de la persona.*
6. Haga clic en el **eliminar** vínculo Eliminar de la persona. En el **eliminar** ver, haga clic en **eliminar** para confirmar la operación.

    ![Eliminación de una persona](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *Eliminación de una persona*
7. Vuelva a Visual Studio y presione **MAYÚS + F5** para detener la depuración.

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>Ejercicio 3: Crear un controlador de API Web mediante Scaffolding

El marco API Web es parte de la pila de ASP.NET y diseñado para facilitar la implementación servicios HTTP, por lo general, enviar y recibir datos con formato JSON o XML a través de una API de RESTful.

En este ejercicio, se usará Scaffolding de ASP.NET nuevo para generar un controlador Web API. Usará el mismo **persona** y **PersonContext** clases desde el ejercicio anterior para proporcionar los mismos datos de person en formato JSON. Verá cómo puede exponer los mismos recursos de diferentes maneras en la misma aplicación de ASP.NET.

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>Tarea 1: creación de un controlador de API Web

En esta tarea creará una nueva **controlador Web API** que va a exponer los datos de la persona en un formato de equipo puede consumir como JSON.

1. Si aún no está abierto, abra **Visual Studio Express 2013 para Web** y abra el **MyHybridSite.sln** solución se encuentra en la **Begin/origen/Ex3-WebAPI** carpeta. Como alternativa, puede continuar con la solución que obtuvo en el ejercicio anterior.

    > [!NOTE]
    > Si empieza con la solución de inicio del ejercicio 3, presione **CTRL + MAYÚS + B** para compilar la solución.
2. En **el Explorador de soluciones**, haga clic en el **controladores** carpeta de la **MyHybridSite** del proyecto y seleccione **Add | Nuevo elemento con scaffolding...** .

    ![Crear un nuevo controlador con scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *Crear un nuevo controlador con scaffolding*
3. En el **agregar Scaffold** cuadro de diálogo, seleccione **API Web** en el panel izquierdo, a continuación, **controlador Web API 2 con acciones mediante Entity Framework** en el panel central y, a continuación, haga clic en  **Agregar.**

    ![Seleccionar controlador Web API 2 con acciones y Entity Framework](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "seleccionar controlador de Web API 2 con acciones y Entity Framework")

    *Seleccionar controlador Web API 2 con acciones y Entity Framework*
4. Establecer *ApiPersonController* como el **nombre del controlador**, seleccione el **usar acciones de controlador asincrónicas** opción y seleccione **persona (MyHybridSite.Models)**  y **PersonContext (MyHybridSite.Models)** como el **modelo** y **contexto de datos** clases respectivamente. A continuación, haga clic en **Agregar**.

    ![Agregar un controlador de API Web con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "agregando un controlador de API Web con la técnica scaffolding")

    *Agregar un controlador Web API con la técnica scaffolding*
5. Visual Studio, a continuación, se generará el **ApiPersonController** clase con las cuatro acciones CRUD para trabajar con los datos.

    ![Después de crear el controlador de Web API con la técnica scaffolding](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "después de crear el controlador de Web API con la técnica scaffolding")

    *Después de crear el controlador de Web API con la técnica scaffolding*
6. Abra el **ApiPersonController.cs** archivo e inspeccione el *GetPeople* método de acción. Este método consulta el campo de base de datos de **PersonContext** tipo con el fin de obtener datos de las personas.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. Ahora observe el comentario anterior sobre la definición del método. Proporciona el URI que expone esta acción que se va a usar en la siguiente tarea.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > De forma predeterminada, la API Web está configurada para detectar las consultas para el */API* ruta de acceso para evitar colisiones con los controladores MVC. Si necesita cambiar esta configuración, consulte [Routing in ASP.NET Web API](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md).

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>Tarea 2: ejecución de la solución

En esta tarea usará el Explorador de Internet **herramientas de desarrollo F12** para inspeccionar la respuesta completa desde el controlador de API Web. Verá cómo puede capturar el tráfico de red para obtener más información sobre los datos de aplicación.

> [!NOTE]
> Asegúrese de que **Internet Explorer** está seleccionado en el **iniciar** situado en la barra de herramientas de Visual Studio.
> 
> ![Opción de Internet Explorer](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> El **herramientas de desarrollo F12** tiene un amplio conjunto de funcionalidad que no se trata en este laboratorio práctico. Si desea obtener más información, consulte [mediante herramientas de desarrollo F12](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85)).


1. Presione **F5** para ejecutar la solución.

    > [!NOTE]
    > Para seguir esta tarea correctamente, la aplicación necesita que los datos. Si la base de datos está vacío, puede volver a la tarea 3 en el ejercicio 2 y siga los pasos sobre cómo crear a una nueva persona mediante las vistas MVC.
2. En el explorador, presione **F12** para abrir el **Developer Tools** panel. Presione **CTRL** + **4** o haga clic en el **red** icono y, a continuación, haga clic en botón de la flecha verde para empezar a capturar el tráfico de red.

    ![Iniciando la captura de red de la API Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "captura de red iniciando la API Web")

    *Iniciando la captura de red de la API Web*
3. Anexar **api/ApiPerson** a la dirección URL en la barra de direcciones del explorador. Ahora inspeccionará los detalles de la respuesta de la **ApiPersonController**.

    ![Recuperar datos de la persona a través de Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "recuperar datos de la persona a través de Web API")

    *Recuperar datos de la persona a través de Web API*

    > [!NOTE]
    > Una vez finalizada la descarga, se le pedirá que realice una acción con el archivo descargado. Deje el cuadro de diálogo Abrir con el fin de poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.
4. Ahora inspeccionará el cuerpo de la respuesta. Para ello, haga clic en el **detalles** pestaña y, a continuación, haga clic en **cuerpo de respuesta**. Se puede comprobar que los datos descargados están una lista de objetos con las propiedades **Id**, **nombre** y **Age** que corresponden a la **persona** clase.

    ![Visualización del cuerpo de respuesta de la API de Web](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "ver cuerpo de respuesta de la API de Web")

    *Cuerpo de respuesta de la API Web de visualización*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>Tarea 3: adición de páginas de Ayuda de la API Web

Cuando se crea una API Web, es útil crear una página de ayuda para que otros desarrolladores sepan cómo llamar a la API. Puede crear y actualizar manualmente las páginas de documentación, pero es mejor para evitar tener que realizar trabajo de mantenimiento genere automáticamente. En esta tarea usará un paquete Nuget para generar automáticamente las páginas de Ayuda de API Web a la solución.

1. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de NuGet**y, a continuación, haga clic en **Package Manager Console**.
2. En el **Package Manager Console** ventana, ejecute el siguiente comando:

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > El **Microsoft.AspNet.WebApi.HelpPage** paquete instala los ensamblados necesarios y agrega las vistas de MVC para las páginas de ayuda en la **áreas/HelpPage** carpeta.

    ![Área HelpPage](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage área")

    *Área HelpPage*
3. De forma predeterminada, la Ayuda de las páginas tienen cadenas de marcador de posición para la documentación. Puede usar los comentarios de documentación XML para crear la documentación. Para habilitar esta característica, abra el **HelpPageConfig.cs** archivo se encuentra en la **áreas/aplicación/HelpPage\_iniciar** carpeta y quite el comentario de la línea siguiente:

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. En **el Explorador de soluciones**, haga clic en el proyecto **MyHybridSite**, seleccione **propiedades** y haga clic en el **compilar** ficha.

    ![Pestaña compilar](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "sección compilación")

    *Pestaña de la compilación*
5. En **salida**, seleccione **archivo de documentación XML**. En el cuadro de edición, escriba **aplicación\_Data/XmlDocument.xml**.

    ![Salida de la sección en la pestaña compilación](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "sección en la pestaña compilación de salida")

    *Sección de salida en la pestaña compilación*
6. Presione **CTRL** + **S** para guardar los cambios.
7. Abra el **ApiPersonController.cs** de archivos desde el **controladores** carpeta.
8. Escriba una nueva línea entre el *GetPeople* firma del método y el */ / obtener api/ApiPerson* comentar y, a continuación, escriba tres barras diagonales.

    > [!NOTE]
    > Visual Studio inserta automáticamente los elementos XML que definen la documentación del método.
9. Agregar un texto de resumen y el valor devuelto para la *GetPeople* método. Debería ser similar al siguiente.

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. Presione **F5** para ejecutar la solución.
11. Anexar **/ayuda** a la dirección URL en la barra de direcciones para ir a la página de ayuda.

    ![Página de Ayuda de ASP.NET Web API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "página de Ayuda de ASP.NET Web API")

    *Página de Ayuda de ASP.NET Web API*

    > [!NOTE]
    > El contenido de la página principal es una tabla de API, agrupadas por controlador. Las entradas de tabla se generan de forma dinámica, mediante el **IApiExplorer** interfaz. Si agrega o actualiza un controlador de API, la tabla se actualizará automáticamente la próxima vez que compile la aplicación.
    > 
    > El **API** columna muestra el método HTTP y el URI relativo. El **descripción** columna contiene información que se ha extraído de la documentación del método.
12. Tenga en cuenta que la descripción que ha agregado la definición del método se muestra en la columna Descripción.

    ![Descripción del método API](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "descripción del método de API")

    *Descripción del método de API*
13. Haga clic en uno de los métodos de API para navegar a una página con información más detallada, incluyendo cuerpos de respuesta de ejemplo.

    ![Página de información detallada](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "página de información de detalle")

    *Página de información detallada*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Al completar este laboratorio práctico ha aprendido cómo:

- Cree una nueva aplicación Web con una experiencia de ASP.NET en Visual Studio 2013
- Integrar varias tecnologías ASP.NET en un proyecto único
- Generar controladores y vistas MVC desde las clases de modelo mediante Scaffolding de ASP.NET
- Generar controladores de API Web que usan características como la programación asincrónica y el acceso a los datos a través de Entity Framework
- Generar automáticamente las páginas de Ayuda de Web API para que los controladores
