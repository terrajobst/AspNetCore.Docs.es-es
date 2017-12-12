---
uid: whitepapers/what-is-new-in-aspnet-mvc
title: Novedades de ASP.NET MVC 2 | Documentos de Microsoft
author: rick-anderson
description: "Este documento describe las nuevas características y mejoras introducidas en ASP.NET MVC 2. Este documento también está disponible para su descarga."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/20/2010
ms.topic: article
ms.assetid: 69a8d6f8-4b10-4602-8822-2d6c05fc432b
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/what-is-new-in-aspnet-mvc
msc.type: content
ms.openlocfilehash: e7f92dd7a09d1986ad775203effcbce76fb0e6f4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-mvc-2"></a>Novedades de ASP.NET MVC 2
====================
> Este documento describe las nuevas características y mejoras introducidas en ASP.NET MVC 2. Este documento también está disponible para [descargar](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/WhatIsNewInMVC_2.pdf)


[Introducción](#_TOC1)   
[Actualizar un proyecto de MVC de ASP.NET 1.0 a ASP.NET MVC 2](#_TOC2)   
[Nuevas características](#_TOC3)   
[Aplicaciones auxiliares con plantilla](#_TOC3_1)   
[Áreas](#_TOC3_2)   
[Compatibilidad con controladores asincrónicos](#_TOC3_3)   
[Compatibilidad con DefaultValueAttribute en parámetros de método de acción](#_TOC3_4)   
[Admite el enlace de datos binarios con enlazadores de modelos](#_TOC3_5)   
[Clases de ModelMetadataProvider y ModelMetadata](#_TOC3_6)   
[Compatibilidad con los atributos de DataAnnotations](#_TOC3_7)   
[Proveedores de validadores de modelo](#_TOC3_8)   
[Validación del lado cliente](#_TOC3_9)   
[Nuevos fragmentos de código para Visual Studio 2010](#_TOC3_10)   
[Nuevo filtro de acción RequireHttpsAttribute](#_TOC3_11)   
[Reemplazar el verbo HTTP (método)](#_TOC3_12)   
[Nueva clase HiddenInputAttribute para aplicaciones auxiliares con plantilla](#_TOC3_13)   
[Método auxiliar Html.ValidationSummary puede mostrar errores de nivel de modelo](#_TOC3_14)   
[Plantillas T4 en generar código de Visual Studio que son específicos a la versión de destino de .NET Framework](#_TOC3_15)[mejoras de la API](#_TOC4)  
[Cambios importantes](#_TOC5)  
[Declinación de responsabilidades](#_TOC6)  

## <a id="_TOC1"></a>Introducción

ASP.NET MVC 2 se basa en ASP.NET MVC 1.0 y presenta un gran conjunto de mejoras y características que se centran en aumento de la productividad. Esta versión es compatible con ASP.NET MVC 1.0, por lo que todo el conocimiento, habilidades, código y extensiones para ASP.NET MVC 1.0 continuarán aplicándose.

Para obtener más información sobre ASP.NET MVC, visite los siguientes recursos:

- [Documentación de ASP.NET MVC en MSDN](https://go.microsoft.com/fwlink/?LinkId=159758)
- [El sitio Web de ASP.NET MVC](https://asp.net/mvc/)
- [Los foros de ASP.NET MVC](https://forums.asp.net/1146.aspx)

## <a id="_TOC2"></a>Actualizar un proyecto de MVC de ASP.NET 1.0 a ASP.NET MVC 2

ASP.NET MVC 2 puede instalarse paralelo con ASP.NET MVC 1.0 en el mismo servidor, que proporciona la flexibilidad de los desarrolladores de aplicaciones al elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2. Para obtener información sobre cómo actualizar, consulte el documento [actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185459).

## <a id="_TOC3"></a>Nuevas características

Esta sección describen las características que se han introducido en la versión de MVC 2.

### <a id="_TOC3_1"></a>Aplicaciones auxiliares con plantilla

Aplicaciones auxiliares con plantilla permiten asociados automáticamente elementos HTML para su edición y mostrar con tipos de datos. Por ejemplo, cuando se muestran datos del tipo System.DateTime en una vista, un elemento de interfaz de usuario de selector de fecha puede automáticamente representará. Esto es similar a cómo funcionan las plantillas de campo de datos dinámicos de ASP.NET. Para obtener más información, consulte [usar aplicaciones auxiliares con plantilla para mostrar datos](https://go.microsoft.com/fwlink/?LinkId=159062) en el sitio Web de MSDN.

### <a id="_TOC3_2"></a>Áreas

Las áreas permiten organizar un proyecto grande en varias secciones más pequeñas para administrar la complejidad de una aplicación Web grande. Normalmente, cada sección ("área") representa una sección independiente de un sitio Web grande y se usa para agrupar conjuntos de controladores y vistas relacionados. Para obtener más información, consulte [Tutorial: organizar una aplicación de ASP.NET MVC mediante áreas](https://go.microsoft.com/fwlink/?LinkId=158978) en el sitio Web de MSDN.

Para crear una nueva área, en el Explorador de soluciones, haga clic en el proyecto, haga clic en Agregar y, a continuación, haga clic en área. Esto muestra un cuadro de diálogo que le pide el nombre del área. Después de escribir el nombre del área, Visual Studio agrega una nueva área para el proyecto.

La siguiente ilustración muestra un ejemplo de diseño para un proyecto con dos áreas, administración y Blogs.

![](what-is-new-in-aspnet-mvc/_static/image1.png)

Cuando se crea un área, Visual Studio agrega una clase que deriva de AreaRegistration a cada área. Esta clase es necesaria para registrar el área y sus rutas, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample1.cs)]

La plantilla de proyecto predeterminada de ASP.NET MVC 2 incluye una llamada al método RegisterAllAreas en el código para el archivo Global.asax. Este método registra cada área en el proyecto busca todos los tipos que derivan de la clase AreaRegistration, creando una instancia del tipo y, a continuación, llamar al método de RegisterArea en la instancia. En el ejemplo siguiente se muestra cómo hacerlo.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample2.cs)]

Si no especifica el espacio de nombres en el método RegisterArea llamando el contexto. Método de Namespaces.Add, el espacio de nombres de la clase de registro se utiliza de forma predeterminada.

### <a id="_TOC3_3"></a>Compatibilidad con controladores asincrónicos

ASP.NET MVC 2 ahora permite a los controladores procesar las solicitudes de forma asincrónica. Esto puede producir mejoras de rendimiento al permitir que los servidores que llaman frecuentemente a operaciones de bloqueo (por ejemplo, las solicitudes de red) para llamar a homólogos antibloqueo en su lugar. Para obtener más información, consulte el [mediante un controlador asincrónico en ASP.NET MVC](https://msdn.microsoft.com/en-us/library/ee728598(v=VS.100).aspx) tema en MSDN.

### <a id="_TOC3_4"></a>Compatibilidad con DefaultValueAttribute en parámetros de método de acción

La clase de atributo System.ComponentModel.DefaultValueAttribute permite un valor predeterminado va a proporcionar para el parámetro del argumento a un método de acción. Por ejemplo, supongamos que se define la ruta predeterminada siguiente:

[!code-json[Main](what-is-new-in-aspnet-mvc/samples/sample3.json)]

También se supone que se define el método de acción y controlador siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample4.cs)]

Cualquiera de la siguiente solicitud de direcciones URL va a invocar el método de acción de vista que se define en el ejemplo anterior.

- / Artículo/vista/123
- / Artículo/vista/123? página = 1 (eficazmente es la misma que la solicitud anterior)
- / Artículo/vista/123? página = 2

Sin el atributo DefaultValueAttribute, la primera dirección URL de la lista anterior no funcionará, porque el argumento de página es un tipo de valor que no aceptan valores NULL cuyo valor no se ha proporcionado.

Si el código está escrito en Visual Basic 2010 o Visual C# 2010, puede usar parámetros opcionales en lugar del atributo DefaultValueAttribute, tal como se muestra en el ejemplo siguiente:

[!code-vb[Main](what-is-new-in-aspnet-mvc/samples/sample5.vb)]

### <a id="_TOC3_5"></a>Admite el enlace de datos binarios con enlazadores de modelos

Hay dos nuevas sobrecargas de la aplicación auxiliar Html.Hidden que codificar valores binarios como cadenas codificadas en base 64:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample6.cs)]

Un uso típico consiste en incrustar una marca de tiempo de un objeto en la vista. Por ejemplo, la aplicación puede incluir el siguiente objeto de producto:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample7.cs)]

Un formulario de edición puede representar la propiedad de marca de tiempo en el formulario tal y como se muestra en el ejemplo siguiente:

[!code-aspx[Main](what-is-new-in-aspnet-mvc/samples/sample8.aspx)]

Este marcado representa un elemento input oculto con el valor de marca de tiempo como una cadena codificado en base 64 que es similar al siguiente:

[!code-html[Main](what-is-new-in-aspnet-mvc/samples/sample9.html)]

Este formulario podría registrarse en un método de acción que tiene un argumento de tipo de producto, tal como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample10.cs)]

En el método de acción, la propiedad de marca de tiempo se rellena correctamente porque la cadena codificado en base 64 expuesta se convierte en una matriz de bytes.

### <a id="_TOC3_6"></a>Clases de ModelMetadataProvider y ModelMetadata

La clase ModelMetadataProvider proporciona una abstracción para obtener metadatos del modelo dentro de una vista. MVC 2 incluye un proveedor predeterminado que pone a disposición los metadatos que se exponen mediante los atributos en el espacio de nombres System.ComponentModel.DataAnnotations. Es posible crear proveedores de metadatos que proporcionan los metadatos de otros almacenes de datos, como bases de datos o archivos XML.

La clase ViewDataDictionary expone un objeto ModelMetadata que contiene los metadatos que se extraen del modelo de la clase ModelMetadataProvider. Esto permite que las aplicaciones auxiliares con plantilla utilizar estos metadatos y ajustar su resultado según sea necesario.

Para obtener más información, consulte la documentación para el [ModelMetadata](https://msdn.microsoft.com/en-us/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) y [ModelMetadataProvider](https://msdn.microsoft.com/en-us/library/system.web.mvc.modelmetadataprovider(VS.100).aspx) clases.

### <a id="_TOC3_7"></a>Compatibilidad con los atributos de DataAnnotations

ASP.NET MVC 2 admite el uso de los atributos de validación RangeAttribute, RequiredAttribute, StringLengthAttribute y RegexAttribute (definidos en el espacio de nombres System.ComponentModel.DataAnnotations) cuando se enlaza a un modelo con el fin de proporcionar la entrada validación.

Para obtener más información, consulte [Cómo: validar datos de modelo mediante atributos de DataAnnotations](https://go.microsoft.com/fwlink/?LinkId=159063) en el sitio Web de MSDN. Está disponible para su descarga en un proyecto de ejemplo que ilustra el uso de estos atributos [https://go.microsoft.com/fwlink/?LinkId=157753](https://go.microsoft.com/fwlink/?LinkId=157753).

### <a id="_TOC3_8"></a>Proveedores de validadores de modelo

La clase de proveedor de validación del modelo representa una abstracción que proporciona lógica de validación para el modelo. ASP.NET MVC incluye un proveedor de valor predeterminado basado en atributos de validación que se incluyen en el espacio de nombres System.ComponentModel.DataAnnotations. También puede crear sus propios proveedores de validación que definen las reglas de validación personalizada y asignaciones personalizadas de reglas de validación para el modelo. Para obtener más información, consulte la documentación para el [ModelValidatorProvider](https://msdn.microsoft.com/en-us/library/system.web.mvc.ModelValidatorProvider(VS.100).aspx) clase.

### <a id="_TOC3_9"></a>Validación del lado cliente

La clase de proveedor de validadores de modelo expone metadatos de validación en el explorador en el formulario de datos JSON serializados que se pueden usar en una biblioteca de validación del lado cliente. ASP.NET MVC 2 incluye una biblioteca de validación de cliente y el adaptador compatible con los atributos de validación del espacio de nombres de DataAnnotations se ha indicado anteriormente. La clase de proveedor también permite usar otras bibliotecas de validación del cliente mediante la escritura de un adaptador que procesa los datos JSON y llama a la biblioteca alternativa.

### <a id="_TOC3_10"></a>Nuevos fragmentos de código para Visual Studio 2010

Un conjunto de fragmentos de código HTML para ASP.NET MVC 2 se instala con Visual Studio 2010. Para ver una lista de estos fragmentos de código, en el menú Herramientas, seleccione Administrador de fragmentos de código. Para el idioma, seleccione HTML y, para la ubicación, seleccione ASP.NET MVC 2. Para obtener más información acerca de cómo usar los fragmentos de código, vea la documentación de Visual Studio.

### <a id="_TOC3_11"></a>Nuevo filtro de acción RequireHttpsAttribute

ASP.NET MVC 2 incluye una nueva clase RequireHttpsAttribute que se pueden aplicar a los controladores y métodos de acción. De forma predeterminada, el filtro redirige una solicitud no SSL HTTP en el equivalente de habilitado SSL (HTTPS).

### <a id="_TOC3_12"></a>Reemplazar el verbo HTTP (método)

Al compilar un sitio Web usando el estilo de arquitectura REST, verbos HTTP se usan para determinar qué acción va a realizar para un recurso. REST requiere aplicaciones compatibles con toda la gama de verbos HTTP comunes, incluidos GET, PUT, POST y eliminar.

ASP.NET MVC 2 incluye nuevos atributos que se pueden aplicar a los métodos de acción y esa sintaxis compacta de característica. Estos atributos permiten que ASP.NET MVC seleccionar un método de acción basándose en el verbo HTTP. En el ejemplo siguiente, una solicitud POST llamará al primer método de acción y una solicitud PUT llamará al segundo método de acción.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample11.cs)]

En versiones anteriores de ASP.NET MVC, estos métodos de acción necesitan sintaxis más detallada, como se muestra en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample12.cs)]

Dado que los exploradores admiten los verbos GET y HTTP POST, no es posible publicar en una acción que requiere un verbo diferente. Por lo tanto no es posible admitir todas las solicitudes REST de forma nativa.

Sin embargo, para admitir RESTful solicitudes durante las operaciones POST, ASP.NET MVC 2 presenta un nuevo método de aplicación auxiliar de HTML HttpMethodOverride. Este método representa un elemento input oculto que hace que el formulario emular eficazmente cualquier método HTTP. Por ejemplo, mediante el método de aplicación auxiliar de HTML HttpMethodOverride, puede hacer que un envío de formulario aparezca sea una solicitud PUT o DELETE. El comportamiento de HttpMethodOverride afecta a los siguientes atributos:

- HttpPostAttribute
- HttpPutAttribute
- HttpGetAttribute
- HttpDeleteAttribute
- AcceptVerbsAttribute

El elemento de entrada oculto tiene su nombre X-HTTP-Method-Override y su valor se establece en el verbo HTTP para emular. También se puede especificar el valor de invalidación en un encabezado HTTP o en un valor de cadena de consulta como un par de nombre/valor.

La invalidación solo puede usarse cuando la solicitud real es una solicitud POST. Se omitirá el valor de reemplazo para las solicitudes que utilizan cualquier otro verbo HTTP.

### <a id="_TOC3_13"></a>Nueva clase HiddenInputAttribute para aplicaciones auxiliares con plantilla

Puede aplicar el nuevo atributo HiddenInputAttribute a una propiedad de modelo para indicar si se debe representar un elemento input oculto cuando se muestra el modelo en una plantilla de editor. (El atributo establece un valor de UIHint implícito de HiddenInput). La propiedad del atributo valor de visualización le permite especificar si el valor se muestra en el editor y modos de presentación. Cuando el valor de visualización se establece en false, no se muestra nada, ni siquiera el marcado HTML que rodea normalmente a un campo. El valor predeterminado para el valor de visualización es true.

Puede usar el atributo HiddenInputAttribute en los escenarios siguientes:

- Cuando una vista permite a los usuarios editar el identificador de un objeto y es necesario mostrar el valor, así como para proporcionar un elemento input oculto que contiene el identificador anterior para que se puede pasar hacia el controlador.
- Cuando una vista permite a los usuarios edición una propiedad binaria que nunca se debe mostrar, como una propiedad de marca de tiempo. En ese caso, no se muestran el valor y el marcado HTML circundante (por ejemplo, la etiqueta y el valor).

En el ejemplo siguiente se muestra cómo utilizar la clase HiddenInputAttribute.

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample13.cs)]

Cuando el atributo se establece en true (o se especifica ningún parámetro), ocurre lo siguiente:

- En las plantillas de presentación, se representa una etiqueta y el valor se muestra al usuario.
- En las plantillas de editor, se representa una etiqueta y el valor se representa en un elemento input oculto.

Cuando el atributo se establece en false, ocurre lo siguiente:

- En las plantillas de presentación, nada se representa para ese campo.
- En las plantillas de editor, no se representa una etiqueta y el valor se representa en un elemento input oculto.

### <a id="_TOC3_14"></a>Método auxiliar Html.ValidationSummary puede mostrar errores de nivel de modelo

En lugar de mostrar siempre todos los errores de validación, el método de aplicación auxiliar de Html.ValidationSummary tiene una nueva opción para mostrar solo los errores de nivel de modelo. Esto permite que los errores de nivel de modelo que se mostrará en el resumen de validación y errores específicos de los campos que se mostrará junto a cada campo.

### <a id="_TOC3_15"></a>Plantillas T4 en generar código de Visual Studio que son específicos a la versión de destino de .NET Framework

Una nueva propiedad está disponible para archivos T4 desde el host de ASP.NET MVC T4 que especifica la versión de .NET Framework que se usa la aplicación. Esto permite plantillas T4 generar código y marcado específico de una versión de .NET Framework. En Visual Studio 2008, el valor siempre es .NET 3.5. En Visual Studio 2010, el valor es .NET 3.5 o .NET 4.

## <a id="_TOC4"></a>Mejoras de la API

Esta sección describe los cambios en los tipos de MVC de ASP.NET existentes y los miembros.

- Agrega un método virtual protegido de CreateActionInvoker en la clase de controlador. Este método es invocado por la propiedad ActionInvoker del controlador y permite la creación de instancias diferida del invocador si ya no se ha establecido ningún invocador.
- Agrega un método virtual protegido de HandleUnauthorizedRequest en la clase de AuthorizeAttribute. Esto permite que los filtros que se derivan de AuthorizeAttribute para controlar el comportamiento cuando se produce un error de autorización.
- Agrega un método Add (cadena, objeto clave-valor) en la clase ValueProviderDictionary. Esto le permite usar la sintaxis de inicializador de diccionario para ValueProviderDictionary, como en el ejemplo siguiente:

[!code-csharp[Main](what-is-new-in-aspnet-mvc/samples/sample14.cs)]

- Agrega una operación get\_método en la clase Sys.Mvc.AjaxContext del objeto. Se trata de un método de JavaScript que es similar a get\_método de datos, pero si el tipo de contenido de la respuesta es application/json, obtener\_object devuelve el objeto JSON.
- Agrega una propiedad de ActionDescriptor en la clase AuthorizationContext.
- Agrega un símbolo (token) de UrlParameter.Optional que puede usarse para solucionar problemas cuando se enlaza a un modelo que contiene una propiedad ID cuando la propiedad está presente en un formulario post. Para obtener información más detallada, vea la entrada [parámetros opcionales de dirección URL de ASP.NET MVC 2](http://haacked.com/archive/2010/02/12/asp-net-mvc-2-optional-url-parameters.aspx) en el blog de Phil Haack.

## <a id="_TOC5"></a>Cambios importantes

Los cambios siguientes pueden producir errores en las aplicaciones existentes de ASP.NET MVC 1.0.

#### <a name="change-in-property-validation-behavior-for-classes-that-implement-idataerrorinfo"></a>Cambio en el comportamiento de validación de propiedad para las clases que implementan IDataErrorInfo

Para los objetos de modelo que use IDataErrorInfo para realizar la validación, se validan todas las propiedades, independientemente de si se ha establecido un nuevo valor. En ASP.NET MVC 1.0, se validaron solo las propiedades que establecido nuevos valores. En ASP.NET MVC 2, se llama a la propiedad de Error de IDataErrorInfo solo si todos los validadores de propiedad se realizaron correctamente.

#### <a name="iis-script-mapping-script-is-no-longer-available-in-the-installer"></a>Secuencia de comandos de asignación de script IIS ya no está disponible en el programa de instalación

La secuencia de comandos de asignación de script IIS es un script de línea de comandos que se usa para configurar asignaciones de secuencias de comandos para IIS 6 y 7 de IIS en modo clásico. La secuencia de comandos de asignación de script no es necesario si utiliza el servidor de desarrollo de Visual Studio o si usa IIS 7 en el modo integrado. Las secuencias de comandos están disponibles como descarga independiente no admitida en el [WebStack ASP.NET](https://github.com/aspnet/AspNetWebStack).

#### <a name="the-htmlsubstitute-helper-method-in-mvc-futures-is-no-longer-available"></a>El método de aplicación auxiliar de Html.Substitute en MVC futuros ya no está disponible

Debido a cambios en el comportamiento de representación de motores de vista MVC, el método de aplicación auxiliar de Html.Substitute no funciona y se ha quitado.

#### <a name="the-ivalueprovider-interface-replaces-all-uses-of-idictionary"></a>La interfaz IValueProvider reemplaza todos los usos de IDictionary

Cada argumento de método o propiedad que acepta IDictionary en MVC 1.0 ahora acepta IValueProvider. Este cambio sólo afecta a aplicaciones que incluyen proveedores de valores personalizados o enlazadores de modelos personalizados. A continuación se indican algunos ejemplos de propiedades y métodos que se ven afectados por este cambio:

- La propiedad ValueProvider de las clases ControllerBase y ModelBindingContext.
- Los métodos TryUpdateModel de la clase de controlador.

#### <a name="new-css-classes-were-added-in-the-sitecss-file"></a>Se han agregado nuevas clases CSS en el archivo Site.css

El archivo Site.css en las plantillas de proyecto de MVC de ASP.NET se ha actualizado para incluir nuevos estilos utilizados por la funcionalidad de validación y por las aplicaciones auxiliares con plantilla.

#### <a name="helpers-now-return-an-mvchtmlstring-object"></a>Aplicaciones auxiliares devuelven ahora un objeto MvcHtmlString

Para aprovechar las ventajas de la nueva sintaxis de expresión de codificación HTML en ASP.NET 4, el tipo de valor devuelto para las aplicaciones auxiliares HTML es ahora MvcHtmlString en lugar de una cadena. Si usa ASP.NET MVC 2 y las funciones auxiliares de nuevo en ASP.NET 3.5, no podrá aprovechar las ventajas de la sintaxis de codificación HTML; la nueva sintaxis está disponible sólo cuando se ejecuta ASP.NET MVC 2 en ASP.NET 4.

#### <a name="jsonresult-now-responds-only-to-http-post-requests"></a>JsonResult ahora sólo responde a las solicitudes HTTP POST

Para mitigar JSON cambiando la configuración de los ataques que tienen la posibilidad de divulgación de información, de forma predeterminada, la clase JsonResult ahora sólo responde a las solicitudes HTTP POST. OBTENER AJAX llamadas a métodos de acción que devuelven un objeto JsonResult deben cambiarse para utilizar POST en su lugar. Si es necesario, puede invalidar este comportamiento estableciendo la nueva propiedad JsonRequestBehavior de JsonResult. Para obtener más información acerca de la vulnerabilidad de seguridad posible, consulte la entrada de blog [cambiando la configuración de JSON](http://haacked.com/archive/2009/06/25/json-hijacking.aspx) en el blog de Phil Haack.

#### <a name="model-and-modeltype-property-setters-on-modelbindingcontext-are-obsolete"></a>Modelo y establecedores de propiedad ModelType en ModelBindingContext están obsoletos

Se agregó una nueva propiedad ModelMetadata configurable para la clase ModelBindingContext. La nueva propiedad encapsula el modelo y las propiedades de ModelType. Aunque las propiedades de modelo y ModelType están obsoletas, por motivos de compatibilidad los captadores de propiedades seguirán funcionan; delegar a la propiedad ModelMetadata para recuperar el valor.

#### <a name="changes-to-the-defaultcontrollerfactory-class-break-custom-controller-factories-that-derive-from-it"></a>Cambios en la clase DefaultControllerFactory interrumpir generadores de controladores personalizados que derivan de él

La clase DefaultControllerFactory se corrigió mediante la eliminación de la propiedad RequestContext. En lugar de esta propiedad, la instancia de contexto de solicitud se pasa a los métodos virtuales protegidos GetControllerInstance y GetControllerType. Este cambio afecta a los generadores de controladores personalizados que derivan de DefaultControllerFactory.

Generadores de controladores personalizados a menudo se usan para proporcionar aplicaciones de inyección de dependencia para ASP.NET MVC. Para actualizar los generadores de controlador personalizado para admitir ASP.NET MVC 2, cambiar la firma del método o firmas para que coincida con las firmas nuevo y usar el parámetro de contexto de solicitud en lugar de la propiedad.

#### <a name="area-is-a-now-a-reserved-route-value-key"></a>"Área" es ahora una clave de ruta-valor reservado

La cadena "área" en los valores de ruta ahora tiene un significado especial en ASP.NET MVC, de la misma manera que realizan la "acción" y "controller". Una implicación es que si se proporcionan métodos auxiliares HTML con un diccionario de valor de la ruta que contiene "área", las aplicaciones auxiliares ya no se anexará "área" en la cadena de consulta.

Si está utilizando la característica de áreas, asegúrese de no usar {área} como parte de la dirección URL de la ruta.


## <a id="_TOC6"></a>Declinación de responsabilidades

Este documento es preliminar y puede cambiar considerablemente antes de la versión comercial final del software que se describe en él.

La información que incluye este documento representa el punto de vista actual de Microsoft Corporation sobre las cuestiones descritas en la fecha de la publicación. Debido a que Microsoft debe responder a las condiciones de mercado cambiantes, no se debe interpretar como un compromiso por parte de Microsoft y Microsoft no puede garantizar la precisión de la información presentada después de la fecha de publicación.

Estas notas del producto solo tienen fines informativos. MICROSOFT NO OTORGA NINGUNA GARANTÍA, EXPRESA, IMPLÍCITA O ESTUTARIAS, EN LA INFORMACIÓN DE ESTE DOCUMENTO.

Es responsabilidad del usuario el cumplimiento de toda la legislación aplicable en materia de copyright. Sin limitación de los derechos protegidos por copyright, ninguna parte del presente documento podrá ser reproducida, almacenada o introducida en un sistema de recuperación, o bien transmitida en ninguna forma o medio (electrónico, mecánico, mediante fotocopia o grabación, etc.), ni con ningún propósito, sin la autorización expresa y por escrito de Microsoft Corporation.

Microsoft puede ser titular de patentes, solicitudes de patentes, marcas, derechos de autor u otros derechos de propiedad intelectual sobre los contenidos de este documento. Este documento no otorga ninguna licencia sobre estas patentes, marcas, derechos de autor u otros derechos de propiedad intelectual, a menos que se indique expresamente en un contrato escrito de licencia de Microsoft.

A menos que se indique lo contrario, los nombres de las compañías, organizaciones, productos, dominios, direcciones de correo electrónico, logotipos, personas, lugares y hechos mencionados son ficticios. No se pretende indicar, ni debe deducirse ninguna asociación con ninguna compañía, organización, producto, dominio, dirección de correo electrónico, logotipo, persona, lugar o hecho reales.

© 2010 Microsoft Corporation. Todos los derechos reservados.

Microsoft y Windows son marcas registradas o marcas comerciales de Microsoft Corporation en Estados Unidos y en otros países.

Los nombres de las compañías y productos reales mencionados en el presente documento pueden ser marcas comerciales de sus respectivos propietarios.
