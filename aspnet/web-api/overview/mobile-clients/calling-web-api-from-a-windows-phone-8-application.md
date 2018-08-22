---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: Llamar a API Web desde un Windows Phone 8 aplicación (C#) | Microsoft Docs
author: rmcmurray
description: Crear un escenario de extremo a otro completo que consta de una aplicación de ASP.NET Web API que proporciona un catálogo de libros a una aplicación de Windows Phone 8.
ms.author: riande
ms.date: 10/09/2013
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: ca2b5f41f6c3bd38faacd1e15c4dee6f6210aff7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827538"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a>Llamar a API Web desde una aplicación de Windows Phone 8 (C#)
====================
por [Robert McMurray](https://github.com/rmcmurray)

En este tutorial, obtendrá información sobre cómo crear un escenario de extremo a otro completo que consta de una aplicación de ASP.NET Web API que proporciona un catálogo de libros a una aplicación de Windows Phone 8.

### <a name="overview"></a>Información general

Servicios de rESTful como ASP.NET Web API simplifican la creación de aplicaciones basadas en HTTP para los desarrolladores mediante la abstracción de la arquitectura para aplicaciones de cliente y servidor. En lugar de crear un protocolo propietario basado en sockets para la comunicación, los desarrolladores de API Web basta publicar los métodos HTTP necesarios para su aplicación, (por ejemplo: GET, POST, PUT, DELETE), y los desarrolladores de aplicaciones de cliente solo necesitan consumirlo los métodos HTTP que son necesarios para su aplicación.

En este tutorial de extremo a otro, obtendrá información sobre cómo usar la API Web para crear los siguientes proyectos:

- En el [primera parte de este tutorial](#STEP1), creará una aplicación de ASP.NET Web API que admite todas las operaciones de creación, lectura, actualización y eliminación (CRUD) para administrar un catálogo de libros. Esta aplicación usará el [archivo XML (books.xml) de ejemplo](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) de MSDN.
- En el [segunda parte de este tutorial](#STEP2), creará una aplicación de Windows Phone 8 interactiva que recupera los datos de la aplicación Web API.

#### <a name="prerequisites"></a>Requisitos previos

- Visual Studio 2013 con instalado el SDK de Windows Phone 8
- Windows 8 o posterior en un sistema de 64 bits con Hyper-V instalado
- Para obtener una lista de requisitos adicionales, consulte el *requisitos del sistema* sección en la [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) página de descarga.

> [!NOTE]
> Si va a probar la conectividad entre la API Web y proyectos de Windows Phone 8 en su sistema local, deberá seguir las instrucciones de la *[conectando el emulador de Windows Phone 8 a las aplicaciones de API Web en una variable Local Equipo](https://go.microsoft.com/fwlink/?LinkId=324014)* artículo para configurar el entorno de pruebas.


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a>Paso 1: Crear el proyecto de la librería de API Web

El primer paso de este tutorial de extremo a otro es crear un proyecto Web API que admite todas las operaciones CRUD; Tenga en cuenta que se agregará el proyecto de aplicación de Windows Phone para esta solución en [paso 2](#STEP2) de este tutorial.

1. Abra **Visual Studio 2013**.
2. Haga clic en **archivo**, a continuación, **nueva**y, a continuación, **proyecto**.
3. Cuando el **nuevo proyecto** se muestra el cuadro de diálogo, expanda **instalado**, a continuación, **plantillas**, a continuación, **Visual C#** y, a continuación, **Web**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Haga clic en la imagen para expandir                                                                |


4. Resaltar **aplicación Web ASP.NET**, escriba **librería** para el nombre del proyecto y, a continuación, haga clic en **Aceptar**.
5. Cuando el **nuevo proyecto ASP.NET** se muestra el cuadro de diálogo, seleccione el **API Web** plantilla y, a continuación, haga clic en **Aceptar**.


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                Haga clic en la imagen para expandir                                                                |


6. Cuando se abre el proyecto Web API, quite el controlador de ejemplo del proyecto:

    1. Expanda el **controladores** carpeta en el Explorador de soluciones.
    2. Haga clic en el **ValuesController.cs** de archivo y, a continuación, haga clic en **eliminar**.
    3. Haga clic en **Aceptar** cuando se le pida que confirme la eliminación.
7. Agregar un archivo de datos XML al proyecto Web API; Este archivo contiene el contenido del catálogo librería:

   1. Haga clic en el **aplicación\_datos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **nuevo elemento**.
   2. Cuando el **Agregar nuevo elemento** se muestra el cuadro de diálogo, resalte el **archivo XML** plantilla.
   3. Nombre del archivo **Books.xml**y, a continuación, haga clic en **agregar**.
   4. Cuando el **Books.xml** se abre el archivo, reemplace el código en el archivo con el código XML del ejemplo **books.xml** archivo en MSDN: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. Guarde y cierre el archivo XML.

8. Agregar el modelo de librería al proyecto Web API; Este modelo contiene la lógica de creación, lectura, actualización y eliminación (CRUD) para la aplicación de la librería:

   1. Haga clic en el **modelos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **clase**.
   2. Cuando el **Agregar nuevo elemento** se muestra el cuadro de diálogo, asigne el nombre del archivo de clase **BookDetails.cs**y, a continuación, haga clic en **agregar**.
   3. Cuando el **BookDetails.cs** se abre el archivo, reemplace el código en el archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. Guarde y cierre el **BookDetails.cs** archivo.

9. Agregue el controlador de la librería al proyecto Web API:

   1. Haga clic en el **controladores** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **controlador**.
   2. Cuando el **agregar Scaffold** se muestra el cuadro de diálogo, resalte **controlador Web API 2 - vacío**y, a continuación, haga clic en **agregar**.
   3. Cuando el **Agregar controlador** se muestra el cuadro de diálogo, asigne el nombre del controlador **BooksController**y, a continuación, haga clic en **agregar**.
   4. Cuando el **BooksController.cs** se abre el archivo, reemplace el código en el archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. Guarde y cierre el **BooksController.cs** archivo.

10. Compile la aplicación de API Web para comprobar si hay errores.

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a>Paso 2: Agregar el proyecto de catálogo de Windows Phone 8 librería

El siguiente paso de este escenario de extremo a otro es crear la aplicación de catálogo para Windows Phone 8. Esta aplicación usará el *Windows Phone Databound App* plantilla para la interfaz de usuario predeterminada y se usará la aplicación de API Web que creó en [paso 1](#STEP1) de este tutorial como origen de datos.

1. Haga clic en el **librería** solución en la en el Explorador de soluciones, haga clic en **agregar**y, a continuación, **nuevo proyecto**.
2. Cuando el **nuevo proyecto** se muestra el cuadro de diálogo, expanda **instalado**, a continuación, **Visual C#** y, a continuación, **Windows Phone**.
3. Resaltar **Windows Phone Databound App**, escriba **BookCatalog** para el nombre y, a continuación, haga clic en **Aceptar**.
4. Agregue el paquete Json.NET NuGet para la **BookCatalog** proyecto:

    1. Haga clic en **referencias** para el **BookCatalog** del proyecto en el Explorador de soluciones y, a continuación, haga clic en **administrar paquetes de NuGet**.
    2. Cuando el **administrar paquetes de NuGet** se muestra el cuadro de diálogo, expanda el **Online** sección y resaltar **nuget.org**.
    3. Escriba **Json.NET** en la búsqueda del campo y haga clic en el icono de búsqueda.
    4. Resaltar **Json.NET** en los resultados de búsqueda y, a continuación, haga clic en **instalar**.
    5. Cuando haya finalizado la instalación, haga clic en **cerrar**.
5. Agregar el **BookDetails** modelos a la **BookCatalog** proyecto; contiene un modelo genérico de la clase librería:

   1. Haga clic en el **BookCatalog** del proyecto en el Explorador de soluciones y, después, haga clic en **agregar**y, a continuación, haga clic en **nueva carpeta**.
   2. Nombre de la nueva carpeta **modelos**.
   3. Haga clic en el **modelos** , a continuación, haga clic en la carpeta en el Explorador de soluciones, **agregar**y, a continuación, haga clic en **clase**.
   4. Cuando el **Agregar nuevo elemento** se muestra el cuadro de diálogo, asigne el nombre del archivo de clase **BookDetails.cs**y, a continuación, haga clic en **agregar**.
   5. Cuando el **BookDetails.cs** se abre el archivo, reemplace el código en el archivo por lo siguiente: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. Guarde y cierre el **BookDetails.cs** archivo.

6. Actualización de la **MainViewModel.cs** clase para que incluya la funcionalidad para comunicarse con la aplicación de API Web de librería:

   1. Expanda el **ViewModels** carpeta en el Explorador de soluciones y, a continuación, haga doble clic en el **MainViewModel.cs** archivo.
   2. Cuando el **MainViewModel.cs** se abre el archivo, reemplace el código en el archivo con el siguiente; tenga en cuenta que deberá actualizar el valor de la `apiUrl` constante con la dirección URL real de la API Web: 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. Guarde y cierre el **MainViewModel.cs** archivo.

7. Actualización de la **MainPage.xaml** archivo para personalizar el nombre de la aplicación:

   1. Haga doble clic en el **MainPage.xaml** archivo en el Explorador de soluciones.
   2. Cuando el **MainPage.xaml** archivo se abre, busque las siguientes líneas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. Reemplace las líneas con lo siguiente: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. Guarde y cierre el **MainPage.xaml** archivo.

8. Actualización de la **DetailsPage.xaml** archivo para personalizar los elementos mostrados:

   1. Haga doble clic en el **DetailsPage.xaml** archivo en el Explorador de soluciones.
   2. Cuando el **DetailsPage.xaml** archivo se abre, busque las siguientes líneas de código: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. Reemplace las líneas con lo siguiente: 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. Guarde y cierre el **DetailsPage.xaml** archivo.

9. Compile la aplicación de Windows Phone para comprobar si hay errores.

### <a name="step-3-testing-the-end-to-end-solution"></a>Paso 3: Probar la solución de extremo a otro

Como se mencionó en el *requisitos previos* proyectos de la sección de este tutorial, cuando esté probando la conectividad entre la API Web y Windows Phone 8 en su sistema local, debe seguir las instrucciones en el *[ Conectar el emulador de Windows Phone 8 a las aplicaciones de API Web en un equipo Local](https://go.microsoft.com/fwlink/?LinkId=324014)* artículo para configurar el entorno de pruebas.

Una vez configurado el entorno de pruebas, deberá establecer la aplicación de Windows Phone como proyecto de inicio. Para ello, resalte el **BookCatalog** aplicación en el Explorador de soluciones y, a continuación, haga clic en **establecer como proyecto de inicio**:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| Haga clic en la imagen para expandir |

Al presionar F5, Visual Studio iniciará ambos el Windows Phone Emulator, que se mostrará un &quot;espere&quot; mensaje mientras se recuperan los datos de aplicación de la API Web:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| Haga clic en la imagen para expandir |

Si todo se realiza correctamente, debería ver el catálogo de muestra:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| Haga clic en la imagen para expandir |

Si pulsa en cualquier título de libro, la aplicación mostrará la descripción del libro:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| Haga clic en la imagen para expandir |

Si la aplicación no puede comunicarse con la API Web, se mostrará un mensaje de error:

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| Haga clic en la imagen para expandir |

Si pulsa en el mensaje de error, se mostrará detalles adicionales sobre el error:


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 Haga clic en la imagen para expandir                                                                 |

