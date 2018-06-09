---
uid: single-page-application/overview/templates/breezeangular-template
title: Plantilla de facilísima/Angular | Documentos de Microsoft
author: madskristensen
description: Plantilla de aplicación de página única es sencilla/Angular
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506694"
---
<a name="breezeangular-template"></a>Plantilla de facilísima/Angular
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC sea sencillo/Angular escribió Ward Bell
> 
> [Descargue la plantilla MVC sea sencillo/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) es una biblioteca de código abierto de Google para la creación de aplicaciones de una sola página (SPAs). Ofrece administración de la pantalla, enlace de datos e inserción de dependencias. Combinar con [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), otra biblioteca de código abierto para el modelado de datos y administración de datos y tienen los ingredientes esenciales para una gran aplicación de cliente HTML/JavaScript.

La plantilla es sencilla/Angular SPA es una variación en el [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md) incluido en la actualización de 2012.2 de herramientas de Web y ASP.NET. Si tiene Visual Studio, tendrá un ejemplo SPA a trabajar en menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Apariencia, la aplicación parece muy similar a la plantilla de KnockoutJS SPA. Pero es muy diferente en segundo plano. La plantilla de KnockoutJS utiliza Knockout para AJAX sin formato para el acceso a los datos y enlace de datos. La plantilla es sencilla/Angular usa Angular para enlace de datos y es sencilla para acceso a datos. Estas bibliotecas habilitan funcionalidades adicionales, incluido el historial y la navegación de página.

Esta es la página de información acerca de la aplicación:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Esta página muestra un registro de eventos durante la sesión de usuario actual, incluidos:

- Paginación. Tenga en cuenta la creación del controlador de tareas en #2 y #7.
- Las consultas remotas (3) y consultas de la caché local (#7).
- Guardar nuevas (5, 6 #) y modificar las entidades (#4).
- Cambios validados en el cliente (#9), por lo que el usuario puede corregir los errores antes de confirmar los cambios a la base de datos.

Hay varias de ellas para explorar en esta plantilla, incluidos:

- Carga dinámica de plantillas de la vista HTML.
- Enlace de datos personalizados a través de Angular "directivas".
- Inyección de dependencia y la modularidad.
- Filtros de consulta, ordenaciones, paginación, las proyecciones y la inclusión de entidades relacionadas.
- Compartir datos entre varias pantallas.
- Guardar varios cambios como una sola transacción.
- Reglas de validación se propagan automáticamente desde el servidor al cliente de JavaScript.

Comencemos.

## <a name="create-a-breezeangular-template-project"></a>Crear un proyecto de plantilla es sencilla/Angular

Descargue e instale la plantilla, haga clic en el botón de descarga más arriba. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Debe reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

En el **nuevo proyecto** asistente, seleccione **Facilísima Angular SPA**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Haga clic en el vínculo "About" en la esquina superior derecha para ver un registro de estas actividades.

La lógica de validación es cliente realizada por es sencilla. Atributos de validación en las clases del modelo de servidor se propagan al cliente y se ejecutan automáticamente antes de que el cliente se comunica con el servidor.

Revisar el tráfico de red. Tenga en cuenta que no había ninguna llamada al servidor cuando sea sencillo ha detectado un error. Cada cambio válidas generó una solicitud POST a "/ api/tareas/SaveChanges". Empaqueta los cambios es sencilla y los envía juntos como una única solicitud para el controlador de Web API `SaveChanges` método. Que es diferente de la plantilla de KockoutJS SPA, lo que hace PUT, POST y eliminar las solicitudes para cada elemento por separado.

Además, observe que no hay ningún tráfico de red cuando se cambia entre el TodoList y acerca de las páginas. Eso es porque la consulta se ha restringido a la caché local de es sencilla.

## <a name="peek-inside"></a>Peek dentro de

Esta aplicación tiene un cliente y un servidor. La pila del lado cliente está formada por un poco HTML y una combinación de módulos de JavaScript de la aplicación (en la carpeta "app") además de las bibliotecas de JavaScript de terceros (en la carpeta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

La arquitectura de interfaz de usuario separa los widgets HTML de las vistas desde el código auxiliar de presentación en los controladores. El sistema de enlace de datos Angular coordina vistas y controladores de modo que cada uno puede hacer su trabajo sin un conocimiento profundo de la otra.

El controlador solicita el contexto de datos para adquirir y guardar las entidades del modelo. El contexto de datos delega la mayor parte del trabajo en modo sencillo, que construye objetos del modelo de seguimiento propio de resultados de la consulta JSON.

La pila del lado servidor está formada por algún código de desarrollador y tres bibliotecas de .NET de principio: API de Web, Entity Framework y Breeze.NET:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla de KockoutJS SPA. Sin embargo, la implementación es mucho más fácil: se eliminaron el dto y la mayoría de los detalles de Entity Framework se han delegado a Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Se recomienda que examine el código, guiado por la [una amplia discusión](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) del cliente y las pilas de servidor en el sitio Web es sencilla.

Puede intentar reproducir con consultas de cliente es sencilla; Agregue algunos filtros y los ordena. Puede agregar más propiedades del modelo y entidades más para hacerse una idea mejor para el desarrollo de SPA-to-end. Cuando esté seguro de que el diseño, puede anular sobre las características de la lista de tareas y reemplácelos por los suyos propios.

¡Que disfrute programando!
