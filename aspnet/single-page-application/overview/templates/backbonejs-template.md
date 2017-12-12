---
uid: single-page-application/overview/templates/backbonejs-template
title: Plantilla de la red troncal | Documentos de Microsoft
author: madskristensen
description: Plantilla SPA backbone.js
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 3b8eabd3cefcb96dc40bbf6cc6e3ee81accb0d7c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="backbone-template"></a>Plantilla de la red troncal
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de SPA Backbone escribió Kazi Manzur Rashid
> 
> [Descargue la plantilla SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


La plantilla de SPA Backbone.js está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web interactivas del lado cliente con [Backbone.js.](http://backbonejs.org/)

La plantilla proporciona un esqueleto inicial para desarrollar una aplicación de Backbone.js en ASP.NET MVC. De fábrica proporciona funcionalidad de inicio de sesión de usuario básico, incluido el restablecimiento de contraseña de inicio de sesión, inicio de sesión de usuario y la confirmación de usuario con plantillas de correo electrónico básico.

Requisitos:

- [Actualización ASP.NET y 2012.2 de herramientas Web](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Crear un proyecto de plantilla de la red troncal

Descargue e instale la plantilla, haga clic en el botón de descarga más arriba. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Debe reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

![](backbonejs-template/_static/image1.png)

En el **nuevo proyecto** asistente, seleccione proyecto de SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](backbonejs-template/_static/image3.png)

Haga clic en "Mi cuenta", se abrirá la página de inicio de sesión:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Tutorial: Código de cliente

Vamos a empieza por el lado del cliente. Las secuencias de comandos de la aplicación cliente se encuentran en la carpeta ~/Scripts/application. La aplicación está escrita en [TypeScript](http://www.typescriptlang.org/) (archivos .ts) que se compilan en JavaScript (archivos .js).

**Aplicación**

`Application`se define en application.ts. Este objeto inicializa la aplicación y actúa como el espacio de nombres raíz. Mantiene información de configuración y el estado que se comparte entre la aplicación, como si el usuario ha iniciado sesión.

El `application.start` método crea las vistas modales y adjunta controladores de eventos para eventos de nivel de aplicación, como inicio de sesión de usuario. A continuación, crea el enrutador predeterminado y comprueba si se especifica cualquier dirección URL de cliente. Si no es así, redirige a la dirección url predeterminada (#! /).

**Eventos**

Eventos siempre son importantes al desarrollar débilmente acoplados componentes. Las aplicaciones suelen realizan varias operaciones en respuesta a una acción del usuario. Red troncal proporciona eventos integrados con componentes como modelo, la recopilación y la vista. En lugar de crear la interdependencia entre estos componentes, la plantilla utiliza un modelo de "pub/sub": la `events` objeto, definido en events.ts, actúa como un concentrador de eventos para publicar y suscribirse a eventos de aplicación. La `events` objeto es un singleton. El código siguiente muestra cómo suscribirse a un evento y, a continuación, desencadenar el evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Enrutador**

En Backbone.js, un enrutador proporciona métodos para el enrutamiento de páginas de cliente y conéctelos a los eventos y acciones. La plantilla define un único enrutador, en router.ts. El enrutador crea las vistas activable y mantiene el estado al cambiar de vista. (Vistas activable se describen en la sección siguiente). Inicialmente, el proyecto tiene dos vistas ficticias, principal y sobre cómo. También tiene una vista NotFound, que se muestra si no se conoce la ruta.

**Vistas**

Las vistas se definen en ~/Scripts/application o vistas. Hay dos tipos de vistas, vistas activable y cuadro de diálogo modal. Vistas activable invocadas por el enrutador. Cuando se muestre una vista activable, todas las demás vistas activable se convierta en inactivos. Para crear una vista activable, ampliar la vista con la `Activable` objeto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Ampliar con `Activable` agrega dos nuevos métodos a la vista, `activate` y `deactivate`. El enrutador que llama a estos métodos para activar y desactive la vista.

Las vistas modales se implementan como [Bootstrap Twitter](http://twitter.github.com/bootstrap/) cuadros de diálogo modales. El `Membership` y `Profile` vistas son modal. Vistas del modelo pueden invocarse mediante los eventos de aplicación. Por ejemplo, en la `Navigation` muestra la vista, haga clic en el vínculo "Mi cuenta" en la `Membership` vista o la `Profile` vista, dependiendo de si el usuario ha iniciado sesión. El `Navigation` adjuntará, haga clic en controladores de eventos para los elementos secundarios que tienen el `data-command` atributo. Este es el marcado HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Este es el código en navigation.ts para enlazar los eventos:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelos**

Los modelos se definen en ~/Scripts/application/modelos. Todos los modelos tengan tres tareas básicas: atributos predeterminados, reglas de validación y un extremo de servidor. Este es un ejemplo típico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Uso de complementos**

La carpeta ~/Scripts/application/lib contiene algunos complementos jQuery práctica. El archivo form.ts define un complemento para trabajar con datos del formulario. A menudo se necesita serializar o deserializar los datos de formulario y mostrar los errores de validación del modelo. El complemento form.ts tiene métodos como `serializeFields`, `deserializeFields`, y `showFieldErrors`. En el ejemplo siguiente se muestra cómo serializar un formulario a un modelo.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

El complemento flashbar.ts ofrece diversos tipos de mensajes de comentarios al usuario. Los métodos son `$.showSuccessbar`, `$.showErrorbar` y `$.showInfobar`. En segundo plano, utiliza alertas de arranque de Twitter para mostrar mensajes bien animados.

El complemento confirm.ts reemplaza el explorador confirme el cuadro de diálogo, aunque la API es ligeramente distinta:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Tutorial: Código de servidor

Ahora veamos el lado del servidor.

**Controladores**

En una aplicación Web, el servidor desempeña solo una pequeña función en la interfaz de usuario. Normalmente, el servidor presenta la página inicial y, a continuación, envía y recibe datos JSON.

La plantilla tiene dos controladores MVC: `HomeController` presenta la página inicial, y `SupportsController` se utiliza para confirmar nuevas cuentas de usuario y restablecer contraseñas. Todos los demás controladores en la plantilla son controladores de ASP.NET Web API, que envían y reciben datos JSON. De forma predeterminada, los controladores de usan el nuevo `WebSecurity` clase para realizar tareas relacionadas con el usuario. Sin embargo, también tienen constructores opcionales que le permiten pasar en delegados para estas tareas. Esto facilita las pruebas y le permite reemplazar `WebSecurity` por algo más, mediante el uso de un contenedor de IoC. A continuación se muestra un ejemplo:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Vistas

Las vistas están diseñadas para ser modular: cada sección de una página tiene su propia vista dedicado. En una aplicación Web, es común para incluir vistas que no tiene ningún controlador correspondiente. Puede incluir una vista mediante una llamada a `@Html.Partial('myView')`, pero este modo se obtiene una tarea tediosa. Para facilitar esta tarea, la plantilla define un método auxiliar, `IncludeClientViews`, que representa todas las vistas en una carpeta especificada:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Si no se especifica el nombre de la carpeta, el nombre de carpeta predeterminado es "ClientViews". Si la vista de cliente también usa las vistas parciales, nombre de la vista parcial con un carácter de subrayado (por ejemplo, `_SignUp`). El `IncludeClientViews` método excluye cualquier vista cuyo nombre empieza con un carácter de subrayado. Para incluir una vista parcial en la vista de cliente, llame a `Html.ClientView('SignUp')` en lugar de `Html.Partial('_SignUp')`.

**Enviar correo electrónico**

Para enviar correo electrónico, se utiliza la plantilla [Postal](http://aboutcode.net/postal). Sin embargo, se extrae Postal del resto del código con el `IMailer` de la interfaz, por lo que se puede reemplazar fácilmente con otra implementación. Las plantillas de correo electrónico se encuentran en la carpeta vistas o correos electrónicos. Dirección de correo electrónico del remitente se especifica en el archivo web.config, en la `sender.email` clave de la **appSettings** sección. Además, cuando `debug="true"` en el archivo web.config, la aplicación no requiere confirmación de correo electrónico del usuario, para acelerar el desarrollo.

## <a name="github"></a>GitHub

También puede encontrar la plantilla Backbone.js SPA en [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
