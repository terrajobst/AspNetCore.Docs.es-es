---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: Trabajar con formularios HTML en los sitios de ASP.NET Web Pages (Razor) | Documentos de Microsoft
author: tfitzmac
description: Un formulario es una sección de un documento HTML en la que colocar los controles de entrada del usuario, como cuadros de texto, casillas de verificación, botones de opción y listas desplegables. Utilizar formularios qu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a>Trabajar con formularios HTML en sitios de ASP.NET Web Pages (Razor)
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo procesar un formulario HTML (con los botones y cuadros de texto) cuando se trabaja en un sitio Web de ASP.NET Web Pages (Razor).
> 
> **Lo que aprenderá:** 
> 
> - Cómo crear un formulario HTML.
> - Cómo leer la entrada de usuario del formulario.
> - Cómo validar proporcionados por el usuario.
> - Cómo restaurar los valores de formulario una vez enviada la página.
> 
> Estos son los conceptos presentados en el artículo de programación de ASP.NET:
> 
> - Objeto `Request`.
> - Validación de entrada.
> - Codificación HTML.
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>Versiones de software que se usa en el tutorial
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> Este tutorial también funciona con ASP.NET Web Pages 2.


## <a name="creating-a-simple-html-form"></a>Crear un formulario HTML sencillo

1. Crear un nuevo sitio Web.
2. En la carpeta raíz, cree una página web denominada *Form.cshtml* y escriba el siguiente marcado:

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. Inicie la página en el explorador. (En WebMatrix, en la **archivos** área de trabajo, haga clic en el archivo y, a continuación, seleccione **iniciar en un explorador**.) Un formulario simple con tres campos de entrada y un **enviar** se muestra el botón.

    ![Captura de pantalla de un formulario con tres cuadros de texto.](4-working-with-forms/_static/image1.jpg)

    En este punto, si hace clic en el **enviar** botón, no ocurre nada. Para que el formulario sea útil, tienes que agregar código que se ejecutará en el servidor.

## <a name="reading-user-input-from-the-form"></a>Leer la entrada de usuario del formulario

Para procesar el formulario, agregue código que lee los valores de campo enviado y realiza una acción con ellos. Este procedimiento muestra cómo leer los campos y mostrar la entrada de usuario en la página. (En una aplicación de producción, se suele hacer cosas más interesantes con proporcionados por el usuario. Llevará a cabo en el artículo sobre cómo trabajar con bases de datos.)

1. En la parte superior de la *Form.cshtml* de archivos, escriba el código siguiente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    Cuando el usuario solicita la página por primera vez, se muestra sólo el formulario vacío. El usuario (que será también) rellene el formulario y, a continuación, haga clic en **enviar**. Esto envía (o publica) la entrada del usuario en el servidor. De forma predeterminada, la solicitud se pasa a la misma página (es decir, *Form.cshtml*).

    Cuando se envía a la página en este momento, se muestran los valores que especificó justo encima de la forma:

    ![Captura de pantalla que muestra los valores que ha escrito aparece en la página.](4-working-with-forms/_static/image2.jpg)

    Examine el código de la página. Realice primero la `IsPost` método para determinar si la página se está publicando &#8212; es decir, si un usuario hace clic en el **enviar** botón. Si se trata de una publicación, `IsPost` devuelve true. Esta es la manera estándar en ASP.NET Web Pages para determinar si está trabajando con una solicitud inicial (una solicitud GET) o una devolución de datos (una solicitud POST). (Para obtener más información acerca de GET y POST, consulte la barra lateral "HTTP GET y POST y la IsPost propiedad" en [Introducción a ASP.NET Web Pages de programación mediante la sintaxis Razor](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)

    A continuación, se obtienen los valores que el usuario rellena a partir de la `Request.Form` objeto y se coloca en variables para más tarde. La `Request.Form` objeto contiene todos los valores que se enviaron con la página, cada uno de ellos se identifican mediante una clave. La clave es el equivalente a la `name` atributo del campo de formulario que desea leer. Por ejemplo, para leer el `companyname` campo (cuadro de texto), se utiliza `Request.Form["companyname"]`.

    Valores de formulario se almacenan en la `Request.Form` objeto como cadenas. Por lo tanto, cuando tiene que trabajar con un valor como un número o una fecha o algún otro tipo, tendrá que convertir una cadena a ese tipo. En el ejemplo, el `AsInt` método de la `Request.Form` se usa para convertir el valor del campo de los empleados (que contiene un recuento de empleados) en un entero.
2. Inicie la página en el explorador, rellene los campos de formulario y haga clic en **enviar**. La página muestra los valores especificados.

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a>Codificación de seguridad y la apariencia de HTML
> 
> HTML tiene usos especiales para los caracteres como `<`, `>`, y `&`. Si aparecen estos caracteres especiales en el que no están previsto, que pueden que se dañen la apariencia y funcionalidad de la página web. Por ejemplo, el explorador interpreta la `<` de caracteres (a menos que vaya seguido de un espacio) como el principio de un elemento HTML, como `<b>` o `<input ...>`. Si el explorador no reconoce el elemento, simplemente se descarta la cadena que comienza con `<` hasta que llega a algo que volverá a reconocer. Obviamente, esto puede producir alguna representación raro en la página.
> 
> Codificación HTML, estos caracteres reservados reemplaza con un código que exploradores interpretan como el símbolo correcto. Por ejemplo, el `<` carácter se sustituye por `&lt;` y `>` carácter se sustituye por `&gt;`. El explorador presenta estas cadenas de reemplazo como los caracteres que desea ver.
> 
> Es una buena idea usar siempre que se muestran las cadenas de codificación HTML (entrada) que se obtuvo de un usuario. Si no lo hace, un usuario puede intentar obtener la página web para ejecutar un script malintencionado o hacer algo más que pone en peligro la seguridad del sitio o que simplemente no es lo que deseado. (Esto es especialmente importante si ninguna entrada del usuario, almacenar un lugar y, a continuación, mostrar más tarde &#8212; por ejemplo, como un comentario de blog, revisión de usuario o algo similar a.)
> 
> Para ayudar a evitar estos problemas, ASP.NET Web Pages automáticamente codifica en HTML cualquier texto de contenido que usted desde el código de salida. Por ejemplo, al mostrar el contenido de una variable o una expresión mediante código como `@MyVar`, ASP.NET Web Pages codifica automáticamente la salida.


## <a name="validating-user-input"></a>Validar la entrada del usuario

Los usuarios cometen errores. Pídales que completen en un campo y se olvidan de hacerlo, o pídale que especifique el número de empleados y escriban un nombre en su lugar. Para asegurarse de que un formulario se ha rellenado correctamente antes de que lo procesa, validar la entrada del usuario.

Este procedimiento muestra cómo validar todos los campos de formulario tres para asegurarse de que el usuario no dejarlo en blanco. También comprobar que el valor de recuento de empleados es un número. Si hay errores, se mostrará un error de mensaje que indica al usuario qué valores no pasa la validación.

1. En el *Form.cshtml* de archivos, sustituya el primer bloque de código con el código siguiente: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    Para validar entrada de usuario, puede utilizar el `Validation` auxiliar. Registrar campos obligatorios mediante una llamada a `Validation.RequireField`. Registrar otros tipos de validación mediante una llamada a `Validation.Add` y especificar el campo que se valida y el tipo de validación que se realiza.

    Cuando se ejecuta la página, ASP.NET realiza todas la validación por usted. Puede comprobar los resultados mediante una llamada a `Validation.IsValid`, que devuelve true si todo lo que pasa y false si cualquier campo de un error de validación. Normalmente, se llama a `Validation.IsValid` antes de realizar cualquier procesamiento en la entrada del usuario.
2. Actualización del `<body>` elemento mediante la adición de tres llamadas a la `Html.ValidationMessage` método, similar al siguiente:

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    Para mostrar mensajes de error de validación, puede llamar a Html.`ValidationMessage` y pásele el nombre del campo que desea que el mensaje.
3. Ejecute la página. Deje los campos en blanco y haga clic en **enviar**. Vea mensajes de error.

    ![Captura de pantalla que muestra los mensajes de error aparece si proporcionados por el usuario no pasan la validación.](4-working-with-forms/_static/image3.jpg)
4. Agregar una cadena (por ejemplo, "ABC") a la **Employee Count** campo y haga clic en **enviar** nuevo. Esta vez que vea un error que indica que la cadena no está en el formato correcto, es decir, un número entero.

    ![Captura de pantalla que muestra los mensajes de error aparece si los usuarios escribir una cadena para el campo de los empleados.](4-working-with-forms/_static/image4.jpg)

ASP.NET Web Pages proporciona más opciones para validar la entrada del usuario, incluida la capacidad para realizar automáticamente la validación mediante script de cliente, por lo que los usuarios obtengan información inmediata en el explorador. Consulte la [recursos adicionales](#Additional_Resources) sección más adelante para obtener más información.

## <a name="restoring-form-values-after-postbacks"></a>Restablecer los valores de formulario después de las devoluciones de datos

Al probar la página en la sección anterior, puede que haya observado si tuviera un error de validación, todo lo que escribió (no solo los datos no válidos) se ha desaparecido y tenía que volver a escribir valores para todos los campos. Esto ilustra un aspecto importante: al enviar una página, procesarlo y, a continuación, vuelva a procesar la página, la página se vuelve a crear desde cero. Como ha podido comprobar, esto significa que se pierden los valores que estaban en la página cuando se envió.

Puede corregir esto fácilmente, sin embargo. Tener acceso a los valores que se enviaron (en la `Request.Form` del objeto, por lo que puede rellenar los valores en los campos del formulario cuando se procesa la página.

1. En el *Form.cshtml* archivo, reemplace la `value` atributos de la `<input>` elementos mediante el `value` atributo.: 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    El `value` atributo de la `<input>` elementos se ha establecido en dinámicamente leer el valor del campo de la `Request.Form` objeto. La primera vez que se solicita la página, los valores de la `Request.Form` objeto están vacías. Esto es correcto, ya que de este modo el formulario está en blanco.
2. Iniciar la página en el explorador, rellene los campos de formulario o dejarlo en blanco y haga clic en **enviar**. Se muestra una página que se muestra los valores enviados.

    ![5 de formularios](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>Recursos adicionales

- [1.001 maneras para obtener la entrada de usuarios Web](https://msdn.microsoft.com/library/ms971057.aspx)
- [Uso de formularios y el procesamiento proporcionados por el usuario](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)
- [Validar la entrada del usuario en los sitios de ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=253002)
- [Usar Autocompletar en formularios HTML](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)
