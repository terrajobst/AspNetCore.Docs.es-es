---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Crear una granja de servidores con el marco de trabajo de la granja de servidores Web | Documentos de Microsoft
author: jrjlee
description: Este tema describe cómo usar Web Farm Framework (WFF) 2.0 para crear y configurar una granja de servidores web de una colección de servidores.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/04/2012
ms.topic: article
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 53a91660953795f2c55edcd795b053641d308dfe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Crear una granja de servidores con el marco de trabajo de la granja de servidores Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descarga de PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo usar Web Farm Framework (WFF) 2.0 para crear y configurar una granja de servidores web de una colección de servidores.


WFF le permite sincronizar componentes y productos de la plataforma web, aplicaciones web, sitios Web y opciones de configuración en varios servidores web con equilibrio de carga. En escenarios donde es necesario más de un servidor web, como entornos de ensayo y producción, esto puede simplificar enormemente el proceso de implementación y configuración. Puede implementar una aplicación web en un único servidor&#x2014;la *servidor principal*&#x2014;y WFF replicará automáticamente de esa aplicación web en todos los demás servidores de web de la granja de servidores.

## <a name="understanding-the-web-farm-framework"></a>Descripción del marco de granja de servidores Web

Puede usar WFF 2.0 para aprovisionar, administrar y distribuir contenido a un grupo de servidores web. Una implementación de WFF consta de tres funciones de servidor principales:

- El *servidor de controlador*. Este servidor se usa para crear y configurar las granjas de servidores WFF. El servidor de controlador administra la sincronización de componentes de plataforma web, valores de configuración y las aplicaciones entre los servidores web en una granja de servidores. Instalar WFF 2.0 en el servidor de controlador y el servidor de controlador a su vez instalará al agente WFF en cada uno de los servidores en una granja de servidores. El servidor de controlador conceptualmente no pertenece a ninguna granja de servidores WFF y un servidor de controlador único puede administrar varias granjas de servidores. En este escenario, se utiliza un único servidor de controlador WFF para crear y administrar la granja de servidores de ensayo y de la granja de servidores de producción.
- El *servidor principal*. Cada granja de servidores WFF incluye un único servidor principal. Al instalar los componentes de plataforma web o implementar aplicaciones en el servidor principal, el WFF sincroniza los cambios realizados en todos los demás servidores de la granja de servidores.
- El *servidor secundario*. Cada conjunto de servidores WFF incluye uno o más servidores secundarios. Los cambios que realice en el servidor principal se replican en cada servidor secundario dentro de la granja de servidores.

Esto muestra cómo se relacionan estos roles de servidor a los entornos de ensayo y producción de Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

En este escenario, el entorno de ensayo y el entorno de producción se configuran como las granjas de servidores WFF. Un único servidor de controlador WFF administra las granjas de servidores. En cada granja de servidores, los cambios realizados en el servidor principal se replican en cada servidor secundario.

Antes de empezar a configurar los entornos de ensayo y producción, se recomienda que lea los siguientes artículos para familiarizarse con los conceptos básicos de WFF 2.0:

- [Información general sobre el marco de granja de servidores Web 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Cómo configurar una granja de servidores con Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisitos del sistema y plataforma para el marco de granja de servidores Web 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Información general sobre tareas

Para completar las tareas y los tutoriales de este tema, necesitará al menos tres servidores&#x2014;un controlador WFF, un servidor web principal para la granja de servidores y uno o más servidores web secundaria para la granja de servidores. Puede agregar más servidores secundarios a una granja de servidores WFF en cualquier momento. En un nivel alto, para crear y configurar una granja de servidores WFF para su entorno de ensayo o de producción que necesite:

- Crear un servidor de controlador al instalar Internet Information Services (IIS) 7.5 y WFF 2.0.
- Preparar servidores principales y secundarios mediante la creación de una cuenta de administrador común y configurar las excepciones del firewall.
- Configurar la granja de servidores mediante el Administrador de IIS en el servidor de controlador.
- Configurar el equilibrio de carga mediante IIS aplicación enrutamiento de solicitud (ARR) o una tecnología alternativa de equilibrio de carga.

Las tareas y los tutoriales en este tema se suponen que está empezando con compilaciones de servidor limpio que ejecuta Windows Server 2008 R2. Antes de comenzar, para cada servidor, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido al dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información sobre unir equipos a un dominio, consulte [unir equipos al dominio e iniciar sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información acerca de cómo configurar direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Crear el servidor de controlador WFF

Para crear un servidor de controlador WFF, debe instalar IIS 7 o posterior y WFF 2.0 o posterior. Tras los bastidores, WFF utiliza la herramienta de implementación Web de IIS (Web Deploy) 2.x para sincronizar los servidores en la granja de servidores. Si usa al instalador de plataforma Web para instalar WFF, el programa de instalación automáticamente se descargar e instalar Web Deploy para usted.

**Para crear el servidor de controlador WFF**

1. Descargue e instale el [instalador de plataforma Web](https://go.microsoft.com/?linkid=9739157).
2. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **Server**.
4. En el **configuración recomendada para IIS 7** la fila, haga clic en **agregar**.
5. En el <strong>Web Farm Framework 2.</strong> <em>x</em> la fila, haga clic en <strong>agregar</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Haga clic en **Instalar**. Observe que el instalador de plataforma Web ha agregado la herramienta de implementación Web, junto con diversas otras dependencias a la lista de instalación.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Revise los términos de licencia y, si acepta los términos, haga clic en **acepto**.
8. Una vez completada la instalación, haga clic en **finalizar**y, a continuación, cierre el **instalador de plataforma Web 3.0** ventana.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurar los servidores principales y secundario

Antes de crear una granja de servidores WFF, debe completar algunas tareas de preparación en los servidores web que formarán parte de la granja de servidores:

- Agregar excepciones de firewall para permitir el **redes principales**, **administración remota**, y **compartir archivos e impresoras** características para comunicarse con el servidor de controlador WFF .
- Crear una cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) en Active Directory y agregue al grupo de administradores local en cada servidor. Usará esta cuenta como la cuenta de administrador de granja de servidor cuando se crea la granja de servidores.

Para obtener más información sobre cómo configurar estas excepciones de firewall en Firewall de Windows, vea [requisitos del sistema y plataforma de Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805128). Para otros sistemas de firewall, consulte la documentación del producto.

Puede usar el procedimiento siguiente para agregar una cuenta de dominio al grupo de administradores local en Windows Server 2008 R2. Debe llevar a cabo este procedimiento en todos los servidores que desea agregar a la granja de servidores&#x2014;en otras palabras, agregar la misma cuenta de dominio al grupo de administradores locales en el servidor principal y en cada servidor secundario.

**Para agregar una cuenta de dominio al grupo de administradores local**

1. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **el administrador del servidor**.
2. En el **el administrador del servidor** , en el panel de vista de árbol, expanda **configuración**, expanda **usuarios y grupos locales**y, a continuación, haga clic en **grupos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. En el **grupos** panel, haga doble clic en **administradores**.
4. En el **propiedades de administradores de** cuadro de diálogo, haga clic en **agregar**.
5. En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, escriba (o busque) a su cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. En el **propiedades de administradores de** cuadro de diálogo, haga clic en **Aceptar**.

Los servidores están preparados para agregarse a una granja de servidores. En el caso del servidor principal, puede configurar el servidor para satisfacer los requisitos de la aplicación antes o después de crear la granja de servidores&#x2014;en ambos casos, el WFF sincronizará los servidores mediante la implementación de los mismos productos, componentes o configuración a los servidores secundarios. Por simplicidad, este tutorial se supone que configurará el servidor principal cuando haya terminado de crear la granja de servidores.

## <a name="create-the-wff-server-farm"></a>Crear la granja de servidores WFF

En este momento, todos los servidores están preparados para agregarse a una granja de servidores WFF:

- Ha instalado WFF en el servidor de controlador.
- Ha configurado las excepciones del firewall en los servidores web principal y secundaria.
- Ha agregado una cuenta de dominio al grupo de administradores locales en los servidores web principal y secundaria.

El siguiente paso es crear la granja de servidores en WFF. Puede hacer esto desde el Administrador de IIS en el servidor de controlador WFF.

**Para crear una granja de servidores WFF**

1. En el servidor de controlador WFF, en la **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Internet Information Services (IIS) Manager**.
2. En el **conexiones** panel, expanda el nodo del servidor local, haga clic en **granjas de servidores**y, a continuación, haga clic en **crear granja de servidores**.
3. En el **crear granja de servidores** diálogo cuadro, escriba un nombre descriptivo para la granja de servidores (por ejemplo, **granja de servidores de ensayo**) y, a continuación, seleccione **granja de servidores de aprovisionar**.
4. Escriba el nombre de usuario y la contraseña de la cuenta de dominio que ha agregado al grupo de administradores local en cada servidor.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Haga clic en **Siguiente**.
6. En el **agregar servidores** página, escriba el nombre de dominio completo (FQDN) del servidor principal, seleccione **servidor principal**y, a continuación, haga clic en **agregar**.
7. En este momento, WFF intentará ponerse en contacto con el servidor principal con las credenciales proporcionadas. Si la conexión se realiza correctamente, el servidor principal se agrega a la tabla en la **agregar servidores** página.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Es posible que haya observado que **servidor está disponible para el equilibrio de carga** está activada de forma predeterminada. WFF utiliza el módulo de ARR IIS para implementar el equilibrio de carga y, por tanto, distribuir las solicitudes entre los servidores web de la granja de servidores. En la mayoría de los escenarios, solo eliminaría la **servidor está disponible para el equilibrio de carga** opción si desea utilizar una en su lugar de la solución de equilibrio de carga de terceros.
8. En el **agregar servidores** página, escriba el FQDN del primer servidor secundario y, a continuación, haga clic en **agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Repita el paso 7 para los servidores secundarios adicionales en la granja de servidores y, a continuación, haga clic en **finalizar**.

La granja de servidores WFF está ahora en funcionamiento. Los productos de la plataforma web o los componentes que se instalan en el servidor principal y las aplicaciones web o contenido que se implementa en el servidor principal, se pueden aprovisionar automáticamente en todos los servidores secundarios.

WFF es un tema amplio y complejo, y puede obtener más información sobre la [Microsoft Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805129) sitio Web. Por el momento, sin embargo, hay dos áreas de características que debe tener en cuenta:

- *Aprovisionamiento de aplicaciones* es el proceso que se replica contenido desde el servidor principal, como las aplicaciones web y opciones de configuración, todos los servidores secundarios en la granja de servidores. Por ejemplo, si implementa la solución de ejemplo póngase en contacto con el administrador en el servidor de almacenamiento provisional principal, el proceso de aprovisionamiento de aplicación WFF implementará esta solución en todos los servidores de ensayo secundarios. De forma predeterminada, el proceso de aprovisionamiento de la aplicación se ejecuta cada 30 segundos.
- *Aprovisionamiento de la plataforma* es el proceso que sincroniza los productos de la plataforma web y los componentes del servidor principal para todos los servidores secundarios en la granja de servidores. Por ejemplo, si instala ASP.NET MVC 3 en el servidor de almacenamiento provisional principal, el proceso de aprovisionamiento de plataforma utilizará al instalador de plataforma Web para instalar ASP.NET MVC 3 en todos los servidores de ensayo secundarios. De forma predeterminada, el proceso de aprovisionamiento de plataforma se ejecuta cada cinco minutos.

Puede administrar básicos de aplicación y configuración de aprovisionamiento de plataforma desde el Administrador de IIS en el servidor de controlador WFF.

**Explorar la aplicación y la configuración de aprovisionamiento de plataforma**

1. En el Administrador de IIS, en la **conexiones** panel, seleccione la granja de servidores.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. En el **granja de servidores** panel, haga doble clic en **aprovisionamiento de aplicaciones**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Como puede ver, la granja de servidores está configurada actualmente para sincronizar los valores de configuración y contenido de web entre el servidor principal y los servidores secundarios cada 30 segundos.
4. Haga clic en **Atrás**y, a continuación, haga doble clic en **aprovisionamiento de la plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Como puede ver, la granja de servidores está configurada actualmente para sincronizar componentes entre el servidor principal y los servidores secundarios y los productos de la plataforma web cada cinco minutos.
6. Haga clic en **volver**.
7. Para forzar la granja de servidores para sincronizar inmediatamente, los productos de la plataforma web en el **acciones** panel, haga clic en **aprovisionar plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Aprovisionamiento de la plataforma puede tardar algún tiempo. El proceso del instalador se ejecuta en segundo plano en los servidores secundarios en la granja de servidores.
8. Una vez que se ha permitido el tiempo suficiente para completar el proceso de aprovisionamiento, puede comprobar que los productos y componentes que ha agregado al servidor principal se han replicado ahora en los servidores secundarios. Por ejemplo, puede iniciar sesión en un servidor secundario y usar la **el administrador del servidor** ventana para comprobar que se ha instalado el rol de servidor web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. También puede comprobar la lista de programas instalados para comprobar que se han agregado varios componentes de plataforma web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurar el equilibrio de carga

Cuando se crea una granja de servidores web, debe configurar alguna forma de equilibrio de carga para distribuir las solicitudes HTTP entre los servidores web. Podría tratarse de ARR de IIS, el equilibrio de carga de red de Windows Server 2008 o una tercero basado en hardware o software solución equilibrio de carga.

WFF está diseñado para integrarse estrechamente con IIS ARR. Para aprovechar las ventajas de esta integración, debe instalar el módulo ARR en el servidor de controlador WFF. A continuación, dirige todo el tráfico web en el servidor de controlador, normalmente mediante la configuración de registros de sistema de nombres de dominio (DNS). El servidor de controlador, a continuación, encargará de distribuir las solicitudes entrantes entre los servidores de la granja de servidores, en función de la disponibilidad del servidor y varios otros criterios.

> [!NOTE]
> No tienes que usar ARR con WFF; Puede configurar WFF para trabajar con soluciones de equilibrio de carga de terceros. Para obtener más información, consulte [información general de Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805126).


Equilibrio de carga con ARR es un tema complejo, más de los cuales está fuera del ámbito de este tutorial. Sin embargo, puede utilizar el procedimiento siguiente para instalar el módulo ARR y empezar a trabajar con equilibrio de carga.

**Para configurar el equilibrio de carga en el servidor de controlador WFF**

1. En el servidor de controlador WFF, inicie al instalador de plataforma Web.
2. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **Server**.
4. En el **2.5 de enrutamiento de solicitud de aplicación** la fila, haga clic en **agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Haga clic en **instalar**y, a continuación, siga las instrucciones que aparecen en la **instalación de plataforma Web** ventana.
6. Una vez completada la instalación, inicie el Administrador de IIS y en el **conexiones** panel, haga clic en el nodo de la granja de servidores. Tenga en cuenta que se han agregado varios iconos nuevo a la **granja de servidores** panel.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. En el **granja de servidores** panel, haga doble clic en **equilibrio de carga**.
8. En el **equilibrio de carga** panel, seleccione una carga equilibrar algoritmo (por ejemplo, **solicitud actual menos**).

    > [!NOTE]
    > Para obtener más información sobre los algoritmos y otras opciones de configuración de equilibrio de carga, consulte [el módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. En el **acciones** panel, haga clic en **aplicar**.

Ya ha configurado básica equilibrio de carga para los servidores de la granja de servidores. Si se dirige todo el tráfico de granja de servidores web en el servidor de controlador, las solicitudes se distribuirán entre los servidores de la granja de servidores según la disponibilidad y el algoritmo de equilibrio de carga que ha seleccionado.

Para obtener más información sobre cómo configurar el equilibrio de carga con ARR, consulte [el módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitor de la granja de servidores

Puede supervisar el estado de la granja de servidores en cualquier momento mediante el Administrador de IIS en el servidor de controlador. En el **conexiones** panel, expanda la granja de servidores y, a continuación, haga clic en **servidores**. El panel central mostrará un resumen de todos los servidores de la granja de servidores junto con un registro de seguimiento de la actividad reciente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusión

La granja de servidores WFF ahora debe estar activados y ejecutándose. Puede configurar el servidor principal para admitir el enfoque de implementación que prefiera&#x2014;consulte la sección de información adicional para obtener más información&#x2014;y su configuración se replicará en cada servidor secundario en la granja de servidores.

## <a name="further-reading"></a>Información adicional

Para obtener más información sobre todos los aspectos de cómo configurar y usar el WFF, consulte el [Microsoft Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805129) sitio Web.

> [!div class="step-by-step"]
> [Anterior](configuring-a-database-server-for-web-deploy-publishing.md)
> [Siguiente](configuring-deployment-properties-for-a-target-environment.md)
