---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
title: 'Parte 2: Controladores | Documentos de Microsoft'
author: jongalloway
description: Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 2 trata los controladores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 998ce4e1-9d72-435b-8f1c-399a10ae4360
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-2
msc.type: authoredcontent
ms.openlocfilehash: 680cdea388d9b01961bd626643c0fd91c9205ed7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
ms.locfileid: "30878703"
---
<a name="part-2-controllers"></a>Parte 2: controladores
====================
por [Jon Galloway](https://github.com/jongalloway)

> La tienda de música de MVC es una aplicación de tutorial que presenta y explica paso a paso cómo usar ASP.NET MVC y Visual Studio para el desarrollo web.  
>   
> La tienda de música de MVC es una implementación de almacén de ejemplo ligera que vende álbumes de música en línea e implementa administración básica del sitio, en el inicio de sesión de usuario y la funcionalidad del carro de la compra.  
>   
> Esta serie de tutoriales detalla todos los pasos realizados para compilar la aplicación de ejemplo de la tienda de música de MVC de ASP.NET. Parte 2 trata los controladores.


Con marcos web tradicionales, las direcciones URL entrantes suelen asignarse a los archivos de disco. Por ejemplo: una solicitud para una dirección URL, como "/ Products.aspx" o "/ Products.php" podría ser procesados por un archivo "Products.aspx" o "Products.php".

Marcos de trabajo basado en Web de MVC asignan las direcciones URL al código del servidor de forma ligeramente diferente. En lugar de asignar direcciones URL entrantes a los archivos, se asignan las direcciones URL en su lugar a métodos en clases. Estas clases se denominan "Controladores" y son responsables de procesar las solicitudes HTTP entrantes, control proporcionados por el usuario, recuperar y guardar los datos y determinar la respuesta para enviar al cliente (Mostrar HTML, descargar un archivo, redirija a otra Dirección URL, etcetera).

## <a name="adding-a-homecontroller"></a>Agregar un HomeController

Comenzaremos nuestra aplicación de tienda de música de MVC agregando una clase de controlador que controlará las direcciones URL a la página principal de nuestro sitio. Comenzaremos siguen las convenciones de nomenclatura predeterminado de ASP.NET MVC y llámelo HomeController.

Haga clic en la carpeta "Controladores" en el Explorador de soluciones y seleccione "Agregar" y, a continuación, el "controlador..." comando:

![](mvc-music-store-part-2/_static/image1.jpg)

Se abrirá el cuadro de diálogo "Agregar controlador". Asigne al controlador "HomeController" y presione el botón Agregar.

![](mvc-music-store-part-2/_static/image1.png)

Esto creará un nuevo archivo, HomeController.cs, con el código siguiente:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample1.cs)]

Para iniciar tan simple como sea posible, vamos a sustituir el método de índice con un método simple que solo devuelva una cadena. Nos aseguraremos de hacer dos cambios:

- Cambiar el método para devolver una cadena en lugar de un ActionResult
- Cambie la instrucción return para devolver "Hello de inicio"

El método debe tener el siguiente aspecto:

[!code-csharp[Main](mvc-music-store-part-2/samples/sample2.cs)]

## <a name="running-the-application"></a>Ejecutar la aplicación

Ahora vamos a ejecutar el sitio. Podemos iniciar el servidor web y probar el sitio mediante cualquiera de las siguientes acciones:

- Elija el elemento de menú Iniciar depuración de depuración ⇨
- Haga clic en el botón de flecha verde en la barra de herramientas ![](mvc-music-store-part-2/_static/image2.jpg)
- Utilice el método abreviado de teclado F5.

Mediante cualquiera de los pasos anteriores se compile el proyecto y, a continuación, hacer que el servidor de desarrollo de ASP.NET que está integrada en Visual Web Developer para iniciar. Una notificación aparecerá en la esquina inferior de la pantalla para indicar que el servidor de desarrollo de ASP.NET se ha iniciado y se mostrará al número de puerto que se está ejecutando bajo.

![](mvc-music-store-part-2/_static/image2.png)

Visual Web Developer, a continuación, se abrirá una ventana del explorador cuya dirección URL señala a nuestro servidor web automáticamente. Esto nos permitirá probar rápidamente la aplicación web:

![](mvc-music-store-part-2/_static/image3.png)

Bien, que era bastante rápida: hemos creado un nuevo sitio Web, agrega una función de tres líneas, y tenemos texto en un explorador. No rocket ciencia, pero es un tutorial de inicio.

*Nota: Visual Web Developer incluye el servidor de desarrollo de ASP.NET, que se ejecutará el sitio Web en un número aleatorio libre "puerto". En la captura de pantalla anterior, el sitio se ejecuta en `http://localhost:26641/`, por lo que está utilizando el puerto 26641. El número de puerto será diferente. Cuando hablamos /Store/Browse like de la dirección URL en este tutorial, que pasa después del número de puerto. Suponiendo que un número de puerto de 26641, vaya a/almacén/examinar significará examinan `http://localhost:26641/Store/Browse`.*

## <a name="adding-a-storecontroller"></a>Agregar un StoreController

Hemos agregado un HomeController simple que implementa la página principal de nuestro sitio. Ahora agreguemos otro controlador que vamos a usar para implementar la funcionalidad de exploración de la tienda de música. El controlador de almacén será compatible con tres escenarios:

- Una página de lista de los géneros de música en nuestra tienda de música
- Una página de búsqueda que enumera todos los álbumes de música de un género determinado
- Una página de detalles que muestra información sobre un álbum de música específico

Comenzaremos agregando una nueva clase StoreController... Si no lo ha hecho ya, detener la ejecución de la aplicación por cerrar el explorador o seleccionar el elemento de menú Detener depuración ⇨ de depuración.

Ahora, agregue una nueva StoreController. Al igual que hicimos con HomeController, deberá hacer esto, haga doble clic en la carpeta "Controladores" en el Explorador de soluciones y elija Agregar -&gt;elemento de menú de controlador

![](mvc-music-store-part-2/_static/image4.png)

Nuestro StoreController nuevo ya tiene un método de "Index". Vamos a usar este método "Índice" implementar nuestra página de lista que enumera todos los juegos en nuestra tienda de música. Agregaremos también dos métodos adicionales para implementar otros de los dos escenarios que deseamos que nuestra StoreController para controlar: Examinar y detalles.

Se llaman a estos métodos (índices, examinar y detalles) dentro de nuestro controlador "Acciones del controlador" y, como ya ha visto con el método de acción de HomeController.Index (), su trabajo es responder a las solicitudes de dirección URL y determinar (por lo general) el contenido se debe enviar en el explorador o el usuario que invocó la dirección URL.

Comenzaremos nuestra implementación StoreController cambiando el método theIndex() para devolver la cadena "Hello de Store.Index()" y vamos a agregar métodos similares para Browse() y Details():

[!code-csharp[Main](mvc-music-store-part-2/samples/sample3.cs)]

Vuelva a ejecutar el proyecto y examinar las direcciones URL siguientes:

- / Almacén
- / Almacén/examinar
- / / Detalles del almacén

Obtener acceso a estas direcciones URL se invocar los métodos de acción dentro de nuestro controlador y devuelva las respuestas de la cadena:

![](mvc-music-store-part-2/_static/image5.png)

Que es grande, pero se trata de cadenas constantes solo. Vamos a darles dinamismo, lo que toman la información de la dirección URL y mostrarla en el resultado de la página.

En primer lugar, se cambiará el método de acción de exploración para recuperar un valor de cadena de consulta de la dirección URL. Podemos hacerlo mediante la adición de un parámetro de "género" para el método de acción. Al hacer esto ASP.NET MVC pasará automáticamente los parámetros de envío de cadena de consulta o un formulario con el nombre "género" para el método de acción cuando se invoca.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample4.cs)]

*Nota: Estamos usando el método de utilidad HttpUtility.HtmlEncode al corregir la entrada del usuario. ¿Esto impide que los usuarios insertar Javascript en la vista con un vínculo como /Store/Browse? Género =&lt;script&gt;window.location='http://hackersite.com'&lt;/script&gt;.*

¿Ahora vamos a examinar para/almacén/examinar? Género = Disco

![](mvc-music-store-part-2/_static/image6.png)

Vamos a cambiar a continuación la acción de detalles para leer y mostrar un parámetro de entrada con el nombre de identificador. A diferencia de nuestro método anterior, no puede incrustar el valor de identificador como un parámetro de cadena de consulta. En su lugar, se podrá insertar directamente dentro de la propia dirección URL. Por ejemplo: /Store/Details/5.

ASP.NET MVC nos permite hacerlo fácilmente sin tener que configurar nada. Convención de enrutamiento de ASP.NET MVC predeterminada consiste en tratar el segmento de una dirección URL después del nombre de método de acción como un parámetro denominado "ID". Si el método de acción tiene un parámetro denominado Id. a continuación, ASP.NET MVC automáticamente pasará el segmento de dirección URL para usted como un parámetro.

[!code-csharp[Main](mvc-music-store-part-2/samples/sample5.cs)]

Ejecute la aplicación y vaya a /Store/Details/5:

![](mvc-music-store-part-2/_static/image7.png)

Resumamos lo que hemos hecho hasta ahora:

- Hemos creado un nuevo proyecto de MVC de ASP.NET en Visual Web Developer
- Hemos hablado de la estructura de carpetas básica de una aplicación ASP.NET MVC
- Ahora sabe cómo ejecutar nuestro sitio Web mediante el servidor de desarrollo de ASP.NET
- Hemos creado dos clases de controlador: un HomeController y un StoreController
- Hemos agregado los métodos de acción a nuestro controladores que respondan a solicitudes de dirección URL y devuelven texto en el explorador


> [!div class="step-by-step"]
> [Anterior](mvc-music-store-part-1.md)
> [Siguiente](mvc-music-store-part-3.md)
