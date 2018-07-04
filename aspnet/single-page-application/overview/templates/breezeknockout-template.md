---
uid: single-page-application/overview/templates/breezeknockout-template
title: Plantilla de BREEZE/Knockout | Microsoft Docs
author: madskristensen
description: Plantilla de aplicación de página única de BREEZE/Knockout
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 48ee0463fe950c28832523986a2242417411c96a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376869"
---
<a name="breezeknockout-template"></a>Plantilla de BREEZE/Knockout
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC de Breeze/Knockout escribió Ward Bell
> 
> [Descargue la plantilla MVC de Breeze/Knockout](https://go.microsoft.com/fwlink/?LinkId=282649)


Ha oído hablar de "aplicación de página única" (SPA) y se ha preguntado qué es. Aunque podría leer acerca de él, podría experimentar en su lugar por sí mismo. ¿Pero quién tiene tiempo para descargar un ejemplo? Si tiene Visual Studio, tendrá un SPA de ejemplo y ejecuta en menos de 60 segundos con ASP.NET MVC 4 plantilla "Aplicación de página única de Breeze/Knockout".

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>¿Qué es la plantilla de SPA de Breeze/Knockout?

La mayoría de las plantillas de proyecto generan una esqueleto de la aplicación. Colocar elementos en los huesos agregando el código y, finalmente, ofrecer una aplicación de trabajo. La plantilla de Breeze/Knockout SPA es diferente. Genera una aplicación de ejemplo para el estudio. Muestra un diseño de la aplicación SPA y muchas de las técnicas para crear una SPA.

La plantilla de Breeze/Knockout es una variación en el [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md) incluido en ASP.NET y Web Tools 2012.2 Update. La plantilla de Breeze SPA genera una aplicación con la misma experiencia de usuario, pero tiene una implementación diferente, uso Breeze para la administración de datos.

La plantilla KnockoutJS SPA realiza solicitudes de servicio con sin formato jQuery AJAX, que es adecuado para una aplicación sencilla. Pero más sofisticadas aplicaciones tienen requisitos más exigentes de administración de datos. Por ejemplo, la mayoría de las aplicaciones:

- Consultar y volver a consultar el servidor durante una sesión de usuario extendida.
- Agregar filtros de consulta, ordenación y paginación.
- Compartir los mismos datos en varias pantallas.
- Acumular cambios a muchos objetos, a continuación, guardarlos como una sola transacción.
- Validar los cambios en el cliente, por lo que el usuario puede corregir los errores antes de confirmar los cambios en la base de datos.

La biblioteca de BreezeJS controla estas tareas por usted, lo que le libera para desarrollar la aplicación lógica y experiencia del usuario que más le importa.

[**BREEZE** ](http://www.breezejs.com/?utm_source=ms-spa) es una biblioteca de código abierto para compilar aplicaciones de datos enriquecidos en JavaScript y HTML, los tipos de aplicaciones que históricamente se han entregado como aplicaciones de escritorio independientes.

La plantilla de Breeze/Knockout le ayuda a tomar ese primer paso fundamental para una infraestructura de administración de datos más sólida. Genera una aplicación Todo de ejemplo que aparentemente sea idéntica a la plantilla KnockoutJS SPA. En el interior, reemplaza la capa de datos de AJAX con Breeze, para poder comparar los dos enfoques en paralelo. Por supuesto, apenas toca el potencial de una aplicación de Breeze. Pero verá cómo funciona Breeze y lo poco, es necesario para realizar esa transición.

Comencemos.

## <a name="create-a-breezeknockout-template-project"></a>Crear un proyecto de plantilla de Breeze/Knockout

Descargue e instale la plantilla, haga clic en el botón de descarga anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que deba reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

En el **nuevo proyecto** asistente, seleccione **Breeze Knockout SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

Cuando la aplicación se ejecuta en primer lugar, muestra una pantalla de inicio de sesión. Haga clic en el vínculo "Registrarse" y una nueva página tendones en la vista, donde puede escribir un nombre de usuario y una contraseña. (Las páginas de inicio de sesión y registro se compilan mediante ASP.NET MVC). Cuando se envía el formulario de registro, el servidor genera un TodoList con dos elementos para su cuenta. A continuación, presenta al usuario en una nota en amarilla.

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

Ahora está en la tierra de SPA. Todo lo que puede ver y experimenta mientras la manipulación de tareas pendientes se representan y se administra en el cliente con la Ayuda de Knockout y Breeze. Exploración de la aplicación como un usuario... pero con los ojos del desarrollador. Use las herramientas de desarrollo en el explorador para capturar el tráfico de red. (En Internet Explorer: presione F12, seleccione el **red** ficha y haga clic en **empezar a capturar**.) Ahora pruebe lo siguiente:

- Agregue un nuevo elemento Todo.
- Haga clic en la etiqueta y editar el título del elemento de lista de tareas
- Active una casilla para marcar el elemento done. Tenga en cuenta que el cuadro de texto está deshabilitado, por lo que el título ya no es editable.
- Haga clic en la 'x' a la derecha de la etiqueta. El elemento desaparece y se elimina de la base de datos.
- Seleccione otro elemento y desactive su título. Obtendrá un error de validación que se requiere el título. Tras una breve pausa, se restaura el título anterior.
- Escriba un título muy largo. Obtendrá un error de validación diferente que el título es demasiado largo.
- Haga clic en el botón "Agregar la lista de tareas pendientes". Una nueva lista aparece a la izquierda de la lista anterior.
- Jugar con el título de TodoList, desencadenar su necesarios y las validaciones de longitud.
- Haga clic en el cuadro de texto de título para borrar el mensaje de error.
- Haga clic en la "x" en el círculo en la esquina superior derecha para eliminar el TodoList y sus tareas pendientes.

La lógica de validación es del cliente realizado por Breeze. Atributos de validación en las clases del modelo de servidor se propaga al cliente y ejecuta automáticamente antes de que el cliente contacta con el servidor.

Revise el tráfico de red. Tenga en cuenta que no había ninguna llamada al servidor cuando Breeze ha detectado un error. Cada cambio válido dieron lugar a una solicitud POST a "/ api/Todo/SaveChanges". BREEZE agrupa los cambios y los envía juntos como una única solicitud para el controlador de Web API `SaveChanges` método. Que es diferente de la plantilla de SPA KockoutJS, lo que hace que PUT, POST y DELETE individualmente las solicitudes para cada elemento.

## <a name="peek-inside"></a>Peek dentro de

Esta aplicación tiene un cliente y un servidor. La pila del lado cliente consta de un poco HTML y una combinación de módulos de JavaScript de la aplicación (en la carpeta de "aplicación") además de las bibliotecas de JavaScript de terceros (en la carpeta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

Si se ha investigado la plantilla KnockoutJS SPA, esto resultará muy familiar. Se centran en los cuadros azules. La arquitectura de interfaz de usuario es Model-View-ViewModel (MVVM), en el que los widgets HTML de la vista se separan correctamente desde el código auxiliar de presentación en el modelo de vista. Un sistema de enlace de datos (Knockout en este caso) coordina la vista y el modelo de vista para que cada uno de ellos pueda realizar su trabajo sin un conocimiento profundo de la otra.

El modelo encapsula los datos de la lista de tareas. Las entidades del modelo se construyen mediante Breeze con propiedades observables de Knockout, por lo que se puede enlazar directamente a widgets en la vista. El modelo de vista pide el contexto de datos para adquirir y guardar las entidades del modelo. El contexto de datos delega la mayor parte del trabajo para Breeze.

La pila del lado servidor consta de código para desarrolladores y tres bibliotecas de .NET de principio: Breeze.NET, Entity Framework y Web API:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla de SPA KockoutJS. Sin embargo, la implementación es mucho más sencilla: se eliminaron el dto y la mayoría de los detalles de Entity Framework le ha sido delegada Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Le recomendamos que explore el código, guiado por el [amplia discusión](http://www.breezejs.com/spa-template?utm_source=ms-spa) del cliente y las pilas de servidor en el sitio Web de Breeze.

Podría intentar reproducir con una consulta del lado cliente Breeze; Agregue algunos filtros y órdenes. Puede agregar más propiedades del modelo y entidades más para hacerse una idea más clara para el desarrollo de SPA-to-end. Cuando esté seguro de que el diseño, puede retirar las características de la lista de tareas y reemplazarlos por los suyos propios.

Pronto estará listo para el siguiente paso importante: agregar pantallas de cliente y navegar entre ellos. Podrá dejar esta plantilla SPA y dirigirse a una pila SPA más completa, como [Hot toalla John Papa](https://github.com/johnpapa/HotTowel#readme "toalla Hot"), que agrega a la combinación de Breeze y Knockout Durandal.
