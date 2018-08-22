---
uid: web-forms/overview/moving-to-aspnet-20/master-pages
title: Páginas maestras | Microsoft Docs
author: microsoft
description: Uno de los componentes clave para un sitio Web tenga éxito es un aspecto coherente. En ASP.NET 1.x, los desarrolladores usan los controles de usuario para replicar comunes elem. de página...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 9c0cce4d-efd9-4c14-b0e8-a1a140abb3f4
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/master-pages
msc.type: authoredcontent
ms.openlocfilehash: f40eb338a1b6b8eebb6578dd7938e96a05b1617f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837642"
---
<a name="master-pages"></a>Páginas maestras
====================
por [Microsoft](https://github.com/microsoft)

> Uno de los componentes clave para un sitio Web tenga éxito es un aspecto coherente. En ASP.NET 1.x, los desarrolladores usan controles de usuario para replicar elementos comunes de la página a través de una aplicación Web. Aunque ciertamente es una solución operativa, utilizando los controles de usuario tiene algunas desventajas. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas en un sitio. Los controles de usuario no se representan también en la vista de diseño después de que se va a insertar en una página.


Uno de los componentes clave para un sitio Web tenga éxito es un aspecto coherente. En ASP.NET 1.x, los desarrolladores usan controles de usuario para replicar elementos comunes de la página a través de una aplicación Web. Aunque ciertamente es una solución operativa, utilizando los controles de usuario tiene algunas desventajas. Por ejemplo, un cambio en la posición de un control de usuario requiere un cambio en varias páginas en un sitio. Los controles de usuario no se representan también en la vista de diseño después de que se va a insertar en una página.

ASP.NET 2.0 introduce Master páginas como una manera de mantener una apariencia coherente y, como pronto comprobará, Master páginas representan una mejora considerable sobre el método de control de usuario.

## <a name="why-master-pages"></a>¿Páginas maestras de por qué?

Quizás se pregunte por qué se necesitaron páginas maestras en ASP.NET 2.0. Después de todo, los desarrolladores de sitios Web ya están usando los controles de usuario en ASP.NET 1.x a compartir áreas de contenido entre las páginas. Hay realmente varios motivos por los controles de usuario de una solución menos óptimo para crear un diseño común.

Los controles de usuario realmente no definen el diseño de página. En su lugar, definen el diseño y la funcionalidad de una parte de una página. La distinción entre estos dos es importante porque hace mucho más difícil la facilidad de uso de una solución de control de usuario. Por ejemplo, cuando desea cambiar la posición de un control de usuario en la página, debe editar la página donde aparezca el control de usuario. Thats correctamente si tiene pocas páginas, pero en sitios de gran tamaño, se vuelve pronto una pesadilla de administración del sitio.

Otro inconveniente del uso de controles de usuario para definir un diseño común se basa en la arquitectura de ASP.NET. Si se cambia cualquier miembro público de un control de usuario, requiere volver a compilar todas las páginas que usan el control de usuario. A su vez, ASP.NET, a continuación, acceder las páginas cuando están primer de re JIT. Una vez más, esto, genera una arquitectura que no es escalable y un problema de administración de sitio para sitios de mayor tamaño.

Bien, ambos de estos problemas (y muchos más) se direccionan mediante páginas maestras en ASP.NET 2.0.

## <a name="how-master-pages-work"></a>Cómo funcionan las páginas maestras

Una página maestra es análoga a una plantilla para otras páginas. Elementos de la página que se deben compartir entre otras páginas (es decir, los menús, bordes, etc.) se agregan a la página maestra. Cuando se agregan nuevas páginas en el sitio, se puede asociar con una página maestra. Se llama a una página que está asociada a una página maestra un **página de contenido**. De forma predeterminada, una página de contenido toma la apariencia de la página maestra. Sin embargo, cuando se crea una página maestra, puede definir las partes de la página que la página de contenido puede sustituir por su propio contenido. Estas partes se definen mediante un nuevo control presentado en ASP.NET 2.0; el **ContentPlaceHolder** control.

Una página principal puede contener cualquier número de controles ContentPlaceHolder (o ninguno en absoluto). En la página de contenido, aparece el contenido de los controles ContentPlaceHolder dentro de **contenido** controles, otro control nuevo en ASP.NET 2.0. De forma predeterminada, los controles de contenido de las páginas de contenido están vacías para que pueda proporcionar su propio contenido. Si desea usar el contenido de la página maestra dentro de los controles de contenido, puede hacerlo así como verá más adelante en este módulo. El control de contenido se asigna al control ContentPlaceHolder a través del atributo ContentPlaceHolderID del control de contenido. El código siguiente se asigna un control de contenido a un control ContentPlaceHolder llamado mainBody en una página maestra.

[!code-aspx[Main](master-pages/samples/sample1.aspx)]

> [!NOTE]
> A menudo escuchará las personas se describen las páginas maestras como una clase base para otras páginas. Thats realmente no es true. La relación entre las páginas maestras y las páginas de contenido no es uno de herencia.


**Figura 1** muestra una página maestra y una página de contenido asociada, tal y como aparecen en Visual Studio 2005. Puede ver el control ContentPlaceHolder en la página maestra y el correspondiente control en la página de contenido de contenido. Tenga en cuenta que el contenido de las páginas maestras que se encuentra fuera del control ContentPlaceHolder es visible pero atenuadas en la página de contenido. Solo el contenido existente en el ContentPlaceHolder puede ser suplantado por la página de contenido. Resto del contenido que proviene de la página maestra es inmutable.


![Una página maestra y la página de contenido asociada](master-pages/_static/image1.jpg)

**Figura 1**: una página maestra y la página de contenido asociada


## <a name="creating-a-master-page"></a>Creación de una página maestra

Para crear una nueva página principal:

1. Abra Visual Studio 2005 y cree un nuevo sitio Web.
2. Haga clic en archivo, nuevo, de archivos.
3. Elegir archivo maestro desde el cuadro de diálogo Agregar nuevo elemento, como se muestra en **figura 2**.
4. Haga clic en Agregar.


![Crear una nueva página maestra](master-pages/_static/image2.jpg)

**Figura 2**: crear una nueva página maestra


Tenga en cuenta que la extensión de archivo para una página maestra es <em>.master</em>. Esta es una de las maneras en que una página maestra difiere de una página normal. La principal diferencia es que en lugar de un @Page la directiva, la página maestra contiene un @Master directiva. Cambie a vista de origen para el patrón de página que acaba de crear y revisar el código.

Una nueva página principal tendrá un control ContentPlaceHolder de forma predeterminada. En la mayoría de los casos, tiene más sentido para crear los elementos de página comunes primero y, a continuación, insertar controles ContentPlaceHolder donde desee contenido personalizado. En esos casos, los programadores desearán eliminar el control ContentPlaceHolder predeterminado e insertar otros nuevos mientras se desarrolla la página. Controles ContentPlaceHolder no son redimensionables a pesar del hecho de que muestren los controladores de tamaño. Los tamaños de control ContentPlaceHolder automáticamente según el contenido que lo contiene con una excepción; Si se coloca un control ContentPlaceHolder dentro de un elemento de bloque como una celda de tabla, ajustarse según el tamaño del elemento.

## <a name="lab-1-working-with-master-pages"></a>Laboratorio 1 trabajar con páginas maestras

En este laboratorio, creará una nueva página maestra y definir tres controles ContentPlaceHolder. A continuación, creará una nueva página de contenido y reemplace el contenido de al menos uno de los controles ContentPlaceHolder.

1. Crear una página maestra e insertar controles ContentPlaceHolder. 

    1. Cree una nueva página principal como se describió anteriormente.
    2. Elimine el control ContentPlaceHolder predeterminado.
    3. Seleccione el control ContentPlaceHolder haciendo clic en el borde sombreado superior del control y, a continuación, eliminarlo presionando la tecla SUPR del teclado.
    4. Insertar una nueva tabla con el *encabezado y lado* plantilla tal como se muestra en la figura 3. Cambiar el ancho y alto en el 90% para que esté visible en el Diseñador de toda la tabla.


![](master-pages/_static/image3.jpg)

**Figura 3**


1. Coloque el cursor en cada celda de la tabla y establezca el *valign* propiedad *superior*.
2. Desde el cuadro de herramientas, inserte un control ContentPlaceHolder en la celda superior de la tabla (la celda de encabezado).
3. Cuando se inserta este control ContentPlaceHolder, observará que el alto de fila ocupa casi toda la página como se muestra en la figura 4. Se preocupa en este momento.


![Es el espacio vacío en la misma celda, como el control ContentPlaceHolder](master-pages/_static/image1.gif)

**Figura 4**: es el espacio vacío en la misma celda, como el control ContentPlaceHolder


1. Coloque un control ContentPlaceHolder en las dos celdas. Una vez que se han insertado los demás controles ContentPlaceHolder, el tamaño de las celdas de tabla debe ser como se esperaría. La página debe parecerse a la página que aparece en **figura 5**.


![El maestro con todos los controles ContentPlaceHolder. Tenga en cuenta que el alto de celda para la celda de encabezado es ahora lo que debe ser](master-pages/_static/image2.gif)

**Figura 5**: Master The con todos los controles ContentPlaceHolder. Tenga en cuenta que el alto de celda para la celda de encabezado es ahora lo que debe ser


1. Escriba algún texto que prefiera en cada uno de los tres controles ContentPlaceHolder.
2. Guarde la página maestra como exercise1.master.
3. Crear un nuevo formulario Web y asociarla a la página maestra exercise1.master.
4. Seleccione el archivo, nuevo, el archivo en Visual Studio 2005.
5. Seleccione **formulario Web Forms** en el cuadro de diálogo Agregar nuevo elemento.
6. Asegúrese de que esté marcada la casilla de verificación Seleccionar la página principal tal como se muestra en la figura 6.


![Agregar una nueva página de contenido](master-pages/_static/image3.gif)

**Figura 6**: agregar una nueva página de contenido


1. Haga clic en Agregar.
2. Seleccione exercise1.master en seleccionar un cuadro de diálogo página principal como se muestra en la figura 7.
3. Haga clic en Aceptar para agregar la nueva página de contenido.

Aparece la página de contenido nuevo en Visual Studio con un control de contenido para cada control ContentPlaceHolder en la página maestra. De forma predeterminada, los controles de contenido están vacíos, por lo que puede agregar su propio contenido. Si le gusta iba para que poder usar el contenido desde el control ContentPlaceHolder en la página maestra, simplemente haga clic en el símbolo de etiqueta inteligente (la pequeña flecha negra en la esquina superior derecha del control) y elija *predeterminado el contenido de patrones* en la etiqueta inteligente tal como se muestra en **figura 8**. Al hacerlo, el elemento de menú cambia a *crear contenido personalizado*. Al hacer clic en él en ese momento, se quita el contenido de la página maestra que le permite definir contenido personalizado para ese control de contenido determinado.


![Establecer un Control de contenido en el valor predeterminado el contenido de las páginas principal](master-pages/_static/image4.gif)

**Figura 7**: establecer un Control de contenido en predeterminado el contenido de las páginas principal


## <a name="connecting-master-page-and-content-pages"></a>Conexión de la página maestra y las páginas de contenido

La asociación entre una página maestra y una página de contenido puede configurarse en uno de cuatro formas distintas:

- El <strong>MasterPageFile</strong> atributo de la @Page directiva
- Establecer el **Page.MasterPageFile** propiedad en el código.
- El **&lt;páginas&gt;** elemento en el archivo de configuración de aplicaciones (web.config en la carpeta raíz de la aplicación)
- El **&lt;páginas&gt;** elemento en un archivo de configuración (web.config en una subcarpeta) de las subcarpetas

## <a name="masterpagefile-attribute"></a>MasterPageFile (atributo)

MasterPageFile (atributo) facilita a una página maestra se aplican a una determinada página ASP.NET. También es el método utilizado para aplicar la página principal cuando se protege el **seleccionar la página maestra** casilla como se hizo en el ejercicio 1.

## <a name="setting-pagemasterpagefile-in-code"></a>Establecer Page.MasterPageFile en código.

Estableciendo la propiedad MasterPageFile en el código, puede aplicar una determinada página maestra a su contenido en tiempo de ejecución. Esto es útil en casos donde es posible que deba aplicar una página maestra específica en función de un rol de los usuarios u otros criterios. La propiedad MasterPageFile debe establecerse en el método PreInit. Si se establece después del método PreInit, se producirá una excepción InvalidOperationException. La página en el que se establece esta propiedad también debe tener un contenido como control de nivel superior de la página. En caso contrario, se producirá una HttpException cuando se establece la propiedad MasterPageFile.

## <a name="using-the-ltpagesgt-element"></a>Mediante el &lt;páginas&gt; elemento

Puede configurar una página principal para las páginas estableciendo el atributo masterPageFile el &lt;páginas&gt; elemento del archivo web.config. Cuando se usa este método, tenga en cuenta que los archivos web.config inferiores en la estructura de la aplicación pueden invalidar esta configuración. Cualquier atributo MasterPageFile establecido un @Page directiva también invalidará esta configuración. Mediante el &lt;páginas&gt; elemento simplifica el proceso crear un <em>maestro</em> página maestra que se puede invalidar si es necesario en determinadas carpetas o archivos.

## <a name="properties-in-master-pages"></a>Propiedades en las páginas maestras

Una página maestra puede exponer propiedades basta con realizar esas propiedades públicas dentro de la página maestra. Por ejemplo, el código siguiente define una propiedad denominada SomeProperty:

[!code-csharp[Main](master-pages/samples/sample2.cs)]

Para obtener acceso a la propiedad SomeProperty desde la página de contenido, deberá usar el patrón de propiedad de este modo:

[!code-csharp[Main](master-pages/samples/sample3.cs)]

## <a name="nesting-master-pages"></a>Páginas maestras de anidamiento

Las páginas principales son la solución perfecta para garantizar un aspecto común a través de una aplicación Web grande. Sin embargo, no es raro tener determinadas partes de un recurso compartido de sitios grande una interfaz común, mientras que otras partes compartan una interfaz diferente. Para abordar esta necesidad, varias páginas principales son la solución perfecta. Sin embargo, aún así no trata el hecho de que una aplicación de gran tamaño puede tener ciertos componentes (por ejemplo, un menú, por ejemplo) que se comparten entre todas las páginas y otros componentes que se comparten con solo determinadas secciones del sitio. En ese tipo de situaciones, las páginas maestras anidadas cubre las necesidades perfectamente. Como ha visto una página maestra normal consta de una página maestra y una página de contenido. En una situación de la página maestra anidada, hay dos páginas principales; una página maestra principal y un servidor maestro secundario. La página maestra secundaria también es una página de contenido y su patrón es la página maestra primaria.

Este es el código para una página principal típica:

[!code-aspx[Main](master-pages/samples/sample4.aspx)]

En un escenario principal anidado, esto sería el maestro de primaria. Otra página maestra usaría esta página como su página principal, y ese código tendría el aspecto siguiente:

[!code-aspx[Main](master-pages/samples/sample5.aspx)]

Tenga en cuenta que en este escenario, el maestro secundario es también una página de contenido de la página maestra principal. Todo el contenido del patrón de secundarios de aparece dentro de un control de contenido que obtiene su contenido desde el control ContentPlaceHolder del elemento primario.

> [!NOTE]
> Compatibilidad con el diseñador no está disponible para las páginas maestras anidadas. Cuando está desarrollando con patrones anidados, deberá usar la vista del origen.


Este vídeo muestra un tutorial sobre cómo usar las páginas maestras anidadas.


![](master-pages/_static/image1.png)


[Abra vídeo de pantalla completa](master-pages/_static/nested1.wmv)


![Seleccionar una página maestra](master-pages/_static/image4.jpg)

**Figura 8**: seleccionar una página maestra
