---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
title: 'Parte 6: Uso de anotaciones de datos para la validación del modelo | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 6 se describe el uso de anotaciones de datos para el modelo V...
ms.author: aspnetcontent
ms.date: 04/21/2011
ms.assetid: b3193d33-2d0b-4d98-9712-58bd897c62ec
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6
msc.type: authoredcontent
ms.openlocfilehash: 10884f569f0f5d95517b73daab31fbd269a4726a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825958"
---
<a name="part-6-using-data-annotations-for-model-validation"></a>Parte 6: Usar anotaciones de datos para la validación del modelo
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 6 se describe el uso de anotaciones de datos para la validación del modelo.


Tenemos un problema importante con los formularios de Create y Edit: no hacen ninguna validación. Podemos hacer cosas como dejar los campos obligatorios en blanco o escriba las letras en el campo de precio y el primer error que veremos es desde la base de datos.

Podemos agregar validación a nuestra aplicación fácilmente mediante la adición de anotaciones de datos a nuestras clases de modelo. Las anotaciones de datos nos permiten describir las reglas que queremos aplicadas a propiedades del modelo y ASP.NET MVC se encargará de aplicarlas y mostrar los mensajes adecuados a nuestros usuarios.

## <a name="adding-validation-to-our-album-forms"></a>Agregar validación a los formularios de álbum

Vamos a usar los siguientes atributos de anotación de datos:

- **Requiere** : indica que la propiedad es un campo obligatorio
- **DisplayName** : define el texto que desea utilizar en los campos de formulario y los mensajes de validación
- **StringLength** : define una longitud máxima para un campo de cadena
- **Intervalo** : da como resultado un valor máximo y mínimo de un campo numérico
- **Enlazar** : una lista de campos para excluir o incluir al enlazar los valores de parámetro o un formulario con las propiedades del modelo
- **ScaffoldColumn** : permite ocultar los campos de formularios de editor

*Nota: Para obtener más información sobre la validación del modelo utilizando atributos de anotación de datos, vea la documentación de MSDN en*[`https://go.microsoft.com/fwlink/?LinkId=159063`](https://go.microsoft.com/fwlink/?LinkId=159063)

Abra la clase de álbum y agregue la siguiente *mediante* instrucciones a la parte superior.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample1.cs)]

A continuación, actualice las propiedades para agregar atributos de presentación y la validación tal como se muestra a continuación.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample2.cs)]

Mientras se encuentra allí, también hemos cambiado el género y el intérprete a propiedades virtuales. Esto permite a Entity Framework para la carga diferida ellos según sea necesario.

[!code-csharp[Main](mvc-music-store-part-6/samples/sample3.cs)]

Después de tener agregados estos atributos en nuestro modelo de álbum, nuestra pantalla de Create y Edit inmediatamente comenzar la validación de campos y con los nombres para mostrar hemos elegido (por ejemplo, álbum arte Url en lugar de AlbumArtUrl). Ejecute la aplicación y vaya a /StoreManager/Create.

![](mvc-music-store-part-6/_static/image1.png)

A continuación, dividiremos algunas reglas de validación. Escriba un precio de 0 y deje que el título en blanco. Al hacer clic en el botón Crear, vemos el formulario mostrado validación los mensajes de error que muestra los campos que no cumple las reglas de validación que hemos definido.

![](mvc-music-store-part-6/_static/image2.png)

## <a name="testing-the-client-side-validation"></a>Probar la validación del lado cliente

Validación del lado servidor es muy importante desde una perspectiva de la aplicación, ya que los usuarios pueden eludir la validación del lado cliente. Sin embargo, los formularios de página Web que solo implementan la validación de servidor presentan tres problemas importantes.

1. El usuario tiene que esperar para que el formulario se registra, se valida en el servidor y para la respuesta se envíe a su explorador.
2. El usuario no recibe una respuesta inmediata cuando corrija un campo para que ahora supera las reglas de validación.
3. Nos estamos desperdiciar recursos del servidor para llevar a cabo la lógica de validación en lugar de aprovechar el explorador del usuario.

Afortunadamente, las plantillas de scaffolding de ASP.NET MVC 3 tienen la validación del lado cliente integrado, no requerir ningún trabajo adicional de ningún tipo.

Escribir una sola letra en el campo Título cumple los requisitos de validación, por lo que se quita inmediatamente el mensaje de validación.

![](mvc-music-store-part-6/_static/image3.png)


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-5.md)
> [Siguiente](mvc-music-store-part-7.md)
