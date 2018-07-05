---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Microsoft Docs'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 2 trata los controladores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: a854feff675cae302b9927d209808257b7d087cb
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37381808"
---
<a name="part-2-controllers"></a>Parte 2: controladores
====================
por [Jon Galloway](https://github.com/jongalloway)

> El Store de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> El Store de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa la administración básica del sitio, inicio de sesión de usuario y funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de Music Store de ASP.NET MVC. Parte 2 trata los controladores.


Con marcos web tradicionales, las direcciones URL entrantes suelen asignarse a los archivos en disco. Por ejemplo: una solicitud para una dirección URL como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".

Marcos basados en Web MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente. En lugar de asignar direcciones URL entrantes a los archivos, en su lugar asignar URL a métodos en clases. Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control de entrada del usuario, recuperar y guardar los datos y determinar la respuesta enviada al cliente (Mostrar HTML, descargar un archivo, redirigir a otro Dirección URL, etcetera).

## <a name="adding-a-homecontroller"></a>Agregar un HomeController

Empezaremos a nuestra aplicación de MVC Music Store mediante la adición de una clase de controlador que controlará las direcciones URL a la página principal de nuestro sitio. Comenzaremos siguen las convenciones de nomenclatura predeterminado de ASP.NET MVC y llámelo HomeController.

Haga clic en la carpeta "Controladores" en el Explorador de soluciones y seleccione "Agregar" y, a continuación, el comando "controlador":

![](mvc-music-store-part-2/_static/image1.jpg)

Se abrirá el cuadro de diálogo "Agregar controlador". Nombre del controlador "HomeController" y presione el botón Agregar.

![](mvc-music-store-part-2/_static/image1.png)

Esto creará un nuevo archivo, HomeController.cs, con el código siguiente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para iniciar tan simple como sea posible, vamos a reemplazar el método Index con un método sencillo que simplemente devuelve una cadena. Haremos dos cambios:

- Cambiar el método para devolver una cadena en lugar de un ActionResult
- Cambie la instrucción return para devolver "Hello de hogar"

El método ahora un aspecto similar al siguiente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ejecutar la aplicación

Ahora vamos a ejecutar el sitio. Podemos iniciar nuestro servidor web y pruebe el sitio mediante cualquiera de las siguientes acciones:

- Elija el elemento de menú Iniciar depuración ⇨ de depuración
- Haga clic en el botón de flecha verde en la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)
- Utilice el método abreviado F5.

Con cualquiera de los pasos anteriores compilar el proyecto y, a continuación, hacer que el servidor de desarrollo de ASP.NET que está integrada en Visual Web Developer para iniciar. Una notificación aparecerá en la esquina inferior de la pantalla para indicar que el servidor de desarrollo de ASP.NET haya iniciado y mostrará al número de puerto que se está ejecutando bajo.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer, a continuación, se abrirá una ventana del explorador cuya dirección URL apunta a nuestro servidor web automáticamente. Esto nos permitirá probar rápidamente nuestra aplicación web:

![](mvc-music-store-part-2/_static/image3.png)

Bien, que era bastante rápido, que creamos un nuevo sitio Web, ha agregado una función de tres líneas, y tenemos texto en un explorador. Rocket no ciencia, pero es un comienzo.

*Nota: Visual Web Developer incluye el servidor de desarrollo de ASP.NET, que se ejecutará el sitio Web en un número aleatorio libre "puerto". En la captura de pantalla anterior, se está ejecutando en el sitio `http://localhost:26641/`, por lo que está utilizando el puerto 26641. El número de puerto será diferente. Cuando hablamos sobre como /Store/Browse la dirección URL en este tutorial, que pasará después del número de puerto. Suponiendo que un número de puerto de 26641, vaya a/Store/examinar significa ir a `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Agregar un StoreController

Se ha agregado un HomeController simple que implementa la página principal de nuestro sitio. Ahora agreguemos otro controlador que vamos a usar para implementar la funcionalidad de exploración de nuestra tienda de música. Nuestro controlador de almacén será compatible con tres escenarios:

- Una página de listado de los géneros de música en nuestra tienda de música
- Una página de exploración que enumera todos los álbumes de música de un género determinado
- Una página de detalles que muestra información sobre un álbum de música específicos

Comenzaremos por agregar una nueva clase StoreController... Si no lo ha hecho ya, detenga la ejecución de la aplicación, ya sea cerrando el explorador o seleccionando el elemento de menú Detener depuración ⇨ de depuración.

Ahora, agregue un nuevo StoreController. Tal como se hizo con HomeController, lo haremos con el botón secundario en la carpeta "Controladores" en el Explorador de soluciones y eligiendo agregar -&gt;elemento de menú del controlador

![](mvc-music-store-part-2/_static/image4.png)

Nuestro nuevo StoreController ya tiene un método "Index". Este método "Index" vamos a usar para implementar nuestra página de lista que se enumera todos los géneros en nuestra tienda de música. También agregaremos dos métodos adicionales para implementar otros de los dos escenarios que deseamos que nuestro StoreController para controlar: Examinar y detalles.

Estos métodos (índice, examinar y detalles) dentro de nuestro controlador se denominan "Controlador de acciones" y como ya hemos visto con el método de acción HomeController.Index (), su trabajo es responder a las solicitudes de dirección URL y (general) determinar qué contenido se debe enviar al explorador o usuario que invocó la dirección URL.

Comenzaremos a nuestra implementación StoreController cambiando theIndex() al método para devolver la cadena "Hello de Store.Index()" y agregaremos métodos similares para Browse() y Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Vuelva a ejecutar el proyecto y examinar las direcciones URL siguientes:

- / Store
- / Store/examinar
- / Store/detalles

Obtener acceso a estas direcciones URL se invocan los métodos de acción dentro de nuestro controlador y devolver las respuestas de cadena:

![](mvc-music-store-part-2/_static/image5.png)

Eso está muy bien, pero estas son las cadenas constantes solo. Aumentemos su dinámica, por lo que toman la información de la dirección URL y mostrarlos en el resultado de la página.

Primero, cambiaremos el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL. Esto se logra mediante la adición de un parámetro de "género" a nuestro método de acción. Al hacer esto ASP.NET MVC pasará automáticamente los parámetros post de cadena de consulta o formulario denominados "género" a nuestro método de acción cuando se invoca.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: Vamos a usar el método de utilidad HttpUtility.HtmlEncode para sanear la entrada del usuario. ¿Esto impide que los usuarios de inyección de Javascript en nuestra vista con un vínculo como /Store/Browse? Género =&lt;script&gt;Window.Location = "http://hackersite.com'&lt;/script&gt;.*

¿Ahora vamos a examinar para/Store/examinar? Género = Disco

![](mvc-music-store-part-2/_static/image6.png)

Vamos a próximo cambio de la acción de detalles para leer y mostrar un parámetro de entrada denominado Id. A diferencia de nuestro método anterior, no se puede incrustar el valor de identificador como un parámetro de cadena de consulta. En su lugar, se insertará directamente dentro de la propia dirección URL. Por ejemplo: /Store/Details/5.

ASP.NET MVC nos permite hacerlo fácilmente sin tener que configurar nada. Convención de enrutamiento predeterminado de ASP.NET MVC es tratar el segmento de una dirección URL después del nombre del método de acción como un parámetro denominado "ID". Si el método de acción tiene un parámetro denominado Id. a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Ejecute la aplicación y vaya a /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Resumamos lo que hemos hecho hasta ahora:

- Hemos creado un nuevo proyecto de ASP.NET MVC en Visual Web Developer
- Hemos analizado la estructura de carpetas básica de una aplicación ASP.NET MVC
- Nos hemos aprendido a ejecutar nuestro sitio Web mediante el servidor de desarrollo de ASP.NET
- Hemos creado dos clases de controlador: un HomeController y un StoreController
- Hemos agregado métodos de acción a nuestro controladores que responden a solicitudes de dirección URL y devuelven texto en el explorador


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)
