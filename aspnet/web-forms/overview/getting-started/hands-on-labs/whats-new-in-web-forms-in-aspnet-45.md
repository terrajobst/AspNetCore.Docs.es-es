---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Novedades de Web Forms de ASP.NET 4.5 | Documentos de Microsoft
author: rick-anderson
description: La nueva versión de formularios Web Forms de ASP.NET presenta a una serie de mejoras que se centra en la mejora la experiencia del usuario cuando se trabaja con datos. En versiones anteriores de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: e230faac0dc81b67d74945dc98eee80f83205f65
ms.sourcegitcommit: 3a893ae05f010656d99d6ddf55e82f1b5b6933bc
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/18/2018
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Novedades de formularios Web Forms de ASP.NET 4.5
====================
Por [Web colonias equipo](https://twitter.com/webcamps)

> La nueva versión de formularios Web Forms de ASP.NET presenta a una serie de mejoras que se centra en la mejora la experiencia del usuario cuando se trabaja con datos.
> 
> En versiones anteriores de formularios Web Forms, cuando se usa el enlace de datos para emitir el valor de un miembro de objeto, ha utilizado las expresiones de enlace de datos Bind() o Eval(). En la nueva versión de ASP.NET, pueden declarar qué tipo de datos de un control se va a enlazar a utilizando una nueva propiedad ItemType. Al establecer esta propiedad le permitirá utilizar una variable fuertemente tipado para recibir todos los beneficios de la experiencia de desarrollo de Visual Studio, como IntelliSense, navegación de miembro y comprobación en tiempo de compilación.
> 
> Con los controles enlazados a datos, ahora puede también especificar sus propios métodos personalizados para seleccionar, actualizar, eliminar e insertar datos, simplificar la interacción entre los controles de página y la lógica de aplicación. Además, las capacidades de enlace de modelo se agregaron a ASP.NET, lo que significa que puede asignar datos de la página directamente en parámetros de tipo de método.
> 
> Validar la entrada del usuario también debe ser más fácil con la versión más reciente de formularios Web Forms. Ahora puede agregar anotaciones a las clases de modelo con atributos de validación de la **System.ComponentModel.DataAnnotations** solicitud que controla todo el sitio y el espacio de nombres validan entrada de usuario mediante esa información. Validación del lado cliente en formularios Web Forms está ahora integrado con jQuery, proporciona limpiador código del lado cliente y características de JavaScript discretos.
> 
> En el área de validación de solicitud, se han realizado mejoras para que resulten más fáciles de leer datos de la solicitud no válidas o desactivar la validación de solicitud de partes concretas de las aplicaciones de forma selectiva.
> 
> Se realizaron algunas mejoras en formularios Web Forms controles de servidor para aprovechar las nuevas características de HTML5:
> 
> - La propiedad TextMode del control de cuadro de texto se actualizó para admitir los nuevos tipos de entrada de HTML5 como correo electrónico, fecha y hora y así sucesivamente.
> - El control FileUpload ahora es compatible con varias cargas de archivos de los exploradores que admiten esta característica de HTML5.
> - Validador controla ahora compatibilidad con validación HTML5 elementos de entrada.
> - Nuevos elementos de HTML5 que tienen atributos que representan una dirección URL ahora admiten runat =&quot;server&quot;. Como resultado, puede utilizar las convenciones de ASP.NET en las rutas de acceso de dirección URL, como el ~ (operador) para representar la raíz de la aplicación (por ejemplo, &lt;vídeo runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/vídeo&gt;).
> - Se ha corregido el control UpdatePanel para admitir campos de entrada de registro HTML5.
> 
> En el portal ASP.NET oficial encontrará más ejemplos de las nuevas características en ASP.NET WebForms 4.5: [What's New en ASP.NET 4.5 y Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de aprendizaje de Web colonias, disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar expresiones de enlace de datos fuertemente tipados
- Usar nuevas características de enlace de modelo de formularios Web Forms
- Usar proveedores de valores para la asignación de datos de la página a los métodos de código subyacente
- Usar anotaciones de datos para la validación de entrada de usuario
- Tomar advange de validación del lado cliente unobstrusive con jQuery en formularios Web Forms
- Implementar la validación de solicitud específico
- Implementar el procesamiento de formularios Web Forms asincrónico de la página

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea obtener información sobre cómo utilizarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio de prácticas incluye los ejercicios siguientes:

1. [Ejercicio 1: Modelo de enlace en los formularios Web de ASP.NET](#Exercise1)
2. [Ejercicio 2: Validación de datos](#Exercise2)
3. [Ejercicio 3: Página asincrónica de procesamiento en ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debería obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ejercicio 1: Modelo de enlace en los formularios Web de ASP.NET

La nueva versión de formularios Web Forms de ASP.NET presenta a una serie de mejoras que se centra en la mejora de la experiencia al trabajar con datos. En este ejercicio, aprenderá acerca de los controles de datos fuertemente tipados y enlace de modelo.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarea 1: uso de enlaces de datos fuertemente tipados

En esta tarea, verá los nuevos fuertemente tipado enlaces disponibles en ASP.NET 4.5.

1. Abra la **comenzar** solución ubicado en **origen/Ex1-ModelBinding/Begin/** carpeta.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la **Customers.aspx** página. Incluir una lista sin numerar en el control principal e incluir un control de repetidor dentro para enumerar a cada cliente. Establece el nombre del Repetidor en **customersRepeater** tal como se muestra en el código siguiente.

    En versiones anteriores de formularios Web Forms, cuando se usa el enlace de datos para emitir el valor de un miembro de un objeto está enlace de datos, usaría una expresión de enlace de datos, junto con una llamada al método Eval, pasando el nombre del miembro como una cadena.

    En tiempo de ejecución, estas llamadas al valor Eval se usar la reflexión en el objeto enlazado actualmente para leer el valor del miembro con el nombre especificado y mostrará el resultado en el código HTML. Este enfoque facilita enlazar con datos arbitrarios, unshaped.

    Por desgracia, se pierden muchas de las características de gran experiencia en tiempo de desarrollo en Visual Studio, que incluye IntelliSense para los nombres de miembro, la compatibilidad para la navegación (por ejemplo, ir a definición) y la comprobación en tiempo de compilación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra la **Customers.aspx.cs** archivo.
4. Agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. En el **página\_carga** método, agregue código para rellenar el repetidor con la lista de clientes.

    (Código de fragmento de código: *Web de origen de datos de los clientes de formularios laboratorio - Ex01 - Bind*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La solución utiliza Entity Framework junto con CodeFirst para crear y obtener acceso a la base de datos. En el código siguiente, la customersRepeater está enlazada a una consulta materializada que devuelve a todos los clientes de la base de datos.
6. Presione **F5** para ejecutar la solución y vaya a la **clientes** página para ver el Repetidor en acción. Como la solución usa CodeFirst, la base de datos se crean y se rellenan en la instancia local de SQL Express cuando se ejecuta la aplicación.

    ![Lista de los clientes con un repetidor](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "enumerar los clientes con un repetidor")

    *Lista de los clientes con un repetidor*

    > [!NOTE]
    > En Visual Studio 2012, IIS Express es el servidor de desarrollo de Web predeterminado.
7. Cierre el explorador y vuelva a Visual Studio.
8. Ahora, reemplace la implementación para usar enlaces fuertemente tipados. Abra la **Customers.aspx** página y use la nueva **ItemType** atributo del repetidor para establecer el **cliente** tipo como el tipo de enlace.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La propiedad ItemType permite declarar el tipo de datos el control va a enlazar a y le permite usar fuertemente tipada de enlace dentro del control enlazado a datos.
9. Reemplace el contenido con el código siguiente ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Una desventaja con los enfoques anteriores es que las llamadas a Eval() y Bind() enlazado tardíamente - lo que significa que pasar cadenas para representar los nombres de propiedad. Esto significa que no obtendrá Intellisense para los nombres de miembro, soporte técnico para la exploración del código (por ejemplo, ir a definición), ni compatibilidad comprobación en tiempo de compilación.

    Establecer la propiedad ItemType hace dos nuevas variables que se generarán en el ámbito de las expresiones de enlace de datos con tipo: **elemento** y **BindItem**. Puede usar estas variables fuertemente tipadas en las expresiones de enlace de datos y obtener todos los beneficios de la experiencia de desarrollo de Visual Studio.

    El &quot; **:** &quot; utilizados en la expresión configurará automáticamente y codificar en HTML el resultado para evitar problemas de seguridad (por ejemplo, entre sitios ataques XSS). Esta notación estaba disponible desde .NET 4 para escribir de respuesta, pero ahora también está disponible en las expresiones de enlace de datos.

    > [!NOTE]
    > El miembro de elemento funciona para el enlace unidireccional. Si desea realizar un enlace bidireccional use la **BindItem** miembro.

    ![Compatibilidad con IntelliSense en enlace fuertemente tipado](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "compatibilidad con IntelliSense en enlace fuertemente tipados")

    *Compatibilidad con IntelliSense en enlace fuertemente tipados*
10. Presione **F5** para ejecutar la solución e ir a la página de los clientes para asegurarse de que los cambios funcionan según lo previsto.

    ![Mostrar detalles del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "mostrar detalles del cliente")

    *Mostrar detalles del cliente*
11. Cierre el explorador y vuelva a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarea 2: modelo de presentación enlazar en formularios Web Forms

En versiones anteriores de formularios Web Forms de ASP.NET, cuando desea realizar el enlace de datos bidireccional, tanto al recuperar y actualizar datos, necesita usar un objeto de origen de datos. Podría tratarse de un origen de datos de objeto, un origen de datos de SQL, un origen de datos de LINQ y así sucesivamente. Sin embargo si su escenario requiere código personalizado para controlar los datos, había que usar el origen de datos de objeto y esto pone tiene algunas desventajas. Por ejemplo, necesaria para evitar los tipos complejos y sean necesarios para controlar las excepciones cuando se ejecuta la lógica de validación.

En la nueva versión de ASP.NET Web Forms los controles enlazados a datos admiten el enlace de modelos. Esto significa que puede especificar seleccionar, actualizar, insertar y eliminar métodos directamente en el control enlazado a datos para llamar a la lógica de su archivo de código subyacente o de otra clase.

Para obtener información acerca de esto, utilizará un control GridView para mostrar las categorías de producto con el nuevo **SelectMethod** atributo. Este atributo permite especificar un método para recuperar los datos de GridView.

1. Abra la **Products.aspx** página e incluir un **GridView**. Configurar el control GridView tal y como se muestra a continuación para utilizar los enlaces fuertemente tipadas y habilitar la ordenación y paginación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use la nueva **SelectMethod** atributo para configurar el control GridView para llamar a un **GetCategories** método para seleccionar los datos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra el **Products.aspx.cs** código subyacente de archivos y agregue las siguientes instrucciones using.

    (Código de fragmento de código: *Web espacios de nombres de formularios laboratorio - Ex01 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Agregar un miembro privado en la **productos** clase y asignar una nueva instancia de **ProductsContext**. Esta propiedad, almacenará el contexto de datos de Entity Framework que permite que se conecte a la base de datos.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Crear un **GetCategories** método para recuperar la lista de categorías mediante LINQ. La consulta incluirá el **productos** propiedad GridView puede mostrar la cantidad de productos de cada categoría. Tenga en cuenta que el método devuelve un objeto IQueryable sin formato que representan la consulta se ejecuta más adelante en el ciclo de vida de la página.

    (Código de fragmento de código: *Web Forms laboratorio - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > En versiones anteriores de formularios Web Forms de ASP.NET, lo que permite ordenar y paginar mediante su propia lógica de repositorio dentro de un contexto de origen de datos de objeto, necesarios para escribir su propio código personalizado y reciba todos los parámetros necesarios. Ahora, como los métodos de enlace de datos pueden devolver IQueryable y representa una consulta todavía para ejecutarlos, ASP.NET puede ocuparse de modificar la consulta para agregar una ordenación correcta y parámetros de paginación.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Debería ver que el control GridView se rellena con las categorías de devuelto por el método GetCategories.

    ![Rellenar un control GridView que usa enlace de modelos](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "rellenar un control GridView que usa enlace de modelos")

    *Rellenar un control GridView que usa enlace de modelos*
7. Presione **MAYÚS**+**F5** detener la depuración.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tarea 3: proveedores de valores en el enlace de modelos

Enlace de modelos no sólo permite especificar métodos personalizados para trabajar con los datos directamente en el control enlazado a datos, sino que también le permite asignar datos desde la página a los parámetros de estos métodos. En el parámetro de método, puede usar atributos de proveedor de valor para especificar el origen de datos del valor. Por ejemplo:

- Controles de la página
- Valores de cadena de consulta
- Ver datos
- Estado de sesión
- Cookies
- Datos de formulario expuesto
- Estado de vista
- También se admiten proveedores de valores personalizados

Si ha usado ASP.NET MVC 4, observará que la compatibilidad de enlace de modelo es similar. De hecho, estas características se han tomado de ASP.NET MVC y movido el **System.Web** ensamblado para que pueda usar también en formularios Web Forms.

En esta tarea, actualizará GridView para filtrar los resultados por la cantidad de productos para cada categoría, recibe el parámetro filter con el enlace de modelos.

1. Vuelva a la **Products.aspx** página.
2. En la parte superior del control GridView, agregue un **etiqueta** y un **ComboBox** para seleccionar el número de productos de cada categoría, tal y como se muestra a continuación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Agregar un **EmptyDataTemplate** a GridView para mostrar un mensaje cuando no hay ninguna categoría con el número seleccionado de productos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra la **Products.aspx.cs** código subyacente y agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificar el **GetCategories** método para recibir un número entero **minProductsCount** argumento y filtrar los resultados devueltos. Para ello, reemplace el método por el código siguiente.

    (Código de fragmento de código: *Web Forms laboratorio - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    El nuevo **[Control]** del atributo en el **minProductsCount** argumento indicará a ASP.NET su valor se debe rellenar con un control en la página. ASP.NET buscará cualquier control que coincida con el nombre del argumento (minProductsCount) y realizar la asignación necesaria y la conversión para rellenar el parámetro con el valor del control.

    Como alternativa, el atributo proporciona un constructor sobrecargado que le permite especificar el control desde el que se va a obtener el valor.

    > [!NOTE]
    > Un objetivo de las características de enlace de datos consiste en reducir la cantidad de código que debe escribirse para la interacción de página. Además el proveedor de valor [Control], puede usar otros proveedores de enlace de modelos en los parámetros de método. Algunos de ellos se muestran en la introducción de la tarea.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Seleccione un número de productos en la lista desplegable y observe cómo se ha actualizado la GridView.

    ![Filtrado de GridView con un valor de la lista desplegable](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrado GridView con un valor de lista desplegable")

    *Filtrado de GridView con un valor de lista desplegable*
7. Detenga la depuración.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarea 4: uso de modelo de enlace para filtrar

En esta tarea, agregará un segundo, secundarios GridView para mostrar los productos dentro de la categoría seleccionada.

1. Abra la **Products.aspx** página y actualice las categorías GridView para generar automáticamente el botón de selección.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Agregue un segundo **GridView** denominado **productsGrid** en la parte inferior. Establecer el **ItemType** a **WebFormsLab.Model.Product**, el **DataKeyNames** a **ProductId** y **SelectMethod**  a **GetProducts**. Establecer **AutoGenerateColumns** a **false** y agregar las columnas ProductId, ProductName, descripción y UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra la **Products.aspx.cs** archivo de código subyacente. Implemente el **GetProducts** método para recibir el identificador de categoría de la categoría de GridView y filtrar los productos. Enlace de modelos se establecerá el valor de parámetro con la fila seleccionada en el **categoriesGrid**. Puesto que no coincide con el nombre de argumento y el nombre de control, debe especificar el nombre del control en el proveedor de valores de Control.

    (Código de fragmento de código: *Web Forms laboratorio - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Este enfoque facilita unidad estos métodos de prueba. En un contexto de prueba unitaria, donde los formularios Web Forms no se está ejecutando, el atributo [Control] no realizará ninguna acción específica.
4. Abra la **Products.aspx** página y buscar los productos GridView. Actualizar los productos de GridView para mostrar un vínculo para la edición del producto seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra la **ProductDetails.aspx** página de código subyacente y reemplace la **SelectProduct** método por el código siguiente.

    (Código de fragmento de código: *Web Forms laboratorio - Ex01 - SelectProduct método*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Tenga en cuenta que la **[consulta]** atributo se usa para rellenar el parámetro de método de un parámetro de productId en la cadena de consulta.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Seleccione cualquier categoría de las categorías de GridView y observe que los productos GridView se actualiza.

    ![Que muestre los productos de la categoría seleccionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "que muestre los productos de la categoría seleccionada")

    *Que muestre los productos de la categoría seleccionada*
7. Haga clic en el **vista** vínculo en un producto para abrir la página ProductDetails.aspx.

    Tenga en cuenta que la página está recuperando el producto con el SelectMethod mediante el parámetro productId de la cadena de consulta.

    ![Ver los detalles del producto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "ver los detalles del producto")

    *Ver los detalles del producto*

    > [!NOTE]
    > En el siguiente ejercicio se implementará la capacidad para escribir una descripción de HTML.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarea 5: enlace para las operaciones de actualización de modelos usando

En la tarea anterior, ha utilizado el enlace de modelos principalmente para seleccionar los datos, en esta tarea aprenderá a utilizar el enlace de modelos en las operaciones de actualización.

Actualizará las categorías de GridView para que el usuario pueda actualizar las categorías.

1. Abra la **Products.aspx** página y actualice las categorías GridView para generar automáticamente el botón Editar y use la nueva **UpdateMethod** atributo para especificar un **UpdateCategory**método para actualizar el elemento seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    El atributo DataKeyNames en GridView definir que son los miembros que identifican de forma única el objeto enlazado del modelo y por lo tanto, que son los parámetros que al menos debería recibir el método update.
2. Abra la **Products.aspx.cs** archivo de código subyacente e implemente el **UpdateCategory** método. El método debería recibir el identificador de categoría para cargar la categoría actual, rellenar los valores de GridView y, a continuación, actualizar la categoría.

    (Código de fragmento de código: *Web Forms laboratorio - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    El nuevo **TryUpdateModel** método en la clase de página es responsable de rellenar el objeto de modelo con los valores de los controles en la página. En este caso, reemplazarán los valores actualizados de la fila actual de GridView que se está editando en el **categoría** objeto.

    > [!NOTE]
    > El siguiente ejercicio explicará el uso de la ModelState.IsValid para validar los datos introducidos por el usuario al editar el objeto.
3. Ejecute el sitio Web y vaya a la página de productos. Editar una categoría. Escriba un nombre nuevo y, a continuación, haga clic en **actualización** para conservar los cambios.

    ![Editar categorías](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "editar categorías")

    *Edición de categorías*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ejercicio 2: Validación de datos

En este ejercicio, obtendrá información sobre las nuevas características de validación de datos en ASP.NET 4.5. Comprobará las nuevas características de validación discreto en formularios Web Forms. Usará las anotaciones de datos en las clases del modelo de aplicación para la validación de entrada de usuario y, por último, obtendrá información sobre cómo activar o desactivar la validación de solicitudes a controles individuales en una página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarea 1: validación discreto

Los formularios con datos complejos, incluidos los validadores tienden a generar demasiada código JavaScript en la página, que puede representar aproximadamente el 60% del código. Con la validación discreta habilitada, el código HTML tendrá un aspecto más limpio y más ordenada.

En esta sección, podrá aplicar validación discreta en ASP.NET para comparar el código HTML generado por ambas configuraciones.

1. Abra **Visual Studio 2012** y abra el **comenzar** soluciones se encuentran en la **Source\Ex2 Validation\Begin** carpeta de este laboratorio. Como alternativa, puede seguir trabajando en la solución existente en el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el **WebFormsLab** proyecto **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Presione **F5** para iniciar la aplicación web. Vaya a los clientes de página y haga clic en el **agregar un nuevo cliente** vínculo.
3. Haga doble clic en la página del explorador y seleccione **ver código fuente** opción para abrir el código HTML generado por la aplicación.

    ![Mostrar la página de código HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "que muestra el código HTML de la página")

    *Mostrar la página de código HTML*
4. Desplácese por el código fuente de la página y tenga en cuenta que ASP.NET ha insertado validadores de código y los datos de JavaScript en la página para realizar las validaciones y mostrar la lista de errores.

    ![Código de JavaScript de validación en la página de CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "código JavaScript de validación en la página de CustomerDetails")

    *Código de JavaScript de validación en la página de CustomerDetails*
5. Cierre el explorador y vuelva a Visual Studio.
6. Ahora podrá aplicar validación discreto. Abra **Web.Config** y busque **ValidationSettings:UnobtrusiveValidationMode** clave en el **AppSettings** sección **.** Establece el valor de clave en **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > También puede establecer esta propiedad el &quot; **página\_carga** &quot; evento en caso de que desea habilitar la validación discreto solo para algunas páginas.
7. Abra **CustomerDetails.aspx** y presione **F5** para iniciar la aplicación Web.
8. Presione la tecla F12 para abrir las herramientas de desarrollo de Internet Explorer. Una vez que las herramientas de desarrollo está abierto, seleccione la pestaña de script. Seleccione **CustomerDetails.aspx** en el menú y toman tenga en cuenta que las secuencias de comandos necesitan para ejecutar jQuery en la página se han cargado en el explorador desde el sitio local.

    ![Cargando el jQuery JavaScript archivos directamente desde el servidor IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "cargar jQuery JavaScript archivos directamente desde el servidor IIS local")

    *Cargar los archivos de JavaScript de jQuery directamente desde el servidor IIS local*
9. Cierre el explorador para volver a Visual Studio. Abra la **Site.Master** archivo nuevo y busque la **ScriptManager**. Agregue el atributo **EnableCdn** propiedad con el valor **True**. Esto forzará jQuery debe cargarse desde la dirección URL en línea, no desde la dirección URL del sitio local.
10. Abra **CustomerDetails.aspx** en Visual Studio. Presione la tecla F5 para ejecutar el sitio. Una vez que se abre Internet Explorer, presione la tecla F12 para abrir las herramientas de desarrollo. Seleccione el **Script** ficha y, a continuación, eche un vistazo a la lista desplegable. Tenga en cuenta que los archivos de JavaScript de jQuery ya no se cargan desde el sitio local, pero en su lugar desde la red CDN de jQuery en línea.

    ![Cargando el jQuery JavaScript archivos desde la red CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "cargar jQuery JavaScript archivos desde la red CDN")

    *Cargar los archivos de jQuery JavaScript de la red CDN*
11. Abra el código de origen de página HTML nuevo con la opción de origen de la vista en el explorador. Observe que al habilitar la validación discreta ASP.NET ha reemplazado el código JavaScript insertado con datos \*atributos.

    ![Código de validación discreto](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "código de validación discreto")

    *Código de validación discreto*

    > [!NOTE]
    > En este ejemplo, ha visto cómo un resumen con las anotaciones de datos de validación se ha simplificado para unas pocas HTML y JavaScript líneas. Anteriormente, sin validación discreta, los controles de validación más que agregar, cuanto mayor sea el código de validación de JavaScript va a crecer.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarea 2: validar el modelo con las anotaciones de datos

ASP.NET 4.5 presenta la validación de las anotaciones de datos de formularios Web Forms. En lugar de tener un control de validación en cada entrada, ahora puede definir restricciones en las clases de modelo y utilizarlos en la aplicación de web. En esta sección, obtendrá información sobre cómo usar las anotaciones de datos para validar un formulario de cliente nueva/Editar.

1. Abra **CustomerDetail.aspx** página. Tenga en cuenta que el cliente nombre primero y segundo nombre en el **EditItemTemplate** y **InsertItemTemplate** secciones se validan con los controles RequiredFieldValidator. Cada control de validación está asociada a una determinada condición, por lo que necesitará incluir tantos validadores como condiciones que deben cumplir.
2. Agregar anotaciones de datos para validar la clase del modelo de cliente. Abra **Customer.cs** clase en el **modelo** carpeta y *decorar* cada propiedad utilizando atributos de anotación de datos.

    (Código de fragmento de código: *Web Forms anotaciones de datos de laboratorio - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 amplió la colección de anotaciones de datos existente. Estas son algunas de las anotaciones de datos que puede usar: [CreditCard], [Phone], [EmailAddress], [intervalo], [comparar], [Url], [FileExtensions], [Required], [clave], [expresión regular].
    > 
    > Algunos ejemplos de uso:
    > 
    > [clave]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > También puede definir sus propios mensajes de error dentro de cada atributo.
3. Abra **CustomerDetails.aspx** y quite todos los RequiredFieldvalidators para los campos de nombre y apellido en las secciones en EditItemTemplate e InsertItemTemplate del control FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Una ventaja del uso de anotaciones de datos es que la lógica de validación no se duplica en las páginas de aplicación. Define una vez en el modelo y usar en todas las páginas de aplicación que manipulan los datos.
4. Abra **CustomerDetails.aspx** código subyacente y busque el método SaveCustomer. Este método se llama cuando se inserta a un nuevo cliente y recibe el parámetro de cliente de los valores de control de FormView. Cuando la asignación entre los controles de página y el objeto de parámetro se produce, ASP.NET se ejecutará la validación del modelo frente a todas las anotaciones de datos atributos y rellenar el diccionario ModelState con los errores encontrados, si lo hay.

    El ModelState.IsValid solo devolverá true si todos los campos en el modelo son válidos después de realizar la validación.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Agregar un **ValidationSummary** control al final de la página CustomerDetails para mostrar la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    El **ShowModelStateErrors** es una nueva propiedad en el control ValidationSummary control que cuando se establece en **true**, el control mostrará los errores del diccionario ModelState. Estos errores proceden de la validación de las anotaciones de datos.
6. Presione **F5** para ejecutar la aplicación Web. Rellene el formulario con algunos valores erróneos y haga clic en **guardar** para ejecutar la validación. Tenga en cuenta el resumen en la parte inferior del error.

    ![Validación con las anotaciones de datos](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "validación con las anotaciones de datos")

    *Validación con las anotaciones de datos*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarea 3: controlar los errores de base de datos personalizada con ModelState

En la versión anterior de formularios Web Forms, control de errores de base de datos como una cadena demasiado larga o una infracción de clave única puede implicar producir excepciones en el código del repositorio y, a continuación, controlar las excepciones en el código subyacente para mostrar un error. Se requiere una gran cantidad de código para hacer algo relativamente sencilla.

En Web Forms 4.5, el objeto de ModelState puede utilizarse para mostrar los errores en la página, en su modelo o de la base de datos de una manera coherente.

En esta tarea, agregará código para controlar las excepciones de base de datos y mostrar el mensaje adecuado para el usuario mediante el objeto ModelState correctamente.

1. Mientras la aplicación continúa ejecutándose, pruebe a actualizar el nombre de una categoría con un valor duplicado.

    ![Actualizar una categoría con un nombre duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "actualizar una categoría con un nombre duplicado")

    *Actualizar una categoría con un nombre duplicado*

    Tenga en cuenta que se produce una excepción debido a la &quot;único&quot; restricción de la **CategoryName** columna.

    ![La excepción para los nombres de categoría duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "excepción para los nombres de categoría duplicado")

    *Excepción para los nombres de categoría duplicado*
2. Detenga la depuración. En el **Products.aspx.cs** archivo de código subyacente, actualice el **UpdateCategory** método para controlar las excepciones producidas por la base de datos. Método SaveChanges() llame y agregue un error a la **ModelState** objeto.

    El nuevo **TryUpdateModel** método actualiza el objeto de categoría que se recuperan de la base de datos usando los datos de formulario proporcionados por el usuario.

    (Código de fragmento de código: *Web Forms laboratorio - Ex02 - UpdateCategory identificador errores*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, deberá identificar la causa de la DbUpdateException y comprobar si la causa raíz es la infracción de una restricción de clave única.
3. Abra **Products.aspx** y agregue un **ValidationSummary** control por debajo de las categorías de GridView que muestre la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Ejecute el sitio Web y vaya a la página de productos. Pruebe a actualizar el nombre de una categoría con un valor duplicado.

    Tenga en cuenta que se controló la excepción y el mensaje de error aparece en el **ValidationSummary** control.

    ![Duplica el error de categoría](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicado error de categoría")

    *Error de categoría duplicado*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarea 4: validación de formularios Web Forms ASP.NET 4.5 de solicitudes

La característica de validación de solicitud de ASP.NET proporciona un cierto nivel de protección predeterminado frente a ataques de scripting entre sitios (XSS). En versiones anteriores de ASP.NET, la validación de solicitudes estaba habilitada de forma predeterminada y solo puede deshabilitarse para una página completa. Con la nueva versión de ASP.NET Web Forms ahora puede deshabilitar la validación de solicitudes para un control único, realizar la validación de solicitud diferida o tener acceso a datos de solicitud no validados (tenga cuidado si lo!).

1. Presione **CTRL+F5** para iniciar el sitio sin depurar y vaya a la página de productos. Seleccione una categoría y, a continuación, haga clic en el **editar** vínculo en cualquiera de los productos.
2. Escriba una descripción que contienen contenido potencialmente peligroso, por ejemplo incluir etiquetas HTML. Tomar nota de la excepción que se produce debido a la validación de solicitudes.

    ![Edición de un producto con contenido potencialmente peligroso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edición de un producto con contenido potencialmente peligroso")

    *Edición de un producto con contenido potencialmente peligroso*

    ![Excepción que se produce debido a la validación de solicitudes](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "excepción que se produce debido a la validación de solicitudes")

    *Excepción que se produce debido a la validación de solicitudes*
3. Cierre la página y, en Visual Studio, presione **MAYÚS+F5** para detener la depuración.
4. Abra la **ProductDetails.aspx** página y busque el **descripción** cuadro de texto.
5. Agregue el nuevo **ValidateRequestMode** propiedad en el cuadro de texto y establezca su valor en **deshabilitado**.

    El nuevo **ValidateRequestMode** atributo permite deshabilitar la validación de solicitudes granularmente en cada control. Esto es útil cuando desea utilizar una entrada que puede recibir el código HTML, pero desea conservar la validación del trabajo para el resto de la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Presione **F5** para ejecutar la aplicación web. Vuelva a abrir la página de edición del producto y completar una descripción del producto incluidas las etiquetas HTML. Observe que ahora puede agregar contenido HTML a la descripción.

    ![Deshabilitado para la descripción del producto de validación de solicitudes](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "deshabilitado para la descripción del producto de validación de solicitudes")

    *Validación de solicitud deshabilitada para la descripción del producto*

    > [!NOTE]
    > En una aplicación de producción, debe corregir el código HTML especificado por el usuario para asegurarse de que se especifican etiquetas HTML solo seguros (por ejemplo, no hay ningún &lt;script&gt; etiquetas). Para ello, puede usar [biblioteca de protección de Microsoft Web](https://www.nuget.org/packages/AntiXSS).
7. Modifique el producto de nuevo. Escribe el código HTML en el campo nombre y haga clic en **guardar**. Tenga en cuenta que solicitar validación solo está deshabilitada para el campo de descripción y el resto de los campos re todavía se valida con el contenido potencialmente peligroso.

    ![Solicitar habilitada la validación en el resto de los campos](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "habilitada la validación en el resto de los campos de solicitud")

    *Habilitada en el resto de los campos de la validación de solicitud*

    Formularios Web Forms ASP.NET 4.5 incluye un nuevo modo de validación de solicitud para realizar la validación de solicitudes de forma diferida. Con el modo de validación de solicitud establecido en **4.5**, si un fragmento de código tiene acceso a *Request.Form [&quot;clave&quot;]*, desencadenador solo de ASP.NET 4.5 solicitud validación tendrán la validación de solicitudes para ese elemento específico de la colección de formulario.

    Además, ASP.NET 4.5 ahora incluye rutinas de codificación de núcleo de la biblioteca de Microsoft Anti-XSS v4.0. Las rutinas de codificación se implementan mediante la nueva Anti-XSS *AntiXssEncoder* tipo se encuentra en el nuevo **System.Web.Security.AntiXss** espacio de nombres. Con el **encoderType** configurado para usar el parámetro *AntiXssEncoder*, toda la salida automáticamente la codificación en ASP.NET usa las rutinas de codificación de nuevo.
8. Validación de solicitudes 4.5 de ASP.NET también admite el acceso sin validar a los datos de solicitud. ASP.NET 4.5 agrega una nueva propiedad de colección a la **HttpRequest** objeto denominado **Unvalidated**. Cuando se desplaza en **HttpRequest.Unvalidated** tendrá acceso a todas las partes comunes de datos de la solicitud, incluidos los formularios, cadenas, las Cookies, las direcciones URL y así sucesivamente.

    ![Objeto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objeto")

    *Objeto Request.Unvalidated*

    > [!NOTE]
    > **Use la propiedad HttpRequest.Unvalidated con precaución.** Asegúrese de que cuidadosamente realizar validación personalizada en los datos de solicitud sin formato para asegurarse de que hay ida y vuelta no texto peligroso y se representan a los clientes no es sospechosa.

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ejercicio 3: Página asincrónica de procesamiento en ASP.NET Web Forms

En este ejercicio, se presentarán a la nueva página asincrónica procesamiento características en formularios Web Forms de ASP.NET.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Página para cargar y mostrar imágenes de detalles de tarea 1: actualizar el producto

En esta tarea, actualizará la página de detalles del producto para permitir al usuario especificar una dirección URL de imagen para el producto y mostrarlo en la vista de solo lectura. Se creará una copia local de la imagen especificada mediante la descarga de forma sincrónica. En la tarea siguiente, se actualizará esta implementación para que funcione de forma asincrónica.

1. Abra **Visual Studio 2012** y cargar la **comenzar** soluciones se encuentran en **Source\Ex3 Async\Begin** de carpeta de este laboratorio. Como alternativa, puede seguir trabajando en la solución existente de los ejercicios anteriores.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el **WebFormsLab** de proyecto y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo clic en **generar** | **generar solución**.

      > [!NOTE]
      > Una de las ventajas del uso de NuGet es que no tiene que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con NuGet Power Tools, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar la primera vez que ejecute el proyecto de todas las bibliotecas necesarias. Este es el motivo por el que se deben ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra la **ProductDetails.aspx** página origen y agregue un campo en ItemTemplate de FormView para mostrar la imagen del producto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Agregue un campo para especificar la dirección URL de imagen en EditTemplate de FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra la **ProductDetails.aspx.cs** código subyacente de archivos y agregue las siguientes directivas de espacio de nombres.

    (Código de fragmento de código: *Web espacios de nombres de formularios laboratorio - Ex03 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Crear un **UpdateProductImage** método utilizado para almacenar imágenes remotas en el equipo local **imágenes** carpeta y actualización de la entidad de producto con el nuevo valor de ubicación de la imagen.

    (Código de fragmento de código: *Web Forms laboratorio - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Actualización de la **UpdateProduct** método para llamar a la **UpdateProductImage** método.

    (Código de fragmento de código: *Web Forms laboratorio - Ex03 - UpdateProductImage llamada*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Ejecute la aplicación e intente cargar una imagen para un producto. Por ejemplo, puede usar la siguiente dirección URL de imagen de artes de Clip de Office: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![Configuración de una imagen para un producto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "establecer una imagen de un producto")

    *Configuración de una imagen para un producto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarea 2: adición de procesamiento a la página de detalles de producto asincrónico

En esta tarea, actualizará la página de detalles del producto para que funcione de forma asincrónica. Una tarea de ejecución prolongada - el proceso de descarga de imagen - mejorará mediante el uso de procesamiento asincrónico de páginas de ASP.NET 4.5.

Métodos asincrónicos en las aplicaciones web se pueden utilizar para optimizar la forma en que se usan grupos de subprocesos ASP.NET. En ASP.NET no existe se solicita un número limitado de subprocesos en el grupo de subprocesos para prestar atención, por lo tanto, cuando todos los subprocesos están ocupados, ASP.NET empieza a Rechazar nuevas solicitudes, envía mensajes de error de aplicación y hace que el sitio no esté disponible.

Las operaciones que consumen muchos recursos en el sitio web son buenos candidatos para la programación asincrónica, ya que ocupan el subproceso asignado durante mucho tiempo. Esto incluye las solicitudes de ejecución prolongada, páginas con una gran cantidad de los distintos elementos y las páginas que requieren operaciones sin conexión, consultar una base de datos o tener acceso a un servidor web externo. La ventaja es que si utiliza métodos asincrónicos para estas operaciones, mientras se procesa la página, el subproceso se liberan y devueltos al subproceso de grupo y puede utilizarse para asistir a una nueva solicitud de página. Esto significa, la página comenzará a procesar en un subproceso del grupo de subprocesos y puede completar el procesamiento en otro diferente, al finalizar el procesamiento asincrónico.

1. Abra la **ProductDetails.aspx** página. Agregar el **Async** de atributo en el **página** elemento y establézcalo en **true**. Este atributo indica a ASP.NET que implementa la interfaz IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Agregar una etiqueta en la parte inferior de la página para mostrar los detalles de los subprocesos de ejecución de la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra **ProductDetails.aspx.cs** y agregue las siguientes directivas de espacio de nombres.

    (Código de fragmento de código: *Web Forms espacios de nombres de laboratorio - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificar el **UpdateProductImage** método para descargar la imagen con una tarea asincrónica. Reemplazará la **WebClient** **DownloadFile** método con el **DownloadFileTaskAsync** método e incluyen el **await** palabra clave.

    (Código de fragmento de código: *Web Forms laboratorio - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    El RegisterAsyncTask registra una nueva tarea asincrónica de página que se ejecutará en un subproceso diferente. Recibe una expresión lambda con que se ejecute la tarea (t). El **await** palabra clave en el **DownloadFileTaskAsync** método convierte el resto del método en una devolución de llamada que se invoca de forma asincrónica después la **DownloadFileTaskAsync** método ha completado. ASP.NET se reanudará la ejecución del método manteniendo automáticamente todos los valores de solicitud HTTP originales. El nuevo modelo de programación asincrónico en .NET 4.5 permite escribir código asincrónico que se parece mucho a código sincrónico y dejar que el compilador gestione las complicaciones de funciones de devolución de llamada o código de continuación.
    > [!NOTE]
    > RegisterAsyncTask y PageAsyncTask ya estaban disponible desde la versión de .NET 2.0. La palabra clave await es nueva desde el modelo de programación asincrónico de .NET 4.5 y puede utilizarse junto con los nuevos métodos de TaskAsync desde el objeto .NET WebClient.
5. Agregue código para mostrar los subprocesos en el que el código había iniciado y finalizado su ejecución. Para hacerlo, actualice el **UpdateProductImage** método por el código siguiente.

    (Código de fragmento de código: *Web Forms laboratorio - Ex03 - Mostrar subprocesos*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra el sitio web **Web.config** archivo. Agregue la siguiente variable appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Presione **F5** para ejecutar la aplicación y cargar una imagen para el producto. Tenga en cuenta el identificador de subprocesos que el código iniciado y finalizado puede ser diferente. Esto es porque las tareas asincrónicas que se ejecutan en un subproceso independiente del grupo de subprocesos ASP.NET. Cuando se complete la tarea, ASP.NET devuelva la tarea de la cola y asigna cualquiera de los subprocesos disponibles.

    ![Descargar una imagen de forma asincrónica](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "descargar una imagen de forma asincrónica")

    *Descargar una imagen de forma asincrónica*

> [!NOTE]
> Además, puede implementar esta aplicación a Azure siguiente [Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, abordar y muestra los conceptos siguientes:

- Usar expresiones de enlace de datos fuertemente tipados
- Usar nuevas características de enlace de modelo de formularios Web Forms
- Usar proveedores de valores para la asignación de datos de la página a los métodos de código subyacente
- Usar anotaciones de datos para la validación de entrada de usuario
- Tomar advange de validación del lado cliente unobstrusive con jQuery en formularios Web Forms
- Implementar la validación de solicitud específico
- Implementar el procesamiento de formularios Web Forms asincrónico de la página

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión usando la **[instalador de plataforma Web de Microsoft](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* con *instalador de plataforma Web de Microsoft*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). O bien, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con Azure SDK</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tiene **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo primero.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Lea los términos y licencias de todos los productos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Se completó la instalación](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Se completó la instalación*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **Express frente a**&quot;, a continuación, haga clic en el **VS Express para Web** colocar en mosaico.

    ![Express de VS para icono Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *Express de VS para icono Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

Este apéndice le mostrará cómo crear un nuevo sitio web desde el Portal de Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, aprovechando la característica de publicación Web Deploy proporcionada por Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: crear un nuevo sitio Web del Portal de Azure

1. Vaya a la [Portal de administración de Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Azure puede hospedar 10 sitios Web de ASP.NET de forma gratuita y, a continuación, ajustar la escala a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección de URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No se incluyen pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![En el nuevo sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "exploración al sitio web nuevo")

    *Examinar el sitio web nuevo*

    ![Sitio Web que ejecute](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en la **vista rápida** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web en Azure.

    ![Descargar el sitio web de perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "descargar el sitio web de perfil de publicación")

    *Descargar el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo utilizar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación realiza el uso de SQL Server debe crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Azure en **bases de datos Sql** | **servidores** | **panel del servidor**. Si no tiene un servidor que creó, puede crear uno mediante la **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y la dirección URL, nombre de inicio de sesión de administrador y contraseña**, tal y como se va a utilizar en las siguientes tareas. No cree la base de datos, tal y como se creará en una fase posterior.

    ![Panel de servidor de base de datos SQL](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos de Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguela en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) botón.

    ![Agregar dirección IP del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que se ha guardado en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en la **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de la base de datos (es decir, **DefaultConnection**).

    ![Configuración de Web deploy](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configuración de Web deploy")

    *Configuración de Web deploy*
5. Configure la conexión de base de datos de la manera siguiente:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante la *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión de administrador de servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configurar la cadena de conexión de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "configurar la cadena de conexión de destino")

     *Configurar la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le solicite para crear la base de datos, haga clic en **Sí**.

    ![Crear la base de datos](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "crear la cadena de la base de datos")

    *Crear la base de datos*
7. La cadena de conexión que se va a usar para conectarse a la base de datos de SQL en Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **vista previa** página, haga clic en **publicar**.

    ![Publicar la aplicación web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: usar fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le indicará exactamente cuándo utilizarlas, tal como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento (sin espacios ni guiones).
3. Observe como coincidencia de nombres de fragmentos de código de muestra de IntelliSense.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga clic en donde desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante de la lista haciendo clic en él.

![Menú contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga clic en donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Seleccione el fragmento de código relevante de la lista haciendo clic en él](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "elegir el fragmento de código relevante de la lista haciendo clic en él")

*Seleccione el fragmento de código relevante de la lista haciendo clic en él*
