---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
title: Introducción a ASP.NET 4.7 Web Forms y Visual Studio 2017 | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales paso a paso le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante Microsoft Visual Studio y ASP.NET 4.7
ms.author: riande
ms.date: 01/09/2019
ms.assetid: 9b96eaa1-8ef0-4338-a2e8-e0f970bfaf68
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview
msc.type: authoredcontent
ms.openlocfilehash: fb41ce72e9454d8d670a0b95234d2bc3f909f0ee
ms.sourcegitcommit: 42a8164b8aba21f322ffefacb92301bdfb4d3c2d
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/16/2019
ms.locfileid: "54341558"
---
<a name="getting-started-with-aspnet-45-web-forms-and-visual-studio-2017"></a>Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2017
====================

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

Esta serie de tutoriales muestra cómo crear una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio 2017. 

## <a name="introduction"></a>Introducción

Esta serie de tutoriales le guiará en el proceso de creación de una aplicación de formularios Web Forms de ASP.NET mediante Visual Studio 2017 y ASP.NET 4.5. Se creará una aplicación denominada **Wingtip Toys** : un sitio web escaparate simplificada de venta de artículos en línea. Durante la serie, se resaltan las nuevas características de ASP.NET 4.5.

### <a name="target-audience"></a>Audiencia de destino

Los programadores nuevos en ASP.NET Web Forms son la audiencia objetivo de esta serie de tutoriales.

Debe tener algunos conocimientos en las áreas siguientes:

- Programación orientada a objetos (OOP) e idiomas
- Desarrollo Web (HTML, CSS, JavaScript)
- bases de datos relacionales
- Arquitectura de N niveles

Para revisar estas áreas, considere la posibilidad de estudiar el siguiente contenido:

- [Introducción a Visual C#](https://msdn.microsoft.com/library/a72418yk.aspx)
- [Desarrollo Web](https://msdn.microsoft.com/beginner/bb308760.aspx), [HTML, CSS, JavaScript, SQL, PHP, JQuery](http://w3schools.com/)
- [Base de datos relacional](http://en.wikipedia.org/wiki/Relational_database)
- [Arquitectura de varios niveles](http://en.wikipedia.org/wiki/Multitier_architecture)

### <a name="application-features"></a>Características de la aplicación

Las características de formulario Web Forms de ASP.NET presentadas en esta serie incluyen:

- El proyecto de aplicación Web (no el proyecto de sitio Web)
- Formularios Web Forms
- Páginas maestras, configuración
- Bootstrap
- Entity Framework Code First, LocalDB
- La validación de solicitudes
- Controles de datos fuertemente tipados
- Enlace de modelos
- Anotaciones de datos
- Proveedores de valor
- SSL y OAuth
- ASP.NET Identity, configuración y la autorización
- Validación discreta
- Enrutamiento
- Control de errores de ASP.NET

### <a name="application-scenarios-and-tasks"></a>Tareas y escenarios de aplicación

Las tareas de la serie de tutoriales incluyen:

- Crear, revisar y ejecutar un proyecto nuevo
- Creación de una estructura de base de datos
- Inicializar y la propagación de una base de datos
- Personalizar la interfaz de usuario con los estilos, gráficos y una página maestra
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

Esta serie de tutoriales es está diseñado para un usuario familiarizado con conceptos de programación, pero es nuevo en ASP.NET Web Forms. Si ya está familiarizado con ASP.NET Web Forms, esta serie todavía puede ayudar a obtener información sobre las nuevas características de ASP.NET 4.5. Para los lectores familiarizados con conceptos de programación y formularios Web Forms de ASP.NET, consulte los tutoriales de formularios Web Forms adicionales proporcionados en el [Introducción](../../../index.md) sección en el sitio Web de ASP.NET.

El proporcionado en esta serie de tutoriales de ASP.NET 4.5 incluye las siguientes características:

- Una interfaz de usuario simple para crear proyectos que ofrece [compatibilidad para muchos marcos ASP.NET](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#add) (formularios Web Forms, MVC y Web API).
- [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap), un diseño, creación de temas y marco de diseño con capacidad de respuesta.
- [ASP.NET Identity](../../../../identity/index.md), un nuevo sistema de pertenencia ASP.NET que funciona igual en todos los marcos de ASP.NET y funciona con el software que no sean IIS de hospedaje web.
- [Entity Framework 6](https://msdn.microsoft.com/data/ef.aspx)

  Una actualización de Entity Framework, lo que le permite:
  - Recuperar y manipular datos como objetos fuertemente tipados
  - Obtener acceso a datos de forma asincrónica
  - Controlar errores transitorios de conexión
  - Instrucciones de registro SQL

Para obtener una lista completa de característica ASP.NET 4.5, vea [ASP.NET and Web Tools para Visual Studio 2013 notas](../../../../visual-studio/overview/2013/release-notes.md).

### <a name="the-wingtip-toys-sample-application"></a>La aplicación de ejemplo Wingtip Toys

Son las siguientes capturas de pantalla de la aplicación de formularios Web Forms de ASP.NET que se crea en esta serie de tutoriales. Al ejecutar la aplicación en Visual Studio, aparece la siguiente página principal de web.

![Wingtip Toys - página predeterminada](introduction-and-overview/_static/image1.png)

Puede registrar como un nuevo usuario, o inicie sesión como un usuario existente. El panel de navegación superior tiene vínculos a las categorías de productos y sus productos de la base de datos.

Si selecciona **productos**, se muestran todos los productos disponibles. 

![Wingtip Toys - productos](introduction-and-overview/_static/image2.png)

Si selecciona un producto específico, se muestran detalles del producto.


![Wingtip Toys - detalles del producto](introduction-and-overview/_static/image3.png)

Como usuario, puede registrar e inicie sesión con la funcionalidad de formularios Web Forms plantilla predeterminada. En este tutorial también se explica cómo iniciar sesión con una cuenta de Gmail existente. Además, puede iniciar sesión como administrador para agregar y quitar los productos de la base de datos.

![Wingtip Toys - inicio de sesión](introduction-and-overview/_static/image4.png)

Una vez que haya iniciado sesión como un usuario, puede agregar productos al carro de la compra y desprotección con PayPal. La aplicación de ejemplo está diseñada para trabajar en espacio aislado de desarrollador de PayPal. No hay ninguna transacción dinero real realiza.

![Wingtip Toys - carro de la compra](introduction-and-overview/_static/image5.png)

PayPal confirma su información de cuenta, el orden y el pago.

![Wingtip Toys - PayPal](introduction-and-overview/_static/image6.png)

Después de volver de PayPal, puede revisar y completar su pedido.

![Wingtip Toys - revisión de pedidos](introduction-and-overview/_static/image7.png)

## <a name="prerequisites"></a>Requisitos previos

Antes de empezar, asegúrese de que está instalado el software siguiente en el equipo:

- [Microsoft Visual Studio 2017 o Microsoft Visual Studio Community 2017](https://visualstudio.microsoft.com/downloads/).

.NET Framework se instala automáticamente.

Esta serie de tutoriales usa Microsoft Visual Studio Community 2017. Puede usar cualquier que o Microsoft Visual Studio 2017 para completar esta serie de tutoriales.

Tenga en cuenta lo siguiente acerca de Visual Studio:

* Microsoft Visual Studio 2017 y Microsoft Visual Studio Community 2017 se conocen como *Visual Studio* a lo largo de esta serie de tutoriales.

* Visual Studio 2017 se instala junto a las versiones anteriores ya instaladas. Los sitios creados en versiones anteriores se pueden abrir en Visual Studio 2017 y continuarán para abrir en versiones anteriores.

* La primera vez inicia Visual Studio, se supone que ha seleccionado la *desarrollo Web* configuración. Para obtener más información, vea [Cómo: Seleccione la configuración del entorno de desarrollo Web](https://msdn.microsoft.com/library/ff521558.aspx).

Después de instalar los requisitos previos, está listo para empezar a crear el proyecto Web que se presentan en esta serie de tutoriales.

## <a name="download-the-sample-application"></a>Descargue la aplicación de ejemplo

 Puede descargar el applicatiion ejemplo completado en cualquier momento desde el sitio de ejemplos de MSDN:

[Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) 

 Esta descarga contiene los elementos siguientes:

- La aplicación de ejemplo en el *WingtipToys* carpeta.
- Los recursos utilizados para crear la aplicación de ejemplo en el *WingtipToys-activos* carpeta en el *WingtipToys* carpeta.

La descarga es un *.zip* archivo. Para ver el proyecto completado que crea esta serie de tutoriales, busque y seleccione el *C#* carpeta en el archivo zip. Guardar el C# a la carpeta que se utiliza para trabajar con proyectos de Visual Studio. De forma predeterminada, es la carpeta de proyectos de Visual Studio 2017:

<strong>C:\Users&#92;</strong><strong><em>&lt;username&gt;</em></strong><strong>\source\repos</strong>

Cambiar el nombre de la ***C#*** carpeta ***WingtipToys***.

> [!NOTE]
> Si ya tiene una carpeta denominada *WingtipToys* en la carpeta de proyectos, cambie temporalmente el nombre esa carpeta existente antes de cambiar el nombre de la *C#* carpeta *WingtipToys*.

Para ejecutar el proyecto completado, abra el *WingtipToys* carpeta y haga doble clic en el *WingtipToys.sln* archivo. Visual Studio 2017 abre el proyecto. A continuación, haga clic en el *Default.aspx* archivo **el Explorador de soluciones** y seleccione **ver en el explorador**.

## <a name="take-a-aspnet-web-forms-quiz-to-review-content"></a>Cuestionario de formularios Web Forms ASP.NET para revisar el contenido

Después de completar la serie de tutoriales, cuestionario a prueba sus conocimientos y reforzar los conceptos clave. Cada pregunta proporciona una explicación y vínculos a guías adicionales.

 * [Cuestionario de ASP.NET Web Forms](https://blogs.msdn.microsoft.com/erikreitan/2016/01/08/asp-net-web-forms-quiz/) 

## <a name="tutorial-support-and-comments"></a>Comentarios y soporte técnico de tutorial

Para preguntas y comentarios, use la sección de preguntas y respuestas que se incluye en el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) página de ejemplo.

Comentarios en esta serie de tutoriales son bienvenidos. Cuando se actualiza esta serie de tutoriales, se realizan todos los esfuerzos que tener en cuenta las correcciones o sugerencias para mejorar.

Si se produce un error, los mensajes de error correspondiente podrían resultar confusos, sin buena explicación sobre cómo corregirlo. Para obtener ayuda, puede comprobar el [foros de ASP.NET](https://forums.asp.net/). Otra buena fuente es la sección de preguntas y respuestas en el [Introducción a ASP.NET 4.5 Web Forms y Visual Studio 2013 - Wingtip Toys](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) (C#) página de ejemplo. 

> [!div class="step-by-step"]
> [Siguiente](create-the-project.md)
