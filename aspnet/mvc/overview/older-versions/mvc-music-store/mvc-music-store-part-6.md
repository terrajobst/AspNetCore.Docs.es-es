---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Usar anotaciones de datos para la validación del modelo | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 6 incluye el uso de anotaciones de datos para el modelo V...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 328eccb4324bb10a7e8dec819a70129fc14c42c4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872421"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Usar anotaciones de datos para la validación del modelo
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 6 incluye el uso de anotaciones de datos para la validación del modelo.


Tenemos un problema importante con nuestro formas de crear y editar: no realiza ninguna validación. También podríamos hacer cosas como deje los campos obligatorios en blanco o escriba las letras en el campo Precio y el primer error que veremos procede de la base de datos.

Podemos agregar validación a nuestra aplicación fácilmente mediante la adición de anotaciones de datos para las clases de modelo. Las anotaciones de datos permiten describir las reglas que se aplica a las propiedades de nuestro modelo, queremos y ASP.NET MVC se encargará de aplicándolos y mostrar los mensajes adecuados a los usuarios.

## <a name="adding-validation-to-our-album-forms"></a>Agregar validación a los formularios de álbum

Vamos a usar los atributos de anotación de datos siguientes:

- **Requiere** : indica que la propiedad es un campo obligatorio
- **DisplayName** : define el texto que desea usar en los campos de formulario y mensajes de validación
- **StringLength** : define una longitud máxima para un campo de cadena
- **Intervalo** – da como resultado un valor máximo y mínimo para un campo numérico
- **Enlazar** : enumera los campos para excluir o incluir al enlazar los valores de parámetro o un formulario a propiedades del modelo
- **ScaffoldColumn** : permite ocultar los campos de formularios de editor

*Nota: Para obtener más información sobre la validación de modelo de uso de atributos de anotación de datos, vea la documentación de MSDN en*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra la clase de álbum y agregue el siguiente *con* instrucciones a la parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

A continuación, actualice las propiedades para agregar atributos de presentación y la validación, tal y como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Mientras se esté allí, también hemos cambiado el género y el intérprete a propiedades virtuales. Esto permite a Entity Framework una carga diferida ellos según sea necesario.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Después de tener agrega estos atributos a nuestro modelo de álbum, nuestra pantalla de creación y edición de comenzar inmediatamente la validación de campos y con los nombres para mostrar que hemos elegido (por ejemplo, álbum carátulas de dirección Url en lugar de AlbumArtUrl). Ejecute la aplicación y vaya a /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

A continuación, se estropea algunas reglas de validación. Escriba un precio de 0 y deje en blanco el título. Cuando se haga clic en el botón Crear, se verá el formulario muestra validación mensajes de error que muestra los campos que no cumple las reglas de validación que hemos definido.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Probar la validación del lado cliente

Validación del lado servidor es muy importante desde una perspectiva de aplicación, ya que los usuarios pueden eludir la validación del lado cliente. Sin embargo, los formularios de la página Web que implementan solo la validación del lado servidor presentan tres problemas importantes.

1. El usuario tiene que esperar para que el formulario se registra, se valida en el servidor y de la respuesta se envíen a su explorador.
2. El usuario no recibe una respuesta inmediata cuando corrija un campo para que pase ahora las reglas de validación.
3. Estamos malgastar los recursos del servidor para llevar a cabo una lógica de validación en lugar de aprovechar el explorador del usuario.

Afortunadamente, las plantillas de scaffolding de ASP.NET MVC 3 tienen validación del lado cliente integrada, la necesidad de realizar ningún trabajo adicional alguno.

Escribir una sola letra en el campo Título cumple los requisitos de validación, por lo que se quita inmediatamente el mensaje de validación.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Siguiente](mvc-music-store-part-7.md)
