---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
title: Crear la capa de acceso a datos | Documentos de Microsoft
author: Erikre
description: "Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 0bbf7a6e-d7eb-4091-91e4-fff892777f32
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create_the_data_access_layer
msc.type: authoredcontent
ms.openlocfilehash: ebace10dc8a861ab38bd5c834c2225e3373f13fe
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-the-data-access-layer"></a>Crear la capa de acceso a datos
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


Este tutorial describe cómo crear, acceder y revisar los datos de una base de datos con ASP.NET Web Forms y Entity Framework Code First. Este tutorial se basa en el tutorial anterior, "Crear el proyecto" y forma parte de la serie de tutoriales de almacén de Wingtip Toys. Cuando haya completado este tutorial, habrá creado un grupo de clases de acceso a datos que se encuentran en el *modelos* carpeta del proyecto.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo crear los modelos de datos.
- Cómo inicializar y la base de datos de origen.
- Cómo actualizar y configurar la aplicación para soportar la base de datos.

### <a name="these-are-the-features-introduced-in-the-tutorial"></a>Estas son las características introducidas en el tutorial:

- Entity Framework Code First
- LocalDB
- Anotaciones de datos

## <a name="creating-the-data-models"></a>Crear los modelos de datos

[Entity Framework](https://msdn.microsoft.com/en-us/data/aa937723) es un marco de trabajo de asignación relacional de objetos (ORM). Le permite trabajar con datos relacionales como objetos, lo que elimina la mayoría del código de acceso a datos que normalmente tendría que escribir. Si usa Entity Framework, puede emitir consultas mediante [LINQ](https://msdn.microsoft.com/en-us/library/bb397926.aspx), a continuación, recuperar y manipular los datos como objetos fuertemente tipados. LINQ proporciona patrones para consultar y actualizar datos. Entity Framework permite centrarse en crear el resto de la aplicación, en lugar de centrarse en los datos de los fundamentos de acceso. Más adelante en esta serie de tutoriales, le mostraremos cómo usar los datos para rellenar las consultas de navegación y el producto.

Entity Framework admite un paradigma de desarrollo llama *Code First*. Code First le permite definir los modelos de datos utilizando las clases. Una clase es una construcción que le permite crear sus propios tipos personalizados agrupándolas junto a las variables de otros tipos, métodos y eventos. Puede asignar clases a una base de datos existente o usarlos para generar una base de datos. En este tutorial, creará los modelos de datos mediante la escritura de clases del modelo de datos. A continuación, se podrá permitir que Entity Framework crear la base de datos sobre la marcha de estas clases nuevas.

Se iniciará mediante la creación de las clases de entidad que definen los modelos de datos para la aplicación de formularios Web Forms. A continuación, creará una clase de contexto que administra las clases de entidad y proporciona acceso a datos en la base de datos. También creará una clase de inicializador que va a usar para rellenar la base de datos.

### <a name="entity-framework-and-references"></a>Referencias y entity Framework

De forma predeterminada, Entity Framework se incluye cuando se crea un nuevo **aplicación Web ASP.NET** mediante la **formularios Web Forms** plantilla. Entity Framework se pueda instalar, desinstalar y actualizar como un paquete de NuGet.

Este paquete de NuGet incluye lo siguiente **en tiempo de ejecución** ensamblados dentro de su proyecto:

- EntityFramework.dll: todo el código de tiempo de ejecución común utilizado por Entity Framework
- EntityFramework.SqlServer.dll: el proveedor de Microsoft SQL Server para Entity Framework

### <a name="entity-classes"></a>Clases de entidad

Las clases que cree para definir el esquema de los datos se denominan clases de entidad. Si está familiarizado con el diseño de la base de datos, considere las clases de entidad como definiciones de tabla de una base de datos. Cada propiedad de la clase especifica una columna en la tabla de la base de datos. Estas clases proporcionan una interfaz sencilla y objeto-relacional entre código orientado a objetos y la estructura de tabla relacional de la base de datos.

En este tutorial, comenzará mediante la adición de las clases de entidad simple que representa los esquemas de productos y categorías. La clase products contiene definiciones para cada producto. El nombre de cada uno de los miembros de la clase de producto será `ProductID`, `ProductName`, `Description`, `ImagePath`, `UnitPrice`, `CategoryID`, y `Category`. La clase de categoría contiene definiciones para cada categoría de un producto puede pertenecer a, como coche, barco o plano. El nombre de cada uno de los miembros de la clase de categoría será `CategoryID`, `CategoryName`, `Description`, y `Products`. Cada producto pertenece a una de las categorías. Estas clases de entidad se agregará en el proyecto existente *modelos* carpeta.

1. En **el Explorador de soluciones**, haga clic en el *modelos* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**. 

    ![Crear nuevo menú de elemento de la capa de acceso a datos:](create_the_data_access_layer/_static/image1.png)

 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. En **Visual C#** desde el **instalado** panel de la izquierda, seleccione **código**. 

    ![Crear nuevo menú de elemento de la capa de acceso a datos:](create_the_data_access_layer/_static/image2.png)
3. Seleccione **clase** desde el panel central y esta nueva clase el nombre *Product.cs*.
4. Haga clic en **Agregar**.  
 El nuevo archivo de clase se muestra en el editor.
5. Reemplace el código predeterminado por el código siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample1.cs)]
6. Cree otra clase repitiendo los pasos del 1 al 4, sin embargo, la nueva clase el nombre *Category.cs* y reemplace el código predeterminado por el código siguiente:  

    [!code-csharp[Main](create_the_data_access_layer/samples/sample2.cs)]

Como se mencionó anteriormente, el `Category` clase representa el tipo de producto que la aplicación está diseñada para vender (como <a id="a"> </a> &quot;automóviles&quot;, &quot;botes&quot;, &quot;Cohetes&quot;, etc.) y la `Product` clase representa los productos individuales (toys) en la base de datos. Cada instancia de un `Product` objeto corresponderá a una fila dentro de una tabla de base de datos relacional, y cada propiedad de la clase de producto se asignará a una columna en la tabla de base de datos relacional. Más adelante en este tutorial, podrá revisar los datos de productos incluidos en la base de datos.

### <a name="data-annotations"></a>Anotaciones de datos

Es podrán que haya observado que determinados miembros de las clases tienen atributos especifica los detalles sobre el miembro, como `[ScaffoldColumn(false)]`. Se trata de *las anotaciones de datos*. Los atributos de anotación de datos pueden describir cómo validar la entrada del usuario para ese miembro, para especificar un formato y para especificar cómo se se modela cuando se crea la base de datos.

### <a name="context-class"></a>Context (Clase)

Para empezar a usar las clases de acceso a datos, debe definir una clase de contexto. Como se mencionó anteriormente, la clase de contexto administra las clases de entidad (como el `Product` clase y la `Category` clase) y proporciona acceso a datos en la base de datos.

Este procedimiento agrega una nueva clase de contexto C# a la *modelos* carpeta.

1. Haga clic en el *modelos* carpeta y, a continuación, seleccione **agregar**  - &gt; **nuevo elemento**.   
 Se abrirá el cuadro de diálogo **Agregar nuevo elemento**.
2. Seleccione **clase** desde el panel central, asígnele el nombre *ProductContext.cs* y haga clic en **agregar**.
3. Reemplace el código predeterminado contenido en la clase con el código siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample3.cs)]

Este código agrega el `System.Data.Entity` espacio de nombres para que tengan acceso a toda la funcionalidad básica de Entity Framework, que incluye la capacidad para consultar, insertar, actualizar y eliminar datos trabajando con objetos fuertemente tipados.

El `ProductContext` clase representa el contexto de base de datos de producto de Entity Framework, que administra las filas, almacenar y actualizar `Product` instancias en la base de datos de clase. El `ProductContext` clase se deriva de la `DbContext` proporcionada por Entity Framework de clase base.

### <a name="initializer-class"></a>Clase de inicializador

Debe ejecutar lógica personalizada para inicializar la hora de la primera de la base de datos que se usa el contexto. Esto permitirá que los datos de inicialización para agregarse a la base de datos para que se pueden mostrar inmediatamente productos y categorías.

Este procedimiento agrega una nueva clase inicializador C# para el *modelos* carpeta.

1. Crear otra `Class` en el *modelos* carpeta y asígnele el nombre *ProductDatabaseInitializer.cs*.
2. Reemplace el código predeterminado contenido en la clase con el código siguiente:   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample4.cs)]

Como puede ver en el código anterior, cuando se crea la base de datos y se inicializa, el `Seed` se invalida y establecer propiedad. Cuando el `Seed` propiedad está establecida, los valores de las categorías y los productos se usan para rellenar la base de datos. Si intenta actualizar los datos de inicialización, modifique el código anterior una vez creada la base de datos, no verá las actualizaciones cuando se ejecuta la aplicación Web. La razón es que el código anterior usa una implementación de la `DropCreateDatabaseIfModelChanges` clase para que reconozca si ha cambiado el modelo (esquema) antes de restablecer los datos de inicialización. Si no se realizan cambios en el `Category` y `Product` no se reinicializarán las clases de entidad, la base de datos con los datos de inicialización.

> [!NOTE] 
> 
> Si desea que la base de datos se vuelven a crear cada vez ejecutó la aplicación, puede usar el `DropCreateDatabaseAlways` clase en lugar de la `DropCreateDatabaseIfModelChanges` clase. Sin embargo, para esta serie de tutoriales, use la `DropCreateDatabaseIfModelChanges` clase.


En este momento en este tutorial, tendrá un *modelos* carpeta con cuatro nuevas clases y la clase de un valor predeterminado:

![Crear carpeta de modelos de la capa de acceso a datos:](create_the_data_access_layer/_static/image3.png)

### <a name="configuring-the-application-to-use-the-data-model"></a>Configuración de la aplicación para usar el modelo de datos

Ahora que ha creado las clases que representan los datos, debe configurar la aplicación para utilizar las clases. En el *Global.asax* archivo, puedes agregar código que inicializa el modelo. En el *Web.config* archivo agregar información que indica a la aplicación a lo que la base de datos usará para almacenar los datos que se representan mediante las nuevas clases de datos. El *Global.asax* archivo puede usarse para controlar eventos de aplicación o los métodos. El *Web.config* archivo le permite controlar la configuración de la aplicación web ASP.NET.

#### <a name="updating-the-globalasax-file"></a>Actualizando el archivo Global.asax

Para inicializar los modelos de datos cuando se inicia la aplicación, se actualizará el `Application_Start` controlador en el *Global.asax.cs* archivo.

> [!NOTE] 
> 
> En el Explorador de soluciones, seleccione el *Global.asax* archivo o la *Global.asax.cs* archivo para editar la *Global.asax.cs* archivo.


1. Agregue el código siguiente aparecen resaltado en amarillo en el `Application_Start` método en el *Global.asax.cs* archivo.   

    [!code-csharp[Main](create_the_data_access_layer/samples/sample5.cs?highlight=9-10,22-23)]

> [!NOTE] 
> 
> El explorador debe admitir HTML5 para ver el código que se resalta en amarillo cuando se ven esta serie de tutoriales en un explorador.


Como se muestra en el código anterior, cuando se inicia la aplicación, la aplicación especifica el inicializador de la que se ejecutará durante la primera vez que los datos se tiene acceso. Los dos espacios de nombres adicionales son necesarios para tener acceso a la `Database` objeto y el `ProductDatabaseInitializer` objeto.

 Modificar el archivo Web.Config 

Aunque Entity Framework Code First generará una base de datos de en una ubicación predeterminada cuando la base de datos se rellena con datos de valor de inicialización, agregar su propia información de conexión a la aplicación ofrece control de la ubicación de la base de datos. Especifique esta conexión de base de datos usando una cadena de conexión en la aplicación *Web.config* archivo en la raíz del proyecto. Al agregar una nueva cadena de conexión, puede dirigir la ubicación de la base de datos (*wingtiptoys.mdf*) que se crea en el directorio de datos de la aplicación (*aplicación\_datos*), en lugar de su valor predeterminado ubicación. Al realizar este cambio le permitirá encontrar e inspeccione el archivo de base de datos más adelante en este tutorial.

1. En **el Explorador de soluciones**, busque y abra la *Web.config* archivo.
2. Agregar la cadena de conexión que se resalta en amarillo en el `<connectionStrings>` sección de la *Web.config* como sigue:  

    [!code-xml[Main](create_the_data_access_layer/samples/sample6.xml?highlight=4-7)]

Cuando se ejecuta la aplicación por primera vez, creará la base de datos en la ubicación especificada por la cadena de conexión. Sin embargo antes de ejecutar la aplicación, vamos a crear, en primer lugar.

## <a name="building-the-application"></a>Compilar la aplicación

Para asegurarse de que todas las clases y los cambios en su aplicación Web funcionan, debe compilar la aplicación.

1. Desde el **depurar** menú, seleccione **WingtipToys generar**.  
 El **salida** se muestra la ventana y, si todo va bien, verá un *se realizó correctamente* mensaje.  

    ![Crear la capa de acceso a datos - las ventanas de salida](create_the_data_access_layer/_static/image4.png)

Si experimenta un error, vuelva a comprobar los pasos anteriores. La información de la **salida** ventana le indicará qué archivo tiene un problema y donde se requiere un cambio en el archivo. Esta información le permitirá determinar qué parte de los pasos anteriores se deben revisarse y corregido en el proyecto.

## <a name="summary"></a>Resumen

En este tutorial de la serie se ha creado el modelo de datos, así como, agrega el código que se usarán para inicializar y la base de datos de origen. También ha configurado la aplicación para usar los modelos de datos cuando se ejecuta la aplicación.

En el siguiente tutorial, deberá actualizar la interfaz de usuario, agregar funciones de navegación y recuperar datos de la base de datos. El resultado será la base de datos que se creen automáticamente en función de las clases de entidad que creó en este tutorial.

## <a name="additional-resources"></a>Recursos adicionales

[Introducción a Entity Framework](https://msdn.microsoft.com/en-us/library/bb399567.aspx)   
[Guía para principiantes ADO.NET Entity Framework](https://msdn.microsoft.com/en-us/data/ee712907)   
[Primer desarrollo con Entity Framework de código](http://www.msteched.com/2010/Europe/DEV212) (vídeo)   
[API fluida de código First de relaciones](https://msdn.microsoft.com/en-us/data/hh134698)   
[Anotaciones de datos primer código](https://msdn.microsoft.com/en-us/data/gg193958)  
[Mejoras de productividad para Entity Framework](https://blogs.msdn.com/b/efdesign/archive/2010/06/21/productivity-improvements-for-the-entity-framework.aspx?wa=wsignin1.0)

>[!div class="step-by-step"]
[Anterior](create-the-project.md)
[Siguiente](ui_and_navigation.md)
