---
uid: web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
title: "Creación de una capa de acceso a datos (C#) | Documentos de Microsoft"
author: rick-anderson
description: "En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que con los conjuntos de datos, para tener acceso a la información en una base de datos."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/05/2010
ms.topic: article
ms.assetid: cfe2a6a0-1e56-4dc8-9537-c8ec76ba96a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/introduction/creating-a-data-access-layer-cs
msc.type: authoredcontent
ms.openlocfilehash: c610f84cfb82f38f9c67b757aa341c7a1497369c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-data-access-layer-c"></a>Creación de una capa de acceso a datos (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar la aplicación de ejemplo](http://download.microsoft.com/download/4/6/3/463cf87c-4724-4cbc-b7b5-3f866f43ba50/ASPNET_Data_Tutorial_1_CS.exe) o [descarga de PDF](creating-a-data-access-layer-cs/_static/datatutorial01cs1.pdf)

> En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que con los conjuntos de datos, para tener acceso a la información en una base de datos.


## <a name="introduction"></a>Introducción

Como los desarrolladores web, nuestras vidas giran en torno a trabajar con datos. Creamos las bases de datos para almacenar los datos, el código para recuperar y modificar la base de datos y páginas web para recopilar y resumirlos. Este es el primer tutorial de una serie larga que se explorará técnicas para implementar estos patrones comunes en ASP.NET 2.0. Comenzaremos con la creación de un [arquitectura de software](http://en.wikipedia.org/wiki/Software_architecture) compuesto de una capa de acceso de datos (DAL) con los conjuntos de datos de tipo, una capa de lógica empresarial (BLL) que aplica reglas de negocios y páginas de una capa de presentación que se compone de ASP.NET que compartir un diseño de página común. Una vez que se ha diseñado este fundamento de back-end, se moverá en informes, que muestra cómo mostrar, resumir, recopilar y validar los datos desde una aplicación web. Estos tutoriales están pensados para ser conciso y se proporcionan instrucciones paso a paso con una gran cantidad de capturas de pantalla para guiarle a través del proceso visualmente. Cada tutorial está disponible en versiones de Visual Basic y C# e incluye la descarga de código completo usado. (Este primer tutorial es bastante larga, pero el resto se presentan en fragmentos por mucho más).

Para estos tutoriales usaremos una versión de Microsoft SQL Server 2005 Express Edition de la base de datos de Northwind que se coloca en el **aplicación\_datos** directory. Además del archivo de base de datos, el **aplicación\_datos** carpeta también contiene los scripts SQL para crear la base de datos, en caso de que desea usar una versión diferente de la base de datos. Estos scripts se también se pueden [descargan directamente desde Microsoft](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en), si lo prefiere. Si usa una versión diferente de SQL Server de la base de datos Northwind, debe actualizar el **NORTHWNDConnectionString** configuración de la aplicación **Web.config** archivo. La aplicación web se creó con Visual Studio 2005 Professional Edition como un proyecto de sitio Web basado en el sistema de archivos. Sin embargo, todos los tutoriales funcione igual de bien con la versión gratuita de Visual Studio 2005, [Visual Web Developer](https://msdn.microsoft.com/vstudio/express/vwd/).  
  
En este tutorial comenzaremos desde el principio y crear la capa de acceso de datos (DAL), que seguido de creación de la capa de lógica empresarial (BLL) en el segundo tutorial y trabajar en el diseño de página y la navegación en el tercer argumento. Los tutoriales después de la tercera uno se compilará en la base se disponen en los tres primeros. Tenemos mucho para abarcarlas en este primer tutorial, por lo que desencadenará Visual Studio y Comencemos!

## <a name="step-1-creating-a-web-project-and-connecting-to-the-database"></a>Paso 1: Crear un proyecto Web y conectarse a la base de datos

Antes de que podemos crear la capa de acceso a datos (DAL), primero es necesario crear un sitio web y configurar la base de datos. Empiece por crear un nuevo sitio de web ASP.NET basada en sistema de archivos. Para ello, vaya al menú archivo y elija Nuevo sitio Web, mostrar el cuadro de diálogo nuevo sitio Web. Elija la plantilla de sitio Web ASP.NET, o establecer la lista de la lista desplegable de ubicación al sistema de archivos, elija una carpeta para colocar el sitio web y establecer el idioma en C#.


[![Crear un nuevo archivo basado en sistema sitio Web](creating-a-data-access-layer-cs/_static/image2.png)](creating-a-data-access-layer-cs/_static/image1.png)

**Figura 1**: crear un sitio Web de New File System-Based ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image3.png))


Esto creará un nuevo sitio web con un **Default.aspx** página ASP.NET y un **aplicación\_datos** carpeta.

Con el sitio web creado, el siguiente paso es agregar una referencia a la base de datos en el Explorador de servidores de Visual Studio. Mediante la adición de una base de datos en el Explorador de servidores puede agregar tablas, procedimientos almacenados, vistas y así sucesivamente todo desde dentro de Visual Studio. También puede ver los datos de la tabla o crear sus propias consultas a mano o gráficamente mediante el generador de consultas. Además, cuando se compilación los conjuntos de datos con tipo para la capa DAL necesitaremos a punto de Visual Studio a la base de datos desde el que se debe construir los conjuntos de datos con tipo. Mientras que podemos proporcionar esta información de conexión en ese momento, Visual Studio rellena automáticamente una lista desplegable de las bases de datos ya está registrado en el Explorador de servidores.

Los pasos para agregar la base de datos Northwind en el Explorador de servidores dependen de si desea utilizar la base de datos de SQL Server 2005 Express Edition en el **aplicación\_datos** carpeta o si tiene un Microsoft SQL Server 2000 o 2005 configuración de servidor de bases de datos que desea usar en su lugar.

## <a name="using-a-database-in-theappdatafolder"></a>Usar una base de datos en theApp\_DataFolder

Si no tiene un servidor SQL Server 2000 o 2005 servidor de base de datos para conectarse a, o simplemente desea evitar tener que agregar la base de datos a un servidor de base de datos, puede usar la versión de SQL Server 2005 Express Edition de la base de datos de Northwind que se encuentra en la websit descargado e's **aplicación\_datos** carpeta (**NORTHWND. MDF**).

Una base de datos se coloca en el **aplicación\_datos** carpeta se agrega automáticamente en el Explorador de servidores. Debería ver un nodo denominado NORTHWND suponiendo que tenga instalados en su equipo SQL Server 2005 Express Edition. MDF en el Explorador de servidores, que se puede expandir y explorar sus tablas, vistas, procedimientos almacenados y así sucesivamente (consulte la figura 2).

El **aplicación\_datos** carpeta también puede contener Microsoft Access **.mdb** archivos, lo que, como sus homólogos de SQL Server, se agregan automáticamente en el Explorador de servidores. Si no desea utilizar cualquiera de las opciones de SQL Server, siempre puede [descargar una versión de Microsoft Access del archivo de base de datos de Northwind](https://www.microsoft.com/downloads/details.aspx?FamilyID=C6661372-8DBE-422B-8676-C632D66C529C&amp;displaylang=EN) y colocarlo en la **aplicación\_datos** directory. Tenga en cuenta, sin embargo, que las bases de datos de Access tienen tantas características como SQL Server y no están diseñados para usarse en escenarios de sitio web. Además, un par de los tutoriales de 35 + vaya a usar ciertas características de nivel de base de datos que no están compatible con el acceso.

## <a name="connecting-to-the-database-in-a-microsoft-sql-server-2000-or-2005-database-server"></a>Conectarse a la base de datos en un servidor de base de datos de Microsoft SQL Server 2000 o 2005

O bien, puede conectarse a una base de datos de Northwind instalada en un servidor de base de datos. Si el servidor de base de datos ya no dispone de la base de datos de Northwind instalada, primero debe agregarse al servidor de base de datos mediante la ejecución de la secuencia de comandos de instalación que se incluye en la descarga de este tutorial o por [descargar la versión de SQL Server 2000 de Northwind scripts de instalación](https://www.microsoft.com/downloads/details.aspx?FamilyID=06616212-0356-46a0-8da2-eebc53a68034&amp;DisplayLang=en) directamente desde el sitio web de Microsoft.

Una vez que tenga instalada la base de datos, vaya al explorador de servidores en Visual Studio, haga doble clic en el nodo Conexiones de datos y elija Agregar conexión. Si no ve el Explorador de servidores para que vaya a la vista / Explorador de servidores o llamadas Ctrl + Alt + S. Se abrirá el cuadro de diálogo Agregar conexión, donde puede especificar el servidor al que conectarse, la información de autenticación y el nombre de base de datos. Una vez que ha configurado la información de conexión de base de datos y hace clic en el botón Aceptar, se agregará como un nodo bajo el nodo Conexiones de datos de la base de datos. Puede expandir el nodo de base de datos para explorar sus tablas, vistas, procedimientos almacenados y así sucesivamente.


![Agregar una conexión a la base de datos de su servidor de base de datos Northwind](creating-a-data-access-layer-cs/_static/image4.png)

**Figura 2**: agregar una conexión a la base de datos de su servidor de base de datos Northwind


## <a name="step-2-creating-the-data-access-layer"></a>Paso 2: Crear la capa de acceso a datos

Cuando se trabaja con una opción de datos es incrustar la lógica específica de datos directamente en la capa de presentación (en una aplicación web, la marca de páginas ASP.NET de la capa de presentación). Esto puede adoptar la forma de escribir código ADO.NET en la parte correspondiente al código de la página ASP.NET o usar el control SqlDataSource desde la parte de marcado. En cualquier caso, este enfoque relaciona estrechamente la lógica de acceso a datos con el nivel de presentación. Sin embargo, es el enfoque recomendado separar la lógica de acceso a datos de la capa de presentación. Esta capa independiente se conoce como la capa de acceso a datos, la capa DAL para abreviar y normalmente se implementa como un proyecto de biblioteca de clases independiente. Las ventajas de esta arquitectura por niveles están bien documentadas (consulte la sección "Lecturas adicionales" al final de este tutorial para obtener información sobre las siguientes ventajas) y es el enfoque tomaremos de esta serie.

Todo el código que es específico del origen de datos subyacente como la creación de una conexión a la base de datos, emitir **seleccione**, **insertar**, **actualización**, y  **ELIMINAR** comandos y así sucesivamente deben encontrarse en la capa DAL. El nivel de presentación no debe contener todas las referencias a este código de acceso de datos, pero en su lugar, debe realizar llamadas en la capa DAL para las solicitudes de todos los datos. Niveles de acceso a datos normalmente contienen métodos para tener acceso a la base de datos subyacente. Por ejemplo, tiene la base de datos Northwind, **productos** y **categorías** tablas que registran los productos para la venta y las categorías a la que pertenecen. En nuestro DAL tenemos métodos como:

- **GetCategories(),** que devolverá información sobre todas las categorías
- **GetProducts()**, que devuelve información acerca de todos los productos
- **GetProductsByCategoryID (*categoryID*)**, lo que devolverá todos los productos que pertenecen a una categoría específica
- **GetProductByProductID (*productID*)**, que devolverá información sobre un producto determinado

Estos métodos, cuando se invoca, conéctese a la base de datos, emitir la consulta apropiada y devolver los resultados. ¿Cómo se devuelven estos resultados es importante. Estos métodos pudieron devolver simplemente un conjunto de datos o DataReader rellenado por la consulta de base de datos, pero lo ideal es que se deben devolver estos resultados usando *objetos fuertemente tipados*. Un objeto fuertemente tipado es uno cuyo esquema se define estrictamente en tiempo de compilación, mientras que lo contrario, un objeto débilmente tipadas, es uno cuyo esquema no se conoce hasta el tiempo de ejecución.

Por ejemplo, el DataReader y el conjunto de datos (de forma predeterminada) son objetos de imprecisa puesto que su esquema se define mediante las columnas devueltas por la consulta de base de datos utilizada para rellenarlos. Para obtener acceso a una columna concreta de un objeto DataTable débilmente tipadas que debemos usar sintaxis similar a la:   ***DataTable*. Filas [*índice*] ["*columnName*"]**. La tabla de datos flexible escribiendo en este ejemplo se exhibe por el hecho de que es necesario obtener acceso al nombre de columna mediante una cadena o el índice ordinal. Un DataTable fuertemente tipado, por otro lado, tendrá cada una de sus columnas que se implementan como propiedades, lo que genera código similar: ***DataTable*.Filas[*índice*].*columnName***.

Para devolver objetos fuertemente tipados, los desarrolladores pueden crear sus propios objetos de negocios personalizada o usar conjuntos de datos con tipo. Un objeto de negocios se implementa con el desarrollador como representa una clase cuyas propiedades normalmente reflejan las columnas de la tabla de base de datos subyacente del objeto de negocios. Un conjunto de datos con tipo es una clase generada automáticamente por Visual Studio basado en un esquema de base de datos y cuyos miembros son fuertemente tipada de acuerdo con este esquema. El conjunto de datos con tipo propio consta de las clases que extienden las clases de conjunto de datos de ADO.NET y DataTable, DataRow. Además de tablas de datos fuertemente tipados, conjuntos de datos con tipo ahora también incluyen TableAdapters, que son clases con métodos para rellenar tablas de datos del conjunto de datos y la propagación de modificaciones en las tablas de datos a la base de datos.

> [!NOTE]
> Para obtener más información sobre las ventajas y desventajas del uso de conjuntos de datos con tipo frente a objetos comerciales personalizados, consulte [diseñar componentes de nivel de datos y pasar datos a través de los niveles de](https://msdn.microsoft.com/en-us/library/ms978496.aspx).


Vamos a usar conjuntos de datos fuertemente tipados para la arquitectura de estos tutoriales. La figura 3 ilustra el flujo de trabajo entre los diferentes niveles de una aplicación que usa conjuntos de datos con tipo.


[![Código de acceso de todos los datos es relegado a la capa DAL](creating-a-data-access-layer-cs/_static/image6.png)](creating-a-data-access-layer-cs/_static/image5.png)

**Figura 3**: código de acceso de todos los datos es relegado a la capa DAL ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image7.png))


## <a name="creating-a-typed-dataset-and-table-adapter"></a>Crear un conjunto de datos con tipo y el adaptador de tabla

Para empezar a crear la capa DAL, comenzamos agregando un DataSet con tipo al proyecto. Para ello, haga doble clic en el nodo del proyecto en el Explorador de soluciones y elija Agregar un nuevo elemento. Seleccione la opción de conjunto de datos en la lista de plantillas y asígnele el nombre **Northwind.xsd**.


[![Optar por agregar un nuevo conjunto de datos al proyecto](creating-a-data-access-layer-cs/_static/image9.png)](creating-a-data-access-layer-cs/_static/image8.png)

**Figura 4**: optar por agregar un nuevo conjunto de datos a un proyecto ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image10.png))


Tras hacer clic en Agregar, cuando se le solicite para agregar el conjunto de datos a la **aplicación\_código** carpeta, elija Sí. A continuación, se mostrará el diseñador para el conjunto de datos con tipo y se iniciará el Asistente para configuración de TableAdapter, lo que le permite agregar su primera TableAdapter al conjunto de datos con tipo.

Un conjunto de datos con tipo actúa como una colección fuertemente tipada de datos; se compone de las instancias de DataTable fuertemente tipado, cada uno de los cuales a su vez se compone de instancias de DataRow fuertemente tipada. Se creará una tabla de datos fuertemente tipados para cada una de las tablas de base de datos subyacentes que necesitamos para trabajar con en esta serie de tutoriales. Puede empezar con la creación de una tabla de datos para la **productos** tabla.

Tenga en cuenta que DataTables fuertemente tipada no incluyen toda la información sobre cómo tener acceso a datos de la tabla de base de datos subyacente. Para recuperar los datos para rellenar la tabla de datos, usamos una clase TableAdapter, que funciona como la capa de acceso a datos. Para nuestro **productos** DataTable, lo TableAdapter contendrá los métodos **GetProducts()**,  **GetProductByCategoryID (*categoryID*)**, etc. que se podrá invocar desde la capa de presentación. Rol de la tabla de datos es para que actúe como el objetos fuertemente tipados que se usa para pasar datos entre las capas.

El Asistente para configuración de TableAdapter comienza le pide que seleccione a qué base de datos para trabajar con. La lista desplegable muestra las bases de datos en el Explorador de servidores. Si no se agregó la base de datos Northwind en el Explorador de servidores, puede hacer clic en el botón nueva conexión en este momento para hacerlo.


[![Elija la base de datos Northwind en la lista desplegable](creating-a-data-access-layer-cs/_static/image12.png)](creating-a-data-access-layer-cs/_static/image11.png)

**Figura 5**: elija la base de datos Northwind en la lista desplegable ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image13.png))


Después de seleccionar la base de datos y haga clic en siguiente, se le preguntará si desea guardar la cadena de conexión en el **Web.config** archivo. Si guarda la cadena de conexión que se evitará tener disco duro codificadas en las clases TableAdapter, que simplifica las cosas si cambia la información de cadena de conexión en el futuro. Si decide para guardar la cadena de conexión en el archivo de configuración se coloca en el  **&lt;connectionStrings&gt;**  sección, que puede ser [opcionalmente cifrada](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx) para mejorado seguridad o modificados más adelante a través de la nueva página de propiedades de ASP.NET 2.0 dentro de la herramienta de administración de GUI de IIS, que es más adecuada para los administradores.


[![Guardar la cadena de conexión en Web.config](creating-a-data-access-layer-cs/_static/image15.png)](creating-a-data-access-layer-cs/_static/image14.png)

**Figura 6**: Guardar cadena de conexión en **Web.config** ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image16.png))


A continuación, necesitamos definir el esquema para la primera DataTable fuertemente tipadas y proporcionan el primer método para nuestro TableAdapter que se usará al rellenar el conjunto de datos fuertemente tipados. Estos dos pasos se llevan a cabo simultáneamente mediante la creación de una consulta que devuelve las columnas de la tabla que queremos reflejadas en la tabla de datos. Al final del asistente se asignará un nombre de método para esta consulta. Una vez que se haya realizado antes, este método se puede invocar desde la capa de presentación. El método se ejecuta la consulta definida y rellenar un DataTable fuertemente tipada.

Para empezar a definir la consulta SQL se debemos indicar cómo se desea el TableAdapter que emitir la consulta. Podemos utilizar una instrucción de SQL ad hoc, cree un nuevo procedimiento almacenado o utilizar un procedimiento almacenado existente. Para estos tutoriales, vamos a usar instrucciones SQL ad hoc. Hacer referencia a [Brian Noyes](http://briannoyes.net/)del artículo, [crear una capa de acceso a datos con el Diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner) para obtener un ejemplo del uso de procedimientos almacenados.


[![Consulta los datos mediante una instrucción de SQL Ad Hoc](creating-a-data-access-layer-cs/_static/image18.png)](creating-a-data-access-layer-cs/_static/image17.png)

**Figura 7**: consultar los datos utilizando una instrucción de SQL Ad Hoc ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image19.png))


En este momento se podemos escribir en la consulta SQL manualmente. Al crear el primer método en TableAdapter normalmente desea que la consulta devuelva las columnas que deben expresarse en la tabla de datos correspondiente. Podemos lograr esto mediante la creación de una consulta que devuelve todas las columnas y todas las filas de la **productos** tabla:


[![Escriba la consulta SQL en el cuadro de texto](creating-a-data-access-layer-cs/_static/image21.png)](creating-a-data-access-layer-cs/_static/image20.png)

**Figura 8**: escriba el SQL consulta en el cuadro de texto ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image22.png))


Como alternativa, utilice el generador de consultas y crear gráficamente la consulta, como se muestra en la figura 9.


[![Crear la consulta de forma gráfica, a través del Editor de consultas](creating-a-data-access-layer-cs/_static/image24.png)](creating-a-data-access-layer-cs/_static/image23.png)

**Figura 9**: crear la consulta gráficamente, mediante el Editor de consultas ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image25.png))


Después de crear la consulta, pero antes de pasar a la siguiente pantalla, haga clic en el botón Opciones avanzadas. En los proyectos de sitio Web, "instrucciones generar Insert, Update y Delete" es la única opción seleccionada de forma predeterminada; avanzada Si ejecuta a este asistente desde una biblioteca de clases o un proyecto de Windows también se seleccionará la opción "Usar simultaneidad optimista". Deje desactivada la opción "Usar simultaneidad optimista" por ahora. Examinaremos simultaneidad optimista en tutoriales futuros.


[![Solo el generar Insert, Update y Delete instrucciones SELECT opción](creating-a-data-access-layer-cs/_static/image27.png)](creating-a-data-access-layer-cs/_static/image26.png)

**Figura 10**: solo el generar Insert, Update y Delete instrucciones Select opción ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image28.png))


Después de comprobar las opciones avanzadas, haga clic en siguiente para ir a la última pantalla. Aquí se nos pide que seleccione los métodos para agregar al objeto TableAdapter. Hay dos patrones para rellenar los datos:

- **Rellenar un DataTable** con este enfoque se crea un método que toma una tabla de datos como parámetro y lo rellena basándose en los resultados de la consulta. La clase DataAdapter de ADO.NET, por ejemplo, implemente este patrón con su **Fill()** método.
- **Devolver un DataTable** con este enfoque el método crea y rellena la tabla de datos por usted y lo devuelve como valor devuelven de los métodos.

Puede hacer que el objeto TableAdapter implemente uno o ambos de estos patrones. También puede cambiar el nombre de los métodos que se muestran a continuación. Vamos a deje ambas casillas de verificación está activados, aunque solo usaremos el último modelo a lo largo de estos tutoriales. Además, vamos a cambiar el nombre en su lugar genérico **GetData** método **GetProducts**.

Si se activa, crea la casilla de verificación final, "GenerateDBDirectMethods," **Insert()**, **Update()**, y **Delete()** métodos de TableAdapter. Si deja esta opción no está activada, todas las actualizaciones debe realizarse a través de exclusiva del TableAdapter **Update()** método, que toma en el conjunto de datos con tipo, una tabla de datos, una sola fila de datos o una matriz de objetos DataRow. (Si ha desactivado el "generar Insert, Update y Delete instrucciones" opción de las propiedades avanzadas en la figura 9 esta casilla de verificación configuración no tendrá ningún efecto.) Vamos a dejar esta casilla activada.


[![Cambie el nombre de método GetData por GetProducts](creating-a-data-access-layer-cs/_static/image30.png)](creating-a-data-access-layer-cs/_static/image29.png)

**Figura 11**: cambiar el nombre del método de **GetData** a **GetProducts** ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image31.png))


Complete el asistente, haga clic en Finalizar. Después de que se cierre el asistente se estamos devuelto en el Diseñador de DataSet que se muestra en la tabla de datos que acabamos de crear. Puede ver la lista de columnas en la **productos** DataTable (**ProductID**, **ProductName**, etc.), así como los métodos de la  **ProductsTableAdapter** (**Fill()** y **GetProducts()**).


[![La tabla de datos de productos y ProductsTableAdapter se agregaron al conjunto de datos con tipo](creating-a-data-access-layer-cs/_static/image33.png)](creating-a-data-access-layer-cs/_static/image32.png)

**Figura 12**: el **productos** DataTable y **ProductsTableAdapter** se ha agregado al conjunto de datos con tipo ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image34.png))


En este momento tenemos un conjunto de datos con tipo con una sola tabla de datos (**Northwind.Products**) y una clase fuertemente tipada de DataAdapter (**NorthwindTableAdapters.ProductsTableAdapter**) con un  **GetProducts()** método. Estos objetos se pueden usar para tener acceso a una lista de todos los productos desde código como:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample1.html)]

Este código no era necesario que escribir un bit de código de acceso específico de datos. No se tenía que crear instancias de las clases ADO.NET, no tiene que hacer referencia a las cadenas de conexión, las consultas SQL, o procedimientos almacenados. En su lugar, lo TableAdapter proporciona el código de acceso a datos de bajo nivel para nosotros.

Cada objeto que se usa en este ejemplo también está fuertemente tipado, lo que permite a Visual Studio proporcionar IntelliSense y la comprobación de tipos en tiempo de compilación. Y mejor de DataTables devueltas por el objeto TableAdapter se puede enlazar a datos ASP.NET controles Web, como GridView, DetailsView, DropDownList, CheckBoxList y algunas de las. En el ejemplo siguiente se muestra el enlace el DataTable devuelto por la **GetProducts()** método a un control GridView en solo un exploraciones tres líneas de código dentro de la **página\_carga** controlador de eventos.

AllProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample2.aspx)]

AllProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample3.cs)]


[![Se muestra la lista de productos en un control GridView](creating-a-data-access-layer-cs/_static/image36.png)](creating-a-data-access-layer-cs/_static/image35.png)

**Figura 13**: se muestra la lista de productos en un GridView ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image37.png))


Si bien en este ejemplo es necesario que escribimos tres líneas de código en nuestra página de ASP.NET **página\_carga** controlador de eventos, en el futuro tutoriales examinaremos cómo usar el ObjectDataSource mediante declaración recuperar los datos desde el DAL. Con el ObjectDataSource no se tendrá que escribir ningún código y obtendrá compatibilidad de paginación y ordenación así!

## <a name="step-3-adding-parameterized-methods-to-the-data-access-layer"></a>Paso 3: Agregar parámetros de métodos para la capa de acceso a datos

En este momento nuestro **ProductsTableAdapter** clase tiene pero un método, **GetProducts()**, que devuelve todos los productos de la base de datos. Mientras que la capacidad para trabajar con todos los productos es muy útiles, hay ocasiones cuando es conveniente para recuperar información sobre un producto específico o todos los productos que pertenecen a una categoría determinada. Para agregar esta funcionalidad a la capa de acceso a datos podemos agregar métodos con parámetros al objeto TableAdapter.

Vamos a agregar la  **GetProductsByCategoryID (*categoryID*)** método. Para agregar un nuevo método a la capa DAL, vuelva al diseñador de DataSet, pulse el botón derecho en el **ProductsTableAdapter** sección y elija Agregar consulta.


![Haga doble clic en el objeto TableAdapter y elija Agregar consulta](creating-a-data-access-layer-cs/_static/image38.png)

**Figura 14**: haga doble clic en el objeto TableAdapter y elija Agregar consulta


En primer lugar, se nos pide sobre si desea tener acceso a la base de datos mediante una instrucción de SQL "ad-hoc" o un procedimiento almacenado nuevo o existente. Vamos a optar por usar una instrucción de SQL ad hoc nuevo. A continuación, se nos pide qué tipo de consulta SQL le gustaría utilizar. Puesto que deseamos devolver todos los productos que pertenecen a una determinada categoría, desea escribir un **seleccione** instrucción que devuelve filas.


[![Elija crear una instrucción SELECT que devuelve filas](creating-a-data-access-layer-cs/_static/image40.png)](creating-a-data-access-layer-cs/_static/image39.png)

**Figura 15**: optar por crear un **seleccione** que devuelve filas del informe de ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image41.png))


El paso siguiente es definir la consulta SQL utilizada para tener acceso a los datos. Puesto que deseamos devolver solo los productos que pertenecen a una categoría determinada, usar el mismo **seleccione** instrucción desde **GetProducts()**, pero agregue el siguiente **donde** cláusula: **donde CategoryID = @CategoryID** . El  **@CategoryID**  parámetro indica el Asistente de TableAdapter para que el método estamos creando requerirá un parámetro de entrada del tipo correspondiente (es decir, un entero que acepta valores NULL).


[![Escribir una consulta para devolver sólo los productos para una categoría específica](creating-a-data-access-layer-cs/_static/image43.png)](creating-a-data-access-layer-cs/_static/image42.png)

**Figura 16**: escriba una consulta para devolver sólo los productos de una categoría especificada ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image44.png))


En el paso final podemos elegir que patrones a usar, así como personalizar los nombres de los métodos generados de acceso a datos. Para la trama de relleno, vamos a cambiar el nombre a **FillByCategoryID** y para la devolución de un objeto DataTable retorno patrón (el **obtener*X*** métodos), vamos a usar **GetProductsByCategoryID**.


[![Elija los nombres de los métodos de TableAdapter](creating-a-data-access-layer-cs/_static/image46.png)](creating-a-data-access-layer-cs/_static/image45.png)

**Figura 17**: elija los nombres de los métodos de TableAdapter ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image47.png))


Después de completar al asistente, el Diseñador de DataSet incluye los nuevos métodos de TableAdapter.


![Los productos ahora pueden ser consultadas por categoría](creating-a-data-access-layer-cs/_static/image48.png)

**Figura 18**: The el productos puede ahora ser consultadas por categoría


Tómese un momento para agregar una  **GetProductByProductID (*productID*)** método utilizando la misma técnica.

Estas consultas con parámetros se pueden probar directamente desde el Diseñador de DataSet. Haga doble clic en el método de TableAdapter y elija la vista previa de datos. A continuación, escriba los valores para usar con los parámetros y haga clic en vista previa.


[![Se muestran la pertenencia de esos productos a la categoría de bebidas](creating-a-data-access-layer-cs/_static/image50.png)](creating-a-data-access-layer-cs/_static/image49.png)

**Figura 19**: se muestran los productos que pertenecen a la categoría bebidas ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image51.png))


Con el  **GetProductsByCategoryID (*categoryID*)** método en la capa DAL, ahora podemos crear una página ASP.NET que muestra solo los productos para una categoría específica. El ejemplo siguiente muestra todos los productos que están en la categoría Bebidas, que tienen un **CategoryID** de 1.

Beverages.ASP

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample4.aspx)]

Beverages.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample5.cs)]


[![Se muestran los productos de la categoría Bebidas](creating-a-data-access-layer-cs/_static/image53.png)](creating-a-data-access-layer-cs/_static/image52.png)

**Figura 20**: se muestran los productos de la categoría bebidas ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image54.png))


## <a name="step-4-inserting-updating-and-deleting-data"></a>Paso 4: Insertar, actualizar y eliminar datos

Hay dos patrones que se usa habitualmente para insertar, actualizar y eliminar datos. El primer patrón, que a la que llamaré el patrón directa de la base de datos, implica la creación de métodos que, cuando se invoca, problema una **insertar**, **actualización**, o **eliminar** comando a la base de datos que funciona en un registro único de la base de datos. Estos métodos se pasan normalmente en una serie de valores escalares (enteros, cadenas, valores booleanos, fechas y horas y así sucesivamente) que corresponden a los valores para insertar, actualizar o eliminar. Por ejemplo, con este patrón para el **productos** tabla el método delete tardaría en un parámetro de número entero que indica la **ProductID** del registro que desea eliminar, mientras que el método de inserción se tardaría un la cadena para la **ProductName**, un decimal para la **UnitPrice**, un entero para la **UnitsOnStock**, y así sucesivamente.


[![Cada Insert, Update y Delete de solicitud se envían a la base de datos inmediatamente](creating-a-data-access-layer-cs/_static/image56.png)](creating-a-data-access-layer-cs/_static/image55.png)

**Figura 21**: cada inserción, actualización y eliminar solicitud se envía a la base de datos inmediatamente ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image57.png))


El otro modelo, que hará referencia como el lote de actualizaciones de patrón, consiste en actualizar un DataSet, DataTable o colección de filas de datos en una llamada al método completo. Con este patrón de un desarrollador elimina, inserta, modifica las filas de datos en una tabla de datos y, a continuación, pasa esos objetos DataRow o DataTable a un método de actualización. Este método, a continuación, enumera las filas de datos pasado, determina si o no ha se ha modificado, agregados o eliminar (a través de la DataRow [propiedad RowState](https://msdn.microsoft.com/en-us/library/system.data.datarow.rowstate.aspx) valor) y emite la solicitud de base de datos adecuada para cada registro.


[![Todos los cambios se sincronizan con la base de datos cuando se invoca el método de actualización](creating-a-data-access-layer-cs/_static/image59.png)](creating-a-data-access-layer-cs/_static/image58.png)

**Figura 22**: todos los cambios se sincronizan con la base de datos cuando se invoca el método de actualización ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image60.png))


TableAdapter utiliza el patrón de actualización por lotes de forma predeterminada, pero también admite el patrón directa de la base de datos. Puesto que hemos seleccionado la opción "generar Insert, Update y Delete instrucciones" desde las propiedades avanzadas al crear nuestra TableAdapter, el **ProductsTableAdapter** contiene un **Update()** (método), que implementa el patrón de actualización por lotes. En concreto, lo TableAdapter contiene un **Update()** método que se puede pasar el conjunto de datos con tipo, una tabla de datos fuertemente tipados o uno o más filas de datos. Si deja activa la casilla de "GenerateDBDirectMethods" al crear por primera vez el objeto TableAdapter el patrón directa de la base de datos también se implementará a través de **Insert()**, **Update()**, y **Delete()**  métodos.

Ambos patrones de modificación de datos utiliza el TableAdapter **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades para emitir sus **insertar** , **Actualización**, y **eliminar** comandos para la base de datos. Puede inspeccionar y modificar el **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades haciendo clic en el objeto TableAdapter en el Diseñador de DataSet y, a continuación, pasar en la ventana Propiedades. (Asegúrese de que ha seleccionado el TableAdapter y que la **ProductsTableAdapter** objeto está seleccionado en la lista desplegable en la ventana Propiedades.)


[![El objeto TableAdapter tiene InsertCommand, UpdateCommand y DeleteCommand propiedades](creating-a-data-access-layer-cs/_static/image62.png)](creating-a-data-access-layer-cs/_static/image61.png)

**Figura 23**: tiene el TableAdapter **InsertCommand**, **UpdateCommand**, y **DeleteCommand** propiedades ([haga clic aquí para ver imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image63.png))


Para examinar o modificar cualquiera de estas propiedades de comando de base de datos, haga clic en el **CommandText** subpropiedad, que abrirá el generador de consultas.


[![Configurar la INSERCIÓN, actualización y las instrucciones DELETE en el generador de consultas](creating-a-data-access-layer-cs/_static/image65.png)](creating-a-data-access-layer-cs/_static/image64.png)

**Figura 24**: configurar la **insertar**, **actualización**, y **eliminar** las instrucciones en el generador de consultas ([haga clic aquí para ver la imagen a tamaño completo ](creating-a-data-access-layer-cs/_static/image66.png))


En el ejemplo de código siguiente se muestra cómo usar el patrón de actualización por lotes para doblar el precio de todos los productos que no están suspendidos y que tienen 25 unidades en existencias o menos:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample6.cs)]

El código siguiente muestra cómo usar el patrón directa de base de datos para eliminar un producto determinado mediante programación, a continuación, actualizar uno y, a continuación, agregue una nueva:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample7.cs)]

## <a name="creating-custom-insert-update-and-delete-methods"></a>Insert, Update y Delete métodos de creación personalizados

El **Insert()**, **Update()**, y **Delete()** métodos creados por el método directo de la base de datos pueden ser un poco complicadas, especialmente para las tablas con muchas columnas. Buscar en el ejemplo de código anterior, sin IntelliSense ayuda no está especialmente claro qué **productos** columna de tabla se asigna a cada parámetro de entrada para el **Update()** y **Insert()**  métodos. Puede darse cuando solo desea actualizar una única columna o dos, o desea un personalizada **Insert()** método que, probablemente, devuelve el valor del registro recién insertado **identidad** (incremento automático) campo.

Para crear un método personalizado de este tipo, vuelva al diseñador de DataSet. Haga doble clic en el objeto TableAdapter y elija Agregar consulta devolver para el Asistente de TableAdapter. En la segunda pantalla podemos indicar el tipo de consulta para crear. Vamos a crear un método que agrega un nuevo producto y, a continuación, devuelve el valor de la entrada recién agregado **ProductID**. Por lo tanto, optar por crear un **insertar** consulta.


[![Cree un método para agregar una nueva fila a la tabla Products](creating-a-data-access-layer-cs/_static/image68.png)](creating-a-data-access-layer-cs/_static/image67.png)

**Figura 25**: crear un método para agregar una fila nueva a la **productos** tabla ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image69.png))


En la siguiente pantalla del **InsertCommand**del **CommandText** aparece. Aumentar esta consulta agregando **seleccionar ámbito\_IDENTITY()** al final de la consulta, que devolverá el último valor de identidad insertado en un **identidad** columna en el mismo ámbito. (Consulte la [documentación técnica](https://msdn.microsoft.com/en-us/library/ms190315.aspx) para obtener más información acerca de **ámbito\_IDENTITY()** y por qué probable que desee [utilizar ámbito\_IDENTITY() en lugar de @ @IDENTITY](http://weblogs.sqlteam.com/travisl/archive/2003/10/29/405.aspx).) Asegúrese de que finalice la **insertar** instrucción con un punto y coma antes de agregar el **seleccione** instrucción.


[![Aumentar la consulta para devolver el valor de SCOPE_IDENTITY)](creating-a-data-access-layer-cs/_static/image71.png)](creating-a-data-access-layer-cs/_static/image70.png)

**Ilustración 26**: aumentar la consulta para devolver el **ámbito\_IDENTITY()** valor ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image72.png))


Por último, llame al método nuevo **InsertProduct**.


[![Establece el nombre del nuevo método en InsertProduct](creating-a-data-access-layer-cs/_static/image74.png)](creating-a-data-access-layer-cs/_static/image73.png)

**Figura 27**: establece el nombre del nuevo método en **InsertProduct** ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image75.png))


Cuando vuelva al diseñador de DataSet, verá que el **ProductsTableAdapter** contiene un nuevo método, **InsertProduct**. Si este método nuevo no tiene un parámetro para cada columna de la **productos** tabla, lo más probable es que se haya olvidado de finalizar la **insertar** instrucción con un punto y coma. Configurar la **InsertProduct** método y tiene un punto y coma delimita el **insertar** y **seleccione** instrucciones.

De forma predeterminada, inserte el problema no es de consulta métodos, lo que significa que devuelven el número de filas afectadas. Sin embargo, deseamos la **InsertProduct** método para devolver el valor devuelto por la consulta, no el número de filas afectadas. Para lograr esto, ajustar el **InsertProduct** del método **ExecuteMode** propiedad **escalar**.


[![Cambie la propiedad ExecuteMode a escalar](creating-a-data-access-layer-cs/_static/image77.png)](creating-a-data-access-layer-cs/_static/image76.png)

**Figura 28**: cambiar la **ExecuteMode** propiedad **escalar** ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image78.png))


El código siguiente muestra esta nueva **InsertProduct** método en acción:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample8.cs)]

## <a name="step-5-completing-the-data-access-layer"></a>Paso 5: Completar la capa de acceso a datos

Tenga en cuenta que la **ProductsTableAdapters** clase devuelve el **CategoryID** y **SupplierID** los valores de la **productos** tabla, pero no incluye el **CategoryName** columna desde la **categorías** tabla o la **CompanyName** columna desde la **proveedores**tabla, aunque probablemente se trata las columnas que desea mostrar al mostrar información del producto. Se puede aumentar el método del TableAdapter inicial, **GetProducts()**, debe incluir tanto el **CategoryName** y **CompanyName** valores de columna, que actualizarán el DataTable fuertemente tipado para incluir estas nuevas columnas.

Esto puede suponer un problema, sin embargo, como los métodos del TableAdapter para insertar, actualizar, y eliminar datos se basan en este método inicial. Afortunadamente, los métodos generados automáticamente para insertar, actualizar y eliminar no están afectadas por subconsultas en la **seleccione** cláusula. Por teniendo cuidado para agregar nuestro consultas a **categorías** y **proveedores** como subconsultas, en lugar de **UNIR** s, se evitará tener que rehacer estos métodos para modificar los datos. Haga doble clic en el **GetProducts()** método en el **ProductsTableAdapter** y elija Configurar. A continuación, ajustar el **seleccione** cláusula para que se asemeje al igual que:

[!code-sql[Main](creating-a-data-access-layer-cs/samples/sample9.sql)]


[![Actualización de la instrucción SELECT para el método GetProducts()](creating-a-data-access-layer-cs/_static/image80.png)](creating-a-data-access-layer-cs/_static/image79.png)

**Figura 29**: actualización de la **seleccione** instrucción para el **GetProducts()** (método) ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image81.png))


Después de actualizar el **GetProducts()** método que desea utilizar esta nueva consulta la tabla de datos incluirá dos nuevas columnas: **CategoryName** y **NombreProveedor**.


![La tabla de datos de productos tiene dos nuevas columnas](creating-a-data-access-layer-cs/_static/image82.png)

**Figura 30**: el **productos** DataTable tiene dos nuevas columnas


Tómese un momento para actualizar la **seleccione** cláusula en la  **GetProductsByCategoryID (*categoryID*)** método así.

Si actualiza el **GetProducts()** **seleccione** con **UNIR** sintaxis el Diseñador de DataSet no podrán generar automáticamente los métodos para insertar, actualizar y eliminar base de datos usando el patrón directa de la base de datos. En su lugar, tendrá que crearlas manualmente mucho al igual que hicimos con la **InsertProduct** método anteriormente en este tutorial. Además, manualmente tendrá que proporcionar el **InsertCommand**, **UpdateCommand**, y **DeleteCommand** si desea usar el lote de patrón de la actualización de los valores de propiedad.

## <a name="adding-the-remaining-tableadapters"></a>Agregar los TableAdapters restantes

Hasta ahora, hemos visto solo al trabajar con un TableAdapter único para una tabla de base de datos único. Sin embargo, la base de datos de Northwind contiene varias tablas relacionadas que se necesitará para trabajar con en nuestra aplicación web. Un conjunto de datos con tipo puede contener varios relacionados con tablas de datos. Por lo tanto, para completar nuestro DAL que necesitamos agregar tablas de datos para las demás tablas que se usarán en estos tutoriales. Para agregar un nuevo TableAdapter a un conjunto de datos con tipo, abra el Diseñador de DataSet, pulse el botón derecho en el diseñador y seleccione Agregar / TableAdapter. Esto creará una nueva tabla de datos y TableAdapter y le guían a través del asistente, examinamos anteriormente en este tutorial.

Tardar unos minutos para crear los siguientes métodos con las consultas siguientes y los TableAdapters. Tenga en cuenta que las consultas en el **ProductsTableAdapter** incluyen las subconsultas para obtener los nombres de categoría y el proveedor de cada producto. Además, si nos han estado siguiendo, ya ha agregado el **ProductsTableAdapter** la clase **GetProducts()** y  **GetProductsByCategoryID (*categoryID*)** métodos.

- **ProductsTableAdapter**

    - **GetProducts**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample10.sql)]
    - **GetProductsByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample11.sql)]
    - **GetProductsBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample12.sql)]
    - **GetProductByProductID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample13.sql)]
- **CategoriesTableAdapter**

    - **GetCategories**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample14.sql)]
    - **GetCategoryByCategoryID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample15.sql)]
- **SuppliersTableAdapter**

    - **GetSuppliers**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample16.sql)]
    - **GetSuppliersByCountry**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample17.sql)]
    - **GetSupplierBySupplierID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample18.sql)]
- **EmployeesTableAdapter**

    - **GetEmployees**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample19.sql)]
    - **GetEmployeesByManager**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample20.sql)]
    - **GetEmployeeByEmployeeID**: 

        [!code-sql[Main](creating-a-data-access-layer-cs/samples/sample21.sql)]


[![El Diseñador de DataSet después de haber agregado los cuatro TableAdapters](creating-a-data-access-layer-cs/_static/image84.png)](creating-a-data-access-layer-cs/_static/image83.png)

**Figura 31**: el conjunto de datos diseñador después de los cuatro TableAdapters se han agregado ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image85.png))


## <a name="adding-custom-code-to-the-dal"></a>Agregar código personalizado a la capa DAL

Los TableAdapters y las DataTables que agrega al conjunto de datos con tipo se expresan como un archivo de definición de esquemas XML (**Northwind.xsd**). Puede ver esta información de esquema con el botón secundario en el **Northwind.xsd** un archivo en el Explorador de soluciones y elija Ver código.


[![El archivo de definición (XSD) del esquema XML para el Northwinds escribió el conjunto de datos](creating-a-data-access-layer-cs/_static/image87.png)](creating-a-data-access-layer-cs/_static/image86.png)

**Figura 32**: archivo de la definición de esquemas de XML (XSD) para el conjunto de datos con tipo de Northwinds ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image88.png))


Esta información de esquema se convierte en código C# o Visual Basic en tiempo de diseño cuando se compila o en tiempo de ejecución (si es necesario), punto en el que puede ir a través de él con el depurador. Para ver el código generado automáticamente Bajar en la vista de clases y la obtención de detalles a las clases TableAdapter o conjunto de datos con tipo. Si no ve la vista de clases en la pantalla, vaya al menú Ver y seleccione desde allí o Ctrl + Mayús + C de llamadas. Desde la vista de clases puede ver las propiedades, métodos y eventos de las clases de conjunto de datos con tipo y un TableAdapter. Para ver el código para un método concreto, haga doble clic en el nombre del método en la vista de clases o doble clic en él y elija Ir a definición.


![Inspeccionar el código generado automáticamente, seleccione Ir a definición de la vista de clases](creating-a-data-access-layer-cs/_static/image89.png)

**Figura 33**: inspeccionar el código generado automáticamente, seleccione Ir a definición de la vista de clases


Mientras que el código generado automáticamente puede ser un gran ahorro de tiempo, el código suele ser muy genérico y debe personalizarse para satisfacer las necesidades específicas de una aplicación. El riesgo de extender el código generado automáticamente, sin embargo, es que la herramienta que generó el código podría decidir es el momento "regenerar" y sobrescribir las personalizaciones. Con el nuevo concepto de clase parcial de .NET 2.0, es fácil dividir una clase en varios archivos. Esto nos permite agregar nuestra propia métodos, propiedades y eventos para las clases generadas automáticamente sin tener que preocuparse sobre Visual Studio sobrescribiendo nuestro personalizaciones.

Para mostrar cómo se personaliza la capa DAL, vamos a agregar un **GetProducts()** método a la **SuppliersRow** clase. El **SuppliersRow** clase representa un único registro en el **proveedores** tabla; cada proveedor de proveedor puede cero a muchos productos, por lo que **GetProducts()** los devolverá productos del proveedor especificado. Para lograr esto crea un nuevo archivo de clase en el **aplicación\_código** carpeta denominada **SuppliersRow.cs** y agregue el código siguiente:

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample22.cs)]

Esta clase parcial indica al compilador que, cuando generar el **Northwind.SuppliersRow** clase para que incluya la **GetProducts()** que acabamos de definir el método. Si compila el proyecto y, a continuación, volver a la vista de clases, verá **GetProducts()** ahora aparece como un método de **Northwind.SuppliersRow**.


![El método GetProducts() es ahora parte de la clase de Northwind.SuppliersRow](creating-a-data-access-layer-cs/_static/image90.png)

**Figura 34**: el **GetProducts()** método forma ahora parte de la **Northwind.SuppliersRow** (clase)


El **GetProducts()** método ahora se puede utilizar para enumerar el conjunto de productos para un proveedor determinado, como se muestra en el código siguiente:

[!code-html[Main](creating-a-data-access-layer-cs/samples/sample23.html)]

Estos datos también se pueden mostrar en cualquiera de ASP. Controles Web de datos de NET. La página siguiente, utiliza un control GridView con dos campos:

- BoundField que muestra el nombre de cada proveedor, y
- TemplateField que contiene un control BulletedList que está enlazado a los resultados devueltos por la **GetProducts()** método para cada proveedor.

Examinaremos cómo mostrar dichos informes principal-detalle en tutoriales futuros. Por ahora, en este ejemplo está diseñado para mostrar mediante el método personalizado que agregó a la **Northwind.SuppliersRow** clase.

SuppliersAndProducts.aspx

[!code-aspx[Main](creating-a-data-access-layer-cs/samples/sample24.aspx)]

SuppliersAndProducts.aspx.cs

[!code-csharp[Main](creating-a-data-access-layer-cs/samples/sample25.cs)]


[![Nombre de la compañía del proveedor se muestra en la columna izquierda, sus productos a la derecha](creating-a-data-access-layer-cs/_static/image92.png)](creating-a-data-access-layer-cs/_static/image91.png)

**Figura 35**: nombre del proveedor el de la empresa se muestra en la columna izquierda, sus productos a la derecha ([haga clic aquí para ver la imagen a tamaño completo](creating-a-data-access-layer-cs/_static/image93.png))


## <a name="summary"></a>Resumen

Al compilar una aplicación web de creación de la capa DAL debe ser uno de los primeros pasos, que se producen antes de empezar a crear la capa de presentación. Con Visual Studio, la creación de un DAL basado en conjuntos de datos con tipo es una tarea que se pueden realizar en 10-15 minutos sin tener que escribir una línea de código. Los tutoriales avanzando generará en esta capa DAL. En el [siguiente tutorial](creating-a-business-logic-layer-cs.md) comenzaremos definir una serie de reglas de negocios y vea cómo implementarlas en una capa de lógica empresarial independiente.

Feliz programación.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre los temas tratados en este tutorial, consulte los siguientes recursos:

- [Creación de un DAL mediante fuertemente tipados los TableAdapters y las DataTables en VS 2005 y ASP.NET 2.0](https://weblogs.asp.net/scottgu/435498)
- [Diseño de componentes de nivel de datos y pasar datos a través de niveles](https://msdn.microsoft.com/en-us/library/ms978496.aspx)
- [Crear una capa de acceso a datos con el Diseñador de DataSet de Visual Studio 2005](http://www.theserverside.net/articles/showarticle.tss?id=DataSetDesigner)
- [Cifrar la información de configuración en ASP.NET 2.0 las aplicaciones](http://aspnet.4guysfromrolla.com/articles/021506-1.aspx)
- [Introducción a TableAdapter](https://msdn.microsoft.com/en-us/library/bz9tthwx.aspx)
- [Trabajar con un conjunto de datos con tipo](https://msdn.microsoft.com/en-us/library/esbykkzb.aspx)
- [Uso de acceso a datos fuertemente tipados en Visual Studio 2005 y ASP.NET 2.0](http://aspnet.4guysfromrolla.com/articles/020806-1.aspx)
- [Cómo extender métodos de TableAdapter](https://blogs.msdn.com/vbteam/archive/2005/05/04/ExtendingTableAdapters.aspx)
- [Recuperación de datos escalar de un procedimiento almacenado](http://aspnet.4guysfromrolla.com/articles/062905-1.aspx)

### <a name="video-training-on-topics-contained-in-this-tutorial"></a>Aprendizaje mediante vídeo sobre los temas incluidos en este Tutorial

- [Niveles de acceso a datos en aplicaciones ASP.NET](../../../videos/data-access/adonet-data-services/data-access-layers-in-aspnet-applications.md)
- [Cómo enlazar manualmente un conjunto de datos a un control Datagrid](../../../videos/data-access/adonet-data-services/how-to-manually-bind-a-dataset-to-a-datagrid.md)
- [Cómo trabajar con conjuntos de datos y los filtros de una aplicación ASP](../../../videos/data-access/adonet-data-services/how-to-work-with-datasets-and-filters-from-an-asp-application.md)

## <a name="about-the-author"></a>Acerca del autor

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml), autor de siete libros sobre ASP/ASP.NET y fundador de [4GuysFromRolla.com](http://www.4guysfromrolla.com), ha trabajado con las tecnologías Web de Microsoft desde 1998. Scott funciona como un consultor independiente, instructor y escritor. Su último libro es [ *SAM enseñar a usted mismo ASP.NET 2.0 en 24 horas*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco). Puede ponerse en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com) o a través de su blog, que se pueden encontrar en [http://ScottOnWriting.NET](http://ScottOnWriting.NET).

## <a name="special-thanks-to"></a>Agradecimientos especiales a

Esta serie de tutoriales se revisó por varios revisores útiles. Los revisores iniciales para este tutorial eran Ron Green, Hilton Giesenow, Dennis Patterson, Liz Shulok, Abel Gomez y Carlos Santos. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com.](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[Siguiente](creating-a-business-logic-layer-cs.md)
