---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
title: Configurar un servidor de base de datos para Web publicación de la implementación | Microsoft Docs
author: jrjlee
description: Este tema describe cómo configurar un servidor de base de datos de SQL Server 2008 R2 para admitir la publicación e implementación de web. Las tareas descritas en este tema son co...
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: e7c447f9-eddf-4bbe-9f18-3326d965d093
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/configuring-a-database-server-for-web-deploy-publishing
msc.type: authoredcontent
ms.openlocfilehash: 9ec938298ea63609c276aed807a67722bf9bd66d
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841984"
---
<a name="configuring-a-database-server-for-web-deploy-publishing"></a>Configurar un servidor de base de datos para publicación de la implementación Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo configurar un servidor de base de datos de SQL Server 2008 R2 para admitir la publicación e implementación de web.
> 
> Las tareas descritas en este tema son comunes a todos los escenarios de implementación&#x2014;no importa si los servidores web están configurados para usar el servicio de agente remoto de la herramienta de implementación Web de IIS (Web Deploy), el controlador de implementación Web o implementación sin conexión o su aplicación se ejecuta en un único servidor web o una granja de servidores. Puede cambiar la forma de implementar la base de datos según los requisitos de seguridad y otras consideraciones. Por ejemplo, podría implementar la base de datos con o sin datos de ejemplo y puede implementar las asignaciones de rol de usuario o configurarlos manualmente después de la implementación. Sin embargo, la forma de configurar el servidor de base de datos sigue siendo el mismo.


No tienes que instalar otros productos y herramientas para configurar un servidor de base de datos para admitir la implementación web. Suponiendo que el servidor de base de datos y el servidor web se ejecutan en máquinas diferentes, basta con:

- Permitir que SQL Server se comunique con TCP/IP.
- Permitir el tráfico de SQL Server a través de los firewalls.
- Asigne a un inicio de sesión de SQL Server de la cuenta del equipo servidor web.
- El inicio de sesión de cuenta de equipo se asignan a los roles de base de datos necesarios.
- Conceda a la cuenta que se ejecutará un permisos del creador de inicio de sesión y la base de datos de SQL Server de la implementación.
- Para admitir implementaciones repetitivas, asignar el inicio de sesión de cuenta de implementación para el **db\_propietario** rol de base de datos.

En este tema le mostrará cómo realizar cada uno de estos procedimientos. Las tareas y los tutoriales en este tema se suponen que está empezando con una instancia predeterminada de SQL Server 2008 R2 que se ejecutan en Windows Server 2008 R2. Antes de continuar, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido al dominio.
- El servidor tiene una dirección IP estática.
- Se instalan SQL Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.

La instancia de SQL Server sólo tiene que incluir el **servicios de motor de base de datos** rol, que se incluye automáticamente en cualquier instalación de SQL Server. Sin embargo, para facilitar la configuración y mantenimiento, se recomienda que incluya el **herramientas de administración – básica** y **herramientas de administración – completa** roles de servidor.

> [!NOTE]
> Para obtener más información sobre la unión a los equipos a un dominio, consulte [unir equipos al dominio e iniciar sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre cómo configurar direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx). Para obtener más información acerca de cómo instalar SQL Server, vea [instalar SQL Server 2008 R2](https://technet.microsoft.com/library/bb500395.aspx).


## <a name="enable-remote-access-to-sql-server"></a>Habilitar el acceso remoto a SQL Server

SQL Server usa TCP/IP para comunicarse con equipos remotos. Si el servidor de base de datos y el servidor web están en equipos diferentes, es preciso:

- Configurar opciones de red de SQL Server para permitir la comunicación a través de TCP/IP.
- Configurar los firewalls de hardware o software para permitir el tráfico TCP (y en algunos casos, protocolo de datagramas de usuario (UDP) el tráfico) en los puertos que utiliza la instancia de SQL Server.

Para habilitar SQL Server para comunicarse a través de TCP/IP, use el Administrador de configuración de SQL Server para cambiar la configuración de red para la instancia de SQL Server.

**Para habilitar SQL Server para comunicarse con TCP/IP**

1. En el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**, haga clic en **herramientas de configuración**y, a continuación, haga clic en **Administrador de configuración de SQL Server**.
2. En el panel de vista de árbol, expanda **configuración de red de SQL Server**y, a continuación, haga clic en **protocolos para MSSQLSERVER**.

   > [!NOTE]
   > Si ha instalado varias instancias de SQL Server, verá un <strong>protocolos de</strong><em>[nombre de instancia]</em> elemento para cada instancia. Deberá configurar una red en una base por instancia.
3. En el panel de detalles, haga clic en el **TCP/IP** fila y, a continuación, haga clic en **habilitar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image1.png)
4. En el **advertencia** cuadro de diálogo, haga clic en **Aceptar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image2.png)
5. Deberá reiniciar el servicio MSSQLSERVER para que surta efecto la nueva configuración de red. Puede hacerlo en un símbolo del sistema, desde la consola de servicios, o desde SQL Server Management Studio. En este procedimiento, usará SQL Server Management Studio.
6. Cierre el Administrador de configuración de SQL Server.
7. En el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**y, a continuación, haga clic en **SQL Server Management Studio**.
8. En el **conectar al servidor** cuadro de diálogo el **nombre del servidor** , escriba el nombre del servidor de base de datos y, a continuación, haga clic en **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image3.png)
9. En el **Explorador de objetos** panel, haga clic en el nodo del servidor principal (por ejemplo, **TESTDB1**) y, a continuación, haga clic en **reiniciar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image4.png)
10. En el **Microsoft SQL Server Management Studio** cuadro de diálogo, haga clic en **Sí**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image5.png)
11. Cuando se haya reiniciado el servicio, cierre SQL Server Management Studio.

Para permitir el tráfico de SQL Server a través de un firewall, primero deberá saber qué puertos que usa la instancia de SQL Server. Esto dependerá de cómo se ha creado y configurado la instancia de SQL Server:

- Un *instancia predeterminada* de SQL Server (y responde a) las solicitudes en el puerto TCP 1433.
- Un *instancia con nombre* de SQL Server (y responde a) las solicitudes en un puerto TCP asignado dinámicamente.
- Si el servicio SQL Server Browser está habilitado, los clientes pueden consultar el servicio en el puerto UDP 1434 para averiguar qué puerto TCP que se usará para una determinada instancia de SQL Server. Sin embargo, a menudo se deshabilita este servicio por motivos de seguridad.

Suponiendo que usa una instancia predeterminada de SQL Server, deberá configurar el firewall para permitir el tráfico.

| Dirección | Desde el puerto | Al puerto | Tipo de puerto |
| --- | --- | --- | --- |
| Entrada | Cualquiera | 1433 | TCP |
| Saliente | 1433 | Cualquiera | TCP |
  

> [!NOTE]
> Técnicamente, un equipo cliente utilizará un puerto TCP asignado de forma aleatoria entre 1024 y 5000 para comunicarse con SQL Server, y puede restringir las reglas de firewall en consecuencia. Para obtener más información sobre los puertos de SQL Server y los firewalls, consulte [números de puerto TCP/IP necesarios para comunicarse con SQL a través de un firewall](https://go.microsoft.com/?linkid=9805125) y [Cómo: configurar un servidor para que escuche en un puerto de TCP específico (configuración de SQL Server El administrador)](https://msdn.microsoft.com/library/ms177440.aspx).


En la mayoría de los entornos de Windows Server, probablemente tendrá que configurar el Firewall de Windows en el servidor de base de datos. De forma predeterminada, Firewall de Windows permite todo el tráfico saliente a menos que una regla prohíba específicamente. Para habilitar el servidor web para llegar a la base de datos, deberá configurar una regla de entrada que permita el tráfico TCP en el número de puerto que utiliza la instancia de SQL Server. Si usa una instancia predeterminada de SQL Server, puede usar el procedimiento siguiente para configurar esta regla.

**Para configurar el Firewall de Windows para permitir la comunicación con una instancia de SQL Server predeterminada**

1. En el servidor de base de datos, en el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Firewall de Windows con seguridad avanzada**.
2. En el panel de vista de árbol, haga clic en **reglas de entrada**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image6.png)
3. En el **acciones** panel, en **reglas de entrada**, haga clic en **nueva regla**.
4. En el nuevo Asistente de regla de entrada, en el **tipo de regla** página, seleccione **puerto**y, a continuación, haga clic en **siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image7.png)
5. En el **protocolo y puertos** página, asegúrese de que **TCP** está seleccionada y en el **puertos locales específicos** , escriba **1433**y, a continuación, haga clic en **Siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image8.png)
6. En el **acción** página, deje **permitir la conexión** seleccionado y haga clic en **siguiente**.
7. En el **perfil** página, deje **dominio** seleccionada, desactive la **privada** y **pública** casillas de verificación y, a continuación, haga clic en  **Siguiente**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image9.png)
8. En el **nombre** página, asigne un nombre descriptivo adecuadamente a la regla (por ejemplo, **instancia predeterminada de SQL Server: acceso a la red**) y, a continuación, haga clic en **finalizar**.

Para obtener más información sobre cómo configurar Firewall de Windows para SQL Server, especialmente si tiene que comunicarse con SQL Server a través de puertos no estándares o dinámicos, consulte [Cómo: configurar un Firewall de Windows para acceder al motor de base de datos](https://technet.microsoft.com/library/ms175043.aspx).

## <a name="configure-logins-and-database-permissions"></a>Configurar inicios de sesión y permisos de base de datos

Al implementar una aplicación web para Internet Information Services (IIS), la aplicación ejecuta con la identidad del grupo de aplicaciones. En un entorno de dominio, las identidades del grupo de aplicaciones usa la cuenta de equipo del servidor en el que ejecuta para acceder a recursos de red. Cuentas de equipo que adoptan la forma <em>[nombre de dominio]</em><strong>\</strong ><em>[nombreMáquina]</em><strong>$</strong>&#x2014;por ejemplo, <strong>FABRIKAM\TESTWEB1$</strong>. Para permitir que la aplicación web tener acceso a una base de datos a través de la red, deberá:

- Agregar un inicio de sesión para la cuenta del equipo servidor web a la instancia de SQL Server.
- El inicio de sesión de cuenta de equipo se asignan a los roles de base de datos necesarios (normalmente **db\_datareader** y **db\_datawriter**).

Si la aplicación web se ejecuta en una granja de servidores, en lugar de un solo servidor, deberá repetir estos procedimientos para cada servidor web de la granja de servidores.

> [!NOTE]
> Para obtener más información sobre las identidades del grupo de aplicaciones y acceder a recursos de red, consulte [identidades del grupo de aplicación](https://go.microsoft.com/?linkid=9805123).


Puede llevar estas tareas de varias maneras. Para crear el inicio de sesión, puede:

- Crear el inicio de sesión manualmente en el servidor de base de datos, mediante Transact-SQL o SQL Server Management Studio.
- Usar un proyecto de servidor de SQL Server 2008 en Visual Studio para crear e implementar el inicio de sesión.

Un inicio de sesión de SQL Server es un objeto de nivel de servidor, en lugar de un objeto de nivel de base de datos, por lo que no es dependiente de la base de datos que desea implementar. Por lo tanto, puede crear el inicio de sesión en cualquier momento y el enfoque más sencillo es a menudo crear el inicio de sesión manualmente en el servidor de base de datos para empezar a implementar las bases de datos. Puede usar el procedimiento siguiente para crear un inicio de sesión en SQL Server Management Studio.

**Para crear un inicio de sesión de SQL Server para la cuenta del equipo servidor web**

1. En el servidor de base de datos, en el **iniciar** menú, elija **todos los programas**, haga clic en **Microsoft SQL Server 2008 R2**y, a continuación, haga clic en **SQL Server Management Studio** .
2. En el **conectar al servidor** cuadro de diálogo el **nombre del servidor** , escriba el nombre del servidor de base de datos y, a continuación, haga clic en **Connect**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image10.png)
3. En el **Explorador de objetos** panel, haga clic en **seguridad**, apunte a **New**y, a continuación, haga clic en **inicio de sesión**.
4. En el **inicio de sesión – nuevo** cuadro de diálogo el **nombre de inicio de sesión** , escriba el nombre de la cuenta del equipo servidor web (por ejemplo, **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image11.png)
5. Haga clic en **Aceptar**.

En este momento, el servidor de base de datos está listo para la publicación de Web Deploy. Sin embargo, las soluciones que se implementan no funcionarán hasta que el inicio de sesión de cuenta de equipo se asignan a los roles de base de datos necesarios. Asignar el inicio de sesión a los roles de base de datos requiere mucho más pensado, que no funciones de mapa hasta después de ha implementado la base de datos. Para asignar el inicio de sesión de cuenta de equipo a los roles de base de datos necesarios, puede:

- Asignar los roles de base de datos para el inicio de sesión manualmente, después de haber implementado la base de datos por primera vez.
- Use un script posterior a la implementación para asignar los roles de base de datos para el inicio de sesión.

Para obtener más información acerca de cómo automatizar la creación de inicios de sesión y las asignaciones de rol de base de datos, vea [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Como alternativa, puede usar el procedimiento siguiente para asignar manualmente el inicio de sesión de cuenta de equipo a los roles de base de datos necesarios. Recuerde que no se puede realizar este procedimiento hasta que *después* ha implementado la base de datos.

**Asignar roles de base de datos para el inicio de sesión de cuenta del equipo de servidor web**

1. Abra SQL Server Management Studio como antes.
2. En el **Explorador de objetos** panel, expanda el **seguridad** nodo, expanda el **inicios de sesión** nodo y, a continuación, haga doble clic en el inicio de sesión de cuenta de equipo (por ejemplo,  **FABRIKAM\TESTWEB1$**).

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image12.png)
3. En el **propiedades de inicio de sesión** cuadro de diálogo, haga clic en **asignación de usuario**.
4. En el **usuarios asignados a este inicio de sesión** tabla, seleccione el nombre de la base de datos (por ejemplo, **ContactManager**).
5. En el **pertenencia al rol de la base de datos:** *[nombre de base de datos]* lista, seleccione los permisos necesarios. En el caso de la solución de ejemplo Contact Manager, debe seleccionar la **db\_datareader** y **db\_datawriter** roles.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image13.png)
6. Haga clic en **Aceptar**.

Aunque la asignación manual de los roles de base de datos a menudo resulta más adecuado para entornos de prueba, es menos deseable para implementaciones automatizadas o con un solo clic a los entornos de ensayo o producción. Puede encontrar más información sobre la automatización de este tipo de tarea con secuencias de comandos posteriores a la implementación en [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md).

> [!NOTE]
> Para obtener más información sobre los proyectos de servidor y los proyectos de base de datos, vea [Visual Studio 2010 SQL Server Database Projects](https://msdn.microsoft.com/library/ff678491.aspx).


## <a name="configure-permissions-for-the-deployment-account"></a>Configurar permisos para la cuenta de implementación

Si la cuenta que va a usar para ejecutar la implementación no es un administrador de SQL Server, también deberá crear un inicio de sesión para esta cuenta. Para crear la base de datos, la cuenta debe ser un miembro de la **dbcreator** rol de servidor o tener permisos equivalentes.

> [!NOTE]
> Al usar Web Deploy o VSDBCMD para implementar una base de datos, puede usar las credenciales de Windows o SQL Server (si la instancia de SQL Server está configurada para admitir la autenticación de modo mixto). El siguiente procedimiento se supone que va a utilizar las credenciales de Windows, pero no hay nada que le impida especificando un nombre de usuario de SQL Server y la contraseña en la cadena de conexión al configurar la implementación.


**Para configurar permisos para la cuenta de implementación**

1. Abra SQL Server Management Studio como antes.
2. En el **Explorador de objetos** panel, haga clic en **seguridad**, apunte a **New**y, a continuación, haga clic en **inicio de sesión**.
3. En el **inicio de sesión – nuevo** cuadro de diálogo el **nombre de inicio de sesión** , escriba el nombre de la cuenta de implementación (por ejemplo, **FABRIKAM\matt**).
4. En el **seleccionar una página** panel, haga clic en **Roles de servidor**.
5. Seleccione **dbcreator**y, a continuación, haga clic en **Aceptar**.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image14.png)

Para admitir las implementaciones posteriores, también tendrá que agregar la cuenta al implementar la **db\_propietario** rol en la base de datos tras la primera implementación. Esto es porque en las implementaciones posteriores está la modificación del esquema de base de datos existente, en lugar de crear una nueva base de datos. Como se describe en la sección anterior, no se puede agregar un usuario a un rol de base de datos hasta que haya creado la base de datos por razones obvias.

**Para asignar el inicio de sesión de cuenta de implementación a la base de datos\_rol de propietario de la base de datos**

1. Abra SQL Server Management Studio como antes.
2. En el **Explorador de objetos** ventana, expanda el **seguridad** nodo, expanda el **inicios de sesión** nodo y, a continuación, haga doble clic en el inicio de sesión de cuenta de equipo (por ejemplo,  **FABRIKAM\matt**).
3. En el **propiedades de inicio de sesión** cuadro de diálogo, haga clic en **asignación de usuario**.
4. En el **usuarios asignados a este inicio de sesión** tabla, seleccione el nombre de la base de datos (por ejemplo, **ContactManager**).
5. En el **pertenencia al rol de la base de datos:** *[nombre de base de datos]* lista, seleccione el **db\_propietario** rol.

    ![](configuring-a-database-server-for-web-deploy-publishing/_static/image15.png)
6. Haga clic en **Aceptar**.

## <a name="conclusion"></a>Conclusión

El servidor de base de datos ya debe estar listo para aceptar las implementaciones de la base de datos remota y permitir que los servidores de web IIS remotos tener acceso a las bases de datos. Antes de intentar implementar y usar bases de datos, es posible que desee comprobar estos puntos clave:

- ¿Ha configurado SQL Server para aceptar conexiones TCP/IP remotas?
- ¿Ha configurado los firewalls para permitir el tráfico de SQL Server?
- ¿Ha creado un inicio de sesión de cuenta de equipo para todos los servidores web que tendrá acceso a SQL Server?
- ¿La implementación de la base de datos incluye un script para crear asignaciones de rol de usuario, o es necesario crearlas manualmente después de implementar la base de datos por primera vez?
- ¿Ha creado un inicio de sesión para la cuenta de implementación y agregarlo a la **dbcreator** rol de servidor?

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre la implementación de proyectos de base de datos, vea [implementar proyectos de base de datos](../web-deployment-in-the-enterprise/deploying-database-projects.md). Para obtener instrucciones sobre la creación de las pertenencias a roles de base de datos ejecutando un script posterior a la implementación, consulte [implementar pertenencias a roles de base de datos en entornos de prueba](../advanced-enterprise-web-deployment/deploying-database-role-memberships-to-test-environments.md). Para obtener instrucciones sobre cómo superar los desafíos de implementación único que suponen las bases de datos de pertenencia, vea [implementar bases de datos de pertenencia a entornos empresariales](../advanced-enterprise-web-deployment/deploying-membership-databases-to-enterprise-environments.md).

> [!div class="step-by-step"]
> [Anterior](configuring-a-web-server-for-web-deploy-publishing-offline-deployment.md)
> [Siguiente](creating-a-server-farm-with-the-web-farm-framework.md)
