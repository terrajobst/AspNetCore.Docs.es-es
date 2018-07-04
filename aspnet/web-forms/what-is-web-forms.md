---
uid: web-forms/what-is-web-forms
title: ¿Qué es Web Forms | Microsoft Docs
author: rick-anderson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: 6444577afa49512397f69055df1737d8aec148c0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362961"
---
<a name="what-is-web-forms"></a>¿Qué es Web Forms
====================
Formularios Web Forms ASP.NET es una parte del marco de aplicación web ASP.NET y se incluye con [Visual Studio](https://www.asp.net/downloads). Es uno de los cuatro modelos de programación que puede usar para crear aplicaciones web ASP.NET, los demás son aplicaciones de página única de ASP.NET, ASP.NET Web Pages y ASP.NET MVC.

Formularios Web Forms son páginas que los usuarios solicitan mediante su explorador. Estas páginas se pueden escribir mediante una combinación de HTML, script de cliente, los controles de servidor y código de servidor. Cuando los usuarios solicitan una página, se compila y se ejecuta en el servidor, el marco de trabajo y, a continuación, el marco de trabajo genera el marcado HTML que puede representar el explorador. Una página de formularios Web Forms de ASP.NET presenta información al usuario en cualquier explorador o dispositivo cliente.

Con Visual Studio, puede crear formularios Web Forms de ASP.NET. El entorno de desarrollo integrado (IDE) de Visual Studio le permite arrastrar y colocar los controles de servidor para diseñar su página de formularios Web Forms. A continuación, puede establecer fácilmente las propiedades, métodos y eventos para controles de la página o para la misma página. Estas propiedades, métodos y eventos se usan para definir el comportamiento de la página web, apariencia y funcionamiento y así sucesivamente. Para escribir código para controlar la lógica de la página del servidor, puede usar un lenguaje. NET, como Visual Basic o C#.

> [!NOTE] 
> 
> Documentación de ASP.NET y Visual Studio abarca varias versiones. Los temas que resaltan las características de versiones anteriores pueden ser útiles para sus tareas actuales y escenarios de uso de las versiones más recientes.


**Formularios Web Forms ASP.NET son:** 

- Se basa en tecnología de Microsoft ASP.NET, en que el código que se ejecuta en el servidor de forma dinámica genera resultados de la página Web en el explorador o dispositivo cliente.
- Es compatible con cualquier explorador o dispositivo móvil. Una página Web ASP.NET representa de forma automática el código HTML adecuado al explorador para características como estilos, diseño y así sucesivamente.
- Es compatible con cualquier lenguaje compatible con .NET common language runtime, como Microsoft Visual Basic y Microsoft Visual C#.
- Se basa en Microsoft .NET Framework. Esto proporciona todas las ventajas de framework, incluidos un entorno administrado, seguridad de tipos y herencia.
- Flexible porque puede agregar creada por el usuario y controles de terceros a ellos.

**Oferta de formularios Web Forms ASP.NET:** 

- Separación de HTML y otro código de interfaz de usuario de lógica de la aplicación.
- Un conjunto enriquecido de controles de servidor para tareas comunes, incluido el acceso a datos.
- Eficaz enlace de datos, con compatibilidad con herramientas excelentes.
- Compatibilidad con scripting del lado cliente que se ejecuta en el explorador.
- Compatibilidad con una variedad de otras funciones, incluido el enrutamiento, seguridad, rendimiento, internacionalización, pruebas, depuración, control de errores y administración de Estados.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Formularios Web Forms ASP.NET le ayuda a superar los retos

Programación de aplicaciones Web presenta desafíos que plantea normalmente al programar aplicaciones tradicionales basadas en el cliente. Entre los retos son:

- **Implementar una interfaz de usuario Web enriquecida** : puede ser difícil y tedioso para diseñar e implementar un usuario mediante las funciones básicas de HTML, especialmente si la página tiene un diseño complejo, una gran cantidad de contenido dinámico, la interfaz y completa objetos de usuario interactivo.
- **Separación de cliente y servidor** -aplicación de una Web, el cliente (explorador) y el servidor son diferentes programas a menudo se ejecutan en equipos diferentes (e incluso en diferentes sistemas operativos). Por lo tanto, las dos mitades de la aplicación comparten muy poca información; puede comunicarse, pero normalmente solo pequeños fragmentos de información simple de exchange.
- **Ejecución sin estado** : cuando un servidor Web recibe una solicitud para una página, encuentra la página, lo procesa, lo envía al explorador y, a continuación, se descarta toda la información de página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia, volver a procesar la página desde el principio. Dicho de otro modo, un servidor no tiene memoria de páginas que ha procesado, no tienen estado. Por lo tanto, si una aplicación necesita mantener información sobre una página, su naturaleza sin estado puede constituir un problema.
- **Capacidades de cliente desconocido** -en muchos casos, las aplicaciones Web son accesibles para muchos usuarios con distintos exploradores. Los exploradores tienen las mismas capacidades, lo que dificulta la creación de una aplicación que se ejecutará igualmente bien en todos ellos.
- **Complicaciones con el acceso a datos** -leer y escribir en un origen de datos en las aplicaciones Web tradicionales pueden ser complicada y consumir muchos recursos.
- **Complicaciones con la escalabilidad** : en muchos casos, las aplicaciones de Web diseñadas con los métodos existentes no cumplir los objetivos de escalabilidad debido a la falta de compatibilidad entre los distintos componentes de la aplicación. Esto suele ser un punto de error comunes para las aplicaciones en un ciclo de crecimiento intenso.

Cumplir los desafíos para las aplicaciones Web puede requerir bastante tiempo y esfuerzo. ASP.NET Web Forms y ASP.NET framework enfrentan estos desafíos de las maneras siguientes:

- **Modelo de objetos coherente e intuitivo** -marco de páginas de ASP.NET presenta un modelo de objetos que le permite pensar en los formularios como una unidad, no como partes independientes de cliente y servidor. En este modelo, puede programar la página de una manera más intuitiva que en las aplicaciones Web tradicionales, incluida la capacidad de establecer las propiedades de elementos de la página y responder a eventos. Además, los controles de servidor ASP.NET son una abstracción del contenido físico de una página HTML y de la interacción directa entre el explorador y el servidor. En general, puede usar controles de servidor de la manera en que puede trabajar con controles en una aplicación cliente y no tiene que pensar en cómo crear el código HTML para presentar y procesar los controles y su contenido.
- **Modelo de programación orientada a eventos** -ASP.NET Web Forms aportan a las aplicaciones Web un modelo familiar de escribir controladores de eventos para eventos que se producen en el cliente o el servidor. El marco de páginas ASP.NET abstrae este modelo de tal manera que el mecanismo subyacente de un evento en el cliente de captura, transmitirlos al servidor y una llamada al método adecuado es automático e invisible para el usuario. El resultado es una estructura clara y escribir fácilmente código que admite el desarrollo controlado por eventos.
- **Administración del estado intuitiva** : marco de páginas ASP.NET controla automáticamente la tarea de mantenimiento del estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación. Esto se logra sin un uso intensivo de recursos del servidor y puede implementarse con o sin enviar cookies en el explorador.
- **Aplicaciones independientes del explorador** -marco de páginas de ASP.NET le permite crear toda la lógica de aplicación en el servidor, lo que elimina la necesidad de escribir explícitamente código para las diferencias en los exploradores. Sin embargo, todavía podrá aprovechar las ventajas de las características específicas de explorador mediante la escritura de código de cliente para proporcionar un rendimiento mejorado y una experiencia de cliente enriquecida.
- **Compatibilidad de .NET framework common language runtime** -marco de páginas ASP.NET se basa en .NET Framework, por lo que el marco de trabajo completo está disponible para cualquier aplicación ASP.NET. Las aplicaciones pueden escribirse en cualquier lenguaje que sea compatible con el tiempo de ejecución. Además, el acceso a datos se simplifica mediante la infraestructura de acceso de datos proporcionada por .NET Framework, incluidas ADO.NET.
- **Rendimiento de servidor escalable de .NET framework** -marco de páginas de ASP.NET le permite escalar la aplicación Web desde un equipo con un solo procesador a una granja Web de varios equipo perfectamente y sin cambios complicados en la aplicación lógica.

## <a name="features-of-aspnet-web-forms"></a>Características de ASP.NET Web Forms

- **Los controles de servidor**-controles de servidor Web de ASP.NET son objetos en las páginas Web ASP.NET que se ejecutan cuando se solicita la página y ese marcado de representación en el explorador. Muchos controles de servidor Web son similares a los elementos HTML conocidos, como botones y cuadros de texto. Otros controles abarcan un comportamiento complejo, por ejemplo, un calendario de controles y controles que puede usar para conectarse a orígenes de datos y mostrar los datos.
- **Páginas maestras**-páginas maestras de ASP.NET permiten crear un diseño coherente para las páginas en la aplicación. Una sola página maestro define la apariencia y el comportamiento estándar que desea para todas las páginas (o un grupo de páginas) de la aplicación. A continuación, puede crear páginas de contenido individuales que contienen el contenido que desea mostrar. Cuando los usuarios solicitan las páginas de contenido, éstas se combinan con la página maestra para producir el resultado que combine el diseño de la página maestra con el contenido de la página de contenido.
- **Trabajar con datos**-ASP.NET proporciona muchas opciones para almacenar, recuperar y mostrar los datos. En una aplicación de formularios Web Forms de ASP.NET, usa controles enlazados a datos para automatizar la entrada de datos de elementos de interfaz de usuario de página web como las tablas y los cuadros de texto y listas desplegables o presentación.
- **Pertenencia**-ASP.NET Identity almacena las credenciales de los usuarios en una base de datos creado por la aplicación. Cuando los usuarios inicien sesión, la aplicación valida sus credenciales mediante la lectura de la base de datos. El proyecto *cuenta* carpeta contiene los archivos que implementan las distintas partes de pertenencia: registro, iniciar sesión, cambiar una contraseña y autorizar el acceso. Además, formularios Web Forms de ASP.NET es compatible con OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios inicien sesión en el sitio con las credenciales existentes, desde estas cuentas, como Facebook, Twitter, Windows Live y Google. De forma predeterminada, la plantilla crea una base de datos de pertenencia mediante un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de desarrollo de base de datos que se incluye con Visual Studio Express 2013 para Web.
- **Script de cliente y marcos de trabajo de cliente**-puede mejorar las características basadas en servidor de ASP.NET al incluir funcionalidad de script de cliente en las páginas de formulario Web Forms de ASP.NET. Puede usar el script de cliente para proporcionar una interfaz de usuario más completa y más capacidad de respuesta a los usuarios. También puede usar el script de cliente para realizar llamadas asincrónicas al servidor Web mientras se ejecuta una página en el explorador.
- **Enrutamiento**-enrutamiento de direcciones URL le permite configurar una aplicación para aceptar solicitudes de direcciones URL que no se asignan a los archivos físicos. Una dirección URL de solicitud es simplemente la dirección URL de un usuario escribe en su explorador para buscar una página en el sitio web. Utilizar el enrutamiento para definir direcciones URL que son semánticamente significativos para los usuarios y que puede ayudarle con la optimización de motor de búsqueda (SEO).
- **Administración de estado**-formularios Web Forms de ASP.NET incluye varias opciones que le ayudarán a mantener los datos una función de la página y una base de toda la aplicación.
- **Seguridad**-una parte importante de desarrollar una aplicación más segura es entender las amenazas a él. Microsoft ha desarrollado una manera de clasificar las amenazas: suplantación de identidad, manipulación, rechazo, divulgación de información, denegación de servicio, elevación de privilegios (STRIDE). En los formularios Web Forms de ASP.NET, puede agregar puntos de extensibilidad y las opciones de configuración que le permiten personalizar distintos comportamientos de seguridad en formularios Web Forms de ASP.NET.
- **Rendimiento**-rendimiento puede ser un factor clave en un proyecto o sitio Web tenga éxito. ASP.NET Web Forms le permite modificar el rendimiento relacionados con la página y el procesamiento del control de servidor, administración de Estados, acceso a datos, configuración de la aplicación y la carga y eficaz de las prácticas de codificación.
- **Internacionalización**: ASP.NET Web Forms le permite crear páginas web que puede obtener contenido y otros datos basándose en la configuración de idioma preferido para el explorador o según la elección del usuario explícita del lenguaje. Contenido y otros datos se conocen como recursos y estos datos pueden almacenarse en archivos de recursos u otros orígenes. En una página de formularios Web Forms de ASP.NET, configurar controles para obtener sus valores de propiedad de recursos. En tiempo de ejecución, las expresiones de recurso se reemplazan por los recursos del archivo de recursos localizado adecuado.
- **Depuración y control de errores**-ASP.NET incluye características para ayudarle a diagnosticar los problemas que pueden surgir en la aplicación de formularios Web Forms. Depuración y control de errores también se admiten en ASP.NET Web Forms para que sus aplicaciones, compilación y ejecutan de forma eficaz.
- **Implementación y hospedaje**-IIS, ASP.NET, Azure y Visual Studio proporcionan herramientas que le ayudarán en el proceso de implementación y hospedaje de la aplicación de formularios Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidir cuándo se debe crear una aplicación de formularios Web

Debe considerar cuidadosamente si implementar una aplicación Web mediante el uso de ASP.NET Web Forms modelo u otro modelo, por ejemplo, el marco de MVC de ASP.NET. El marco de MVC no reemplaza el modelo de formularios Web Forms; Puede usar cualquier marco de aplicaciones Web. Antes de decidirse a usar el modelo de formularios Web Forms o el marco de MVC para un sitio Web concreto, sopese las ventajas de cada enfoque.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Ventajas de una aplicación Web basada en formularios de Web

El marco de trabajo basado en formularios Web Forms ofrece las siguientes ventajas:

- Admite un modelo de eventos que conserva el estado a través de HTTP, lo que beneficia a desarrollo de aplicaciones Web de línea de negocio. La aplicación basada en formularios Web Forms proporciona docenas de eventos que se admiten en centenares de controles de servidor.
- Usa un modelo de controlador de página que agrega funcionalidad a páginas individuales. Para obtener más información, consulte [página controlador](https://go.microsoft.com/fwlink/?LinkId=106359 "página controlador") en el sitio Web de MSDN.
- Usa el estado de vista ni formularios basados en servidor, lo que pueden facilitar información de estado de administración.
- Funciona bien para equipos pequeños de desarrolladores Web y los diseñadores que deseen aprovechar las ventajas de un gran número de componentes disponibles para el desarrollo rápido de aplicaciones.
- En general, es menos complejo para el desarrollo de aplicaciones, porque los componentes (la **página** clase, controles etc.) se integran estrechamente y suelen requerir menos código que el modelo de MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Ventajas de una aplicación Web basada en MVC

El marco de ASP.NET MVC ofrece las siguientes ventajas:

- Resulta más fácil de administrar la complejidad al dividir una aplicación en el modelo, la vista y el controlador.
- No utiliza el estado de vista ni formularios basados en servidor. Esto hace que el marco de MVC ideal para los desarrolladores que desean el control total sobre el comportamiento de una aplicación.
- Usa un modelo de controlador frontal que procesa las solicitudes de aplicación Web a través de un único controlador. Esto le permite diseñar una aplicación que admite una infraestructura de enrutamiento avanzada. Para obtener más información, consulte [controlador frontal](https://go.microsoft.com/fwlink/?LinkId=106357 "controlador frontal") en el sitio Web de MSDN.
- Ofrece compatibilidad mejorada para el desarrollo controlado por pruebas (TDD).
- Funciona bien para las aplicaciones Web que son compatibles con equipos grandes de desarrolladores y diseñadores Web que necesitan un alto grado de control sobre el comportamiento de la aplicación.
