---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: Depurar procedimientos almacenados (C#) | Documentos de Microsoft
author: rick-anderson
description: Ediciones de Visual Studio Professional y Team System permiten establecer puntos de interrupción y paso a paso los procedimientos almacenados de SQL Server, que realiza la depuración almacenados...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 52bb409798dae550c664b78521f0fb4793464833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876506"
---
<a name="debugging-stored-procedures-c"></a>Depurar procedimientos almacenados (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip) o [descarga de PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Ediciones de Visual Studio Professional y Team System permiten establecer puntos de interrupción y paso a paso los procedimientos almacenados de SQL Server, que realiza la depuración de procedimientos almacenados tan sencillo como la depuración de código de la aplicación. Este tutorial muestra la depuración de base de datos directa y depuración de la aplicación de procedimientos almacenados.


## <a name="introduction"></a>Introducción

Visual Studio proporciona una rica experiencia de depuración. Con unas pocas pulsaciones de teclas o clics del mouse, se puede usar los puntos de interrupción para detener la ejecución de un programa y examinar su estado y control de flujo de s. Junto con la depuración de código de la aplicación, Visual Studio ofrece compatibilidad para depurar procedimientos almacenados desde dentro de SQL Server. Al igual que se pueden establecer puntos de interrupción en el código de una clase de código subyacente ASP.NET o una clase de la capa de lógica empresarial, por lo que demasiado se pueden colocar dentro de los procedimientos almacenados.

En este tutorial, veremos ejecuta paso a paso los procedimientos almacenados desde el Explorador de servidores en Visual Studio, así como cómo establecer puntos de interrupción que se producen cuando el procedimiento almacenado se llama desde la aplicación ASP.NET en ejecución.

> [!NOTE]
> Por desgracia, solo se pueden ejecutar paso a paso y depurar a través de las versiones Professional y sistemas de equipo de Visual Studio los procedimientos almacenados. Si está utilizando la versión estándar de Visual Studio o Visual Web Developer, es para leer a lo largo de tal y como le guiará por los pasos necesarios para depurar procedimientos almacenados, pero no podrá replicar estos pasos en su equipo.


## <a name="sql-server-debugging-concepts"></a>Conceptos de depuración de SQL Server

Microsoft SQL Server 2005 se diseñó para proporcionar una integración con la [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), que es el tiempo de ejecución utilizado por todos los ensamblados. NET. Por lo tanto, SQL Server 2005 admite objetos de base de datos administrados. Es decir, puede crear objetos de base de datos como procedimientos almacenados y funciones definidas por el usuario (UDF) como métodos en una clase de C#. Esto permite que estos procedimientos almacenados y UDF para utilizar la funcionalidad de .NET Framework y de sus propias clases personalizadas. Por supuesto, SQL Server 2005 también proporciona compatibilidad con objetos de base de datos de T-SQL.

SQL Server 2005 ofrece compatibilidad con la depuración de T-SQL y objetos de base de datos administrados. Sin embargo, solo se pueden depurar estos objetos a través de las ediciones de Visual Studio 2005 Professional y sistemas de equipo. En este tutorial se examinará la depuración de los objetos de base de datos de T-SQL. El tutorial posterior se examina depurar objetos de base de datos administrados.

El [información general de T-SQL y CLR la depuración en SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrada del blog de la [equipo de integración de CLR de SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) resalta las tres maneras de depurar objetos de SQL Server 2005 desde Visual Studio:

- **Dirigir la depuración de base de datos (DDD)** : en el Explorador de servidores se puede ir a cualquier objeto de base de datos de T-SQL, como procedimientos almacenados y UDF. Examinaremos DDD en el paso 1.
- **Depuración de aplicación** -podemos establecer puntos de interrupción dentro de un objeto de base de datos y, a continuación, ejecutar la aplicación ASP.NET. Cuando se ejecuta el objeto de base de datos, se tendrán en cuenta el punto de interrupción y control traspase al depurador. Tenga en cuenta que con la depuración de la aplicación se no podemos depurar paso a paso un objeto de base de datos desde el código de aplicación. Debemos explícitamente establecemos puntos de interrupción en los procedimientos almacenados o UDF donde se desea que el depurador se detenga. Depuración de la aplicación se examina a partir del paso 2.
- **Depuración de un proyecto de SQL Server** -ediciones de Visual Studio Professional y sistemas de equipo son un tipo de proyecto de SQL Server que se usa normalmente para crear objetos de base de datos administrados. Examinaremos con proyectos de SQL Server y la depuración de su contenido en el siguiente tutorial.

Visual Studio puede depurar procedimientos almacenados en instancias de SQL Server locales y remotos. Una instancia de SQL Server local es aquella que está instalado en el mismo equipo que Visual Studio. Si la base de datos de SQL Server que está utilizando no se encuentra en el equipo de desarrollo, se considera una instancia remota. Para estos tutoriales se ha utilizado las instancias de SQL Server locales. Depurar procedimientos almacenados en una instancia remota de SQL server requiere más pasos de configuración que cuando depura los procedimientos almacenados en una instancia local.

Si está utilizando una instancia de SQL Server local, puede iniciar con el paso 1 y trabajar a través de este tutorial hasta el final. Si utilizas una instancia remota de SQL Server, sin embargo, tendrá que primero debe asegurarse de que al depurar se registran en el equipo de desarrollo con una cuenta de usuario de Windows que tiene un inicio de sesión de SQL Server en la instancia remota. Moveover, este inicio de sesión de base de datos y el inicio de sesión de base de datos que se usa para conectarse a la base de datos de la aplicación ASP.NET en ejecución debe ser miembros de los `sysadmin` rol. Ver los objetos de base de datos de T-SQL depuración en la sección de instancias remotas al final de este tutorial para obtener más información acerca de cómo configurar Visual Studio y SQL Server para depurar una instancia remota.

Por último, comprender que la compatibilidad de depuración para objetos de base de datos de T-SQL no es tan variado como compatibilidad de depuración para aplicaciones .NET. Por ejemplo, condiciones de punto de interrupción y los filtros no son compatibles, únicamente un subconjunto de las ventanas de depuración están disponibles, no se puede usar Editar y continuar, se representa la ventana Inmediato inservible y así sucesivamente. Vea [las limitaciones de características y comandos del depurador](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) para obtener más información.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Paso 1: Entrar directamente en un procedimiento almacenado

Visual Studio resulta fácil de depurar directamente un objeto de base de datos. Permiten s mire cómo usar la característica de depuración de base de datos directa (DDD) para ir a la `Products_SelectByCategoryID` procedimiento almacenado en la base de datos Northwind. Como su nombre implica, `Products_SelectByCategoryID` devuelve información de producto de una categoría determinada; se creó en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial. Iniciar desde el Explorador de servidores y expanda el nodo de base de datos Northwind. A continuación, profundizar en la carpeta procedimientos almacenados, haga doble clic en el `Products_SelectByCategoryID` procedimiento almacenado y elija la opción de ir a procedimiento almacenado en el menú contextual. Se iniciará al depurador.

Puesto que la `Products_SelectByCategoryID` procedimiento almacenado espera un `@CategoryID` parámetro de entrada, se nos pide que proporcionar este valor. Escriba 1, que se devolverá información sobre las bebidas.


![Utilice el valor 1 para el @CategoryID parámetro](debugging-stored-procedures-cs/_static/image1.png)

**Figura 1**: utilice el valor 1 para el `@CategoryID` parámetro


Después de suministrar el valor de la `@CategoryID` , el procedimiento almacenado se ejecuta. Sin embargo, en lugar de ejecutarse hasta su finalización, el depurador detiene la ejecución en la primera instrucción. Tenga en cuenta la flecha amarilla en el margen, que indica la ubicación actual en el procedimiento almacenado. Puede ver y editar los valores de parámetro a través de la ventana Inspección o mantenga el puntero sobre el nombre del parámetro en el procedimiento almacenado.


[![El depurador se detiene en la primera instrucción del procedimiento almacenado](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**Figura 2**: el depurador se detiene en la primera instrucción del procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image4.png))


Para conocer los pasos a través de la instrucción de un procedimiento almacenado a la vez, haga clic en el botón paso a paso por la barra de herramientas o presionar la tecla F10. El `Products_SelectByCategoryID` procedimiento almacenado contiene un único `SELECT` instrucción, por lo que presionar F10 recorre paso a paso por la instrucción única y complete la ejecución del procedimiento almacenado. Cuando haya finalizado el procedimiento almacenado, el resultado aparecerá en la ventana de salida y se cerrará el depurador.

> [!NOTE]
> Depuración de T-SQL se produce en el nivel de instrucción; no se puede ir a un `SELECT` instrucción.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Paso 2: Configuración del sitio Web para depurar la aplicación

Al depurar un procedimiento almacenado directamente desde el Explorador de servidores es útil, en muchos escenarios se está más interesados en depurar el procedimiento almacenado cuando se llama desde la aplicación de ASP.NET. Podemos agregar puntos de interrupción a un procedimiento almacenado desde dentro de Visual Studio y, a continuación, empiece a depurar la aplicación de ASP.NET. Cuando se invoca un procedimiento almacenado con los puntos de interrupción de la aplicación, se detendrá la ejecución en el punto de interrupción y podemos ver y cambiar los valores de parámetro de procedimiento almacenado s y recorre paso a paso sus instrucciones, al igual que hicimos en el paso 1.

Antes de que podemos empezar a depurar procedimientos almacenados que se llama desde la aplicación, es necesario indicar a la aplicación web ASP.NET para integrar con el depurador de SQL Server. Iniciar con el botón secundario en el nombre del sitio Web en el Explorador de soluciones (`ASPNET_Data_Tutorial_74_CS`). Elija la opción de páginas de propiedades en el menú contextual, seleccione el elemento de opciones de inicio de la izquierda y Active la casilla de SQL Server en la sección de depuradores (consulte la figura 3).


[![Active la casilla SQL Server en las páginas de propiedades de aplicación s](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**Figura 3**: Active la casilla de SQL Server en las páginas de propiedades de asignación de aplicación ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image7.png))


Además, deberá actualizar la cadena de conexión de base de datos utilizada por la aplicación para que la agrupación de conexiones está deshabilitada. Cuando se cierra una conexión a una base de datos, la correspondiente `SqlConnection` objeto se coloca en un grupo de conexiones disponibles. Al establecer una conexión a una base de datos, un objeto de conexión disponibles se pueden recuperar de este grupo en lugar de tener que crear y establecer una nueva conexión. Esta agrupación de objetos de conexión es una mejora del rendimiento y está habilitada de forma predeterminada. Sin embargo, al depurar que queremos desactivar la agrupación de conexiones, dado que la infraestructura de depuración no se restablece correctamente cuando se trabaja con una conexión que se tomó del grupo.

Agrupación de conexiones deshabilitada, actualizar el `NORTHWNDConnectionString` en `Web.config` para que incluya la configuración de `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> Cuando haya terminado de depuración de SQL Server a través de la aplicación ASP.NET Asegúrese de volver a aplicar la agrupación de conexiones mediante la eliminación de la `Pooling` de la cadena de conexión (o si se establece en `Pooling=true` ).


En este momento la aplicación de ASP.NET se ha configurado para permitir que Visual Studio depurar objetos de base de datos de SQL Server cuando se invoca a través de la aplicación web. Lo único que queda ahora es agregar un punto de interrupción a un procedimiento almacenado e iniciar la depuración.

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Paso 3: Agregar un punto de interrupción y depuración

Abra la `Products_SelectByCategoryID` procedimiento almacenado y establezca un punto de interrupción al principio de la `SELECT` instrucción haciendo clic en el margen en el lugar adecuado o colocando el cursor al principio de la `SELECT` instrucción y presionar la tecla F9. Como se muestra en la figura 4, el punto de interrupción aparece como un círculo rojo en el margen.


[![Establecer un punto de interrupción en el Products_SelectByCategoryID procedimiento almacenado](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**Figura 4**: establecer un punto de interrupción en la `Products_SelectByCategoryID` procedimiento almacenado ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image10.png))


En el orden de un objeto de base de datos SQL que desea depurar a través de una aplicación cliente, es imperativo que la base de datos se configura para admitir la depuración de la aplicación. Al establecer un punto de interrupción, automáticamente se debe cambiar esta configuración, pero es recomendable comprobar de nuevo. Haga doble clic en el `NORTHWND.MDF` nodo en el Explorador de servidores. El menú contextual debe incluir un comprobado elemento de menú denominado depurar la aplicación.


![Asegúrese de que está habilitada la opción de depuración de aplicaciones](debugging-stored-procedures-cs/_static/image11.png)

**Figura 5**: asegúrese de que está habilitada la opción de depuración de aplicaciones


Con el conjunto de puntos de interrupción y la opción de depuración de la aplicación habilitada, estamos preparados para depurar el procedimiento almacenado cuando se llama desde la aplicación de ASP.NET. Iniciar al depurador, vaya al menú Depurar y elegir Iniciar depuración, al presionar F5 o haciendo clic en el color verde icono reproducir en la barra de herramientas. Esto inicia al depurador e iniciar el sitio Web.

El `Products_SelectByCategoryID` se creó el procedimiento almacenado en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md) tutorial. Su página web correspondiente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene una GridView que muestra los resultados devueltos por este procedimiento almacenado. Visite esta página a través del explorador. Al llegar a la página, el punto de interrupción en el `Products_SelectByCategoryID` se alcanzarán el procedimiento almacenado y devuelve el control a Visual Studio. Al igual que en el paso 1, puede recorrer las instrucciones de s de procedimiento almacenado y la vista y modificar los valores de parámetro.


[![La página ExistingSprocs.aspx muestra inicialmente las bebidas](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**Figura 6**: el `ExistingSprocs.aspx` página muestra inicialmente las bebidas ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image14.png))


[![El procedimiento almacenado s se ha alcanzado el punto de interrupción](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**Figura 7**: s el procedimiento almacenado se ha alcanzado el punto de interrupción ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image17.png))


Como la ventana de inspección en la figura 7 muestra, el valor de la `@CategoryID` del parámetro es 1. Esto es porque el `ExistingSprocs.aspx` página inicialmente muestra los productos en la categoría Bebidas, que tiene un `CategoryID` valor 1. Elija una categoría diferente en la lista desplegable. Si lo hace, provoca una devolución de datos y vuelve a ejecutar la `Products_SelectByCategoryID` procedimiento almacenado. El punto de interrupción, pero esta vez el `@CategoryID` el valor de parámetro s refleja el elemento de lista desplegable seleccionado s `CategoryID`.


[![Elija una categoría diferente en la lista desplegable](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**Figura 8**: elija una categoría diferente en la lista desplegable ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image20.png))


[![El @CategoryID parámetro refleja la categoría seleccionada desde la página Web](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**Figura 9**: el `@CategoryID` parámetro refleja la categoría seleccionada desde la página Web ([haga clic aquí para ver la imagen a tamaño completo](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> Si el punto de interrupción en la `Products_SelectByCategoryID` procedimiento almacenado no se alcanza cuando se visita el `ExistingSprocs.aspx` página, asegúrese de que la casilla de verificación de SQL Server se ha comprobado en la sección de depuradores de la aplicación de ASP.NET s página de propiedades, que ha sido la agrupación de conexiones deshabilitado, y que la base de datos s: opción de depuración de la aplicación está habilitada. Si se van a sigue teniendo problemas, reinicie Visual Studio y vuelva a intentarlo.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Depurar objetos de base de datos de T-SQL en instancias remotas

Depurar objetos de base de datos a través de Visual Studio es bastante sencillo cuando la instancia de base de datos de SQL Server está en el mismo equipo que Visual Studio. Sin embargo, si SQL Server y Visual Studio se encuentran en equipos diferentes, a continuación, una configuración cuidadosa debe hacer que todo funciona correctamente. Hay dos tareas principales que nos enfrentamos con:

- Asegúrese de que el inicio de sesión utilizado para conectarse a la base de datos mediante ADO.NET pertenece a la `sysadmin` rol.
- Asegúrese de que la cuenta de usuario de Windows utilizada por Visual Studio en el equipo de desarrollo es una cuenta válida de inicio de sesión de SQL Server que pertenezca a la `sysadmin` rol.

El primer paso es relativamente sencillo. En primer lugar, identificar la cuenta de usuario utilizada para conectarse a la base de datos de la aplicación ASP.NET y, a continuación, desde SQL Server Management Studio, agregue esa cuenta de inicio de sesión para el `sysadmin` rol.

La segunda tarea requiere que la cuenta de usuario de Windows que se utilizar para depurar la aplicación sea un inicio de sesión válido en la base de datos remota. Sin embargo, lo más probable es que conectado a la estación de trabajo con la cuenta de Windows no es un inicio de sesión válido en SQL Server. En lugar de agregar la cuenta de inicio de sesión determinado a SQL Server, sería una opción mejor designar alguna cuenta de usuario de Windows como la cuenta de depuración de SQL Server. A continuación, para depurar los objetos de base de datos de una instancia de SQL Server remota, podría ejecutar Visual Studio con las credenciales que cuenta s de inicio de sesión de Windows.

Un ejemplo debe ayudar a aclarar cosas. Imagine que hay una cuenta de Windows denominada `SQLDebug` dentro del dominio de Windows. Esta cuenta se debe agregarse a la instancia remota de SQL Server como un inicio de sesión válido, como un miembro de la `sysadmin` rol. A continuación, para depurar la instancia de SQL Server remota desde Visual Studio, se necesita ejecutar Visual Studio como la `SQLDebug` usuario. Esto puede realizarse al cerrar sesión en la estación de trabajo, en el registro como `SQLDebug`, y, a continuación, inicie Visual Studio, pero un enfoque más simple sería iniciar sesión en la estación de trabajo con nuestros propio credenciales y, a continuación, usar `runas.exe` para iniciar Visual Studio como el `SQLDebug` usuario. `runas.exe` permite que una aplicación concreta que se ejecuta en forma de una cuenta de usuario diferente. Para iniciar Visual Studio como `SQLDebug`, puede escribir la siguiente instrucción en la línea de comandos:


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

Para obtener una explicación más detallada acerca de este proceso, consulte [William R. Vaughn](http://betav.com/BLOG/billva/) s *Autoestopista s Guide en Visual Studio y SQL Server, edición séptimo* como [How To: establecer permisos de SQL Server para la depuración](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Si el equipo de desarrollo está ejecutando Windows XP Service Pack 2, debe configurar el Firewall de conexión a Internet para permitir la depuración remota. [Cómo para: habilitar la depuración de SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) artículo notas que esto conlleva dos pasos: (a) en el equipo host de Visual Studio, debe agregar `Devenv.exe` a la lista de excepciones y abrir el puerto TCP 135; y (b) en el equipo remoto de (SQL), debe abrir TCP 135 de puerto y agregue `sqlservr.exe` a la lista de excepciones. Si la directiva de dominio exige que la comunicación de red se realicen a través de IPSec, debe abrir los puertos UDP 4500 y UDP 500.


## <a name="summary"></a>Resumen

Además de proporcionar compatibilidad con la depuración de código de la aplicación. NET, Visual Studio también proporciona una variedad de opciones de depuración para SQL Server 2005. En este tutorial, observamos dos de estas opciones: depuración directa de base de datos y depurar la aplicación. Para depurar directamente un objeto de base de datos de T-SQL, busque el objeto mediante el Explorador de servidores, a continuación, haga doble clic en él y elija Depurar paso a paso. El depurador se inicia y se detiene en la primera instrucción en el objeto de base de datos, momento en que puede recorrer las instrucciones de s de objeto y la vista y modificar valores de parámetro. En el paso 1 se utiliza este enfoque para depurar paso a paso el `Products_SelectByCategoryID` procedimiento almacenado.

Depuración de la aplicación permite a los puntos de interrupción se pueden establecer directamente en los objetos de base de datos. Cuando un objeto de base de datos con puntos de interrupción se invoca desde una aplicación de cliente (por ejemplo, una aplicación web ASP.NET), el programa se detiene como asume el depurador. Depuración de la aplicación es útil porque muestra con más claridad qué acción de aplicación provoca que un objeto de base de datos concreta que se debe invocar. Sin embargo, requiere un poco más de configuración y el programa de instalación de la depuración directa de base de datos.

También se pueden depurar objetos de base de datos a través de los proyectos de SQL Server. Examinaremos usando proyectos de SQL Server y cómo utilizarlas para crear y depurar los objetos de base de datos administrados en el tutorial siguiente.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](protecting-connection-strings-and-other-configuration-information-cs.md)
> [Siguiente](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
