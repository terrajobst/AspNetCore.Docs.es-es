---
uid: web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
title: ¿Cómo se puede utilizar el Control de cuadro combinado? (C#) | Microsoft Docs
author: microsoft
description: Cuadro combinado es un control de AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones desde el que los usuarios pueden elegir.
ms.author: riande
ms.date: 05/12/2009
ms.assetid: 0bbf4134-04df-4226-8930-d5bb99e27128
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/combobox/how-do-i-use-the-combobox-control-cs
msc.type: authoredcontent
ms.openlocfilehash: edf3786600a8ec7b58422e1ec20e71e2b749d6e4
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824088"
---
<a name="how-do-i-use-the-combobox-control-c"></a>¿Cómo se puede utilizar el Control de cuadro combinado? (C#)
====================
por [Microsoft](https://github.com/microsoft)

> Cuadro combinado es un control de AJAX de ASP.NET que combina la flexibilidad de un cuadro de texto con una lista de opciones desde el que los usuarios pueden elegir.


El objetivo de este tutorial es explicar el control de cuadro combinado de AJAX Control Toolkit. El cuadro combinado funciona como una combinación entre un control DropDownList ASP.NET estándar y un control de cuadro de texto. Puede seleccionar de una lista de elementos ya existente o escriba un nuevo elemento.

El cuadro combinado es similar para el extensor de control de Autocompletar, pero los controles se usan en escenarios diferentes. El extensor AutoComplete consulta a un servicio web para obtener entradas coincidentes. En cambio, el control de cuadro combinado, se inicializa con un conjunto de elementos. Usar el extensor de Autocompletar hace que tiene sentido cuando se trabaja con un gran conjunto de datos (millones de piezas de automóviles) al usar el control de cuadro combinado tiene sentido cuando se trabaja con un pequeño conjunto de datos (decenas de piezas de automóviles).

## <a name="selecting-from-a-static-list-of-items"></a>Seleccionar de una lista estática de elementos

Permiten s empiece con un ejemplo sencillo de utilizar el control de cuadro combinado. Imagine que desea mostrar una lista estática de elementos en una lista desplegable. Sin embargo, desea dejar abierta la posibilidad de que la lista no está completa. Desea permitir que un usuario escriba un valor personalizado en la lista.

Nos ll, cree una nueva página de formularios Web Forms ASP.NET y utilizar el control de cuadro combinado en la página. Agregar la nueva página ASP.NET al proyecto y cambie a la vista Diseño.

Si desea utilizar el control de cuadro combinado en la página, a continuación, debe agregar un control ScriptManager a la página. Arrastre el control ScriptManager de la pestaña Extensiones de AJAX a la superficie del diseñador. Debe agregar el control ScriptManager en la parte superior de la página. Puede agregar inmediatamente debajo de la apertura del servidor &lt;formulario&gt; etiqueta.

A continuación, arrastre el control de cuadro combinado a la página. Puede encontrar el control de cuadro combinado en el cuadro de herramientas con los demás controles de AJAX Control Toolkit y los extensores de control (consulte la figura 1).


[![Formulario simple para crear una tarjeta de presentación](how-do-i-use-the-combobox-control-cs/_static/image1.jpg)](how-do-i-use-the-combobox-control-cs/_static/image1.png)

**Figura 01**: seleccionar el control ComboBox desde el cuadro de herramientas ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image2.png))


Se deberá usar el control ComboBox para mostrar una lista estática de opciones. El usuario puede seleccionar un nivel determinado de haber para su comida desde una lista de tres opciones: suave, medio y nivel de acceso frecuente (consulte la figura 2).


[![Seleccionar de una lista estática de elementos](how-do-i-use-the-combobox-control-cs/_static/image2.jpg)](how-do-i-use-the-combobox-control-cs/_static/image3.png)

**Figura 02**: selección de una lista estática de elementos ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image4.png))


Hay dos formas que puede agregar estas opciones en el control ComboBox. En primer lugar, seleccione la opción de editar opciones de la tarea cuando mantiene el mouse sobre el control en la vista Diseño y abra el Editor de elementos (consulte la figura 3).


[![Editar elementos de cuadro combinado](how-do-i-use-the-combobox-control-cs/_static/image3.jpg)](how-do-i-use-the-combobox-control-cs/_static/image5.png)

**Figura 03**: elementos de cuadro combinado de edición ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image6.png))


La segunda opción es agregar la lista de elementos entre la apertura y cierre &lt;asp: ComboBox&gt; etiquetas en la vista de origen. La página del listado 1 contiene el cuadro combinado actualizado que tiene la lista de elementos.

**Listado 1 - Static.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample1.aspx)]

Al abrir la página del listado 1, puede seleccionar una de las opciones ya existentes en el cuadro combinado. En otras palabras, el cuadro combinado funciona igual que un control DropDownList.

Sin embargo, también tiene la opción de escribir una nueva opción (por ejemplo, Super picantes) que no está en la lista existente. Por lo tanto, el cuadro combinado también funciona como un control de cuadro de texto.

Independientemente de si elige preexistente elemento o especificar un elemento personalizado, al enviar el formulario, aparece su elección en el control de etiqueta. Cuando se envía el formulario, el btnSubmit\_haga clic en controlador se ejecuta y actualiza la etiqueta (consulte la figura 4).


[![Mostrar el elemento seleccionado](how-do-i-use-the-combobox-control-cs/_static/image4.jpg)](how-do-i-use-the-combobox-control-cs/_static/image7.png)

**Figura 04**: mostrar el elemento seleccionado ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image8.png))


El cuadro combinado es compatible con las mismas propiedades que el control DropDownList para recuperar el elemento seleccionado después de que se envíe un formulario:

- SelectedItem.Text - muestra el valor de la propiedad Text del elemento seleccionado.
- SelectedItem.Value - muestra el valor de la propiedad Value del elemento seleccionado o el texto escrito en el cuadro combinado.
- SelectedValue: igual que SelectedItem.Value excepto que esta propiedad le permite especificar el elemento seleccionado de forma predeterminada (inicial).

Si escribe una opción personalizada en el control ComboBox, a continuación, la opción personalizado se asigna a las propiedades de la SelectedItem.Text y SelectedItem.Value.

## <a name="selecting-the-list-of-items-from-the-database"></a>Seleccionar la lista de elementos de la base de datos

Puede recuperar la lista de elementos que muestra el cuadro combinado de una base de datos. Por ejemplo, puede enlazar el cuadro combinado a un control SqlDataSource, un control ObjectDataSource, un LinqDataSource o un EntityDataSource.

Imagine que desea mostrar una lista de películas en un cuadro combinado. Desea recuperar la lista de películas de la tabla de base de datos de películas. Siga estos pasos:

1. Cree una página denominada Movies.aspx
2. Agregar un control ScriptManager a la página arrastrando el control ScriptManager en la pestaña Extensiones de AJAX en el cuadro de herramientas hasta la página.
3. Agregue un control de cuadro combinado a la página, arrastre el cuadro combinado a la página.
4. En la vista Diseño, mantenga el mouse sobre el control de cuadro combinado y seleccione el **Elegir origen de datos** opción de tarea (consulte la figura 5). Se inicia el Asistente para configuración de orígenes de datos.
5. En el **elegir un origen de datos** paso, seleccione la &lt;nuevo origen de datos&gt; opción.
6. En el **elegir un tipo de origen de datos** paso, seleccione la base de datos.
7. En el **elegir la conexión de datos** paso, seleccione la base de datos (por ejemplo, MoviesDB.mdf).
8. En el **Guardar cadena de conexión en el archivo de configuración de aplicación** paso, seleccione la opción para guardar la cadena de conexión.
9. En el **configurar la instrucción Select** paso, seleccione la tabla de base de datos de películas y seleccione todas las columnas.
10. En el **consulta prueba** paso, haga clic en el botón Finalizar.
11. En el **Elegir origen de datos** paso, seleccione la columna de título para el campo Mostrar y la columna de Id. de los datos de campo (consulte la figura).
12. Haga clic en el botón Aceptar para cerrar al asistente.


[![Elegir un origen de datos](how-do-i-use-the-combobox-control-cs/_static/image5.jpg)](how-do-i-use-the-combobox-control-cs/_static/image9.png)

**Figura 05**: elegir un origen de datos ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image10.png))


[![Elegir los campos de texto y el valor de datos](how-do-i-use-the-combobox-control-cs/_static/image6.jpg)](how-do-i-use-the-combobox-control-cs/_static/image11.png)

**Figura 06**: selección de los campos de texto y el valor de datos ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image12.png))


Después de completar los pasos anteriores, el cuadro combinado está enlazado a un control SqlDataSource que representa las películas de la tabla de base de datos de películas. El origen de la página es similar de la lista 2 (limpian el formato un poco).

**Listado 2 - Movies.aspx**

[!code-aspx[Main](how-do-i-use-the-combobox-control-cs/samples/sample2.aspx)]

Observe que el control de cuadro combinado tiene una propiedad DataSourceID que señala el control SqlDataSource. Al abrir la página en un explorador, se muestra la lista de películas de la base de datos (consulte la figura 7). Puede que una selección una película en la lista o escriba una película nueva escribiendo la película en el cuadro combinado.


[![Mostrar una lista de películas](how-do-i-use-the-combobox-control-cs/_static/image7.jpg)](how-do-i-use-the-combobox-control-cs/_static/image13.png)

**Figura 07**: mostrar una lista de películas ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image14.png))


## <a name="setting-the-dropdownstyle"></a>Establecer el DropDownStyle

Puede usar la propiedad ComboBox DropDownStyle para cambiar el comportamiento del control ComboBox. Esta propiedad no existe acepta los valores posibles:

- Lista desplegable: muestra el ComboBox de (valor predeterminado) una lista desplegable lista al hacer clic en la flecha y puede especificar un valor personalizado.
- Simple: el cuadro combinado muestra una lista desplegable automáticamente y puede especificar un valor personalizado.
- DropDownList - ComboBox funciona igual que un control DropDownList.

La diferencia entre la lista desplegable y Simple es cuando se muestra la lista de elementos. En el caso sencillo, la lista se muestra inmediatamente al mover el foco al control ComboBox. En el caso de la lista desplegable, debe hacer clic en la flecha para ver la lista de elementos.

El valor de DropDownList hace que el control de cuadro combinado para que funcione como un control DropDownList estándar. Sin embargo, hay una diferencia importante aquí. Las versiones anteriores de Internet Explorer muestran un control DropDownList con un índice z infinito para que el control aparecerá delante de cualquier control que se coloca delante de él. Dado que el cuadro combinado representa un elemento HTML &lt;div&gt; etiqueta en lugar de un elemento HTML &lt;seleccione&gt; etiqueta, el cuadro combinado correctamente respeta orden z.

## <a name="setting-the-autocompletemode"></a>Establecer el AutoCompleteMode

Utilice la propiedad ComboBox AutoCompleteMode para especificar lo que sucede cuando alguien escribe texto en el cuadro combinado. Esta propiedad acepta los siguientes valores posibles:

- None: (valor predeterminado) el cuadro combinado no proporciona ningún comportamiento de Autocompletar.
- Sugerir - en el cuadro combinado muestra la lista y resalta el elemento correspondiente en la lista (consulte la figura 8).
- Anexar: el control ComboBox no muestra la lista y anexa el elemento correspondiente de la lista a lo que ha escrito (consulte la figura 9).
- SuggestAppend - ComboBox tanto muestra la lista y anexa el elemento correspondiente de la lista a lo que ha escrito (consulte la figura 10).


[![El cuadro combinado hace que una sugerencia](how-do-i-use-the-combobox-control-cs/_static/image8.jpg)](how-do-i-use-the-combobox-control-cs/_static/image15.png)

**Figura 08**: hace que el cuadro combinado de una sugerencia ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image16.png))


[![Cuadro combinado anexa texto coincidente](how-do-i-use-the-combobox-control-cs/_static/image9.jpg)](how-do-i-use-the-combobox-control-cs/_static/image17.png)

**Figura 09**: cuadro combinado anexa texto coincidente ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image18.png))


[![El cuadro combinado se sugiere y anexa](how-do-i-use-the-combobox-control-cs/_static/image10.jpg)](how-do-i-use-the-combobox-control-cs/_static/image19.png)

**Figura 10**: el ComboBox sugiere y anexa ([haga clic aquí para ver imagen en tamaño completo](how-do-i-use-the-combobox-control-cs/_static/image20.png))


## <a name="summary"></a>Resumen

En este tutorial, ha aprendido a usar el control ComboBox para mostrar un conjunto fijo de elementos. Se enlaza el control de cuadro combinado tanto a un conjunto de elementos de estático a una tabla de base de datos. Por último, ha aprendido cómo modificar el comportamiento del control ComboBox estableciendo sus propiedades DropDownStyle y AutoCompleteMode.

> [!div class="step-by-step"]
> [Siguiente](how-do-i-use-the-combobox-control-vb.md)
