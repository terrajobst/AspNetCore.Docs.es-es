---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: Depurar procedimientos almacenados (VB) | Microsoft Docs
author: rick-anderson
description: Las ediciones de Visual Studio Professional y Team System permiten establecer puntos de interrupción y se encargan de los procedimientos almacenados en SQL Server, facilitando la depuración almacenado...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: 67714284c44c7bc9b06dc599601e116b83017e3e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402010"
---
<a name="debugging-stored-procedures-vb"></a>Depurar procedimientos almacenados (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar código](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip) o [descargar PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Las ediciones de Visual Studio Professional y Team System permiten establecer puntos de interrupción y se encargan de los procedimientos almacenados en SQL Server, facilitando la depuración de procedimientos almacenados tan sencillo como depurarlas en código de la aplicación. Este tutorial muestra la depuración de la base de datos directa y depuración de procedimientos almacenados de la aplicación.


## <a name="introduction"></a>Introducción

Visual Studio proporciona una experiencia de depuración. Con unas pocas pulsaciones de teclas o los clics del mouse, él s posible usar puntos de interrupción para detener la ejecución de un programa y examinar su estado y control de flujo. Junto con la depuración de código de la aplicación, Visual Studio ofrece compatibilidad para depurar procedimientos almacenados desde SQL Server. Al igual que se pueden establecer puntos de interrupción en el código de una clase de código subyacente ASP.NET o una clase de la capa de lógica empresarial, por lo que también se pueden colocar dentro de procedimientos almacenados.

En este tutorial veremos ejecución paso a paso en los procedimientos almacenados desde el Explorador de servidores dentro de Visual Studio, así como cómo establecer puntos de interrupción que se alcancen cuando se llama al procedimiento almacenado desde la aplicación ASP.NET en ejecución.

> [!NOTE]
> Desafortunadamente, sólo se pueden ejecutar paso a paso y depurar a través de las versiones Professional y sistemas de equipo de Visual Studio los procedimientos almacenados. Si usa la versión estándar de Visual Studio o de Visual Web Developer, es para leer el contenido tal como se describen los pasos necesarios para depurar procedimientos almacenados, pero no podrá replicar estos pasos en el equipo.


## <a name="sql-server-debugging-concepts"></a>Conceptos de depuración de SQL Server

Microsoft SQL Server 2005 se diseñó para proporcionar integración con el [Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx), que es el tiempo de ejecución utilizado por todos los ensamblados. NET. Por lo tanto, SQL Server 2005 admite objetos de base de datos administrados. Es decir, puede crear objetos de base de datos como procedimientos almacenados y funciones definidas por el usuario (UDF) como métodos en una clase de Visual Basic. Esto permite que estos procedimientos almacenados y UDF para utilizar la funcionalidad de .NET Framework y de sus propias clases personalizadas. Por supuesto, SQL Server 2005 también proporciona compatibilidad para los objetos de base de datos de Transact-SQL.

SQL Server 2005 ofrece compatibilidad con la depuración de Transact-SQL y objetos de base de datos administrados. Sin embargo, estos objetos solo se pueden depurar a través de las ediciones de Visual Studio 2005 Professional y sistemas de equipo. En este tutorial, examinaremos los objetos de base de datos de depuración de T-SQL. El tutorial posterior examina depurar objetos de base de datos administrados.

El [información general de Transact-SQL y depuración de CLR en SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx) entrada de blog de la [equipo de integración de CLR de SQL Server 2005](https://blogs.msdn.com/sqlclr/default.aspx) resalta las tres formas de depurar objetos de SQL Server 2005 desde Visual Studio:

- **Dirigir la base de datos de depuración (DDD)** : en el Explorador de servidores, podemos ir a cualquier objeto de base de datos de Transact-SQL, como procedimientos almacenados y UDF. Examinaremos DDD en el paso 1.
- **Depuración de aplicación** -podemos establecer puntos de interrupción dentro de un objeto de base de datos y, a continuación, ejecutar la aplicación de ASP.NET. Cuando se ejecuta el objeto de base de datos, el punto de interrupción se alcanzarán y control traspase al depurador. Tenga en cuenta que con la depuración de aplicaciones no podemos entra en un objeto de base de datos desde el código de aplicación. Debemos expresamente configuramos los puntos de interrupción en esos procedimientos almacenados o las UDF donde queremos que el depurador se detenga. Depuración de la aplicación se examina comenzando en el paso 2.
- **Depuración desde un proyecto de SQL Server** -las ediciones de Visual Studio Professional y sistemas de equipo son un tipo de proyecto de SQL Server que se usa normalmente para crear objetos de base de datos administrados. Examinaremos con proyectos de SQL Server y la depuración de su contenido en el siguiente tutorial.

Visual Studio puede depurar procedimientos almacenados en instancias de SQL Server locales y remotos. Una instancia de SQL Server local es aquella que está instalado en el mismo equipo que Visual Studio. Si no se encuentra la base de datos de SQL Server que se usa en el equipo de desarrollo, se considera una instancia remota. Estos tutoriales que hemos usado las instancias locales de SQL Server. Depurar procedimientos almacenados en una instancia remota de SQL server requiere más pasos de configuración que al depurar procedimientos almacenados en una instancia local.

Si usa una instancia de SQL Server local, puede comenzar con el paso 1 y trabajar con este tutorial al final. Si usa una instancia remota de SQL Server, sin embargo, tendrá que primero es preciso asegurarse de que al depurar se registran en el equipo de desarrollo con una cuenta de usuario de Windows que tiene un inicio de sesión de SQL Server en la instancia remota. Moveover, este inicio de sesión de base de datos y el inicio de sesión de base de datos que se usa para conectarse a la base de datos de la aplicación ASP.NET en ejecución debe ser miembros de la `sysadmin` rol. Ver los objetos de base de datos de T-SQL depuración en la sección de instancias remotas al final de este tutorial para obtener más información sobre cómo configurar Visual Studio y SQL Server para depurar una instancia remota.

Por último, comprender que compatibilidad de depuración para los objetos de base de datos de Transact-SQL no es como característica enriquecido como soporte para aplicaciones .NET de depuración. Por ejemplo, filtros y las condiciones de punto de interrupción no se admiten, solo un subconjunto de las ventanas de depuración están disponibles, no se puede usar Editar y continuar, se representa la ventana Inmediato inútil y así sucesivamente. Consulte [limitaciones de las características y comandos del depurador](https://msdn.microsoft.com/library/ms165035(VS.80).aspx) para obtener más información.

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>Paso 1: Entrar directamente en un procedimiento almacenado

Visual Studio facilita la depuración directamente un objeto de base de datos. Permiten s veamos cómo usar la característica de depuración de base de datos directa (DDD) paso a paso por la `Products_SelectByCategoryID` procedimiento almacenado en la base de datos Northwind. Como su nombre implica, `Products_SelectByCategoryID` devuelve información de producto de una categoría determinada; que se creó el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. Iniciar desde el Explorador de servidores y expanda el nodo de base de datos Northwind. A continuación, profundizar en la carpeta procedimientos almacenados, haga doble clic en el `Products_SelectByCategoryID` procedimiento almacenado y elija la opción de ir a procedimiento almacenado en el menú contextual. Esto iniciará al depurador.

Puesto que la `Products_SelectByCategoryID` procedimiento almacenado espera un `@CategoryID` parámetro de entrada, se nos pide que proporcionar este valor. Escriba 1, lo que se devolverá información acerca de las bebidas.


![Use el valor 1 para el @CategoryID parámetro](debugging-stored-procedures-vb/_static/image1.png)

**Figura 1**: Use el valor 1 para el `@CategoryID` parámetro


Después de suministrar el valor de la `@CategoryID` parámetro, el procedimiento almacenado se ejecuta. Sin embargo, en lugar de ejecutarse hasta su finalización, el depurador detiene la ejecución en la primera instrucción. Tenga en cuenta la flecha amarilla en el margen, que indica la ubicación actual en el procedimiento almacenado. Puede ver y editar los valores de parámetro a través de la ventana Inspección o mantenga el puntero sobre el nombre del parámetro del procedimiento almacenado.


[![El depurador se detiene en la primera instrucción del procedimiento almacenado](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**Figura 2**: el depurador se detiene en la primera instrucción del procedimiento almacenado ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image4.png))


Para seguir los pasos de la instrucción de un procedimiento almacenado a la vez, haga clic en el botón paso a través de la barra de herramientas o presione la tecla F10. El `Products_SelectByCategoryID` procedimiento almacenado contiene una sola `SELECT` instrucción, por lo que presionar F10 se paso por procedimientos la instrucción única y completar la ejecución del procedimiento almacenado. Cuando haya finalizado el procedimiento almacenado, su salida aparecerán en la ventana de salida y el depurador se cerrará.

> [!NOTE]
> Depuración de T-SQL se produce en el nivel de instrucción; no puede recorrer un `SELECT` instrucción.


## <a name="step-2-configuring-the-website-for-application-debugging"></a>Paso 2: Configurar el sitio Web para depurar la aplicación

Aunque es útil depurar un procedimiento almacenado directamente desde el Explorador de servidores, en muchos escenarios estamos más interesados en la depuración del procedimiento almacenado cuando se llama desde nuestra aplicación de ASP.NET. Podemos agregar puntos de interrupción a un procedimiento almacenado desde dentro de Visual Studio y, a continuación, iniciar la depuración de la aplicación ASP.NET. Cuando se invoca un procedimiento almacenado con los puntos de interrupción de la aplicación, se detendrá la ejecución en el punto de interrupción y podemos ver y cambiar los valores de parámetro de procedimiento almacenado s y recorrer sus Estados, tal como se hizo en el paso 1.

Antes de que podemos comenzar a depurar procedimientos almacenados llamados desde la aplicación, es necesario indicar a la aplicación web ASP.NET para integrarse con el depurador de SQL Server. Iniciar con el botón secundario en el nombre del sitio Web en el Explorador de soluciones (`ASPNET_Data_Tutorial_74_VB`). Elija la opción de páginas de propiedades en el menú contextual, seleccione el elemento de opciones de inicio de la izquierda y Active la casilla de SQL Server en la sección de depuradores (consulte la figura 3).


[![Active la casilla SQL Server en las páginas de propiedades de s aplicación](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**Figura 3**: Active la casilla de SQL Server en las páginas de propiedades de aplicación s ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image7.png))


Además, es necesario actualizar la cadena de conexión de base de datos utilizada por la aplicación para que la agrupación de conexiones está deshabilitada. Cuando se cierra una conexión a una base de datos, la correspondiente `SqlConnection` objeto se coloca en un grupo de conexiones disponibles. Al establecer una conexión a una base de datos, un objeto de conexión disponibles se pueden recuperar de este grupo en lugar de tener que crear y establecer una conexión nueva. Esta agrupación de objetos de conexión es una mejora del rendimiento y está habilitada de forma predeterminada. Sin embargo, al depurar queremos desactivar la agrupación de conexiones, dado que la infraestructura de depuración no se restablece correctamente cuando se trabaja con una conexión que se ha tomado de la agrupación.

Agrupación de conexiones deshabilitada, actualice el `NORTHWNDConnectionString` en `Web.config` para que incluya la configuración `Pooling=false` .


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> Cuando haya terminado la depuración de SQL Server a través de la aplicación ASP.NET Asegúrese de restablecer la agrupación de conexiones mediante la eliminación de la `Pooling` de la cadena de conexión (o si se establece en `Pooling=true` ).


En este momento la aplicación ASP.NET se configuró para permitir que Visual Studio depurar objetos de base de datos de SQL Server cuando se invoca a través de la aplicación web. Todo lo que queda ahora es agregar un punto de interrupción a un procedimiento almacenado e iniciar la depuración!

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>Paso 3: Agregar un punto de interrupción y depuración

Abra el `Products_SelectByCategoryID` procedimiento almacenado y establezca un punto de interrupción al principio de la `SELECT` instrucción haciendo clic en el margen en el lugar adecuado, o colocando el cursor al principio de la `SELECT` instrucción y presionar F9. Como se muestra en la figura 4, el punto de interrupción aparece como un círculo rojo en el margen.


[![Establecer un punto de interrupción en el Products_SelectByCategoryID procedimiento almacenado](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**Figura 4**: establecer un punto de interrupción en el `Products_SelectByCategoryID` Stored Procedure ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image10.png))


Para un objeto de base de datos SQL que se desea depurar a través de una aplicación cliente, es imperativo que la base de datos se ha configurado para admitir la depuración de la aplicación. Al establecer un punto de interrupción, automáticamente se debe cambiar esta configuración, pero es recomendable comprobar de nuevo. Haga doble clic en el `NORTHWND.MDF` nodo en el Explorador de servidores. El menú contextual debe incluir un menú activado elemento denominado la depuración de la aplicación.


![Asegúrese de que está habilitada la opción de depuración de aplicaciones](debugging-stored-procedures-vb/_static/image11.png)

**Figura 5**: Compruebe que está habilitada la opción de depuración de aplicaciones


Con el conjunto de punto de interrupción y la opción de depuración de la aplicación habilitada, estamos preparados depurar el procedimiento almacenado cuando se llama desde la aplicación ASP.NET. Iniciar al depurador, vaya al menú Depurar y elegir iniciar la depuración, presionando F5 o haciendo clic en verde el icono de reproducción en la barra de herramientas. Se iniciará al depurador y el sitio Web de inicio.

El `Products_SelectByCategoryID` se creó el procedimiento almacenado en el [utilizando procedimientos almacenados existentes para la s de conjunto de datos con tipo TableAdapters](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md) tutorial. Su página web correspondiente (`~/AdvancedDAL/ExistingSprocs.aspx`) contiene una GridView que muestra los resultados devueltos por este procedimiento almacenado. Visite esta página a través del explorador. Al llegar a la página, el punto de interrupción en el `Products_SelectByCategoryID` se alcanzarán el procedimiento almacenado y devuelve el control a Visual Studio. Al igual que en el paso 1, puede recorrer las instrucciones de s de procedimiento almacenado y la vista y modificar los valores de parámetro.


[![La página ExistingSprocs.aspx muestra inicialmente las bebidas](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**Figura 6**: el `ExistingSprocs.aspx` página muestra inicialmente las bebidas ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image14.png))


[![La s de procedimiento almacenado se ha alcanzado el punto de interrupción](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**Figura 7**: s el procedimiento almacenado se ha alcanzado el punto de interrupción ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image17.png))


Como la ventana Inspección en la figura 7 muestra, el valor de la `@CategoryID` del parámetro es 1. Esto es porque el `ExistingSprocs.aspx` página inicialmente muestra productos de la categoría Bebidas, que tiene un `CategoryID` valor 1. Elija una categoría diferente de la lista desplegable. Si lo hace, se produce un postback y se vuelve a ejecutar el `Products_SelectByCategoryID` procedimiento almacenado. El punto de interrupción, pero esta vez el `@CategoryID` valor del parámetro s refleja el elemento de lista desplegable seleccionado s `CategoryID`.


[![Elija una categoría diferente de la lista desplegable](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**Figura 8**: elija una categoría diferente de la lista desplegable ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image20.png))


[![El @CategoryID parámetro refleja la categoría seleccionada en la página Web](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**Figura 9**: el `@CategoryID` parámetro refleja la categoría seleccionada en la página Web ([haga clic aquí para ver imagen en tamaño completo](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> Si el punto de interrupción en el `Products_SelectByCategoryID` no se encuentra el procedimiento almacenado cuando se visita el `ExistingSprocs.aspx` página, asegúrese de que esté activada la casilla de verificación de SQL Server en la sección de los depuradores de la aplicación ASP.NET s página de propiedades, que ha sido la agrupación de conexiones deshabilitado y que está habilitada la opción de depuración de la aplicación de s de la base de datos. Si re sigue teniendo problemas, reinicie Visual Studio e inténtelo de nuevo.


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>Depurar objetos de base de datos de Transact-SQL en instancias remotas

Depurar objetos de base de datos a través de Visual Studio es bastante sencillo cuando la instancia de base de datos de SQL Server está en el mismo equipo que Visual Studio. Sin embargo, si SQL Server y Visual Studio se encuentran en distintos equipos se requiere una configuración cuidadosa para que todo funcione correctamente. Hay dos tareas principales que nos enfrentamos con:

- Asegúrese de que el inicio de sesión utilizado para conectarse a la base de datos a través de ADO.NET pertenece a la `sysadmin` rol.
- Asegúrese de que la cuenta de usuario de Windows que usa Visual Studio en el equipo de desarrollo es una cuenta válida de inicio de sesión de SQL Server que pertenece el `sysadmin` rol.

El primer paso es relativamente sencillo. En primer lugar, identificar la cuenta de usuario utilizada para conectarse a la base de datos de la aplicación ASP.NET y, a continuación, desde SQL Server Management Studio, agregue esa cuenta de inicio de sesión a la `sysadmin` rol.

La segunda tarea requiere que la cuenta de usuario de Windows que usa para depurar la aplicación sea un inicio de sesión válido en la base de datos remoto. Sin embargo, lo más probable es que la cuenta de Windows, iniciado sesión en su estación de trabajo no es un inicio de sesión válido en SQL Server. En lugar de agregar la cuenta de inicio de sesión determinado a SQL Server, una mejor opción sería designar una cuenta de usuario de Windows como la cuenta de depuración de SQL Server. A continuación, para depurar los objetos de base de datos de una instancia remota de SQL Server, ejecutaría con ese credenciales de cuenta s de inicio de sesión de Windows de Visual Studio.

Un ejemplo ayudará a aclarar las cosas. Imagine que hay una cuenta de Windows denominada `SQLDebug` dentro del dominio de Windows. Esta cuenta deberá agregarse a la instancia remota de SQL Server como un inicio de sesión válido, como un miembro de la `sysadmin` rol. A continuación, para depurar la instancia remota de SQL Server desde Visual Studio, se deberá ejecutar Visual Studio como el `SQLDebug` usuario. Esto puede hacerse mediante cerrar sesión en nuestra estación de trabajo, vuelven a iniciar sesión como `SQLDebug`, y, a continuación, inicie Visual Studio, pero un enfoque más sencillo sería iniciar sesión en nuestra estación de trabajo con nuestras propias credenciales y, a continuación, usar `runas.exe` para iniciar Visual Studio como el `SQLDebug` usuario. `runas.exe` permite que una aplicación concreta que se ejecutará en forma de una cuenta de usuario diferente. Para iniciar Visual Studio como `SQLDebug`, puede escribir la siguiente instrucción en la línea de comandos:


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

Para obtener una explicación más detallada sobre este proceso, consulte [William R. Vaughn](http://betav.com/BLOG/billva/) s *Hitchhiker s guía para Visual Studio y SQL Server, la séptima edición* como [Cómo: establecer permisos de SQL Server para la depuración](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx).

> [!NOTE]
> Si el equipo de desarrollo se está ejecutando Windows XP Service Pack 2, deberá configurar el Firewall de conexión a Internet para permitir la depuración remota. [El procedimiento para: habilitar la depuración de SQL Server 2005](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx) artículo indica que esto implica dos pasos: (a) en el equipo host de Visual Studio, debe agregar `Devenv.exe` a la lista de excepciones y abrir el puerto TCP 135; y (b) en el equipo remoto de (SQL), debe abrir el TCP 135 de puerto y agregue `sqlservr.exe` a la lista de excepciones. Si la directiva de dominio requiere que las comunicaciones se realicen a través de IPSec, debe abrir los puertos UDP 4500 y 500.


## <a name="summary"></a>Resumen

Además de proporcionar compatibilidad con la depuración de código de la aplicación. NET, Visual Studio también proporciona una variedad de opciones de depuración para SQL Server 2005. En este tutorial explicamos dos de estas opciones: depuración directa de la base de datos y depurar la aplicación. Para depurar directamente un objeto de base de datos de Transact-SQL, busque el objeto mediante el Explorador de servidores, a continuación, haga doble clic en él y elija Depurar paso a paso. Esto inicia al depurador y se detiene en la primera instrucción en el objeto de base de datos, momento en que puede recorrer las instrucciones de s de objeto y ver y modificar valores de parámetro. En el paso 1, usamos este enfoque paso a paso por la `Products_SelectByCategoryID` procedimiento almacenado.

Depuración de la aplicación permite a los puntos de interrupción se puede establecer directamente dentro de los objetos de base de datos. Cuando un objeto de base de datos con puntos de interrupción se invoca desde una aplicación de cliente (por ejemplo, una aplicación web ASP.NET), el programa se detiene como asume el depurador. Depuración de la aplicación es útil porque muestra con mayor claridad qué acción de aplicación provoca que un objeto de base de datos concreta que se debe invocar. Sin embargo, requiere la instalación y configuración de depuración directa de la base de datos un poco más.

También se pueden depurar objetos de base de datos a través de los proyectos de SQL Server. Veremos mediante proyectos de SQL Server y cómo usarlas para crear y depurar administra objetos de base de datos en el siguiente tutorial.

Feliz programación.

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), trabaja con tecnologías Web de Microsoft desde 1998. Scott trabaja como consultor independiente, instructor y escritor. Su último libro es [*SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que puede encontrarse en [ http://ScottOnWriting.NET ](http://ScottOnWriting.NET).

> [!div class="step-by-step"]
> [Anterior](protecting-connection-strings-and-other-configuration-information-vb.md)
> [Siguiente](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
