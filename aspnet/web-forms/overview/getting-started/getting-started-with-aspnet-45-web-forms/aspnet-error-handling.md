---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: Control de errores de ASP.NET | Documentos de Microsoft
author: Erikre
description: "Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: c5ec43ac78be4a9452ebaa6495a6883506ac162f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-error-handling"></a>Control de errores de ASP.NET
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


En este tutorial, modificará la aplicación de ejemplo Wingtip Toys para que incluya control de errores y registro de errores. Control de errores le permitirá la aplicación controlar los errores correctamente y mostrar mensajes de error en consecuencia. Registro de errores le permitirá buscar y corregir los errores que se han producido. Este tutorial se basa en el tutorial anterior "Enrutamiento de direcciones URL" y forma parte de la serie de tutoriales de Wingtip Toys.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo agregar a la configuración de la aplicación de control de errores global.
- Cómo agregar en la página de aplicación y los niveles de código de control de errores.
- Cómo registrar los errores para su posterior revisión.
- Cómo mostrar mensajes de error que no poner en peligro la seguridad.
- Cómo implementar el registro de errores de módulos de registro de Error y controladores (ELMAH).

## <a name="overview"></a>Información general

Las aplicaciones de ASP.NET deben ser capaces de controlar los errores que se producen durante la ejecución de una manera coherente. ASP.NET usa common language runtime (CLR), que proporciona una manera de notificar a las aplicaciones de errores de manera uniforme. Cuando se produce un error, se produce una excepción. Una excepción es cualquier error, condición o un comportamiento inesperado que encuentra una aplicación.

En .NET Framework, una excepción es un objeto que hereda de la clase `System.Exception`. Una excepción se inicia desde un área del código en la que ha producido un problema. La excepción se pasa a la pila de llamadas en un lugar donde la aplicación proporciona código para controlar la excepción. Si la aplicación no controla la excepción, el explorador se obliga a mostrar los detalles del error.

Como práctica recomendada, controlar los errores en el nivel de código en `Try` / `Catch` / `Finally` bloques dentro del código. Intente colocar estos bloques para que el usuario puede corregir problemas en el contexto en el que se producen. Si los bloques de control de errores están demasiado lejos de donde se produjo el error, es más difícil proporcionar a los usuarios con la información necesaria solucionar el problema.

### <a name="exception-class"></a>Exception (clase)

La clase de excepción es la clase base desde la que se derivan las excepciones. La mayoría de los objetos de excepción son instancias de alguna clase derivada de la clase de excepción, como el `SystemException` (clase), el `IndexOutOfRangeException` (clase), o la `ArgumentNullException` clase. La clase de excepción tiene propiedades, como el `StackTrace` propiedad, el `InnerException` propiedad y el `Message` propiedad, que proporcionan información específica acerca del error que se ha producido.

### <a name="exception-inheritance-hierarchy"></a>Jerarquía de herencia de excepción

El tiempo de ejecución tiene un conjunto base de excepciones que derivan de la `SystemException` clase que el tiempo de ejecución que se produce cuando se produce una excepción. La mayoría de las clases que heredan de la clase de excepción, como el `IndexOutOfRangeException` clase y la `ArgumentNullException` de clases, implementan miembros adicionales. Por lo tanto, se puede encontrar la información más importante para una excepción en la jerarquía de excepciones, el nombre de la excepción y la información contenida en la excepción.

### <a name="exception-handling-hierarchy"></a>Jerarquía de control de excepciones

En una aplicación de formularios Web Forms de ASP.NET, se pueden controlar excepciones en función de una jerarquía de control específico. Se puede controlar una excepción en los siguientes niveles:

- Nivel de aplicación
- Nivel de página
- Nivel de código

Cuando una aplicación controla las excepciones, información adicional sobre la excepción que se hereda de la clase de excepción a menudo se recupera y muestra al usuario. Además de la aplicación, la página y el nivel de código, también puede controlar excepciones en el nivel de módulo HTTP y mediante el uso de un controlador personalizado de IIS.

### <a name="application-level-error-handling"></a>Control de errores de nivel de aplicación

Puede controlar los errores de forma predeterminada en el nivel de aplicación mediante la modificación de la configuración de la aplicación o mediante la adición de un `Application_Error` controlador en el *Global.asax* archivo de la aplicación.

Puede controlar los errores HTTP y errores de forma predeterminada mediante la adición de un `customErrors` sección a la *Web.config* archivo. El `customErrors` sección le permite especificar una página predeterminada que se redirigirán a los usuarios cuando se produce un error. También permite especificar las páginas individuales para los errores de código de estado específico.

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

Desgraciadamente, cuando se usa la configuración para redirigir al usuario a una página distinta, no tiene los detalles del error que se ha producido.

Sin embargo, puede capturar los errores que se produzcan en cualquier parte en la aplicación agregando código a la `Application_Error` controlador en el *Global.asax* archivo.

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>Control de eventos de Error de nivel de página

Un controlador de nivel de página devuelve al usuario a la página donde se produjo el error, sino también porque no se mantienen las instancias de controles, ya no habrá nada en la página. Para proporcionar los detalles del error para el usuario de la aplicación, debe escribir específicamente los detalles del error en la página.

Normalmente, utilizaría un controlador de errores de nivel de página para registrar los errores no controlados o para llevar al usuario a una página que puede mostrar información útil.

Este ejemplo de código muestra un controlador para el evento de Error en una página Web ASP.NET. Este controlador detecta todas las excepciones que aún no se administran dentro de `try` / `catch` bloques en la página.

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

Después de controlar un error, deberá borrarlo mediante una llamada a la `ClearError` método del objeto de servidor (`HttpServerUtility` clase), en caso contrario, verá un error que previamente se ha producido.

### <a name="code-level-error-handling"></a>Control de errores de nivel de código

La instrucción try-catch consta de un bloque try seguido de uno o más cláusulas catch, que especifican controladores para diferentes excepciones. Cuando se produce una excepción, common language runtime (CLR) busca la instrucción catch que controla esta excepción. Si el método actualmente en ejecución no contiene un bloque catch, CLR busca el método que llama el método actual y así sucesivamente, la pila de llamadas. Si no se encuentra ningún bloque catch, CLR muestra al usuario un mensaje de excepción no controlada y detiene la ejecución del programa.

En el ejemplo de código siguiente se muestra una manera común de usar `try` / `catch` / `finally` para controlar los errores.

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

En el código anterior, el bloque try contiene el código que debe protegerse contra una posible excepción. El bloque se ejecuta hasta que se produce una excepción o el bloque se completó correctamente. Si un `FileNotFoundException` excepción o un `IOException` se produce una excepción, la ejecución se transfiere a otra página. A continuación, el código incluido en el bloque finally se ejecuta, si se produjo un error o no.

## <a name="adding-error-logging-support"></a>Agregar compatibilidad con el registro de errores

Antes de agregar el control de errores a la aplicación de ejemplo Wingtip Toys, agregará compatibilidad con el registro de errores mediante la adición de un `ExceptionUtility` clase a la *lógica* carpeta. Al hacerlo, cada vez que la aplicación controla un error, los detalles del error se agregará el archivo de registro de errores.

1. Haga clic en el *lógica* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **código** grupo de plantillas de la izquierda. A continuación, seleccione **clase**desde la mitad de lista y asígnele el nombre **ExceptionUtility.cs**.
3. Haga clic en **Agregar**. Se muestra el nuevo archivo de clase.
4. Reemplace el código existente por el siguiente:  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

Cuando se produce una excepción, la excepción se puede escribir en un archivo de registro de excepción mediante una llamada a la `LogException` método. Este método toma dos parámetros, el objeto de excepción y una cadena que contiene los detalles sobre el origen de la excepción. El registro de excepción se escribe en el *ErrorLog.txt* un archivo en el *aplicación\_datos* carpeta.

### <a name="adding-an-error-page"></a>Agregar una página de Error

En la aplicación de ejemplo Wingtip Toys, una página se utilizará para mostrar los errores. La página de error está diseñada para mostrar un mensaje de error seguro a los usuarios del sitio. Sin embargo, si el usuario es un desarrollador que realiza una solicitud HTTP que se sirve localmente en el equipo donde reside el código, se mostrarán los detalles de error adicional en la página de error.

1. Haga clic en el nombre del proyecto (**Wingtip Toys**) en **el Explorador de soluciones** y seleccione **agregar**  - &gt; **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione el **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda. En la lista central, seleccione **formulario Web con página maestra**y asígnele el nombre **ErrorPage.aspx**.
3. Haga clic en **Agregar**.
4. Seleccione el *Site.Master* de archivos como la página maestra y, a continuación, elija **Aceptar**.
5. Reemplace el marcado existente con lo siguiente:   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. Reemplace el código existente de código subyacente (*ErrorPage.aspx.cs*) para que aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

Cuando se muestra la página de error, el `Page_Load` se ejecuta el controlador de eventos. En el `Page_Load` controlador, se determina la ubicación de donde se controló en primer lugar el error. A continuación, se determina el último error producido por llamada la `GetLastError` método del objeto Server. Si la excepción ya no existe, se crea una excepción genérica. A continuación, si la solicitud HTTP se realizó de forma local, se muestran todos los detalles del error. En este caso, solo el equipo local que ejecuta la aplicación web podrá ver estos detalles de error. Después de que se ha mostrado la información de error, el error se agrega al archivo de registro y se borra el error del servidor.

### <a name="displaying-unhandled-error-messages-for-the-application"></a>Mostrar mensajes de Error no controlado de la aplicación

Mediante la adición de un `customErrors` sección a la *Web.config* archivo, puede controlar rápidamente los errores simples que se producen a lo largo de la aplicación. También puede especificar cómo controlar los errores basándose en su valor de código de estado como 404 - no se encontró el archivo.

#### <a name="update-the-configuration"></a>Actualizar la configuración

Actualizar la configuración mediante la adición de un `customErrors` sección a la *Web.config* archivo.

1. En **el Explorador de soluciones**, busque y abra la *Web.config* archivo en la raíz de la aplicación de ejemplo Wingtip Toys.
2. Agregar el `customErrors` sección a la *Web.config* archivo dentro de la `<system.web>` nodo como se indica a continuación:   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. Guardar el *Web.config* archivo.

El `customErrors` sección especifica el modo, que se establece en "On". También especifica el `defaultRedirect`, que indica qué página al que navegar cuando se produce un error a la aplicación. Además, ha agregado un elemento de error específico que especifica cómo controlar un error 404 cuando no se encuentra una página. Más adelante en este tutorial, agregará los errores adicionales que capturarán los detalles de un error en el nivel de aplicación.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación ahora para ver las rutas actualizadas.

1. Presione **F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 El explorador se abre y muestra el *Default.aspx* página.
2. Escriba la siguiente dirección URL en el explorador (no olvide usar **su** número de puerto):  
    `https://localhost:44300/NoPage.aspx`
3. Revise el *ErrorPage.aspx* muestra en el explorador. 

    ![Control de errores de ASP.NET: no encontró la página Error](aspnet-error-handling/_static/image1.png)

Cuando se solicita la *NoPage.aspx* página, que no existe, la página de error mostrará el mensaje de error simple y la información detallada del error si hay detalles adicionales. Sin embargo, si el usuario solicitó una página no existente desde una ubicación remota, la página de error solo se mostrará el mensaje de error en rojo.

### <a name="including-an-exception-for-testing-purposes"></a>Incluyendo una excepción para fines de pruebas

Para comprobar cómo funciona la aplicación cuando un error se produce, deliberadamente puede crear condiciones de error en ASP.NET. En la aplicación de ejemplo Wingtip Toys, producirá una excepción de prueba cuando se carga la página predeterminada para ver lo que ocurre.

1. Abra el código subyacente de la *Default.aspx* página en Visual Studio.   
 El *Default.aspx.cs* se mostrará la página de código subyacente.
2. En el `Page_Load` controlador, agregue código para que el controlador aparece como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

Es posible crear diversos tipos de excepciones. En el código anterior, está creando un `InvalidOperationException` cuando el *Default.aspx* se carga la página.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación para ver cómo la aplicación controla la excepción.

1. Presione **CTRL+F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación produce la excepción InvalidOperationException. 

    > [!NOTE] 
    > 
    > Debe presionar **CTRL+F5** para mostrar la página sin interrumpir el código para ver el origen del error en Visual Studio.
2. Revise el *ErrorPage.aspx* muestra en el explorador. 

    ![Control de errores de ASP.NET - página de Error](aspnet-error-handling/_static/image2.png)

Como puede ver en los detalles del error, la excepción se ha capturado por la `customError` sección la *Web.config* archivo.

### <a name="adding-application-level-error-handling"></a>Agregar control de errores de nivel de aplicación

En lugar de captura la excepción mediante la `customErrors` sección la *Web.config* de archivos, donde obtendrá poca información sobre la excepción, puede intercepta el error en el nivel de aplicación y recuperar los detalles del error.

1. En **el Explorador de soluciones**, busque y abra la *Global.asax.cs* archivo.
2. Agregar un **aplicación\_Error** controlador para que aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

Cuando se produce un error en la aplicación, el `Application_Error` se llamará al controlador. En este controlador, la última excepción recuperan y revisada. Si no se controló la excepción y la excepción contiene detalles de la excepción interna (es decir, `InnerException` no es null), la aplicación transfiere la ejecución a la página de error donde se muestran los detalles de la excepción.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación para ver los detalles de error adicionales proporcionados por controlar la excepción en el nivel de aplicación.

1. Presione **CTRL+F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación inicia el `InvalidOperationException` .
2. Revise el *ErrorPage.aspx* muestra en el explorador. 

    ![Control de errores de ASP.NET - Error de nivel de aplicación](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>Agregar control de errores de nivel de página

Puede agregar el control de errores de nivel de página a una página, ya sea mediante la adición de un `ErrorPage` atribuir a la `@Page` la directiva de la página, o agregando un `Page_Error` controlador de eventos para el código subyacente de una página. En esta sección, agregará un `Page_Error` controlador de eventos que transfiera la ejecución a la *ErrorPage.aspx* página.

1. En **el Explorador de soluciones**, busque y abra la *Default.aspx.cs* archivo.
2. Agregar un `Page_Error` controlador para que el código subyacente aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

Cuando se produce un error en la página, el `Page_Error` se llama al controlador de eventos. En este controlador, la última excepción recuperan y revisada. Si un `InvalidOperationException` se produce, la `Page_Error` controlador de eventos transfiere la ejecución a la página de error donde se muestran los detalles de la excepción.

#### <a name="running-the-application"></a>Ejecutar la aplicación

Puede ejecutar la aplicación ahora para ver las rutas actualizadas.

1. Presione **CTRL+F5** para ejecutar la aplicación de ejemplo Wingtip Toys.  
 La aplicación inicia el `InvalidOperationException` .
2. Revise el *ErrorPage.aspx* muestra en el explorador. 

    ![Control de errores de ASP.NET - Error en el nivel de página](aspnet-error-handling/_static/image4.png)
3. Cierre la ventana del explorador.

### <a name="removing-the-exception-used-for-testing"></a>Quitar la excepción que se usa para pruebas

Para permitir que la aplicación de ejemplo Wingtip Toys funcionando correctamente sin necesidad de iniciar la excepción que agregó anteriormente en este tutorial, quite la excepción.

1. Abra el código subyacente de la *Default.aspx* página.
2. En el `Page_Load` controlador, quite el código que produce la excepción para que el controlador aparece como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>Agregar registro de errores de nivel de código

Como se mencionó anteriormente en este tutorial, puede agregar instrucciones try/catch para intentar ejecutar una sección de código y controlar el primer error que se produce. En este ejemplo, solo escribirá los detalles del error en el archivo de registro de errores para que se puede revisar el error más tarde.

1. En **el Explorador de soluciones**, en la *lógica* carpeta, busque y abra la *PayPalFunctions.cs* archivo.
2. Actualización de la `HttpCall` método para que el código aparezca como sigue:   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

El código anterior se llama el `LogException` método que se encuentra en la `ExceptionUtility` clase. Ha agregado la *ExceptionUtility.cs* archivo de clase para la *lógica* carpeta anteriormente en este tutorial. El método `LogException` toma dos parámetros. El primer parámetro es el objeto de excepción. El segundo parámetro es una cadena que se utiliza para identificar el origen del error.

### <a name="inspecting-the-error-logging-information"></a>Inspeccionar la información de registro de errores

Como se mencionó anteriormente, puede usar el registro de errores para determinar qué errores en la aplicación deben corregirse en primer lugar. Por supuesto, se registrarán solo los errores que se haya quedado bloqueados y se escriben en el registro de errores.

1. En **el Explorador de soluciones**, busque y abra la *ErrorLog.txt* un archivo en el *aplicación\_datos* carpeta.   
 Debe seleccionar el "**mostrar todos los archivos**" opción o la "**actualizar**" opción de la parte superior de **el Explorador de soluciones** para ver el *ErrorLog.txt*archivo.
2. Revise el registro de errores que se muestran en Visual Studio: 

    ![Control de errores de ASP.NET - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>Mensajes de Error seguros

Es **importante tener en cuenta** que cuando la aplicación muestra mensajes de error, no debería incluir información que un usuario malintencionado puede resultarle útil para atacar la aplicación. Por ejemplo, si la aplicación intenta sin éxito escribir una base de datos, no debe mostrar un mensaje de error que incluye el nombre de usuario que está usando. Por esta razón, se muestra un mensaje de error genérico en rojo para el usuario. Solo se muestran todos los detalles de error adicionales para el desarrollador en el equipo local.

## <a name="using-elmah"></a>Usar ELMAH

ELMAH (módulos de registro de Error y controladores) es una función de registro de error que se conectan con la aplicación ASP.NET como un paquete de NuGet. ELMAH proporciona las siguientes capacidades:

- Registro de las excepciones no controladas.
- Una página web para ver todo el registro de las excepciones no controladas recodificadas.
- Una página web para ver los detalles completos de cada uno registra la excepción.
- Notificaciones de correo electrónico de cada error en el momento en que se produce.
- Una fuente RSS de los 15 últimos errores desde el registro.

Para poder trabajar con ELMAH, debe instalarlo. Esto es fácil con el *NuGet* instalador del paquete. Como se mencionó anteriormente en esta serie de tutoriales, NuGet es una extensión de Visual Studio que facilita la instalación y actualización de bibliotecas de código abierto y herramientas en Visual Studio.

1. En Visual Studio, desde la **herramientas** menú, seleccione **Administrador de paquetes de biblioteca**  - &gt; **administrar paquetes de NuGet para la solución**. 

    ![Control de errores de ASP.NET - administrar paquetes de NuGet para la solución](aspnet-error-handling/_static/image6.png)
2. El **administrar paquetes de NuGet** cuadro de diálogo se muestra en Visual Studio.
3. En el **administrar paquetes de NuGet** cuadro de diálogo, expanda **en línea** a la izquierda y, a continuación, seleccione **nuget.org**. A continuación, buscar e instalar el **ELMAH** paquete de la lista de paquetes disponibles en línea. 

    ![Control de errores de ASP.NET - paquete de NuGet ELMA](aspnet-error-handling/_static/image7.png)
4. Debe tener una conexión a internet para descargar el paquete.
5. En el **seleccionar proyectos** diálogo cuadro, asegúrese de que el **WingtipToys** selección está seleccionado y, a continuación, haga clic en **Aceptar**. 

    ![Control de errores de ASP.NET: seleccione el cuadro de diálogo de proyectos](aspnet-error-handling/_static/image8.png)
6. Haga clic en **cerrar** en **administrar paquetes de NuGet** cuadro de diálogo si es necesario.
7. Si Visual Studio solicita que vuelva a cargar los archivos abiertos, seleccione "**sí a todo**".
8. El paquete ELMAH agrega entradas para sí mismo en el *Web.config* archivo en la raíz del proyecto. Si Visual Studio le pregunta si desea volver a cargar modificados *Web.config* de archivos, haga clic en **Sí**.

ELMAH ahora está listo para almacenar los errores no controlados que se producen.

### <a name="viewing-the-elmah-log"></a>Ver el registro ELMAH

Ver el registro ELMAH es fácil, pero en primer lugar creará una excepción no controlada que se registrarán en el registro ELMAH.

1. Presione **CTRL+F5** para ejecutar la aplicación de ejemplo Wingtip Toys.
2. Para escribir una excepción no controlada en el registro ELMAH, navegar por el explorador hasta la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/NoPage.aspx`Se mostrará la página de error.
3. Para mostrar el registro ELMAH, navegar por el explorador hasta la siguiente dirección URL (con el número de puerto):  
    `https://localhost:44300/elmah.axd`

    ![Control de errores de ASP.NET - registro de errores ELMAH](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>Resumen

En este tutorial, ha aprendido sobre el control de errores en el nivel de aplicación, el nivel de página y el nivel de código. También ha aprendido a registrar los errores controlados y para su posterior revisión. Ha agregado la utilidad ELMAH para proporcionar el registro de excepciones y la notificación a la aplicación mediante NuGet. Además, ha aprendido sobre la importancia de los mensajes de error seguros.

## <a name="tutorial-series-conclusion"></a>Conclusión de la serie de tutoriales

*Gracias por seguir. Espero que este conjunto de tutoriales le ayudó familiarizarse más con ASP.NET Web Forms. Si necesita obtener más información acerca de las características de formularios Web Forms disponibles en ASP.NET 4.5 y Visual Studio 2013, consulte* [ *ASP.NET y herramientas Web para Visual Studio 2013 notas* ](../../../../visual-studio/overview/2013/release-notes.md)  *. Además, asegúrese de echar un vistazo el tutorial mencionado en el* ***pasos *** sección y defintely probar la* [ *Azure evaluación gratuita* ](https://azure.microsoft.com/pricing/free-trial/)*.*

![Gracias - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>Pasos siguientes

Más información acerca de cómo implementar la aplicación web en Microsoft Azure, consulte [implementar una aplicación de Secure ASP.NET Web Forms, con pertenencia, OAuth y base de datos SQL en un sitio Web de Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/).

## <a name="free-trial"></a>Prueba gratuita

[Microsoft Azure - versión de prueba gratuita](https://azure.microsoft.com/pricing/free-trial/)  
 Publicar su sitio Web en Microsoft Azure ahorrará tiempo, el mantenimiento y el gasto. Es un proceso rápido para implementar la aplicación web en Azure. Si necesita mantener y supervisar la aplicación web, Azure ofrece una variedad de herramientas y servicios. Administrar datos, el tráfico, identidad, las copias de seguridad, mensajería, multimedia y el rendimiento en Azure. Y todo esto se proporciona en un enfoque muy rentable.

## <a name="additional-resources"></a>Recursos adicionales

[Detalles de Error de registro con la supervisión de estado de ASP.NET](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>Reconocimientos

Me gustaría dar las gracias a las siguientes personas que realizado aportaciones importantes para el contenido de esta serie de tutoriales:

- [Alberto Poblacion, MVP &amp; MCT, España](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen (Países Bajos)](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier, Estados Unidos](http://andret503.wordpress.com/)
- Apurva Joshi, Microsoft
- [Bojan Vrhovnik, Eslovenia](http://twitter.com/bvrhovnik)
- [Bruno Sonnino, Brasil](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Dos de Carlos Santos, Brasil](http://www.carloscds.net/)
- [David Campbell, Estados Unidos](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael almohadillas, Estados Unidos](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike Pope
- [Mitchel vendedores, Estados Unidos](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba, Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado, Portugal](http://paulomorgado.net/)
- [Pranav Rastogi, Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann, Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra, Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>Contribuciones de la Comunidad

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 relacionados con el ejemplo de código en MSDN: [navegación Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 relacionados con el ejemplo de código en MSDN: [serie de Tutorial de ASP.NET 4.5 Web Forms en Visual Basic](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - colaborador de audiencia técnica de Microsoft (de twitter: @driazevedo)  
 Traducción Visual Studio 2012: [Iniciando com ASP.NET Web Forms 4.5 - Parte 1: Introdução e Visão Geral](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[Anterior](url-routing.md)
