---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: "Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 | Documentos de Microsoft"
author: Erikre
description: "Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: 6ae398f94c0ac3872601bdc8fd935f6d285793db
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 03/15/2018
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. [Prueba de ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Pruebe sus conocimientos y reforzar los conceptos clave tomando el examen de ASP.NET Web Forms. Esta prueba se diseñó específicamente desde el contenido de esta serie de tutoriales. Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.


## <a name="introduction"></a>Introducción

Esta serie de tutoriales le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio Express 2013 para Web y ASP.NET 4.5.

Con el nombre de la aplicación podrá crear **Wingtip Toys**. Es un ejemplo simplificado de un sitio web de almacén frontal que vende productos en línea. Esta serie de tutoriales destaca las nuevas características disponibles en ASP.NET 4.5.

Los comentarios son bienvenidos y nos aseguraremos de hacer todo lo posible para actualizar esta serie de tutoriales en función de sus sugerencias.

### <a name="download-completed-project"></a>Proyecto descargado

Puede descargar un proyecto de C# que contiene el tutorial completado.

- [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Revise el contenido realizando el examen de formularios Web Forms de ASP.NET relacionado

Después de completar este tutorial, probar su conocimiento y reforzar los conceptos clave tomando el [ASP.NET Web Forms cuestionario](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Esta prueba se diseñó específicamente desde el contenido de esta serie de tutoriales. Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.

- [Prueba de ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Audiencia

Los destinatarios de esta serie de tutoriales son programadores experimentados que están familiarizados con ASP.NET Web Forms. Un desarrollador interesado en esta serie de tutoriales debe tener los conocimientos siguientes:

- Que están familiarizados con un objeto orientada a servicios de lenguaje de programación (OOP)
- Familiarizado con conceptos de desarrollo de Web (HTML, CSS, JavaScript)
- Familiarizado con conceptos de base de datos relacional
- Familiarizado con conceptos de arquitectura de n niveles

Si está interesado en revisar los problemas descritos anteriormente, considere la posibilidad de revisar el contenido siguiente:

- [Introducción a Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitectura de varios niveles](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Características de la aplicación

Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:

- El proyecto de aplicación Web (no el proyecto de sitio Web)
- formularios Web Forms
- Páginas principales, configuración
- bootstrap
- Entity Framework Code First, LocalDB
- La validación de solicitudes
- Establecimiento inflexible de tipos controles de datos, modelo de enlace, las anotaciones de datos y el valor de proveedores
- SSL y OAuth
- Identidad de ASP.NET, la configuración y la autorización
- Validación discreto
- Enrutamiento
- Control de errores de ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tareas y escenarios de aplicación

Las tareas que se muestran en esta serie incluyen:

- Crear, revisar y ejecutar el proyecto nuevo
- Creación de la estructura de base de datos
- Inicializar y la propagación de la base de datos
- Personalizar la interfaz de usuario mediante estilos, gráficos y una página maestra
- Agregar páginas y navegación
- Mostrar detalles de menú y los datos de producto
- Creación de un carro de la compra
- Compatibilidad con SSL agregando y OAuth
- Agregar un método de pago
- Como un rol de administrador y un usuario a la aplicación
- Restringir el acceso a páginas específicas y carpeta
- Cargar un archivo a la aplicación web
- Implementar la validación de entrada
- Registrar las rutas para la aplicación web
- Implementar el control de errores y registro de errores

## <a name="overview"></a>Información general

Si está familiarizado con formularios Web Forms de ASP.NET pero no se haya familiarizado con conceptos de programación, tendrá el tutorial derecho. Si ya está familiarizado con ASP.NET Web Forms, puede beneficiarse de esta serie de tutoriales por las nuevas características disponibles en ASP.NET 4.5. Si no está familiarizado con conceptos de programación y formularios Web Forms de ASP.NET, vea los tutoriales adicionales proporcionados en los formularios Web Forms [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.

Específico del **más reciente** características de ASP.NET 4.5 proporcionados en este formularios Web Forms serie de tutoriales incluye lo siguiente:

- Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y API Web).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un marco de trabajo de diseño y creación de temas que proporciona capacidades de diseño y temas con capacidad de respuesta.
- [ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de trabajo ASP.NET y funciona con software que no sean IIS de hospedaje web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de una actualización de Entity Framework que permite recuperar y manipular los datos como fuertemente tipados, acceder a los datos de forma asincrónica, controlen errores transitorios de conexión y registrar las instrucciones SQL.

Para obtener una lista completa de características de ASP.NET 4.5, consulte [ASP.NET y herramientas Web para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>La aplicación de ejemplo Wingtip Toys

Las capturas de pantalla siguientes proporcionan una vista rápida de la aplicación de formularios Web de ASP.NET que va a crear en esta serie de tutoriales. Cuando se ejecuta la aplicación desde Visual Studio Express 2013 para Web, verá la siguiente página de inicio de web.

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

Puede registrar como un nuevo usuario o inicie sesión como un usuario existente. Navegación se proporciona en la parte superior de cada categoría de producto mediante la recuperación de los productos disponibles de la base de datos.

Al seleccionar el vínculo de productos, podrá ver una lista de todos los productos disponibles.

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

También puede ver detalles de los productos individuales haciendo clic en cualquiera de los productos enumerados.

![Wingtip Toys - detalles de producto](introduction-and-overview/_static/image3.png)

Como un usuario, puede registrar e inicie sesión usando la función predeterminada de la plantilla de formularios Web Forms. Este tutorial también explica cómo iniciar sesión con una cuenta de Gmail existente. Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

Una vez que ha iniciado sesión como un usuario, puede agregar productos del carro de la compra y desprotección con PayPal. Tenga en cuenta que esta aplicación de ejemplo está diseñada para funcionar con el espacio aislado de desarrollador de PayPal. No hay ninguna transacción dinero real llevará a cabo.

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

PayPal confirmará tu cuenta, el orden y la información de pago.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Después de volver de PayPal, puede revisar y completar el pedido.

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene el siguiente software instalado en el equipo:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework se instala automáticamente.

Esta serie de tutoriales usa Microsoft Visual Studio Express 2013 para Web. Puede usar el modo de Microsoft Visual Studio Express 2013 para Web o Microsoft Visual Studio 2013 para completar esta serie de tutoriales.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se conocerán como Visual Studio a lo largo de esta serie de tutoriales.


Si ya tiene instalada una versión de Visual Studio, el proceso de instalación instalará Visual Studio 2013 o Microsoft Visual Studio Express 2013 para Web junto a la versión existente. Los sitios que crearon en versiones anteriores se pueden abrir en Visual Studio 2013 y continuarán abrir en versiones anteriores.

> [!NOTE] 
> 
> En este tutorial se da por supuesto que ha seleccionado la *desarrollo Web* colección de valores de la primera vez que inicia Visual Studio. Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Descargar la aplicación de ejemplo

Después de instalar los requisitos previos, está listo para empezar a crear el nuevo proyecto Web que se presenta en esta serie de tutoriales. Si le gustaría **opcionalmente** ejecutar la aplicación de ejemplo que crea esta serie de tutoriales, puede descargarlo desde el sitio de muestras de MSDN. Esta descarga contiene lo siguiente:

- La aplicación de ejemplo en el *WingtipToys* carpeta.
- Los recursos que usa para crear la aplicación de ejemplo en el *WingtipToys activos* carpeta en el *WingtipToys* carpeta.

#### <a name="download-the-file-from-msdn-samples-site"></a>Descargue el archivo desde el sitio de muestras de MSDN:

[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

La descarga es un *.zip* archivo. Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el *C#*carpeta en el *.zip* archivo. Guardar el *C#* carpeta a la carpeta usada para trabajar con proyectos de Visual Studio 2013. De forma predeterminada, la carpeta de proyectos de Visual Studio 2013 es la siguiente:

**C:\Users\*****&lt;username&gt;*****\Documents\Visual Studio 2013\Projects**

Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.

> [!NOTE]
> Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambiar temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.


Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo. Visual Studio 2013 se abrirá el proyecto. A continuación, haga clic en el *Default.aspx* un archivo en la ventana Explorador de soluciones y haga clic en ver en el explorador en el menú contextual.

### <a name="tutorial-support-and-comments"></a>Comentarios y soporte técnico de tutorial

Utilice la sección de preguntas y r incluida con el [Introducción a formularios Web Forms de ASP.NET 4.5 y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ejemplo (C#) para cualquier pregunta o enviar comentarios.

Comentarios en esta serie de tutoriales son bienvenidos y cuando se actualiza esta serie de tutoriales se realizará todo lo posible para tener en cuenta correcciones o sugerencias para mejoras en el que se proporcionan en los comentarios del tutoriales.

Cuando se produce un error durante el desarrollo, o si el sitio Web no se ejecuta correctamente, los mensajes de error pueden provocar pistas complejos para el origen del problema o no podrían explicar cómo corregirlo. Para ayudarle con algunos escenarios comunes de problema, también puede usar el [foros ASP.NET](https://forums.asp.net/) o en la sección de preguntas y r incluido con el [Introducción a formularios Web Forms de ASP.NET 4.5 y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ejemplo. Si aparece un mensaje de error o algo no funciona tal y como se vaya a través de los tutoriales, asegúrese de comprobar las ubicaciones anteriores.

>[!div class="step-by-step"]
[Siguiente](create-the-project.md)
