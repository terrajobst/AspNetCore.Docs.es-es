---
uid: web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
title: Creación de una granja de servidores con el marco de granja de servidores Web | Microsoft Docs
author: jrjlee
description: Este tema describe cómo usar Web Farm Framework (WFF) 2.0 para crear y configurar una granja de servidores web de una colección de servidores.
ms.author: aspnetcontent
ms.date: 05/04/2012
ms.assetid: 656dd06d-806c-467c-863d-9fc45e5ba3ab
msc.legacyurl: /web-forms/overview/deployment/configuring-server-environments-for-web-deployment/creating-a-server-farm-with-the-web-farm-framework
msc.type: authoredcontent
ms.openlocfilehash: 5fdd1088f1f4e21f5e79b8c8379b7865f825ad53
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808945"
---
<a name="creating-a-server-farm-with-the-web-farm-framework"></a>Creación de una granja de servidores con el marco de granja de servidores Web
====================
por [Jason Lee](https://github.com/jrjlee)

[Descargar PDF](https://msdnshared.blob.core.windows.net/media/MSDNBlogsFS/prod.evol.blogs.msdn.com/CommunityServer.Blogs.Components.WeblogFiles/00/00/00/63/56/8130.DeployingWebAppsInEnterpriseScenarios.pdf)

> Este tema describe cómo usar Web Farm Framework (WFF) 2.0 para crear y configurar una granja de servidores web de una colección de servidores.


WFF le permite sincronizar los productos de la plataforma web y componentes, las aplicaciones web, sitios Web y opciones de configuración en varios servidores web con equilibrio de carga. En escenarios donde necesita más de un servidor web, como entornos de ensayo y producción, esto puede simplificar enormemente el proceso de implementación y configuración. Puede implementar una aplicación web en un único servidor&#x2014;el *servidor principal*&#x2014;y WFF replicará automáticamente esa aplicación web en todos los demás servidores de web en la granja de servidores.

## <a name="understanding-the-web-farm-framework"></a>Descripción del marco de granja de servidores Web

Puede usar WFF 2.0 para aprovisionar, administrar y distribuir contenido a un grupo de servidores web. Una implementación de WFF consta de tres funciones de servidor principales:

- El *servidor controlador*. Utilice este servidor para crear y configurar las granjas de servidores WFF. El servidor de controlador administra la sincronización de los componentes de plataforma web, configuración y las aplicaciones entre los servidores web en una granja de servidores. Instale WFF 2.0 en el servidor de controlador y el servidor de controlador a su vez instalará al agente WFF en cada uno de los servidores en una granja de servidores. El servidor de controlador conceptualmente no pertenece a ninguna granja de servidores WFF y un servidor de controlador único puede administrar varias granjas de servidores. En este escenario, usará un único servidor de controlador de wff que se va a crear y administrar la granja de servidores de ensayo y la granja de servidores de producción.
- El *servidor principal*. Cada conjunto de servidores WFF incluye un único servidor principal. Al instalar los componentes de plataforma web o implementar aplicaciones en el servidor principal, el WFF sincroniza los cambios realizados en todos los servidores en la granja de servidores.
- El *servidor secundario*. Cada conjunto de servidores WFF incluye uno o más servidores secundarios. Cualquier cambio que realice en el servidor principal se replica en cada servidor secundario dentro de la granja de servidores.

Esto muestra cómo se relacionan estos roles de servidor en los entornos de ensayo y producción de Fabrikam, Inc.:

![](creating-a-server-farm-with-the-web-farm-framework/_static/image1.png)

En este escenario, el entorno de ensayo y el entorno de producción se configuran como las granjas de servidores WFF. Un único servidor de controlador WFF administra ambos granjas de servidores. Dentro de cada conjunto de servidores, los cambios en el servidor principal se replican en cada servidor secundario.

Antes de empezar a configurar los entornos de ensayo y producción, recomendamos que lea estos artículos para familiarizarse con los conceptos clave de WFF 2.0:

- [Información general sobre el marco de granja de servidores de Web 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805126)
- [Configurar una granja de servidores con la Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805127)
- [Requisitos del sistema y plataforma para el marco de granja de servidores Web 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805128)

## <a name="task-overview"></a>Información general de tarea

Para completar las tareas y los tutoriales de este tema, necesitará al menos tres servidores&#x2014;uno o más servidores web secundaria para la granja de servidores, un servidor web principal para la granja de servidores y un controlador WFF. Puede agregar más servidores secundarios a una granja de servidores WFF en cualquier momento. En un nivel alto, para crear y configurar una granja de servidores WFF para su entorno de ensayo o producción, deberá:

- Crear un servidor de controlador mediante la instalación de Internet Information Services (IIS) 7.5 y 2.0 de WFF.
- Preparar los servidores principales y secundario mediante la creación de una cuenta de administrador comunes y configuración de excepciones de firewall.
- Configurar la granja de servidores mediante el Administrador de IIS en el servidor de controlador.
- Configurar Equilibrio de carga mediante IIS solicitud enrutamiento aplicaciones (ARR) o una tecnología alternativa de equilibrio de carga.

Las tareas y los tutoriales en este tema se suponen que está empezando con las compilaciones de servidor limpio que ejecuta Windows Server 2008 R2. Antes de comenzar, para cada servidor, asegúrese de que:

- Se instalan Windows Server 2008 R2 Service Pack 1 y todas las actualizaciones disponibles.
- El servidor está unido al dominio.
- El servidor tiene una dirección IP estática.

> [!NOTE]
> Para obtener más información sobre la unión a los equipos a un dominio, consulte [unir equipos al dominio e iniciar sesión](https://technet.microsoft.com/library/cc725618(v=WS.10).aspx). Para obtener más información sobre cómo configurar direcciones IP estáticas, consulte [configurar una dirección IP estática](https://technet.microsoft.com/library/cc754203(v=ws.10).aspx).


## <a name="create-the-wff-controller-server"></a>Crear el servidor de controlador WFF

Para crear un servidor de controlador WFF, deberá instalar IIS 7 o posterior y WFF 2.0 o posterior. En segundo plano, WFF usa la herramienta de implementación Web de IIS (Web Deploy) 2.x para sincronizar los servidores de la granja. Si utiliza al instalador de plataforma Web para instalar WFF, el instalador se descarga automáticamente e instalar Web Deploy para usted.

**Para crear el servidor de controlador WFF**

1. Descargue e instale el [instalador de plataforma Web](https://go.microsoft.com/?linkid=9739157).
2. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **Server**.
4. En el **configuración recomendada de IIS 7** la fila, haga clic en **agregar**.
5. En el <strong>Web Farm Framework 2.</strong> <em>x</em> la fila, haga clic en <strong>agregar</strong>.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image2.png)
6. Haga clic en **Instalar**. Tenga en cuenta que el instalador de plataforma Web ha agregado la herramienta de implementación Web, junto con diversas otras dependencias, a la lista de instalación.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image3.png)
7. Revise los términos de licencia y, si acepta los términos, haga clic en **acepto**.
8. Una vez completada la instalación, haga clic en **finalizar**y, a continuación, cierre el **instalador de plataforma Web 3.0** ventana.

## <a name="configure-the-primary-and-secondary-servers"></a>Configurar los servidores principales y secundario

Antes de crear una granja de servidores WFF, debe completar algunas tareas de preparación en los servidores web que se van a conformar la granja de servidores:

- Agregar excepciones de firewall para permitir el **redes principales**, **administración remota**, y **compartir archivos e impresoras** características para comunicarse con el servidor de controlador WFF .
- Crear una cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) en Active Directory y agréguelo al grupo de administradores local en cada servidor. Usará esta cuenta como cuenta de administrador de granja de servidor al crear la granja de servidores.

Para obtener más información sobre cómo configurar estas excepciones de firewall en el Firewall de Windows, consulte [del sistema y requisitos de plataforma Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805128). Para otros sistemas de firewall, consulte la documentación del producto.

Puede usar el procedimiento siguiente para agregar una cuenta de dominio al grupo de administradores local en Windows Server 2008 R2. Debe realizar este procedimiento en todos los servidores que desea agregar a la granja de servidores&#x2014;en otras palabras, agregue la misma cuenta de dominio al grupo de administradores local en el servidor principal y en cada servidor secundario.

**Para agregar una cuenta de dominio al grupo de administradores local**

1. En el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **administrador del servidor**.
2. En el **administrador del servidor** , en el panel de vista de árbol, expanda **configuración**, expanda **usuarios y grupos locales**y, a continuación, haga clic en **grupos**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image4.png)
3. En el **grupos** panel, haga doble clic en **administradores**.
4. En el **propiedades de administradores** cuadro de diálogo, haga clic en **agregar**.
5. En el **Seleccionar usuarios, equipos, cuentas de servicio o grupos** cuadro de diálogo, escriba (o busque) a su cuenta de dominio (por ejemplo, **FABRIKAM\stagingfarm**) y, a continuación, haga clic en **Aceptar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image5.png)
6. En el **propiedades de administradores** cuadro de diálogo, haga clic en **Aceptar**.

Los servidores ahora están listos para agregarse a una granja de servidores. En el caso del servidor principal, puede configurar el servidor para satisfacer las necesidades de su aplicación antes o después de crear la granja de servidores&#x2014;en ambos casos, el WFF sincronizará los servidores mediante la implementación de los mismos productos, componentes o configuración para los servidores secundarios. Por simplicidad, este tutorial se supone que configurará el servidor principal cuando haya terminado de crear la granja de servidores.

## <a name="create-the-wff-server-farm"></a>Crear la granja de servidores WFF

En este momento, todos los servidores están preparados para agregarse a una granja de servidores WFF:

- Ha instalado en el servidor de controlador de WFF.
- Ha configurado las excepciones de firewall en los servidores web principal y secundaria.
- Ha agregado una cuenta de dominio al grupo de administradores local en los servidores web principal y secundaria.

El siguiente paso es crear la granja de servidores de WFF. Puede hacerlo desde el Administrador de IIS en el servidor de controlador WFF.

**Para crear una granja de servidores WFF**

1. En el servidor de controlador WFF, en el **iniciar** menú, elija **herramientas administrativas**y, a continuación, haga clic en **Internet Information Services (IIS) Manager**.
2. En el **conexiones** panel, expanda el nodo del servidor, haga clic en **granjas de servidores**y, a continuación, haga clic en **crear granja de servidores**.
3. En el **crear granja de servidores** diálogo cuadro, escriba un nombre descriptivo para la granja de servidores (por ejemplo, **granja de servidores de ensayo**) y, a continuación, seleccione **aprovisionar granja de servidores**.
4. Escriba el nombre de usuario y la contraseña de la cuenta de dominio que ha agregado al grupo de administradores local en cada servidor.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image6.png)
5. Haga clic en **Siguiente**.
6. En el **agregar servidores** página, escriba el nombre de dominio completo (FQDN) del servidor principal, seleccione **servidor principal**y, a continuación, haga clic en **agregar**.
7. En este momento, WFF intentará ponerse en contacto con el servidor principal con las credenciales proporcionadas. Si la conexión se realiza correctamente, el servidor principal se agregarán a la tabla en la **agregar servidores** página.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image7.png)

    > [!NOTE]
    > Es posible que haya observado que **servidor está disponible para el equilibrio de carga** está activada de forma predeterminada. WFF usa el módulo de ARR para IIS para implementar el equilibrio de carga y, por tanto, distribuir las solicitudes entre los servidores web de la granja de servidores. En la mayoría de los escenarios, solo borraría el **servidor está disponible para el equilibrio de carga** opción si desea utilizar en su lugar de solución de equilibrio de carga de terceros.
8. En el **agregar servidores** página, escriba el FQDN de su primer servidor secundario y, a continuación, haga clic en **agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image8.png)
9. Repita el paso 7 para todos los servidores secundarios adicionales en la granja de servidores y, a continuación, haga clic en **finalizar**.

La granja de servidores WFF está ahora en funcionamiento. Los productos de la plataforma web o los componentes que se instalación en el servidor principal y las aplicaciones web o contenido que se implementa en el servidor principal, se aprovisionarán automáticamente en todos los servidores secundarios.

WFF es un tema amplio y complejo, y puede obtener más información sobre la [Microsoft Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805129) sitio Web. Por el momento, sin embargo, hay dos áreas de características que se deben tener en cuenta:

- *Aprovisionamiento de aplicaciones* es el proceso que replica contenido desde el servidor principal, como aplicaciones web y las opciones de configuración, en todos los servidores secundarios en la granja de servidores. Por ejemplo, si implementa la solución de ejemplo de Contact Manager en el servidor principal provisional, el proceso de aprovisionamiento de aplicaciones de WFF implementará esta solución para todos los servidores de almacenamiento provisionales secundarios. De forma predeterminada, el proceso de aprovisionamiento de la aplicación se ejecuta cada 30 segundos.
- *Aprovisionamiento de la plataforma* es el proceso que sincroniza los productos de la plataforma web y los componentes del servidor principal para todos los servidores secundarios en la granja de servidores. Por ejemplo, si instala ASP.NET MVC 3 en el servidor principal provisional, el proceso de aprovisionamiento de la plataforma usará al instalador de plataforma Web para instalar ASP.NET MVC 3 en todos los servidores de almacenamiento provisionales secundarios. De forma predeterminada, el proceso de aprovisionamiento de la plataforma se ejecuta cada cinco minutos.

Puede administrar aplicaciones básicas y configuración de aprovisionamiento de plataforma desde el Administrador de IIS en el servidor de controlador WFF.

**Explorar la plataforma de aplicaciones y configuración de aprovisionamiento**

1. En el Administrador de IIS, en el **conexiones** panel, seleccione la granja de servidores.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image9.png)
2. En el **granja de servidores** panel, haga doble clic en **aprovisionamiento de aplicaciones**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image10.png)
3. Como puede ver, la granja de servidores está configurada actualmente para sincronizar la configuración de contenido y configuración de web entre el servidor principal y los servidores secundarios cada 30 segundos.
4. Haga clic en **Atrás**y, a continuación, haga doble clic en **aprovisionamiento de la plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image11.png)
5. Como puede ver, la granja de servidores está configurada actualmente para sincronizar los productos de la plataforma web y componentes entre el servidor principal y los servidores secundarios cada cinco minutos.
6. Haga clic en **volver**.
7. Para forzar la granja de servidores para sincronizar inmediatamente, los productos de la plataforma web en el **acciones** panel, haga clic en **aprovisionar plataforma**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image12.png)

    > [!NOTE]
    > Aprovisionamiento de la plataforma puede tardar algún tiempo. El proceso del instalador se ejecuta en segundo plano en los servidores secundarios en la granja de servidores.
8. Una vez que ha permitido el tiempo suficiente para completar el proceso de aprovisionamiento, puede comprobar que los productos y componentes que ha agregado al servidor principal ya se han replicado en los servidores secundarios. Por ejemplo, puede iniciar sesión en un servidor secundario y usar el **administrador del servidor** ventana para comprobar que se ha instalado el rol de servidor web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image13.png)
9. También puede comprobar la lista de programas instalados para comprobar que se han agregado varios componentes de plataforma web.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image14.png)

## <a name="configure-load-balancing"></a>Configurar el equilibrio de carga

Cuando se crea una granja de servidores web, deberá configurar algún tipo de equilibrio de carga para distribuir las solicitudes HTTP entre los servidores web. Podría tratarse de ARR para IIS, el equilibrio de carga de red de Windows Server 2008 o una tercero basado en hardware o software solución equilibrio de carga.

WFF está diseñado para integrarse estrechamente con IIS ARR. Para poder aprovechar esta integración, deberá instalar el módulo ARR en el servidor de controlador WFF. , A continuación, dirigir todo el tráfico web al servidor de controlador, normalmente mediante la configuración de los registros del sistema de nombres de dominio (DNS). El servidor de controlador, a continuación, distribuirá las solicitudes entrantes entre los servidores de la granja de servidores, según la disponibilidad del servidor y otros criterios.

> [!NOTE]
> No tiene que usar ARR con WFF; Puede configurar WFF para trabajar con soluciones de equilibrio de carga de terceros. Para obtener más información, consulte [general de Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805126).


Equilibrio de carga con ARR es un tema complejo, más de los cuales está fuera del ámbito de este tutorial. Sin embargo, puede usar el procedimiento siguiente para instalar el módulo ARR y empezar a trabajar con equilibrio de carga.

**Para configurar el equilibrio de carga en el servidor de controlador WFF**

1. En el servidor de controlador WFF, inicie al instalador de plataforma Web.
2. En la parte superior de la **instalador de plataforma Web 3.0** ventana, haga clic en **productos**.
3. En el lado izquierdo de la ventana, en el panel de navegación, haga clic en **Server**.
4. En el **2.5 de enrutamiento de solicitud de aplicación** la fila, haga clic en **agregar**.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image15.png)
5. Haga clic en **instalar**y, a continuación, siga las instrucciones en el **instalación de la plataforma Web** ventana.
6. Una vez completada la instalación, inicie el Administrador de IIS y en el **conexiones** panel, haga clic en el nodo de granja de servidor. Tenga en cuenta que se han agregado varios iconos de nuevo a la **granja de servidores** panel.

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image16.png)
7. En el **granja de servidores** panel, haga doble clic en **equilibrio de carga**.
8. En el **equilibrio de carga** algoritmo equilibrar el panel, seleccione una carga (por ejemplo, **solicitud actual menos**).

    > [!NOTE]
    > Para obtener más información sobre otras opciones de configuración y los algoritmos de equilibrio de carga, consulte [módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

    ![](creating-a-server-farm-with-the-web-farm-framework/_static/image17.png)
9. En el **acciones** panel, haga clic en **aplicar**.

Ahora ha configurado para los servidores de la granja de servidores de equilibrio de carga básica. Si se dirige todo el tráfico al servidor de controlador de granja de servidores de web, las solicitudes se distribuye entre los servidores de la granja de servidores según la disponibilidad y el algoritmo de equilibrio de carga que ha seleccionado.

Para obtener más información sobre cómo configurar el equilibrio de carga con ARR, consulte [módulo de enrutamiento de solicitud de aplicación](https://go.microsoft.com/?linkid=9805130).

## <a name="monitor-the-server-farm"></a>Monitor de la granja de servidores

Puede supervisar el estado de la granja de servidores en cualquier momento a través del Administrador de IIS en el servidor de controlador. En el **conexiones** panel, expanda la granja de servidores y, a continuación, haga clic en **servidores**. El panel central mostrará un resumen de cada servidor en la granja de servidores junto con un registro de seguimiento de la actividad reciente.

![](creating-a-server-farm-with-the-web-farm-framework/_static/image18.png)

## <a name="conclusion"></a>Conclusión

Ahora debe ser la granja de servidores WFF en marcha. Puede configurar el servidor principal para admitir el enfoque de implementación que prefiera&#x2014;consulte la sección Lecturas adicionales para obtener más información&#x2014;y su configuración se replicará en cada servidor secundario en la granja de servidores.

## <a name="further-reading"></a>Información adicional

Para obtener instrucciones sobre todos los aspectos de la configuración y el uso de la WFF, consulte el [Microsoft Web Farm Framework 2.0 para IIS 7](https://go.microsoft.com/?linkid=9805129) sitio Web.

> [!div class="step-by-step"]
> [Anterior](configuring-a-database-server-for-web-deploy-publishing.md)
> [Siguiente](configuring-deployment-properties-for-a-target-environment.md)
