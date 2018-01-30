---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
title: Opciones (VB) de hospedaje de ASP.NET | Documentos de Microsoft
author: rick-anderson
description: "Las aplicaciones web ASP.NET se diseñan habitualmente, crean y probaron en un entorno de desarrollo local y deben implementarse para un o del entorno de producción..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 492f5ae2-bad7-4107-89a9-f04a9525dee7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-vb
msc.type: authoredcontent
ms.openlocfilehash: 54bac82a96a35d871d764849856c8e31f6570666
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/30/2018
---
<a name="aspnet-hosting-options-vb"></a>Opciones de hospedaje de ASP.NET (VB)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descarga de PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_vb.pdf)

> Las aplicaciones web ASP.NET normalmente se diseñado, creadas y probadas en un entorno de desarrollo local y tienen que implementarse en un entorno de producción una vez que esté lista para el lanzamiento. Este tutorial proporciona una descripción general del proceso de implementación y actúa como una introducción a esta serie de tutoriales.


## <a name="introduction"></a>Introducción

Aplicaciones Web normalmente se diseñado, creadas y probadas en un entorno de desarrollo que solo es accesible para los programadores que trabajan en el sitio. Una vez que la aplicación está lista para su publicación, se mueve a un entorno de producción en el sitio puede tener acceso cualquier persona en Internet. Este proceso de implementación presenta a una serie de desafíos:

- Un entorno de producción debe existir y ser correctamente el programa de instalación para poder implementar una aplicación ASP.NET; Además, el entorno de producción debe mantenerse al día con las últimas revisiones de seguridad.
- El conjunto correcto de archivos de marcado, los archivos de código y los archivos auxiliares debe copiarse desde el entorno de desarrollo al entorno de producción. Para las aplicaciones controladas por datos, esto podría requerir copiar el esquema de base de datos o datos, también.
- Puede haber diferencias de configuración entre los dos entornos. El servidor correo electrónico o de cadena de conexión base de datos utilizado en el entorno de desarrollo probablemente será diferente del entorno de producción. Es más, el comportamiento de la aplicación puede depender del entorno. Por ejemplo, cuando se produce un error en el desarrollo de los detalles del error se pueden mostrar en pantalla, pero cuando se produce un error en producción, se debe mostrar una página de error fácil de usar en su lugar, y los detalles del error por correo electrónico a los desarrolladores de software.

Para excluir el primer desafío - configurar y mantener un entorno de producción - muchas personas y empresas externalizar sus entornos de producción a *web de proveedores de hospedaje*. Un proveedor de hospedaje web es una empresa que administra el entorno de producción en su nombre. Existen incontables web proveedores de host, cada uno con distintos de los precios y niveles de servicio; vea la sección "Buscar un proveedor de hospedaje Web" para obtener sugerencias acerca de la ubicación de este tipo de proveedor de servicio.

Es el primer elemento de una serie de tutoriales que examinan los pasos necesarios para implementar una aplicación web ASP.NET en un entorno de producción administrado por un proveedor de hospedaje web. En el transcurso de estos tutoriales examinaremos:

- ¿Qué archivos deben implementarse en el proveedor de hospedaje web.
- Herramientas para simplificar el proceso de implementación.
- Cómo implementar una base de datos.
- Sugerencias para la implementación de una base de datos que usa [el proveedor de pertenencia basadas en SQL y los Roles](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), junto con las formas para imitar la herramienta de administración de sitios Web en un entorno de producción.
- Estrategias para la actualización sin problemas de la base de datos de producción con los cambios realizados durante el desarrollo.
- Técnicas para el registro de errores que se produce en producción y formas para notificar a los desarrolladores cuando se produce un error.

Estos tutoriales están orientados a ser conciso y para proporcionar instrucciones paso a paso con una gran cantidad de capturas de pantalla para guiarle a través del proceso visualmente. Este tutorial inaugural proporciona información general sobre el proceso de implementación de ASP.NET y consejos sobre cómo buscar un proveedor de hospedaje web. Comencemos.

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Información general sobre el proceso de implementación de ASP.NET

En pocas palabras, la implementación de una aplicación de ASP.NET implica los tres pasos siguientes:

1. Configurar la aplicación web, el servidor web y la base de datos en el entorno de producción.
2. Sincronizar las páginas ASP.NET, archivos de código, los ensamblados en la `Bin` carpetas y archivos de soporte técnico relacionados con HTML como los archivos CSS y JavaScript.
3. Sincronizar el esquema de base de datos o datos.

La información de configuración para una aplicación web normalmente se encuentra en la `Web.config` de archivos e incluye cadenas de conexión de base de datos, criterios de control de errores, las reglas y la información del servidor de correo electrónico de la reconfiguración de direcciones URL. A menudo esta información es diferente para una aplicación en desarrollo frente a la misma aplicación en producción. Por ejemplo, al desarrollar una aplicación es mejor usar una base de datos de desarrollo para que no se está probando en la base de datos de producción. Como resultado, las cadenas de conexión de base de datos normalmente difieren entre las aplicaciones de desarrollo y producción. Debido a estas diferencias, parte de la implementación implica realizar cambios en la información de configuración de la aplicación web.

Además de los cambios de configuración de aplicación web, paso 1 también puede implicar la configuración para el servidor web y la base de datos. Por ejemplo, si una página ASP.NET crea o elimina archivos de un directorio en el servidor web, a continuación, el servidor web debe configurarse para permitir que estas modificaciones del sistema de archivos. De forma similar, puede haber configuración de autenticación o permisos que deben realizarse en la base de datos.


Paso 2 implica la sincronización el conjunto de páginas ASP.NET esenciales y archivos de compatibilidad entre los entornos de desarrollo y producción. El conjunto determinado de ASP. Archivos relacionados con la red que deben estar sincronizada entre los dos entornos depende del tipo de proyecto creado en Visual Studio y el análisis en el tutorial siguiente, es  *[determinar qué archivos deben implementarse](determining-what-files-need-to-be-deployed-vb.md)*. Los tutoriales terceros y cuarto -  *[implementar su sitio mediante FTP](deploying-your-site-using-an-ftp-client-vb.md)*y  *[implementar su sitio con Visual Studio](deploying-your-site-using-visual-studio-vb.md)*  -examinar diferentes herramientas y técnicas de sincronización de estos archivos.

Al compilar aplicaciones orientadas a datos hay normalmente dos bases de datos que se va a usar: uno para desarrollo y otro en producción. Durante el desarrollo, esquema de la base de datos del desarrollo puede modificarse para incluir nuevas tablas, columnas, procedimientos almacenados y desencadenadores, o puede modificar para quitar o cambiar el nombre de los objetos de base de datos existentes. Entre el tiempo que se realizan estos cambios y la hora en que la aplicación se implementa en producción, las bases de datos de desarrollo y producción están sincronizados. Este asincronía debe solucionarse durante el proceso de implementación. Estos desafíos se examinarán en tutoriales futuros.

## <a name="finding-a-web-host-provider"></a>Buscar un proveedor de hospedaje Web

Las aplicaciones de ASP.NET se pueden implementar en cualquier servidor web que tiene .NET Framework e instalado Internet Information Services (IIS). Puede hospedar un sitio desde su equipo personal, suponiendo que tenía una conexión de banda ancha a Internet y los conocimientos de cómo configurar el enrutador para permitir las solicitudes web entrantes. También puede hospedar un sitio desde un equipo en una intranet, igual que muchas empresas. Sin embargo, el enfoque de estos tutoriales, hospeda el sitio Web con un proveedor de hospedaje web.

> [!NOTE]
> [IIS](https://www.iis.net/) es el servidor de web de nivel empresarial de Microsoft. Se distribuye con las ediciones Home no son de Windows, como Windows Server 2008 y en ciertas ediciones de Windows Vista. No es necesario instalar IIS para dar servicio a las aplicaciones de ASP.NET en un entorno de desarrollo, como Visual Studio incluye el servidor Web de desarrollo de ASP.NET. Sin embargo, el servidor Web de desarrollo de ASP.NET solo acepta las conexiones locales y, por tanto, no se puede usar en un entorno de producción.


Antes de implementar el sitio de un proveedor de hospedaje web en primer lugar debe decidir qué empresa hacer negocios con. Existen incontables hospedaje compañías en marketplace; más de cinco millones de resultados de una búsqueda de "empresa de hospedaje web". ¿Cómo puede encontrar uno que sea adecuado para usted? El motor de búsqueda es un buen punto de partida, ya que es sitios Web como [TopHosts](http://www.tophosts.com/) y [HostCritique](http://www.hostcritique.net/), que comparar y contrastar varios servicios de hospedaje. También aconsejo piden sus compañeros y compañeros de trabajo ninguna recomendación; También puede pedir recomendaciones en la [hospedaje foro abierto](https://forums.asp.net/158.aspx) aquí en el [foros de ASP.NET](https://forums.asp.net/).

Normalmente, compañías de hospedaje Web ofrecen planes de hospedaje compartidos y dedicado planes de hospedaje. Con compartido hospeda un host de servidor web único docenas o incluso cientos de sitios Web diferentes. Con el hospedaje dedicado concesión un equipo de la compañía que actúa el sitio y el sitio independiente. Un plan de hospedaje compartido podría incluir soporte para las páginas ASP.NET, la capacidad de trabajar con bases de datos de Microsoft Access, 5 GB de espacio en disco y de tráfico mensual de ancho de banda de 100 GB por 9,95 dólares al mes. Otro plan de hospedaje compartido puede incluir compatibilidad con páginas ASP.NET, de acceso para el servidor de base de datos de Microsoft SQL Server 2008, 10 GB de espacio en disco y 250 GB de tráfico mensual de ancho de banda por 19,95 dólares al mes. Planes de hospedaje dedicado son normalmente mucho más caros, administración de costos varios cientos de dólares por mes, pero ofrecen un rendimiento mejor y más control que compartido opciones de hospedaje. El plan que seleccione depende de su presupuesto, la cantidad de tráfico recibe su sitio Web, y las características que se prevé necesitará.

Dos consideraciones importantes al elegir un proveedor de hospedaje web son la calidad de servicio y el servicio al cliente. Si tiene alguna pregunta o un problema de configuración, ¿cuánto tiempo tarda envíe el problema al departamento de soporte técnico del host web hasta que obtenga una respuesta? ¿Grado de confiabilidad son servicios de la compañía? ¿Tienen las interrupciones de la base de datos con frecuencia? ¿La frecuencia con su servidor de correo electrónico queden sin conexión? Puede Preguntar siempre una empresa para proporcionar detalles sobre su tiempo de actividad y realizar consultas sobre su directiva de servicio al cliente, pero una forma más segura es solicitar los comentarios de los clientes actuales y pasados, lo que puede hacer a través de servidores de listas de correo electrónico, grupos de noticias y foros en línea .

> [!NOTE]
> Algunas compañías de hospedaje web centran su negocio en una pila de tecnología específica, como .NET o [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, y **P** HP), por lo tanto, asegúrese de que la empresa selecciona hospeda las aplicaciones ASP.NET. Compruebe también que admiten la versión de ASP.NET que usa para compilar la aplicación. Y si está compilando una aplicación controlada por datos, asegúrese de que el host web ofrece el mismo servidor de base de datos y la misma versión que está usando.


## <a name="summary"></a>Resumen

Aplicaciones web ASP.NET son normalmente diseñadas, creadas y probadas en un entorno de desarrollo local. Una vez que una versión está lista para el lanzamiento, se mueve a un entorno de producción. Aunque es posible para hospedar sitios Web ASP.NET en el equipo o en servidores dentro de su empresa, eligen muchas empresas y usuarios externalizar sus hospedaje a un proveedor de hospedaje web.

Esta serie de tutoriales examina los pasos para implementar una aplicación ASP.NET para un proveedor de hospedaje web, explorar los desafíos más comunes. Este tutorial ofrece una descripción general del proceso de implementación de ASP.NET y dio sugerencias para buscar un proveedor de host web adecuado. El siguiente tutorial examina qué archivos relacionados con ASP.NET deben copiarse en el entorno de producción al implementar el sitio Web.

Feliz programación.

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales se revisó por varios revisores útiles. Revisor inicial para este tutorial era Teresa Murphy. ¿Está interesado en revisar mi próximos artículos MSDN? Si es así, me quitar una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

>[!div class="step-by-step"]
[Anterior](users-and-roles-on-the-production-website-cs.md)
[Siguiente](determining-what-files-need-to-be-deployed-vb.md)
