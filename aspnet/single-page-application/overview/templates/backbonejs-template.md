---
uid: single-page-application/overview/templates/backbonejs-template
title: Plantilla de Backbone | Microsoft Docs
author: madskristensen
description: Plantilla SPA backbone.js
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/04/2013
ms.topic: article
ms.assetid: 00aca413-f067-4108-9bd1-cf21e64a2646
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/backbonejs-template
msc.type: authoredcontent
ms.openlocfilehash: 641e149155fbee2655024bec3b76dce5243e7d59
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362510"
---
<a name="backbone-template"></a>Plantilla de Backbone
====================
por [Mads Kristensen](https://github.com/madskristensen)

> La plantilla de SPA troncal escribió Kazi Manzur Rashid
> 
> [Descargue la plantilla SPA Backbone.js](https://go.microsoft.com/fwlink/?LinkId=293631)


La plantilla de SPA Backbone.js está diseñada para ayudarle a comenzar a crear rápidamente aplicaciones web interactivas del lado cliente con [Backbone.js.](http://backbonejs.org/)

La plantilla proporciona un esqueleto inicial para desarrollar una aplicación Backbone.js en ASP.NET MVC. De fábrica proporciona funcionalidad de inicio de sesión de usuario básico, incluido el restablecimiento de contraseña de registro, inicio de sesión de usuario y la confirmación del usuario con plantillas de correo electrónico básico.

Requisitos:

- [Actualización ASP.NET y Web Tools 2012.2](https://go.microsoft.com/fwlink/?LinkId=282650)

## <a name="create-a-backbone-template-project"></a>Crear un proyecto de plantilla de Backbone

Descargue e instale la plantilla, haga clic en el botón de descarga anterior. La plantilla se empaqueta como un archivo de extensión de Visual Studio (VSIX). Es posible que deba reiniciar Visual Studio.

En el **plantillas** panel, seleccione **plantillas instaladas** y expanda el **Visual C#** nodo. En **Visual C#**, seleccione **Web**. En la lista de plantillas de proyecto, seleccione **aplicación Web de ASP.NET MVC 4**. Denomine el proyecto y haga clic en **Aceptar**.

![](backbonejs-template/_static/image1.png)

En el **nuevo proyecto** asistente, seleccione proyecto de SPA Backbone.js.

![](backbonejs-template/_static/image2.png)

Presione Ctrl-F5 para compilar y ejecutar la aplicación sin depurar o presione F5 para ejecutar con la depuración.

![](backbonejs-template/_static/image3.png)

Al hacer clic en "Mi cuenta", se abre la página de inicio de sesión:

![](backbonejs-template/_static/image4.png)

## <a name="walkthrough-client-code"></a>Tutorial: Código de cliente

Vamos a comienza con el lado del cliente. Las secuencias de comandos de la aplicación cliente se encuentran en la carpeta ~/Scripts/application. La aplicación está escrita [TypeScript](http://www.typescriptlang.org/) (archivos .ts) que se compilan en JavaScript (archivos .js).

**Aplicación**

`Application` se define en application.ts. Este objeto inicializa la aplicación y actúa como el espacio de nombres raíz. Mantiene información de configuración y el estado que se comparte entre la aplicación, como si el usuario ha iniciado sesión.

El `application.start` método crea las vistas modales y adjunta los controladores de eventos para eventos de nivel de aplicación, como inicio de sesión de usuario. A continuación, se crea al enrutador predeterminado y comprueba si se especifica cualquier dirección URL del lado cliente. Si no, redirige a la dirección url predeterminada (#! /).

**Eventos**

Los eventos son siempre importantes al desarrollar débilmente acoplados componentes. Las aplicaciones suelen realizan varias operaciones en respuesta a una acción del usuario. Red troncal proporciona eventos integrados con componentes como modelo, la colección y la vista. En lugar de crear interdependencias entre estos componentes, la plantilla utiliza un modelo de "pub/sub": el `events` objeto, definido en events.ts, actúa como un centro de eventos para publicar y suscribirse a eventos de aplicación. La `events` objeto es un singleton. El código siguiente muestra cómo suscribirse a un evento y, a continuación, desencadenar el evento:

[!code-csharp[Main](backbonejs-template/samples/sample1.cs)]

**Enrutador**

En Backbone.js, un enrutador proporciona métodos de enrutamiento de las páginas del lado cliente y conectarlos a las acciones y eventos. La plantilla define un único enrutador router.ts. El enrutador crea las vistas activable y mantiene el estado al cambiar de vista. (Vistas activable se describen en la sección siguiente). Inicialmente, el proyecto tiene dos vistas ficticias, principal y punto. También tiene una vista NotFound, que se muestra si no se conoce la ruta.

**Vistas**

Las vistas se definen en ~/Scripts/application o vistas. Hay dos tipos de vistas, vistas activable y cuadro de diálogo modal. Vistas activable se invocan el enrutador. Cuando se muestra una vista activable, todas las demás vistas activable se vuelven inactivas. Para crear una vista activable, amplíe la vista con el `Activable` objeto:

[!code-javascript[Main](backbonejs-template/samples/sample2.js)]

Ampliar con `Activable` agrega dos nuevos métodos a la vista, `activate` y `deactivate`. El enrutador llama a estos métodos para activar y desactivar la queda la vista de instancia.

Las vistas modales se implementan como [Twitter Bootstrap](http://twitter.github.com/bootstrap/) cuadros de diálogo modales. El `Membership` y `Profile` las vistas son modales. Vistas de modelo pueden invocarse mediante los eventos de aplicación. Por ejemplo, en el `Navigation` muestra la vista, haga clic en el vínculo "Mi cuenta" cualquiera el `Membership` vista o la `Profile` vista, dependiendo de si el usuario ha iniciado sesión. El `Navigation` adjunta, haga clic en los controladores de eventos a los elementos secundarios que tienen el `data-command` atributo. Aquí está el marcado HTML:

[!code-html[Main](backbonejs-template/samples/sample3.html)]

Este es el código en navigation.ts para enlazar los eventos:

[!code-csharp[Main](backbonejs-template/samples/sample4.cs)]

**Modelos**

Los modelos se definen en ~/Scripts/application/modelos. Todos los modelos tengan tres aspectos básicos: atributos predeterminados, reglas de validación y un punto de conexión de servidor. Este es un ejemplo típico:

[!code-javascript[Main](backbonejs-template/samples/sample5.js)]

**Los complementos**

La carpeta ~/Scripts/application/lib contiene algunos complementos de jQuery útil. El archivo form.ts define un complemento para trabajar con datos del formulario. A menudo necesitará serializar o deserializar los datos de formulario y mostrar los errores de validación del modelo. El complemento form.ts incorpora métodos como `serializeFields`, `deserializeFields`, y `showFieldErrors`. El ejemplo siguiente muestra cómo serializar un formulario a un modelo.

[!code-javascript[Main](backbonejs-template/samples/sample6.js)]

El complemento flashbar.ts ofrece diversos tipos de mensajes de comentarios al usuario. Los métodos son `$.showSuccessbar`, `$.showErrorbar` y `$.showInfobar`. En segundo plano usa las alertas de Twitter Bootstrap para mostrar mensajes bien animados.

El complemento confirm.ts reemplaza el explorador confirme el cuadro de diálogo, aunque la API es un poco diferente:

[!code-javascript[Main](backbonejs-template/samples/sample7.js)]

## <a name="walkthrough-server-code"></a>Tutorial: Código de servidor

Ahora veamos el lado del servidor.

**Controladores**

En una aplicación de página única, el servidor reproduce solo una pequeña función en la interfaz de usuario. Normalmente, el servidor procesa la página inicial y, a continuación, envía y recibe datos JSON.

La plantilla tiene dos controladores MVC: `HomeController` representa la página inicial, y `SupportsController` se utiliza para confirmar las nuevas cuentas de usuario y restablecer contraseñas. Todos los demás controladores en la plantilla son controladores de ASP.NET Web API, que envían y reciben datos JSON. De forma predeterminada, los controladores de usan el nuevo `WebSecurity` clase para realizar tareas relacionadas con el usuario. Sin embargo, también tienen constructores opcionales que le permiten pasar los delegados para estas tareas. Esto facilita las pruebas y le permite reemplazar `WebSecurity` con algo más, mediante el uso de un contenedor de IoC. A continuación se muestra un ejemplo:

[!code-csharp[Main](backbonejs-template/samples/sample8.cs)]

## <a name="views"></a>Vistas

Las vistas están diseñadas para ser modular: cada sección de una página tiene su propia vista dedicado. En una aplicación de página única, es habitual incluir vistas que no tienen ningún controlador correspondiente. Puede incluir una vista mediante una llamada a `@Html.Partial('myView')`, pero esto se torna tedioso. Para facilitar esta tarea, la plantilla define un método auxiliar, `IncludeClientViews`, que representa todas las vistas en una carpeta especificada:

[!code-cshtml[Main](backbonejs-template/samples/sample9.cshtml)]

Si no se especifica el nombre de carpeta, el nombre de la carpeta predeterminada es "ClientViews". Si la vista de cliente también usa las vistas parciales, mencione la vista parcial con un carácter de subrayado (por ejemplo, `_SignUp`). El `IncludeClientViews` método excluye todas las vistas cuyo nombre empieza con un carácter de subrayado. Para incluir una vista parcial en la vista de cliente, llame a `Html.ClientView('SignUp')` en lugar de `Html.Partial('_SignUp')`.

**Envío de correo electrónico**

Para enviar correo electrónico, utiliza la plantilla [Postal](http://aboutcode.net/postal). Sin embargo, se abstrae Postal del resto del código con el `IMailer` interfaz, por lo que puede reemplazar fácilmente con otra implementación. Las plantillas de correo electrónico se encuentran en la carpeta vistas/mensajes de correo electrónico. Dirección de correo electrónico del remitente se especifica en el archivo web.config, en el `sender.email` clave de la **appSettings** sección. También, cuando `debug="true"` en web.config, la aplicación no requiere confirmación por correo electrónico del usuario, para acelerar el desarrollo.

## <a name="github"></a>GitHub

También puede encontrar la plantilla de SPA Backbone.js en [GitHub](https://github.com/kazimanzurrashid/AspNetMvcBackboneJsSpa).
