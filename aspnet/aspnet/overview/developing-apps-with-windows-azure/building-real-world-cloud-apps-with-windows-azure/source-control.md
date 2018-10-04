---
uid: aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
title: (Crear aplicaciones de nube reales con Azure) del Control de código fuente | Microsoft Docs
author: MikeWasson
description: Building Real World Cloud Apps with e-book de Azure se basa en una presentación desarrollada por Scott Guthrie. Explican el 13 de patrones y prácticas que puede...
ms.author: riande
ms.date: 06/23/2015
ms.assetid: 2a0370d3-c2fb-4bf3-88b8-aad5a736c793
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control
msc.type: authoredcontent
ms.openlocfilehash: 5df863762523b62759bb4f7849ca2635e5241b0a
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577800"
---
<a name="source-control-building-real-world-cloud-apps-with-azure"></a>Control de código fuente (crear aplicaciones de nube reales con Azure)
====================
por [Mike Wasson](https://github.com/MikeWasson), [Rick Anderson]((https://twitter.com/RickAndMSFT)), [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto corregirlo](http://code.msdn.microsoft.com/Fix-It-app-for-Building-cdd80df4) o [descargar libro electrónico](http://blogs.msdn.com/b/microsoft_press/archive/2014/07/23/free-ebook-building-cloud-apps-with-microsoft-azure.aspx)

> El **Building Real World Cloud Apps with Azure** eBook se basa en una presentación desarrollada por Scott Guthrie. Se explican el 13 patrones y prácticas que pueden ayudarle a tener éxito el desarrollo de aplicaciones web para la nube. Para obtener información sobre el libro electrónico, consulte [el primer capítulo](introduction.md).

Control de código fuente es esencial para todos los proyectos de desarrollo en la nube, no solo los entornos de equipo. No piensa editar código fuente o incluso un documento de Word sin necesidad de una función de deshacer y copias de seguridad automáticas y control de código fuente le esas funciones en un nivel de proyecto donde puede guardar incluso más tiempo cuando algo va mal. Con servicios de control de código fuente en la nube, ya no tiene que preocuparse sobre la instalación y puede usar el control de código fuente de repositorios de Azure gratis para 5 usuarios.

La primera parte de este capítulo explica tres recomendaciones clave a tener en cuenta:

- [Tratar los scripts de automatización como código fuente](#scripts) y versión se les junto con el código de aplicación.
- [No buscar nunca en secretos](#secrets) (datos confidenciales, como las credenciales) en un repositorio de código fuente.
- [Configurar las ramas de origen](#devops) para habilitar el flujo de trabajo de DevOps.

El resto del capítulo ofrece algunas implementaciones de ejemplo de estos patrones en los repositorios de Azure, Azure y Visual Studio:

- [Agregar secuencias de comandos de control de código fuente en Visual Studio](#vsscripts)
- [Datos confidenciales en Azure Store](#appsettings)
- [Usar Git en Visual Studio y repositorios de Azure](#gittfs)

<a id="scripts"></a>
## <a name="treat-automation-scripts-as-source-code"></a>Tratar los scripts de automatización como código fuente

Cuando se trabaja en un proyecto de nube está cambiando las cosas con frecuencia y desea ser capaz de reaccionar rápidamente a los problemas notificados por los clientes. Responder rápidamente implica el uso de scripts de automatización, como se explica en el [automatizar todo](automate-everything.md) capítulo. Todos los scripts que usan para crear su entorno, implementar en él, escala, etc., se necesita para estar sincronizado con el código fuente de aplicación.

Para mantener sincronizadas con el código de secuencias de comandos, debe almacenarlas en el sistema de control de código fuente. A continuación, si necesita revertir los cambios o realizar una corrección rápida en el código de producción que es distinto del código de desarrollo, no tiene que perder tiempo intentando localizar qué valores han cambiado o que los miembros del equipo tienen copias de la versión que necesita. Está seguro de que los scripts que necesita están sincronizados con la base de código que se necesite para y le garantiza que todos los miembros del equipo están trabajando con los mismos scripts. A continuación, si se necesita automatizar las pruebas y la implementación de una revisión para desarrollar nuevas características o de producción, tendrá la secuencia de comandos adecuada para el código que debe actualizarse.

<a id="secrets"></a>
## <a name="dont-check-in-secrets"></a>No se registran los secretos

Un repositorio de código fuente es normalmente se accede a demasiadas personas para sea un lugar seguro adecuadamente para datos confidenciales, como contraseñas. Si las secuencias de comandos se basan en secretos como contraseñas, parametrizar esa configuración para que no se guarda en el código fuente y almacenar los secretos en otro lugar.

Por ejemplo, Azure le permite que descargar los archivos que contienen configuración de publicación con el fin de automatizar la creación de perfiles de publicación. Estos archivos incluyen los nombres de usuario y contraseñas que están autorizadas a administrar los servicios de Azure. Si utiliza este método para crear perfiles de publicación y, si se comprueban estos archivos al control de código fuente, puede ver cualquier persona con acceso al repositorio de esos nombres de usuario y contraseñas. Puede almacenar con seguridad la contraseña en el propio perfil porque está cifrada y se encuentra en un *. pubxml.user* archivo que no se incluye en el control de código fuente de forma predeterminada.

<a id="devops"></a>
## <a name="structure-source-branches-to-facilitate-devops-workflow"></a>Ramas de origen de estructura para facilitar el flujo de trabajo de DevOps

Cómo implementar las ramas del repositorio, afecta a su capacidad de desarrollar nuevas características y corregir problemas en producción. Este es un patrón que mucho del medio de tamaño de los equipos usan:

![Estructura de bifurcación de origen](source-control/_static/image1.png)

La rama maestra siempre coincide con el código que se encuentra en producción. Subramas master corresponden a las diferentes etapas en el ciclo de vida de desarrollo. La rama de desarrollo es donde se implementan nuevas características. Para un equipo pequeño podría tener solo maestro y desarrollo, pero a menudo, se recomienda que las personas tienen una rama de ensayo entre el desarrollo y maestra. Puede utilizar el almacenamiento provisional para las pruebas de integración final antes de que una actualización se mueve a producción.

Para que los equipos grandes puede ser bifurcaciones independientes para cada nueva característica; para un equipo pequeño podría tener todos los usuarios la comprobación a la rama de desarrollo.

Si tiene una rama para cada característica, una característica una vez listo combinación sus cambios de código fuente de la seguridad en el desarrollo de rama y abajo en otras bifurcaciones de característica. Este código fuente, proceso de mezcla puede llevar mucho tiempo, y para evitar ese trabajo mientras se mantiene las características independientes, algunos equipos implementan alternativa denominada *[/desactivaciones](http://en.wikipedia.org/wiki/Feature_toggle)* (también conocida como *marcas de características*). Esto significa que todo el código para todas las características está en la misma rama, pero habilitar o deshabilitar cada característica utilizando los modificadores en el código. Por ejemplo, suponga que una característica es un nuevo campo para las tareas de aplicación Fix It y de características B agrega funcionalidad de almacenamiento en caché. El código para ambas características puede estar en la bifurcación development, pero sólo la mostrará de aplicación el nuevo campo cuando una variable se establece en true y sólo utilizará almacenamiento en caché cuando se establece una variable diferente en true. Si no está listo para promocionar una característica, pero la característica B está listo, puede promover todo el código a producción con el modificador de característica A desactivar y activar la característica B. A continuación, puede finalizar una característica y promoverlo más adelante, todo ello con ninguna combinación de código fuente.

Si utiliza bifurcaciones o conmutadores de características, una estructura de bifurcación así le permite a su código desde el desarrollo en producción de una manera ágil y repetible.

Esta estructura también le permite responder rápidamente a los comentarios de clientes. Si necesita realizar una corrección rápida en producción, puede hacerlo de forma eficaz también de forma ágil. Puede crear una rama fuera maestra o de almacenamiento provisional y cuando esté listo, combinarlos en master y abajo en bifurcaciones de desarrollo y la característica.

![Rama de revisión](source-control/_static/image2.png)

Sin una estructura de rama similar al siguiente con su separación de producción y ramas de desarrollo, un problema de producción podría poner en la posición de la necesidad de promover el nuevo código de función junto con la corrección de producción. El nuevo código de función podría no ser totalmente probada y lista para producción y es posible que deba hacer una gran cantidad de trabajo de seguridad de los cambios que no está listos. O bien, es posible que deba retrasar la corrección con el fin de probar los cambios y tenerlos preparados implementar.

A continuación verá ejemplos de cómo implementar estos tres modelos en Visual Studio, Azure y repositorios de Azure. Estos son algunos ejemplos, en lugar de instrucciones detalladas paso a paso de how-to--TI; Para obtener instrucciones detalladas que ofrecen todas el contexto necesario, consulte la [recursos](#resources) sección al final del segmento.

<a id="vsscripts"></a>
## <a name="add-scripts-to-source-control-in-visual-studio"></a>Agregar secuencias de comandos de control de código fuente en Visual Studio

Puede agregar scripts para el control de código fuente en Visual Studio incluyéndolos en una carpeta de soluciones de Visual Studio (suponiendo que el proyecto de control de código fuente). Aquí es una manera de hacerlo.

Cree una carpeta para las secuencias de comandos en la carpeta de solución (la misma carpeta que tiene su *.sln* archivo).

![Carpeta de automatización](source-control/_static/image3.png)

Copie los archivos de script en la carpeta.

![Contenido de la carpeta de automatización](source-control/_static/image4.png)

En Visual Studio, agregue una carpeta de soluciones para el proyecto.

![Nueva selección de menú de la carpeta de solución](source-control/_static/image5.png)

Y agregue los archivos de script a la carpeta de soluciones.

![Agregar elemento existente selección del menú](source-control/_static/image6.png)

![Cuadro de diálogo Agregar elemento existente](source-control/_static/image7.png)

Ahora se incluyen los archivos de script en el proyecto y control de código fuente está realizando el seguimiento de los cambios de versión, junto con los cambios de código fuente correspondiente.

<a id="appsettings"></a>
## <a name="store-sensitive-data-in-azure"></a>Datos confidenciales en Azure Store

Si ejecuta la aplicación en un sitio Web de Azure, es una forma de evitar almacenar las credenciales en el control de código fuente almacenar en Azure.

Por ejemplo, la aplicación Fix It se almacena en sus archivo dos cadenas de conexión Web.config que tendrán las contraseñas en producción y una clave que proporciona acceso a su cuenta de almacenamiento de Azure.

[!code-xml[Main](source-control/samples/sample1.xml?highlight=2-3,11)]

Si coloca los valores de producción real sobre estas opciones en su *Web.config* archivo, o si los coloca el *Web.Release.config* archivo para configurar una transformación de Web.config para insertarlos durante la implementación, que se va almacenar en el repositorio de origen. Si escribe las cadenas de conexión de base de datos en la producción, perfil de publicación, la contraseña estará en su *.pubxml* archivo. (Puede excluir la *.pubxml* de archivo de control de código fuente, pero, a continuación, se pierde la ventaja de uso compartido de todas las demás opciones de implementación.)

Azure le ofrece una alternativa para el **appSettings** y secciones de las cadenas de conexión el *Web.config* archivo. Aquí es la parte relevante de la **configuración** pestaña para un sitio web del portal de administración de Azure:

![appSettings y connectionStrings de portal](source-control/_static/image8.png)

Al implementar un proyecto para este sitio web y se ejecuta la aplicación, rastreando los valores que se ha almacenado en Azure invalidan los valores están en el archivo Web.config.

Puede establecer estos valores en Azure mediante el portal de administración o secuencias de comandos. El script de automatización de la creación de entorno que vio en el [automatizar todo](automate-everything.md) capítulo crea una base de datos de SQL Azure, obtiene el almacenamiento y las cadenas de conexión de base de datos SQL y almacena estos secretos en la configuración de su sitio web.

[!code-powershell[Main](source-control/samples/sample2.ps1)]

[!code-powershell[Main](source-control/samples/sample3.ps1)]

Tenga en cuenta que las secuencias de comandos se parametrizan para que los valores reales no se guardan en el repositorio de origen.

Cuando se ejecuta localmente en el entorno de desarrollo, la aplicación lee el archivo Web.config local y la conexión de cadena apunta a una base de datos LocalDB de SQL Server en el *aplicación\_datos* carpeta del proyecto web. Cuando se ejecuta la aplicación en Azure y la aplicación intenta leer estos valores desde el archivo Web.config, lo que lo recibe y usa son los valores almacenados para el sitio Web, no lo que aparece realmente en el archivo Web.config.

<a id="gittfs"></a>
## <a name="use-git-in-visual-studio-and-azure-devops"></a>Usar Git en Visual Studio y Azure DevOps

Puede usar cualquier entorno de control de código fuente para implementar la estructura de bifurcación de DevOps presentada anteriormente. Para equipos distribuidos un [distribuyen el sistema de control de versiones](http://en.wikipedia.org/wiki/Distributed_revision_control) (DVCS) podría funcionar mejor; para otros equipos una [sistema centralizada](http://en.wikipedia.org/wiki/Revision_control) podría funcionar mejor.

[GIT](http://git-scm.com/) es un sistema de control de versión distribuida popular. Cuando se usa Git para control de código fuente, tiene una copia completa del repositorio con todo su historial en el equipo local. Muchas personas prefieren que porque es más fácil seguir trabajando cuando no está conectado a la red, aún puede hacer confirma y reversiones, crear y cambiar las ramas y así sucesivamente. Incluso cuando está conectado a la red, es más fácil y rápido crear ramas y cambiar las ramas cuando todo es local. También puede realizar confirmaciones locales y reversiones sin tener un impacto en otros desarrolladores. Y puede procesar por lotes las confirmaciones antes de enviarlos al servidor.

[Repositorios de Azure](/azure/devops/repos/index?view=vsts) ofrece ambos [Git](/azure/devops/repos/git/?view=vsts) y [Team Foundation Version Control](/azure/devops/repos/tfvc/index?view=vsts) (TFVC; centralizados de control de código fuente). Introducción a Azure DevOps [aquí](https://app.vsaex.visualstudio.com/signup).

Visual Studio 2017 incluye integrado, la primera clase [compatibilidad con Git](https://msdn.microsoft.com/library/hh850437.aspx). Este es un rápido de demostración de cómo funciona.

Con un proyecto abierto en Visual Studio, haga clic en la solución en **el Explorador de soluciones**y, a continuación, elija **Agregar solución al Control de código fuente**.

![Agregar solución al Control de código fuente](source-control/_static/image9.png)

Visual Studio le pregunta si desea usar TFVC (control de versiones centralizado) o Git.

![Elija el Control de código fuente](source-control/_static/image10.png)

Al seleccionar Git y haga clic en **Aceptar**, Visual Studio crea un nuevo repositorio Git local en la carpeta de solución. El nuevo repositorio no aún no hay ningún archivo; tendrá que agregarlos al repositorio haciendo una confirmación de Git. Haga clic en la solución en **el Explorador de soluciones**y, a continuación, haga clic en **confirmar**.

![Confirmar](source-control/_static/image11.png)

Visual Studio automáticamente crea etapas en todos los archivos de proyecto para la confirmación y mostrarlos en **Team Explorer** en el **cambios incluidos** panel. (Si se produjeron algunos que no desea incluir en la confirmación, puede seleccionar, con el botón secundario y haga clic en **excluir**.)

![Team Explorer](source-control/_static/image12.png)

Escriba un comentario de confirmación y haga clic en **confirmación**, y Visual Studio se ejecuta la confirmación y muestra el identificador de confirmación.

![Cambios de Team Explorer](source-control/_static/image13.png)

Ahora si cambia algo de código para que sea distinto del que está en el repositorio, puede ver fácilmente las diferencias. Con el botón secundario en un archivo que ha cambiado, seleccione **comparar con Unmodified**, y obtendrá una pantalla de comparación que muestra los cambios sin confirmar.

![Comparar con sin modificar](source-control/_static/image14.png)

![Diff mostrando los cambios](source-control/_static/image15.png)

Puede ver fácilmente los cambios que está realizando y protegerlos.

Suponga que necesita realizar una bifurcación, puede hacerlo también en Visual Studio. En **Team Explorer**, haga clic en **nueva rama**.

![Nueva rama de Team Explorer](source-control/_static/image16.png)

Escriba un nombre de rama, haga clic en **crear rama**, y si ha seleccionado **extraer rama**, Visual Studio desprotege automáticamente la nueva bifurcación.

![Nueva rama de Team Explorer](source-control/_static/image17.png)

Ahora puede realizar cambios en archivos y protegerlas en esta rama. Y puede cambiar fácilmente entre las ramas y Visual Studio automáticamente se sincroniza los archivos de rama donde se han desprotegido. En este ejemplo de título en la página web  *\_Layout.cshtml* ha cambiado a "Revisión 1" en HotFix1 rama.

![Rama Hotfix1](source-control/_static/image18.png)

Si cambia al maestro de rama, el contenido de la  *\_Layout.cshtml* archivo automáticamente volver a lo que están en la rama principal.

![Rama "master"](source-control/_static/image19.png)

Este un ejemplo sencillo de cómo puede crear una rama y voltear y hacia atrás entre ramas rápidamente. Esta característica permite un flujo de trabajo altamente ágil con la estructura de bifurcación y scripts de automatización que se presentan en el [automatizar todo](automate-everything.md) capítulo. Por ejemplo, puede estar trabajando en la bifurcación Development, crear una bifurcación de revisión partir de la maestra, cambie a la nueva rama, efectúan los cambios y confirmarlos y, a continuación, cambie a la bifurcación Development y continuar lo que estabas haciendo.

Lo que ha visto aquí es cómo trabajar con un repositorio de Git local en Visual Studio. En un entorno de equipo normalmente también inserta cambios en un repositorio común. Las herramientas de Visual Studio también le permiten para que apunte a un repositorio Git remoto. Puede usar GitHub.com para ese fin, o puede usar [Git y repositorios de Azure](/azure/devops/repos/git/overview?view=vsts) integrado con todas las demás funcionalidades de DevOps de Azure como elemento de trabajo y seguimiento de errores.

Esto no es la única manera que puede implementar una estrategia de ramificación de agile, por supuesto. Puede habilitar el mismo flujo de trabajo ágil mediante un repositorio de control de código fuente centralizado.

## <a name="summary"></a>Resumen

Medir el éxito de su sistema de control de código fuente según rápidamente cómo puede realizar un cambio y obtener en vivo de una manera segura y predecible. Si se descubre asustado realizar un cambio porque tiene que hacer un día o dos de las pruebas manuales en él, quizás se pregunte lo que debe hacer process-wise o test-wise para que puedan realizar ese cambio en cuestión de minutos o en el peor ya no es de una hora. Una estrategia para hacerlo consiste en implementar la integración continua y entrega continua, que abordaremos en el [siguiente capítulo](continuous-integration-and-continuous-delivery.md).

<a id="resources"></a>
## <a name="resources"></a>Recursos

Para obtener más información acerca de las estrategias de bifurcación, consulte los siguientes recursos:

- [Creación de una canalización de versión con Team Foundation Server 2012](https://msdn.microsoft.com/library/dn449957.aspx). Documentación de Microsoft Patterns and Practices. Consulte el capítulo 6 para obtener una explicación de las estrategias de bifurcación. Asesores de característica alterna a través de las bifurcaciones de característica y si se utilizan bifurcaciones de características, abogados manteniéndolos corta duración (horas o días como máximo).
- [Guía de Control de versión](https://aka.ms/vsarsolutions). Guía para las estrategias de bifurcación por ALM Rangers. Consulte Strategies.pdf bifurcación en la ficha de descargas.
- [Desarrollo de software con conmutadores de característica](https://msdn.microsoft.com/magazine/dn683796.aspx). Artículo de MSDN Magazine.
- [Botón de alternancia de características](http://martinfowler.com/bliki/FeatureToggle.html). Introducción a la característica alterna o las marcas de características en el blog de Martin.
- [Características de vs activa o desactiva las bifurcaciones de característica](http://geekswithblogs.net/Optikal/archive/2013/02/10/152069.aspx). Otra entrada de blog sobre los conmutadores de característica, Dylan Smith.

Para obtener más información sobre cómo controlar la información confidencial que no se debe mantener en repositorios de control de código fuente, consulte los siguientes recursos:

- [Procedimientos recomendados para implementar contraseñas y otros datos confidenciales en ASP.NET y Azure App Service](../../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).
- [Sitios Web de Azure: Cómo cadenas de aplicación y conexión funcionan las cadenas](https://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/). Explica la característica de Azure que invalida `appSettings` y `connectionStrings` datos en el *Web.config* archivo.
- [Personalizar valores de configuración y aplicación en Azure websites: con Stefan Schackow](https://azure.microsoft.com/documentation/videos/configuration-and-app-settings-of-azure-web-sites/).

Para obtener información acerca de otros métodos para mantener la información confidencial fuera de control de código fuente, vea [ASP.NET MVC: mantener privados configuración fuera de Control de código fuente](http://typecastexception.com/post/2014/04/06/ASPNET-MVC-Keep-Private-Settings-Out-of-Source-Control.aspx).

> [!div class="step-by-step"]
> [Anterior](automate-everything.md)
> [Siguiente](continuous-integration-and-continuous-delivery.md)
