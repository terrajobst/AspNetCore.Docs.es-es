---
uid: single-page-application/overview/templates/breezeknockout-template
title: "Plantilla de facilísima/Knockout | Documentos de Microsoft"
author: madskristensen
description: "Plantilla de aplicación de página única es sencilla/Knockout"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 07ec099a0381458fe42c1972a2554f76fd34638c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="breezeknockout-template"></a>Plantilla de facilísima/Knockout
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC sea sencillo/Knockout escribió Ward Bell
> 
> [Descargue la plantilla MVC sea sencillo/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Ha escuchado de "aplicación de una página" (contraseña segura SPA) y se ha preguntado qué es. Mientras que puede leer sobre ella, en su lugar podría experimentar por sí mismo. Pero ¿quién tiene tiempo para descargar un ejemplo? Si tiene Visual Studio, tendrá un ejemplo SPA y ejecuta en menos de 60 segundos con ASP.NET MVC 4 plantilla de "Aplicación de una página única es sencilla/Knockout".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>¿Qué es la plantilla de SPA Facilísima/Knockout?

Las plantillas de proyecto mayoría generar una aplicación esqueleto. Colocar elementos en esos esqueleto agregando el código y finalmente entregar una aplicación en funcionamiento. La plantilla es sencilla/Knockout SPA es diferente. Genera una aplicación de ejemplo para su estudio. Muestra un diseño de la aplicación de SPA y muchas de las técnicas para la creación de un SPA.

La plantilla es sencilla/Knockout es una variación en el [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md) incluido en la actualización de 2012.2 de herramientas de Web y ASP.NET. La plantilla es sencilla SPA genera una aplicación con la misma experiencia de usuario, pero tiene una implementación diferente, que se usa es sencilla para la administración de datos.

La plantilla KnockoutJS SPA realiza las solicitudes de servicio con jQuery sin formato AJAX, que es el adecuado para una aplicación sencilla. Pero más sofisticadas aplicaciones tienen requisitos de administración de datos más exigentes. Por ejemplo, la mayoría de las aplicaciones:

- Consultar y vuelva a consultar el servidor durante una sesión de usuario extendida.
- Agregar filtros de consulta, ordenación y la paginación.
- Compartir los mismos datos en varias pantallas.
- Acumular cambios a muchos objetos, a continuación, guardarlos como una sola transacción.
- Validar los cambios en el cliente, por lo que el usuario puede corregir los errores antes de confirmar los cambios en la base de datos.

La biblioteca de BreezeJS controla estos quehaceres automáticamente, lo que le libera para desarrollar la aplicación lógica y experiencia del usuario que son más importantes.

[**Facilísima** ](http://www.breezejs.com/?utm_source=ms-spa) es una biblioteca de código abierto para crear aplicaciones de datos enriquecidos en JavaScript y HTML, los tipos de aplicaciones que tradicionalmente se han entregado como aplicaciones de escritorio independientes.

La plantilla es sencilla/Knockout le ayuda a tomar ese primer paso fundamental para una infraestructura de administración de datos más sólida. Genera una aplicación de lista de tareas de ejemplo que aparentemente sea idéntica a la plantilla de KnockoutJS SPA. En el interior, reemplaza la capa de datos de AJAX con es sencilla, por lo que puede comparar los dos enfoques en paralelo. Por supuesto, apenas toca el potencial de una aplicación es sencilla. Pero podrá ver cómo funciona es sencilla y cómo poco, es necesario para realizar esa transición.

Comencemos.

## <a name="create-a-breezeknockout-template-project"></a>Crear un proyecto de plantilla es sencilla/Knockout

Descargue e instale la plantilla, haga clic en el botón de descarga más arriba. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Debe reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

En el **nuevo proyecto** asistente, seleccione **Facilísima Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Cuando la aplicación se ejecuta en primer lugar, muestra una pantalla de inicio de sesión. Haga clic en el vínculo "Suscribirse" y una nueva página glides en la vista, donde puede escribir un nombre de usuario y una contraseña. (Las páginas de inicio de sesión y el registro se crean mediante ASP.NET MVC). Cuando se envía el formulario de registro, el servidor genera un TodoList con dos elementos para su cuenta. A continuación, presenta al usuario en un sentido amarillo.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Ahora se le asignará la tierra de SPA. Todo lo que ven y experimentan mientras manipular Todos se representan y se administra en el cliente con la Ayuda de Knockout y es sencilla. Explore la aplicación como un usuario... pero con la opción ojos del desarrollador. Use las herramientas de desarrollo en el explorador para capturar el tráfico de red. (En Internet Explorer: presione F12, seleccione la **red** ficha y haga clic en **empezar a capturar**.) Ahora pruebe lo siguiente:

- Agregar un nuevo elemento de lista de tareas.
- Haga clic en la etiqueta y editar el título del elemento de lista de tareas
- Active una casilla para marcar el elemento done. Tenga en cuenta que el cuadro de texto está deshabilitado, por lo que el título ya no es editable.
- Haga clic en la "x" a la derecha de la etiqueta. El elemento desaparece y se elimina de la base de datos.
- Elija otro elemento y borrar su título. Obtendrá un error de validación que se requiere el título. Tras una breve pausa, se restaura el título de la anterior.
- Escriba un título demasiado largo. Obtendrá un error de validación diferente que el título es demasiado largo.
- Haga clic en el botón "Agregar la lista de tareas". Una nueva lista aparece a la izquierda de la lista anterior.
- Practicar con el título de TodoList, desencadenar necesario y validaciones de longitud.
- Haga clic en el cuadro de texto de título para borrar el mensaje de error.
- Haga clic en la "x" en el círculo en la esquina superior derecha para eliminar el TodoList y su todos.

La lógica de validación es cliente realizada por es sencilla. Atributos de validación en las clases del modelo de servidor se propagan al cliente y se ejecutan automáticamente antes de que el cliente se comunica con el servidor.

Revisar el tráfico de red. Tenga en cuenta que no había ninguna llamada al servidor cuando sea sencillo ha detectado un error. Cada cambio válidas generó una solicitud POST a "/ api/tareas/SaveChanges". Empaqueta los cambios es sencilla y los envía juntos como una única solicitud para el controlador de Web API `SaveChanges` método. Que es diferente de la plantilla de KockoutJS SPA, lo que hace PUT, POST y eliminar las solicitudes para cada elemento por separado.

## <a name="peek-inside"></a>Peek dentro de

Esta aplicación tiene un cliente y un servidor. La pila del lado cliente está formada por un poco HTML y una combinación de módulos de JavaScript de la aplicación (en la carpeta "app") además de las bibliotecas de JavaScript de terceros (en la carpeta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Si se ha investigado la plantilla KnockoutJS SPA, este deberá ser muy familiar. Se centran en los cuadros azules. La arquitectura de interfaz de usuario es Model-View-ViewModel (MVVM), en el que los widgets HTML de la vista se separan limpiamente desde el código auxiliar de presentación en el modelo de vista. Un sistema de enlace de datos (Knockout en este caso) coordina la vista y el modelo de vista de modo que cada uno puede hacer su trabajo sin un conocimiento profundo de la otra.

El modelo encapsula los datos de la lista de tareas. Las entidades del modelo se construyen es sencilla con propiedades observables de cobertura, por lo que se puede enlazar directamente a los widgets de la vista. El modelo de vista le pide el contexto de datos para adquirir y guardar las entidades del modelo. El contexto de datos delega la mayor parte del trabajo que sea sencillo.

La pila del lado servidor está formada por algún código de desarrollador y tres bibliotecas de .NET de principio: API de Web, Entity Framework y Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla de KockoutJS SPA. Sin embargo, la implementación es mucho más fácil: se eliminaron el dto y la mayoría de los detalles de Entity Framework se han delegado a Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que examine el código, guiado por la [una amplia discusión](http://www.breezejs.com/spa-template?utm_source=ms-spa) del cliente y las pilas de servidor en el sitio Web es sencilla.

Puede intentar reproducir con consultas de cliente es sencilla; Agregue algunos filtros y los ordena. Puede agregar más propiedades del modelo y entidades más para hacerse una idea mejor para el desarrollo de SPA-to-end. Cuando esté seguro de que el diseño, puede anular sobre las características de la lista de tareas y reemplácelos por los suyos propios.

Pronto estará preparado para el siguiente paso importante: agregar pantallas de cliente y navegar entre ellas. Podrá dejar esta plantilla SPA y volver a una pila SPA más completa, como [activa toallas de John Papa](https://github.com/johnpapa/HotTowel#readme "toallas activa"), que agrega Durandal a la combinación es sencilla y cobertura.
