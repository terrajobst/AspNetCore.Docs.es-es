---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Creación de aplicaciones de nube reales con Azure | Microsoft Docs
author: MikeWasson
description: Este libro electrónico le guiará a través de un enfoque basado en patrones para crear soluciones de nube del mundo real. Los patrones se aplican al proceso de desarrollo, así como a un...
ms.author: aspnetcontent
ms.date: 06/12/2014
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 11388b9c58d2d21c7a337a343c10d33c7257ccf1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840503"
---
<a name="building-real-world-cloud-apps-with-azure"></a>Creación de aplicaciones de nube reales con Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este libro electrónico le guiará a través de un enfoque basado en patrones para crear soluciones de nube del mundo real. Los patrones se aplican al proceso de desarrollo, así como a la arquitectura y las prácticas de codificación.
> 
> El contenido se basa en una presentación desarrollado por Scott Guthrie ofreció por él en Norwegian Developers Conference (NDC) en junio de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) y en Microsoft Tech Ed Australia en Septiembre de 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muchos otros](more-patterns-and-guidance.md#acknowledgments) actualizado y ampliado el contenido al pasarlo de vídeo para impresa.


## <a name="intended-audience"></a>Público destinatario

Los desarrolladores que tiene curiosidad sobre el desarrollo para la nube, considerando cambiar a la nube o están familiarizado con el desarrollo de nube aquí encontrará información general concisa de los conceptos y procedimientos recomendados que necesitan saber más importantes. Los conceptos se ilustran con ejemplos concretos y cada capítulo se vincula a otros recursos para obtener información más detallada. Los ejemplos y los vínculos a recursos adicionales son servicios y marcos de Microsoft, pero los principios ilustrados se aplican a otros marcos de desarrollo web y entornos también en la nube.

Los desarrolladores que ya se están desarrollando para la nube pueden encontrar aquí ideas que le ayudarán a hacerlas más éxito. Cada capítulo de la serie se puede leer de forma independiente, por lo que puede seleccionar y elegir los temas que le interesa.

Cualquier persona que vio Scott Guthrie *Building Real World Cloud Apps with Azure* presentación y desea obtener más detalles e información actualizada descubrirán que aquí.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Patrones de desarrollo de nube

Este libro explica que trece recomienda patrones para el desarrollo de nube. "Pattern" se utiliza aquí en un sentido amplio para indicar el método recomendado para hacer cosas: la mejor forma de desarrollar, diseñar y codificar aplicaciones en la nube. Estos son los patrones clave que le ayudarán a "pertenecen a la fosa del éxito" si las sigue.

- [Automatizar todo](automate-everything.md).

    - Usar scripts para maximizar la eficacia y minimizar los errores en los procesos repetitivos.
    - Demostración: Scripts administración de Azure.
- [Control de código fuente](source-control.md). 

    - Configurar la estructura de bifurcación en el control de código fuente para facilitar el flujo de trabajo de DevOps.
    - Demostración: agregar secuencias de comandos de control de código fuente.
    - Demostración: mantener los datos confidenciales fuera de control de código fuente.
    - Demostración: usar Git en Visual Studio.
- [Integración y entrega continuas](continuous-integration-and-continuous-delivery.md). 

    - Automatizar compilación e implementación con cada comprobación de control de código fuente.
- [Prácticas recomendadas de desarrollo de Web](web-development-best-practices.md). 

    - Mantenga el nivel web sin estado.
    - Demostración: el escalado y escalado automático en las aplicaciones Web en Azure App Service.
    - Evitar el estado de sesión.
    - Usar una red CDN con una acción de reserva cuando la red CDN no está disponible.
    - Utilice el modelo de programación asincrónica.
    - Demostración: async en ASP.NET MVC y Entity Framework.
- [Inicio de sesión único](single-sign-on.md). 

    - Introducción a Azure Active Directory.
    - Demostración: crear una aplicación ASP.NET que usa Azure Active Directory.
- [Opciones de almacenamiento de datos](data-storage-options.md). 

    - Tipos de almacenes de datos.
    - Cómo elegir el almacén de datos adecuado.
    - Demostración: Base de datos SQL Azure.
- [Las estrategias de particionamiento de datos](data-partitioning-strategies.md). 

    - Datos de partición vertical, horizontal o ambos para facilitar el escalado de una base de datos relacional.
- [Almacenamiento de blobs no estructurados](unstructured-blob-storage.md). 

    - Store los archivos en la nube mediante el uso de blob service.
    - Demostración: usar blob storage en la aplicación Fix It.
- [Diseño para sobrevivir a errores](design-to-survive-failures.md). 

    - Tipos de errores.
    - Ámbito del error.
    - Descripción SLA.
- [Supervisión y telemetría](monitoring-and-telemetry.md). 

    - ¿Por qué se debe comprar una aplicación de telemetría y escribir su propio código para instrumentar la aplicación.
    - Demostración: New Relic para Azure
    - Demostración: registro del código en la aplicación Fix It.
    - Demostración: inserción de dependencias en la aplicación Fix It.
    - Demostración: compatibilidad de registro integrados en Azure.
- [Control de errores transitorios](transient-fault-handling.md). 

    - Usar lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios.
    - Demostración: reintento/retroceso en Entity Framework 6.
- [Almacenamiento en caché distribuido](distributed-caching.md). 

    - Mejorar la escalabilidad y reducir los costos de transacción de base de datos mediante el uso de almacenamiento en caché distribuido.
- [Patrón de trabajo centrado en colas](queue-centric-work-pattern.md). 

    - Habilitar la alta disponibilidad y mejorar la escalabilidad mediante los niveles web y trabajador de acoplamiento flexible.
    - Demostración: Colas de almacenamiento de Azure en la aplicación Fix It.
- [Más patrones de aplicación y las directrices de la nube](more-patterns-and-guidance.md).
- [Apéndice: Aplicación de ejemplo Reparar](the-fix-it-sample-application.md)

    - Problemas conocidos
    - Procedimientos recomendados
    - Cómo descargar, compilar, ejecutar e implementar.

Estos patrones se aplican a todos los entornos de nube, pero mostraremos mediante el uso de ejemplos basados en tecnologías de Microsoft y servicios, como Visual Studio, Team Foundation Service, ASP.NET y Azure.

En el resto de este capítulo presenta la aplicación de ejemplo Fix It y las aplicaciones Web en el entorno de nube de Azure App Service que se ejecuta la aplicación Fix It en.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>La corrección de aplicación de ejemplo

La mayoría de las capturas de pantalla y ejemplos de código se muestra en este libro electrónico se basa en la aplicación Fix It fue desarrollada originalmente por [Scott Guthrie](https://weblogs.asp.net/scottgu/) para demostrar los patrones de desarrollo de aplicaciones recomendada en la nube y prácticas.

![Corregir página principal de aplicación](introduction/_static/image1.png)

La aplicación de ejemplo es un elemento de trabajo sencillo sistema de vales. Cuando necesite solucionar un problema, crea un vale y la asigna a alguien etc. puede iniciar sesión y ver los vales asignados a ellos y marque vales como completada cuando se realiza el trabajo.

Es un proyecto web de Visual Studio estándar. Se basa en ASP.NET MVC y usa una base de datos de SQL Server. Puede ejecutar localmente en IIS Express y se puede implementar un sitio Web de Azure para ejecutar en la nube. Puede iniciar sesión con autenticación de formularios y una base de datos local o mediante un proveedor de redes sociales como Google. (Más adelante también mostraremos cómo iniciar sesión con una cuenta organizativa de Active Directory.)

![Inicie sesión en la página](introduction/_static/image2.png)

Una vez que ha iniciado sesión en crear un vale, asignarla a otra persona y cargar una imagen de lo que desea corregir.

![Crear una tarea Fix It](introduction/_static/image3.png)

![Corregirlo tarea creada](introduction/_static/image4.png)

Puede seguir el progreso de los elementos de trabajo que creó, consulte vales asignados para ver los detalles de vale y marcar los elementos como completados.

Se trata de una aplicación muy sencilla desde una perspectiva de características, pero verá cómo crearla para que se puede escalar a millones de usuarios y que sea resistente a cosas como errores de base de datos y las terminaciones de conexiones. También verá cómo crear un flujo de trabajo de desarrollo ágil y automatizados, lo que permite iniciar simple y que la aplicación sea mejor y mejor mediante la iteración del ciclo de desarrollo rápido y eficaz.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplicaciones Web en Azure App Service

El entorno de nube que se utiliza para la aplicación Fix It es un servicio de Azure que se llame a los sitios Web. Este servicio es una manera que puede hospedar su propia aplicación web en Azure sin tener que crear las máquinas virtuales y mantenerlos actualizados, instalar y configurar IIS, etcetera. Se hospede su sitio en nuestras máquinas virtuales y proporcionan automáticamente la copia de seguridad y recuperación y otros servicios para usted. El servicio sitios Web funciona con ASP.NET, Node.js, PHP y Python. Permite implementar rápidamente con Visual Studio, Web Deploy, FTP, Git o TFS. Suele ser tan solo unos segundos entre la hora de que iniciar una implementación y la hora en que la actualización está disponible a través de Internet. Es totalmente gratuito empezar a trabajar, y puede escalar verticalmente a medida que crece el tráfico.

En segundo plano, aplicaciones Web en Azure App Service proporciona una gran cantidad de componentes de la arquitectura y las características que se tendría que crear usted mismo si se va a hospedar un sitio web con IIS en sus propias máquinas virtuales. Un componente es un punto de conexión de implementación que automáticamente configura IIS y la aplicación se instala en tantas máquinas virtuales como desee a su sitio se ejecuta en.

![Servicios de implementación](introduction/_static/image5.png)

Cuando un usuario alcanza el sitio web, no aparezcan en las máquinas virtuales de IIS directamente, que atraviesan [enrutamiento de solicitud de aplicaciones (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) equilibradores de carga. Puede usar estos elementos con sus propios servidores, pero aquí la ventaja es que esté listo para usted automáticamente. Utiliza una heurística inteligente que toma en cuenta factores como la afinidad de sesión, la profundidad de cola en IIS, y el uso de CPU en cada máquina para dirigir el tráfico a las máquinas virtuales que hospedan el sitio web.

![Equilibrador de carga ARR](introduction/_static/image6.png)

Si una máquina deja de funcionar, Azure automáticamente extrae de la rotación, pone en marcha una nueva instancia de máquina virtual y comienza a dirigir el tráfico a la nueva instancia, todo ello con ningún tiempo de inactividad de la aplicación.

![Recuperación automática de error de la máquina](introduction/_static/image7.png)

Todo esto se realiza automáticamente. Todo lo que necesita hacer es crear un sitio web e implementar su aplicación, con Windows PowerShell, Visual Studio o el portal de administración de Azure.

Para un rápido y sencillo tutorial paso a paso que muestra cómo crear una aplicación web en Visual Studio e implementarlo en un sitio Web de Azure, consulte [empezar a trabajar con Azure y ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumen

Esta introducción ofrecimos una lista de temas que se tratará el libro, capturas de pantalla de la aplicación de ejemplo y una breve introducción a las aplicaciones Web en el entorno de nube de Azure App Service. Una de las grandes ventajas de desarrollo de aplicaciones en y para la nube es que resulta fácil de automatizar las tareas de desarrollo repetitivo, como la creación de un entorno de prueba e implementación de su código en él. Cómo hacer lo que es el asunto de la [siguiente capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obtener más información sobre los temas tratados en este capítulo, consulte los siguientes recursos.

Documentación:

- [Web Apps en Azure App Service](https://azure.microsoft.com/services/app-service/web/). Página de portal de documentación sobre Web Apps de Azure.
- [¿Las máquinas virtuales, servicios en la nube y aplicaciones Web: cuándo usar cada?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS tal como se muestra en este capítulo es sólo una de estas tres maneras puede ejecutar aplicaciones web en Azure. En este artículo se explica las diferencias entre las tres formas y proporciona orientación sobre cómo elegir cuál es la adecuada para su escenario. Al igual que los sitios Web, servicios en la nube es una característica de PaaS de Azure. Las máquinas virtuales son una característica de IaaS. Para obtener una explicación de PaaS en lugar de IaaS, vea el [opciones datos](data-storage-options.md#paasiaas) capítulo.

Vídeos:

- [Scott Guthrie se inicia en el paso 0: ¿qué es el sistema operativo en la nube de Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitectura de sitios Web: con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Aspectos internos de sitios Web de Azure con Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

> [!div class="step-by-step"]
> [Siguiente](automate-everything.md)
