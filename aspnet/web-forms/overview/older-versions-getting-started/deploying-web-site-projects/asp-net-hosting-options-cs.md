---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
title: Opciones (C#) de hospedaje de ASP.NET | Microsoft Docs
author: rick-anderson
description: Aplicaciones web ASP.NET están normalmente diseñadas, crean y probaron en un entorno de desarrollo local y deben implementarse en una o entorno de producción...
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 89a1d2bc-fdfd-4c5c-a3b0-49a08baaf63a
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/asp-net-hosting-options-cs
msc.type: authoredcontent
ms.openlocfilehash: 12d4e0e4332cf611e304799155b45f5d668da8f6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804192"
---
<a name="aspnet-hosting-options-c"></a>Opciones de hospedaje de ASP.NET (C#)
====================
por [Scott Mitchell](https://twitter.com/ScottOnWriting)

[Descargar PDF](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial01_Basics_cs.pdf)

> Las aplicaciones web ASP.NET son normalmente diseñadas, creadas y probadas en un entorno de desarrollo local y se deben implementar en un entorno de producción una vez que esté listo para la versión. Este tutorial proporciona una descripción general del proceso de implementación y sirve de introducción a esta serie de tutoriales.


## <a name="introduction"></a>Introducción

Las aplicaciones Web son normalmente diseñadas, creadas y probadas en un entorno de desarrollo que solo es accesible para los programadores trabajando en el sitio. Una vez que la aplicación está lista para publicarse, se mueve a un entorno de producción, donde el sitio puede tener acceso a cualquier persona en Internet. Este proceso de implementación presenta a una serie de desafíos:

- Un entorno de producción debe existir y estar correctamente instalada antes de que se puede implementar una aplicación ASP.NET; Además, el entorno de producción debe mantenerse al día con las últimas revisiones de seguridad.
- El conjunto correcto de archivos de marcado, los archivos de código y archivos auxiliares debe copiarse desde el entorno de desarrollo al entorno de producción. Para las aplicaciones controladas por datos, esto podría requerir copiar el esquema de base de datos o datos, así.
- Puede haber diferencias de configuración entre los dos entornos. El servidor correo electrónico o la cadena de conexión base de datos utilizado en el entorno de desarrollo probablemente será diferente del entorno de producción. Además, el comportamiento de la aplicación puede depender del entorno. Por ejemplo, cuando se produce un error en el desarrollo de los detalles del error se pueden mostrar en pantalla, pero cuando se produce un error en producción, se debe mostrar una página de error descriptivo en su lugar, y los detalles del error por correo electrónico a los desarrolladores.

Para obviar el primer desafío - configurar y mantener un entorno de producción - muchas personas y empresas externalizar sus entornos de producción para *proveedores de hospedaje de web*. Un proveedor de hospedaje web es una empresa que administra el entorno de producción en su nombre. Existen innumerables web proveedores de host, cada una con diferentes precios y niveles de servicio; Consulte la sección "Búsqueda de un proveedor de hospedaje Web" para obtener sugerencias sobre cómo localizar este tipo de proveedor de servicio.

Éste es el primero de una serie de tutoriales que examinan los pasos necesarios para implementar una aplicación web ASP.NET en un entorno de producción administrada por un proveedor de hospedaje web. En el transcurso de estos tutoriales, examinaremos:

- Qué archivos se deben implementarse en el proveedor de hospedaje web.
- Herramientas para simplificar el proceso de implementación.
- Cómo implementar una base de datos.
- Sugerencias para la implementación de una base de datos que usa [el proveedor de pertenencia basadas en SQL y las funciones](../../older-versions-security/membership/creating-the-membership-schema-in-sql-server-cs.md), además de las formas para imitar la herramienta de administración del sitio Web en un entorno de producción.
- Estrategias de actualización sin problemas de la base de datos en producción con los cambios realizados durante el desarrollo.
- Técnicas para el registro de errores que se produce en producción y maneras para notificar a los desarrolladores cuando se produce un error.

Estos tutoriales están orientados a ser conciso y proporcionar instrucciones paso a paso con una gran cantidad de capturas de pantalla que le guiarán a través del proceso visualmente. Este tutorial inaugural proporciona información general sobre el proceso de implementación de ASP.NET y consejos sobre cómo buscar un proveedor de hospedaje web. Comencemos.

## <a name="an-overview-of-the-aspnet-deployment-process"></a>Información general sobre el proceso de implementación de ASP.NET

En pocas palabras, la implementación de una aplicación de ASP.NET implica los tres pasos siguientes:

1. Configurar la aplicación web, el servidor web y la base de datos en el entorno de producción.
2. Sincronizar las páginas ASP.NET, los archivos de código, los ensamblados en la `Bin` carpetas y archivos de soporte técnico relacionados con HTML, como archivos CSS y JavaScript.
3. Sincronizar el esquema de base de datos o datos.

La información de configuración para una aplicación web se encuentra normalmente en el `Web.config` de archivos e incluye las cadenas de conexión de base de datos, criterios de control de errores, dirección URL de reescritura de reglas y la información del servidor de correo electrónico. A menudo, esta información es diferente para una aplicación en desarrollo frente a la misma aplicación en producción. Por ejemplo, al desarrollar una aplicación es mejor usar una base de datos de desarrollo para que no se está probando en la base de datos de producción. Como resultado, las cadenas de conexión de base de datos normalmente difieren entre las aplicaciones de desarrollo y producción. Debido a estas diferencias, parte de la implementación implica realizar cambios en la información de configuración de la aplicación web.

Además de los cambios de configuración de aplicación web, paso 1 también puede implicar la configuración del servidor web y la base de datos. Por ejemplo, si una página ASP.NET crea o elimina archivos de un directorio en el servidor web, a continuación, el servidor web debe configurarse para permitir que estas modificaciones del sistema de archivos. De forma similar, puede haber configuración de permiso o de autenticación que deben realizarse en la base de datos.


Paso 2 implica sincronizando el conjunto de páginas ASP.NET esenciales y archivos de compatibilidad entre los entornos de desarrollo y producción. El conjunto determinado de ASP. Archivos de red que deben sincronizarse entre los dos entornos depende del tipo de proyecto creado en Visual Studio y es la explicación en el siguiente tutorial, [ *determinar qué archivos se deben implementarse*](determining-what-files-need-to-be-deployed-cs.md). Los tutoriales terceros y cuarto - [ *implementar su sitio mediante FTP* ](deploying-your-site-using-an-ftp-client-cs.md) y [ *implementar su sitio con Visual Studio* ](deploying-your-site-using-visual-studio-cs.md) -examinar diferentes herramientas y técnicas para la sincronización de estos archivos.

Al compilar aplicaciones controladas por datos hay normalmente dos bases de datos que se va a usar: uno para desarrollo y otro en producción. Durante el desarrollo, esquema de la base de datos del desarrollo se puede modificar para incluir nuevas tablas, columnas, procedimientos almacenados y desencadenadores, o se puede modificar para quitar o cambiar el nombre de los objetos de base de datos existente. Entre el tiempo que se realizan estos cambios y la hora en que la aplicación se implementa en producción, las bases de datos de desarrollo y producción no están sincronizados. Este asincronía debe solucionarse durante el proceso de implementación. Estos desafíos se examinarán en tutoriales futuros.

## <a name="finding-a-web-host-provider"></a>Buscar un proveedor de hospedaje Web

Las aplicaciones ASP.NET se pueden implementar en cualquier servidor web que tiene el .NET Framework e instalado Internet Information Services (IIS). Puede hospedar un sitio desde su equipo, suponiendo que tenía una conexión de banda ancha a Internet e informado de cómo configurar el enrutador para permitir solicitudes web entrantes. También puede hospedar un sitio desde un equipo en una intranet, igual que muchas empresas. Sin embargo, el enfoque de estos tutoriales, hospeda el sitio Web con un proveedor de hospedaje web.

> [!NOTE]
> [IIS](https://www.iis.net/) es servidor de web empresarial de Microsoft. Se suministra con las ediciones Home no son de Windows, como Windows Server 2008 y en ciertas ediciones de Windows Vista. No es necesario instalar IIS para dar servicio a las aplicaciones ASP.NET en un entorno de desarrollo, como Visual Studio incluye el servidor Web de desarrollo de ASP.NET. Sin embargo, el servidor Web de desarrollo de ASP.NET solo acepta conexiones locales y, por tanto, no se puede usar en un entorno de producción.


Antes de implementar el sitio de un proveedor de hospedaje web primero debe decidir qué hacer negocios con la empresa. Existen innumerables hospedaje compañías en marketplace. más de cinco millones de resultados de una búsqueda de "empresa de hospedaje web". ¿Cómo encontrar uno que sea adecuada para usted? El motor de búsqueda es un buen punto de partida, como son sitios Web como [TopHosts](http://www.tophosts.com/) y [HostCritique](http://www.hostcritique.net/), que comparar y contrastar los distintos servicios de hospedaje. También es recomendable solicitar ninguna recomendación; sus compañeros y compañeros de trabajo También puede pedir recomendaciones en la [hospedaje foro abierto](https://forums.asp.net/158.aspx) en el [foros de ASP.NET](https://forums.asp.net/).

Las empresas de hospedaje Web normalmente ofrecen planes de hospedaje compartidos y los planes de hospedaje dedicado. Con un host de servidor web único docenas o incluso cientos de diferentes sitios Web de hospedaje compartido. Con hospedaje dedicado arrendar un equipo de la compañía que actúa el sitio y el sitio por sí solo. Un plan de hospedaje compartido puede incluir compatibilidad con páginas ASP.NET, la capacidad de trabajar con bases de datos de Microsoft Access, 5 GB de espacio en disco y 100 GB de tráfico mensual de ancho de banda para 9,95 dólares al mes. Otro plan de hospedaje compartido puede incluir compatibilidad con páginas ASP.NET, acceso para el servidor de base de datos de Microsoft SQL Server 2008, 10 GB de espacio en disco y 250 GB de tráfico mensual de ancho de banda por 19,95 dólares al mes. Los planes de hospedaje dedicado son normalmente mucho más caros, administración de costos varios cientos de dólares por mes, pero ofrecen un rendimiento mejor y más control que compartido las opciones de hospedaje. ¿Qué plan que elija depende de su presupuesto, la cantidad de tráfico recibe su sitio Web y las características que prevé que necesitará.

Dos consideraciones importantes al elegir un proveedor de hospedaje web son la calidad de servicio y servicio al cliente. Si tiene alguna pregunta o un problema de configuración, ¿cuánto tiempo tarda envíe su problema al departamento de soporte técnico del host web hasta que obtenga una respuesta? ¿Grado de confiabilidad son los servicios de la empresa? ¿Tienen con frecuencia las interrupciones de la base de datos? ¿Con qué frecuencia el servidor de correo electrónico queden sin conexión? Siempre puede solicitar una empresa para proporcionar detalles sobre su tiempo de actividad y realizar consultas sobre su directiva de servicio al cliente, pero es una forma más segura solicitar los comentarios de clientes actuales y pasados, lo que puede hacer a través de servidores de listas de correo electrónico, grupos de noticias y foros en línea .

> [!NOTE]
> Algunas compañías de hospedaje web centran sus negocios en una pila de tecnología en particular, como .NET o [LAMP](http://en.wikipedia.org/wiki/LAMP_stack) (**L** inux, **A** pache, **M** ySQL, y **P** HP), así que asegúrese de que la empresa que seleccionar hospeda las aplicaciones ASP.NET. Compruebe también que admiten la versión de ASP.NET utiliza para compilar la aplicación. Y si está creando una aplicación controlada por datos, asegúrese de que el host web ofrece el mismo servidor de base de datos y la misma versión que está usando.


## <a name="summary"></a>Resumen

Las aplicaciones web ASP.NET son normalmente diseñadas, creadas y probadas en un entorno de desarrollo local. Una vez que una versión está lista para publicarse, se mueve a un entorno de producción. Aunque es posible hospedar sitios Web ASP.NET en su equipo personal o en servidores dentro de su empresa, muchas empresas e individuos elegir externalizar sus hospeda un proveedor de hospedaje web.

Esta serie de tutoriales examina los pasos para implementar una aplicación ASP.NET en un proveedor de hospedaje web, exploración de los desafíos comunes. Este tutorial ofrece una descripción general del proceso de implementación de ASP.NET y dio sugerencias para buscar un proveedor de host web adecuado. El siguiente tutorial examina lo que deben copiarse en el entorno de producción al implementar su sitio Web archivos relacionados con ASP.NET.

Feliz programación.

### <a name="special-thanks-to"></a>Agradecimientos especiales a...

Esta serie de tutoriales ha sido revisada por muchos revisores útiles. Revisor de clientes potencial para este tutorial era Teresa Murphy. ¿Está interesado en leer mi próximos artículos de MSDN? Si es así, envíeme una línea en [ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com).

> [!div class="step-by-step"]
> [Siguiente](determining-what-files-need-to-be-deployed-cs.md)
