---
uid: web-forms/what-is-web-forms
title: "¿Qué es formularios Web Forms | Documentos de Microsoft"
author: rick-anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: 5fa1daf9-1161-4cfa-bd4c-658f48b2c229
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/what-is-web-forms
msc.type: content
ms.openlocfilehash: dcaba60a8640ce83f73460a5c8ee457257b9397e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="what-is-web-forms"></a>¿Qué es formularios Web Forms
====================
Formularios Web Forms ASP.NET es una parte del marco de aplicación web ASP.NET y se incluye con [Visual Studio](https://www.asp.net/downloads). Es uno de los cuatro modelos de programación que puede usar para crear aplicaciones web ASP.NET, los demás son ASP.NET MVC, ASP.NET Web Pages y ASP.NET solo las aplicaciones de la página.

Formularios Web Forms son páginas que los usuarios solicitan con su explorador. Estas páginas pueden escribirse utilizando una combinación de HTML, script de cliente, los controles de servidor y código de servidor. Cuando los usuarios solicitan una página, se compila y se ejecuta en el servidor por el marco de trabajo y, a continuación, el marco de trabajo genera el marcado HTML que puede representar el explorador. Una página de formularios Web Forms de ASP.NET presenta información al usuario en cualquier explorador o dispositivo cliente.

Con Visual Studio, puede crear formularios Web Forms de ASP.NET. El entorno de desarrollo integrado (IDE) de Visual Studio le permite arrastrar y colocar controles de servidor para mostrar la página de formularios Web Forms. A continuación, puede establecer fácilmente propiedades, métodos y eventos de controles de la página o de la manera de independiente de la página. Estas propiedades, métodos y eventos se usan para definir el comportamiento de la página web, apariencia y funcionamiento y así sucesivamente. Para escribir código de servidor para controlar la lógica de la página, puede usar un lenguaje de .NET como Visual Basic o C#.

> [!NOTE] 
> 
> Documentación de ASP.NET y Visual Studio abarca varias versiones. Temas que resaltan las características de versiones anteriores pueden ser útiles para sus tareas actuales y escenarios que utilizan las versiones más recientes.


**Formularios Web Forms ASP.NET son:** 

- Se basa en tecnología de Microsoft ASP.NET, en el que código que se ejecuta en el servidor de forma dinámica genera el resultado de la página Web en el explorador o dispositivo cliente.
- Es compatible con cualquier explorador o dispositivo móvil. Una página Web ASP.NET representa automáticamente el código HTML adecuado al explorador para funciones tales como estilos, diseño y así sucesivamente.
- Es compatible con cualquier lenguaje compatible con .NET common language runtime, como Microsoft Visual Basic y Microsoft Visual C#.
- Se basa en Microsoft .NET Framework. Esto proporciona todas las ventajas de framework, incluidos un entorno administrado, seguridad de tipos y herencia.
- Flexible porque puede agregar creados por el usuario como controles de terceros a ellos.

**Oferta de formularios Web Forms ASP.NET:** 

- Separación de HTML y otro código de interfaz de usuario de lógica de la aplicación.
- Un conjunto completo de controles de servidor para las tareas comunes, incluido el acceso de datos.
- Enlace, con compatibilidad con excelentes herramientas de datos eficaces.
- Compatibilidad con secuencias de comandos de cliente que se ejecuta en el explorador.
- Admite una gran variedad de otras funciones, incluido el enrutamiento, seguridad, rendimiento, internacionalización, pruebas, depuración, control de errores y administración de Estados.

## <a name="aspnet-web-forms-helps-you-overcome-challenges"></a>Formularios Web Forms ASP.NET le ayuda a superar los retos de

Programación de aplicaciones Web presenta retos que no surgen normalmente al programar aplicaciones tradicionales basadas en cliente. Entre los desafíos son:

- **Implementar una interfaz de usuario Web enriquecida** : puede ser difícil y tedioso diseñar e implementar un usuario con las funciones básicas de HTML, especialmente si la página tiene un diseño complejo, una gran cantidad de contenido dinámico, la interfaz y completa objetos de usuario interactivo.
- **Separación de cliente y servidor** -aplicación en un Web, el cliente (explorador) y el servidor son programas diferentes que a menudo se ejecutan en equipos diferentes (e incluso en diferentes sistemas operativos). Por lo tanto, las dos mitades de la aplicación comparten muy poca información; se pueden comunicar, pero normalmente intercambian sólo pequeñas porciones de información simple.
- **Ejecución sin estado** : cuando un servidor Web recibe una solicitud de una página, encuentra la página, lo procesa, lo envía al explorador y, a continuación, descarta toda la información de página. Si el usuario solicita la misma página de nuevo, el servidor repite toda la secuencia, volver a procesar la página desde el principio. Dicho de otro modo, un servidor no tiene memoria de páginas que han procesado, no tienen estado. Por lo tanto, si una aplicación necesita mantener información sobre una página, su naturaleza sin estado puede constituir un problema.
- **Capacidades de cliente desconocido** -en muchos casos, las aplicaciones Web son accesibles para muchos usuarios con distintos exploradores. Exploradores tienen las mismas capacidades, lo que dificulta la crear una aplicación que se ejecutará igualmente bien en todos ellos.
- **Complicaciones con el acceso a datos** -leer y escribir en un origen de datos en las aplicaciones Web tradicionales pueden resultar complicado y gran cantidad de recursos.
- **Complicaciones con la escalabilidad** : en muchos casos producirá un error en aplicaciones Web diseñadas con los métodos existentes para lograr los objetivos de escalabilidad debido a la falta de compatibilidad entre los distintos componentes de la aplicación. Por lo general, suele ser un punto de error común para las aplicaciones en un ciclo de crecimiento intenso.

Cumplir los desafíos para las aplicaciones Web puede requerir mucho tiempo y esfuerzo. ASP.NET Web Forms y el marco de ASP.NET resolver estos desafíos de las maneras siguientes:

- **Modelo de objetos coherente e intuitivo** -marco de páginas de ASP.NET presenta un modelo de objetos que permite concebir los formularios como una unidad, no como partes de cliente y servidor independientes. En este modelo, puede programar la página de un modo más intuitivo que en las aplicaciones Web tradicionales, incluida la capacidad de establecer las propiedades de elementos de la página y responder a eventos. Además, los controles de servidor ASP.NET son una abstracción de los contenidos físicos de una página HTML y de la interacción directa entre el explorador y el servidor. En general, puede usar controles de servidor del modo que puede trabajar con controles en una aplicación cliente y no tienen que preocuparse de cómo crear el código HTML para presentar y procesar los controles y su contenido.
- **Modelo de programación orientada a eventos** -ASP.NET Web Forms aportan a las aplicaciones Web un modelo familiar de la escritura de controladores de eventos para eventos que se producen en el cliente o el servidor. El marco de páginas ASP.NET abstrae este modelo de tal manera que el mecanismo subyacente de captura de los eventos en el cliente, transmitir al servidor y llamar al método apropiado es todo automático e invisible para el usuario. El resultado es una estructura de código clara y escrita con facilidad, que admite el desarrollo controlado por eventos.
- **Administración del estado intuitiva** : marco de páginas de ASP.NET controla automáticamente la tarea de mantener el estado de la página y sus controles, y proporciona métodos explícitos para mantener el estado de la información específica de la aplicación. Esto se logra sin un uso intensivo de recursos del servidor y se puede implementar con o sin enviar cookies al explorador.
- **Aplicaciones independientes del explorador** -marco de páginas de ASP.NET permite crear toda la lógica de aplicación en el servidor, lo que elimina la necesidad de escribir explícitamente código para las diferencias en los exploradores. Sin embargo, todavía permite aprovechar las características específicas del explorador mediante la escritura de código de cliente para proporcionar un mejor rendimiento y una experiencia de cliente.
- **Compatibilidad de .NET framework common language runtime** -marco de páginas de ASP.NET se basa en .NET Framework, por lo que todo el marco de trabajo está disponible para cualquier aplicación ASP.NET. Las aplicaciones pueden escribirse en cualquier lenguaje que sea compatible con el tiempo de ejecución. Además, el acceso a datos se ha simplificado mediante la infraestructura de acceso de datos proporcionada por .NET Framework, incluido ADO.NET.
- **Rendimiento de servidor escalable de .NET framework** -marco de páginas ASP.NET permite escalar la aplicación Web desde un equipo con un solo procesador a una granja de servidores de Web de varios equipo limpiamente y sin cambios complicados en la aplicación lógica.

## <a name="features-of-aspnet-web-forms"></a>Características de ASP.NET Web Forms

- **Controles de servidor**-controles de servidor Web de ASP.NET son objetos en las páginas Web ASP.NET que se ejecutan cuando se solicita la página y que representan marcado en el explorador. Muchos controles de servidor Web son similares a elementos HTML conocidos, como botones y cuadros de texto. Otros controles abarcan comportamiento complejo, como un calendario de controles y controles que puede usar para conectarse a orígenes de datos y mostrar los datos.
- **Páginas maestras**-páginas maestras de ASP.NET permiten crear un diseño coherente para las páginas en la aplicación. Una sola página maestra define la apariencia y el comportamiento estándar que desea que tengan todas las páginas (o un grupo de páginas) de la aplicación. A continuación, puede crear páginas de contenido individuales que contienen el contenido que desea mostrar. Cuando los usuarios solicitan las páginas de contenido, combinan con la página maestra para tener una salida que combine el diseño de la página maestra con el contenido de la página de contenido.
- **Trabajar con datos**-ASP.NET proporciona muchas opciones para almacenar, recuperar y mostrar datos. En una aplicación de formularios Web Forms de ASP.NET, usa controles enlazados a datos para automatizar la entrada de datos de elementos de interfaz de usuario de página web, como tablas y cuadros de texto y listas desplegables o presentación.
- **Pertenencia**-ASP.NET Identity almacena las credenciales de los usuarios en una base de datos creado por la aplicación. Cuando los usuarios inicien sesión, la aplicación valida las credenciales mediante la lectura de la base de datos. El proyecto *cuenta* carpeta contiene los archivos que implementan las distintas partes de pertenencia: registrar, inicio de sesión, cambiar una contraseña y autorizar el acceso. Además, los formularios Web Forms de ASP.NET admite OAuth y OpenID. Estas mejoras de autenticación permiten a los usuarios inicien sesión en el sitio mediante las credenciales existentes, de estas cuentas, como Facebook, Twitter, Windows Live, Google y. De forma predeterminada, la plantilla crea una base de datos de pertenencia con un nombre de base de datos predeterminado en una instancia de SQL Server Express LocalDB, el servidor de base de datos de desarrollo que se incluye con Visual Studio Express 2013 para Web.
- **Script de cliente y marcos de trabajo de cliente**-puede mejorar las características basadas en servidor de ASP.NET mediante la inclusión de funcionalidad de script de cliente en páginas de formularios Web de ASP.NET. Puede usar scripts de cliente para proporcionar una interfaz de usuario más enriquecida y más capacidad de respuesta a los usuarios. También puede usar el script de cliente para realizar llamadas asincrónicas al servidor Web mientras se ejecuta una página en el explorador.
- **Enrutamiento**-enrutamiento de direcciones URL le permite configurar una aplicación para aceptar solicitudes de direcciones URL que no se asignen a archivos físicos. Una dirección URL de solicitud es simplemente la dirección URL de un usuario escribe en su explorador para buscar una página en el sitio web. Utilizar el enrutamiento para definir direcciones URL que son semánticamente significativos a los usuarios y que puede ayudarle con la optimización de motor de búsqueda (SEO).
- **La administración de estados**-formularios Web Forms de ASP.NET incluye varias opciones que le ayudarán a conservar los datos en una base por cada página y una base de toda la aplicación.
- **Seguridad**-una parte importante del desarrollo de una aplicación más segura es comprender las amenazas a él. Microsoft ha desarrollado una manera de categorizar las amenazas: suplantación, manipulación, rechazo, divulgación de información, denegación de servicio, elevación de privilegios (intervalo). En los formularios Web Forms de ASP.NET, puede agregar puntos de extensibilidad y opciones de configuración que le permiten personalizar distintos comportamientos de seguridad en formularios Web Forms de ASP.NET.
- **Rendimiento**-rendimiento puede ser un factor clave en un proyecto o sitio Web que tenga éxito. ASP.NET Web Forms le permite modificar rendimiento asociado a la página y el procesamiento del control de servidor, administración de Estados, acceso a datos, configuración de la aplicación y carga y eficaz de las prácticas de codificación.
- **Internacionalización**: formularios Web Forms de ASP.NET le permite crear páginas web que puede obtener contenido y otros datos basándose en la configuración de idioma preferido del explorador o en función de la elección del usuario explícita del lenguaje. Contenido y otros datos se conocen como recursos y estos datos pueden almacenarse en archivos de recursos u otros orígenes. En una página de formularios Web Forms de ASP.NET, configura los controles para obtener sus valores de propiedad de recursos. En tiempo de ejecución, las expresiones de recursos se reemplazan por los recursos del archivo de recursos localizado adecuado.
- **Depuración y control de errores**-ASP.NET incluye características para ayudarle a diagnosticar los problemas que pueden surgir en la aplicación de formularios Web Forms. Depuración y control de errores también se admiten en formularios Web Forms de ASP.NET para que las aplicaciones, compilan y ejecutan de forma eficaz.
- **Implementación y hospedaje**-IIS, ASP.NET, Azure y Visual Studio proporcionan herramientas que le ayudarán a con el proceso de implementación y hospeda la aplicación de formularios Web Forms.

## <a name="deciding-when-to-create-a-web-forms-application"></a>Decidir cuándo se debe crear una aplicación de formularios Web

Debe considerar cuidadosamente si implementar una aplicación Web utilizando ASP.NET Web Forms modelo u otro modelo, por ejemplo, el marco de MVC de ASP.NET. El marco de MVC no reemplaza el modelo de formularios Web Forms; Puede usar cualquier marco de aplicaciones Web. Antes de decidirse a usar el modelo de formularios Web Forms o el marco de MVC para un sitio Web concreto, sopese las ventajas de cada enfoque.

### <a name="advantages-of-a-web-forms-based-web-application"></a>Ventajas de una aplicación Web basada en formularios de Web

El marco de trabajo basado en formularios Web Forms ofrece las siguientes ventajas:

- Admite un modelo de eventos que conserva el estado a través de HTTP, lo que beneficia a desarrollo de aplicaciones Web de línea de negocio. La aplicación basada en formularios Web Forms proporciona docenas de eventos que se admiten en centenares de controles de servidor.
- Usa un modelo de controlador de página que agrega funcionalidad a las páginas individuales. Para obtener más información, consulte [Page Controller](https://go.microsoft.com/fwlink/?LinkId=106359 "Page Controller") en el sitio Web de MSDN.
- Usa el estado de vista ni formularios basados en servidor, que pueden facilitar la administración de información de estado.
- Funciona bien para los equipos pequeños de desarrolladores Web y diseñadores que deseen aprovechar el gran número de componentes disponibles para el desarrollo rápido de aplicaciones.
- En general, es menos complejo para el desarrollo de aplicaciones, porque los componentes (la **página** clase, controles etc.) se integran estrechamente y suelen requerir menos código que el modelo MVC.

### <a name="advantages-of-an-mvc-based-web-application"></a>Ventajas de una aplicación Web basada en MVC

El marco de ASP.NET MVC ofrece las siguientes ventajas:

- Resulta más fácil de administrar la complejidad al dividir una aplicación en el modelo, la vista y el controlador.
- No utiliza el estado de vista ni formularios basados en servidor. Esto hace que el marco de MVC ideal para los desarrolladores que deseen un control completo sobre el comportamiento de una aplicación.
- Usa un modelo de controlador frontal que procesa las solicitudes de aplicación Web a través de un único controlador. Esto le permite diseñar una aplicación que admite una infraestructura de enrutamiento avanzada. Para obtener más información, consulte [controlador frontal](https://go.microsoft.com/fwlink/?LinkId=106357 "controlador frontal") en el sitio Web de MSDN.
- Proporciona la mejor compatibilidad para el desarrollo controlado por pruebas (TDD).
- Funciona bien para las aplicaciones Web que son compatibles con equipos grandes de desarrolladores y diseñadores Web que necesitan un alto grado de control sobre el comportamiento de la aplicación.
