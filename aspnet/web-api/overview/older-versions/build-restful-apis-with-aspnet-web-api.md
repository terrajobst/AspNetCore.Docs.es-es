---
uid: web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
title: Compilar API RESTful con ASP.NET Web API | Microsoft Docs
author: rick-anderson
description: En los últimos años, se ha convertido en claro que HTTP no es solo para servir como páginas HTML. También es una plataforma eficaz para la creación de API de Web, con un puñado de o...
ms.author: aspnetcontent
ms.date: 02/18/2013
ms.assetid: 87daa99f-3810-407e-b969-dd28a192959d
msc.legacyurl: /web-api/overview/older-versions/build-restful-apis-with-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: 9866b5f75771c633165587ba04e694f72a1e626c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835604"
---
<a name="build-restful-apis-with-aspnet-web-api"></a>Compilar API RESTful con ASP.NET Web API
====================
por [campamentos Web Team](https://twitter.com/webcamps)

> En los últimos años, se ha convertido en claro que HTTP no es solo para servir como páginas HTML. También es una plataforma eficaz para crear las API Web, con unos cuantos verbos (GET, POST etc.) además de algunos conceptos simples como *URI* y *encabezados*. ASP.NET Web API es un conjunto de componentes que simplifican la programación de HTTP. Dado que se basa en el tiempo de ejecución de ASP.NET MVC, API Web controla automáticamente los detalles de bajo nivel de transporte de HTTP. Al mismo tiempo, API Web expone naturalmente el modelo de programación HTTP. De hecho, es uno de los objetivos de la API Web *no* abstraer la realidad de HTTP. Como resultado, la API Web es flexible y fácil de extender. En este laboratorio práctico, usará la API Web para crear una sencilla API de REST para una aplicación de administrador de contactos. También creará un cliente para usar la API. El estilo arquitectónico REST ha demostrado para ser una forma eficaz de aprovechar HTTP - aunque ciertamente no es el enfoque sólo es válido para HTTP. El Administrador de contactos expondrá el RESTful para enumerar, agregar y quitar contactos, entre otros. Esta práctica requiere una comprensión básica de HTTP, REST y se supone que tiene conocimientos prácticos básicos de HTML, JavaScript y jQuery.
> 
> > [!NOTE]
> > El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ https://asp.net/web-api ](https://asp.net/web-api). Este sitio seguirá proporcionando información más reciente, ejemplos y noticias relacionadas con la API Web, así que comprobación con frecuencia si gustaría profundizar más en el arte de la creación de API Web personalizadas disponibles para prácticamente cualquier marco de desarrollo o de dispositivo.
> > 
> > ASP.NET Web API, similar a ASP.NET MVC 4, tiene gran flexibilidad en cuanto a la separación de la capa de servicio desde los controladores que le permite usar algunos de los marcos de trabajo disponibles de inserción de dependencias bastante fácil. Hay un buen ejemplo de MSDN que se muestra cómo usar Ninject para inserción de dependencias en un proyecto de ASP.NET Web API que puede descargarse desde [aquí](https://code.msdn.microsoft.com/ASPNET-Web-API-JavaScript-d0d64dd7).
> 
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Implementar una API Web RESTful
- Llamar a la API desde un cliente HTML

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice B](#AppendixB) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice A: Using Code Snippets](#AppendixA)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye en el siguiente ejercicio:

1. [Ejercicio 1: Crear una API de Web de solo lectura](#Exercise1)
2. [Ejercicio 2: Crear una API Web de lectura/escritura](#Exercise2)
3. [Ejercicio 3: Usar la API Web desde un cliente HTML](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Create_a_Read-Only_Web_API"></a>
### <a name="exercise-1-create-a-read-only-web-api"></a>Ejercicio 1: Crear una API de Web de solo lectura

En este ejercicio, implementará los métodos GET de solo lectura para el Administrador de contactos.

<a id="Ex1Task1"></a>

<a id="Task_1_-_Creating_the_API_Project"></a>
#### <a name="task-1---creating-the-api-project"></a>Tarea 1: crear el proyecto de API

En esta tarea, utilizará las nuevas plantillas de proyecto web ASP.NET para crear una aplicación web de API Web.

1. Ejecute **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.
2. Desde el **archivo** menú, seleccione **nuevo proyecto**. Seleccione el **Visual C# | Web** tipo desde la vista de árbol del tipo de proyecto del proyecto y, después, seleccione el **aplicación Web de ASP.NET MVC 4** tipo de proyecto. Establezca el proyecto **nombre** a *ContactManager* y el **nombre de la solución** a *comenzar*, a continuación, haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web](build-restful-apis-with-aspnet-web-api/_static/image1.png "crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web")

    *Crear un nuevo proyecto de aplicación de ASP.NET MVC 4.0 Web*
3. En el cuadro de diálogo de tipo de proyecto de ASP.NET MVC 4, seleccione el **API Web** tipo de proyecto. Haga clic en **Aceptar**.

    ![Especifica el tipo de proyecto Web API](build-restful-apis-with-aspnet-web-api/_static/image2.png "especifica el tipo de proyecto Web API")

    *Especifica el tipo de proyecto Web API*

<a id="Ex1Task2"></a>

<a id="Task_2_-_Creating_the_Contact_Manager_API_Controllers"></a>
#### <a name="task-2---creating-the-contact-manager-api-controllers"></a>Tarea 2: creación de los controladores de API de Contact Manager

En esta tarea, creará las clases del controlador en el que van a residir los métodos de API.

1. Eliminar el archivo denominado **ValuesController.cs** en **controladores** carpeta del proyecto.
2. Haga clic en el **controladores** carpeta en el proyecto y seleccione **Add | Controlador** en el menú contextual.

    ![Agregar un nuevo controlador para el proyecto](build-restful-apis-with-aspnet-web-api/_static/image3.png "agregando un nuevo controlador al proyecto")

    *Agregar un nuevo controlador al proyecto*
3. En el **Agregar controlador** cuadro de diálogo que aparece, seleccione **controlador de API en blanco** en el menú de la plantilla. Nombre de la clase de controlador **ContactController**. A continuación, haga clic en **agregar.**

    ![Mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web](build-restful-apis-with-aspnet-web-api/_static/image4.png "mediante el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web")

    *Usar el cuadro de diálogo Agregar controlador para crear un nuevo controlador de API Web*
4. Agregue el código siguiente a la **ContactController**.

    (Código de fragmento de código - *Web API laboratorio - Ex01 - Get (método) API*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample1.cs)]
5. Presione **F5** para depurar la aplicación. Debería aparecer la página principal predeterminada para un proyecto Web API.

    ![La página principal predeterminada de una aplicación de ASP.NET Web API](build-restful-apis-with-aspnet-web-api/_static/image5.png "la página principal predeterminada de una aplicación de ASP.NET Web API")

    *La página principal predeterminada de una aplicación de ASP.NET Web API*
6. En la ventana de Internet Explorer, presione el **F12** tecla para abrir el **Developer Tools** ventana. Haga clic en el **red** pestaña y, a continuación, haga clic en el **Iniciar captura** botón para empezar a capturar el tráfico de red en la ventana.

    ![Abre la pestaña red e iniciando captura de red](build-restful-apis-with-aspnet-web-api/_static/image6.png "abre la pestaña red e iniciando captura de red")

    *Abre la pestaña red e iniciar la captura de red*
7. Anexe la dirección URL en la barra de direcciones del explorador con **/api/contact** y presione ENTRAR. Los detalles de la transmisión aparecerán en la ventana de captura de red. Tenga en cuenta que el tipo MIME de la respuesta es **application/json**. Esto demuestra cómo el formato de salida predeterminado es JSON.

    ![Ver la salida de la solicitud de API Web en la vista de red](build-restful-apis-with-aspnet-web-api/_static/image7.png "viendo la salida de la solicitud de API Web en la vista de red")

    *Ver la salida de la solicitud de API Web en la vista de red*

    > [!NOTE]
    > Configuración predeterminada de Internet Explorer 10 en este momento será preguntar si el usuario desea guardar o abrir la secuencia resultante de la llamada de API Web. El resultado será un archivo de texto que contiene el resultado JSON de la llamada a la dirección URL de Web API. No cancele el cuadro de diálogo para poder ver el contenido de la respuesta a través de la ventana de herramientas de desarrolladores.
8. Haga clic en el **vaya a la vista detallada** botón para ver más detalles sobre la respuesta de la llamada API.

    ![Cambie a la vista detallada](build-restful-apis-with-aspnet-web-api/_static/image8.png "cambiar a vista de detalles")

    *Cambie a la vista detallada*
9. Haga clic en el **cuerpo de respuesta** pestaña para ver el texto de respuesta JSON real.

    ![Ver el JSON de salida de texto en el monitor de red](build-restful-apis-with-aspnet-web-api/_static/image9.png "ver el JSON de salida de texto en el monitor de red")

    *Ver el texto de salida JSON en el monitor de red*

<a id="Ex1Task3"></a>

<a id="Task_3_-_Creating_the_Contact_Models_and_Augment_the_Contact_Controller"></a>
#### <a name="task-3---creating-the-contact-models-and-augment-the-contact-controller"></a>Tarea 3: creación de los modelos de contacto y aumentar el controlador de contacto

En esta tarea, creará las clases del controlador en el que van a residir los métodos de API.

1. Haga clic en el **modelos** carpeta y seleccione **Add | Clase...**  en el menú contextual.

    ![Agregar un modelo nuevo a la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image10.png "adición de un modelo nuevo a la aplicación web")

    *Agregar un modelo nuevo a la aplicación web*
2. En el **Agregar nuevo elemento** cuadro de diálogo, el nombre del nuevo archivo **Contact.cs** y haga clic en **agregar.**

    ![Crear el nuevo archivo de clase de contacto](build-restful-apis-with-aspnet-web-api/_static/image11.png "crear el nuevo archivo de clase de contacto")

    *Crear el nuevo archivo de clase de contacto*
3. Agregue el código resaltado siguiente a la **póngase en contacto con** clase.

    (Código de fragmento de código - *clase póngase en contacto con el laboratorio de API - Ex01 - Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample2.cs)]
4. En el **ContactController** de clases, seleccione la palabra **cadena** en la definición de método de la **obtener** método y escriba la palabra *póngase en contacto con*. Una vez se ha escrito la palabra, aparecerá un indicador al principio de la palabra **póngase en contacto con**. Ya sea mantenga presionada la tecla el **Ctrl** clave y presione la tecla de punto (.) o haga clic en el icono con el mouse para abrir el cuadro de diálogo de asistencia en el editor de código para rellenar automáticamente el **mediante** la directiva para los modelos espacio de nombres.

    ![Mediante la asistencia de Intellisense para las declaraciones de espacio de nombres](build-restful-apis-with-aspnet-web-api/_static/image12.png)

    *Mediante la asistencia de Intellisense para las declaraciones de espacio de nombres*
5. Modifique el código para el **obtener** método para que devuelva una matriz de instancias de modelo de contacto.

    (Código de fragmento de código - *devolver una lista de contactos de laboratorio de API Web - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample3.cs)]
6. Presione **F5** para depurar la aplicación web en el explorador. Para ver los cambios realizados en la salida de respuesta de la API, realice los pasos siguientes.

   1. Una vez que el explorador se abre, presione **F12** si las herramientas de desarrollo no están abiertas todavía.
   2. Haga clic en el **red** ficha.
   3. Presione el **Iniciar captura** botón.
   4. Agregue el sufijo de dirección URL **/api/contact** a la dirección URL en la barra de direcciones y presione la **ENTRAR** clave.
   5. Presione el **vaya a la vista detallada** botón.
   6. Seleccione el **cuerpo de respuesta** ficha. Debería ver una cadena JSON que representa el formulario serializado de una matriz de instancias de contacto.

      ![Salida de una llamada al método de Web API compleja serializados de JSON](build-restful-apis-with-aspnet-web-api/_static/image13.png "salida de una llamada al método de Web API compleja serializados de JSON")

      *Salida de una llamada al método de Web API compleja serializados de JSON*

<a id="Ex1Task4"></a>

<a id="Task_4_-_Extracting_Functionality_into_a_Service_Layer"></a>
#### <a name="task-4---extracting-functionality-into-a-service-layer"></a>Tarea 4: funcionalidad de extracción en una capa de servicio

Esta tarea mostrará cómo extraer la funcionalidad en una capa de servicio para que sea fácil para los desarrolladores separar su funcionalidad de servicio de la capa de controlador, lo que permite la reutilización de los servicios que realmente realizan el trabajo.

1. Cree una nueva carpeta en la raíz de la solución y asígnele el nombre **servicios**. Para ello, haga clic en **ContactManager** proyecto, seleccione **agregar** | **nueva carpeta**, asígnele el nombre *servicios*.

    ![Creando carpeta de servicios](build-restful-apis-with-aspnet-web-api/_static/image14.png "carpeta crear servicios")

    *Creando la carpeta de servicios*
2. Haga clic en el **servicios** carpeta y seleccione **Add | Clase...**  en el menú contextual.

    ![Agregar una nueva clase a la carpeta servicios](build-restful-apis-with-aspnet-web-api/_static/image15.png "agregando una nueva clase a la carpeta de servicios")

    *Agregar una nueva clase a la carpeta de servicios*
3. Cuando el **Agregar nuevo elemento** aparece el cuadro de diálogo, el nombre de la nueva clase **ContactRepository** y haga clic en **agregar**.

    ![Creación de un archivo de clase para que contenga el código de la capa de repositorio póngase en contacto con servicio](build-restful-apis-with-aspnet-web-api/_static/image16.png "creación de un archivo de clase para que contenga el código de la capa de servicio del repositorio de contacto")

    *Creación de un archivo de clase para que contenga el código de la capa de servicio del repositorio de contacto*
4. Agregue una mediante la directiva a la **ContactRepository.cs** archivo para incluir el espacio de nombres de los modelos.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample4.cs)]
5. Agregue el código resaltado siguiente a la **ContactRepository.cs** archivo para implementar el método GetAllContacts.

    (Código de fragmento de código - *Web de repositorio de contactos de laboratorio de API - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample5.cs)]
6. Abra el **ContactController.cs** archivo si aún no está abierto.
7. Agregue lo siguiente mediante la instrucción a la sección de declaración de espacio de nombres del archivo.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample6.cs)]
8. Agregue el código resaltado siguiente a la **ContactController.cs** clase para agregar un campo privado para representar la instancia del repositorio, por lo que usar el resto de la clase pueden hacer los miembros de la implementación del servicio.

    (Código de fragmento de código - *API laboratorio - Ex01 - contacto controlador Web*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample7.cs)]
9. Cambiar el **obtener** método por lo que ofrece utilizar el servicio del repositorio de contactos.

    (Código de fragmento de código - *devolver una lista de contactos a través del repositorio de laboratorio de API Web - Ex01 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample8.cs)]
10. Coloque un punto de interrupción en el **ContactController**del **obtener** definición de método.

   ![Agregar puntos de interrupción en el controlador de contacto](build-restful-apis-with-aspnet-web-api/_static/image17.png "agregar puntos de interrupción en el controlador de contacto")

   *Agregar puntos de interrupción en el controlador de contacto*
11. Presione **F5** para ejecutar la aplicación.
12. Cuando el explorador se abre, presione **F12** para abrir las herramientas de desarrollo.
13. Haga clic en el **red** ficha.
14. Haga clic en el **Iniciar captura** botón.
15. Anexe la dirección URL en la barra de direcciones con el sufijo **/api/contact** y presione **ENTRAR** para cargar el controlador de API.
16. Visual Studio 2012 debe interrumpir una vez **obtener** método comienza la ejecución.

   ![Importantes dentro del método Get](build-restful-apis-with-aspnet-web-api/_static/image18.png "importantes dentro del método Get")

   *Última hora dentro del método Get*
17. Presione **F5** para continuar.
18. Vuelva a Internet Explorer si aún no está en el foco. Tenga en cuenta la ventana de captura de red.

    ![Vista en Internet Explorer que muestra los resultados de la llamada de API Web de red](build-restful-apis-with-aspnet-web-api/_static/image19.png "vista en Internet Explorer que muestra los resultados de la llamada de API Web de red")

    *Vista de red en Internet Explorer que muestra los resultados de la llamada de API Web*
19. Haga clic en el **vaya a la vista detallada** botón.
20. Haga clic en el **cuerpo de respuesta** ficha. Tenga en cuenta la salida JSON de la llamada de API y cómo representa los dos contactos recuperados por la capa de servicio.

    ![Ver la salida JSON desde la API Web en la ventana de herramientas de desarrollador](build-restful-apis-with-aspnet-web-api/_static/image20.png "ver la salida JSON desde la API Web en la ventana de herramientas para desarrolladores")

    *Ver la salida JSON desde la API Web en la ventana de herramientas para desarrolladores*

<a id="Exercise2"></a>

<a id="Exercise_2_Create_a_ReadWrite_Web_API"></a>
### <a name="exercise-2-create-a-readwrite-web-api"></a>Ejercicio 2: Crear una API Web de lectura/escritura

En este ejercicio, implementará POST y PUT métodos para el Administrador de contactos habilitarlo con características de edición de datos.

<a id="Ex2Task1"></a>

<a id="Task_1_-_Opening_the_Web_API_Project"></a>
#### <a name="task-1---opening-the-web-api-project"></a>Tarea 1: abrir el proyecto de API Web

En esta tarea, preparará mejorar el proyecto Web API que creó en el ejercicio 1 para que pueda aceptar la entrada del usuario.

1. Ejecute **Visual Studio 2012 Express para Web**, para ello, vaya a **iniciar** y tipo **VS Express para Web** , a continuación, presione **ENTRAR**.
2. Abra el **comenzar** solución ubicado en **origen/Ex02-ReadWriteWebAPI/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra el **Services/ContactRepository.cs** archivo.

<a id="Ex2Task2"></a>

<a id="Task_2_-_Adding_Data-Persistence_Features_to_the_Contact_Repository_Implementation"></a>
#### <a name="task-2---adding-data-persistence-features-to-the-contact-repository-implementation"></a>Tarea 2: agregar características de persistencia de datos para la implementación de repositorio de contactos

En esta tarea, se aumentan la clase ContactRepository del proyecto de API Web creado en el ejercicio 1 para que pueda conservar y aceptar entrada del usuario y nuevas instancias de contacto.

1. Agregue la siguiente constante a la **ContactRepository** clase para representar el nombre de la memoria caché elemento clave nombre del servidor web más adelante en este ejercicio.

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample9.cs)]
2. Agregue un constructor a la **ContactRepository** que contiene el código siguiente.

    (Código de fragmento de código - *Web Constructor del repositorio de contactos de laboratorio API - Ex02 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample10.cs)]
3. Modifique el código para el **GetAllContacts** método tal y como se muestra a continuación.

    (Código de fragmento de código - *Web API laboratorio - Ex02 - obtener todos los contactos*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample11.cs)]

    > [!NOTE]
    > En este ejemplo es para fines de demostración y usa memoria caché del servidor web como un medio de almacenamiento, para que los valores se estén disponibles para varios clientes simultáneamente, en lugar de usar un mecanismo de almacenamiento de sesión o una duración de almacenamiento de la solicitud. Se podría usar Entity Framework, almacenamiento de XML o cualquier otra variedad en lugar de la caché del servidor web.
4. Implementar un nuevo método denominado **SaveContact** a la **ContactRepository** clase para realizar el trabajo del proceso de guardar un contacto. El **SaveContact** método debe tomar un único **póngase en contacto con** valor de parámetro y devuelva un valor booleano que indica éxito o error.

    (Código de fragmento de código - *Web API laboratorio - Ex02 - implementación del método SaveContact*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample12.cs)]

<a id="Exercise3"></a>

<a id="Exercise_3_Consume_the_Web_API_from_an_HTML_Client"></a>
### <a name="exercise-3-consume-the-web-api-from-an-html-client"></a>Ejercicio 3: Usar la API Web desde un cliente HTML

En este ejercicio, creará un cliente HTML para llamar a la API Web. Este cliente facilitará el intercambio de datos con la API Web con JavaScript y mostrará los resultados en un explorador web mediante el formato HTML.

<a id="Ex3Task1"></a>

<a id="Task_1_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Displaying_Contacts"></a>
#### <a name="task-1---modifying-the-index-view-to-provide-a-gui-for-displaying-contacts"></a>Tarea 1: modificación de la vista de índice para proporcionar una interfaz gráfica de usuario para mostrar contactos

En esta tarea, modificará la vista de índice predeterminada de la aplicación web para admitir el requisito de mostrar la lista de contactos existentes en un explorador HTML.

1. Abra **Visual Studio 2012 Express para Web** si aún no está abierto.
2. Abra el **comenzar** solución ubicado en **origen/Ex03-ConsumingWebAPI/inicio/** carpeta. En caso contrario, es posible que siga usando la **final** solución obtenido completando el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
3. Abra el **Index.cshtml** ubicado en el archivo **vistas/inicio** carpeta.
4. Reemplace el código HTML dentro del elemento div con el identificador **cuerpo** para que quede como el código siguiente.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample13.html)]
5. Agregue el siguiente código Javascript en la parte inferior del archivo para realizar la solicitud HTTP a la API Web.

    [!code-cshtml[Main](build-restful-apis-with-aspnet-web-api/samples/sample14.cshtml)]
6. Abra el **ContactController.cs** archivo si aún no está abierto.
7. Coloque un punto de interrupción en el **obtener** método de la **ContactController** clase.

    ![Al colocar un punto de interrupción en el método Get del controlador API](build-restful-apis-with-aspnet-web-api/_static/image21.png "colocando un punto de interrupción en el método Get del controlador de API")

    *Al colocar un punto de interrupción en el método Get del controlador de API*
8. Presione **F5** para ejecutar el proyecto. El explorador cargará el documento HTML.

    > [!NOTE]
    > Asegúrese de que va a examinar la dirección URL raíz de la aplicación.
9. Una vez que la página se carga y ejecuta el código JavaScript, se alcanzará el punto de interrupción y se pausará la ejecución del código en el controlador.

    ![Depuración en las llamadas de API Web con VS Express para Web](build-restful-apis-with-aspnet-web-api/_static/image22.png "depuración en las llamadas de API Web con VS Express para Web")

    *Depuración en la llamada de API Web mediante Visual Studio 2012 Express para Web*
10. Quitar el punto de interrupción y presione **F5** o la barra de herramientas depuración **continuar** botón para continuar cargando la vista en el explorador. Una vez que se complete la llamada de API Web debería ver los contactos devueltos por la API Web llame a mostrados como elementos de lista en el explorador.

    ![Resultados de la llamada de API que se muestra en el explorador como elementos de lista](build-restful-apis-with-aspnet-web-api/_static/image23.png "los resultados de la llamada de API que se muestra en el explorador como elementos de lista")

    *Resultados de la llamada de API que se muestra en el explorador como elementos de lista*
11. Detenga la depuración.

<a id="Ex3Task2"></a>

<a id="Task_2_-_Modifying_the_Index_View_to_Provide_a_GUI_for_Creating_Contacts"></a>
#### <a name="task-2---modifying-the-index-view-to-provide-a-gui-for-creating-contacts"></a>Tarea 2: modificar la vista de índice para proporcionar una interfaz gráfica de usuario para la creación de contactos

En esta tarea, continuará modificar la vista de índice de la aplicación MVC. Se agregará un formulario a la página HTML que capturará la entrada del usuario y enviarlo a la API Web para crear un nuevo contacto, y se creará un nuevo método de controlador Web API para recopilar la fecha de la interfaz gráfica de usuario.

1. Abra el **ContactController.cs** archivo.
2. Agregue un nuevo método a la clase de controlador denominada **Post** tal como se muestra en el código siguiente.

    (Código de fragmento de código - *Web método Post de laboratorio de API - Ex03 -*)

    [!code-csharp[Main](build-restful-apis-with-aspnet-web-api/samples/sample15.cs)]
3. Abra el **Index.cshtml** archivo en Visual Studio si aún no está abierto.
4. Agregue el código HTML siguiente al archivo justo después de la lista sin ordenar que agregó en la tarea anterior.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample16.html)]
5. Dentro del elemento de secuencia de comandos en la parte inferior del documento, agregue el siguiente código resaltado para controlar los eventos de clic de botón, que enviará los datos a la API Web mediante una llamada HTTP POST.

    [!code-html[Main](build-restful-apis-with-aspnet-web-api/samples/sample17.html)]
6. En **ContactController.cs**, coloque un punto de interrupción en el **Post** método.
7. Presione **F5** para ejecutar la aplicación en el explorador.
8. Una vez que se carga la página en el explorador, escriba un nuevo nombre de contacto y el identificador y haga clic en el **guardar** botón.

    ![Carga el documento de cliente HTML en el explorador](build-restful-apis-with-aspnet-web-api/_static/image24.png "cargará el documento de cliente HTML en el explorador")

    *Carga el documento HTML de cliente en el explorador*
9. Cuando la ventana del depurador se interrumpe la **Post** método, echar un vistazo a las propiedades de la **póngase en contacto con** parámetro. Los valores deben coincidir con los datos especificados en el formulario.

    ![El objeto de contacto que se envían a la API Web desde el cliente](build-restful-apis-with-aspnet-web-api/_static/image25.png "objeto póngase en contacto con el que se envían a la API Web desde el cliente")

    *El objeto de contacto que se envían a la API Web desde el cliente*
10. Paso a través del método en el depurador hasta el **respuesta** variable se ha creado. Tras la inspección en el **variables locales** ventana en el depurador, verá que se han establecido todas las propiedades.

   ![La respuesta después de la creación en el depurador](build-restful-apis-with-aspnet-web-api/_static/image26.png "la respuesta después de la creación en el depurador")

   *La respuesta después de la creación en el depurador*
11. Si presiona **F5** o haga clic en **continuar** en el depurador se completará la solicitud. Cuando cambie al explorador, el nuevo contacto se agregó a la lista de contactos almacenados por la **ContactRepository** implementación.

   ![El explorador refleja la creación correcta de la nueva instancia de contacto](build-restful-apis-with-aspnet-web-api/_static/image27.png "el explorador refleja la creación correcta de la nueva instancia de contacto")

   *El explorador refleja la creación correcta de la nueva instancia de contacto*

> [!NOTE]
> Además, puede implementar esta aplicación a Azure siguiente [Apéndice C: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixC).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

Este laboratorio presenta el nuevo marco de ASP.NET Web API y la implementación de API Web RESTful mediante el marco de trabajo. Desde aquí, puede crear un nuevo repositorio que facilita la persistencia de datos mediante cualquier número de mecanismos y conectar ese servicio en lugar del simple proporcionada como ejemplo en este laboratorio. Web API es compatible con un número de características adicionales, como la habilitación de la comunicación de clientes que no sean HTML escritos en cualquier lenguaje que admita HTTP y JSON o XML. La capacidad para hospedar una API Web fuera de una aplicación web típica también es posible, así como es la capacidad para crear sus propios formatos de serialización.

El sitio Web de ASP.NET tiene un área dedicada para el marco de ASP.NET Web API en [ [ https://asp.net/web-api ](https://asp.net/web-api) ](https://asp.net/web-api). Este sitio seguirá proporcionando información más reciente, ejemplos y noticias relacionadas con la API Web, así que comprobación con frecuencia si gustaría profundizar más en el arte de la creación de API Web personalizadas disponibles para prácticamente cualquier marco de desarrollo o de dispositivo.

<a id="AppendixA"></a>

<a id="Appendix_A_Using_Code_Snippets"></a>
## <a name="appendix-a-using-code-snippets"></a>Apéndice A: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](build-restful-apis-with-aspnet-web-api/_static/image28.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

<a id="CodeSnippetUsingKeyBoard"></a>

<a id="To_add_a_code_snippet_using_the_keyboard_C_only"></a>
### <a name="to-add-a-code-snippet-using-the-keyboard-c-only"></a>Para agregar un fragmento de código mediante el teclado (solo C#)

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
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

1. Haga doble clic en el desea insertar el fragmento de código.
2. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
3. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

    ![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](build-restful-apis-with-aspnet-web-api/_static/image32.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

    *Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

    ![Elegir el fragmento de código relevante en la lista, hacer clic en ella](build-restful-apis-with-aspnet-web-api/_static/image33.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

    *Elegir el fragmento de código relevante en la lista, hacer clic en ella*

<a id="AppendixB"></a>

<a id="Appendix_B_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-b-installing-visual-studio-express-2012-for-web"></a>Apéndice B: instalar Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con Azure SDK</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](build-restful-apis-with-aspnet-web-api/_static/image34.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](build-restful-apis-with-aspnet-web-api/_static/image35.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](build-restful-apis-with-aspnet-web-api/_static/image36.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](build-restful-apis-with-aspnet-web-api/_static/image37.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](build-restful-apis-with-aspnet-web-api/_static/image38.png)

    *VS Express para el icono de Web*

<a id="AppendixC"></a>

<a id="Appendix_C_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-c-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice C: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy

En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Azure.

<a id="ApxCTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde el Portal de Azure

1. Vaya a la [Portal de administración de Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image39.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](build-restful-apis-with-aspnet-web-api/_static/image40.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](build-restful-apis-with-aspnet-web-api/_static/image41.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![Exploración al sitio web nuevo](build-restful-apis-with-aspnet-web-api/_static/image42.png "exploración al sitio web nuevo")

    *Exploración al sitio web nuevo*

    ![Sitio Web que se ejecuta](build-restful-apis-with-aspnet-web-api/_static/image43.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](build-restful-apis-with-aspnet-web-api/_static/image44.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en un Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web en Azure.

    ![Descargando el sitio web de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image45.png "descargando el sitio web de perfil de publicación")

    *Descargando el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image46.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxCTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Azure en **bases de datos Sql** | **servidores** | **panel del servidor**. Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas. No cree la base de datos, como se crearán en una etapa posterior.

    ![Panel de base de datos SQL Server](build-restful-apis-with-aspnet-web-api/_static/image47.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](build-restful-apis-with-aspnet-web-api/_static/image48.png) botón.

    ![Agregar dirección IP del cliente](build-restful-apis-with-aspnet-web-api/_static/image49.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](build-restful-apis-with-aspnet-web-api/_static/image50.png)

    *Confirmar cambios*

<a id="ApxCTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](build-restful-apis-with-aspnet-web-api/_static/image51.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](build-restful-apis-with-aspnet-web-api/_static/image52.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](build-restful-apis-with-aspnet-web-api/_static/image53.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de implementación Web](build-restful-apis-with-aspnet-web-api/_static/image54.png "configuración de implementación Web")

    *Configuración de implementación Web*
5. Configure la conexión de base de datos como sigue:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.
   - Escriba un nuevo nombre de base de datos, por ejemplo: *MVC4SampleDB*.

     ![Configuración de la cadena de conexión de destino](build-restful-apis-with-aspnet-web-api/_static/image55.png "configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.

    ![Creación de la base de datos](build-restful-apis-with-aspnet-web-api/_static/image56.png "creación de la cadena de la base de datos")

    *Creación de la base de datos*
7. La cadena de conexión que se usará para conectarse a la base de datos de SQL en Windows Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](build-restful-apis-with-aspnet-web-api/_static/image57.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **Preview** página, haga clic en **publicar**.

    ![Publicar la aplicación web](build-restful-apis-with-aspnet-web-api/_static/image58.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

    ![Publica la aplicación en Windows Azure](build-restful-apis-with-aspnet-web-api/_static/image59.png "publica la aplicación en Windows Azure")

    *Aplicación publicada en Azure*
