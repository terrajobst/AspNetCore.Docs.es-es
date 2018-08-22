---
uid: single-page-application/overview/templates/breezeangular-template
title: Plantilla de BREEZE/Angular | Microsoft Docs
author: madskristensen
description: Plantilla de aplicación de página única de BREEZE/Angular
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: a3e8b42cdadf99df6971a278834b1429e129ce72
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837600"
---
<a name="breezeangular-template"></a>Plantilla de BREEZE/Angular
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de MVC de Breeze/Angular escribió Ward Bell
> 
> [Descargue la plantilla MVC Breeze/Angular](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) es una biblioteca de código abierto de Google para compilar aplicaciones de página única (SPA). Ofrece el enlace de datos, inserción de dependencias y la administración de la pantalla. Combinarla con [BreezeJS](http://www.breezejs.com/?utm_source=ms-spa), otra biblioteca de código abierto para el modelado de datos y administración de datos y tienen los ingredientes esencial para una gran aplicación de cliente HTML/JavaScript.

La plantilla de Breeze/Angular SPA es una variación en el [plantilla KnockoutJS SPA](../introduction/knockoutjs-template.md) incluido en ASP.NET y Web Tools 2012.2 Update. Si tiene Visual Studio, tendrá un SPA de ejemplo en marcha en menos de 60 segundos.

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

Hacia afuera, la aplicación parece muy similar a la plantilla KnockoutJS SPA. Pero es bastante diferente bajo el capó. La plantilla KnockoutJS utiliza Knockout para el enlace de datos y de AJAX sin procesar para el acceso a datos. La plantilla de Breeze/Angular usa Angular para el enlace de datos y Breeze para el acceso a datos. Estas bibliotecas habilitan funcionalidades adicionales, incluido el historial y la navegación de página.

Aquí está la página acerca de la aplicación:

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

Esta página muestra un registro de eventos durante la sesión del usuario actual, incluidos:

- Paginación. Tenga en cuenta la creación del controlador Todo en #2 y #7.
- Las consultas remotas (3) y consultas de la caché local (#7).
- Guardar nueva (5, 6 de #) y modificar las entidades (4).
- Cambios que se valida en el cliente (9), por lo que el usuario puede corregir los errores antes de confirmar los cambios a la base de datos.

Hay más para explorar en esta plantilla, incluidos:

- Carga dinámica de plantillas de vista HTML.
- Enlace de datos personalizados a través de Angular "directivas".
- Inyección de dependencia y la modularidad.
- Filtros de consulta, ordenaciones, paginación, proyecciones y la inclusión de las entidades relacionadas.
- Compartir datos entre varias pantallas.
- Guardando cambios varias como una sola transacción.
- Las reglas de validación se propagan automáticamente desde el servidor al cliente de JavaScript.

Comencemos.

## <a name="create-a-breezeangular-template-project"></a>Crear un proyecto de plantilla de Breeze/Angular

Descargue e instale la plantilla, haga clic en el botón de descarga anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que deba reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

En el **nuevo proyecto** asistente, seleccione **SPA de Angular Breeze**.

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

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
- Haga clic en el vínculo "About" en la esquina superior derecha para ver un registro de estas actividades.

La lógica de validación es del cliente realizado por Breeze. Atributos de validación en las clases del modelo de servidor se propaga al cliente y ejecuta automáticamente antes de que el cliente contacta con el servidor.

Revise el tráfico de red. Tenga en cuenta que no había ninguna llamada al servidor cuando Breeze ha detectado un error. Cada cambio válido dieron lugar a una solicitud POST a "/ api/Todo/SaveChanges". BREEZE agrupa los cambios y los envía juntos como una única solicitud para el controlador de Web API `SaveChanges` método. Que es diferente de la plantilla de SPA KockoutJS, lo que hace que PUT, POST y DELETE individualmente las solicitudes para cada elemento.

Además, observe que no hay ningún tráfico de red cuando se cambia entre el TodoList y acerca de las páginas. Eso es porque se ha restringido la consulta a la caché local de Breeze.

## <a name="peek-inside"></a>Peek dentro de

Esta aplicación tiene un cliente y un servidor. La pila del lado cliente consta de un poco HTML y una combinación de módulos de JavaScript de la aplicación (en la carpeta de "aplicación") además de las bibliotecas de JavaScript de terceros (en la carpeta "Scripts").

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

La arquitectura de interfaz de usuario separa los widgets HTML de las vistas desde el código auxiliar de presentación en los controladores. El sistema de enlace de datos Angular coordina las vistas y controladores para que cada uno de ellos pueda realizar su trabajo sin un conocimiento profundo de la otra.

El controlador solicita el contexto de datos para adquirir y guardar las entidades del modelo. El contexto de datos delega la mayor parte del trabajo para Breeze, que construye objetos del modelo de seguimiento propio de los resultados de consulta de JSON.

La pila del lado servidor consta de código para desarrolladores y tres bibliotecas de .NET de principio: Breeze.NET, Entity Framework y Web API:

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

La arquitectura básica es la misma que la plantilla de SPA KockoutJS. Sin embargo, la implementación es mucho más sencilla: se eliminaron el dto y la mayoría de los detalles de Entity Framework le ha sido delegada Breeze.NET.

## <a name="next-steps"></a>Pasos siguientes

Le recomendamos que explore el código, guiado por el [amplia discusión](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa) del cliente y las pilas de servidor en el sitio Web de Breeze.

Podría intentar reproducir con una consulta del lado cliente Breeze; Agregue algunos filtros y órdenes. Puede agregar más propiedades del modelo y entidades más para hacerse una idea más clara para el desarrollo de SPA-to-end. Cuando esté seguro de que el diseño, puede retirar las características de la lista de tareas y reemplazarlos por los suyos propios.

¡Que disfrute programando!
