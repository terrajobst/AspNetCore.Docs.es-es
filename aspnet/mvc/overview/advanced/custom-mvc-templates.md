---
uid: mvc/overview/advanced/custom-mvc-templates
title: Plantilla personalizada de MVC | Microsoft Docs
author: joeloff
description: Crear una plantilla como una extensión VSIX.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2012
ms.topic: article
ms.assetid: b0a214c7-2f38-4dbc-b47f-bd7bd9df97bd
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/advanced/custom-mvc-templates
msc.type: authoredcontent
ms.openlocfilehash: 715990e2d7151ad1ce8288169ef4bdd5c4243369
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37367249"
---
<a name="custom-mvc-template"></a>Plantilla personalizada de MVC
====================
por [Jacques Eloff](https://github.com/joeloff)

La versión de MVC 3 Tools Update para Visual Studio 2010 introdujo a un asistente de proyecto independiente para los proyectos de MVC. El cambio se realizó por dos factores. En primer lugar, la introducción de nuevas plantillas de MVC 3 y compatibilidad con motores de vista adicionales como Razor dar lugar a que genera el cuadro de diálogo nuevo proyecto en Visual Studio. En segundo lugar, los clientes tenían han formulado para puntos de extensibilidad y el Asistente para nuevo proyecto MVC nos podría permitirse la oportunidad de responder a estas solicitudes.

Adición de plantillas personalizadas era un proceso arduo que dependía de mediante el registro para que las nuevas plantillas visible para el Asistente para proyectos MVC. El autor de una nueva plantilla tenía incluirlo dentro de un archivo MSI para asegurarse de que las entradas del registro necesarias se crearía en tiempo de instalación. La alternativa era hacer que un archivo ZIP que contiene la plantilla disponible y tiene el usuario final cree manualmente las entradas del registro necesarias.

Ninguno de los enfoques mencionados anteriormente es ideal, por lo que decidimos aprovechar algunas de las infraestructuras existentes proporcionadas por [VSIX](https://msdn.microsoft.com/library/ff363239.aspx) extensiones para que sea más fácil de crear, distribuir e instalar las plantillas personalizadas de MVC a partir de MVC 4 para Visual Studio 2012. Algunas de las ventajas que ofrece este enfoque son:

- Una extensión VSIX puede contener varias plantillas que admiten diferentes lenguajes (C# y Visual Basic) y varios motores de vista (ASPX y Razor).
- Una extensión VSIX puede tener como destino varias SKU de Visual Studio incluidas SKU de Express.
- El [Galería de Visual Studio](https://visualstudiogallery.msdn.microsoft.com/) facilita la distribución de la extensión a un público amplio.
- Facilitando la tarea crear las correcciones y actualizaciones de las plantillas personalizadas, se pueden actualizar las extensiones VSIX.

## <a name="prerequisites"></a>Requisitos previos

- Los usuarios deben estar familiarizado con la creación de plantillas de proyecto, incluido el marcado necesario para los archivos de vstemplate, etcetera.
- Los usuarios deberán tener Visual Studio Professional y versiones superiores instalados. Crear proyectos VSIX no admiten las SKU de Express.
- [Visual Studio 2012 SDK](https://www.microsoft.com/download/details.aspx?id=30668) instalado.

## <a name="example"></a>Ejemplo

El primer paso es crear un nuevo proyecto VSIX con C# o Visual Basic. Seleccione **archivo > Nuevo proyecto**, a continuación, haga clic en **extensibilidad** en el panel izquierdo y seleccione el **proyecto VSIX**.

![nuevo proyecto](custom-mvc-templates/_static/image1.jpg)

Una vez creado el proyecto, se abrirá el diseñador VSIX.

![Metadatos del Diseñador de proyectos](custom-mvc-templates/_static/image2.jpg)

El diseñador puede usarse para modificar algunas de las propiedades generales de la extensión que se mostrará a los usuarios cuando instale la extensión o examinar las extensiones instaladas en Visual Studio (**Herramientas > extensiones y actualizaciones**). Cuando haya completado la información general haga clic en el **pestaña destinos de instalación**.

![Destinos de instalación de diseñador del proyecto](custom-mvc-templates/_static/image3.jpg)

Esta ficha sirve para especificar la SKU y versiones de Visual Studio que son compatibles con la extensión. Seleccione la casilla de verificación **este VSIX se instala para todos los usuarios** para habilitar las instalaciones por equipo de la extensión VSIX. Haga clic en el **New** situado a la derecha para agregar SKU adicionales, como Web Developer Express (VWD).

![Agregar nuevo destino de instalación](custom-mvc-templates/_static/image4.jpg)

Si piensa admitir todos los profesionales y versiones posteriores SKU (Professional, Premium y Ultimate) sólo deberá seleccionar la SKU mínima de la familia de **Microsoft.VisualStudio.Pro**. No olvide guardar todos los cambios una vez completados los destinos de instalación.

![Destinos de instalación de diseñador del proyecto](custom-mvc-templates/_static/image5.jpg)

El **activos** ficha sirve para agregar todos los archivos de contenido a la extensión VSIX. Debido a que MVC requiere metadatos personalizados que va a editar el XML sin formato del archivo de manifiesto de VSIX en lugar de usar el **activos** tab para agregar contenido. Empiece agregando el contenido de la plantilla al proyecto VSIX. Es importante que la estructura de la carpeta y el contenido refleja el diseño del proyecto. El ejemplo siguiente contiene cuatro plantillas de proyecto que se derivan de la plantilla de proyecto de MVC básica. Asegúrese de que todos los archivos que componen la plantilla de proyecto (todos los elementos debajo de la carpeta ProjectTemplates) se agregan a la **contenido** itemgroup en VSIX de proyecto de archivos y que cada elemento contiene el  **CopyToOutputDirectory** y **IncludeInVsix** establecer metadatos, como se muestra en el ejemplo siguiente.

&lt;Incluir contenido =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx\BasicWeb.config&quot;&gt;

&lt;CopyToOutputDirectory&gt;siempre&lt;/CopyToOutputDirectory&gt;

&lt;IncludeInVSIX&gt;true&lt;/IncludeInVSIX&gt;

&lt;/ Content&gt;

Si no es así, el IDE intentará compilar el contenido de la plantilla al compilar la extensión VSIX y probablemente verá un error. Los archivos de código en las plantillas a menudo contienen especial [parámetros de plantilla](https://msdn.microsoft.com/library/eehb4faa(v=vs.110).aspx) utilizado por Visual Studio cuando se crea una instancia de la plantilla de proyecto y, por tanto, no se puede compilar en el IDE.

![Explorador de soluciones](custom-mvc-templates/_static/image6.jpg)

Cierre el diseñador VSIX, a continuación, haga clic con el botón derecho en el **source.extension.manifest** archivo **el Explorador de soluciones** y seleccione **abrir con** y elija el **(XML Editor de texto)** opción.

![Abrir cuadro de diálogo](custom-mvc-templates/_static/image7.jpg)

Crear un **&lt;activos&gt;** elemento y agregue un **&lt;activos&gt;** (elemento) para cada archivo que se debe incluir en VSIX. El **tipo** atributo de cada **&lt;activos&gt;** elemento debe establecerse en **Microsoft.VisualStudio.Mvc.Template**. Se trata de un espacio de nombres personalizado que comprende únicamente el Asistente de proyecto MVC. Consulte la documentación de esquema 2.0 de VSIX para obtener más información sobre la estructura y el diseño del archivo de manifiesto.

Limitarse a agregar los archivos a la extensión VSIX no es suficiente para registrar las plantillas con el Asistente MVC. Deberá proporcionar información como el nombre de la plantilla, descripción, motores de vista compatible y lenguaje de programación para el Asistente MVC. Esta información se transporta en atributos personalizados asociados con el **&lt;activos&gt;** para cada elemento **vstemplate** archivo.

&lt;Recurso d:VsixSubPath =&quot;ProjectTemplates\MyMvcWebApplicationProjectTemplate.csaspx&quot;

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

A continuación encontrará una explicación de los atributos personalizados que deben estar presentes:

- **ProjectType** debe establecerse en MVC.
- **Lenguaje** designa el lenguaje de programación compatible con la plantilla. Los valores válidos son de C# o VB.
- **ViewEngine** designa el motor de vistas compatible con la plantilla como Aspx o Razor. Puede especificar un valor personalizado para este campo.
- **TemplateId** se usa para agrupar las plantillas. Si el valor coincide con un identificador de plantilla existente será reemplazar plantillas que se registraron anteriormente con el Asistente MVC.
- **Título** designa la breve descripción que aparece en el Asistente MVC debajo de cada plantilla de proyecto.
- **Descripción** designa una descripción más detallada de la plantilla.

Una vez que haya agregado todos los archivos en el manifiesto y guardarlo, observará que el **activos** ficha en el diseñador mostrará todos los archivos, pero no los atributos personalizados que ha agregado a la **&lt;activos&gt;** elementos para el **vstemplate** archivos.

![Activos de diseñador de proyectos](custom-mvc-templates/_static/image8.jpg)

Todo lo que queda ahora es compilar el proyecto VSIX e instalarlo.

Asegúrese de que estén cerradas todas las instancias de Visual Studio en el equipo donde va a probar la extensión VSIX. Visual Studio examina si hay nuevas extensiones durante el inicio, por lo que si se abre el IDE mientras se instala una extensión VSIX deberá reiniciar Visual Studio. En el explorador, haga doble clic en el archivo VSIX para iniciar el **instalador de VSIX**, haga clic en **instalar** y, a continuación, inicie Visual Studio.

![Instalador de VSIX](custom-mvc-templates/_static/image9.jpg)

En el menú, seleccione **Herramientas > extensiones y actualizaciones** para confirmar que se ha instalado la extensión. Si el instalador de VSIX se notifican los errores durante la instalación de la extensión puede ver el registro del instalador de VSIX para obtener más información. Normalmente, se crea el registro en el **% temp %** carpeta del usuario que instala la extensión, por ejemplo **C:\Users\Bob\AppData\Local\Temp**.

![Extensiones y actualizaciones](custom-mvc-templates/_static/image10.jpg)

Después de cerrar la ventana puede crear un proyecto de MVC 4 para ver si las nuevas plantillas se muestran en el Asistente MVC.

![Nuevo proyecto de ASP.NET MVC 4](custom-mvc-templates/_static/image11.jpg)

## <a name="limitations"></a>Limitaciones

1. El Asistente MVC no admite plantillas personalizadas localizadas.
2. El asistente no notificará los errores si se produce un error buscar las plantillas personalizadas. Si cualquiera de los atributos personalizados necesarios están presente, la plantilla podría excluirse simplemente desde el asistente.
