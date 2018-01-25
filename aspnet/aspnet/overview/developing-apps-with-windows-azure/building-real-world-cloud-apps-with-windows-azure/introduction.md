---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
title: Compilar aplicaciones de nube reales con Azure | Documentos de Microsoft
author: MikeWasson
description: "Este libro electrónico le guía a través de un enfoque basado en patrones para compilar soluciones del mundo real en la nube. Los patrones se aplican al proceso de desarrollo, así como para un..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/12/2014
ms.topic: article
ms.assetid: accfa16a-ab15-4c26-9ad4-babdc2a77d2e
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction
msc.type: authoredcontent
ms.openlocfilehash: 4de0b52e0b4ae7ce00e7b07bce2decfc5068964a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="building-real-world-cloud-apps-with-azure"></a>Creación de aplicaciones de nube reales con Azure
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> Este libro electrónico le guía a través de un enfoque basado en patrones para compilar soluciones del mundo real en la nube. Los patrones se aplican en el proceso de desarrollo, así como a la arquitectura y las prácticas de codificación.
> 
> El contenido se basa en una presentación desarrollado por Scott Guthrie y ofrecidas por él en la conferencia de desarrolladores Noruego (NDC) en junio de 2013 ([parte 1](http://vimeo.com/68215538), [parte 2](http://vimeo.com/68215602)) y en Microsoft Tech Ed Australia en Septiembre de 2013 ([parte 1](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR324), [parte 2](https://channel9.msdn.com/Events/TechEd/Australia/2013/AZR325)). [Muchas otras](more-patterns-and-guidance.md#acknowledgments) actualizado y aumentar el contenido durante la transición de vídeo en forma escrita.


## <a name="intended-audience"></a>Destinatarios

Los desarrolladores que tiene curiosidad sobre el desarrollo de la nube, teniendo en cuenta un cambio a la nube, o son nuevos en la nube de desarrollo aquí encontrará una descripción concisa de los conceptos y procedimientos recomendados que se necesitan conocer más importantes. Los conceptos se ilustran con ejemplos concretos y cada capítulo está vinculado a otros recursos para obtener información más detallada. Los ejemplos y los vínculos a recursos adicionales son para los entornos de Microsoft y servicios, pero los principios ilustrados se aplican a otros entornos de desarrollo web y entornos también en la nube.

Los desarrolladores que ya se están desarrollando para la nube pueden encontrar aquí ideas que le ayudarán a hacerlos más correcta. Cada capítulo de la serie se puede leer de forma independiente, para que pueda elegir y elija temas que le interesa.

Cualquier persona que observar de Guthrie *compilar aplicaciones de nube de mundo Real con Azure* presentación y desea que encontrarán más detalles e información actualizada, que aquí.

<a id="patterns"></a>
## <a name="cloud-development-patterns"></a>Patrones de desarrollo de nube

Este libro electrónico explica que trece recomiendan patrones para el desarrollo de nube. "Modelo" se utiliza aquí en un sentido amplio para indicar el método recomendado para hacer cosas: la mejor manera de ir sobre el desarrollo, diseñar y codificar aplicaciones en la nube. Estos son los patrones clave que le ayudarán a "pertenecen a las pit de éxito" si se seguirlas.

- [Automatizar todo](automate-everything.md).

    - Usar scripts para maximizar la eficacia y minimizar los errores en los procesos repetitivos.
    - Demostración: Secuencias de comandos administración de Azure.
- [Control de código fuente](source-control.md). 

    - Configurar la estructura de bifurcación de control de código fuente para facilitar el flujo de trabajo de DevOps.
    - Demostración: agregar secuencias de comandos de control de código fuente.
    - Demostración: mantener los datos confidenciales fuera del control de código fuente.
    - Demostración: usar Git en Visual Studio.
- [Integración continua y entrega](continuous-integration-and-continuous-delivery.md). 

    - Automatizar la compilación e implementación con cada comprobación de control de código fuente.
- [Prácticas recomendadas de desarrollo de Web](web-development-best-practices.md). 

    - Mantener el nivel web sin estado.
    - Demostración: ajuste de escala y la escala automática en las aplicaciones Web en el servicio de aplicación de Azure.
    - Evitar el estado de sesión.
    - Utilice una CDN.
    - Utilice el modelo de programación asincrónica.
    - Demostración: asincrónico en ASP.NET MVC y Entity Framework.
- [Inicio de sesión único](single-sign-on.md). 

    - Introducción a Azure Active Directory.
    - Demostración: crear una aplicación ASP.NET que usa Azure Active Directory.
- [Opciones de almacenamiento de datos](data-storage-options.md). 

    - Tipos de almacenes de datos.
    - Cómo elegir el almacén de datos adecuado.
    - Demostración: Base de datos SQL Azure.
- [Estrategias de partición de datos](data-partitioning-strategies.md). 

    - Crear particiones de datos vertical, horizontal o ambos para facilitar el ajuste de escala en una base de datos relacional.
- [Almacenamiento de blobs no estructurados](unstructured-blob-storage.md). 

    - Almacenar archivos en la nube mediante el servicio blob.
    - Demostración: con almacenamiento de blobs en la aplicación repararlo.
- [Diseño de supervivencia errores](design-to-survive-failures.md). 

    - Tipos de errores.
    - Ámbito de error.
    - SLA de descripción.
- [Supervisión y telemetría](monitoring-and-telemetry.md). 

    - ¿Por qué se debe comprar una aplicación de telemetría y escribir su propio código para instrumentar la aplicación.
    - Demostración: New Relic de Azure
    - Demostración: el registro del código en la aplicación repararlo.
    - Demostración: inserción de dependencias en la aplicación repararlo.
    - Demostración: la compatibilidad de registro integrados en Azure.
- [Control de errores transitorios](transient-fault-handling.md). 

    - Use lógica de reintento/interrupción inteligente para mitigar el efecto de los errores transitorios.
    - Demostración: reintento/retroceso en Entity Framework 6.
- [Caching distribuido](distributed-caching.md). 

    - Mejorar la escalabilidad y reducir los costes de transacción de base de datos mediante el uso de almacenamiento en caché distribuido.
- [Patrón de trabajo centrado en cola](queue-centric-work-pattern.md). 

    - Habilitar la alta disponibilidad y mejorar la escalabilidad mediante niveles de web y de trabajo de acoplamiento flexible.
    - Demostración: Colas de almacenamiento de Azure en la aplicación repararlo.
- [La nube más patrones de aplicación y las directrices](more-patterns-and-guidance.md).
- [Apéndice: Aplicación de ejemplo Reparar](the-fix-it-sample-application.md)

    - Problemas conocidos
    - Procedimientos recomendados
    - Cómo descargar, crear, ejecutar e implementar.

Estos patrones se aplican a todos los entornos de nube, pero mostraremos mediante el uso de ejemplos basados en tecnologías de Microsoft y servicios, como Visual Studio, Team Foundation Service, ASP.NET y Azure.

Este resto de este capítulo presenta la aplicación de ejemplo repararlo y las aplicaciones Web en el entorno de nube de servicio de aplicaciones de Azure que se ejecuta la aplicación repararlo en.

<a id="fixit"></a>
## <a name="the-fix-it-sample-application"></a>La solución aplicación de ejemplo

La mayoría de las capturas de pantalla y ejemplos de código se muestra en este libro electrónico se basa en la aplicación repararlo originalmente desarrollada por [Scott Guthrie](https://weblogs.asp.net/scottgu/) para mostrar patrones de desarrollo de aplicaciones de nube recomendado y procedimientos recomendados.

![Corregir página de inicio de aplicación](introduction/_static/image1.png)

La aplicación de ejemplo es un sistema de identificación del elemento de trabajo simple. Si necesita solucionar un problema, crea un vale y la asigna a alguien etc. puede iniciar sesión y ver los vales que se asignan a estos y marque vales como completada cuando se realiza el trabajo.

Es un proyecto web de Visual Studio estándar. Se basa en ASP.NET MVC y usa una base de datos de SQL Server. Se puede ejecutar localmente en IIS Express y se pueden implementar en un sitio Web de Azure para que se ejecute en la nube. Puede iniciar sesión con autenticación de formularios y una base de datos local o mediante un proveedor de redes sociales, como Google. (Más adelante también mostraremos cómo iniciar sesión con una cuenta de organización de Active Directory.)

![Inicie sesión en la página](introduction/_static/image2.png)

Una vez que ha iniciado sesión en puede crear un vale, asignar a otra persona y cargar una imagen de lo que desea que la corrección.

![Crear una tarea repararlo](introduction/_static/image3.png)

![Corregirlo tarea creada](introduction/_static/image4.png)

Puede realizar un seguimiento del progreso de los elementos de trabajo que ha creado, vea vales asignados para ver detalles del vale y marcar los elementos como completada.

Esta es una aplicación muy simple desde una perspectiva de características, pero podrá ver cómo compilarlo para que se puede escalar a millones de usuarios y será resistente a los elementos tales como errores de base de datos y las terminaciones de conexiones. También verá cómo crear un flujo de trabajo de desarrollo ágil y automatizada, lo que permite iniciar simple y hacer que la aplicación mejor y mejor recorriendo el ciclo de desarrollo rápido y eficaz.

<a id="waws"></a>
## <a name="web-apps-in-azure-app-service"></a>Aplicaciones Web en el servicio de aplicaciones de Azure

El entorno de nube que se utiliza para la aplicación repararlo es un servicio de Azure que se llame a los sitios Web. Este servicio es una manera que puede hospedar su propia aplicación web en Azure sin tener que crear las máquinas virtuales y mantenerlos actualizados, instalar y configurar IIS, etcetera. Se hospedar su sitio en nuestro máquinas virtuales y proporcionan automáticamente la copia de seguridad y recuperación y otros servicios. El servicio sitios Web funciona con ASP.NET, Node.js, PHP y Python. Permite implementar rápidamente con Visual Studio, Web Deploy, FTP, Git o TFS. Suele ser tan solo unos segundos entre la hora de que iniciar una implementación y el tiempo que la actualización está disponible a través de Internet. Es totalmente gratuito empezar a trabajar y se puede escalar a medida que crece el tráfico.

En segundo plano, las aplicaciones Web en el servicio de aplicación de Azure proporciona una gran cantidad de componentes de la arquitectura y las características que se deben compilar por sí mismo si se va a hospedar un sitio web con IIS en las propias máquinas virtuales. Un componente es un extremo de implementación que configura IIS e instala la aplicación en máquinas virtuales tantas como desee para el sitio se ejecuta automáticamente.

![Servicios de implementación](introduction/_static/image5.png)

Cuando un usuario visita el sitio web, no alcancen directamente las máquinas virtuales de IIS, pasan a través de [enrutamiento de solicitud de aplicaciones (ARR)](https://www.iis.net/downloads/microsoft/application-request-routing) equilibradores de carga. Puede usar junto con sus propios servidores, pero aquí la ventaja es que esté listo para automáticamente. Usan una heurística inteligente que tiene en cuenta factores como la afinidad de la sesión, la profundidad de cola en IIS, y el uso de CPU en cada equipo para dirigir el tráfico a las máquinas virtuales que hospedan el sitio web.

![Equilibrador de carga ARR](introduction/_static/image6.png)

Si un equipo deja de funcionar, Azure automáticamente extrae de la rotación, gira una nueva instancia de máquina virtual y dirigir el tráfico a la nueva instancia--todos con ningún tiempo de inactividad de la aplicación inicia.

![Recuperación automática de error del equipo](introduction/_static/image7.png)

Todo esto se realiza automáticamente. Todo lo que necesita hacer es crear un sitio web e implementar la aplicación, con Windows PowerShell, Visual Studio o el portal de administración de Azure.

Para un rápida y sencillo tutorial paso a paso que muestra cómo crear una aplicación web en Visual Studio e implementarlo en un sitio Web de Azure, consulte [Introducción a Azure y ASP.NET](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/).

<a id="summary"></a>
## <a name="summary"></a>Resumen

Esta introducción ha proporcionado una lista de temas que se tratará el libro, capturas de pantalla de la aplicación de ejemplo y una breve descripción de las aplicaciones Web en el entorno de nube de servicio de aplicaciones de Azure. Una de las grandes ventajas de desarrollo de aplicaciones en y para la nube es que resulta fácil de automatizar las tareas repetitivas de desarrollo, como crear un entorno de prueba e implementar el código en él. Cómo hacer que es el asunto de la [siguiente capítulo](automate-everything.md).

## <a name="resources"></a>Recursos

Para obtener más información sobre los temas tratados en este capítulo, consulte los siguientes recursos.

Documentación:

- [Aplicaciones de servicio de aplicaciones de Azure Web](https://azure.microsoft.com/services/app-service/web/). Página del portal de Azure documentación acerca de las aplicaciones Web.
- [¿Las aplicaciones Web, servicios en la nube y las máquinas virtuales: cuándo usar cada?](https://azure.microsoft.com/documentation/articles/choose-web-site-cloud-service-vm/) WAWS tal y como se muestra en este capítulo es solo una de estas tres maneras puede ejecutar aplicaciones web en Azure. En este artículo se explica las diferencias entre las tres formas y proporciona una orientación sobre cómo elegir cuál es el adecuado para su escenario. Al igual que los sitios Web, servicios en la nube es una característica de PaaS de Azure. Las máquinas virtuales son una característica de IaaS. Para obtener una explicación de PaaS frente a IaaS, vea el [opciones de datos](data-storage-options.md#paasiaas) capítulo.

Vídeos:

- [Scott Guthrie comienza en el paso 0: ¿qué es el sistema operativo en la nube de Azure?](https://azure.microsoft.com/documentation/videos/what-is-the-cloud-os-scottgu/)
- [Arquitectura de sitios Web - con Stefan Schackow](https://azure.microsoft.com/documentation/videos/why-azure-web-sites-plus-architecture/).
- [Funcionamiento interno de sitios Web de Azure con Nir Mashkowski](https://channel9.msdn.com/Shows/Web+Camps+TV/Windows-Azure-Web-Sites-Internals-with-Nir-Mashkowski).

>[!div class="step-by-step"]
[Siguiente](automate-everything.md)
