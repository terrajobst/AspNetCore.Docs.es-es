---
uid: web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
title: Novedades de Web Forms de ASP.NET 4.5 | Microsoft Docs
author: rick-anderson
description: La nueva versión de formularios Web Forms de ASP.NET presenta a una serie de mejoras que se centra en mejorar la experiencia del usuario al trabajar con datos. En versiones anteriores de...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: 0a1f88bd-97da-4ed1-86f1-605199dc75a4
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/hands-on-labs/whats-new-in-web-forms-in-aspnet-45
msc.type: authoredcontent
ms.openlocfilehash: 4e8c8f303851b7f1a01744cab58e27a9b37127a6
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389236"
---
<a name="whats-new-in-web-forms-in-aspnet-45"></a>Novedades de formularios Web Forms en ASP.NET 4.5
====================
por [campamentos Web Team](https://twitter.com/webcamps)

> La nueva versión de formularios Web Forms de ASP.NET presenta a una serie de mejoras que se centra en mejorar la experiencia del usuario al trabajar con datos.
> 
> En versiones anteriores de formularios Web Forms, cuando se usa el enlace de datos para emitir el valor de un miembro de objeto, ha utilizado las expresiones de enlace de datos Bind() o Eval(). En la nueva versión de ASP.NET, es posible declarar qué tipo de datos de un control se va a enlazarse a mediante el uso de una nueva propiedad de tipo de elemento. Al establecer esta propiedad, podrá utilizar una variable fuertemente tipado para recibir todos los beneficios de la experiencia de desarrollo de Visual Studio, como IntelliSense, navegación de miembro y comprobación en tiempo de compilación.
> 
> Con los controles enlazados a datos, ahora también puede especificar sus propios métodos personalizados para seleccionar, actualizar, eliminar e insertar datos, lo que simplifica la interacción entre los controles de página y la lógica de aplicación. Además, se agregaron capacidades de enlace de modelo para ASP.NET, lo que significa que puede asignar datos de la página directamente en los parámetros de tipo de método.
> 
> Validar la entrada del usuario también debe ser más fácil con la versión más reciente de formularios Web Forms. Ahora puede anotar las clases de modelo con atributos de validación de la **System.ComponentModel.DataAnnotations** espacio de nombres y la solicitud que controla todo el sitio validan entrada de usuario mediante esa información. Validación del lado cliente en formularios Web Forms ahora está integrado con jQuery, que proporciona limpiador código del lado cliente y las características de JavaScript discretas.
> 
> En el área de validación de solicitud, se han realizado mejoras para que sea más fácil leer datos de la solicitud no válidas o desactivar la validación de solicitud de partes concretas de las aplicaciones de forma selectiva.
> 
> Se han realizado algunas mejoras a formularios Web Forms controles de servidor para aprovechar las nuevas características de HTML5:
> 
> - La propiedad TextMode del control de cuadro de texto se actualizó para admitir los nuevos tipos de entrada de HTML5 como correo electrónico, fecha y hora y así sucesivamente.
> - El control FileUpload ahora es compatible con varias cargas de archivos desde los exploradores que admiten esta característica de HTML5.
> - Validador controla ahora soporte técnico de validación HTML5 elementos de entrada.
> - Nuevos elementos de HTML5 que tienen atributos que representan una dirección URL ahora admiten runat =&quot;server&quot;. Como resultado, puede usar las convenciones ASP.NET en las rutas de dirección URL, como el ~ operador para representar la raíz de la aplicación (por ejemplo, &lt;vídeo runat =&quot;server&quot; src =&quot;~/myVideo.wmv&quot; &gt; &lt;/video&gt;).
> - Se ha corregido el control UpdatePanel para admitir campos de entrada de registro de HTML5.
> 
> En el portal oficial de ASP.NET se puede encontrar más ejemplos de las nuevas características en ASP.NET Web Forms 4.5: [Novedades de ASP.NET 4.5 y Visual Studio 2012](../../../../whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012.md#_Toc318097385)
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409 ](https://go.microsoft.com/fwlink/?LinkID=248297&clcid=0x409).


<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Usar expresiones de enlace de datos fuertemente tipados
- Usar nuevas características de enlace de modelos en formularios Web Forms
- Usar proveedores de valor para la asignación de datos de la página a los métodos de código subyacente
- Usar anotaciones de datos para la validación de entrada de usuario
- Tomar advange de validación del lado cliente unobstrusive con jQuery en formularios Web Forms
- Implementar la validación de solicitud específico
- Implementar el procesamiento de formularios Web Forms de la página asincrónica

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

Debe tener los elementos siguientes para completar esta práctica:

- [Microsoft Visual Studio Express 2012 para Web](https://www.microsoft.com/visualstudio/eng/products/visual-studio-express-for-web) o superior (leer [Apéndice A](#AppendixA) para obtener instrucciones sobre cómo instalarlo).

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

**Instalación de fragmentos de código**

Para mayor comodidad, gran parte del código que se va a administrar a lo largo de este laboratorio está disponible como fragmentos de código de Visual Studio. Para instalar los fragmentos de código que se ejecute **.\Source\Setup\CodeSnippets.vsi** archivo.

Si no está familiarizado con los fragmentos de código de Visual Studio y desea aprender a usarlas, puede consultar el apéndice de este documento &quot; [Apéndice C: Using Code Snippets](#AppendixC)&quot;.

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los ejercicios siguientes:

1. [Ejercicio 1: Modelo de enlace en formularios Web Forms ASP.NET](#Exercise1)
2. [Ejercicio 2: Validación de datos](#Exercise2)
3. [Ejercicio 3: Página asincrónica de procesamiento en ASP.NET Web Forms](#Exercise3)

> [!NOTE]
> Cada ejercicio está acompañado por un **final** carpeta que contiene la solución resultante debe obtener después de completar los ejercicios. Puede usar esta solución como una guía si necesita ayuda adicional para trabajar a través de los ejercicios.


Tiempo estimado para completar esta práctica: **60 minutos**.

<a id="Exercise1"></a>

<a id="Exercise_1_Model_Binding_in_ASPNET_Web_Forms"></a>
### <a name="exercise-1-model-binding-in-aspnet-web-forms"></a>Ejercicio 1: Modelo de enlace en formularios Web Forms ASP.NET

La nueva versión de formularios Web Forms de ASP.NET presenta a varias mejoras que se centra en mejorar la experiencia al trabajar con datos. En este ejercicio, aprenderá sobre los controles de datos fuertemente tipados y enlace de modelos.

<a id="Task_1_-_Using_Strongly-Typed_Data-Bindings"></a>
#### <a name="task-1---using-strongly-typed-data-bindings"></a>Tarea 1: uso de enlaces de datos fuertemente tipados

En esta tarea, detectará los nuevos inflexibles enlaces disponibles en ASP.NET 4.5.

1. Abra el **comenzar** solución ubicado en **origen/Ex1-ModelBinding/inicio/** carpeta.

   1. Deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, haga clic en el **proyecto** menú y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **Customers.aspx** página. Incluir una lista sin numerar en el control principal e incluir un control repeater dentro para enumerar a cada cliente. Establezca el nombre del control repeater en **customersRepeater** tal como se muestra en el código siguiente.

    En versiones anteriores de formularios Web Forms, cuando se usa el enlace de datos para emitir el valor de un miembro en un objeto es de enlace de datos, se usaría una expresión de enlace de datos, junto con una llamada al método Eval, pasando el nombre del miembro como una cadena.

    En tiempo de ejecución, estas llamadas al valor Eval utilizará reflexión en el objeto enlazado actualmente para leer el valor del miembro con el nombre especificado y mostrar el resultado en el código HTML. Este enfoque resulta muy fácil enlazar datos con datos arbitrarios, unshaped.

    Lamentablemente, se pierden muchas de las características de gran experiencia en tiempo de desarrollo en Visual Studio, incluido IntelliSense para los nombres de miembro, la compatibilidad para la navegación (por ejemplo, ir a definición) y la comprobación de tiempo de compilación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample1.aspx)]
3. Abra el **Customers.aspx.cs** archivo.
4. Agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample2.cs)]
5. En el **página\_carga** método, agregue código para rellenar el control repeater con la lista de clientes.

    (Código de fragmento de código - *Web de origen de datos de los clientes de formularios laboratorio - Ex01 - Bind*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample3.cs)]

    La solución utiliza Entity Framework junto con CodeFirst para crear y tener acceso a la base de datos. En el código siguiente, el customersRepeater se enlaza a una consulta materializada que devuelve a todos los clientes de la base de datos.
6. Presione **F5** para ejecutar la solución y vaya a la **clientes** página para ver el control repeater en acción. Como la solución es usar CodeFirst, se creará la base de datos y se rellena en la instancia de SQL Express local al ejecutar la aplicación.

    ![Lista de los clientes con un control repeater](whats-new-in-web-forms-in-aspnet-45/_static/image1.png "enumerar los clientes con un control repeater")

    *Lista de los clientes con un control repeater*

    > [!NOTE]
    > En Visual Studio 2012, IIS Express es el servidor de desarrollo de Web predeterminado.
7. Cierre el explorador y vuelva a Visual Studio.
8. Ahora, reemplace la implementación para usar enlaces fuertemente tipados. Abra el **Customers.aspx** página y usar el nuevo **ItemType** atributo en el control repeater para establecer el **cliente** tipo como tipo de enlace.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample4.aspx)]

    La propiedad ItemType permite declarar qué tipo de datos el control va a enlazarse a y le permite usar fuertemente tipada dentro del control enlazado a datos de enlace.
9. Reemplace el contenido con el código siguiente ItemTemplate.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample5.aspx)]

    Un inconveniente con los enfoques anteriores es que las llamadas a Eval() y Bind() están enlazadas: es decir, pasar cadenas para representar los nombres de propiedad. Esto significa que no obtiene Intellisense por los nombres de miembro, compatibilidad con la navegación de código (por ejemplo, ir a definición), ni compatibilidad comprobación en tiempo de compilación.

    Establecer la propiedad ItemType hace que dos nuevas variables con tipo que se genere en el ámbito de las expresiones de enlace de datos: **elemento** y **BindItem**. Puede usar estas variables fuertemente tipadas en las expresiones de enlace de datos y obtener los mejores beneficios de la experiencia de desarrollo de Visual Studio.

    El &quot; **:** &quot; utilizados en la expresión configurará automáticamente y codificar en HTML la salida para evitar problemas de seguridad (por ejemplo, ataques de scripting de sitios). Esta notación estaba disponible desde la versión 4 de .NET para escribir de respuesta, pero ahora también está disponible en las expresiones de enlace de datos.

    > [!NOTE]
    > El elemento de miembro funciona para el enlace unidireccional. Si desea realizar un enlace bidireccional use el **BindItem** miembro.

    ![Compatibilidad con IntelliSense en el enlace fuertemente tipado](whats-new-in-web-forms-in-aspnet-45/_static/image2.png "compatibilidad con IntelliSense en el enlace fuertemente tipados")

    *Compatibilidad con IntelliSense en el enlace fuertemente tipados*
10. Presione **F5** para ejecutar la solución y vaya a la página de los clientes para asegurarse de que los cambios funcionan según lo previsto.

    ![Mostrar detalles del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image3.png "lista detalles del cliente")

    *Mostrar detalles del cliente*
11. Cierre el explorador y vuelva a Visual Studio.

<a id="Task_2_-_Introducing_Model_Binding_in_Web_Forms"></a>
#### <a name="task-2---introducing-model-binding-in-web-forms"></a>Tarea 2: modelo de presentación de enlace en formularios Web Forms

En versiones anteriores de ASP.NET Web Forms, cuando desea realizar el enlace de datos bidireccional, tanto al recuperar y actualizar datos, necesita usar un objeto de origen de datos. Podría tratarse de un origen de datos de objeto, un origen de datos SQL, un origen de datos de LINQ y así sucesivamente. Sin embargo si su escenario requiere código personalizado para controlar los datos, era necesario usar el origen de datos de objeto y esto puede poner algunas desventajas. Por ejemplo, es necesario evitar tipos complejos y necesaria para controlar las excepciones cuando se ejecuta la lógica de validación.

En la nueva versión de ASP.NET Web Forms, los controles enlazados a datos admiten el enlace de modelos. Esto significa que puede especificar seleccionar, actualizar, insertar y eliminar métodos directamente en el control enlazado a datos para llamar a la lógica de su archivo de código subyacente o de otra clase.

Para obtener información acerca de esto, usará un control GridView para mostrar las categorías de producto con el nuevo **SelectMethod** atributo. Este atributo permite especificar un método para recuperar los datos de GridView.

1. Abra el **Products.aspx** página e incluyen un **GridView**. Configurar el control GridView, tal como se muestra a continuación, usar enlaces fuertemente tipados y habilitar la ordenación y paginación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample6.aspx)]
2. Use la nueva **SelectMethod** atributo para configurar el control GridView para llamar a un **GetCategories** método para seleccionar los datos.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample7.aspx)]
3. Abra el **Products.aspx.cs** código subyacente de archivo y agregue las siguientes instrucciones using.

    (Código de fragmento de código - *Web espacios de nombres de formularios laboratorio - Ex01 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample8.cs)]
4. Agregar un miembro privado en la **productos** clase y asignar una nueva instancia de **ProductsContext**. Esta propiedad, almacenará el contexto de datos de Entity Framework que permite conectarse a la base de datos.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample9.cs)]
5. Crear un **GetCategories** método para recuperar la lista de categorías mediante LINQ. La consulta incluirá el **productos** propiedad para el control GridView puede mostrar la cantidad de productos de cada categoría. Tenga en cuenta que el método devuelve un objeto IQueryable sin formato que representan la consulta se ejecuta más adelante en el ciclo de vida de la página.

    (Código de fragmento de código - *Web Forms laboratorio - Ex01 - GetCategories*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample10.cs)]

    > [!NOTE]
    > En versiones anteriores de ASP.NET Web Forms, habilitar la ordenación y paginación con su propia lógica de repositorio en un contexto de origen de datos de objeto, necesarios para escribir su propio código personalizado y recibir todos los parámetros necesarios. Ahora, como los métodos de enlace de datos pueden devolver IQueryable y representa una consulta todavía para ejecutarse, ASP.NET puede ocuparse de la modificación de la consulta para agregar una ordenación correcta y parámetros de paginación.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Debería ver que el control GridView se rellena con las categorías devueltas por el método GetCategories.

    ![Rellenar un control GridView con enlace de modelos](whats-new-in-web-forms-in-aspnet-45/_static/image4.png "rellenar un control GridView con enlace de modelos")

    *Rellenar un control GridView con enlace de modelos*
7. Presione **MAYÚS**+**F5** detener la depuración.

<a id="Task_3_-_Value_Providers_in_Model_Binding"></a>
#### <a name="task-3---value-providers-in-model-binding"></a>Tarea 3: los proveedores de valor en el enlace de modelos

Enlace de modelos no sólo le permite especificar los métodos personalizados para trabajar con los datos directamente en el control enlazado a datos, sino que también le permite asignar datos desde la página a los parámetros de estos métodos. En el parámetro de método, puede usar los atributos de proveedor de valor para especificar el origen de datos del valor. Por ejemplo:

- Controles de la página
- Valores de cadena de consulta
- Ver datos
- Estado de sesión
- Cookies
- Datos de formulario publicados
- Estado de vista
- También se admiten proveedores de valores personalizados

Si ha usado ASP.NET MVC 4, observará que la compatibilidad con enlaces del modelo es similar. De hecho, estas características se toman de ASP.NET MVC y mueven a la **System.Web** ensamblado para que pueda usar también en formularios Web Forms.

En esta tarea, actualizará la GridView para filtrar los resultados por la cantidad de productos de cada categoría, recibe el parámetro filter con enlace de modelos.

1. Vuelva a la **Products.aspx** página.
2. En la parte superior del control GridView, agregue un **etiqueta** y un **ComboBox** para seleccionar el número de productos de cada categoría, como se muestra a continuación.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample11.aspx)]
3. Agregar un **EmptyDataTemplate** en el control GridView para mostrar un mensaje cuando no hay ninguna categoría con el número de productos seleccionados.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample12.aspx)]
4. Abra el **Products.aspx.cs** código subyacente y agregue la siguiente instrucción using.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample13.cs)]
5. Modificar el **GetCategories** método para recibir un número entero **minProductsCount** argumento y filtrar los resultados devueltos. Para ello, reemplace el método con el código siguiente.

    (Código de fragmento de código - *Web Forms laboratorio - Ex01 - GetCategories 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample14.cs)]

    El nuevo **[Control]** atributo el **minProductsCount** argumento le permitirá saber que su valor se debe rellenar con un control en la página ASP.NET. ASP.NET buscará cualquier control que coincida con el nombre del argumento (minProductsCount) y realizar la asignación necesaria y conversión para rellenar el parámetro con el valor del control.

    Como alternativa, el atributo proporciona un constructor sobrecargado que le permite especificar el control de dónde obtener el valor.

    > [!NOTE]
    > Es uno de los objetivos de las características de enlace de datos reducir la cantidad de código que debe escribirse para la interacción de la página. Además el proveedor de valores [Control], puede usar otros proveedores de enlace de modelos en los parámetros de método. Algunos de ellos se muestran en la introducción de la tarea.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Seleccione un número de productos en la lista desplegable y observe cómo el control GridView se ha actualizado.

    ![Filtrado de GridView con un valor de la lista desplegable](whats-new-in-web-forms-in-aspnet-45/_static/image5.png "filtrado GridView con un valor de la lista desplegable")

    *Filtrado de GridView con un valor de la lista desplegable*
7. Detenga la depuración.

<a id="Task_4_-_Using_Model_Binding_for_Filtering"></a>
#### <a name="task-4---using-model-binding-for-filtering"></a>Tarea 4: uso de modelo de enlace para el filtrado

En esta tarea, agregará un segundo, secundarios GridView para mostrar los productos dentro de la categoría seleccionada.

1. Abra el **Products.aspx** página y actualizar las categorías de GridView para generar automáticamente el botón de selección.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample15.aspx)]
2. Agregue un segundo **GridView** denominado **productsGrid** en la parte inferior. Establecer el **ItemType** a **WebFormsLab.Model.Product**, el **DataKeyNames** a **ProductId** y **SelectMethod**  a **GetProducts**. Establecer **AutoGenerateColumns** a **false** y agregue las columnas ProductId, ProductName, descripción y UnitPrice.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample16.aspx)]
3. Abra el **Products.aspx.cs** archivo de código subyacente. Implemente el **GetProducts** método para recibir el identificador de categoría de la categoría de GridView y filtrar los productos. Enlace de modelos establecerá el valor del parámetro con la fila seleccionada en el **categoriesGrid**. Puesto que no coincide con el nombre de argumento y el nombre del control, debe especificar el nombre del control en el proveedor de valores de Control.

    (Código de fragmento de código - *Web Forms laboratorio - Ex01 - GetProducts*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample17.cs)]

    > [!NOTE]
    > Este enfoque facilita a unidad de estos métodos de prueba. En un contexto de pruebas unitarias, donde formularios Web Forms no se está ejecutando, el atributo [Control] no realizará ninguna acción específica.
4. Abra el **Products.aspx** página y busque los productos de GridView. Actualizar los productos de GridView para mostrar un vínculo para la edición del producto seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample18.aspx)]
5. Abra el **ProductDetails.aspx** página de código subyacente y reemplace el **SelectProduct** método con el código siguiente.

    (Código de fragmento de código - *Web Forms laboratorio - Ex01 - SelectProduct método*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample19.cs)]

    > [!NOTE]
    > Tenga en cuenta que el **[QueryString]** atributo se utiliza para rellenar el parámetro del método de un parámetro de productId en la cadena de consulta.
6. Presione **F5** para iniciar la depuración en el sitio y vaya a la página de productos. Seleccione cualquier categoría de las categorías de GridView y tenga en cuenta que los productos de GridView se ha actualizado.

    ![Que muestre los productos de la categoría seleccionada](whats-new-in-web-forms-in-aspnet-45/_static/image6.png "que muestre los productos de la categoría seleccionada")

    *Que muestre los productos de la categoría seleccionada*
7. Haga clic en el **vista** vínculo en un producto para abrir la página ProductDetails.aspx.

    Tenga en cuenta que la página está recuperando el producto con SelectMethod mediante el parámetro productId de la cadena de consulta.

    ![Ver los detalles del producto](whats-new-in-web-forms-in-aspnet-45/_static/image7.png "ver los detalles del producto")

    *Ver los detalles del producto*

    > [!NOTE]
    > En el siguiente ejercicio se implementará la capacidad para escribir una descripción en HTML.

<a id="Task_5_-_Using_Model_Binding_for_Update_Operations"></a>
#### <a name="task-5---using-model-binding-for-update-operations"></a>Tarea 5: uso de modelo de enlace para las operaciones de actualización

En la tarea anterior, ha usado el enlace de modelos principalmente para seleccionar datos, en esta tarea obtendrá información sobre cómo usar el enlace de modelos en las operaciones de actualización.

Actualizará las categorías de GridView para permitir al usuario actualizar categorías.

1. Abra el **Products.aspx** página y actualizar las categorías de GridView para generar automáticamente el botón Editar y usar el nuevo **UpdateMethod** atributo para especificar un **UpdateCategory**método para actualizar el elemento seleccionado.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample20.aspx)]

    El atributo DataKeyNames en GridView definir cuáles son los miembros que identifican de forma única el objeto enlazado a un modelo y por lo tanto, que son los parámetros que al menos debe recibir el método update.
2. Abra el **Products.aspx.cs** archivo de código subyacente e implemente el **UpdateCategory** método. El método debería recibir el identificador de categoría para cargar la categoría actual, rellenar los valores de GridView y, a continuación, actualizar la categoría.

    (Código de fragmento de código - *Web Forms laboratorio - Ex01 - UpdateCategory*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample21.cs)]

    El nuevo **TryUpdateModel** método en la clase de página es responsable de rellenar el objeto de modelo mediante los valores de los controles en la página. En este caso, reemplazará los valores actualizados de la fila actual de GridView que se está editando en el **categoría** objeto.

    > [!NOTE]
    > El ejercicio siguiente explica el uso de la ModelState.IsValid para validar los datos introducidos por el usuario al editar el objeto.
3. Ejecute el sitio Web y vaya a la página de productos. Editar una categoría. Escriba un nombre nuevo y, a continuación, haga clic en **actualización** para conservar los cambios.

    ![Edición de categorías](whats-new-in-web-forms-in-aspnet-45/_static/image8.png "edición de categorías")

    *Edición de categorías*

<a id="Exercise2"></a>

<a id="Exercise_2_Data_Validation"></a>
### <a name="exercise-2-data-validation"></a>Ejercicio 2: Validación de datos

En este ejercicio, aprenderá sobre las nuevas características de validación de datos en ASP.NET 4.5. Comprobará las nuevas características de validación discreta en formularios Web Forms. Usará las anotaciones de datos en las clases del modelo de aplicación para la validación de entrada de usuario y, por último, obtendrá información sobre cómo activar o desactivar la validación de solicitudes a controles individuales en una página.

<a id="Task_1_-_Unobtrusive_Validation"></a>
#### <a name="task-1---unobtrusive-validation"></a>Tarea 1: validación discreta

Formas con datos complejos, incluidos los validadores tienden a generar demasiado código JavaScript en la página, que puede representar aproximadamente el 60% del código. Con la validación discreta habilitada, el código HTML tendrá un aspecto más limpio y más limpia.

En esta sección, habilitará validación discreta en ASP.NET para comparar el código HTML generado por ambas configuraciones.

1. Abra **Visual Studio 2012** y abra el **comenzar** solución se encuentra en la **Source\Ex2 Validation\Begin** carpeta de este laboratorio. Como alternativa, puede seguir trabajando en su solución existente desde el ejercicio anterior.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el **WebFormsLab** proyecto **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Presione **F5** para iniciar la aplicación web. Vaya a los clientes de la página y haga clic en el **agregar un nuevo cliente** vínculo.
3. Haga doble clic en la página del explorador y seleccione **ver código fuente** opción para abrir el código HTML generado por la aplicación.

    ![Que muestra la página de código HTML](whats-new-in-web-forms-in-aspnet-45/_static/image9.png "que muestra la página de código HTML")

    *Que muestra la página de código HTML*
4. Desplácese por el código fuente de página y tenga en cuenta que ASP.NET ha insertado los validadores de código y los datos de JavaScript en la página para realizar las validaciones y mostrar la lista de errores.

    ![Código de JavaScript de validación en la página CustomerDetails](whats-new-in-web-forms-in-aspnet-45/_static/image10.png "código JavaScript de validación en la página CustomerDetails")

    *Código de JavaScript de validación en la página CustomerDetails*
5. Cierre el explorador y vuelva a Visual Studio.
6. Ahora habilitará validación discreta. Abra **Web.Config** y busque **ValidationSettings:UnobtrusiveValidationMode** clave en el **AppSettings** sección **.** Establece el valor de clave en **WebForms**.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample22.xml)]

    > [!NOTE]
    > También puede establecer esta propiedad el &quot; **página\_carga** &quot; eventos en el caso de que desea habilitar la validación discreta solo para algunas páginas.
7. Abra **CustomerDetails.aspx** y presione **F5** para iniciar la aplicación Web.
8. Presione la tecla F12 para abrir las herramientas de desarrollo de IE. Una vez que las herramientas de desarrollo está abierto, seleccione la pestaña de script. Seleccione **CustomerDetails.aspx** en el menú y take tenga en cuenta que los scripts necesarios para ejecutar jQuery en la página se han cargado en el explorador desde el sitio local.

    ![Cargando el JavaScript jQuery archivos directamente desde el servidor IIS local](whats-new-in-web-forms-in-aspnet-45/_static/image11.png "cargar jQuery JavaScript archivos directamente desde el servidor IIS local")

    *Al cargar los archivos de JavaScript jQuery directamente desde el servidor IIS local*
9. Cierre el explorador para volver a Visual Studio. Abra el **Site.Master** archivo nuevo y busque el **ScriptManager**. Agregue el atributo **EnableCdn** propiedad con el valor **True**. Esto obligará a jQuery que se cargue desde la dirección URL en línea, pero no desde la dirección URL del sitio local.
10. Abra **CustomerDetails.aspx** en Visual Studio. Presione la tecla F5 para ejecutar el sitio. Una vez que se abre Internet Explorer, presione la tecla F12 para abrir las herramientas de desarrollo. Seleccione el **Script** ficha y, a continuación, eche un vistazo a la lista desplegable. Tenga en cuenta que los archivos de JavaScript jQuery ya no se cargan desde el sitio local, pero en su lugar desde la red CDN de jQuery en línea.

    ![Cargando el JavaScript jQuery archivos desde la red CDN](whats-new-in-web-forms-in-aspnet-45/_static/image12.png "cargar jQuery JavaScript archivos desde la red CDN")

    *Al cargar los archivos de JavaScript jQuery de la red CDN*
11. Abra el código de fuente de página HTML nuevo con la opción de vista de origen en el explorador. Tenga en cuenta que al habilitar la validación discreta ASP.NET ha reemplazado el código JavaScript insertado con datos - \*atributos.

    ![Código de validación discreta](whats-new-in-web-forms-in-aspnet-45/_static/image13.png "código de validación discreta")

    *Código de validación discreta*

    > [!NOTE]
    > En este ejemplo, ha visto cómo un resumen con anotaciones de datos de validación se ha simplificado a solo unas pocas HTML y JavaScript líneas. Anteriormente, sin validación discreta, los controles de validación más que agregar, cuanto mayor sea el código de validación de JavaScript aumentará.

<a id="Task_2_-_Validating_the_Model_with_Data_Annotations"></a>
#### <a name="task-2---validating-the-model-with-data-annotations"></a>Tarea 2: validar el modelo con las anotaciones de datos

ASP.NET 4.5 introduce la validación de formularios Web Forms de anotaciones de datos. En lugar de tener un control de validación en cada entrada, ahora puede definir restricciones en las clases de modelo y úselos en toda la aplicación web. En esta sección, obtendrá información sobre cómo usar anotaciones de datos para validar un formulario de cliente nuevo o editar.

1. Abra **CustomerDetail.aspx** página. Tenga en cuenta que el cliente primero el nombre y el nombre en segundo lugar en el **EditItemTemplate** y **InsertItemTemplate** secciones se validan mediante los controles RequiredFieldValidator. Cada control de validación está asociada a una determinada condición, por lo que deberá incluir tantos validadores como condiciones que deben cumplir.
2. Agregar anotaciones de datos para validar la clase de modelo de Customer. Abra **Customer.cs** clase en el **modelo** carpeta y *decorar* cada propiedad utilizando atributos de anotación de datos.

    (Código de fragmento de código - *Web Forms anotaciones de datos de laboratorio - Ex02 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample23.cs)]

    > [!NOTE]
    > .NET framework 4.5 se amplió la colección de anotaciones de datos existente. Estas son algunas de las anotaciones de datos que puede usar: [CreditCard], [Phone], [EmailAddress], [intervalo], [comparar], [Url], [FileExtensions], [Required], [Key], [RegularExpression].
    > 
    > Algunos ejemplos de uso:
    > 
    > [Key]: Specifies that an attribute is the unique identifier
    > 
    > [Range(0.4, 0.5, ErrorMessage=&quot;{Write an error message}&quot;]: Double range
    > 
    > [EmailAddress(ErrorMessage=&quot;Invalid Email&quot;), MaxLength(56)]: Two annotations in the same line.
    > 
    > También puede definir sus propios mensajes de error dentro de cada atributo.
3. Abra **CustomerDetails.aspx** y quite todas las RequiredFieldvalidators para los campos nombre y apellidos en las secciones en plantillas EditItemTemplate e InsertItemTemplate del control FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample24.aspx)]

    > [!NOTE]
    > Una ventaja del uso de anotaciones de datos es que la lógica de validación no se duplica en páginas de la aplicación. Definir una vez en el modelo y usarlo en todas las páginas de la aplicación que manipulan los datos.
4. Abra **CustomerDetails.aspx** código subyacente y busque el método SaveCustomer. Este método se llama cuando se inserta a un nuevo cliente y recibe el parámetro de cliente de los valores del control FormView. Cuando la asignación entre los controles de página y el objeto de parámetro se produce, ASP.NET se ejecutará la validación del modelo frente a todas las anotaciones de datos de atributos y rellenar el diccionario ModelState con los errores encontrados, si existe.

    El ModelState.IsValid solo devolverá true si todos los campos en el modelo son válidos después de realizar la validación.

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample25.cs)]
5. Agregar un **ValidationSummary** control al final de la página CustomerDetails para mostrar la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample26.aspx)]

    El **ShowModelStateErrors** es una nueva propiedad en el control ValidationSummary controlar que cuando se establece en **true**, el control mostrará los errores del diccionario ModelState. Estos errores salen de la validación de las anotaciones de datos.
6. Presione **F5** para ejecutar la aplicación Web. Complete el formulario con algunos valores erróneos y haga clic en **guardar** para ejecutar la validación. Tenga en cuenta el error que se resumen en la parte inferior.

    ![Validación con las anotaciones de datos](whats-new-in-web-forms-in-aspnet-45/_static/image14.png "validación con las anotaciones de datos")

    *Validación con las anotaciones de datos*

<a id="Task_3_-_Handling_Custom_Database_Errors_with_ModelState"></a>
#### <a name="task-3---handling-custom-database-errors-with-modelstate"></a>Tarea 3: control de errores de base de datos personalizadas con ModelState

En la versión anterior de formularios Web Forms, control de errores de base de datos como una cadena demasiado larga o una infracción de clave única implicaría generar excepciones en el código de repositorio y, a continuación, controlar las excepciones en el código subyacente para mostrar un error. Se requiere una gran cantidad de código para hacer algo relativamente sencilla.

En Web Forms 4.5, el objeto ModelState puede utilizarse para mostrar los errores en la página, o desde el modelo de la base de datos de una manera coherente.

En esta tarea, agregará código para controlar las excepciones de base de datos y mostrar el mensaje adecuado al usuario mediante el objeto ModelState correctamente.

1. Mientras todavía se está ejecutando la aplicación, pruebe a actualizar el nombre de una categoría con un valor duplicado.

    ![Actualización de una categoría con un nombre duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image15.png "actualizar una categoría con un nombre duplicado")

    *Actualización de una categoría con un nombre duplicado*

    Tenga en cuenta que se produce una excepción debido a la &quot;único&quot; puede tomar el **CategoryName** columna.

    ![La excepción para los nombres de categoría duplicado](whats-new-in-web-forms-in-aspnet-45/_static/image16.png "excepción para los nombres de categoría duplicado")

    *Excepción para los nombres de categoría duplicado*
2. Detenga la depuración. En el **Products.aspx.cs** archivo de código subyacente, actualice el **UpdateCategory** método para controlar las excepciones producidas por la base de datos. Método SaveChanges() llamar y agregue un error en la **ModelState** objeto.

    El nuevo **TryUpdateModel** método actualiza el objeto de categoría que se recuperan de la base de datos con los datos del formulario proporcionados por el usuario.

    (Código de fragmento de código - *Web Forms laboratorio - Ex02 - UpdateCategory identificador errores*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample27.cs)]

    > [!NOTE]
    > Idealmente, tendría que identificar la causa de la excepción DbUpdateException y comprobar si la causa raíz es la infracción de restricción de clave única.
3. Abra **Products.aspx** y agregue un **ValidationSummary** por debajo de las categorías de GridView para mostrar la lista de errores del modelo.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample28.aspx)]
4. Ejecute el sitio Web y vaya a la página de productos. Pruebe a actualizar el nombre de una categoría con un valor duplicado.

    Tenga en cuenta que se ha controlado la excepción y el mensaje de error aparece en el **ValidationSummary** control.

    ![Duplicar error categoría](whats-new-in-web-forms-in-aspnet-45/_static/image17.png "duplicado error categoría")

    *Error de categoría duplicado*

<a id="Task_4_-_Request_Validation_in_ASPNET_Web_Forms_45"></a>
#### <a name="task-4---request-validation-in-aspnet-web-forms-45"></a>Tarea 4: solicitud de validación en formularios Web Forms ASP.NET 4.5

La característica de validación de solicitudes en ASP.NET proporciona un cierto nivel de protección predeterminada frente a ataques de scripting entre sitios (XSS). En versiones anteriores de ASP.NET, la validación de solicitudes estaba habilitada de forma predeterminada y solo se pudo deshabilitar de una página completa. Con la nueva versión de ASP.NET Web Forms ahora puede deshabilitar la validación de solicitudes para un solo control, realizar la validación diferida de solicitud o tener acceso a datos de solicitud sin validar (tenga cuidado si lo hace!).

1. Presione **CTRL+F5** para iniciar el sitio sin depurar e ir a la página de productos. Seleccione una categoría y, a continuación, haga clic en el **editar** vínculo de cualquiera de los productos.
2. Escriba una descripción que contiene el contenido potencialmente peligroso, como por ejemplo las etiquetas HTML. Tenga en cuenta la excepción producida debido a la validación de solicitudes.

    ![Edición de un producto con contenido potencialmente peligroso](whats-new-in-web-forms-in-aspnet-45/_static/image18.png "edición de un producto con contenido potencialmente peligroso")

    *Edición de un producto con contenido potencialmente peligroso*

    ![Excepción que se produce debido a la validación de solicitudes](whats-new-in-web-forms-in-aspnet-45/_static/image19.png "excepción debido a la validación de solicitudes")

    *Excepción que se produce debido a la validación de solicitudes*
3. Cierre la página y, en Visual Studio, presione **MAYÚS+F5** para detener la depuración.
4. Abra el **ProductDetails.aspx** página y busque el **descripción** cuadro de texto.
5. Agregue el nuevo **ValidateRequestMode** propiedad en el cuadro de texto y establezca su valor en **deshabilitado**.

    El nuevo **ValidateRequestMode** atributo le permite deshabilitar la validación de solicitudes de forma granular en cada control. Esto es útil cuando desea utilizar una entrada que puede recibir el código HTML, pero desea conservar la validación funciona para el resto de la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample29.aspx)]
6. Presione **F5** para ejecutar la aplicación web. Vuelva a abrir la página de edición del producto y completar una descripción del producto, incluidas las etiquetas HTML. Tenga en cuenta que ahora puede agregar contenido HTML a la descripción.

    ![Solicitar validación deshabilitada para la descripción del producto](whats-new-in-web-forms-in-aspnet-45/_static/image20.png "deshabilitado para la descripción del producto de validación de solicitudes")

    *Validación de solicitudes deshabilitada para la descripción del producto*

    > [!NOTE]
    > En una aplicación de producción, debe corregir el código HTML especificado por el usuario para asegurarse de que se especifican etiquetas HTML solo seguras (por ejemplo, hay ninguna &lt;script&gt; etiquetas). Para ello, puede usar [Microsoft Web Protection Library](https://www.nuget.org/packages/AntiXSS).
7. Modifique el producto de nuevo. Escriba el código HTML en el campo nombre y haga clic en **guardar**. Tenga en cuenta que la validación de solicitudes está deshabilitada solo para el campo de descripción y el resto de los campos re todavía se valida con el contenido potencialmente peligroso.

    ![Habilitado en el resto de los campos de validación de solicitudes](whats-new-in-web-forms-in-aspnet-45/_static/image21.png "habilitada en el resto de los campos de validación de solicitudes")

    *Habilitada en el resto de los campos de la validación de solicitud*

    ASP.NET Web Forms 4.5 incluye un nuevo modo de validación de solicitud para realizar la validación de solicitudes de forma diferida. Con el modo de validación de solicitud establecido en **4.5**, si un fragmento de código accesos *Request.Form [&quot;clave&quot;]*, la validación de solicitudes solicitud validación tendrá que solo se desencadenen de ASP.NET 4.5 para ese elemento específico de la colección de formulario.

    Además, ASP.NET 4.5 ahora incluye rutinas de codificación esenciales de v4.0 de la biblioteca Anti XSS de Microsoft. El Anti-XSS rutinas de codificación se implementan mediante la nuevo *AntiXssEncoder* tipo se encuentra en el nuevo **System.Web.Security.AntiXss** espacio de nombres. Con el **encoderType** configurado para usar el parámetro *AntiXssEncoder*, toda la salida automáticamente la codificación en ASP.NET utiliza las nuevas rutinas de codificación.
8. Validación de solicitudes 4.5 de ASP.NET también admite sin validar el acceso a datos de la solicitud. ASP.NET 4.5 agrega una nueva propiedad de colección a la **HttpRequest** objeto llamado **Unvalidated**. Cuando se desplaza en **HttpRequest.Unvalidated** tendrá acceso a todas las partes comunes de datos de solicitud, incluidos los formularios, las QueryStrings, Cookies, las direcciones URL y así sucesivamente.

    ![Objeto Request.Unvalidated](whats-new-in-web-forms-in-aspnet-45/_static/image22.png "Request.Unvalidated objeto")

    *Objeto Request.Unvalidated*

    > [!NOTE]
    > **Use la propiedad HttpRequest.Unvalidated con precaución.** ¡Asegúrese de que se realizar cuidadosamente la validación personalizada en los datos de solicitud sin procesar para asegurarse de que texto peligroso es ida y vuelta y representan a los clientes confiados no!

<a id="Exercise3"></a>

<a id="Exercise_3_Asynchronous_Page_Processing_in_ASPNET_Web_Forms"></a>
### <a name="exercise-3-asynchronous-page-processing-in-aspnet-web-forms"></a>Ejercicio 3: Página asincrónica de procesamiento en ASP.NET Web Forms

En este ejercicio, le presentaremos a la nueva página asincrónica características en ASP.NET Web Forms de procesamiento.

<a id="Task_1_-_Updating_the_Product_Details_Page_to_Upload_and_Show_Images"></a>
#### <a name="task-1---updating-the-product-details-page-to-upload-and-show-images"></a>Página para cargar y mostrar las imágenes de detalles de tarea 1: actualizar el producto

En esta tarea, actualizará la página de detalles del producto para permitir al usuario especificar una dirección URL de imagen para el producto y mostrarlo en la vista de solo lectura. Creará una copia local de la imagen especificada mediante la descarga de forma sincrónica. En la tarea siguiente, se actualizará esta implementación para que funcione de forma asincrónica.

1. Abra **Visual Studio 2012** y cargar el **comenzar** solución se encuentra en **Source\Ex3 Async\Begin** desde la carpeta de este laboratorio. Como alternativa, puede seguir trabajando en la solución existente de los ejercicios anteriores.

   1. Si abrió proporcionado **comenzar** solución, deberá descargar algunos paquetes de NuGet que faltan antes de continuar. Para ello, en el Explorador de soluciones, haga clic en el **WebFormsLab** del proyecto y seleccione **administrar paquetes de NuGet**.
   2. En el **administrar paquetes de NuGet** cuadro de diálogo, haga clic en **restaurar** para descargar los paquetes que falten.
   3. Por último, compile la solución haciendo **compilar** | **compilar solución**.

      > [!NOTE]
      > Una de las ventajas de usar NuGet es que no tienen que enviar todas las bibliotecas en el proyecto, lo que reduce el tamaño del proyecto. Con las herramientas avanzadas de NuGet, mediante la especificación de las versiones del paquete en el archivo Packages.config, podrá descargar todas las bibliotecas requeridas en la primera vez que ejecute el proyecto. Esto es por eso tendrá que ejecutar estos pasos después de abrir una solución existente de este laboratorio.
2. Abra el **ProductDetails.aspx** página de origen y agregue un campo de FormView ItemTemplate para mostrar la imagen de producto.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample30.aspx)]
3. Agregue un campo para especificar la dirección URL de imagen en EditTemplate de FormView.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample31.aspx)]
4. Abra el **ProductDetails.aspx.cs** código subyacente de archivo y agregue las siguientes directivas de espacio de nombres.

    (Código de fragmento de código - *Web espacios de nombres de formularios laboratorio - Ex03 -*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample32.cs)]
5. Crear un **UpdateProductImage** método para almacenar imágenes remotas local **imágenes** carpeta y actualice la entidad de producto con el nuevo valor de ubicación de la imagen.

    (Código de fragmento de código - *Web Forms laboratorio - Ex03 - UpdateProductImage*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample33.cs)]
6. Actualización de la **UpdateProduct** método para llamar a la **UpdateProductImage** método.

    (Código de fragmento de código - *Web Forms laboratorio - Ex03 - UpdateProductImage llamada*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample34.cs)]
7. Ejecute la aplicación e intenta cargar una imagen para un producto. Por ejemplo, puede usar la siguiente dirección URL de imagen de Office Clip Arts: [[http://officeimg.vo.msecnd.net/images/MB900437099.jpg](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)](http://officeimg.vo.msecnd.net/images/MB900437099.jpg)

    ![El establecimiento de una imagen de un producto](whats-new-in-web-forms-in-aspnet-45/_static/image23.png "establecer una imagen de un producto")

    *El establecimiento de una imagen de un producto*

<a id="Task_2_-_Adding_Asynchronous_Processing_to_the_Product_Details_Page"></a>
#### <a name="task-2---adding-asynchronous-processing-to-the-product-details-page"></a>Tarea 2: adición de procesamiento a la página de detalles del producto asincrónico

En esta tarea, actualizará la página de detalles del producto para que funcione de forma asincrónica. Una tarea de ejecución prolongada - el proceso de descarga de la imagen - mejorará mediante el uso de procesamiento asincrónico de páginas de ASP.NET 4.5.

Los métodos asincrónicos en aplicaciones web pueden usarse para optimizar la forma en que se usan grupos de subprocesos ASP.NET. En ASP.NET allí se solicita un número limitado de subprocesos en el grupo de subprocesos por su asistencia, por lo tanto, cuando todos los subprocesos están ocupados, ASP.NET empieza a Rechazar nuevas solicitudes, envía mensajes de error de aplicación y dejará de estar disponible el sitio.

Las operaciones que requieren mucho tiempo en el sitio web son buenos candidatos para la programación asincrónica, ya que ocupan el subproceso asignado durante mucho tiempo. Esto incluye las solicitudes de larga ejecución, las páginas con una gran cantidad de distintos elementos y las páginas que requieren operaciones sin conexión, este tipo consultar una base de datos o tiene acceso a un servidor web externo. La ventaja es que si usa los métodos asincrónicos para estas operaciones, mientras se procesa la página, el subproceso se libera y vuelve al subproceso del grupo y puede utilizarse para asistir a una nueva solicitud de página. Esto significa que, la página comenzará a procesar en un subproceso del grupo de subprocesos y puede completar el procesamiento en otro diferente, una vez completado el procesamiento asincrónico.

1. Abra el **ProductDetails.aspx** página. Agregar el **Async** atributo en el **página** elemento y establézcalo en **true**. Este atributo indica a ASP.NET que implemente la interfaz de IHttpAsyncHandler.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample35.aspx)]
2. Agregue una etiqueta en la parte inferior de la página para mostrar los detalles de los subprocesos de ejecución de la página.

    [!code-aspx[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample36.aspx)]
3. Abra **ProductDetails.aspx.cs** y agregue las siguientes directivas de espacio de nombres.

    (Código de fragmento de código - *Web Forms espacios de nombres de laboratorio - Ex03 - 2*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample37.cs)]
4. Modificar el **UpdateProductImage** método para descargar la imagen con una tarea asincrónica. Reemplazará la **WebClient** **DownloadFile** método con el **DownloadFileTaskAsync** método e incluyen el **await** palabra clave.

    (Código de fragmento de código - *Web Forms laboratorio - Ex03 - UpdateProductImage Async*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample38.cs)]

    El RegisterAsyncTask registra una nueva tarea asincrónica de página que se ejecutará en un subproceso diferente. Recibe una expresión lambda con la que se ejecute la tarea (t). El **await** palabra clave en el **DownloadFileTaskAsync** método convierte el resto del método en una devolución de llamada que se invoca de forma asincrónica después la **DownloadFileTaskAsync** método ha finalizado. ASP.NET se reanudará la ejecución del método con el mantenimiento automático de todos los valores de solicitud HTTP originales. El nuevo modelo de programación asincrónico en .NET Framework 4.5 permite escribir código asincrónico que se parece mucho al código sincrónico y dejar que el compilador controle las complicaciones de funciones de devolución de llamada o código de continuación.
    > [!NOTE]
    > RegisterAsyncTask y PageAsyncTask ya estaban disponible desde la versión de .NET 2.0. La palabra clave await es nueva desde el modelo de programación asincrónico de .NET 4.5 y puede utilizarse junto con los nuevos métodos TaskAsync desde el objeto .NET WebClient.
5. Agregue código para mostrar los subprocesos en el que el código había iniciado y finalizado la ejecución. Para ello, actualice el **UpdateProductImage** método con el código siguiente.

    (Código de fragmento de código - *subprocesos de Web Forms Lab - Ex03 - Show*)

    [!code-csharp[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample39.cs)]
6. Abra el sitio web **Web.config** archivo. Agregue la siguiente variable de appSetting.

    [!code-xml[Main](whats-new-in-web-forms-in-aspnet-45/samples/sample40.xml)]
7. Presione **F5** para ejecutar la aplicación y cargar una imagen para el producto. Observe el identificador de subprocesos que el código que se inició y finalizó el puede ser diferente. Esto es porque las tareas asincrónicas que se ejecutan en un subproceso independiente del grupo de subprocesos ASP.NET. Cuando se complete la tarea, ASP.NET coloca la tarea en la cola y asigna cualquiera de los subprocesos disponibles.

    ![Descargar una imagen de forma asincrónica](whats-new-in-web-forms-in-aspnet-45/_static/image24.png "descargar una imagen de forma asincrónica")

    *Descargar una imagen de forma asincrónica*

> [!NOTE]
> Además, puede implementar esta aplicación a Azure siguiente [Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy](#AppendixB).


* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, se han solucionado y muestra los conceptos siguientes:

- Usar expresiones de enlace de datos fuertemente tipados
- Usar nuevas características de enlace de modelos en formularios Web Forms
- Usar proveedores de valor para la asignación de datos de la página a los métodos de código subyacente
- Usar anotaciones de datos para la validación de entrada de usuario
- Tomar advange de validación del lado cliente unobstrusive con jQuery en formularios Web Forms
- Implementar la validación de solicitud específico
- Implementar el procesamiento de formularios Web Forms de la página asincrónica

<a id="AppendixA"></a>

<a id="Appendix_A_Installing_Visual_Studio_Express_2012_for_Web"></a>
## <a name="appendix-a-installing-visual-studio-express-2012-for-web"></a>Apéndice A: instalación de Visual Studio Express 2012 para Web

Puede instalar **Microsoft Visual Studio Express 2012 para Web** u otro &quot;Express&quot; versión utilizando el **[Microsoft Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)**. Las instrucciones siguientes le guían a través de los pasos necesarios para instalar *Visual studio Express 2012 para Web* mediante *Microsoft Web Platform Installer*.

1. Vaya a [ [ https://go.microsoft.com/?linkid=9810169 ](https://go.microsoft.com/?linkid=9810169) ](https://go.microsoft.com/?linkid=9810169). Como alternativa, si ya ha instalado el instalador de plataforma Web, puede abrirla y busque el producto &quot; <em>Visual Studio Express 2012 for Web con Azure SDK</em>&quot;.
2. Haga clic en **instalar ahora**. Si no tienes **instalador de plataforma Web** se le redirigirá para descargarlo e instalarlo en primer lugar.
3. Una vez **instalador de plataforma Web** está abierto, haga clic en **instalar** para iniciar el programa de instalación.

    ![Instale Visual Studio Express](whats-new-in-web-forms-in-aspnet-45/_static/image25.png "instale Visual Studio Express")

    *Instale Visual Studio Express*
4. Leer todos los productos las licencias y los términos y haga clic en **acepto** para continuar.

    ![Acepte los términos de licencia](whats-new-in-web-forms-in-aspnet-45/_static/image26.png)

    *Acepte los términos de licencia*
5. Espere hasta que finalice el proceso de descarga e instalación.

    ![Progreso de la instalación](whats-new-in-web-forms-in-aspnet-45/_static/image27.png)

    *Progreso de la instalación*
6. Cuando se complete la instalación, haga clic en **finalizar**.

    ![Instalación completada](whats-new-in-web-forms-in-aspnet-45/_static/image28.png)

    *Instalación completada*
7. Haga clic en **Exit** para cerrar el instalador de plataforma Web.
8. Para abrir Visual Studio Express para Web, vaya a la **iniciar** pantalla y comienza a escribir &quot; **VS Express**&quot;, a continuación, haga clic en el **VS Express para Web** icono.

    ![VS Express para el icono de Web](whats-new-in-web-forms-in-aspnet-45/_static/image29.png)

    *VS Express para el icono de Web*

<a id="AppendixB"></a>

<a id="Appendix_B_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
## <a name="appendix-b-publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Apéndice B: publicar una aplicación ASP.NET MVC 4 mediante Web Deploy

En este apéndice se mostrará cómo crear un nuevo sitio web desde el Portal de Azure y publicar la aplicación que obtuvo siguiendo el laboratorio, que aprovecha la característica de publicación Web Deploy proporcionada por Azure.

<a id="ApxBTask1"></a>

<a id="Task_1_-_Creating_a_New_Web_Site_from_the_Windows_Azure_Portal"></a>
#### <a name="task-1---creating-a-new-web-site-from-the-azure-portal"></a>Tarea 1: crear un nuevo sitio Web desde el Portal de Azure

1. Vaya a la [Portal de administración de Azure](https://manage.windowsazure.com/) e inicie sesión con las credenciales de Microsoft asociadas con su suscripción.

    > [!NOTE]
    > Con Azure puede hospedar 10 sitios Web ASP.NET de forma gratuita y, a continuación, escalar a medida que crece el tráfico. Puede registrarse [aquí](http://aka.ms/aspnet-hol-azure).

    ![Inicie sesión en el portal de Windows Azure](whats-new-in-web-forms-in-aspnet-45/_static/image30.png "inicie sesión en el portal de Windows Azure")

    *Inicie sesión en el Portal*
2. Haga clic en **New** en la barra de comandos.

    ![Crear un nuevo sitio Web](whats-new-in-web-forms-in-aspnet-45/_static/image31.png "crear un nuevo sitio Web")

    *Crear un nuevo sitio Web*
3. Haga clic en **proceso** | **sitio Web**. A continuación, seleccione **creación rápida** opción. Proporcione una dirección URL disponible para el nuevo sitio web y haga clic en **crear sitio Web**.

    > [!NOTE]
    > Azure es el host para una aplicación web que se ejecutan en la nube que puede controlar y administrar. La opción Creación rápida permite implementar una aplicación web completada en Azure desde fuera del portal. No incluye los pasos para configurar una base de datos.

    ![Crear un nuevo sitio Web mediante Creación rápida](whats-new-in-web-forms-in-aspnet-45/_static/image32.png "crear un nuevo sitio Web mediante Creación rápida")

    *Crear un nuevo sitio Web mediante Creación rápida*
4. Espere hasta que el nuevo **sitio Web** se crea.
5. Una vez creado el sitio Web, haga clic en el vínculo situado bajo el **URL** columna. Compruebe que funciona el nuevo sitio Web.

    ![Exploración al sitio web nuevo](whats-new-in-web-forms-in-aspnet-45/_static/image33.png "exploración al sitio web nuevo")

    *Exploración al sitio web nuevo*

    ![Sitio Web que se ejecuta](whats-new-in-web-forms-in-aspnet-45/_static/image34.png "sitio Web que se ejecuta")

    *Sitio Web que se ejecuta*
6. Vuelva al portal y haga clic en el nombre del sitio web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración del sitio web](whats-new-in-web-forms-in-aspnet-45/_static/image35.png "abrir las páginas de administración del sitio web")

    *Abrir las páginas de administración del sitio Web*
7. En el **panel** página, en el **primea** sección, haga clic en el **descargar perfil de publicación** vínculo.

    > [!NOTE]
    > El *perfil de publicación* contiene toda la información necesaria para publicar una aplicación web en Azure para cada método de publicación habilitado. El perfil de publicación contiene las direcciones URL, las credenciales de usuario y las cadenas de base de datos necesarias para conectarse y autenticarse en cada uno de los puntos de conexión para el que está habilitado un método de publicación. **Microsoft WebMatrix 2**, **Microsoft Visual Studio Express para Web** y **Microsoft Visual Studio 2012** admiten la lectura de perfiles de publicación para automatizar la configuración de estos programas para publicación de aplicaciones web en Azure.

    ![Descargando el sitio web de perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image36.png "descargando el sitio web de perfil de publicación")

    *Descargando el sitio Web de perfil de publicación*
8. Descargue el archivo de perfil de publicación en una ubicación conocida. Aún más en este ejercicio verá cómo usar este archivo para publicar una aplicación web en Azure desde Visual Studio.

    ![Guardar el archivo de perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image37.png "guardar el perfil de publicación")

    *Guardar el archivo de perfil de publicación*

<a id="ApxBTask2"></a>

<a id="Task_2_-_Configuring_the_Database_Server"></a>
#### <a name="task-2---configuring-the-database-server"></a>Tarea 2: configurar el servidor de base de datos

Si la aplicación hace uso de SQL Server deberá crear un servidor de base de datos SQL de bases de datos. Si desea implementar una aplicación simple que no utiliza SQL Server, podría omitir esta tarea.

1. Necesitará un servidor de base de datos SQL para almacenar la base de datos de aplicación. Puede ver los servidores de base de datos SQL de la suscripción en el portal de administración de Azure en **bases de datos Sql** | **servidores** | **panel del servidor**. Si no tiene un servidor creado, puede crear una mediante el **agregar** botón en la barra de comandos. Tome nota de la **nombre del servidor y dirección URL, nombre de inicio de sesión de administrador y contraseña**, ya que lo usará en las siguientes tareas. No cree la base de datos, como se crearán en una etapa posterior.

    ![Panel de base de datos SQL Server](whats-new-in-web-forms-in-aspnet-45/_static/image38.png "panel de base de datos SQL Server")

    *Panel de base de datos SQL Server*
2. En la siguiente tarea probará la conexión de base de datos desde Visual Studio, por ese motivo debe incluir la dirección IP local en la lista del servidor de **direcciones IP permitidas**. Para ello, haga clic en **configurar**, seleccione la dirección IP de **dirección IP del cliente actual** y péguelo en el **dirección IP inicial** y **ladirecciónIPfinal** cuadros de texto y haga clic en el ![add-client-ip-address-ok-button](whats-new-in-web-forms-in-aspnet-45/_static/image39.png) botón.

    ![Agregar dirección IP del cliente](whats-new-in-web-forms-in-aspnet-45/_static/image40.png)

    *Agregar dirección IP del cliente*
3. Una vez el **dirección IP del cliente** se agrega a las direcciones IP permitidas de lista, haga clic en **guardar** para confirmar los cambios.

    ![Confirmar cambios](whats-new-in-web-forms-in-aspnet-45/_static/image41.png)

    *Confirmar cambios*

<a id="ApxBTask3"></a>

<a id="Task_3_-_Publishing_an_ASPNET_MVC_4_Application_using_Web_Deploy"></a>
#### <a name="task-3---publishing-an-aspnet-mvc-4-application-using-web-deploy"></a>Tarea 3: publicar una aplicación de ASP.NET MVC 4 mediante Web Deploy

1. Vuelva a la solución de ASP.NET MVC 4. En el **el Explorador de soluciones**, haga clic en el proyecto de sitio web y seleccione **publicar**.

    ![Publicar la aplicación](whats-new-in-web-forms-in-aspnet-45/_static/image42.png "publicar la aplicación")

    *Publicar el sitio web*
2. Importar el perfil de publicación que guardó en la primera tarea.

    ![Importar el perfil de publicación](whats-new-in-web-forms-in-aspnet-45/_static/image43.png "importar el perfil de publicación")

    *Importar perfil de publicación*
3. Haga clic en **validar conexión**. Una vez completada la validación, haga clic en **siguiente**.

    > [!NOTE]
    > Validación está completa cuando aparece una marca de verificación verde junto al botón Validar conexión.

    ![Validación de la conexión](whats-new-in-web-forms-in-aspnet-45/_static/image44.png "validación de la conexión")

    *Validación de la conexión*
4. En el **configuración** página, en el **bases de datos** sección, haga clic en el botón situado junto al cuadro de texto de la conexión de base de datos (es decir, **DefaultConnection**).

    ![Configuración de implementación Web](whats-new-in-web-forms-in-aspnet-45/_static/image45.png "configuración de implementación Web")

    *Configuración de implementación Web*
5. Configure la conexión de base de datos como sigue:

   - En el **nombre del servidor** escriba la dirección URL de base de datos de SQL server mediante el *tcp:* prefijo.
   - En **nombre de usuario** escriba el nombre de inicio de sesión del Administrador de servidor.
   - En **contraseña** escriba la contraseña de inicio de sesión del Administrador de servidor.
   - Escriba un nuevo nombre de base de datos.

     ![Configuración de la cadena de conexión de destino](whats-new-in-web-forms-in-aspnet-45/_static/image46.png "configuración de la cadena de conexión de destino")

     *Configuración de la cadena de conexión de destino*
6. A continuación, haga clic en **Aceptar**. Cuando se le pedirá que cree la base de datos, haga clic en **Sí**.

    ![Creación de la base de datos](whats-new-in-web-forms-in-aspnet-45/_static/image47.png "creación de la cadena de la base de datos")

    *Creación de la base de datos*
7. La cadena de conexión que se usará para conectarse a SQL Database en Azure se muestra en el cuadro de texto de conexión predeterminado. Después, haga clic en **Siguiente**.

    ![Cadena de conexión que apunte a la base de datos SQL](whats-new-in-web-forms-in-aspnet-45/_static/image48.png "cadena de conexión que apunte a la base de datos SQL")

    *Cadena de conexión que apunte a la base de datos SQL*
8. En el **Preview** página, haga clic en **publicar**.

    ![Publicar la aplicación web](whats-new-in-web-forms-in-aspnet-45/_static/image49.png "publicar la aplicación web")

    *Publicar la aplicación web*
9. Una vez que finalice el proceso de publicación, el explorador predeterminado abrirá el sitio web publicado.

<a id="AppendixC"></a>

<a id="Appendix_C_Using_Code_Snippets"></a>
## <a name="appendix-c-using-code-snippets"></a>Apéndice C: uso de fragmentos de código

Con fragmentos de código, tiene todo el código que necesita a su alcance. El documento de laboratorio le dirá exactamente cuándo se pueden utilizar, como se muestra en la ilustración siguiente.

![Uso de fragmentos de código de Visual Studio para insertar código en el proyecto](whats-new-in-web-forms-in-aspnet-45/_static/image50.png "fragmentos de código en Visual Studio para insertar código en el proyecto")

*Uso de fragmentos de código de Visual Studio para insertar código en el proyecto*

***Para agregar un fragmento de código mediante el teclado (solo C#)***

1. Coloque el cursor donde desea insertar el código.
2. Comience a escribir el nombre del fragmento de código (sin espacios ni guiones).
3. Observe cómo IntelliSense muestra los nombres de fragmentos de código coincidentes.
4. Seleccione el fragmento de código correcto (o siga escribiendo hasta que se selecciona el nombre del fragmento de código completo).
5. Presione la tecla Tab dos veces para insertar el fragmento de código en la ubicación del cursor.

![Comience a escribir el nombre del fragmento](whats-new-in-web-forms-in-aspnet-45/_static/image51.png "comience a escribir el nombre del fragmento de código")

*Comience a escribir el nombre del fragmento de código*

![Presione la tecla Tab para seleccionar el fragmento de código resaltada](whats-new-in-web-forms-in-aspnet-45/_static/image52.png "presione Tab para seleccionar el fragmento de código resaltada")

*Presione la tecla Tab para seleccionar el fragmento de código resaltada*

![Vuelva a presionar Tab y el fragmento de código se expandirán](whats-new-in-web-forms-in-aspnet-45/_static/image53.png "vuelva a presionar Tab y el fragmento de código se expandirán")

*Vuelva a presionar Tab y el fragmento de código se expandirán*

***Para agregar un fragmento de código con el mouse (C#, en Visual Basic y XML)*** 1. Haga doble clic en el desea insertar el fragmento de código.

1. Seleccione **Insertar fragmento de código** seguido **Mis fragmentos de código**.
2. Seleccione el fragmento de código relevante en la lista, haciendo clic en él.

![Con el botón secundario donde desea insertar el fragmento de código y seleccione Insertar fragmento de código](whats-new-in-web-forms-in-aspnet-45/_static/image54.png "contextual donde desea insertar el fragmento de código y seleccione Insertar fragmento de código")

*Haga doble clic donde desea insertar el fragmento de código y seleccione Insertar fragmento de código*

![Elegir el fragmento de código relevante en la lista, hacer clic en ella](whats-new-in-web-forms-in-aspnet-45/_static/image55.png "elegir el fragmento de código relevante en la lista, hacer clic en ella")

*Elegir el fragmento de código relevante en la lista, hacer clic en ella*
