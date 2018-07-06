---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express...
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: cda0c6239e2f10186641ab315837440d83b2fda6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841401"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2013"></a>Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. [Cuestionario de ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET)  
> Ponga a prueba sus conocimientos y reforzar los conceptos clave tomando el examen de ASP.NET Web Forms. Este examen se diseñó específicamente desde el contenido de esta serie de tutoriales. Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.


## <a name="introduction"></a>Introducción

Esta serie de tutoriales le guiará por los pasos necesarios para crear una aplicación de formularios Web Forms de ASP.NET con Visual Studio Express 2013 para Web y ASP.NET 4.5.

La aplicación que se va a crear se denomina **Wingtip Toys**. Es un ejemplo simplificado de un sitio web de front-store que vende productos en línea. Esta serie de tutoriales resalta las nuevas características disponibles en ASP.NET 4.5.

Los comentarios son bienvenidos y haremos todo lo posible para actualizar esta serie de tutoriales en función de sus sugerencias.

### <a name="download-completed-project"></a>Proyecto de descarga completada

Puede descargar un proyecto de C# que contiene el tutorial completado.

- [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#)

### <a name="review-the-content-by-taking-the-related-aspnet-web-forms-quiz"></a>Revise el contenido realizando el examen de formularios Web Forms ASP.NET relacionado

Después de completar este tutorial, ponga a prueba sus conocimientos y reforzar los conceptos clave tomando el [ASP.NET Web Forms Quiz](http://quizapp.cloudapp.net/?quiz=ASP.NET&cdn_id=2014-01-17-001). Este examen se diseñó específicamente desde el contenido de esta serie de tutoriales. Cada pregunta de la prueba proporciona una explicación junto con vínculos a guías adicionales.

- [Cuestionario de ASP.NET Web Forms](http://quizapp.cloudapp.net/?quiz=ASP.NET)

### <a name="audience"></a>Audiencia

Los destinatarios de esta serie de tutoriales son que los desarrolladores con experiencia que están familiarizados con ASP.NET Web Forms. Un desarrollador interesado en esta serie de tutoriales debe tener los conocimientos siguientes:

- Está familiarizado con un objeto orientada a servicios de lenguaje de programación (OOP)
- Está familiarizado con conceptos de desarrollo Web (HTML, CSS, JavaScript)
- Está familiarizado con conceptos de base de datos relacional
- Está familiarizado con conceptos de arquitectura de n niveles

Si está interesado en la revisión de los problemas descritos anteriormente, considere la posibilidad de revisar el contenido siguiente:

- [Introducción a Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitectura de varios niveles](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Características de la aplicación

Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:

- El proyecto de aplicación Web (no el proyecto de sitio Web)
- formularios Web Forms
- Páginas maestras, configuración
- Bootstrap
- Entity Framework Code First, LocalDB
- La validación de solicitudes
- Fuertemente tipadas en controles de datos, enlace, las anotaciones de datos de modelos y los proveedores de valor
- SSL y OAuth
- ASP.NET Identity, configuración y la autorización
- Validación discreta
- Enrutamiento
- Control de errores de ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tareas y escenarios de aplicación

Las tareas que se muestra en esta serie incluyen:

- Crear, revisar y ejecutar el proyecto nuevo
- Creación de la estructura de base de datos
- Inicializar y la propagación de la base de datos
- Personalizar la interfaz de usuario mediante los estilos, gráficos y una página maestra
- Agregar páginas y navegación
- Mostrar detalles de menú y los datos de productos
- Creación de un carro de la compra
- Compatibilidad con SSL agregando y OAuth
- Agregar un método de pago
- Como un rol de administrador y un usuario a la aplicación
- Restringir el acceso a páginas específicas y carpeta
- Cargar un archivo a la aplicación web
- Implementar la validación de entrada
- Registrar las rutas para la aplicación web
- Implementación de control de errores y registro de errores

## <a name="overview"></a>Información general

Si está familiarizado con ASP.NET Web Forms, pero están familiarizados con conceptos de programación, tiene el tutorial adecuado. Si ya está familiarizado con ASP.NET Web Forms, puede beneficiarse de esta serie de tutoriales por las nuevas características disponibles en ASP.NET 4.5. Si no está familiarizado con conceptos de programación y formularios Web Forms de ASP.NET, consulte los tutoriales adicionales proporcionados en los formularios Web Forms [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.

Específico del **más reciente** ASP.NET 4.5 características proporcionadas en este formularios Web Forms de serie de tutoriales incluye lo siguiente:

- Una interfaz de usuario simple para crear proyectos de dicha oferta [admite varios marcos de ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y Web API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un marco de diseño y creación de temas que proporciona capacidades de diseño y creación de temas con capacidad de respuesta.
- [ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con el software que no sean IIS de hospedaje web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx), objetos de una actualización de Entity Framework que permite recuperar y manipular los datos como fuertemente tipados, acceder a los datos de forma asincrónica, controlan errores transitorios de conexión y registrar las instrucciones SQL.

Para obtener una lista completa de las características de ASP.NET 4.5, vea [ASP.NET and Web Tools para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>La aplicación de ejemplo Wingtip Toys

Las capturas de pantalla siguientes proporcionan una vista rápida de la aplicación de formularios Web ASP.NET que va a crear en esta serie de tutoriales. Cuando se ejecuta la aplicación desde Visual Studio Express 2013 para Web, verá la siguiente página principal de web.

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

Puede registrar como un nuevo usuario, o inicie sesión como un usuario existente. Navegación se proporciona en la parte superior de cada categoría de producto mediante la recuperación de los productos disponibles de la base de datos.

Al seleccionar el vínculo de productos, podrá ver una lista de todos los productos disponibles.

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

También puede ver los detalles de los productos individuales haciendo clic en cualquiera de los productos enumerados.

![Wingtip Toys - detalles del producto](introduction-and-overview/_static/image3.png)

Como usuario, puede registrar e inicie sesión con la funcionalidad predeterminada de la plantilla de formularios Web Forms. En este tutorial también se explica cómo iniciar sesión con una cuenta de Gmail. Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

Una vez que haya iniciado sesión como un usuario, puede agregar productos al carro de la compra y desprotección con PayPal. Tenga en cuenta que esta aplicación de ejemplo está diseñada para funcionar con el espacio aislado de desarrollador de PayPal. No hay ninguna transacción real dinero llevará a cabo.

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

PayPal confirmará su cuenta, el orden y la información de pago.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Después de volver de PayPal, puede revisar y completar su pedido.

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que tiene el siguiente software instalado en el equipo:

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs) o [Microsoft Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/11/downloads#express-web). .NET Framework se instala automáticamente.

Esta serie de tutoriales usa Microsoft Visual Studio Express 2013 para Web. Puede usar en Microsoft Visual Studio Express 2013 para Web o Microsoft Visual Studio 2013 para completar esta serie de tutoriales.

> [!NOTE] 
> 
> Microsoft Visual Studio 2013 y Microsoft Visual Studio Express 2013 para Web a menudo se hará referencia a que Visual Studio a lo largo de esta serie de tutoriales.


Si ya tiene instalada una versión de Visual Studio, el proceso de instalación instalará Microsoft Visual Studio Express 2013 para Web o Visual Studio 2013 junto a la versión existente. Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2013 y continuarán para abrir en versiones anteriores.

> [!NOTE] 
> 
> En este tutorial se supone que seleccionó el *desarrollo Web* colección de valores de la primera vez que inicia Visual Studio. Para obtener más información, consulte [Cómo: seleccionar configuración de entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).


## <a name="download-the-sample-application"></a>Descargue la aplicación de ejemplo

Después de instalar los requisitos previos, está listo para empezar a crear el nuevo proyecto Web que se presenta en esta serie de tutoriales. Si le gustaría **opcionalmente** ejecutar la aplicación de ejemplo que crea esta serie de tutoriales, puede descargarlo desde el sitio de ejemplos de MSDN. Esta descarga contiene lo siguiente:

- La aplicación de ejemplo en el *WingtipToys* carpeta.
- Los recursos utilizados para crear la aplicación de ejemplo en el *WingtipToys-activos* carpeta en el *WingtipToys* carpeta.

#### <a name="download-the-file-from-msdn-samples-site"></a>Descargue el archivo desde el sitio de ejemplos de MSDN:

[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

La descarga es un <em>.zip</em> archivo. Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el <em>C#</em>carpeta en el <em>.zip</em> archivo. Guardar el <em>C#</em> carpeta a la carpeta que se utiliza para trabajar con proyectos de Visual Studio 2013. De forma predeterminada, la carpeta de proyectos de Visual Studio 2013 es la siguiente:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\Documents\Visual Studio 2013\Projects</strong>

Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.

> [!NOTE]
> Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.


Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo. Visual Studio 2013 se abrirá el proyecto. A continuación, haga clic en el *Default.aspx* de archivos en la ventana Explorador de soluciones y haga clic en ver en el explorador en el menú contextual.

### <a name="tutorial-support-and-comments"></a>Comentarios y soporte técnico de tutorial

Use la sección de preguntas y respuestas incluida con el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) ejemplo (C#) para cualquier pregunta o comentario.

Comentarios en esta serie de tutoriales son bienvenidos y, cuando se actualiza esta serie de tutoriales se realizarán todos los esfuerzos para tener en cuenta correcciones o sugerencias de mejoras que se proporcionan en los comentarios del tutoriales.

Cuando se produce un error durante el desarrollo, o si el sitio Web no se ejecuta correctamente, los mensajes de error pueden dar pistas complejos en el origen del problema o no es posible que se explica cómo corregirlo. Para ayudarle con algunos escenarios comunes de problema, también puede usar el [foros de ASP.NET](https://forums.asp.net/) o en la sección de preguntas y respuestas incluido con el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) () C#) ejemplo. Si recibe un mensaje de error o algo no funciona al avanzar por los tutoriales, asegúrese de comprobar las ubicaciones anteriores.

> [!div class="step-by-step"]
> [Siguiente](create-the-project.md)
