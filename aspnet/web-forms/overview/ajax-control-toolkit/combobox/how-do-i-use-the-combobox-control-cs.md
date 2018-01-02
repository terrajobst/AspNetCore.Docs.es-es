---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: "¿Cómo se puede utilizar el ComboBox Control? (C#) | Documentos de Microsoft"
author: microsoft
description: Cuadro combinado es un control AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones desde el que los usuarios pueden elegir.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 7913affb73c1c314944782ff80cf6c5558502ee9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i-use-the-combobox-control-c"></a>¿Cómo se puede utilizar el ComboBox Control? (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Cuadro combinado es un control AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones desde el que los usuarios pueden elegir.


El objetivo de este tutorial es explicar el control ComboBox de Kit de herramientas de Control de AJAX. El control ComboBox funciona como una combinación entre un control ASP.NET DropDownList estándar y un control de cuadro de texto. Puede seleccionar de una lista de elementos existente o escriba un nuevo elemento.

El cuadro combinado es similar al extensor de control de Autocompletar, pero los controles se utilizan en escenarios diferentes. El extensor AutoComplete consulta un servicio web para obtener entradas coincidentes. El control ComboBox, en cambio, se inicializa con un conjunto de elementos. Mediante la función Autocompletar extender hace que sea sentido cuando se trabaja con un gran conjunto de datos (de hasta millones de piezas de vehículos) al usar el control de cuadro combinado tiene sentido cuando se trabaja con un pequeño conjunto de datos (docenas de partes del automóvil).

## <a name="selecting-from-a-static-list-of-items"></a>Seleccionar de una lista estática de elementos

Permiten s comenzar con un ejemplo sencillo de usar el control ComboBox. Imagine que desea mostrar una lista estática de elementos en una lista desplegable. Sin embargo, desea deje abierta la posibilidad de que la lista no está completa. Desea permitir al usuario especificar un valor personalizado en la lista.

Se ll crear una nueva página de formularios Web Forms de ASP.NET y utilice el control de cuadro combinado en la página. Agregar la nueva página ASP.NET para el proyecto y cambie a la vista de diseño.

Si desea utilizar el control de cuadro combinado en la página, a continuación, debe agregar un control ScriptManager a la página. Arrastre el control ScriptManager de la ficha Extensiones AJAX a la superficie del diseñador. Debe agregar el control ScriptManager en la parte superior de la página; Puede agregar inmediatamente por debajo de la apertura servidor &lt;formulario&gt; etiqueta.

A continuación, arrastre el control de cuadro combinado a la página. Puede encontrar el control de cuadro combinado en el cuadro de herramientas con los demás controles de AJAX Control Toolkit y los extensores de control (consulte la figura 1).


[![Forma sencilla para crear una tarjeta de presentación](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: selecciona el control ComboBox desde el cuadro de herramientas ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Se ll usar el control ComboBox para mostrar una lista estática de opciones. El usuario puede seleccionar un nivel determinado de spiciness para su comida en una lista de tres opciones: suave, mediano y activa (consulte la figura 2).


[![Seleccionar de una lista estática de elementos](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: seleccionar de una lista estática de elementos ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Hay dos maneras que puede agregar estas opciones en el control ComboBox. En primer lugar, seleccione la opción de editar opciones de la tarea cuando mantiene el mouse sobre el control en la vista Diseño y abrir el Editor de elementos (consulte la figura 3).


[![Editar elementos de cuadro combinado](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: elementos de edición ComboBox ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image6.png))


La segunda opción consiste en agregar la lista de elementos entre la apertura y cierre &lt;asp: ComboBox&gt; etiquetas en la vista de origen. La página del listado 1 contiene el ComboBox actualizado que tenga la lista de elementos.

**Lista 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Al abrir la página en la lista 1, puede seleccionar una de las opciones ya existentes en el cuadro combinado. En otras palabras, el control ComboBox funciona igual que un control DropDownList.

Sin embargo, también tiene la opción de escribir una nueva opción (por ejemplo, Super picantes) que no está en la lista existente. Por lo tanto, el control ComboBox también funciona como un control de cuadro de texto.

Independientemente de si se elige preexistente elemento o especificar un elemento personalizado, cuando se envía el formulario, aparece su elección en el control de etiqueta. Cuando se envía el formulario, el btnSubmit\_haga clic en controlador se ejecuta y actualiza la etiqueta (consulte la figura 4).


[![Mostrar el elemento seleccionado](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: mostrar el elemento seleccionado ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image8.png))


El cuadro combinado es compatible con las mismas propiedades que el control DropDownList para recuperar el elemento seleccionado después de que se envía un formulario:

- SelectedItem.Text - muestra el valor de la propiedad de texto del elemento seleccionado.
- SelectedItem.Value - muestra el valor de la propiedad Value del elemento seleccionado o el texto escrito en el cuadro combinado.
- SelectedValue - igual que SelectedItem.Value, salvo que esta propiedad le permite especificar el elemento seleccionado de forma predeterminada (inicial).

Si escribe una opción personalizada en el control ComboBox, a continuación, la opción personalizada se asigna a las propiedades de la SelectedItem.Text y SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Seleccionar la lista de elementos de la base de datos

Puede recuperar la lista de elementos que muestra el control ComboBox de una base de datos. Por ejemplo, puede enlazar el control ComboBox para un control SqlDataSource, un control ObjectDataSource, un LinqDataSource o un EntityDataSource.

Imagine que desea mostrar una lista de películas en un control ComboBox. Van a recuperar la lista de películas de la tabla de base de datos de películas. Siga estos pasos:

1. Crear una página denominada Movies.aspx
2. Agregar un control ScriptManager a la página, arrastre el objeto ScriptManager desde la pestaña Extensiones de AJAX en el cuadro de herramientas hasta la página.
3. Agregar un control de cuadro combinado a la página arrastrando el cuadro combinado en la página.
4. En la vista de diseño, mantenga el mouse sobre el control de cuadro combinado y seleccione el **Elegir origen de datos** opción de tareas (Véase la figura 5). Se inicia el Asistente para configuración de orígenes de datos.
5. En el **elegir un origen de datos** paso, seleccione la &lt;nuevo origen de datos&gt; opción.
6. En el **elegir un tipo de origen de datos** paso a paso, seleccione la base de datos.
7. En el **elegir la conexión de datos** paso a paso, seleccione la base de datos (por ejemplo, MoviesDB.mdf).
8. En el **Guardar cadena de conexión en el archivo de configuración de aplicación** paso a paso, seleccione la opción para guardar la cadena de conexión.
9. En el **configurar la instrucción Select** paso a paso, seleccione la tabla de base de datos de películas y seleccione todas las columnas.
10. En el **consulta prueba** paso a paso, haga clic en el botón Finalizar.
11. En el **Elegir origen de datos** paso, seleccione la columna de título para el campo Mostrar y la columna de Id. de los datos de campo (Véase la figura).
12. Haga clic en el botón Aceptar para cerrar al asistente.


[![Elegir un origen de datos](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: elegir un origen de datos ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Elegir los campos de texto y el valor de datos](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: selección de los campos de texto y el valor de datos ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Después de completar los pasos anteriores, el cuadro combinado está enlazado a un control SqlDataSource que representa las películas de la tabla de base de datos de películas. El origen de la página es similar a listado 2 (limpia el formato un poco).

**La lista 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Observe que el control de cuadro combinado tiene una propiedad DataSourceID que apunta al control SqlDataSource. Al abrir la página en un explorador, se muestra la lista de películas de la base de datos (consulte la figura 7). Puede que una selección una película en la lista o escriba una película nueva escribiendo la película en el cuadro combinado.


[![Muestra una lista de películas](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: mostrar una lista de películas ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Establecer el DropDownStyle

Puede utilizar la propiedad ComboBox DropDownStyle para cambiar el comportamiento del control ComboBox. Esta propiedad se acepta los valores posibles:

- Lista desplegable - muestra el cuadro combinado de (valor predeterminado) una lista desplegable lista al hacer clic en la flecha y puede especificar un valor personalizado.
- Simple: el control ComboBox muestra una lista desplegable automáticamente y puede especificar un valor personalizado.
- DropDownList - ComboBox funciona igual que un control DropDownList.

La diferencia entre la lista desplegable y Simple es cuando se muestra la lista de elementos. En el caso Simple, se muestra la lista inmediatamente al mover el foco al control ComboBox. En el caso de la lista desplegable, debe hacer clic en la flecha para ver la lista de elementos.

El valor de DropDownList hace que el control ComboBox para que funcione como un control DropDownList estándar. Sin embargo, hay una diferencia importante aquí. Las versiones anteriores de Internet Explorer mostrar un control DropDownList con un índice z infinito para que el control aparecerá delante de cualquier control que se coloca delante de él. Dado que el control ComboBox representa un elemento HTML &lt;div&gt; etiqueta en lugar de un elemento HTML &lt;seleccione&gt; etiqueta, el control ComboBox correctamente respeta orden z.

## <a name="setting-the-autocompletemode"></a>Establecer el AutoCompleteMode

Utilice la propiedad ComboBox AutoCompleteMode para especificar lo que sucede cuando un usuario escribe texto en el control ComboBox. Esta propiedad acepta los siguientes valores posibles:

- Ninguno: (valor predeterminado) el ComboBox no proporciona ningún comportamiento de Autocompletar.
- Sugerir - el cuadro combinado muestra la lista y resalta el elemento correspondiente en la lista (consulte la figura 8).
- Anexar: el control ComboBox no muestra la lista y anexa el elemento correspondiente de la lista en lo que ha escrito (consulte la figura 9).
- SuggestAppend - ComboBox tanto muestra la lista y anexa el elemento correspondiente de la lista en lo que ha escrito (consulte la figura 10).


[![Una sugerencia hace que el control ComboBox](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: hace que el control ComboBox una sugerencia ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![ComboBox anexa texto coincidente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: ComboBox anexa texto coincidente ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![El cuadro combinado sugiere y anexa](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: el cuadro combinado sugiere y anexa ([haga clic aquí para ver la imagen a tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Resumen

En este tutorial, aprendió a utilizar el control ComboBox para mostrar un conjunto fijo de elementos. Se enlaza el control ComboBox tanto a una conjunto de elementos de estática a una tabla de base de datos. Por último, aprendió a modificar el comportamiento del control ComboBox estableciendo sus propiedades DropDownStyle y AutoCompleteMode.

>[!div class="step-by-step"]
[Siguiente](how-do-i-use-the-combobox-control-vb.md)
