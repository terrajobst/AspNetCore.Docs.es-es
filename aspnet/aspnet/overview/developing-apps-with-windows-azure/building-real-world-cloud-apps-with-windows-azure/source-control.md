---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: (Creación de aplicaciones de nube reales con Azure) del Control de código fuente | Documentos de Microsoft
author: MikeWasson
description: Las aplicaciones de nube de creación Real World con libros electrónicos Azure se basa en una presentación desarrollada por Scott Guthrie. Se explican 13 patrones y prácticas recomendadas que puede...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/23/2015
ms.topic: article
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 0022458fa89a3be7ee8303750ad0e072df3b1bab
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Control de código fuente (creación de aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson](https://github.com/Rick-Anderson), [Tom Dykstra](https://github.com/tdykstra)

[Descarga corregirlo proyecto](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libros electrónicos](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **compilar aplicaciones de nube de mundo Real con Azure** libros electrónicos se basa en una presentación desarrollada por Scott Guthrie. Se explican los 13 patrones y prácticas recomendadas que le permitirá tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).


Control de código fuente es esencial para todos los proyectos de desarrollo en la nube, no solo los entornos de equipo. No cree de la edición de código fuente o incluso un documento de Word sin una función de deshacer y copias de seguridad automáticas y control de código fuente proporciona las funciones en el nivel de proyecto en la que pueden guardar incluso más tiempo cuando algo va mal. Con servicios de control de código fuente en la nube, ya no tiene que preocuparse sobre instalación complicada y puede usar el control de código fuente de Visual Studio Online gratuita para máximo de 5 usuarios.

La primera parte de este capítulo explica tres prácticas recomendadas clave debe tener en cuenta:

- [Tratar los scripts de automatización como código fuente](#scripts) y versión les junto con el código de aplicación.
- [No buscar nunca en secretos](#secrets) (datos confidenciales, como las credenciales) en un repositorio de código fuente.
- [Configurar las bifurcaciones de origen](#devops) para habilitar el flujo de trabajo de DevOps.

El resto de este capítulo proporciona algunas implementaciones de ejemplo de estos patrones en Visual Studio, Azure y Visual Studio Online:

- [Agregar secuencias de comandos al control de código fuente en Visual Studio](#vsscripts)
- [Almacenar datos confidenciales en Azure](#appsettings)
- [Usar Git en Visual Studio y Visual Studio en línea](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Tratar los scripts de automatización como código fuente

Cuando se trabaja en un proyecto de nube que va a modificar cosas con frecuencia y desea poder reaccionar rápidamente a los problemas notificados por los clientes. Responder rápidamente implica el uso de scripts de automatización, como se explica en la [automatizar todo](automate-everything.md) capítulo. Todos los scripts que utilizan para crear el entorno, implementar en el mismo, escala, etc., necesita estar sincronizado con el código fuente de aplicación.

Para mantener sincronizadas con el código de secuencias de comandos, almacenarlas en el sistema de control de código fuente. A continuación, si alguna vez tiene que revertir los cambios o hacer una corrección rápida en código de producción que es diferente de código para el desarrollo, no debe perder tiempo intentando realizar un seguimiento de los valores que han cambiado o que los miembros del equipo tienen copias de la versión que necesaria. Se asegura que las secuencias de comandos que necesita se sincronizan con la base de código que se necesite para y está seguro de que todos los miembros del equipo están trabajando con los mismos scripts. A continuación, si necesita automatizar las pruebas y la implementación de una revisión a la producción o el desarrollo nuevas características, tendrá el script correcta para el código que debe actualizarse.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>No se comprueban en secretos

Un repositorio de código fuente es suele ser accesible a personas demasiadas para sea un lugar seguro adecuadamente para datos confidenciales, como contraseñas. Si las secuencias de comandos se basan en información confidencial como contraseñas, parametrizar esos valores para que puedan no se guardan en el código fuente y almacenan los secretos en otro lugar.

Por ejemplo, Azure permite que descargar los archivos que contienen configuración de publicación para automatizar la creación de perfiles de publicación. Estos archivos incluyen nombres de usuario y las contraseñas que están autorizadas a administrar los servicios de Azure. Si utiliza este método para crear perfiles de publicación y, si se registra estos archivos al control de código fuente, puede ver cualquier persona con acceso al repositorio de los nombres de usuario y contraseñas. Puede almacenar con seguridad la contraseña en el perfil de publicación propio porque está cifrada y se encuentra en un *. pubxml.user* archivo que, de forma predeterminada, no se incluye en el control de código fuente.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Bifurcaciones de origen de estructura para facilitar el flujo de trabajo de DevOps

Cómo implementar bifurcaciones en el repositorio, afecta a la capacidad de desarrollar nuevas características y corregir problemas en producción. Aquí se muestra un patrón que una gran cantidad de medio tamaño del uso de los equipos:

![Estructura de bifurcación de origen](source-control/_static/image1.png)

La bifurcación principal siempre coincide con código que se encuentra en producción. Bifurcaciones de master corresponden a diferentes etapas en el ciclo de vida de desarrollo. La bifurcación development es donde se implementa las características nuevas. Para un equipo pequeño puede tener solo master y desarrollo, pero a menudo, se recomienda que las personas tienen una bifurcación ensayo entre el desarrollo y maestra. Puede utilizar el almacenamiento provisional para pruebas de integración final antes de que una actualización se mueve a la producción.

Para los equipos grandes puede ser bifurcaciones independientes para cada nueva característica; para un equipo más pequeño es posible que tenga todos los usuarios la comprobación a la bifurcación development.

Si tienes una bifurcación para cada característica, característica A una vez listo mezcla sus cambios de código fuente de la seguridad en el desarrollo de crear una bifurcación y hacia abajo en las demás ramas de característica. Este código de origen, proceso de mezcla puede llevar mucho tiempo y para evitar ese trabajo mientras mantiene características independientes, algunos equipos implementan alternativa llama *[característica alterna](http://en.wikipedia.org/wiki/Feature_toggle)* (también conocida como *característica marcas*). Esto significa que todo el código para todas las características de está en la misma bifurcación y habilitar o deshabilitar cada característica mediante el uso de modificadores en el código. Por ejemplo, suponga que una característica es un nuevo campo para repararlo tareas de aplicación y de características B agrega funcionalidad de almacenamiento en caché. El código para ambas características puede estar en la bifurcación development, pero sólo la mostrará de aplicación el nuevo campo cuando una variable se establece en true y sólo utilizará almacenamiento en caché cuando se establece una variable distinta en true. Si una característica no está listo para promocionar pero características B está listo, puede promocionar todo el código a producción con el modificador de característica A desactivar y activar la característica B. A continuación, puede finalizar una característica y promover una versión posterior, todos con ninguna combinación de código de origen.

Si no utiliza bifurcaciones o alterna para características, una estructura de bifurcación parecido a esto hace posible pasar el código de desarrollo al entorno de producción de una manera ágil y repetible.

Esta estructura permite reaccionar rápidamente a los comentarios de los clientes. Si necesita realizar una corrección rápida en producción, puede hacerlo de forma eficaz también de forma ágil. Puede crear una bifurcación fuera de maestra o de almacenamiento provisional, y cuando esté listo combinarlos en master y hacia abajo en bifurcaciones de desarrollo y la característica.

![Bifurcación de revisión](source-control/_static/image2.png)

Sin una estructura de bifurcación así con su separación de producción y bifurcaciones de desarrollo, un problema de producción podría poner en la posición de tener que promover el nuevo código de la característica junto con la corrección de producción. El nuevo código de función no sean completamente probada y lista para producción y es posible que deba hacer una gran cantidad de trabajo que realizar una copia de los cambios que no están preparados. O bien, es posible que deba retrasar la corrección para probar los cambios y preparar la implementación.

A continuación verá ejemplos de cómo implementar estos tres modelos en Visual Studio, Azure y Visual Studio Online. Estos son algunos ejemplos, en lugar de obtener instrucciones detalladas sobre métodos-a--it; Para obtener instrucciones detalladas que ofrecen todas del contexto es necesario, consulte la [recursos](#resources) sección al final del capítulo.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Agregar secuencias de comandos al control de código fuente en Visual Studio

Puede agregar scripts a control de código fuente en Visual Studio incluyéndolos en una carpeta de soluciones de Visual Studio (suponiendo que el proyecto de control de código fuente). Aquí es una manera de hacerlo.

Cree una carpeta para las secuencias de comandos en la carpeta de soluciones (la misma carpeta que tiene el *.sln* archivo).

![Carpeta de automatización](source-control/_static/image3.png)

Copie los archivos de script en la carpeta.

![Contenido de la carpeta de automatización](source-control/_static/image4.png)

En Visual Studio, agregue una carpeta de soluciones para el proyecto.

![Selección de menú de nueva carpeta de soluciones](source-control/_static/image5.png)

Y agregue los archivos de script a la carpeta de soluciones.

![Agregar elemento existente selección de menú](source-control/_static/image6.png)

![Cuadro de diálogo Agregar elemento existente](source-control/_static/image7.png)

Ahora se incluyen los archivos de script en el proyecto y control de código fuente está realizando el seguimiento de los cambios de versión, junto con los cambios de código de origen correspondientes.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Almacenar datos confidenciales en Azure

Si ejecuta la aplicación en un sitio Web de Azure, es una manera de evitar el almacenamiento de credenciales en el control de código fuente almacenar en Azure.

Por ejemplo, la aplicación repararlo se almacena en sus archivo Web.config archivo dos cadenas de conexión que tendrán las contraseñas en producción y una clave que proporciona acceso a su cuenta de almacenamiento de Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Si coloca los valores de producción real para esta configuración en su *Web.config* archivo, o si los coloca el *Web.Release.config* archivo para configurar una transformación de Web.config para insertarlos durante la implementación, se deberá almacenar en el repositorio de origen. Si especifica las cadenas de conexión de base de datos en la producción de perfil de publicación, la contraseña estará en su *.pubxml* archivo. (Puede excluir la *.pubxml* de archivos de control de código fuente, pero, a continuación, se pierde la ventaja del uso compartido de todas las demás opciones de implementación.)

Azure ofrece una alternativa a la **appSettings** y secciones de las cadenas de conexión la *Web.config* archivo. Aquí es la parte pertinente de la **configuración** ficha para un sitio web en el portal de administración de Azure:

![appSettings y connectionStrings en portal](source-control/_static/image8.png)

Al implementar un proyecto para este sitio web y se ejecuta la aplicación, los valores que haya almacenado en Azure invalidan los valores están en el archivo Web.config.

Puede establecer estos valores en Azure mediante el portal de administración o secuencias de comandos. El script de automatización de la creación de entorno se vio en el [automatizar todo](automate-everything.md) capítulo crea una base de datos de SQL Azure, obtiene el almacenamiento y las cadenas de conexión de base de datos SQL y almacena estos secretos en la configuración para el sitio web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Tenga en cuenta que los scripts tengan parámetros definidos para que los valores reales no se guardan en el repositorio de origen.

Cuando se ejecuta localmente en el entorno de desarrollo, la aplicación lee el archivo Web.config local y la conexión de puntos de cadena a una base de datos LocalDB de SQL Server en el *aplicación\_datos* carpeta del proyecto web. Cuando se ejecuta la aplicación en Azure y la aplicación intenta leer estos valores desde el archivo Web.config, lo que se obtiene de vuelta y se usan son los valores almacenados para el sitio Web, no lo que aparece realmente en el archivo Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-visual-studio-online"></a>Usar Git en Visual Studio y Visual Studio en línea

Puede utilizar cualquier entorno de control de código fuente para implementar la estructura de bifurcación de DevOps presentada anteriormente. Para los equipos distribuidos un [distribuyen el sistema de control de versiones](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) funcionen mejor; para los otros equipos una [sistema centralizada](http://en.wikipedia.org/wiki/Revision_control) podría funcionar mejor.

[GIT](http://git-scm.com/) es un DVCS que es se ha vuelto muy popular. Cuando usa Git para el control de código fuente, tendrá una copia completa del repositorio con todo su historial en el equipo local. Muchas personas prefieren que porque es más fácil seguir trabajando cuando no está conectado a la red, aún puede hacer confirma y reversiones, crear y cambiar las ramas y así sucesivamente. Incluso cuando esté conectado a la red, resulta más fácil y rápido crear bifurcaciones y cambiar las ramas cuando todo lo que es local. También puede hacer local confirmar o revertir sin tener un impacto en otros desarrolladores. Y puede procesar por lotes de confirmaciones antes de enviarlos al servidor.

[Microsoft Visual Studio Online](https://www.visualstudio.com/)(VSO), anteriormente conocido como servicio de Team Foundation, ofrece tanto Git y [Team Foundation Version Control](https://msdn.microsoft.com/library/ms181237(v=vs.120).aspx) (TFVC; centralizada de control de código fuente). Aquí de Microsoft en el grupo de Azure algunos equipos de utilizan el control de fuente centralizada, algunos uso distribuida, y algunos utilizan una combinación (centralizado para algunos proyectos y distribuidas para otros proyectos). El servicio VSO es gratuito para 5 usuarios. Puede registrarse para un plan gratuito [aquí](https://go.microsoft.com/fwlink/?LinkId=307137).

Visual Studio 2013 incorpora de primera clase [Git compatibilidad](https://msdn.microsoft.com/library/hh850437.aspx); esta es una rápida demostración de cómo funciona.

Con un proyecto abierto en Visual Studio 2013, haga clic en la solución en **el Explorador de soluciones**y elija **Agregar solución al Control de código fuente**.

![Agregar solución al Control de código fuente](source-control/_static/image9.png)

Visual Studio le pregunta si desea usar TFVC (control de versiones centralizado) o Git.

![Elija el Control de código fuente](source-control/_static/image10.png)

Al seleccionar la Git y haga clic en **Aceptar**, Visual Studio crea un nuevo repositorio Git local en la carpeta de soluciones. El nuevo repositorio no tiene archivos aún; tendrá que agregarlos al repositorio, realice una confirmación de Git. Haga clic en la solución en **el Explorador de soluciones**y, a continuación, haga clic en **confirmar**.

![Confirmar](source-control/_static/image11.png)

Visual Studio automáticamente crea etapas en todos los archivos de proyecto para la confirmación y mostrarlos en **Team Explorer** en el **cambios incluidos** panel. (Si hubiera algunos que no desea incluir en la confirmación, puede seleccionar, menú contextual y haga clic en **excluir**.)

![Team Explorer](source-control/_static/image12.png)

Escriba un comentario de confirmación y haga clic en **confirmación**, y Visual Studio se ejecuta la confirmación y muestra el identificador de confirmación.

![Cambios de Team Explorer](source-control/_static/image13.png)

Ahora si cambia algún código de modo que sea diferente del que está en el repositorio, puede ver fácilmente las diferencias. Menú contextual de un archivo que ha cambiado, seleccione **comparar con Unmodified**, y recibirá una pantalla de comparación que muestra los cambios sin confirmar.

![Comparar con sin modificar](source-control/_static/image14.png)

![Diferencias mostrando los cambios](source-control/_static/image15.png)

Puede ver fácilmente los cambios que está realizando y protegerlos.

Suponga que necesita realizar una bifurcación, puede hacerlo demasiado en Visual Studio. En **Team Explorer**, haga clic en **nueva bifurcación**.

![Nueva rama de Team Explorer](source-control/_static/image16.png)

Escriba el nombre de una rama, haga clic en **crear la bifurcación**, y si seleccionó **rama de desprotección**, Visual Studio desprotege automáticamente la nueva bifurcación.

![Nueva rama de Team Explorer](source-control/_static/image17.png)

Ahora puede realizar cambios en archivos y protegerlos en esta bifurcación. Y se pueden cambiar fácilmente entre bifurcaciones y Visual Studio automáticamente sincronizaciones de los archivos para crear una bifurcación lo que se han desprotegido. En este ejemplo de título en la página web  *\_Layout.cshtml* ha cambiado a "Revisión 1" en HotFix1 bifurcación.

![Rama Hotfix1](source-control/_static/image18.png)

Si se cambia al maestro de crear una bifurcación, el contenido de la  *\_Layout.cshtml* archivo automáticamente volver a lo que están en la bifurcación principal.

![Bifurcación principal](source-control/_static/image19.png)

Este un ejemplo simple de cómo puede crear una bifurcación y voltear y hacia atrás entre bifurcaciones rápidamente. Esta característica permite a un flujo de trabajo muy ágil mediante la estructura de bifurcación y scripts de automatización que se presentan en el [automatizar todo](automate-everything.md) capítulo. Por ejemplo, puede trabajar en la bifurcación Development, crear una bifurcación de revisión fuera de master, cambiar a la nueva bifurcación, realice los cambios en él y confirmarlos y después cambia a la bifurcación Development y continuar lo que estabas haciendo.

¿Qué ha visto aquí es la forma de trabajar con un repositorio de Git local en Visual Studio. En un entorno de equipo normalmente también empuje cambios hacia arriba en un repositorio común. Las herramientas de Visual Studio también permiten para que señale a un repositorio de Git remoto. Puede usar GitHub.com para ese fin, o puede usar [Git en Visual Studio Online](https://msdn.microsoft.com/library/hh850437.aspx) integrado con todas las otras funcionalidades Visual Studio Online como elemento de trabajo y seguimiento de los errores.

Esto no es la única manera de pueden implementar una estrategia de bifurcación agile, por supuesto. Puede habilitar el mismo flujo de trabajo agile con un repositorio de control de código fuente centralizada.

## <a name="summary"></a>Resumen

Medir el éxito de su sistema de control de código fuente tomando como base rapidez puede realizar un cambio y obtener en vivo de una manera predecible y segura. Si se encuentra asustado realizar un cambio, ya que tendrá que lleve a cabo un día o dos de las pruebas manuales en él, podría plantearse lo que tienes que hacer process-wise o test-wise para que pueda tomar ese cambio en minutos o en el peor ya no es de una hora. Una estrategia para hacer que consiste en implementar la integración continua y la entrega continua, que abordaremos en el [siguiente capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

El [Visual Studio Online](https://www.visualstudio.com/) portal proporciona servicios de soporte y documentación, y puede registrarse para una cuenta. Si tiene Visual Studio 2012 y le gustaría usar Git, vea [Visual Studio Tools para Git](https://visualstudiogallery.msdn.microsoft.com/abafc7d6-dcaa-40f4-8a5e-d6724bdb980c).

Para obtener más información acerca de TFVC (control de versiones centralizado) y Git (control de versiones distribuidas), vea los siguientes recursos:

- [¿Qué sistema de control de versiones se debe usar: TFVC o Git?](https://msdn.microsoft.com/library/vstudio/ms181368.aspx#tfvc_or_git_summary) La documentación de MSDN, incluye una tabla que resume las diferencias entre TFVC y Git.
- [Bueno, me gustaría Team Foundation Server y me gustaría Git, pero que es mejor?](https://blogs.msdn.com/b/visualstudiouk/archive/2013/08/05/well-i-like-team-foundation-server-and-i-like-git-but-which-is-better.aspx) Comparación de TFVC y Git.

Para obtener más información acerca de las estrategias de bifurcación, vea los siguientes recursos:

- [Creación de una canalización de versiones con Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentación de Microsoft Patterns and Practices. Consulte el capítulo 6 para obtener una explicación de las estrategias de bifurcación. Característica de abogados alterna en bifurcaciones de características y si se utilizan bifurcaciones para características, abogados consigue que sigan siendo corta duración (horas o días como máximo).
- [Guía de Control de versión](https://aka.ms/vsarsolutions). Guía de las estrategias de bifurcación por ALM Rangers. Consulte Strategies.pdf de bifurcación en la ficha de descargas.
- [Desarrollo de software con características alterna](https://msdn.microsoft.com/magazine/dn683796.aspx). Artículo de MSDN Magazine.
- [Característica alternar](http://martinfowler.com/bliki/FeatureToggle.html). Introducción a la característica alterna / función coloca una marca en el blog de Martin Fowler.
- [Activa o desactiva vs bifurcaciones de la característica de características](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Otra entrada de blog sobre alterna de característica, Dylan Smith.

Para obtener más información sobre cómo controlar la información confidencial que no se debe mantener en repositorios de control de código fuente, vea los siguientes recursos:

- [Las prácticas recomendadas para implementar las contraseñas y otros datos confidenciales en ASP.NET y el servicio de aplicación de Azure](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sitios Web de Azure: La conexión y las cadenas de aplicación funcionan las cadenas con](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica la característica de Azure que invalida `appSettings` y `connectionStrings` datos en el *Web.config* archivo.
- [Personalizar valores de configuración y aplicación en Azure sitios Web - Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obtener información acerca de otros métodos para mantener la información confidencial fuera del control de código fuente, consulte [ASP.NET MVC: mantener privada configuración fuera de Control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Anterior](automate-everything.md)
> [Siguiente](continuous-integration-and-continuous-delivery.md)
