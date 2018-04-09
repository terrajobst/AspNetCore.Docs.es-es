---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
title: Descripción de localización de AJAX de ASP.NET | Documentos de Microsoft
author: scottcate
description: Localización es el proceso de diseño y la integración del soporte para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. La Mic...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2008
ms.topic: article
ms.assetid: c1a35f18-bab9-41f7-8497-15530c37a09d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-localization
msc.type: authoredcontent
ms.openlocfilehash: 565b0294f57b784bc592b286b3d8b28504110415
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-localization"></a>Descripción de localización de AJAX de ASP.NET
====================
por [Scott ndicar](https://github.com/scottcate)

[Descarga de PDF](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial04_Localization_cs.pdf)

> Localización es el proceso de diseño y la integración del soporte para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. La plataforma Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de las aplicaciones ASP.NET estándar al integrar el modelo de localización estándar. NET; el marco de AJAX de Microsoft usan el modelo integrado para admitir los escenarios distintos en la que se puede realizar la localización.


## <a name="introduction"></a>Introducción

Tecnología de Microsoft, ASP.NET proporciona un modelo de programación orientada a objetos y orientada a eventos y agrupa las ventajas del código compilado. Sin embargo, su modelo de procesamiento en el servidor tiene varias desventajas inherentes de la tecnología, muchas de las cuales se pueden solucionar si las nuevas características incluidas en el espacio de nombres System.Web.Extensions, que encapsula los servicios de AJAX de Microsoft en .NET Framework 3.5. Estas extensiones permiten muchas características de cliente completas, disponibles anteriormente como parte de las extensiones de AJAX de ASP.NET 2.0, pero ahora forma parte de la biblioteca de clases de Base de Framework. Los controles y características en este espacio de nombres incluyen la representación parcial de páginas sin necesidad de una actualización de página completa, la capacidad para tener acceso a servicios Web mediante un script de cliente (incluida la API de generación de perfiles de ASP.NET) y una amplia API del lado cliente diseñada para reflejar muchas de los esquemas de control que se muestra en el conjunto de control de servidor ASP.NET.

Estas notas del producto examina las características de localización presentes en el marco de AJAX de Microsoft y la biblioteca de scripts de AJAX de Microsoft, en el contexto de las necesidades del negocio para la compatibilidad de localización y revisión de soporte técnico ya integrado para la localización en web aplicaciones proporcionadas por .NET Framework. La biblioteca de scripts de AJAX de Microsoft utiliza el formato de archivo de .resx ya en uso por aplicaciones. NET, que proporciona compatibilidad integrada con IDE y un tipo de recurso que se pueda compartir.

Estas notas del producto se basa en la versión Beta 2 de Microsoft Visual Studio 2008. Estas notas del producto también se supone que va a trabajar con Visual Studio 2008, no Visual Web Developer Express y proporcionará tutoriales de acuerdo con la interfaz de usuario de Visual Studio. Algunos ejemplos de código utilizan plantillas de proyecto que podrán no estar disponibles en Visual Web Developer Express.

## <a name="the-need-for-localization"></a>*La necesidad de localización*

Especialmente para los desarrolladores de aplicaciones de empresa y los desarrolladores de componentes, la capacidad para crear herramientas que pueden tener en cuenta las diferencias entre idiomas y referencias culturales ha hecho cada vez más necesaria. Diseño de componentes con la capacidad de adaptación a la configuración regional del cliente, aumenta la productividad del desarrollador y reduce la cantidad de trabajo necesario para la adaptación de un componente a la función global.

Localización es el proceso de diseño y la integración del soporte para un idioma específico y la referencia cultural en una aplicación o un componente de aplicación. La plataforma Microsoft ASP.NET proporciona una amplia compatibilidad para la localización de las aplicaciones ASP.NET estándar al integrar el modelo de localización estándar. NET; el marco de AJAX de Microsoft usan el modelo integrado para admitir los escenarios distintos en la que se puede realizar la localización. Con el marco de AJAX de Microsoft, las secuencias de comandos pueden localizar que se implementa en ensamblados satélite, o usando una estructura del sistema de archivos estáticos.

## <a name="embedding-scripts-with-satellite-assemblies"></a>*Incrustación de secuencias de comandos con ensamblados satélite*

Coherente con la estrategia de localización estándar de .NET Framework, los recursos pueden incluirse en los ensamblados satélite. Ensamblados satélite proporcionan varias ventajas sobre la inclusión de recurso tradicionales en los archivos binarios - cualquier localización determinada puede actualizarse sin actualizar la imagen más grande, adaptaciones adicionales pueden implementarse simplemente mediante la instalación de ensamblados satélite en la carpeta del proyecto y los ensamblados satélite se pueden implementar sin causar una recarga del ensamblado del proyecto principal. Especialmente en proyectos ASP.NET, esto es útil porque puede reducir significativamente la cantidad de recursos del sistema utilizados por las actualizaciones incrementales y uso del sitio Web de producción como mínimo, interrumpirán.

Las secuencias de comandos se incrustan en ensamblados incluyéndolos en administrada .resx (o compilado .resources) archivos, que se incluyen en el ensamblado en tiempo de compilación. Los recursos se ponen a disposición de la aplicación de script a través de código generado en tiempo de ejecución de AJAX, a través de atributos de nivel de ensamblado

*Convenciones de nomenclatura para archivos de Script incrustados*

La administración de la secuencia de comandos de marco de AJAX de Microsoft admite una variedad de opciones para su uso en la implementación y las pruebas de las secuencias de comandos y se proporcionan directrices para facilitar estas opciones.

*Para facilitar la depuración:*

Scripts de lanzamiento (producción) no deben incluir el `.debug` calificador en el nombre de archivo. Las secuencias de comandos que se ha diseñado para la depuración debe incluir `.debug` en el nombre de archivo.

*Para facilitar la localización:*

Las secuencias de comandos de la referencia cultural neutra no deben incluir cualquier identificador de referencia cultural, el nombre del archivo. Para los scripts que contienen recursos localizados, el código de idioma ISO debe especificarse en el nombre de archivo. Por ejemplo, `es-CO` es el acrónimo español, Columbia.

En la tabla siguiente se resume las con ejemplos de las convenciones de nomenclatura de archivos:

| Filename | Significado |
| --- | --- |
| Script.js | Una secuencia de comandos de la referencia cultural neutra de versión de lanzamiento. |
| Script.debug.js | Una secuencia de comandos de la referencia cultural neutra de versión de depuración. |
| Script.en US.js | Un versión, inglés de Estados Unidos script de la versión. |
| Script.debug.es-CO.js | Un script de Columbia español, versión de depuración. |

## <a name="walkthrough-create-an-localized-embedded-script"></a>Tutorial: Crear un Script incrustado, localizado

*Tenga en cuenta: en este tutorial requiere el uso de Visual Studio 2008 como Visual Web Developer Express no incluye una plantilla de proyecto para los proyectos de biblioteca de clases.*

1. Cree un nuevo proyecto de sitio Web con extensiones de AJAX de ASP.NET integrado. Cree otro proyecto, un proyecto de biblioteca de clases, dentro de la solución denominado LocalizingResources.
2. Agregue un archivo Jscript denominado VerifyDeletion.js al proyecto LocalizingResources, así como los archivos de recursos .resx denominados DeletionResources.resx y DeletionResources.es.resx. El primero contendrá los recursos de la referencia cultural neutra; el segundo contendrá recursos de idioma español.
3. Agregue el código siguiente a VerifyDeletion.js:

[!code-javascript[Main](understanding-asp-net-ajax-localization/samples/sample1.js)]

Para quienes no conocen con la sintaxis de JavaScript Regex, texto de barras diagonales únicas (en el ejemplo anterior, /FILENAME/ es un ejemplo) indica un objeto RegExp. MSDN Library contiene una referencia de JavaScript amplia y recursos en objetos nativos de JavaScript pueden encontrarse en línea.

1. Agregue las siguientes cadenas de recursos a DeletionResources.resx: 

    **VerifyDelete**: ¿está seguro de que desea eliminar el nombre de archivo?

    **Eliminar**: nombre de archivo se ha eliminado.

1. Agregue las siguientes cadenas de recursos a DeletionResources.es.resx: 

    **¿VerifyDelete**: Est seguro que desee quitar del nombre de archivo?

    **Eliminar**: nombre de archivo se ha quitado.
2. Agregue las siguientes líneas de código al archivo AssemblyInfo:

[!code-csharp[Main](understanding-asp-net-ajax-localization/samples/sample2.cs)]

1. Agregue referencias a System.Web y System.Web.Extensions al proyecto LocalizingResources.
2. Agregue una referencia al proyecto LocalizingResources del proyecto de sitio Web.
3. En default.aspx, bajo el proyecto de sitio Web, actualice el control ScriptManager con el siguiente marcado adicional:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample3.aspx)]

1. En default.aspx, en cualquier parte de la página, incluya este marcado:

[!code-aspx[Main](understanding-asp-net-ajax-localization/samples/sample4.aspx)]

1. Presione F5. Si se le solicita, habilite la depuración. Cuando se carga la página, presione el botón Eliminar. Tenga en cuenta que deberá en inglés (a menos que el equipo se configura para preferir recursos de idioma de español de forma predeterminada) para la confirmación.
2. Cierre la ventana del explorador y vuelva a default.aspx. En el @Page directiva de encabezado, auto de reemplazo para Culture y UICulture con es-es. Presione F5 para iniciar la aplicación web en el Explorador de nuevo. Esta vez, tenga en cuenta que deberá eliminar el archivo en español:


[![](understanding-asp-net-ajax-localization/_static/image2.png)](understanding-asp-net-ajax-localization/_static/image1.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-localization/_static/image3.png))


[![](understanding-asp-net-ajax-localization/_static/image5.png)](understanding-asp-net-ajax-localization/_static/image4.png)

([Haga clic aquí para ver la imagen a tamaño completo](understanding-asp-net-ajax-localization/_static/image6.png))


Tenga en cuenta que hay diversas variaciones en este tutorial. Por ejemplo, las secuencias de comandos se pudieron registrar con el control ScriptManager mediante programación durante la carga de página.

## <a name="including-a-static-script-file-structure"></a>*Incluida una estructura de archivos de Script estáticos*

Al usar archivos de script estáticos para la implementación, perderá algunas de las ventajas del uso de la combinación de localización .NET inherente. Es principalmente visible que se pierda la automática de tipos generada a partir de incluidos los archivos de script de recursos; Por ejemplo, en el tutorial anterior, los recursos se expusieron por un tipo generado automáticamente llamado mensaje desde el control ScriptManager.

Sin embargo, hay algunas ventajas derivadas del uso de una estructura de archivos de script estáticos. Las actualizaciones pueden realizarse sin tener que volver a compilar y volver a implementar ensamblados satélite, y también es posible el uso de una estructura de archivos estáticos para invalidar el script incrustado para integrar una parte menor de funcionalidad que podrían no haberse incluida con un componente.

Microsoft recomienda evitar un problema de control de versión mediante la generación automática de los recursos de script durante la compilación del proyecto. Al mantener un código de script amplia base, puede ser cada vez más difícil para asegurarse de que los cambios en el código se reflejan en cada script localizados. Como alternativa, puede simplemente mantener una secuencia de comandos de lógica y varias secuencias de comandos de localización, combinar los archivos al compilar el proyecto.

Porque no hay recursos para incluir de forma declarativa, script estáticos deben ser archivos al que hace referencia mediante la adición `<asp:ScriptElement>` elementos como un elemento secundario de la `<Scripts>` etiqueta del control ScriptManager o agregando mediante programación `ScriptReference` objetos para el `Scripts` propiedad de la `ScriptManager` control en la página en tiempo de ejecución.

## <a name="the-scriptmanager-and-its-role-in-localization"></a>*El ScriptManager y su función en la localización*

El ScriptManager permite varios comportamientos automáticos en las aplicaciones localizadas:

- Busca automáticamente archivos de script según su configuración y las convenciones de nomenclatura; Por ejemplo, carga habilitado para depuración de secuencias de comandos en modo de depuración y carga adaptado scripts basados en la selección de interfaz de usuario del explorador.
- Permite la definición de referencias culturales, incluidas las referencias culturales personalizadas.
- Habilita la compresión de archivos de script a través de HTTP.
- Almacena en caché las secuencias de comandos para administrar de forma eficaz muchas solicitudes.
- Agrega una capa de direccionamiento indirecto a secuencias de comandos mediante la canalización de ellos a través de una dirección URL cifrada.

Referencias de script pueden agregarse al control ScriptManager mediante programación o marcado declarativo. Marcado declarativo es especialmente útil al trabajar con scripts incrustados en ensamblados que no sea el proyecto de sitio web, como el nombre de la secuencia de comandos no es probable que se cambiará tal y como se insertan las revisiones a través de.

## <a name="summary"></a>Resumen

Como las aplicaciones web crecen hasta llegar a una audiencia mayor, la necesidad de seguir teniendo acceso más amplia de las Comunidades y referencias culturales se convierte en core a un modelo de negocio; las aplicaciones web de comercio electrónico deben ser capaz de tratar con divisas, sistemas de administración de contenido deben estar presente puede no solo su contenido pero también sus sugerencias de navegación y campos de formulario en otros idiomas y las empresas necesitan saber que esta necesidad puede obtener acceso.

.NET Framework admite intrínsecamente un marco de trabajo de localización enriquecida, utilizando ensamblados satélite y archivos de recursos (.resx) XML para presentar de manera uniforme para buscar imágenes y cadenas de recursos. Las extensiones de AJAX de ASP.NET, incluido el marco de AJAX de Microsoft y la biblioteca de scripts de AJAX de Microsoft, proporcionan compatibilidad para este modelo de programación en código del lado cliente, lo que permite realizar búsquedas de cadena de recursos fácil. Ensamblados satélite admiten la inclusión automática de recursos de script (archivos .js real) a través de ScriptResource.axd siempre y cuando los nombres de archivo siguen un esquema de nomenclatura determinado. Con esta compatibilidad, las extensiones de AJAX de ASP.NET simplifican la localización de scripts y la globalización de las aplicaciones.

## <a name="bio"></a>*Bio*

Scott categoría ha estado trabajando con las tecnologías Web de Microsoft desde 1997 y es el director general de myKB.com ([www.myKB.com](http://www.myKB.com)) donde está especializado en la escritura de ASP.NET en función de las aplicaciones que se centra en las soluciones de Software de Base de conocimiento. Scott se puede contactar a través de correo electrónico en [ scott.cate@myKB.com ](mailto:scott.cate@myKB.com) o su blog en [ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [Anterior](understanding-asp-net-ajax-authentication-and-profile-application-services.md)
> [Siguiente](understanding-asp-net-ajax-web-services.md)
