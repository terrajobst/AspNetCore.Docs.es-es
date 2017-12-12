---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Crear el proyecto | Documentos de Microsoft
author: Erikre
description: "Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET mediante ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 2678342891a87d591476a07e418c118b2ae94d4d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="create-the-project"></a>Crear el proyecto
====================
Por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar libros electrónicos (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales le enseñará los aspectos básicos de la creación de una aplicación de formularios Web Forms de ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible como acompañamiento de esta serie de tutoriales.


En este tutorial se crea, revisar y ejecutar el proyecto de forma predeterminada en Visual Studio, lo que le permitirá familiarizarse con las características de ASP.NET. Además, revisará el entorno de Visual Studio.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo crear un nuevo proyecto de formularios Web Forms.
- La estructura de archivos del proyecto de formularios Web Forms.
- Describe cómo ejecutar el proyecto en Visual Studio.
- Las distintas características de la aplicación de formularios Web de forma predeterminada.
- Algunos conceptos básicos acerca de cómo utilizar el entorno de Visual Studio.

## <a name="creating-the-project"></a>Crear el proyecto

1. Abra Visual Studio.
2. Seleccione **nuevo proyecto** desde el **archivo** menú en Visual Studio. 

    ![Crear el proyecto - nuevo elemento de menú de proyecto](create-the-project/_static/image1.png)
3. Seleccione el **plantillas**  - &gt; **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda.
4. Elija la **aplicación Web ASP.NET** plantilla en la columna central.  
 Esta serie de tutoriales está utilizando .NET Framework 4.5.2.
5. Denomine el proyecto *WingtipToys* y elija la **Aceptar** botón. 

    ![Crear el proyecto - cuadro de diálogo nuevo proyecto](create-the-project/_static/image2.png)

    > [!NOTE]
    > Es el nombre del proyecto en esta serie de tutoriales **WingtipToys**. Se recomienda que use esta *exacta* nombre del proyecto para que el código que proporciona a lo largo de la serie de tutoriales funciones según lo previsto.
6. A continuación, seleccione la **formularios Web Forms** plantilla y elija la **crear proyecto** botón.  

    ![Crear el proyecto - nueva plantilla de proyecto](create-the-project/_static/image3.png)

El proyecto tardará un poco en crear. Cuando esté listo, abra el **Default.aspx** página.

![Crear el proyecto - nueva plantilla de proyecto](create-the-project/_static/image4.png)

Puede cambiar entre **diseño** vista y **origen** vista seleccionando una opción en la parte inferior de la ventana central. **Diseño** vista muestra las páginas Web ASP.NET, páginas maestras, páginas de contenido, páginas HTML y controles de usuario mediante una vista de cerca WYSIWYG. **Origen** vista muestra el marcado HTML de la página Web, que se puede modificar.

> [!TIP] 
> 
> **Descripción de los marcos de trabajo ASP.NET**
> 
> Formularios Web Forms ASP.NET le permite crear sitios Web dinámicos con un modelo familiar de arrastrar y colocar, controlada por eventos. Una superficie de diseño y cientos de controles y componentes le permiten crear rápidamente sofisticados y eficaces sitios controlados por la interfaz de usuario con acceso a datos. El almacén de Wingtip Toys se basa en formularios Web Forms de ASP.NET, pero muchos de los conceptos que aprenderá en esta serie de tutoriales son aplicables a todos los de ASP.NET.
> 
> ASP.NET proporciona cuatro marcos de trabajo de desarrollo principal:
> 
> - [Formularios Web Forms ASP.NET](../../../index.md)  
>  El marco de trabajo de formularios Web Forms dirige a los programadores que prefieren la programación declarativa y basada en controles, como Microsoft Windows Forms (WinForms) y XAML de WPF y Silverlight. Ofrece un modelo de desarrollo controlado por el diseñador WYSIWYG, por lo que es popular con los desarrolladores que buscan un entorno de desarrollo (RAD) rápido de aplicaciones para el desarrollo web. Si está familiarizado con la programación web y está familiarizado con las herramientas de desarrollo de cliente de Microsoft RAD tradicionales (por ejemplo, para Visual Basic y Visual C#), puede crear rápidamente una aplicación web sin tener experiencia en HTML y JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md) (Más información sobre ASP.NET MVC)  
>  ASP.NET MVC se dirige a los desarrolladores interesados en patrones y principios como el desarrollo controlado por pruebas, la separación de aspectos, inversión de control (IoC) e inyección de dependencia (DI). Este marco anima a separar la capa de lógica empresarial de una aplicación web desde su nivel de presentación.
> - [ASP.NET Web Pages](../../../../web-pages/index.md) (Más información sobre páginas web de ASP.NET)  
>  Las páginas Web ASP.NET se dirige a los desarrolladores que deseen un caso de desarrollo web simple, a lo largo de las líneas de PHP. En el modelo de páginas Web, creación de páginas HTML y, a continuación, agregar código de servidor a la página con el fin de controlar cómo se representa ese marcado de forma dinámica. Páginas Web está diseñado específicamente para ser un marco de trabajo ligera, y es el punto de entrada más fácil a ASP.NET para personas que conocer el lenguaje HTML, pero no podrían tener una amplia experiencia de programación: por ejemplo, estudiantes o aficionados. También es un buen método para los desarrolladores web que sabe PHP o marcos de trabajo similar para empezar a usar ASP.NET.
> - [Aplicación de una página ASP.NET sencilla](../../../../single-page-application/index.md)  
>  Aplicación de una página ASP.NET única (SPA) le ayuda a crear aplicaciones que incluyen importantes interacciones del lado cliente mediante HTML 5, 3 de CSS y JavaScript. ASP.NET y herramientas, Web 2012.2 actualización incluye una nueva plantilla para la creación de aplicaciones de una página con knockout.js y ASP.NET Web API. Además de la nueva plantilla SPA, las nuevas plantillas SPA creados por la Comunidad también están disponibles para su descarga.
> 
> Además de los cuatro marcos de desarrollo principal, ASP.NET también ofrece las tecnologías adicionales que son importantes para tener en cuenta y familiarizado con, pero no se tratan en esta serie de tutoriales:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -un marco para crear servicios HTTP que llegan a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.
> - [ASP.NET SignalR](../../../../signalr/index.md) -una biblioteca que facilita la funcionalidad de desarrollo web en tiempo real.


### <a name="reviewing-the-project"></a>Revisar el proyecto

En Visual Studio, el **el Explorador de soluciones** ventana le permite administrar archivos para el proyecto. ¡Eche un vistazo a las carpetas que se han agregado a la aplicación en **el Explorador de soluciones**. La plantilla de aplicación web agrega una estructura de carpetas básico:

![Crear el proyecto: el Explorador de soluciones](create-the-project/_static/image5.png)

Visual Studio crea algunos iniciales carpetas y archivos para el proyecto. Los primeros archivos que va a trabajar con más adelante en este tutorial son los siguientes:

| **Archivo** | **Propósito** |
| --- | --- |
| *Default.aspx* | Normalmente la primera página que aparece cuando se ejecuta la aplicación en un explorador. |
| *Site.Master* | Una página que le permite crear un diseño y el uso estándar un comportamiento coherente para las páginas en la aplicación. |
| *Global.asax* | Un archivo opcional que contiene código para responder a eventos de nivel de aplicación y de nivel de sesión provocados por ASP.NET o por módulos HTTP. |
| *Web.config* | Los datos de configuración para una aplicación. |

### <a name="running-the-default-web-application"></a>Ejecuta la aplicación Web predeterminada

La aplicación Web predeterminada proporciona una experiencia enriquecida en función de compatibilidad y funcionalidad integrada. Sin realizar ningún cambio en el proyecto de formularios Web de forma predeterminada, la aplicación está lista para ejecutarse en el explorador Web local.

1. Presione el ***F5*** clave en Visual Studio.   
 La aplicación se compile y se mostrará en el explorador Web.  

    ![Crear el proyecto: el valor predeterminado de página](create-the-project/_static/image6.png)
2. Una vez haya completado revisar la aplicación en ejecución, cierre la ventana del explorador.

Hay tres páginas principales en esta aplicación Web de manera predeterminada: *Default.aspx* (inicio), *About.aspx*, y *Contact.aspx*. Cada una de estas páginas puede tener acceso desde la barra de navegación superior. También hay dos páginas adicionales incluidas en la carpeta de la cuenta, las páginas Register.aspx y Login.aspx. Estas dos páginas permiten utilizar las funciones de pertenencia de ASP.NET para crear, almacenar y validar las credenciales de usuario.

## <a name="aspnet-web-forms-background"></a>Fondo de ASP.NET Web Forms

Formularios Web Forms ASP.NET son páginas que se basan en tecnología de Microsoft ASP.NET, en que el código que se ejecuta en el servidor de forma dinámica genera resultados de la página Web en el explorador o dispositivo cliente. Una página de formularios Web Forms de ASP.NET representa automáticamente el código HTML adecuado al explorador para funciones tales como estilos, diseño y así sucesivamente. Formularios Web Forms son compatibles con cualquier lenguaje compatible con .NET common language runtime, como Microsoft Visual Basic y Microsoft Visual C#. Además, los formularios Web Forms se compilan en el [Microsoft .NET Framework](https://msdn.microsoft.com/en-US/vstudio/aa496123), que ofrece ventajas como un entorno administrado, seguridad de tipos y herencia.

Cuando se ejecuta una página de formularios Web Forms de ASP.NET, la página pasa por un ciclo de vida en el que realiza una serie de pasos de procesamiento. Estos pasos incluyen la inicialización, crear instancias de controles, restaurar y mantener el estado, ejecuta el código del controlador de eventos y la representación. A medida que se familiarice con la potencia de formularios Web Forms de ASP.NET, es importante que comprenda la [ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) para que pueda escribir código en la fase de ciclo de vida adecuado para el efecto deseado.

Cuando un servidor Web recibe una solicitud para una página, encuentra la página, lo procesa, lo envía al explorador y, a continuación, descarta toda la información de página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia, volver a procesar la página desde el principio. Dicho de otro modo, un servidor no tiene memoria de páginas que tiene páginas procesados no tienen estado. El marco de páginas ASP.NET controla automáticamente la tarea de mantener el estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación.

> [!TIP] 
> 
> **Características de la aplicación Web en la plantilla de aplicación de Web Forms**
> 
> La plantilla de aplicación de ASP.NET Web Forms proporciona un amplio conjunto de funcionalidades integradas. No solo ofrece a un *Home.aspx* página, un *About.aspx* página, un *Contact.aspx* página, pero también incluye la funcionalidad de pertenencia que registra los usuarios y guarda sus credenciales para que puede iniciar sesión en el sitio Web. Esta información general proporciona más información sobre algunas de las características incluidas en la plantilla de aplicación de ASP.NET Web Forms y cómo se utilizan en la aplicación de Wingtip Toys.
> 
> **Pertenencia**
> 
> [ASP.NET](https://msdn.microsoft.com/en-us/library/yh26yfzy.aspx) identidad almacena las credenciales de los usuarios en una base de datos creado por la aplicación. Cuando los usuarios inicien sesión, la aplicación valida las credenciales mediante la lectura de la base de datos. El proyecto *cuenta* carpeta contiene los archivos que implementan las distintas partes de pertenencia: registrar, inicio de sesión, cambiar una contraseña y autorizar el acceso. Además, los formularios Web Forms de ASP.NET admite OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios inicien sesión en el sitio mediante las credenciales existentes, de estas cuentas, como Facebook, Twitter, Windows Live, Google y.
> 
> ![Crear el proyecto: el Explorador de soluciones (identidad de ASP.NET)](create-the-project/_static/image7.png)
> 
> De forma predeterminada, la plantilla crea una base de datos de pertenencia con un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de base de datos de desarrollo que se incluye con Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) es una versión ligera de SQL Server que tiene muchas características de programación de una base de datos de SQL Server. SQL Server Express LocalDB se ejecuta en modo de usuario y tiene una instalación rápida sin configuración que tiene una lista breve de requisitos previos de instalación. En Microsoft SQL Server, cualquier base de datos o el código de Transact-SQL pueden transferirse de SQL Server Express LocalDB a SQL Server y SQL Azure sin ningún paso de actualización. Por lo tanto, SQL Server Express LocalDB puede usarse como un entorno de desarrollo para aplicaciones orientadas a todas las ediciones de SQL Server. SQL Server Express LocalDB habilita características como procedimientos almacenados, funciones definidas por el usuario y agregados, integración de .NET Framework, los tipos espaciales y otros usuarios que no están disponibles en SQL Server Compact.
> 
> **Páginas maestras**
> 
> Un [página principal de ASP.NET](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx) define una apariencia coherente y el comportamiento de todas las páginas de la aplicación. El diseño de la página maestra se combina con el contenido de una página de contenido individual para generar la última página que ve el usuario. En la aplicación Wingtip Toys, modifique la *Site.master* página principal para que todas las páginas en el sitio Web de Wingtip Toys comparten la misma barra de navegación y el logotipo de distintivo.
> 
> **HTML5**
> 
> La plantilla de aplicación de ASP.NET Web Forms admite [HTML5](http://www.w3schools.com/html/html5_intro.asp), que es la versión más reciente del lenguaje de marcado HTML. Es compatible con HTML5 nuevos elementos y funcionalidad que resulte más fácil crear sitios Web.
> 
> **Modernizr**
> 
> Para los exploradores que no son compatibles con HTML5, puede usar [Modernizr](http://www.modernizr.com/). Modernizr es una biblioteca de JavaScript de código abierto que puede detectar si un explorador es compatible con características de HTML5 y habilitarlas si no es así. En la plantilla de aplicación de ASP.NET Web Forms, Modernizr se instala como un paquete de NuGet.
> 
> **Bootstrap**
> 
> Usan las plantillas de proyecto de Visual Studio 2013 [arranque](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa diseños dinámicamente pueden adaptarse a los tamaños de ventana de explorador diferente. También puede utilizar la característica de temas del arranque para llevar a cabo fácilmente un cambio en la apariencia y funcionamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye Bootstrap como un paquete de NuGet.
> 
> **Paquetes de NuGet**
> 
> La plantilla de aplicación de ASP.NET Web Forms incluye un conjunto de [NuGet](http://www.nuget.org/) paquetes. Estos paquetes proporcionan funcionalidad dividida en componentes de la forma de herramientas y las bibliotecas de código abierto. Hay una gran variedad de paquetes para ayudarle a crear y probar las aplicaciones. Visual Studio resulta fácil agregar, quitar y actualizar paquetes de NuGet. Los programadores pueden crear y agregar paquetes a NuGet también.
> 
> ![Crear el proyecto - cuadro de diálogo de NuGet](create-the-project/_static/image8.png)
> 
> Cuando se instala un paquete, NuGet copia los archivos a la solución y realiza automáticamente los cambios que son necesarios, como agregar referencias y cambiar la configuración asociada a la aplicación Web. Si decide quitar la biblioteca, NuGet quita archivos e invierte los cambios efectuado en el proyecto para que no se deja desorden. NuGet está disponible desde el **herramientas** menú en Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) es una biblioteca de JavaScript rápida y concisa que simplifica atraviesa de documento HTML, control de eventos, animar e interacciones de Ajax para el desarrollo web rápido. La biblioteca de JavaScript de jQuery se incluye en la plantilla de aplicación de ASP.NET Web Forms como un paquete de NuGet.
> 
> **Validación discreto**
> 
> Controles de validación integradas se ha configurado para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce la cantidad de JavaScript que se representan en línea en el marcado de la página significativamente y reduce el tamaño de página general. Validación discreto se agrega globalmente a la plantilla de aplicación de ASP.NET Web Forms basándose en la configuración en el &lt;appSettings&gt; elemento de la *Web.config* archivo en la raíz de la aplicación.
> 
> **Entity Framework Code First**
> 
> Además de las características de la plantilla de aplicación de ASP.NET Web Forms, se utiliza la aplicación de Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que es una biblioteca de NuGet que permite el desarrollo centrado en el código cuando se trabaja con datos. En términos simples, crea la parte de la base de datos de su aplicación en función del código creado por usted. Con Entity Framework, recuperar y manipular los datos como objetos fuertemente tipados. Esto le permite centrarse en la lógica de negocios en la aplicación en lugar de los detalles de cómo se tiene acceso a datos.
> 
> Para obtener más información sobre las bibliotecas instaladas y paquetes incluidos con la plantilla de formularios Web Forms de ASP.NET, vea la lista de paquetes de NuGet instaladas. Para ello, en Visual Studio crea un nuevo proyecto de formularios Web Forms, seleccione **herramientas**  - &gt; **Administrador de paquetes de biblioteca**  - &gt; **administrar Paquetes de NuGet para la solución**y seleccione **paquetes instalados** en el **administrar paquetes de NuGet** cuadro de diálogo.


### <a name="touring-visual-studio"></a>Touring Visual Studio

Las ventanas principales en Visual Studio incluyen la **el Explorador de soluciones**, el **Explorador de servidores** (**el Explorador de base de datos** en Express), el **propiedades Ventana**, el **cuadro de herramientas**, **barra de herramientas**y el **ventana del documento**.

![Crear el proyecto - cuadro de diálogo de NuGet](create-the-project/_static/image9.png)

Para obtener más información sobre Visual Studio, vea [guía Visual para Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumen

En este tutorial ha creado, revisar y ejecutar la aplicación de formularios Web Forms de forma predeterminada. Ha revisado las distintas características de la aplicación de formularios Web predeterminada y conoce algunos conceptos básicos acerca de cómo utilizar el entorno de Visual Studio. En los siguientes tutoriales se creará la capa de acceso a datos.

## <a name="additional-resources"></a>Recursos adicionales

[Elegir la solución de modelo de programación](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Web Application Projects versus Web Site Projects](https://msdn.microsoft.com/en-us/library/dd547590.aspx)   
[Información general de las páginas de ASP.NET Web Forms](https://msdn.microsoft.com/en-us/library/428509ah.aspx)

>[!div class="step-by-step"]
[Anterior](introduction-and-overview.md)
[Siguiente](create_the_data_access_layer.md)
