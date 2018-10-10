---
uid: signalr/overview/getting-started/real-time-web-applications-with-signalr
title: 'Laboratorio práctico: Las aplicaciones Web en tiempo real con SignalR | Microsoft Docs'
author: rick-anderson
description: Aplicaciones Web en tiempo real ofrecen la posibilidad de insertar contenido para los clientes conectados, como sucede, en tiempo real del servidor. Para los desarrolladores ASP.NET, ASP...
ms.author: riande
ms.date: 07/16/2014
ms.assetid: ba07958c-42e1-4da0-81db-ba6925ed6db0
msc.legacyurl: /signalr/overview/getting-started/real-time-web-applications-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 59831fb8497c86ec5e02de3912b36a15f416597c
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913247"
---
<a name="hands-on-lab-real-time-web-applications-with-signalr"></a>Laboratorio práctico: Las aplicaciones Web en tiempo real con SignalR
====================
por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](http://aka.ms/webcamps-training-kit)

> Aplicaciones Web en tiempo real ofrecen la posibilidad de insertar contenido para los clientes conectados, como sucede, en tiempo real del servidor. Para los desarrolladores ASP.NET, **ASP.NET SignalR** es una biblioteca para agregar funcionalidad web en tiempo real a sus aplicaciones. Aprovecha de varios transportes, seleccionar automáticamente el mejor transporte disponible según el cliente y mejor transporte disponible del servidor. Aprovecha las ventajas de **WebSocket**, una API de HTML5 que permite la comunicación bidireccional entre el explorador y el servidor.
> 
> **SignalR** también proporciona una API sencilla y de alto nivel para el servidor al cliente RPC (llamada a funciones de JavaScript en exploradores de sus clientes desde el código de .NET del lado servidor) en la aplicación ASP.NET, así como agregar enlaces útiles para la administración de conexiones Por ejemplo, conectar o desconectar eventos, agrupación de conexiones y autorización.
> 
> **SignalR** es una abstracción sobre algunos de los transportes que son necesarios para realizar el trabajo en tiempo real entre cliente y servidor. Un **SignalR** conexión se inicia como HTTP y, a continuación, se promueve a un **WebSocket** conexión si está disponible. **WebSocket** es el transporte ideal para **SignalR**, ya que facilita el uso más eficaz de memoria del servidor, tiene la latencia más baja y tiene las características más subyacentes (como la comunicación dúplex completa entre cliente y servidor), pero también tiene los requisitos más exigentes: **WebSocket** requiere que el servidor a usar **Windows Server 2012** o **Windows 8**, junto con **.NET framework 4.5**. Si no se cumplen estos requisitos, **SignalR** intentará usar otros transportes para realizar las conexiones (como *Ajax de sondeo prolongado*).
> 
> El **SignalR** API contiene dos modelos para la comunicación entre clientes y servidores: **conexiones persistentes** y **Hubs**. Un **conexión** representa un punto de conexión simple para enviar solo-recipient, agrupadas o mensajes de difusión. Un **concentrador** es una canalización de más alto nivel desarrollada sobre la API de conexión que permite que el cliente y servidor llamar a métodos entre sí directamente.
> 
> ![Arquitectura de SignalR](real-time-web-applications-with-signalr/_static/image1.png)
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Enviar notificaciones desde el servidor al cliente mediante SignalR.
- Escalar horizontalmente la aplicación de SignalR con **SQL Server**.

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para poder ejecutar los ejercicios en este laboratorio práctico, deberá configurar primero el entorno.

1. Abra una ventana del explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que se configure su entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
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

1. [Trabajar con datos en tiempo real con SignalR](#Exercise1)
2. [Escalado horizontal con SQL Server](#Exercise2)

Tiempo estimado para completar esta práctica: **60 minutos**

> [!NOTE]
> Primera vez que inicie Visual Studio, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos de este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-working-with-real-time-data-using-signalr"></a>Ejercicio 1: Trabajar con datos en tiempo real con SignalR

Mientras chat a menudo se usa como ejemplo, puede hacer un conjunto mucho más con la funcionalidad Web en tiempo real. Siempre que un usuario actualiza una página web para ver los nuevos datos o implementa la página Ajax largo de sondeo para recuperar los datos nuevos, puede usar SignalR.

SignalR admite **inserción de servidor** o **difusión** funcionalidad; controla la administración de la conexión automáticamente. En conexiones HTTP clásicas para la comunicación cliente / servidor, se vuelve a establecer para cada solicitud de conexión, pero SignalR proporciona una conexión persistente entre el cliente y servidor. En el código de servidor llama a un código de cliente en el explorador mediante llamadas a procedimiento remoto (RPC) de SignalR, en lugar del modelo de solicitud-respuesta que conocemos hoy.

En este ejercicio, configurará la **"geek" Quiz** aplicación para usar SignalR para mostrar el panel de las estadísticas con las métricas actualizados sin la necesidad de actualizar la página completa.

<a id="Ex1Task1"></a>
#### <a name="task-1--exploring-the-geek-quiz-statistics-page"></a>Tarea 1: exploración de la página de estadísticas "geek" cuestionario

En esta tarea, debe ir a través de la aplicación y comprobar cómo se muestra la página de estadísticas y cómo puede mejorar la forma en que la información se actualiza.

1. Abra **Visual Studio Express 2013 para Web** y abra el **GeekQuiz.sln** solución se encuentra en la **Source\Ex1 WorkingWithRealTimeData\Begin** carpeta.
2. Presione **F5** para ejecutar la solución. El **iniciarla** página debería aparecer en el explorador.

    ![Ejecución de la solución](real-time-web-applications-with-signalr/_static/image2.png "ejecución de la solución")

    *Ejecución de la solución*
3. Haga clic en **registrar** en la esquina superior derecha de la página para crear un nuevo usuario en la aplicación.

    ![Registrar el vínculo](real-time-web-applications-with-signalr/_static/image3.png "vínculo de registro")

    *Vínculo de registro*
4. En el **registrar** , escriba un **nombre de usuario** y **contraseña**y, a continuación, haga clic en **registrar**.

    ![Registrar un usuario](real-time-web-applications-with-signalr/_static/image4.png "registrar un usuario")

    *Registrar un usuario*
5. La aplicación registra la nueva cuenta y el usuario está autenticado y redirigido de nuevo a la página principal que muestra la primera pregunta de la prueba.
6. Abra el **estadísticas** página en una nueva ventana y poner el **inicio** página y **estadísticas** página side-by-side.

    ![Windows Side-by-side](real-time-web-applications-with-signalr/_static/image5.png "lado Windows lado")

    *Windows en paralelo*
7. En el **inicio** página, responda a la pregunta, haga clic en una de las opciones.

    ![Responder a una pregunta](real-time-web-applications-with-signalr/_static/image6.png "responder una pregunta")

    *Responder a una pregunta*
8. Después de hacer clic en uno de los botones, debería aparecer la respuesta.

    ![Pregunta respondida correcto](real-time-web-applications-with-signalr/_static/image7.png "pregunta respondida correcto")

    *Preguntas respondidas correctamente*
9. Tenga en cuenta que la información proporcionada en la página de estadísticas no está actualizada. Actualice la página para ver los resultados actualizados.

    ![Página de estadísticas](real-time-web-applications-with-signalr/_static/image8.png "página de estadísticas")

    *Página de estadísticas*
10. Vuelva a Visual Studio y detener la depuración.

<a id="Ex1Task2"></a>
#### <a name="task-2--adding-signalr-to-geek-quiz-to-show-online-charts"></a>Tarea 2: agregar SignalR al cuestionario de experto para mostrar gráficos en línea

En esta tarea, agregará SignalR a la solución y enviar actualizaciones a los clientes automáticamente cuando se envía una respuesta nueva al servidor.

1. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de NuGet**y, a continuación, haga clic en **Package Manager Console**.
2. En el **Package Manager Console** ventana, ejecute el siguiente comando:

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample1.ps1)]

    ![Instalación del paquete SignalR](real-time-web-applications-with-signalr/_static/image9.png "SignalR instalación del paquete")

    *Instalación del paquete de SignalR*

   > [!NOTE]
   > Al instalar **SignalR** versión de los paquetes de NuGet 2.0.2 desde una nueva aplicación MVC 5, deberá actualizar manualmente **OWIN** paquetes a la versión 2.0.1 (o superior) antes de instalar SignalR. Para ello, puede ejecutar el script siguiente en el **Package Manager Console**:
   > 
   > [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample2.ps1)]
   > 
   > En una futura versión de SignalR, se actualizará automáticamente las dependencias OWIN.
3. En **el Explorador de soluciones**, expanda el **Scripts** carpeta y tenga en cuenta que SignalR *js* se agregaron archivos a la solución.

    ![Hace referencia JavaScript de SignalR](real-time-web-applications-with-signalr/_static/image10.png "hace referencia JavaScript de SignalR")

    *Referencias de JavaScript de SignalR*
4. En **el Explorador de soluciones**, haga clic en el **GeekQuiz** proyecto, seleccione **agregar** | **nueva carpeta**y denomínelo  **Concentradores**.
5. Haga clic en el **Hubs** carpeta y seleccione **Add | Nuevo elemento**.

    ![Agregar nuevo elemento](real-time-web-applications-with-signalr/_static/image11.png "Agregar nuevo elemento")

    *Agregar nuevo elemento*
6. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione el **Visual C# | Web | SignalR** nodo en el panel izquierdo, seleccione **clase de concentrador de SignalR (v2)** desde el panel central, el nombre del archivo **StatisticsHub.cs** y haga clic en **agregar**.

    ![Cuadro de diálogo Agregar nuevo elemento](real-time-web-applications-with-signalr/_static/image12.png "cuadro de diálogo Agregar nuevo elemento")

    *Agregar cuadro de diálogo nuevo elemento*
7. Reemplace el código en el **StatisticsHub** con el código siguiente.

    (Código de fragmento de código - *StatisticsHubClass RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample3.cs)]
8. Abra **Startup.cs** y agregue la siguiente línea al final de la **configuración** método.

    (Código de fragmento de código - *MapSignalR RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample4.cs)]
9. Abra el **StatisticsService.cs** página dentro de la **servicios** carpeta y agregue las siguientes directivas using.

    (Código de fragmento de código - *UsingDirectives RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample5.cs)]
10. Para notificar a los clientes conectados de actualizaciones, primero recupere una **contexto** objeto para la conexión actual. El **concentrador** objeto contiene métodos para enviar mensajes a un único cliente o difusión a todos los clientes. Agregue el método siguiente a la **StatisticsService** clase para difundir los datos de estadísticas.

    (Código de fragmento de código - *NotifyUpdatesMethod RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample6.cs)]

    > [!NOTE]
    > En el código anterior, se usa un nombre de método arbitrarios para llamar a una función en el cliente (es decir,: *updateStatistics*). El nombre del método que especifique se interpreta como un objeto dinámico, lo que significa que hay ningún IntelliSense ni la validación en tiempo de compilación para él. La expresión se evalúa en tiempo de ejecución. Cuando se ejecuta la llamada al método, SignalR envía el nombre del método y los valores de parámetro para el cliente. Si el cliente tiene un método que coincide con el nombre, que se llama al método y los valores de parámetros se pasan a él. Si no se encuentra ningún método coincidente en el cliente, se produce ningún error. Para obtener más información, consulte [Guía de la API de ASP.NET SignalR Hubs](../guide-to-the-api/hubs-api-guide-server.md).
11. Abra el **TriviaController.cs** página dentro de la **controladores** carpeta y agregue las siguientes directivas using.

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample7.cs)]
12. Agregue el código resaltado siguiente a la **Post** método de acción.

    (Código de fragmento de código - *NotifyUpdatesCall RealTimeSignalR - Ex1 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample8.cs)]
13. Abra el **Statistics.cshtml** página dentro de la **vistas | Inicio** carpeta. Busque el **Scripts** sección y agregue las siguientes referencias de script al principio de la sección.

    (Código de fragmento de código - *SignalRScriptReferences RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample9.cshtml)]

    > [!NOTE]
    > Al agregar SignalR y otras bibliotecas de script al proyecto de Visual Studio, el Administrador de paquetes puede instalar una versión del archivo de script de SignalR que es más reciente que la versión que se muestra en este tema. Asegúrese de que la referencia de script en el código coincide con la versión de la biblioteca de scripts instalada en su proyecto.
14. Agregue el siguiente código resaltado para conectar al cliente con el centro de SignalR y actualizar los datos de estadísticas cuando se recibe un mensaje nuevo desde el centro.

    (Código de fragmento de código - *SignalRClientCode RealTimeSignalR - Ex1 -*)

    [!code-cshtml[Main](real-time-web-applications-with-signalr/samples/sample10.cshtml)]

    En este código, se crea a un Proxy de concentrador y registrar un controlador de eventos para escuchar los mensajes enviados por el servidor. En este caso, se escuchan los mensajes enviados a través de la *updateStatistics* método.

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>Tarea 3: ejecución de la solución

En esta tarea, ejecutará la solución para comprobar que la vista de las estadísticas se actualiza automáticamente con SignalR tras responder una pregunta nueva.

1. Presione **F5** para ejecutar la solución.

    > [!NOTE]
    > Si no ha iniciado sesión la aplicación, inicie sesión con el usuario que creó en la tarea 1.
2. Abra el **estadísticas** página en una nueva ventana y poner el **inicio** página y **estadísticas** página side-by-side como hizo en la tarea 1.
3. En el **inicio** página, responda a la pregunta, haga clic en una de las opciones.

    ![Responder a otra pregunta](real-time-web-applications-with-signalr/_static/image13.png "responder a otra pregunta")

    *Responder a otra pregunta*
4. Después de hacer clic en uno de los botones, debería aparecer la respuesta. Tenga en cuenta que la información de estadísticas en la página se actualiza automáticamente después de responder a la pregunta con la información actualizada sin necesidad de actualizar la página completa.

    ![Página de estadísticas se actualiza después de la respuesta](real-time-web-applications-with-signalr/_static/image14.png "página de estadísticas se actualiza después de la respuesta")

    *Página de estadísticas actualizado después de la respuesta*

<a id="Exercise2"></a>
### <a name="exercise-2-scaling-out-using-sql-server"></a>Ejercicio 2: Escalado horizontal con SQL Server

Al escalar una aplicación web, por lo general puede elegir entre *escalado* y *ampliación* opciones. *Escalar verticalmente* implica el uso de un servidor más grande, con más recursos (CPU, RAM, etc.) al *escalar horizontalmente* significa agregar más servidores para administrar la carga. El problema con este último es que los clientes pueden obtener enrutar a diferentes servidores. Un cliente que está conectado a un servidor no recibirá los mensajes enviados desde otro servidor.

Puede solucionar estos problemas mediante el uso de un componente denominado *backplane*, para reenviar los mensajes entre servidores. Con un backplane habilitado, cada instancia de la aplicación envía mensajes al backplane y el backplane reenvía a las otras instancias de la aplicación.

Actualmente hay tres tipos de planos posteriores para SignalR:

- **Microsoft Azure Service Bus**. Service Bus es una infraestructura de mensajería que permite a los componentes enviar mensajes de acoplamiento flexible.
- **SQL Server**. El plano posterior de SQL Server escribe mensajes en las tablas SQL. El backplane utiliza a Service Broker para la mensajería eficaz. Sin embargo, también funciona si no está habilitado Service Broker.
- **Redis**. Redis es un almacén de pares clave-valor en memoria. Redis admite un patrón de publicación/suscripción ("pub/sub") para enviar mensajes.

Cada mensaje se envía a través de un bus de mensajes. Un bus de mensajes se implementa el [IMessageBus](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.messaging.imessagebus(v=vs.100).aspx) interfaz, que proporciona una abstracción de publicación/suscripción. Funcionan los planos posteriores reemplazando el valor predeterminado **IMessageBus** con un bus diseñado por el backplane.

Cada instancia del servidor se conecta a la placa posterior a través del bus. Cuando se envía un mensaje, pasa a la placa posterior y el backplane lo envía a todos los servidores. Cuando un servidor recibe un mensaje del backplane, almacena el mensaje en su memoria caché local. El servidor, a continuación, envía mensajes a los clientes de su caché local.

Para obtener más información sobre cómo funciona el backplane SignalR, lea esta [artículo](../performance/scaleout-in-signalr.md).

> [!NOTE]
> Hay algunos escenarios donde un backplane puede convertirse en un cuello de botella. Estos son algunos escenarios típicos de SignalR:
> 
> - [Difusión de servidor](tutorial-server-broadcast-with-signalr.md) (p. ej., tablero de cotizaciones): planos posteriores funcionan bien para este escenario, porque el servidor controla la velocidad a la que se envían los mensajes.
> - [Cliente a cliente](tutorial-getting-started-with-signalr.md) (p. ej., chat): en este escenario, el backplane podría ser un cuello de botella si el número de mensajes se escala con el número de clientes; es decir, si la tasa de mensajes aumenta proporcionalmente como más los clientes se unen.
> - [En tiempo real de alta frecuencia](tutorial-high-frequency-realtime-with-signalr.md) (por ejemplo, juegos en tiempo real): no se recomienda un backplane para este escenario.


En este ejercicio, usará **SQL Server** para distribuir los mensajes a través de la **"geek" Quiz** aplicación. Estas tareas ejecutará en un equipo de prueba individual para obtener información sobre cómo establecer la configuración, sino con el fin de obtener el efecto completo, deberá implementar la aplicación de SignalR en dos o más servidores. También debe instalar a SQL Server en uno de los servidores o en un servidor dedicado independiente.

![Escalado horizontal con diagrama SQL Server](real-time-web-applications-with-signalr/_static/image15.png)

<a id="Ex2Task1"></a>
#### <a name="task-1---understanding-the-scenario"></a>Tarea 1: descripción del escenario

En esta tarea, ejecutará 2 instancias de **"geek" Quiz** simulación de IIS de varias instancias en el equipo local. En este escenario, cuando se responde a preguntas de curiosidades en una aplicación, no recibirá ninguna notificación de actualización en la página de estadísticas de la segunda instancia. Esta simulación es similar a un entorno donde la aplicación se implementa en varias instancias y el uso de un equilibrador de carga para comunicarse con ellos.

1. Abra el **Begin.sln** solución se encuentra en la **Begin/origen/Ex2-ScalingOutWithSQLServer** carpeta. Una vez cargado, observará en el **Explorador de servidores** que la solución tiene dos proyectos con idéntica estructura, pero con distintos nombres. Esto simulará que se ejecutan dos instancias de la misma aplicación en el equipo local.

    ![Comenzar la solución de simulación 2 instancias de prueba "geek"](real-time-web-applications-with-signalr/_static/image16.png "comenzar la solución de simulación 2 instancias de prueba \"geek\"")

    *Comenzar la solución de simulación 2 instancias de prueba "geek"*
2. Abra la página de propiedades de la solución haciendo clic en el nodo de la solución y seleccione **propiedades**. En **proyecto de inicio**, seleccione **varios proyectos de inicio** y cambie el **acción** valor para ambos proyectos a *iniciar*.

    ![A partir de varios proyectos](real-time-web-applications-with-signalr/_static/image17.png "a partir de varios proyectos")

    *A partir de varios proyectos*
3. Presione **F5** para ejecutar la solución. La aplicación iniciará dos instancias de **"geek" Quiz** en puertos diferentes, simulación de varias instancias de la misma aplicación. Anclar uno de los exploradores a izquierda y la otra a la derecha de la pantalla. Inicie sesión con sus credenciales o registre un nuevo usuario. Una vez iniciada la sesión, mantenga la página de elementos triviales de la izquierda y vaya a la **estadísticas** página en el Explorador de la derecha.

    ![Cuestionario de experto en paralelo](real-time-web-applications-with-signalr/_static/image18.png)

    *Cuestionario de experto en paralelo*

    ![Cuestionario de experto en puertos diferentes](real-time-web-applications-with-signalr/_static/image19.png)

    *Cuestionario de experto en puertos diferentes*
4. Empezar a responder preguntas en el Explorador de la izquierda y observará que el **estadísticas** página en el Explorador de la derecha no se está actualizando. Esto es porque **SignalR** usa la caché de una variable local para distribuir los mensajes entre sus clientes y este escenario está simulando varias instancias, por lo tanto, la memoria caché no se comparte entre ellos. Puede comprobar que **SignalR** funciona mediante pruebas de los mismos pasos pero con una sola aplicación. En las siguientes tareas configurará un backplane para replicar los mensajes entre instancias.
5. Vuelva a Visual Studio y detener la depuración.

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-sql-server-backplane"></a>Tarea 2: crear el Backplane SQL Server

En esta tarea, creará una base de datos que actúa como un backplane de la **"geek" Quiz** aplicación. Va a usar **Explorador de objetos de SQL Server** para examinar el servidor e inicializar la base de datos. Además, habilitará la **Service Broker**.

1. En **Visual Studio**, abra menú **vista** y seleccione **Explorador de objetos de SQL Server**.
2. Conectarse a la instancia de LocalDB haciendo clic con el **SQL Server** nodo y seleccione **agregar SQL Server...**  opción.

    ![Adición de una instancia de SQL Server](real-time-web-applications-with-signalr/_static/image20.png "agregar una instancia de SQL Server")

    *Adición de una instancia de SQL Server en el Explorador de objetos de SQL Server*
3. Establecer el **nombre del servidor** a *(localdb) \v11.0* y dejar **Windows autenticación** como el modo de autenticación. Haga clic en **Connect** para continuar.

    ![Conectarse a LocalDB](real-time-web-applications-with-signalr/_static/image21.png "conectarse a LocalDB")

    *Conectarse a LocalDB*
4. Ahora que está conectado a la instancia de LocalDB, deberá crear una base de datos que representa el plano posterior de SQL Server para SignalR. Para ello, haga clic en el **bases de datos** nodo y seleccione **agregar nueva base de datos**.

    ![Agregar una nueva base de datos](real-time-web-applications-with-signalr/_static/image22.png "agregando una nueva base de datos")

    *Agregar una nueva base de datos*
5. Establezca el nombre de la base de datos en *SignalR* y haga clic en **Aceptar** para crearlo.

    ![Creación de la base de datos de SignalR](real-time-web-applications-with-signalr/_static/image23.png "crear la base de datos de SignalR")

    *Creación de la base de datos de SignalR*

    > [!NOTE]
    > Puede elegir cualquier nombre para la base de datos.
6. Para recibir actualizaciones de forma más eficaz de la placa posterior, se recomienda habilitar Service Broker para la base de datos. Service Broker proporciona compatibilidad nativa para la mensajería y puesta en cola en SQL Server. El backplane también funciona sin el Service Broker. Abra una nueva consulta con el botón secundario de la base de datos y seleccione **nueva consulta**.

    ![Abrir una consulta nueva](real-time-web-applications-with-signalr/_static/image24.png "abriendo una nueva consulta")

    *Abrir una consulta nueva*
7. Para comprobar si está habilitado Service Broker, consulta el **es\_broker\_habilitado** columna en el **sys.databases** vista de catálogo. Ejecute el siguiente script en la ventana de consulta abierta recientemente.

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample11.sql)]

    ![Consultar el estado de Service Broker](real-time-web-applications-with-signalr/_static/image25.png "consultar el estado de Service Broker")

    *Consultar el estado de Service Broker*
8. Si el valor de la **es\_broker\_habilitado** columna de la base de datos es &quot;0&quot;, use el siguiente comando para habilitarlo. Reemplace **&lt;sus bases de datos&gt;** con el nombre establecido al crear la base de datos (p. ej.: SignalR).

    [!code-sql[Main](real-time-web-applications-with-signalr/samples/sample12.sql)]

    ![Habilitar Service Broker](real-time-web-applications-with-signalr/_static/image26.png "habilitar Service Broker")

    *Habilitar Service Broker*

    > [!NOTE]
    > Si esta consulta se parece a un interbloqueo, asegúrese de que no hay ninguna aplicación conectada a la base de datos.

<a id="Ex2Task3"></a>
#### <a name="task-3--configuring-the-signalr-application"></a>Tarea 3: configuración de la aplicación de SignalR

En esta tarea, configurará **"geek" Quiz** para conectarse a la placa posterior de SQL Server. En primer lugar agregará el **SignalR.SqlServer** paquete NuGet y establezca la conexión a la base de datos de backplane de cadena.

1. Abra el **Package Manager Console** desde **herramientas** > **Administrador de paquetes de NuGet**. Asegúrese de que **GeekQuiz** proyecto está seleccionado en el **proyecto predeterminado** lista desplegable. Escriba el siguiente comando para instalar el **Microsoft.AspNet.SignalR.SqlServer** paquete NuGet.

    [!code-powershell[Main](real-time-web-applications-with-signalr/samples/sample13.ps1)]
2. Repita el paso anterior, pero esta vez para el proyecto **GeekQuiz2**.
3. Para configurar la placa posterior de SQL Server, abra el **Startup.cs** archivo de la **GeekQuiz** proyecto y agregue el código siguiente a la **configurar** método. Reemplace **&lt;sus bases de datos&gt;** con el nombre de la base de datos que utilizó al crear el backplane de SQL Server. Repita este paso para la **GeekQuiz2** proyecto.

    (Código de fragmento de código - *StartupConfiguration RealTimeSignalR - Ex2 -*)

    [!code-csharp[Main](real-time-web-applications-with-signalr/samples/sample14.cs)]
4. Ahora que ambos proyectos están configurados para usar el backplane de SQL Server, presione **F5** para ejecutarlas al mismo tiempo.
5. Nuevamente, **Visual Studio** iniciará dos instancias de **"geek" Quiz** en puertos diferentes. Uno de los exploradores anclar a la izquierda y la otra a la derecha de la pantalla e inicie sesión con sus credenciales. Mantenga la página de elementos triviales de la izquierda y vaya a **estadísticas** pagein el Explorador de la derecha.
6. Empezar a responder a preguntas en el Explorador de la izquierda. Esta vez, el **estadísticas** se actualiza la página Gracias a la placa posterior. Cambiar entre las aplicaciones (**estadísticas** está ahora a la izquierda, y **curiosidades** está a la derecha) y repita la prueba para validar que se está trabajando para ambas instancias. El backplane actúa como un *compartido caché* de mensajes para cada servidor conectado y cada servidor se almacenarán los mensajes en su propia caché local para distribuir a los clientes conectados.
7. Vuelva a Visual Studio y detener la depuración.
8. El componente de backplane de SQL Server genera automáticamente las tablas necesarias en la base de datos especificado. En el **Explorador de objetos de SQL Server** del panel, abra la base de datos que creó para el backplane (p. ej.: SignalR) y expanda sus tablas. Debería ver las tablas siguientes:

    ![Plano anterior genera tablas](real-time-web-applications-with-signalr/_static/image27.png)

    *Plano anterior genera tablas*
9. Haga clic en el **SignalR.Messages\_0** de tabla y seleccione **ver datos**.

    ![Vista de tabla de mensajes de SignalR Backplane](real-time-web-applications-with-signalr/_static/image28.png)

    *Vista de tabla de mensajes de SignalR Backplane*
10. Puede ver los diferentes mensajes enviados a la **concentrador** al responder a las preguntas de curiosidades. El backplane distribuye estos mensajes a cualquier instancia conectada.

    ![Tabla de mensajes de backplane](real-time-web-applications-with-signalr/_static/image29.png)

    *Tabla de mensajes de backplane*

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido cómo agregar **SignalR** sus aplicaciones y enviar notificaciones desde el servidor a los clientes conectados con **Hubs**. Además, ha aprendido a escalar horizontalmente la aplicación mediante el uso de un *backplane* componente cuando la aplicación se implementa en varias instancias IIS.
