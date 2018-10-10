---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
title: Crear el proyecto | Microsoft Docs
author: Erikre
description: Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para se...
ms.author: riande
ms.date: 09/08/2014
ms.assetid: 2ce36f78-8ecb-4ab1-b748-6d0ab633ea3f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/create-the-project
msc.type: authoredcontent
ms.openlocfilehash: 754f085e3e43f7efa155f410d02a0d29d3349612
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912298"
---
<a name="create-the-project"></a>Crear el proyecto
====================
por [Erik Reitan](https://github.com/Erikre)

[Descargar el proyecto de ejemplo de Wingtip Toys (C#)](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) o [descargar eBook (PDF)](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> Esta serie de tutoriales aprenderá los conceptos básicos de la creación de una aplicación de formularios Web Forms ASP.NET con ASP.NET 4.5 y Microsoft Visual Studio Express 2013 para Web. Un Visual Studio 2013 [proyecto con código fuente de C#](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409) está disponible para acompañar esta serie de tutoriales.


En este tutorial se crea, revise y ejecute el proyecto de forma predeterminada en Visual Studio, que le permitirán familiarizarse con las características de ASP.NET. Además, revisará el entorno de Visual Studio.

## <a name="what-youll-learn"></a>Lo que aprenderá:

- Cómo crear un nuevo proyecto de formularios Web Forms.
- La estructura de archivos del proyecto de formularios Web Forms.
- Cómo ejecutar el proyecto en Visual Studio.
- Las diferentes características de la aplicación de formularios Web de forma predeterminada.
- Algunos conceptos básicos sobre cómo usar el entorno de Visual Studio.

## <a name="creating-the-project"></a>Crear el proyecto

1. Abra Visual Studio.
2. Seleccione **nuevo proyecto** desde el **archivo** menú en Visual Studio. 

    ![Crear el proyecto: nuevo elemento de menú de proyecto](create-the-project/_static/image1.png)
3. Seleccione el **plantillas**  - &gt; **Visual C#**  - &gt; **Web** grupo de plantillas de la izquierda.
4. Elija la **aplicación Web ASP.NET** plantilla en la columna central.  
 Esta serie de tutoriales usa .NET Framework 4.5.2.
5. Nombre del proyecto *WingtipToys* y elija el **Aceptar** botón. 

    ![Crear el proyecto: cuadro de diálogo nuevo proyecto](create-the-project/_static/image2.png)

    > [!NOTE]
    > El nombre del proyecto en esta serie de tutoriales es **WingtipToys**. Se recomienda que use esta *exacta* nombre del proyecto para que el código proporcionado en la serie de tutoriales funciona según lo esperado.

6. Haga clic en el **Cambiar autenticación** botón. Seleccione **cuentas de usuario individuales** y haga clic en el **Aceptar** botón.

7. Seleccione el **formularios Web Forms** plantilla y haga clic en el **Aceptar** botón.

    ![Crear nueva plantilla de proyecto de proyecto:](create-the-project/_static/image3.png)

El proyecto tardará un poco tiempo en crear. Cuando esté listo, abra el **Default.aspx** página.

![Crear nueva plantilla de proyecto de proyecto:](create-the-project/_static/image4.png)

Puede cambiar entre **diseño** vista y **origen** vista seleccionando una opción en la parte inferior de la ventana del centro. **Diseño** vista muestra las páginas Web ASP.NET, las páginas maestras, páginas de contenido, páginas HTML y controles de usuario mediante una vista bastante cerca al modo WYSIWYG. **Origen** vista muestra el marcado HTML para la página Web, que se puede modificar.

> [!TIP] 
> 
> **Descripción de los marcos de ASP.NET**
> 
> Formularios Web Forms ASP.NET le permite crear sitios Web dinámicos con un modelo conocido de arrastrar y colocar, controlado por eventos. Una superficie de diseño y cientos de controles y componentes le permiten crear rápidamente sofisticados y eficaces sitios controlados por la interfaz de usuario con acceso a datos. El Store de Wingtip Toys se basa en ASP.NET Web Forms, pero muchos de los conceptos que aprenderá en esta serie de tutoriales son aplicables a todos los de ASP.NET.
> 
> ASP.NET ofrece cuatro marcos de desarrollo principal:
> 
> - [Formularios Web Forms ASP.NET](../../../index.md)  
>  El marco de trabajo de formularios Web Forms dirige a los desarrolladores que prefieren la programación declarativa y basada en control, como Microsoft Windows Forms (WinForms) y XAML de WPF o Silverlight. Ofrece un modelo de desarrollo controlado por el diseñador WYSIWYG, por lo que es popular con los desarrolladores que buscan un entorno de desarrollo (RAD) rápido de aplicaciones para el desarrollo web. Si está familiarizado con la programación web y está familiarizado con las herramientas de desarrollo de cliente de Microsoft RAD tradicionales (por ejemplo, para Visual Basic y Visual C#), puede crear rápidamente una aplicación web sin tener experiencia en HTML y JavaScript.
> - [ASP.NET MVC](../../../../mvc/index.md) (Más información sobre ASP.NET MVC)  
>  ASP.NET MVC se dirige a los desarrolladores interesados en patrones y principios, como desarrollo controlado por pruebas, la separación de preocupaciones, inversión de control (IoC) y la inserción de dependencias (DI). Este marco anima a separar la capa de lógica de negocios de una aplicación web de su capa de presentación.
> - [ASP.NET Web Pages](../../../../web-pages/index.md) (Más información sobre páginas web de ASP.NET)  
>  Páginas Web de ASP.NET se dirige a los desarrolladores que desean una historia de desarrollo web simple, a lo largo de las líneas de PHP. En el modelo de páginas Web, crear páginas HTML y, a continuación, agregar código de servidor a la página con el fin de controlar cómo se procesa ese marcado de forma dinámica. Páginas Web está diseñado específicamente para ser un marco ligero, y es el punto de entrada más sencillo a ASP.NET para las personas que conoce HTML, pero no podrían tener amplia experiencia de programación: por ejemplo, los estudiantes o aficionados. También es una buena manera para los desarrolladores web que saben PHP o entornos similares para empezar a usar ASP.NET.
> - [Aplicación de página única de ASP.NET](../../../../single-page-application/index.md)  
>  Aplicación de una página ASP.NET única (SPA) le ayuda a crear aplicaciones que incluyen significativo interacciones del lado cliente con HTML 5, 3 de CSS y JavaScript. ASP.NET y Web Tools 2012.2 Update se incluye una nueva plantilla para crear aplicaciones de página única mediante knockout.js y ASP.NET Web API. Además de la nueva plantilla SPA, nuevas plantillas SPA creados por la Comunidad también están disponibles para su descarga.
> 
> Además de los cuatro marcos de desarrollo principal, ASP.NET ofrece también otras tecnologías que son importantes para ser consciente de y está familiarizado con, pero no se tratan en esta serie de tutoriales:
> 
> - [ASP.NET Web API](../../../../web-api/index.md) -un marco para crear servicios HTTP que lleguen a una amplia gama de clientes, incluidos los exploradores y dispositivos móviles.
> - [ASP.NET SignalR](../../../../signalr/index.md) -una biblioteca que simplifica la funcionalidad de desarrollo web en tiempo real.


### <a name="reviewing-the-project"></a>Revisar el proyecto

En Visual Studio, el **el Explorador de soluciones** ventana le permite administrar los archivos del proyecto. Echemos un vistazo a las carpetas que se han agregado a la aplicación en **el Explorador de soluciones**. La plantilla de aplicación web agrega una estructura de carpetas básica:

![Crear el proyecto - Explorador de soluciones](create-the-project/_static/image5.png)

Visual Studio crea algunos iniciales carpetas y archivos para el proyecto. Los primeros archivos que trabajará con más adelante en este tutorial son los siguientes:

| **Archivo** | **Propósito** |
| --- | --- |
| *Default.aspx* | Normalmente, la primera página que aparece cuando la aplicación se ejecuta en un explorador. |
| *Site.Master* | Una página que le permite crear un diseño y el uso estándar un comportamiento coherente para las páginas en la aplicación. |
| *Global.asax* | Un archivo opcional que contiene código para responder a eventos de nivel de sesión y el nivel de aplicación provocados por ASP.NET o por los módulos HTTP. |
| *Web.config* | Los datos de configuración para una aplicación. |

### <a name="running-the-default-web-application"></a>Ejecuta la aplicación Web predeterminada

La aplicación Web predeterminada proporciona una experiencia en función de soporte técnico y funcionalidad integrada. Sin realizar ningún cambio en el proyecto de formularios Web de forma predeterminada, la aplicación está lista para ejecutarse en el explorador Web local.

1. Presione el ***F5*** clave mientras en Visual Studio.   
 La aplicación se compilará y se mostrará en el explorador Web.  

    ![Crear el proyecto: la página predeterminada](create-the-project/_static/image6.png)
2. Una vez que una revisión completa de la aplicación en ejecución, cierre la ventana del explorador.

Hay tres páginas principales en esta aplicación Web de forma predeterminada: *Default.aspx* (inicio), *About.aspx*, y *Contact.aspx*. Cada una de estas páginas se puede acceder desde la barra de navegación superior. También hay dos páginas adicionales contenidas en la carpeta de la cuenta, la página Register.aspx y la página Login.aspx. Estas páginas permiten usar las capacidades de pertenencia de ASP.NET para crear, almacenar y validar las credenciales de usuario.

## <a name="aspnet-web-forms-background"></a>Formularios Web Forms ASP.NET en segundo plano

ASP.NET Web Forms son páginas que se basan en la tecnología de Microsoft ASP.NET, en que el código que se ejecuta en el servidor de forma dinámica genera resultados de la página Web en el explorador o dispositivo cliente. Una página de formularios Web Forms ASP.NET representa de forma automática el código HTML adecuado al explorador para características como estilos, diseño y así sucesivamente. Formularios Web Forms son compatibles con cualquier lenguaje compatible con .NET common language runtime, como Microsoft Visual Basic y Microsoft Visual C#. Además, los formularios Web Forms se basan en el [Microsoft .NET Framework](https://msdn.microsoft.com/vstudio/aa496123), que ofrece ventajas como un entorno administrado, seguridad de tipos y herencia.

Cuando se ejecuta una página de formularios Web Forms de ASP.NET, la página pasa por un ciclo de vida en el que realiza una serie de pasos de procesamiento. Estos pasos incluyen la inicialización, crear instancias de controles, restauración y mantenimiento del estado, código de controlador de eventos de ejecución y la representación. A medida que se familiarice con la eficacia de formularios Web Forms de ASP.NET, es importante que comprenda el [ciclo de vida de página ASP.NET](https://msdn.microsoft.com/library/ms178472(v=vs.100).aspx) por lo que puede escribir código en la fase de ciclo de vida adecuado para el efecto deseado.

Cuando un servidor Web recibe una solicitud para una página, encuentra la página, lo procesa, lo envía al explorador y, a continuación, se descarta toda la información de página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia, volver a procesar la página desde el principio. Dicho de otro modo, un servidor no tiene memoria de páginas que tiene páginas procesados son sin estado. El marco de páginas ASP.NET controla automáticamente la tarea de mantenimiento del estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación.

> [!TIP] 
> 
> **Características de la aplicación Web en la plantilla de aplicación de formularios Web**
> 
> La plantilla de aplicación de ASP.NET Web Forms proporciona un amplio conjunto de funcionalidades integradas. No sólo le proporciona un *Home.aspx* página, un *About.aspx* página, un *Contact.aspx* página, pero también incluye la funcionalidad de pertenencia que registra los usuarios y los guarda sus credenciales para que inicien sesión en su sitio Web. Esta introducción proporciona más información sobre algunas de las características incluidas en la plantilla de aplicación de ASP.NET Web Forms y cómo se usan en la aplicación Wingtip Toys.
> 
> **Pertenencia**
> 
> [ASP.NET](https://msdn.microsoft.com/library/yh26yfzy.aspx) identidad almacena las credenciales de los usuarios en una base de datos creado por la aplicación. Cuando los usuarios inicien sesión, la aplicación valida sus credenciales mediante la lectura de la base de datos. El proyecto *cuenta* carpeta contiene los archivos que implementan las distintas partes de pertenencia: registro, iniciar sesión, cambiar una contraseña y autorizar el acceso. Además, formularios Web Forms de ASP.NET es compatible con OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios inicien sesión en el sitio con las credenciales existentes, desde estas cuentas, como Facebook, Twitter, Windows Live y Google.
> 
> ![Crear el proyecto - Explorador de soluciones (identidad de ASP.NET)](create-the-project/_static/image7.png)
> 
> De forma predeterminada, la plantilla crea una base de datos de pertenencia mediante un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de desarrollo de base de datos que se incluye con Visual Studio Express 2013 para Web.
> 
> **SQL Server Express LocalDB**
> 
> [SQL Server Express LocalDB](https://technet.microsoft.com/library/hh510202.aspx) es una versión ligera de SQL Server que tiene muchas características de programación de una base de datos de SQL Server. SQL Server Express LocalDB se ejecuta en modo de usuario y tiene una instalación rápida sin configuración que tiene una lista breve de requisitos previos de instalación. En Microsoft SQL Server, cualquier base de datos o código de Transact-SQL se puede mover desde SQL Server Express LocalDB a SQL Server y SQL Azure sin ningún paso de actualización. Por lo tanto, SQL Server Express LocalDB puede usarse como entorno de desarrollo para las aplicaciones destinadas a todas las ediciones de SQL Server. SQL Server Express LocalDB habilita características como procedimientos almacenados, funciones definidas por el usuario y agregados, tipos espaciales, integración de .NET Framework y otros usuarios que no están disponibles en SQL Server Compact.
> 
> **Páginas maestras**
> 
> Un [página maestra ASP.NET](https://msdn.microsoft.com/library/wtxbf3hh.aspx) define una apariencia coherente y el comportamiento de todas las páginas de la aplicación. El diseño de la página maestra se combina con el contenido de una página de contenido individual para generar la última página que ve el usuario. En la aplicación Wingtip Toys, modificará el *Site.master* página principal para que todas las páginas en el sitio Web de Wingtip Toys comparten la misma barra de navegación y el logotipo de distintivo.
> 
> **HTML5**
> 
> La plantilla de aplicación de ASP.NET Web Forms admite [HTML5](http://www.w3schools.com/html/html5_intro.asp), que es la versión más reciente del lenguaje de marcado HTML. HTML5 es compatible con los nuevos elementos y funcionalidad que facilitan la creación de sitios Web.
> 
> **Modernizr**
> 
> Para los exploradores no compatibles con HTML5, puede usar [Modernizr](http://www.modernizr.com/). Modernizr es una biblioteca de JavaScript de código abierto que puede detectar si un explorador es compatible con características de HTML5 y habilitarlas si no es así. En la plantilla de aplicación de ASP.NET Web Forms, Modernizr se instala como un paquete de NuGet.
> 
> **Bootstrap**
> 
> Usan las plantillas de proyecto de Visual Studio 2013 [Bootstrap](http://getbootstrap.com/), un marco de trabajo de diseño y creación de temas creado por Twitter. Bootstrap usa CSS3 para proporcionar un diseño dinámico, lo que significa que los diseños pueden adaptarse dinámicamente a los tamaños de ventana de explorador diferente. También puede usar características de creación de temas de Bootstrap para llevar a cabo fácilmente un cambio en apariencia y comportamiento de la aplicación. De forma predeterminada, la plantilla de aplicación Web ASP.NET en Visual Studio 2013 incluye Bootstrap como un paquete de NuGet.
> 
> **Paquetes de NuGet**
> 
> La plantilla de aplicación de formularios Web ASP.NET incluye un conjunto de [NuGet](http://www.nuget.org/) paquetes. Estos paquetes proporcionan funcionalidad formada por componentes en forma de herramientas y bibliotecas de código abierto. Hay una gran variedad de paquetes que le ayudarán a crear y probar sus aplicaciones. Visual Studio facilita agregar, quitar y actualizar paquetes de NuGet. Los desarrolladores pueden crear y agregar paquetes a NuGet también.
> 
> ![Crear el proyecto: cuadro de diálogo de NuGet](create-the-project/_static/image8.png)
> 
> Cuando se instala un paquete, NuGet copia los archivos a la solución y realiza automáticamente los cambios que son necesarios, como agregar referencias y cambiar la configuración asociada con la aplicación Web. Si decide quitar la biblioteca, NuGet quita archivos e invierte los cambios realizan en el proyecto para que no se deja confusión. NuGet está disponible desde el **herramientas** menú en Visual Studio.
> 
> **jQuery**
> 
> [jQuery](http://jquery.com/) es una biblioteca de JavaScript rápida y concisa que simplifica el recorrido de documento HTML, control de eventos, animar y las interacciones de Ajax para el desarrollo web rápido. La biblioteca JavaScript jQuery se incluye en la plantilla de aplicación de ASP.NET Web Forms como un paquete de NuGet.
> 
> **Validación discreta**
> 
> Los controles validadores integrados se han configurado para usar JavaScript discreto para la lógica de validación del lado cliente. Esto reduce la cantidad de JavaScript que se representan en línea en el marcado de página significativamente y reduce el tamaño total de la página. Validación discreta se agrega globalmente a la plantilla de aplicación de ASP.NET Web Forms según la configuración en el &lt;appSettings&gt; elemento de la *Web.config* archivo en la raíz de la aplicación.
> 
> **Entity Framework Code First**
> 
> Además de las características de la plantilla de aplicación de ASP.NET Web Forms, se utiliza la aplicación Wingtip Toys [Entity Framework Code First](https://weblogs.asp.net/scottgu/archive/2010/12/08/announcing-entity-framework-code-first-ctp5-release.aspx), que es una biblioteca de NuGet que permite el desarrollo centrado en el código cuando se trabaja con datos. Pocas palabras, crea la parte de la base de datos de la aplicación automáticamente basándose en el código que se escribe. Uso de Entity Framework, recuperar y manipular los datos como objetos fuertemente tipados. Este le permite centrarse en la lógica de negocios en la aplicación en lugar de los detalles de cómo se accede a los datos.
> 
> Para obtener más información acerca de las bibliotecas instaladas y paquetes incluidos con la plantilla de formularios Web Forms de ASP.NET, vea la lista de paquetes de NuGet instalados. Para ello, en Visual Studio cree un nuevo proyecto de formularios Web Forms, seleccione **herramientas** > **Administrador de paquetes de NuGet** > **administrar paquetes de NuGet para solución**y seleccione **paquetes instalados** en el **administrar paquetes de NuGet** cuadro de diálogo.

### <a name="touring-visual-studio"></a>Touring Visual Studio

El principal de windows en Visual Studio incluye el **el Explorador de soluciones**, el **Explorador de servidores** (**Database Explorer** Express), el **propiedades Ventana**, el **cuadro de herramientas**, **barra de herramientas**y el **ventana de documento**.

![Crear el proyecto: cuadro de diálogo de NuGet](create-the-project/_static/image9.png)

Para obtener más información acerca de Visual Studio, consulte [guía Visual para Visual Web Developer](https://msdn.microsoft.com/library/ee410104.aspx).

## <a name="summary"></a>Resumen

En este tutorial ha creado, revisado y ejecutar la aplicación de formularios Web Forms de forma predeterminada. Ha revisado las diferentes características de la aplicación de formularios Web predeterminada y aprendido algunos conceptos básicos sobre cómo usar el entorno de Visual Studio. En los tutoriales siguientes creará la capa de acceso a datos.

## <a name="additional-resources"></a>Recursos adicionales

[Elegir el modelo de programación correcto](../../../videos/how-do-i/choosing-the-right-programming-model.md)   
[Proyectos de aplicación Web frente a proyectos de sitio Web](https://msdn.microsoft.com/library/dd547590.aspx)   
[Información general de las páginas de ASP.NET Web Forms](https://msdn.microsoft.com/library/428509ah.aspx)

> [!div class="step-by-step"]
> [Anterior](introduction-and-overview.md)
> [Siguiente](create_the_data_access_layer.md)
