---
uid: whitepapers/overview
title: Notas del producto | Documentos de Microsoft
author: rick-anderson
description: En esta página encontrará notas del producto para ayudarle a instalar y configurar ASP.NET y para ayudarle a escribir aplicaciones ASP.NET seguras, rápidas y flexibles.
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/15/2011
ms.topic: article
ms.assetid: d5e79470-01f2-4d65-8077-11c3e10a6784
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers
msc.type: content
ms.openlocfilehash: ba9fda509605025754dc9753266f86585f38b089
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/10/2018
ms.locfileid: "28049270"
---
<a name="whitepapers"></a>Notas del producto
====================
> En esta página encontrará notas del producto para ayudarle a instalar y configurar ASP.NET y para ayudarle a escribir aplicaciones ASP.NET seguras, rápidas y flexibles.
> 
> - [ASP.NET 4](#aspnet4)
> - [Artículos técnicos de la seguridad ASP.NET](#security)
> - [Instalación y notas del producto del programa de instalación](#setup)
> - [Notas del producto SQL Server](#sql)
> - [Notas del producto general](#general)


<a id="aspnet4"></a>
## <a name="aspnet-4"></a>ASP.NET 4

Información relacionada con ASP.NET 4 y Visual Studio 2010.

[Notas de la versión de ASP.NET MVC 4](mvc4-beta-release-notes.md "notas de la versión de mvc4")

Este documento describe las nuevas características y mejoras presentadas en ASP.NET MVC 4 Developer Preview para Visual Studio 2010, así como notas sobre la instalación y problemas conocidos.

[Notas de la versión de ASP.NET MVC 3](mvc3-release-notes.md "notas de la versión de mvc3")

Este documento describe nuevas características y mejoras introducidas en ASP.NET MVC 3, así como notas sobre la instalación y problemas conocidos.

[ASP.NET 4 y Visual Studio 2010 Introducción al desarrollo Web](aspnet4/index.md "aspnet4")

Próximamente muchos cambios interesantes para ASP.NET en .NET Framework versión 4. Este documento proporciona información general de muchas de las nuevas características que se incluyen en la próxima versión.

[Cambia de separación de la versión Beta 2 de ASP.NET 4](aspnet4/breaking-changes.md "cambios importantes")

Este documento describe los cambios que se han realizado para la versión de .NET Framework versión 4 de Beta 2 (es decir, la versión Beta 2 de ASP.NET 4) que puede afectar a las aplicaciones que se crearon con las versiones anteriores, incluida la versión de ASP.NET 4 Beta 1.

[Novedades de ASP.NET MVC 2](what-is-new-in-aspnet-mvc.md "what ' s new en mvc de aspnet")

Este documento describe las nuevas características y mejoras introducidas en ASP.NET MVC 2.

[Actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2](aspnet-mvc2-upgrade-notes.md "aspnet-mvc2-notas de actualización")

ASP.NET MVC 2 puede instalarse paralelo con ASP.NET MVC 1.0 en el mismo servidor. Esto proporciona flexibilidad a los desarrolladores de aplicaciones en elegir cuándo se debe actualizar una aplicación de ASP.NET MVC 1.0 para ASP.NET MVC 2. Este documento describe ambas cómo actualizar manualmente y con un asistente en Visual...

<a id="security"></a>
## <a name="aspnet-security-whitepapers"></a>Artículos técnicos de la seguridad ASP.NET

La seguridad es un aspecto importante de las aplicaciones de internet, y estas notas del producto describen cómo diseñar e implementar aplicaciones ASP.NET seguras.

[ASP.NET 2.0 de instrumentar aplicaciones para la seguridad](https://msdn.microsoft.com/library/ms998325.aspx)

Este modo se muestra cómo usar eventos de supervisión de estado personalizado para instrumentar la aplicación de ASP.NET para realizar un seguimiento de las operaciones y eventos relacionados con la seguridad. ASP.NET versión 2.0 proporciona una supervisión de estado incluye instrumentación para muchos estándar...

[Realizar una revisión de la implementación de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Este modo se muestra cómo realizar una revisión de la implementación de seguridad para una aplicación de ASP.NET 2.0 identificar posibles vulnerabilidades de seguridad introducidas por los valores de configuración apropiado. La mayor parte del proceso de revisión implica realizar...

[Usar a ADAM para Roles en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998331.aspx)

Este artículo se muestra cómo se puede desarrollar un sitio Web ASP.NET que usa Active Directory Application Mode (ADAM) para almacenar las funciones de ASP.NET. Muestra cómo configurar ADAM y el almacén de directivas del Administrador de autorización (AzMan), cómo crear nuevos roles y...

[Use el Administrador de autorización (AzMan) con ASP.NET 2.0](https://msdn.microsoft.com/library/ms998336.aspx)

En esta muestra cómo utilizar el Administrador de autorización (AzMan) junto con el Administrador de roles ASP.NET API para administrar roles, compruebe la pertenencia al rol de usuario y autorizar funciones para realizar operaciones específicas en un almacén de directivas de AzMan. Los temas de...

[Utilizar la pertenencia en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998347.aspx)

Este artículo se muestra cómo usar la característica de pertenencia en aplicaciones de la versión 2.0 de ASP.NET. Muestra cómo utilizar dos proveedores de pertenencia diferente: ActiveDirectoryMembershipProvider y SqlMembershipProvider. La característica de pertenencia...

[Use el Administrador de roles en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998314.aspx)

Este modo se muestra cómo utilizar el Administrador de roles de ASP.NET 2.0. El Administrador de roles simplifica la tarea de administración de roles y realizar autorización basada en roles en la aplicación. Muestra cómo configurar los distintos proveedores de roles para su uso con su...

[Utilizar autenticación de Windows en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998358.aspx)

Este modo se muestra cómo configurar y usar la autenticación de Windows en una aplicación Web ASP.NET. Autenticación de Windows es el método preferido siempre que los usuarios forman parte de su dominio de Windows. Este enfoque le permite usar un almacén de identidades existente...

[Realizar una revisión de código de seguridad para código administrado (actividad de la línea de base)](https://msdn.microsoft.com/library/ms998364.aspx)

Este modo se muestra cómo realizar revisiones de código de seguridad. Este módulo presenta los pasos implicados en la actividad y las técnicas para analizar los resultados. Utilice este cómo trabajar con "lista de preguntas de seguridad: código (.NET Framework 2.0) administrado"...

[Realizar una revisión de la implementación de seguridad para ASP.NET 2.0](https://msdn.microsoft.com/library/ms998367.aspx)

Este modo se muestra cómo realizar una revisión de la implementación de seguridad para una aplicación de ASP.NET 2.0 identificar posibles vulnerabilidades de seguridad introducidas por los valores de configuración apropiado. La mayor parte del proceso de revisión implica realizar...

[Implementar la delegación Kerberos para Windows 2000](https://msdn.microsoft.com/library/aa302400.aspx)

La delegación Kerberos le permite pasar una identidad autenticada a través de varios niveles físicos de una aplicación para admitir la autorización y autenticación de nivel inferior. En este artículo se muestra que los pasos de configuración necesarios para solucionar este problema.

[Usar la suplantación y delegación en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Este artículo se muestra cómo y cuándo se debe utilizar la suplantación en aplicaciones de ASP.NET 2.0. De forma predeterminada, está desactivada la suplantación y puede tener acceso a recursos utilizando la identidad del proceso de la aplicación Web de ASP.NET. Sin embargo, puede usar...

[Crear un modelo de amenazas para una aplicación Web en tiempo de diseño](https://msdn.microsoft.com/library/ms978527.aspx)

Este procedimiento se describe un método para crear un modelo de amenazas para una aplicación Web. La actividad de modelado de amenazas puede ayudarle a modelar el diseño de seguridad para que pueda exponer posibles errores de diseño de seguridad y las vulnerabilidades antes de invertir...

### <a name="forms-authentication"></a>Autenticación mediante formularios

[Proteger la autenticación de formularios en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998310.aspx)

Este modo se muestra cómo configurar y utilizar la autenticación de formularios con aplicaciones de ASP.NET 2.0 de forma segura. Factores clave a tener en cuenta son correctamente proteger el vale de autenticación y proteger el almacén de identidades de usuario y el acceso a ese almacén. ...

[Utilizar autenticación por formularios con Active Directory en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998360.aspx)

Este cómo se muestra cómo utilizar la autenticación de formularios con el servicio de directorio de Microsoft® Active Directory® utilizando ActiveDirectoryMembershipProvider. El artículo se muestra cómo configurar el proveedor y crear y autenticar a los usuarios...

[Utilizar autenticación por formularios con Active Directory en varios dominios en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998345.aspx)

Este cómo se muestra cómo utilizar la autenticación de formularios con el servicio de directorio de Microsoft® Active Directory® utilizando ActiveDirectoryMembershipProvider. El artículo se muestra cómo configurar el proveedor y crear y autenticar a los usuarios...

[Usar autenticación de formularios con SQL Server en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998317.aspx)

Este artículo se muestra cómo puede usar la autenticación de formularios con el proveedor de pertenencia de SQL Server. Autenticación de formularios con SQL Server es más apropiada en situaciones donde los usuarios de la aplicación no forman parte del dominio de Windows, y como resultado,...

[Crear objetos GenericPrincipal con autenticación de formularios en ASP.NET 1.1](https://msdn.microsoft.com/library/aa302399.aspx)

Este modo se muestra cómo crear y administrar objetos GenericPrincipal y FormsIdentity al usar la autenticación de formularios.

[Utilizar autenticación por formularios con Active Directory en ASP.NET 1.1](https://msdn.microsoft.com/library/aa302397.aspx)

Este artículo cómo muestra cómo implementar la autenticación mediante formularios en un almacén de credenciales de Active Directory.

[Usar autenticación de formularios con SQL Server en ASP.NET 1.1](https://msdn.microsoft.com/library/aa302398.aspx)

Este modo se muestra cómo implementar la autenticación mediante formularios en un almacén de credenciales de SQL Server. También muestra cómo almacenar contraseñas implícitas en la base de datos.

### <a name="user-input-data-validation"></a>Validación de datos de entrada de usuario

[Solicitud de validación: evitar los ataques de secuencia de comandos](request-validation.md "validación de solicitudes")

Este documento describe la característica de validación de solicitud de ASP.NET donde, de forma predeterminada, la aplicación no puede procesar el contenido HTML sin codificar enviado al servidor. Esta característica de validación de solicitud se puede deshabilitar cuando la aplicación ha...

[Impedir el Scripting entre sitios en ASP.NET](https://msdn.microsoft.com/library/ms998274.aspx)

Este artículo se muestra cómo puede ayudar a proteger las aplicaciones ASP.NET frente a ataques de scripts entre sitios mediante el uso de técnicas de validación de entrada correcta y mediante la codificación de la salida. También se describe una serie de otros mecanismos de protección que puede usar en...

[Proteger contra la inyección de SQL en ASP.NET](https://msdn.microsoft.com/library/ms998271.aspx)

Esta página se muestra un número de formas de ayudar a proteger la aplicación ASP.NET frente a ataques de inyección de SQL. Inyección de código SQL puede ocurrir cuando una aplicación utiliza la entrada para construir instrucciones SQL dinámicas o cuando utiliza procedimientos almacenados para conectarse a la...

[Utilizar expresiones regulares para restringir las entradas en ASP.NET](https://msdn.microsoft.com/library/ms998267.aspx)

Este artículo se muestra cómo puede utilizar expresiones regulares en aplicaciones ASP.NET para restringir las entradas que no se confía. Las expresiones regulares son una buena forma de validar los campos de texto como nombres, direcciones, números de teléfono y otra información de usuario. Puede usar...

### <a name="code-access-security"></a>Seguridad de acceso del código

[Utilizar la seguridad de acceso del código en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998326.aspx)

Este artículo se muestra cómo seleccionar un nivel de confianza adecuado para la aplicación y en caso necesario, cómo crear un personalizada ASP.NET código acceso archivo Directiva de seguridad para definir un personalizado nivel de confianza. Puede usar la veracidad de seguridad de acceso de código diferentes...

[Crear un permiso de cifrado personalizado](https://msdn.microsoft.com/library/aa302362.aspx)

Este procedimiento se describe cómo crear un permiso de seguridad de acceso de código personalizado para controlar el acceso mediante programación a la funcionalidad de cifrado no administrado que proporciona la API de protección de datos (DPAPI) de Win32®. Usar este permiso personalizado con el contenedor DPAPI administrado...

[Usar la directiva de seguridad de acceso de código para restringir un ensamblado](https://msdn.microsoft.com/library/aa302361.aspx)

Un administrador puede configurar la directiva de seguridad de acceso del código para restringir las operaciones de código de .NET Framework (ensamblados). En este artículo, configurar la directiva de seguridad de acceso del código para restringir la capacidad de un ensamblado para realizar E/S de archivos y restringir...

### <a name="communications-security"></a>Seguridad de las comunicaciones

[Configurar SSL en un servidor Web](https://msdn.microsoft.com/library/aa302411.aspx)

Un servidor Web debe configurarse para SSL con el fin de admitir las conexiones https desde aplicaciones cliente. Este modo se muestra cómo configurar SSL en un servidor Web.

[Configurar los certificados de cliente](https://msdn.microsoft.com/library/aa302412.aspx)

IIS admite la autenticación de certificado de cliente. Este modo se muestra cómo configurar una aplicación Web para requerir certificados de cliente. También muestra cómo instalar un certificado en un equipo cliente y usarlo cuando se llama a la aplicación Web.

[Utilice IPSec para el filtrado de puertos y la autenticación](https://msdn.microsoft.com/library/aa302366.aspx)

Seguridad de protocolo Internet (IPSec) es un protocolo, no un servicio, que proporciona servicios de autenticación para el tráfico de red basado en IP, integridad y cifrado. Puesto que IPSec proporciona protección de servidor a servidor, puede utilizar IPSec para contrarrestar las amenazas internas...

[Utilice IPSec para proporcionar una comunicación segura entre dos servidores](https://msdn.microsoft.com/library/aa302413.aspx)

IPSec es una tecnología incluida en Windows 2000 que le permite crear canales cifrados entre dos servidores. IPSec puede usarse para filtrar el tráfico IP y para autenticar los servidores. Este artículo se muestra cómo configurar IPSec para proporcionar una ubicación segura (cifrada)...

[Usar SSL para proteger la comunicación con SQL Server](https://msdn.microsoft.com/library/aa302414.aspx)

A menudo resulta vital para las aplicaciones poder proteger los datos transmitidos a y desde un servidor de base de datos de SQL Server. Con SQL Server, puede utilizar SSL para crear un canal cifrado. Este artículo se muestra cómo instalar un certificado en el servidor de base de datos...

[Llamar a un servicio Web mediante certificados de cliente de ASP.NET 1.1](https://msdn.microsoft.com/library/aa302408.aspx)

Este procedimiento se describe cómo pasar un certificado de cliente a un servicio Web para la autenticación desde una aplicación Web ASP.NET o desde una aplicación de formularios Windows Forms. Puede instalar el certificado de cliente en el almacén del equipo local o en el almacén del usuario. If...

[Llamar a un servicio Web mediante SSL desde ASP.NET 1.1](https://msdn.microsoft.com/library/aa302409.aspx)

Cifrado de Secure Sockets Layer (SSL) se puede utilizar para garantizar la integridad y confidencialidad de los mensajes transmitidos a y desde un servicio Web. Este modo se muestra cómo usar SSL con servicios Web.

### <a name="cryptography"></a>Criptografía

[Crear una biblioteca DPAPI en .NET 1.1](https://msdn.microsoft.com/library/aa302402.aspx)

Este modo se muestra cómo crear una biblioteca de clases administradas que expone la funcionalidad DPAPI a las aplicaciones que van a cifrar los datos, por ejemplo, las cadenas de conexión de base de datos y las credenciales de cuenta.

[Crear una biblioteca de cifrado en .NET 1.1](https://msdn.microsoft.com/library/aa302405.aspx)

Este modo se muestra cómo crear una biblioteca de clases administradas para proporcionar funcionalidad de cifrado para las aplicaciones. Permite que una aplicación elegir el algoritmo de cifrado. Algoritmos compatibles incluyen DES, Triple DES, RC2 y Rijndael.

[Almacenar una cadena de conexión cifrada en el registro de ASP.NET 1.1](https://msdn.microsoft.com/library/aa302406.aspx)

Las aplicaciones pueden optar por almacenar datos cifrados, como las cadenas de conexión y credenciales de cuenta en el registro de Windows. Este modo se muestra cómo almacenar y recuperar cadenas cifradas en el registro.

[Usar DPAPI (almacén del equipo) desde ASP.NET 1.1](https://msdn.microsoft.com/library/aa302403.aspx)

Este modo se muestra cómo utilizar DPAPI desde una aplicación Web ASP.NET o servicio Web para cifrar datos confidenciales.

[Usar DPAPI (almacén de usuario) de ASP.NET 1.1 con Enterprise Services](https://msdn.microsoft.com/library/aa302404.aspx)

Este modo se muestra cómo utilizar DPAPI desde una aplicación Web ASP.NET o un servicio para cifrar datos confidenciales. Este artículo se utiliza DPAPI con el almacén de usuario, lo que requiere el uso de un proceso fuera de los componentes de Enterprise Services.

<a id="setup"></a>
## <a name="installation-and-setup-whitepapers"></a>Instalación y notas del producto del programa de instalación

Estas notas del producto proporcionan instrucciones paso a paso para instalar y configurar ASP.NET en el servidor.

[Crear una cuenta de servicio para ASP.NET 2.0 aplicación](https://msdn.microsoft.com/library/ms998297.aspx)

Este modo se muestra cómo crear y configurar una cuenta de servicio personalizado de privilegios mínimos para ejecutar una aplicación Web ASP.NET. De forma predeterminada, una aplicación de ASP.NET en Microsoft Windows Server 2003 e IIS 6.0 se ejecuta con el servicio de red integrado...

[Mejorar la seguridad al hospedar varias aplicaciones en ASP.NET 2.0](https://msdn.microsoft.com/library/aa480478.aspx)

Este artículo se muestra cómo puede aislar varias aplicaciones entre sí y de los recursos de sistema compartido en un servidor Web entorno de hospedaje. El entorno de hospedaje podría ser un servidor Web proporcionado por un proveedor de servicios de Internet (ISP) que hospeda varios...

[Use Medium Trust in ASP.NET 2.0](https://msdn.microsoft.com/library/ms998341.aspx)

Este modo se muestra cómo configurar aplicaciones Web ASP.NET para ejecutarse en el nivel de confianza medio. Si hospeda varias aplicaciones en el mismo servidor, puede utilizar la seguridad de acceso del código y el nivel de confianza medio para proporcionar aislamiento de aplicaciones. Estableciendo...

[Usar la cuenta de servicio de red para tener acceso a recursos en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998320.aspx)

Este artículo se muestra cómo puede usar la cuenta de equipo de NT AUTHORITY\Network Service para obtener acceso a local y recursos de red. De forma predeterminada en Windows Server 2003, en aplicaciones ASP.NET se ejecutan con la identidad de la cuenta. Es un con menos privilegios...

[Use la transición del protocolo y delegación restringida en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998355.aspx)

Este modo se muestra cómo configurar y usar la delegación restringida y transición de protocolo para permitir que la aplicación de ASP.NET para tener acceso a recursos de red al tiempo que suplanta al llamador original. Sistema operativo de Microsoft® Windows® 2000...

[Ejecución de ASP.NET en paralelo de .NET Framework 1.0 y 1.1](side-by-side-with-10.md "en paralelo con 1.0")

Estas notas del producto describen cómo instalar .NET 1.0 y 1.1 de .NET en su equipo, lo que permite una aplicación Web ASP.NET para ejecutarse en cualquiera de las versiones de framework.

[ASP.NET denegará el acceso a directorios IIS](denied-access-to-iis-directories.md "denegado el acceso a directorios de iis")

Estas notas del producto describen lo que debe hacer si una solicitud para la aplicación ASP.NET devuelve el error "acceso denegado a *DirectoryName* directory. No se pudo iniciar la supervisión de cambios en el directorio."

[Ejecute ASP.NET 1.1 en IIS 6.0](aspnet-and-iis6.md "aspnet e IIS 6")

Mientras que Windows Server 2003 incluye IIS 6.0 y ASP.NET 1.1, estos componentes están deshabilitados de forma predeterminada. Estas notas del producto describen cómo habilitar IIS 6.0 y ASP.NET 1.1 y recomienda diversas opciones de configuración para obtener el óptimo...

[Corrección de Error de "Aplicación de servidor no disponible" después de aplicar la actualización de seguridad de Internet Explorer](ms03-32-issue.md "ms03-32-problema")

Este documento describe la revisión que corrige un problema con la actualización de seguridad MS03-32 para Internet Explorer que afecta a las aplicaciones de ASP.NET 1.0 que se ejecutan en Windows XP Professional.

[Crear una cuenta personalizada para ejecutar ASP.NET 1.1](https://msdn.microsoft.com/library/aa302396.aspx)

Las aplicaciones Web ASP.NET suelen ejecutarán con la cuenta ASPNET integrada. En algunas ocasiones, puede que desee utilizar una cuenta personalizada en su lugar. Este artículo muestra cómo crear una cuenta local con privilegios mínimos para ejecutar aplicaciones Web ASP.NET. ...

### <a name="configuration"></a>Configuración

[Configurar la clave del equipo en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998288.aspx)

Este procedimiento explica el &lt;machineKey&gt; elemento en el archivo Web.config y muestra cómo configurar el &lt;machineKey&gt; elemento corrección de alteración de control y el cifrado de estado de vista, la autenticación de formularios los vales y las cookies de rol. Estado de vista está firmado...

[Cifrar secciones de configuración en ASP.NET 2.0 mediante DPAPI](https://msdn.microsoft.com/library/ms998280.aspx)

Este artículo se muestra cómo utilizar el proveedor de configuración protegida de interfaz (DPAPI) y Aspnet de programación de aplicaciones de Windows Data Protection\_regiis.exe herramienta para cifrar secciones de los archivos de configuración. Puede usar Aspnet\_regiis.exe herramienta...

[Cifrar secciones de configuración en ASP.NET 2.0 mediante RSA](https://msdn.microsoft.com/library/ms998283.aspx)

Este artículo se muestra cómo utilizar el proveedor de configuración protegida de RSA y Aspnet\_regiis.exe herramienta para cifrar secciones de los archivos de configuración. Puede usar Aspnet\_regiis.exe herramienta para cifrar datos confidenciales, como cadenas de conexión, se mantiene en los...

[Usar la suplantación y delegación en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998351.aspx)

Este artículo se muestra cómo y cuándo se debe utilizar la suplantación en aplicaciones de ASP.NET 2.0. De forma predeterminada, está desactivada la suplantación y puede tener acceso a recursos utilizando la identidad del proceso de la aplicación Web de ASP.NET. Sin embargo, puede usar...

<a id="sql"></a>
## <a name="sql-server-whitepapers"></a>Notas del producto SQL Server

Mientras que ASP.NET funciona con una variedad de bases de datos, estas notas del producto preste especial atención a conectar aplicaciones de ASP.NET a SQL Server.

[Conectarse a SQL Server mediante autenticación de SQL en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998300.aspx)

Este modo se muestra cómo conectar una aplicación de ASP.NET de forma segura a Microsoft® SQL Server™ cuando la autenticación de acceso de base de datos utiliza la autenticación de SQL nativo. Autenticación de Windows es el método recomendado para conectarse a SQL Server porque se...

[Conectarse a SQL Server mediante autenticación de Windows en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998292.aspx)

En esta muestra cómo conectarse a SQL Server 2000 mediante una cuenta de servicio de Windows desde una aplicación de ASP.NET versión 2.0. Debe usar autenticación de Windows en lugar de la autenticación de SQL siempre que sea posible, ya que no guarde las credenciales en...

[Usar SSL para proteger la comunicación con SQL Server 2000](https://msdn.microsoft.com/library/aa302414.aspx)

A menudo resulta vital para las aplicaciones poder proteger los datos transmitidos a y desde un servidor de base de datos de SQL Server. Con SQL Server, puede utilizar SSL para crear un canal cifrado. Este artículo se muestra cómo instalar un certificado en el servidor de base de datos...

<a id="general"></a>
## <a name="general-whitepapers"></a>Notas del producto general

El siguiente caso práctico describe el proceso que se usó para migrar sitios Web de comunidad de .NET de Microsoft de un entorno de hospedaje tradicional a Microsoft Azure.

[Migrar Microsoft ASP.NET y sitios Web de comunidad de IIS.NET en Microsoft Azure](https://go.microsoft.com/fwlink/?LinkId=400656)

Estas notas del producto abarcan una gran variedad de temas relacionados con ASP.NET.

[Usar la supervisión de estado en ASP.NET 2.0](https://msdn.microsoft.com/library/ms998306.aspx)

Este modo se muestra cómo se utiliza para instrumentar una aplicación para un evento personalizado de supervisión de estado. Para crear un evento de supervisión de estado personalizado, cree una clase que deriva de System.Web.Management.WebBaseEvent, configurar la &lt;healthMonitoring&gt; ...

[Implementar la administración de revisión](https://msdn.microsoft.com/library/aa302364.aspx)

Este procedimiento se explica la administración de revisiones, incluido cómo mantener único o varios servidores actualizados. No es necesario, excepto para las herramientas disponibles para su descarga desde Microsoft software adicional. Las operaciones y la directiva de seguridad deberían adoptar una administración de revisión...

[Controles de servidor ASP.NET para Silverlight en Silverlight 3 SDK](https://go.microsoft.com/fwlink/?LinkId=153377)

Los controles de servidor ASP.NET para Silverlight ("controles de Silverlight de ASP.NET"), que son los controles ASP.NET MediaPlayer y Silverlight, se han quitado de los SDK de Silverlight para Silverlight versión 3. Este documento proporciona instrucciones para los desarrolladores que ha trabajado con estos ASP.NET...

[Creación de aplicaciones Web de alto rendimiento](https://devexpress.com/act)

Obtenga información acerca de cómo usar las nuevas características en la biblioteca de Ajax de ASP.NET para compilar las aplicaciones Web de alto rendimiento
