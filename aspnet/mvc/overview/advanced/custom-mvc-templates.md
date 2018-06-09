---
uid: mvc/overview/advanced/custom-mvc-templates
title: Plantilla MVC personalizado | Documentos de Microsoft
author: joeloff
description: Crear una plantilla como una extensión VSIX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: c3ddd4e341511f520927e924b25d890088adb69e
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "28034612"
---
<a name="custom-mvc-template"></a>Plantilla MVC personalizado
====================
por [Jacques Eloff](https://github.com/joeloff)

La versión de MVC 3 Tools Update para Visual Studio 2010 introdujo a un asistente de proyecto independiente para los proyectos MVC. El cambio se controla mediante dos factores. En primer lugar, la introducción de nuevas plantillas de MVC 3 y soporte técnico para los motores de vista adicionales como Razor dar lugar a que genera el cuadro de diálogo nuevo proyecto en Visual Studio. En segundo lugar, los clientes tenían habían solicitado en puntos de extensibilidad y el Asistente para nuevo proyecto MVC podría permitirse nos la oportunidad de responder a estas solicitudes.

Agregar plantillas personalizadas era un proceso arduo que dependía de mediante el registro para hacer visible para el Asistente para proyecto MVC nuevas plantillas. El autor de una nueva plantilla tuvo que ajustar dentro de un archivo MSI para asegurarse de que las entradas del registro necesarias se crearía durante la instalación. La alternativa consiste en un archivo ZIP que contiene la plantilla disponible y hacer que el usuario final crear manualmente las entradas del registro necesarias.

Ninguno de los métodos mencionados anteriormente es ideal, por lo que hemos decidido aprovechar algunas de la infraestructura existente proporcionada por [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensiones resulte más fácil crear, distribuir e instalar plantillas MVC personalizadas a partir de MVC 4 para Visual Studio 2012. Algunas de las ventajas que ofrece este enfoque son:

- Una extensión VSIX puede contener varias plantillas que admiten diferentes idiomas (C# y Visual Basic) y varios motores de vista (ASPX y Razor).
- Una extensión VSIX puede tener como destino varias SKU de Visual Studio incluidos SKU de Express.
- El [Galería de Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita la distribución de la extensión a un público amplio.
- Lo que sea más fácil crear las correcciones y actualizaciones de sus plantillas personalizadas, se pueden actualizar las extensiones VSIX.

## <a name="prerequisites"></a>Requisitos previos

- Los usuarios deben estar familiarizado con la creación de plantillas de proyecto, incluido el marcado necesario para vstemplate (archivos), etcetera.
- Los usuarios deberán tener Visual Studio Professional y superior instalado. SKU de Express no admiten crear proyectos VSIX.
- [SDK de Visual Studio 2012](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Ejemplo

El primer paso es crear un nuevo proyecto VSIX mediante C# o Visual Basic. Seleccione **archivo > Nuevo proyecto**, a continuación, haga clic en **extensibilidad** en el panel izquierdo y seleccione la **proyecto VSIX**.

![nuevo proyecto](custom-mvc-templates/_static/image1.jpg)

Una vez creado el proyecto, se abrirá el diseñador VSIX.

![Metadatos del Diseñador de proyectos](custom-mvc-templates/_static/image2.jpg)

El diseñador puede utilizarse para modificar algunas de las propiedades generales de la extensión que se mostrará a los usuarios cuando instale la extensión o examinar las extensiones instaladas de Visual Studio (**Herramientas > extensiones y actualizaciones**). Cuando haya completado la información general haga clic en el **pestaña destinos de instalación**.

![Destinos de instalación Diseñador de proyectos](custom-mvc-templates/_static/image3.jpg)

Esta ficha se utiliza para especificar el SKU y versiones de Visual Studio que son compatibles con la extensión. Seleccione la casilla de verificación **este VSIX se instala para todos los usuarios** para habilitar la instalación de cada equipo de la extensión VSIX. Haga clic en el **New** botón de la derecha para agregar las SKU adicionales como Web Developer Express (VWD).

![Agregar nuevo destino de instalación](custom-mvc-templates/_static/image4.jpg)

Si piensa admitir todas las Professional y versiones posteriores SKU (Professional, Premium y Ultimate) solo debe seleccionar la SKU mínima en la familia, **Microsoft.VisualStudio.Pro**. No olvide guardar todos los cambios una vez completados los destinos de instalación.

![Destinos de instalación Diseñador de proyectos](custom-mvc-templates/_static/image5.jpg)

El **activos** ficha sirve para agregar todos los archivos de contenido a la extensión VSIX. Debido a que MVC requiere metadatos personalizados que va a editar el XML sin formato del archivo de manifiesto de VSIX en lugar de utilizar el **activos** tab para agregar contenido. Empiece por agregar el contenido de plantilla al proyecto VSIX. Es importante que la estructura de la carpeta y el contenido refleja el diseño del proyecto. En el ejemplo siguiente contiene cuatro plantillas de proyecto que se derivan de la plantilla de proyecto de MVC básica. Asegúrese de que todos los archivos que componen la plantilla de proyecto (todos los elementos debajo de la carpeta ProjectTemplates) se agregan a la **contenido** itemgroup en VSIX de proyectos de archivo y que cada elemento contiene el  **CopyToOutputDirectory** y **IncludeInVsix** metadatos se establecen como se muestra en el ejemplo siguiente.

&lt;Incluir contenido =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;siempre&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ Content&gt;

De lo contrario, el IDE intentará compilar el contenido de la plantilla al generar la extensión VSIX y probablemente verá un error. Archivos de código en las plantillas a menudo contienen especial [parámetros de plantilla](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) utilizado por Visual Studio cuando se crea una instancia de la plantilla de proyecto y, por tanto, no se pueden compilar en el IDE.

![Explorador de soluciones](custom-mvc-templates/_static/image6.jpg)

Cierre el diseñador VSIX, a continuación, haga clic con el botón secundario en el **source.extension.manifest** en el archivo **el Explorador de soluciones** y seleccione **abrir con** y elija el **() XML Editor de texto)** opción.

![Abrir cuadro de diálogo](custom-mvc-templates/_static/image7.jpg)

Crear un **&lt;activos&gt;** elemento y agregue un **&lt;Asset&gt;** (elemento) para cada archivo que debe incluirse en la extensión VSIX. El **tipo** atributo de cada **&lt;Asset&gt;** elemento debe establecerse en **Microsoft.VisualStudio.Mvc.Template**. Se trata de un espacio de nombres personalizado que entiende únicamente el Asistente de proyecto MVC. Consulte la documentación del esquema de 2.0 VSIX para obtener información adicional sobre la estructura y el diseño del archivo de manifiesto.

No es suficiente para registrar las plantillas con el Asistente MVC limitarse a agregar los archivos a la extensión VSIX. Deberá proporcionar información como el nombre de la plantilla, descripción, los motores de vista admitidas y lenguaje de programación para el Asistente MVC. Esta información se incluye en los atributos personalizados asociados con la **&lt;Asset&gt;** para cada elemento **vstemplate** archivo.

&lt;Asset d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

Type=&quot;Microsoft.VisualStudio.Mvc.Template&quot;

d:Source =&quot;archivo&quot;

Path=&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicMvcWebApplicationProjectTemplate.11.csaspx.vstemplate&quot;

ProjectType =&quot;MVC&quot;

Language =&quot;C#&quot;

ViewEngine =&quot;Aspx&quot;

TemplateId =&quot;MyMvcApplication&quot;

Título =&quot;aplicación Web básica personalizada&quot;

Descripción =&quot;una plantilla personalizada que deriva de una aplicación web de MVC básica (Razor)&quot;

Versión =&quot;4.0&quot;/&gt;

A continuación se muestra una explicación de los atributos personalizados que deben estar presentes:

- **ProjectType** debe establecerse en MVC.
- **Idioma** designa el lenguaje de programación compatible con la plantilla. Los valores válidos son C# o VB.
- **ViewEngine** designa el motor de vistas compatible con la plantilla como Aspx o Razor. Puede especificar un valor personalizado para este campo.
- **TemplateId** se utiliza para agrupar las plantillas. Si el valor coincide con un identificador de plantilla existente será reemplazar plantillas que se registraron anteriormente con el Asistente MVC.
- **Título** designa breve descripción que aparece en el Asistente MVC debajo de cada plantilla de proyecto.
- **Descripción** designa una descripción más detallada de la plantilla.

Después de que haya agregado todos los archivos para el manifiesto y guardarlo, observará que el **activos** ficha en el diseñador mostrará todos los archivos, pero no personalizado atributos que ha agregado a la **&lt;Asset&gt;** elementos para la **vstemplate** archivos.

![Diseñador activos del proyecto](custom-mvc-templates/_static/image8.jpg)

Todo lo que queda ahora es compilar el proyecto VSIX e instalarlo.

Asegúrese de que están cerradas todas las instancias de Visual Studio en el equipo donde vaya a probar la extensión VSIX. Visual Studio examina nuevas extensiones durante el inicio, por ello, si el IDE está abierto al instalar una extensión VSIX debe reiniciar Visual Studio. En el explorador, haga doble clic en el archivo VSIX para iniciar la **instalador VSIX**, haga clic en **instalar** y, a continuación, inicie Visual Studio.

![Instalador VSIX](custom-mvc-templates/_static/image9.jpg)

En el menú, seleccione **Herramientas > extensiones y actualizaciones** para confirmar que se ha instalado la extensión. Si el instalador VSIX no notifica los errores durante la instalación de la extensión puede ver el registro de instalador VSIX para obtener más información. El registro se crea normalmente en el **% temp %** carpeta del usuario que instala la extensión, por ejemplo **C:\Users\Bob\AppData\Local\Temp**.

![Extensiones y actualizaciones](custom-mvc-templates/_static/image10.jpg)

Después de cerrar la ventana puede crear un proyecto de MVC 4 para ver si las nuevas plantillas se muestran en el Asistente MVC.

![Nuevo proyecto de MVC de ASP.NET 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitaciones

1. El Asistente MVC no admite plantillas personalizadas localizadas.
2. El asistente no notificará los errores si se produce un error buscar las plantillas personalizadas. Si cualquiera de los atributos personalizados necesarios están presente, la plantilla se excluirán simplemente desde el Asistente para.
