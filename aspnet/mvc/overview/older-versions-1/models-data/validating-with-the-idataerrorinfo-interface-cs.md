---
uid: mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
title: Validar los datos con la interfaz IDataErrorInfo (C#) | Documentos de Microsoft
author: StephenWalther
description: "Stephen Walther muestra cómo mostrar mensajes de error de validación personalizada implementando la interfaz IDataErrorInfo en una clase de modelo."
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 4733b9f1-9999-48fb-8b73-6038fbcc5ecb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validating-with-the-idataerrorinfo-interface-cs
msc.type: authoredcontent
ms.openlocfilehash: c04088c576481e4a91676d7e6962c03b56e7a8a4
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="validating-with-the-idataerrorinfo-interface-c"></a>Validar los datos con la interfaz IDataErrorInfo (C#)
====================
por [Stephen Walther](https://github.com/StephenWalther)

> Stephen Walther muestra cómo mostrar mensajes de error de validación personalizada implementando la interfaz IDataErrorInfo en una clase de modelo.


El objetivo de este tutorial es explicar un enfoque para realizar la validación en una aplicación ASP.NET MVC. Obtenga información acerca de cómo impedir que alguien enviando un formulario HTML sin proporcionar valores para los campos obligatorios. En este tutorial, aprenderá a realizar la validación mediante la interfaz IErrorDataInfo.

## <a name="assumptions"></a>Suposiciones

En este tutorial, usará la base de datos de MoviesDB y la tabla de base de datos de películas. Esta tabla tiene las columnas siguientes:

<a id="0.5_table01"></a>


| **Nombre de columna** | **Tipo de datos** | **Permitir valores null** |
| --- | --- | --- |
| Id. | Valor int. | False |
| Título | nvarchar (100) | False |
| Director de | nvarchar (100) | False |
| DateReleased | DateTime | False |


En este tutorial, utilizar Microsoft Entity Framework para generar clases del modelo de mi base de datos. La clase de película generada por Entity Framework se muestra en la figura 1.


[![La entidad de película](validating-with-the-idataerrorinfo-interface-cs/_static/image1.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image1.png)

**Figura 01**: entidad de la película ([haga clic aquí para ver la imagen a tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image2.png))


> [!NOTE] 
> 
> Para obtener información adicional acerca del uso de Entity Framework para generar las clases de modelo de base de datos, vea que el tutorial titulada Crear clases de modelo con Entity Framework.


## <a name="the-controller-class"></a>La clase de controlador

Se usa el controlador Home para películas de lista y crear películas nuevas. El código de esta clase se encuentra en la lista 1.

**Lista 1 - controllers\homecontroller**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample1.cs)]

La clase de controlador de inicio en el listado 1 contiene dos acciones Create(). La primera acción muestra el formulario HTML para crear una película nuevo. La segunda acción Create() realiza la inserción real de la película nuevo a la base de datos. La segunda acción Create() se invoca cuando se envía el formulario mostrado por la primera acción Create() al servidor.

Tenga en cuenta que la segunda acción Create() contiene las siguientes líneas de código:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample2.cs)]

La propiedad IsValid devuelve false cuando se produce un error de validación. En ese caso, se vuelve a mostrar en la vista de creación que contiene el formulario HTML para crear una película.

## <a name="creating-a-partial-class"></a>Crear una clase parcial

La clase de la película se genera por Entity Framework. Puede ver el código de la clase de película si expandir el archivo MoviesDBModel.edmx en la ventana Explorador de soluciones y abra el archivo MoviesDBModel.Designer.cs en el Editor de código (consulte la figura 2).


[![El código de la entidad de película](validating-with-the-idataerrorinfo-interface-cs/_static/image2.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image3.png)

**Figura 02**: el código de la entidad de la película ([haga clic aquí para ver la imagen a tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image4.png))


La clase de película es una clase parcial. Esto significa que podemos agregar otra clase parcial con el mismo nombre para ampliar la funcionalidad de la clase de la película. Vamos a agregar la lógica de validación a la nueva clase parcial.

Agregue la clase en el listado 2 a la carpeta de modelos.

**La lista 2 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample3.cs)]

Tenga en cuenta que la clase en el listado 2 incluye la *parcial* modificador. Los métodos o propiedades que se agregan a esta clase se convierten en parte de la clase de película generada por Entity Framework.

## <a name="adding-onchanging-and-onchanged-partial-methods"></a>Agregar OnChanging y métodos parciales OnChanged

Cuando Entity Framework genera una clase de entidad, Entity Framework agrega automáticamente los métodos parciales a la clase. Entity Framework genera OnChanging y OnChanged métodos parciales que corresponden a cada propiedad de la clase.

En el caso de la clase de la película, Entity Framework crea los siguientes métodos:

- OnIdChanging
- OnIdChanged
- OnTitleChanging
- OnTitleChanged
- OnDirectorChanging
- OnDirectorChanged
- OnDateReleasedChanging
- OnDateReleasedChanged

Antes de que se cambia la propiedad correspondiente, se llama al método de OnChanging derecho. Después de cambiar la propiedad, se llama al método de OnChanged derecho.

Puede aprovechar las ventajas de estos métodos parciales para agregar lógica de validación a la clase de la película. La actualización de la clase de la película en el listado 3 comprueba que las propiedades Title y Director de se asignan valores no vacíos.

> [!NOTE] 
> 
> Un método parcial es un método definido en una clase que no son necesarios para implementar. Si no se implementa un método parcial, a continuación, el compilador quita la firma del método y todas las llamadas al método por lo que no son ningún costo de tiempo de ejecución asociado con el método parcial. En el Editor de código de Visual Studio, puede agregar un método parcial escribiendo la palabra clave *parcial* seguido de un espacio para ver una lista de parciales para implementar.


**El listado 3 - Models\Movie.cs**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample4.cs)]

Por ejemplo, si intenta asignar una cadena vacía a la propiedad Title, un mensaje de error se asigna a un diccionario denominado \_errores.

En este momento, realmente no ocurre nada cuando se asigna una cadena vacía a la propiedad de título y se agrega un error a privado \_campo de errores. Es necesario implementar la interfaz IDataErrorInfo para exponer estos errores de validación para el marco de MVC de ASP.NET.

## <a name="implementing-the-idataerrorinfo-interface"></a>Implementación de IDataErrorInfo (interfaz)

IDataErrorInfo (interfaz) ha formado parte de .NET framework desde la primera versión. Esta interfaz es una interfaz muy sencilla:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample5.cs)]

Si una clase implementa la interfaz IDataErrorInfo, el marco de MVC de ASP.NET usará esta interfaz cuando se crea una instancia de la clase. Por ejemplo, el controlador Home Create() acción acepta una instancia de la clase de película:

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample6.cs)]

El marco de MVC de ASP.NET crea la instancia de la película que se pasan a la acción Create() mediante un enlazador de modelos (el DefaultModelBinder). El enlazador de modelos es responsable de crear una instancia del objeto de película enlazando los campos de formulario HTML a una instancia del objeto de película.

El DefaultModelBinder detecta si o no una clase implementa la interfaz IDataErrorInfo. Si una clase implementa esta interfaz, a continuación, el enlazador de modelos invoca el indizador IDataErrorInfo.this para cada propiedad de la clase. Si el indizador devuelve un mensaje de error, a continuación, el enlazador de modelos agrega este mensaje de error para modelar el estado automáticamente.

El DefaultModelBinder también comprueba la propiedad IDataErrorInfo.Error. Esta propiedad sirve para representar errores de validación específica de propiedad no asociados a la clase. Por ejemplo, puede aplicar una regla de validación que depende de los valores de varias propiedades de la clase de la película. En ese caso, debe devolver un error de validación de la propiedad de Error.

La clase de película actualizada en el listado 4 implementa IDataErrorInfo (interfaz).

**Listado 4 - Models\Movie.cs (implementa IDataErrorInfo)**

[!code-csharp[Main](validating-with-the-idataerrorinfo-interface-cs/samples/sample7.cs)]

En el listado 4, se comprueba la propiedad de indizador el \_colección de errores para ver si contiene una clave que se corresponde con el nombre de propiedad que se pasa al indizador. Si no hay ningún error de validación asociado a la propiedad se devuelve una cadena vacía.

No es necesario modificar el controlador Home en ninguna forma de utilizar la clase de película modificada. La página se muestra en la figura 3 muestra lo que sucede cuando se especifica ningún valor para el título o Director de campos del formulario.


[![Creación automática de los métodos de acción](validating-with-the-idataerrorinfo-interface-cs/_static/image3.jpg)](validating-with-the-idataerrorinfo-interface-cs/_static/image5.png)

**Figura 03**: un formulario con los valores que faltan ([haga clic aquí para ver la imagen a tamaño completo](validating-with-the-idataerrorinfo-interface-cs/_static/image6.png))


Tenga en cuenta que el valor de DateReleased se valida automáticamente. Dado que la propiedad DateReleased no acepta valores NULL, el DefaultModelBinder genera un error de validación para esta propiedad automáticamente cuando no tiene un valor. Si desea modificar el mensaje de error para la propiedad DateReleased, a continuación, debe crear un enlazador de modelos personalizado.

## <a name="summary"></a>Resumen

En este tutorial, aprendió a utilizar la interfaz IDataErrorInfo para generar mensajes de error de validación. En primer lugar, se crea una clase parcial de película que extiende la funcionalidad de la clase parcial de película generada por Entity Framework. A continuación, agregamos la lógica de validación a la película clase OnTitleChanging() y OnDirectorChanging() métodos parciales. Por último, hemos implementado la interfaz IDataErrorInfo para exponer estos mensajes de validación para el marco de MVC de ASP.NET.

>[!div class="step-by-step"]
[Anterior](performing-simple-validation-cs.md)
[Siguiente](validating-with-a-service-layer-cs.md)
