---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
title: "Validación con los validadores de anotación de datos (VB) | Documentos de Microsoft"
author: microsoft
description: "Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación de ASP.NET MVC. Obtenga información acerca de cómo usar los diferentes tipos de validador..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 0d23ff2b-f2ec-434a-be3b-1180beeccba3
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-vb
msc.type: authoredcontent
ms.openlocfilehash: 227c1acb5e478047c4e5cdc7dbddedd703e91292
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="validation-with-the-data-annotation-validators-vb"></a>Validación con los validadores de anotación de datos (VB)
====================
por [Microsoft](https://github.com/microsoft)

> Aproveche el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación de ASP.NET MVC. Obtenga información acerca de cómo usar los diferentes tipos de atributos de validación y trabajar con ellos en Microsoft Entity Framework.


En este tutorial, aprenderá a usar los validadores de anotación de datos para realizar la validación en una aplicación ASP.NET MVC. La ventaja de utilizar los validadores de anotación de datos es que le permiten realizar la validación simplemente mediante la adición de uno o varios atributos, como los necesarios o atributo StringLength: para una propiedad de clase.

Para poder usar los validadores de anotación de datos, debe descargar el enlazador de modelos de anotaciones de datos. Puede descargar el ejemplo de enlazador de modelo de anotaciones de datos desde el sitio Web de CodePlex haciendo clic en [aquí](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471).


Es importante comprender que el enlazador de modelos de anotaciones de datos no es una parte oficial de Microsoft ASP.NET MVC framework. Aunque el enlazador de modelos de anotaciones de datos creado por el equipo de Microsoft ASP.NET MVC, Microsoft no ofrece soporte técnico oficial para el enlazador de modelos de anotaciones de datos se describe y usado en este tutorial.


## <a name="using-the-data-annotation-model-binder"></a>Usando el enlazador de modelos de anotación de datos

Para utilizar el enlazador de modelos de anotaciones de datos en una aplicación ASP.NET MVC, primero debe agregar una referencia al ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y el ensamblado System.ComponentModel.DataAnnotations.dll. Seleccione la opción de menú **proyecto, agregar referencia**. A continuación, haga clic en el **examinar** pestaña y vaya a la ubicación donde descargó (y se han descomprimido) en el ejemplo de enlazador de modelos de anotaciones de datos (vea **figura 1**).

[![](validation-with-the-data-annotation-validators-vb/_static/image2.png)](validation-with-the-data-annotation-validators-vb/_static/image1.png)

**Figura 1**: agregar una referencia al enlazador de modelos de anotaciones de datos ([haga clic aquí para ver la imagen a tamaño completo](validation-with-the-data-annotation-validators-vb/_static/image3.png))

Seleccione el ensamblado Microsoft.Web.Mvc.DataAnnotations.dll y el ensamblado System.ComponentModel.DataAnnotations.dll y haga clic en el **Aceptar** botón.


No se puede usar el ensamblado System.ComponentModel.DataAnnotations.dll incluido con Service Pack 1 de .NET Framework con el enlazador de modelos de anotaciones de datos. Debe usar la versión del ensamblado System.ComponentModel.DataAnnotations.dll incluido con la descarga de ejemplo de enlazador de modelo de anotaciones de datos.


Por último, debe registrar el enlazador de modelos DataAnnotations en el archivo Global.asax. Agregue la siguiente línea de código a la aplicación\_Start() controlador de eventos para que la aplicación\_método Start() tiene el siguiente aspecto:

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample1.vb)]

Esta línea de código registra el DataAnnotationsModelBinder como el enlazador de modelos predeterminado para toda la aplicación ASP.NET MVC.

## <a name="using-the-data-annotation-validator-attributes"></a>Mediante los atributos de validador de anotación de datos

Cuando se usa el enlazador de modelos de anotaciones de datos, utilice atributos de validación para realizar la validación. El espacio de nombres System.ComponentModel.DataAnnotations incluye los siguientes atributos de validación:

- Intervalo: le permite validar si el valor de una propiedad que se sitúa entre un intervalo de valores especificado.
- ReqularExpression – le permite validar si el valor de una propiedad coincide con un patrón de expresión regular especificada.
- Necesario: le permite marcar una propiedad según sea necesario.
- StringLength: le permite especificar una longitud máxima para una propiedad de cadena.
- Validación: la clase base para todos los atributos de validación.

> [!NOTE] 
> 
> Si no se satisfacen sus necesidades de validación por cualquiera de los validadores estándares siempre tiene la opción de crear un atributo de validador personalizado mediante la adquisición de un nuevo atributo de validador del atributo de validación base.


La clase de producto en **listado 1** muestra cómo utilizar estos atributos de validación. Las propiedades de nombre, descripción y UnitPrice se marcan según sea necesario. La propiedad Name debe tener una longitud de cadena que tenga menos de 10 caracteres. Por último, la propiedad UnitPrice debe coincidir con un patrón de expresión regular que representa una cantidad de divisa.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample2.vb)]

**Lista 1**: Models\Product.vb

La clase de producto muestra cómo utilizar un atributo adicional: el atributo DisplayName. El atributo DisplayName permite modificar el nombre de la propiedad cuando la propiedad se muestra en un mensaje de error. En lugar de mostrar el mensaje de error "el campo de UnitPrice es obligatorio" puede mostrar el mensaje de error "el campo de precio es obligatorio".

> [!NOTE] 
> 
> Si desea personalizar completamente el mensaje de error mostrado por un validador puede asignar un mensaje de error personalizado a propiedad de ErrorMessage del elemento de validación similar al siguiente:`<Required(ErrorMessage:="This field needs a value!")>`


Puede utilizar la clase de producto en **listado 1** con la acción del controlador Create() en **listado 2**. Esta acción de controlador vuelve a mostrar la vista de creación cuando el estado del modelo contiene los errores.

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample3.vb)]

**La lista 2**: Controllers\ProductController.vb

Por último, puede crear la vista de **listado 3** haciendo clic en la acción Create() y seleccionar la opción de menú **agregar vista**. Crear una vista fuertemente tipada con la clase de producto como la clase del modelo. Seleccione **crear** en la lista de contenido de lista desplegable de vista (vea **figura 2**).

[![](validation-with-the-data-annotation-validators-vb/_static/image5.png)](validation-with-the-data-annotation-validators-vb/_static/image4.png)

**Figura 2**: agregar la vista de creación

[!code-aspx[Main](validation-with-the-data-annotation-validators-vb/samples/sample4.aspx)]

**El listado 3**: Views\Product\Create.aspx

> [!NOTE] 
> 
> Quite el campo de Id. del formulario de creación generado por la **agregar vista** opción de menú. Dado que el campo Id corresponde a una columna de identidad, no desea permitir que los usuarios escriban un valor para este campo.


Si se envía el formulario de creación de un producto y no especifica valores para los campos obligatorios, mensajes de error de validación en **figura 3** se muestran.

[![](validation-with-the-data-annotation-validators-vb/_static/image7.png)](validation-with-the-data-annotation-validators-vb/_static/image6.png)

**Figura 3**: faltan campos obligatorios

Si escribe una cantidad de moneda no válido y, a continuación, el mensaje de error en **figura 4** se muestra.

[![](validation-with-the-data-annotation-validators-vb/_static/image9.png)](validation-with-the-data-annotation-validators-vb/_static/image8.png)

**Figura 4**: cantidad de moneda no válido

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Uso de validadores de anotación de datos con Entity Framework

Si usas Microsoft Entity Framework para generar las clases de modelo de datos no se puede aplicar los atributos de validación directamente a las clases. Dado que el Diseñador de Entity Framework genera las clases del modelo, los cambios que realice en las clases del modelo se sobrescribirá la próxima vez que realice los cambios en el diseñador.

Si desea usar los controles de validación con las clases generadas por Entity Framework, a continuación, debe crear clases de datos de metadatos. Los controles de validación se aplican a la clase de datos de metadatos en lugar de aplicar los controles de validación a la clase real.

Por ejemplo, imagine que ha creado una clase de película mediante Entity Framework (consulte **figura 5**). Además, imagine que desea que el título de la película y el Director propiedades necesarias de propiedades. En ese caso, puede crear la clase parcial y la clase de datos de metadatos en **listado 4**.

[![](validation-with-the-data-annotation-validators-vb/_static/image11.png)](validation-with-the-data-annotation-validators-vb/_static/image10.png)

**Figura 5**: clase de película generada por Entity Framework

[!code-vb[Main](validation-with-the-data-annotation-validators-vb/samples/sample5.vb)]

**Listado 4**: Models\Movie.vb

El archivo en **listado 4** contiene dos clases denominadas película y MovieMetaData. La clase de película es una clase parcial. Se corresponde con la clase parcial generada por Entity Framework que se encuentra en el archivo DataModel.Designer.vb.

Actualmente, .NET framework no admite propiedades parciales. Por lo tanto, no hay ninguna manera de aplicar los atributos de validación a las propiedades de la clase de película definida en el archivo DataModel.Designer.vb aplicando los atributos de validación a las propiedades de la clase de película definida en el archivo **listado 4**.

Tenga en cuenta que la clase parcial de la película se decora con un atributo MetadataType que apunta a la clase MovieMetaData. La clase MovieMetaData contiene propiedades del servidor proxy para las propiedades de la clase de la película.

Los atributos de validación se aplican a las propiedades de la clase MovieMetaData. Las propiedades de título, el Director y DateReleased se marcan como propiedades necesarias. La propiedad Director debe asignarse una cadena que contenga menos de 5 caracteres. Por último, se aplica el atributo DisplayName para la propiedad DateReleased para mostrar un mensaje de error que "el campo de fecha emitido no es necesario." en lugar del error "el campo DateReleased es obligatorio".

> [!NOTE] 
> 
> Tenga en cuenta que las propiedades de proxy en la clase MovieMetaData no es necesario representar los mismos tipos que las propiedades correspondientes de la clase de la película. Por ejemplo, la propiedad Director es una propiedad de cadena en la clase de película y una propiedad de objeto de la clase MovieMetaData.


La página de **figura 6** muestra los mensajes de error devueltos al especificar valores no válidos para las propiedades de la película.

[![](validation-with-the-data-annotation-validators-vb/_static/image13.png)](validation-with-the-data-annotation-validators-vb/_static/image12.png)

**Figura 6**: usar validadores con Entity Framework ([haga clic aquí para ver la imagen a tamaño completo](validation-with-the-data-annotation-validators-vb/_static/image14.png))

## <a name="summary"></a>Resumen

En este tutorial, aprendió a fin de aprovechar el enlazador de modelos de anotación de datos para realizar la validación dentro de una aplicación de ASP.NET MVC. Aprendió a utilizar los distintos tipos de atributos de validación como necesaria y StringLength atributos. También aprendió a utilizar estos atributos cuando se trabaja con Microsoft Entity Framework.

>[!div class="step-by-step"]
[Anterior](validating-with-a-service-layer-vb.md)
