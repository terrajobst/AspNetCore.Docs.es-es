---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Descripción de localización de AJAX de ASP.NET | Microsoft Docs
author: scottcate
description: La localización es el proceso de diseño y la integración de soporte técnico para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. El Mic...
ms.author: riande
ms.date: 03/14/2008
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 86cbf150708f1db711b40ccbc25345afeb3e542a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835382"
---
<a name="understanding-aspnet-ajax-localization"></a>Descripción de localización de AJAX de ASP.NET
====================
por [Scott Cate](https://github.com/scottcate)

[Descargar PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> La localización es el proceso de diseño y la integración de soporte técnico para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. La plataforma de Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de aplicaciones de ASP.NET estándar integrando el modelo de localización estándar. NET; el marco de AJAX de Microsoft usan el modelo integrado para admitir los diversos escenarios en los que se puede realizar la localización.


## <a name="introduction"></a>Introducción

La tecnología de Microsoft ASP.NET ofrece un modelo de programación orientada a objetos y controlado por eventos y une con las ventajas del código compilado. Sin embargo, su modelo de procesamiento en el servidor tiene varias desventajas inherentes de la tecnología, muchas de las cuales se pueden solucionar si las nuevas características incluidas en el espacio de nombres System.Web.Extensions, que encapsula los servicios de AJAX de Microsoft en .NET Framework 3.5. Estas extensiones permiten muchas características de cliente enriquecidas, anteriormente disponibles como parte de las extensiones de AJAX de ASP.NET 2.0, pero ahora forma parte de la biblioteca de clases Base. Los controles y características en este espacio de nombres incluyen la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad de acceder a servicios Web a través de script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente diseñada para reflejar muchas de esquemas de control que se muestra en el conjunto de controles de servidor ASP.NET.

Este artículo examina las características de localización presentes en el marco de AJAX de Microsoft y la secuencia de comandos de Microsoft AJAX Library, en el contexto de la necesidad empresarial de compatibilidad de localización y revisando ya ha integrado la compatibilidad para la localización en web aplicaciones proporcionadas por .NET Framework. La secuencia de comandos de Microsoft AJAX Library se utiliza el formato de archivo de .resx ya utilizado por aplicaciones. NET, que proporciona compatibilidad integrada con IDE y un tipo de recurso que se pueda compartir.

Estas notas del producto se basa en la versión Beta 2 de Microsoft Visual Studio 2008. Estas notas del producto también se da por supuesto que trabajará con Visual Studio 2008, no Visual Web Developer Express y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código utilizarán las plantillas de proyecto que pueden no estar disponibles en Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necesidad de localización*

Especialmente para los desarrolladores de aplicaciones empresariales y los desarrolladores de componentes, la capacidad para crear herramientas que pueden tener en cuenta las diferencias entre idiomas y referencias culturales se ha vuelto cada vez más es necesaria. Diseño de componentes con la capacidad de adaptarse a la configuración regional del cliente aumenta la productividad del desarrollador y reduce la cantidad de trabajo necesario para la adaptación de un componente para funcionar globalmente.

La localización es el proceso de diseño y la integración de soporte técnico para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. La plataforma de Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de aplicaciones de ASP.NET estándar integrando el modelo de localización estándar. NET; el marco de AJAX de Microsoft usan el modelo integrado para admitir los diversos escenarios en los que se puede realizar la localización. Con el marco de AJAX de Microsoft, las secuencias de comandos pueden localizados por la que se implementan en ensamblados satélite, o mediante el uso de una estructura de sistema de archivos estáticos.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incrustación de Scripts con los ensamblados satélite*

Coherente con la estrategia de localización estándar de .NET Framework, los recursos pueden incluirse en los ensamblados satélite. Los ensamblados satélite proporcionan varias ventajas sobre la inclusión de recurso tradicionales en binarios, se puede actualizar cualquier localización determinado sin actualizar la imagen más grande, localizaciones adicionales pueden implementarse simplemente al instalar los ensamblados satélite en la carpeta de proyecto y los ensamblados satélite pueden implementarse sin causar una recarga del ensamblado del proyecto principal. Especialmente en los proyectos ASP.NET, esto es útil, ya que puede reducir significativamente la cantidad de recursos del sistema utilizados por las actualizaciones incrementales y mínimamente interrumpe el uso del sitio Web de producción.

Los scripts incrustados en ensamblados incluyéndolos en .resx de administra (o compilan .resources) archivos, que se incluyen en el ensamblado en tiempo de compilación. Sus recursos, a continuación, se ponen a disposición a la aplicación de script a través del código generado en tiempo de ejecución de AJAX, a través de los atributos de nivel de ensamblado

*Convenciones de nomenclatura para archivos de Script incrustados*

La administración de la secuencia de comandos de marco de AJAX de Microsoft admite una variedad de opciones para su uso en la implementación y prueba de secuencias de comandos y se proporcionan instrucciones para facilitar estas opciones.

*Para facilitar la depuración:*

Los scripts de lanzamiento (producción) no deben incluir el `.debug` calificador en el nombre de archivo. Las secuencias de comandos diseñadas para la depuración debe incluir `.debug` en el nombre de archivo.

*Para facilitar la localización:*

Las secuencias de comandos de la referencia cultural neutra no deben incluir ningún identificador de referencia cultural, el nombre del archivo. Para los scripts que contienen recursos localizados, debe especificarse el código de idioma ISO en el nombre de archivo. Por ejemplo, `es-CO` el acrónimo en español, Columbia.

En la tabla siguiente se resume la con ejemplos de las convenciones de nomenclatura de archivos:

| Filename | Significado |
| --- | --- |
| Script.js | Una secuencia de comandos de la referencia cultural neutra de versión de lanzamiento. |
| Script.debug.js | Una secuencia de comandos de la referencia cultural neutra de versión de depuración. |
| Script.en-US.js | Versión de versión, inglés de Estados Unidos secuencia de comandos. |
| Script.debug.es-CO.js | Un script de Columbia español, versión de depuración. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Tutorial: Crear un Script incrustado, localizado

*Nota: este tutorial requiere el uso de Visual Studio 2008, como Visual Web Developer Express no incluye una plantilla de proyecto para proyectos de biblioteca de clases.*

1. Cree un nuevo proyecto de sitio Web con ASP.NET AJAX Extensions integrado. Cree otro proyecto, un proyecto de biblioteca de clases, dentro de la solución denominado LocalizingResources.
2. Agregue un archivo Jscript denominado VerifyDeletion.js al proyecto LocalizingResources, así como los archivos de recursos .resx denominados DeletionResources.resx y DeletionResources.es.resx. El primero contiene los recursos de la referencia cultural neutra; el último contendrá recursos de idioma de español.
3. Agregue el código siguiente al VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para quienes no conocen la sintaxis de JavaScript Regex, texto dentro únicas barras diagonales (en el ejemplo anterior, es un ejemplo /FILENAME/) denota un objeto RegExp. MSDN Library contiene una amplia referencia de JavaScript y recursos en los objetos nativos de JavaScript se encuentran en línea.

1. Agregue las siguientes cadenas de recursos a DeletionResources.resx: 

    **VerifyDelete**: ¿está seguro de que desea eliminar el nombre de archivo?

    **Eliminar**: nombre de archivo se ha eliminado.

1. Agregue las siguientes cadenas de recursos a DeletionResources.es.resx: 

    **¿VerifyDelete**: Est seguro que desee quitar del nombre de archivo?

    **Eliminar**: nombre de archivo se ha quitado.
2. Agregue las siguientes líneas de código al archivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Agregue referencias a System.Web y System.Web.Extensions al proyecto LocalizingResources.
2. Agregue una referencia al proyecto LocalizingResources desde el proyecto de sitio Web.
3. En default.aspx, bajo el proyecto de sitio Web, actualice el control ScriptManager con el siguiente marcado adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. En default.aspx, en cualquier lugar en la página, incluya este marcado:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Presione F5. Si se le pida, habilite la depuración. Cuando se carga la página, presione el botón Eliminar. Tenga en cuenta que deberá en inglés (a menos que el equipo se configura para preferir recursos de idioma de español de forma predeterminada) para la confirmación.
2. Cierre la ventana del explorador y vuelva a default.aspx. En el @Page directiva de encabezado, auto de reemplazo para Culture y UICulture con es-es al directorio. Presione F5 de nuevo para iniciar la aplicación web nuevo en el explorador. Esta vez, tenga en cuenta que deberá eliminar el archivo en español:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Haga clic aquí para ver imagen en tamaño completo](understanding-asp-net-ajax-localization/_static/image6.png))


Tenga en cuenta que hay diversas variaciones para este tutorial. Por ejemplo, las secuencias de comandos se puede registrar con el control ScriptManager mediante programación durante la carga de página.

## <a name="including-a-static-script-file-structure"></a>*Como una estructura de archivos de Script estático*

Cuando se usan archivos de script estático para la implementación, perderá algunas de las ventajas del uso de la combinación de localización .NET inherente. Es principalmente visible que se pierda el tipo automática generado, que incluyen archivos de recursos de la secuencia de comandos; Por ejemplo, en el tutorial anterior, los recursos se exponen mediante un tipo generado automáticamente llamado mensaje desde el control ScriptManager.

Sin embargo, hay algunas ventajas derivadas del uso de una estructura de archivos de script estático. Las actualizaciones pueden realizarse sin volver a compilar e implementar los ensamblados satélite, y el uso de una estructura de archivo estático también se puede hacer para invalidar el script incrustado, para integrar una parte menor de funcionalidad que es posible que no se han enviado con un componente.

Microsoft recomienda evitar un problema de control de versiones mediante la generación automática de los recursos de script durante la compilación del proyecto. Al mantener un código de script amplia base, puede convertirse en cada vez más difícil para asegurarse de que se reflejan los cambios de código en cada script localizados. Como alternativa, puede simplemente mantener una secuencia de comandos de lógica y varios scripts de localización, combinar los archivos durante la compilación del proyecto.

Porque no hay recursos para incluir de forma declarativa, deben ser archivos de script estático al que hace referencia mediante la adición `<asp:ScriptElement>` elementos como un elemento secundario de la `<Scripts>` etiqueta del control ScriptManager, o agregando mediante programación `ScriptReference` objetos para el `Scripts` propiedad de la `ScriptManager` control en la página en tiempo de ejecución.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*El control ScriptManager y su papel en la localización*

El control ScriptManager habilita varios comportamientos automáticos para las aplicaciones localizadas:

- Busca automáticamente los archivos de script en función de la configuración y las convenciones de nomenclatura; Por ejemplo, carga habilitado para debug scripts cuando está en modo de depuración, y cargas localizadas scripts basados en la selección de interfaz de usuario del explorador.
- Permite la definición de referencias culturales, incluidas las referencias culturales personalizadas.
- Habilita la compresión de archivos de script a través de HTTP.
- Almacena en caché las secuencias de comandos para administrar eficientemente el número de solicitudes.
- Agrega una capa de direccionamiento indirecto a secuencias de comandos mediante la canalización de ellos a través de una dirección URL de cifrado.

Referencias de script pueden agregarse al control ScriptManager mediante programación o marcado declarativo. Marcado declarativo es especialmente útil al trabajar con scripts incrustados en ensamblados que no sea el sitio web propio proyecto, como el nombre de la secuencia de comandos probablemente no cambiará a medida que se insertan las revisiones a través.

## <a name="summary"></a>Resumen

A medida que crecen las aplicaciones web para llegar a un público más amplio, la necesidad de poder tener acceso a más amplias comunidades y referencias culturales se convierte en principal a un modelo de negocio; deben ser capaz de tratar con divisas extranjeras aplicaciones web de comercio electrónico, sistemas de administración de contenido tienen que ser capaz de no solo para presente que su contenido, pero también sus sugerencias de navegación y los campos de formulario en otros lenguajes y las compañías necesitan saber que esta necesidad puede obtener acceso.

.NET Framework admite intrínsecamente un marco de trabajo de localización enriquecida, utilizando los ensamblados satélite y archivos de recursos (.resx) XML para presentar una manera uniforme para buscar imágenes y cadenas de recursos. Las extensiones de AJAX de ASP.NET, incluido el marco de AJAX de Microsoft y la secuencia de comandos de Microsoft AJAX Library, proporcionan compatibilidad para este modelo de programación en el código del lado cliente, lo que permite búsquedas de cadena fácil de los recursos. Los ensamblados satélite admiten la inclusión automática de los recursos de script (archivos .js real) a través de ScriptResource.axd siempre y cuando los nombres de archivo siguen un esquema de nomenclatura determinado. Con esta compatibilidad, ASP.NET AJAX Extensions simplifican la localización de scripts y la globalización de las aplicaciones.

## <a name="bio"></a>*Biografía*

Scott Cate ha estado trabajando con tecnologías Web de Microsoft desde 1997 y es el presidente de myKB.com ([www.myKB.com](http://www.myKB.com)) donde se especializa en la escritura de ASP.NET en función de las aplicaciones que se centra en soluciones de Software de Base de conocimiento. Se puede establecer contacto con Scott por correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Siguiente](understanding-asp-net-ajax-web-services.md)
