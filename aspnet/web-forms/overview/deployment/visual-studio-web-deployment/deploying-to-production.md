---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
title: 'Implementación de Web ASP.NET con Visual Studio: implementación de producción | Documentos de Microsoft'
author: tdykstra
description: Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 416438a1-3b2f-4d27-bf53-6b76223c33bf
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-production
msc.type: authoredcontent
ms.openlocfilehash: f3b3898bd003ace100ba05619f2c45ca808462df
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-production"></a>Implementación de Web ASP.NET con Visual Studio: implementación de producción
====================
por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

En este tutorial, configure una cuenta de Microsoft Azure, crear entornos de ensayo y producción e implementar la aplicación web ASP.NET para el almacenamiento provisional y característica de publicación de entornos de producción con Visual Studio con un solo clic.

Si lo prefiere, puede implementar en un proveedor de hospedaje de terceros. La mayoría de los procedimientos descritos en este tutorial es las mismas para un proveedor de hospedaje o de Azure, salvo que cada proveedor tiene su propia interfaz de usuario para la administración de cuenta y el sitio web. Puede encontrar un proveedor de hospedaje en el [Galería de proveedores de](https://www.microsoft.com/web/hosting) en el sitio web Microsoft.com.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la página de solución de problemas en esta serie de tutoriales.

## <a name="get-a-microsoft-azure-account"></a>Obtener una cuenta de Microsoft Azure

Si ya no tiene una cuenta de Azure, puede crear una cuenta de prueba gratuita en tan solo unos minutos. Para obtener más información, consulte [evaluación gratuita de Azure](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).

## <a name="create-a-staging-environment"></a>Crear un entorno de ensayo

> [!NOTE]
> Puesto que este tutorial se escribió, servicio de aplicaciones de Azure agrega una nueva característica para automatizar muchos de los procesos para crear entornos de ensayo y producción. Vea [establecer entornos de ensayo para aplicaciones web en el servicio de aplicaciones de Azure](https://azure.microsoft.com/documentation/articles/web-sites-staged-publishing/).


Como se explica en la [implementar en el tutorial de entorno de prueba](deploying-to-iis.md), más el entorno de prueba confiable es un sitio web en el proveedor de hospedaje que simplemente como el sitio web de producción. Muchos proveedores de hospedaje de tendría que sopesar las ventajas de este y costo adicional significativo, pero en Azure puede crear una aplicación web gratuita adicionales como la aplicación de almacenamiento provisional. También necesita una base de datos y el gasto adicional para que en el gasto de la base de datos de producción será: ninguno o mínima. En Azure se paga para la cantidad de almacenamiento de base de datos que use en lugar de para cada base de datos y la cantidad de almacenamiento adicional que se usará en ensayo será mínima.

Como se explica en la [implementar en el tutorial de entorno de prueba](deploying-to-iis.md), de ensayo y producción que se va a implementar las dos bases de datos en una base de datos. Si desea mantenerlos independiente, el proceso podría ser el mismo excepto en que debe crear una base de datos adicional para cada entorno y seleccionaría la cadena de destino correcto para cada base de datos cuando se crea el perfil de publicación.

En esta sección del tutorial creará una aplicación web y la base de datos que se usará para el entorno de ensayo, y podrá implementar en almacenamiento provisional y de prueba existe antes de crear e implementar en el entorno de producción.

> [!NOTE]
> Los pasos siguientes muestran cómo crear una aplicación web en el servicio de aplicación de Azure mediante el portal de administración de Azure. En la versión más reciente del SDK de Azure, también puede hacerlo sin salir de Visual Studio, mediante el Explorador de servidores. En Visual Studio 2013, también puede crear una aplicación web directamente desde el cuadro de diálogo de publicación. Para obtener más información, vea [crear una aplicación web ASP.NET en el servicio de aplicación de Azure.](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)


1. En el [Portal de administración de Azure](https://manage.windowsazure.com/), haga clic en **sitios Web**y, a continuación, haga clic en **nuevo**.
2. Haga clic en **sitio Web**y, a continuación, haga clic en **creación personalizada**.

    El **nuevo sitio Web - Creación personalizada** abre el asistente. El **creación personalizada** asistente le permite crear un sitio web y una base de datos al mismo tiempo.
3. En el **crear sitio Web** paso del asistente, escriba una cadena en el **URL** cuadro que se usará como la dirección URL única para la aplicación del entorno de ensayo. Por ejemplo, escriba ContosoUniversity-staging123 (incluidos números aleatorios al final para que sea único en caso de que se toma ContosoUniversity ensayo).

    La dirección URL completa constará de las que se especifican aquí más el sufijo que verá junto al cuadro de texto.
4. En el **región** lista desplegable, elija la región que está más cercana a usted.

    Esta configuración especifica la aplicación web se ejecutará en qué centro de datos.
5. En el **base de datos** desplegable Elija **crear una nueva base de datos SQL**.
6. En el **nombre de cadena de conexión de base de datos** cuadro, deje el valor predeterminado, *DefaultConnection*.
7. Haga clic en la flecha que señala a la derecha en la parte inferior del cuadro.

    La siguiente ilustración muestra la **crear sitio Web** cuadro de diálogo con valores de ejemplo en ella. La dirección URL y la región que ha escrito será diferente.

    ![Crear sitio Web paso](deploying-to-production/_static/image1.png)

    El asistente avanza a la **especificar la configuración de la base de datos** paso.
8. En el **nombre** cuadro, escriba *ContosoUniversity* junto con un número aleatorio para que sea único, por ejemplo *ContosoUniversity123*.
9. En el **Server** cuadro, seleccione **nuevo servidor de base de datos de SQL Server**.
10. Escriba un nombre de administrador y una contraseña.

    No se escribe un nombre existente y la contraseña aquí. Que se están escribiendo un nuevo nombre y una contraseña que se está definiendo ahora para utilizar posteriormente al tener acceso a la base de datos.
11. En el **región** cuadro, elija la misma región que eligió para la aplicación web.

    Mantener el servidor web y el servidor de base de datos en la misma región proporciona el máximo rendimiento y reduce los gastos.
12. Haga clic en la marca de verificación en la parte inferior de la casilla para indicar que haya terminado.

    La siguiente ilustración muestra la **especificar la configuración de la base de datos** cuadro de diálogo con valores de ejemplo en ella. Los valores que ha escrito pueden ser diferentes.

    ![Paso de configuración de base de datos del sitio Web de New - crear con el Asistente para la base de datos](deploying-to-production/_static/image2.png)

    El Portal de administración que se devuelve a la página de sitios Web y el **estado** columna muestra que se está creando la aplicación web. Después de un tiempo (normalmente menos de un minuto), el **estado** columna muestra que la aplicación web se creó correctamente. En la barra de navegación de la izquierda, el número de aplicaciones web que tenga en su cuenta aparece junto a la **sitios Web** aparece junto al icono y el número de bases de datos de la **bases de datos SQL** icono.

    ![Página de sitios Web del Portal de administración de sitio web creado](deploying-to-production/_static/image3.png)

    El nombre de la aplicación será diferente de la aplicación de ejemplo en la ilustración.

## <a name="deploy-the-application-to-staging"></a>Implementar la aplicación en almacenamiento provisional

Ahora que ha creado una aplicación web y la base de datos para el entorno de ensayo, puede implementar el proyecto a él.

> [!NOTE]
> Estas instrucciones muestran cómo crear un perfil de publicación mediante la descarga de un *.publishsettings* archivo, que no solo funciona para Azure sino también para los proveedores de hospedaje de terceros. La versión más reciente del SDK de Azure también permite conectarse directamente a Azure desde Visual Studio y elegir en una lista de las aplicaciones web que tiene en su cuenta de Azure. En Visual Studio 2013, puede iniciar sesión Azure desde el **Publicar Web** diálogo o desde el **Explorador de servidores** ventana. Para obtener más información, consulte [crear una aplicación web ASP.NET en el servicio de aplicación de Azure](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).


### <a name="download-the-publishsettings-file"></a>Descargar el archivo .publishsettings

1. Haga clic en el nombre de la aplicación web que acaba de crear.

    ![Haga clic en el sitio para ir al panel](deploying-to-production/_static/image4.png)
2. En **vista rápida** en el **panel** , haga clic en **descargar perfil de publicación**.

    ![Perfil de publicación vínculo de descarga](deploying-to-production/_static/image5.png)

    Este paso descarga un archivo que contiene todas las opciones que necesita para implementar una aplicación en la aplicación web. Podrá importar este archivo en Visual Studio para que no tenga que escribir esta información manualmente.
3. Guardar el *.publishsettings* archivo en una carpeta que se puede acceder desde Visual Studio.

    ![guardar el archivo .publishsettings](deploying-to-production/_static/image6.png)

    > [!WARNING]
    > Seguridad: el *.publishsettings* archivo contiene las credenciales (sin codificar) que se utilizan para administrar sus servicios y las suscripciones de Azure. La práctica recomendada de seguridad para este archivo es almacenarlo temporalmente fuera de los directorios de origen (por ejemplo, en la carpeta bibliotecas\documentos) y, a continuación, eliminarlo una vez completada la importación. Un usuario malintencionado que obtenga acceso a la *.publishsettings* archivo puede editar, crear y eliminar los servicios de Azure.

### <a name="create-a-publish-profile"></a>Crear un perfil de publicación

1. En Visual Studio, haga clic en el proyecto ContosoUniversity en **el Explorador de soluciones** y seleccione **publicar** en el menú contextual.

    El **Publicar Web** abre el asistente.
2. Haga clic en el **perfil** ficha.
3. Haga clic en **importación**.
4. Navegue hasta la *.publishsettings* archivo que descargó anteriormente y, a continuación, haga clic en **abiertos**.

    ![Cuadro de diálogo de configuración de publicación](deploying-to-production/_static/image7.png)
5. En el **conexión** , haga clic en **validar conexión** para asegurarse de que la configuración es correcta.

    Cuando se ha validado la conexión, se muestra junto a una marca de verificación verde el **validar conexión** botón.

    Para algunos proveedores de hospedaje, al hacer clic en **validar conexión**, es posible que vea una **Error de certificado** cuadro de diálogo. Si lo hace, compruebe que el nombre del servidor es el esperado. Si el nombre del servidor es correcto, seleccione **guardar este certificado para sesiones futuras de Visual Studio** y haga clic en **Accept**. (Este error significa que el proveedor de hospedaje ha elegido evitar el gasto de adquirir un certificado SSL para la dirección URL que va a implementar. Si desea establecer una conexión segura mediante el uso de un certificado válido, póngase en contacto con su proveedor de hospedaje.)
6. Haga clic en **Siguiente**.

    ![icono de conexión correcta y el botón siguiente en la ficha conexión](deploying-to-production/_static/image8.png)
7. En el **configuración** , expanda **File Publish Options**y, a continuación, seleccione **excluir archivos de la aplicación\_carpeta de datos**.

    Para obtener información acerca de las demás opciones bajo **File Publish Options**, consulte el [implementación en IIS](deploying-to-iis.md) tutorial. La captura de pantalla que muestra el resultado de este paso y los siguientes pasos de configuración de base de datos está al final de los pasos de configuración de la base de datos.
8. En **DefaultConnection** en el **bases de datos** sección, configurar la implementación de base de datos de la base de datos de pertenencia.
9. 1. Seleccione **Actualizar base de datos**.

        El **cadena de conexión remota** directamente debajo del cuadro **DefaultConnection** se rellena con la cadena de conexión desde el archivo .publishsettings. La cadena de conexión incluye credenciales de SQL Server, que se almacenan en texto sin formato en el *.pubxml* archivo. Si no desea almacenarlas de forma permanente no existe, puede quitarlos del perfil de publicación después de implementa la base de datos y almacenarlos en Azure. Para obtener más información, consulte [cómo mantener la base de datos ASP.NET las cadenas de conexión segura cuando se implementa en Azure desde origen](http://www.hanselman.com/blog/HowToKeepYourASPNETDatabaseConnectionStringsSecureWhenDeployingToAzureFromSource.aspx) en el blog de Scott Hanselman.
      2. Haga clic en **configurar actualizaciones de base de datos**.
      3. En el **configurar actualizaciones de base de datos** cuadro de diálogo, haga clic en **Agregar secuencia de comandos SQL**.
      4. En el **Agregar secuencia de comandos SQL** , navegue hasta la *prod.sql de datos de aspnet* script que se guardó anteriormente en la carpeta de soluciones y, a continuación, haga clic en **abiertos**.
      5. Cerrar la **configurar actualizaciones de base de datos** cuadro de diálogo.
10. En **SchoolContext** en el **bases de datos** sección, seleccione **ejecutar migraciones de Code First (se ejecuta al iniciarse la aplicación)**.

    Visual Studio muestra **ejecutar Code First Migrations** en lugar de **Actualizar base de datos** de `DbContext` clases. Si desea utilizar el proveedor dbDacFx en lugar de las migraciones para implementar una base de datos que tiene acceso mediante un `DbContext` de clases, consulte [cómo se puede implementar una base de datos Code First sin migraciones?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) en las P+F de implementación Web para Visual Studio y ASP.NET en MSDN.

    El **configuración** ficha ahora es similar al ejemplo siguiente:

    ![Pestaña de configuración para el almacenamiento provisional](deploying-to-production/_static/image9.png)
11. Realice los pasos siguientes para guardar el perfil y cambie su nombre a *ensayo*:

    1. Haga clic en el **perfil** ficha y, a continuación, haga clic en **administrar perfiles**.
    2. La importación crea dos perfiles nuevos, uno para FTP y otro para Web Deploy. Configurar el perfil de Web Deploy: cambiar el nombre de este perfil para *ensayo*.

        ![Cambiar el nombre de perfil de almacenamiento provisional](deploying-to-production/_static/image10.png)
    3. Cerrar la **Editar perfiles de publicación de Web** cuadro de diálogo.
    4. Cerrar la **Publicar Web** asistente.

### <a name="configure-a-publish-profile-transform-for-the-environment-indicator"></a>Configurar una transformación de perfil de publicación para el indicador de entorno

> [!NOTE]
> Esta sección muestra cómo configurar una transformación de Web.config para el indicador de entorno. Dado que el indicador está en el `<appSettings>` elemento, tiene otra alternativa para especificar la transformación cuando se implementa en el servicio de aplicaciones de Azure. Para obtener más información, consulte [configuración de Web.config especificando en Azure](web-config-transformations.md#watransforms).


1. En **el Explorador de soluciones**, expanda **propiedades**y, a continuación, expanda **PublishProfiles**.
2. Haga clic en *Staging.pubxml*y, a continuación, haga clic en **Agregar configuración transformar**.

    ![Agregar transformación de configuración para el almacenamiento provisional](deploying-to-production/_static/image11.png)

    Visual Studio crea el *Web.Staging.config* archivo de transformación y lo abre.
3. En el *Web.Staging.config* transformar el archivo, inserte el código siguiente inmediatamente después de la apertura `configuration` etiqueta.

    [!code-xml[Main](deploying-to-production/samples/sample1.xml)]

    Cuando se usa el perfil de publicación de ensayo, esta transformación establece el indicador de entorno para "Prod". En la aplicación web implementada, no verá ningún sufijo, como "(desarrollo)" o "(Test)" después del encabezado H1 "Universidad de Contoso".
4. Haga clic en el *Web.Staging.config* de archivo y haga clic en **transformar de vista previa** para asegurarse de que la transformación se codificó produce cambios esperados.

    El **vista previa de Web.config** ventana muestra el resultado de aplicar el *Web.Release.config* transforma y *Web.Staging.config* transforma.

### <a name="prevent-public-use-of-the-test-app"></a>Impedir el uso público de la aplicación de prueba

Una consideración importante para la aplicación de almacenamiento provisional es que resultará en vivo en Internet, pero no desea que el público para usarlo. Para reducir la probabilidad de que personas encuentren y usarlo, puede usar uno o varios de los métodos siguientes:

- Establecer reglas de firewall que permitan el acceso a la aplicación de almacenamiento provisional sólo de direcciones IP que usan para probar el almacenamiento provisional.
- Use una dirección URL confuso que sería imposible saber.
- Crear un *robots.txt* archivo para asegurarse de que los motores de búsqueda no rastreará los vínculos de informe y la aplicación de prueba a él en resultados de búsqueda.

El primero de estos métodos es muy eficaz, pero no se trata en este tutorial porque requeriría que implementa en un servicio de nube de Azure en lugar de servicio de aplicaciones de Azure. Para obtener más información acerca de los servicios de nube y las restricciones de IP en Azure, vea [opciones de proceso de hospedaje proporcionado por Azure](https://docs.microsoft.com/azure/cloud-services/cloud-services-choose-me) y [bloque de direcciones IP específicas tengan acceso a un rol Web](https://msdn.microsoft.com/library/windowsazure/jj154098.aspx). Si va a implementar en un proveedor de hospedaje de terceros, póngase en contacto con el proveedor para obtener información sobre cómo implementar restricciones de IP.

Para este tutorial, creará un *robots.txt* archivo.

1. En **el Explorador de soluciones**, haga clic en el proyecto ContosoUniversity y haga clic en **Agregar nuevo elemento**.
2. Crear un nuevo **archivo de texto** denominado *robots.txt*e inserte el siguiente texto en ella:

    [!code-console[Main](deploying-to-production/samples/sample2.cmd)]

    El `User-agent` línea indica los motores de búsqueda que se aplican las reglas en el archivo para todos los rastreadores de web de motor de búsqueda (robots), y la `Disallow` línea especifica que no va a rastrear ninguna página en el sitio.

    ¿Desea motores de búsqueda en el catálogo de la aplicación de producción, por lo que necesita excluir este archivo de implementación de producción. Para ello, deberá configurar una configuración en la producción de perfil de publicación al crearlo.

### <a name="deploy-to-staging"></a>Implementar en almacenamiento provisional

1. Abra la **Publicar Web** Asistente haciendo clic en el proyecto de la Universidad de Contoso y haga clic en **publicar**.
2. Asegúrese de que el **ensayo** se selecciona el perfil.
3. Haga clic en **Publicar**.

    El **salida** ventana muestra qué acciones de implementación se realizaron y notifica la finalización correcta de la implementación. El explorador predeterminado se abre automáticamente a la dirección URL de la aplicación web implementadas.

## <a name="test-in-the-staging-environment"></a>Probar en el entorno de ensayo

Tenga en cuenta que el indicador de entorno esté ausente (no hay ninguna "(Test)" o "(desarrollo)" después del encabezado H1, que muestra que la *Web.config* transformación para el indicador de entorno fue correcta.

![Almacenamiento provisional de la página principal](deploying-to-production/_static/image12.png)

Ejecute el **estudiantes** página para comprobar que la base de datos implementada no tiene ningún alumno.

Ejecute el **instructores** página para comprobar que Code First propagado la base de datos con datos instructor:

Seleccione **agregar estudiantes** desde el **estudiantes** menú, agregue un estudiante y, a continuación, ver el nuevo alumno en el **estudiantes** página para comprobar que puede escribir correctamente en la base de datos .

Desde el **cursos** página, haga clic en **actualización créditos**. El **actualización créditos** página requiere permisos de administrador, por lo que la **inicio de sesión** se muestra la página. Escriba las credenciales de cuenta de administrador que creó anteriormente ("admin" y "prodpwd"). El **actualización créditos** aparece la página, que comprueba que la cuenta de administrador que creó en el tutorial anterior se implementó correctamente en el entorno de prueba.

Solicitar una dirección URL no válida y provoca un error que ELMAH realizará el seguimiento y, a continuación, solicitar el informe de errores ELMAH. Si va a implementar en un proveedor de hospedaje de terceros, es posible que el informe está vacío para la misma razón que estaba vacío en el tutorial anterior. Tendrá que usar herramientas de administración de cuenta del proveedor de hospedaje para configurar permisos de carpeta para habilitar ELMAH escribir en la carpeta de registro.

Ahora se ejecuta la aplicación que creó en la nube en una aplicación web que es similar a lo que va a utilizar para producción. Puesto que todo funciona correctamente, el paso siguiente es para implementarse en producción.

## <a name="deploy-to-production"></a>Implementar en producción

El proceso para crear una aplicación web de producción y la implementación en producción es la misma que la de almacenamiento provisional, salvo que tendrá que excluir el *robots.txt* de implementación. Para ello, edite el archivo de perfil de publicación.

### <a name="create-the-production-environment-and-the-production-publish-profile"></a>Crear el entorno de producción y perfil de publicación de la producción

1. Crear la aplicación web de producción y la base de datos en Azure, siga el mismo procedimiento que usa para el almacenamiento provisional.

    Cuando se crea la base de datos, puede optar por colocar en el mismo servidor que creó anteriormente, o crear un nuevo servidor.
2. Descargue el *.publishsettings* archivo.
3. Crear el perfil de publicación mediante la importación de la producción *.publishsettings* archivo, siguiendo el mismo procedimiento que usa para el almacenamiento provisional.

    No olvide configurar el script de implementación de datos con **DefaultConnection** en el **bases de datos** sección de la **configuración** ficha.
4. Cambiar el nombre del perfil de publicación para *producción*.
5. Configurar una transformación de perfil de publicación para el indicador de entorno, siguiendo el mismo procedimiento que usa para el almacenamiento provisional...

### <a name="edit-the-pubxml-file-to-exclude-robotstxt"></a>Edite el archivo .pubxml para excluir robots.txt

Se denominan los archivos de perfil de publicación &lt;profilename&gt;*.pubxml* y se encuentran en el *PublishProfiles* carpeta. El *PublishProfiles* carpeta se encuentra en la *propiedades* carpeta en una aplicación web de C# del proyecto, en la *mi proyecto* carpeta en un proyecto de aplicación web VB o en el *Aplicación\_datos* carpeta en un proyecto de aplicación web. Cada *.pubxml* archivo contiene valores que se aplican a un perfil de publicación. Los valores que escriba en el Asistente de publicación Web se almacenan en estos archivos, y puede modificarlos para crear o cambiar la configuración que no está disponible en la interfaz de usuario de Visual Studio.

De forma predeterminada, *.pubxml* archivos se incluyen en el proyecto cuando se crea un perfil de publicación, pero se puede excluir del proyecto y Visual Studio todavía los utilizará. Visual Studio busca en el *PublishProfiles* carpeta para *.pubxml* archivos, independientemente de si se incluyen en el proyecto.

Para cada *.pubxml* archivo hay una *. pubxml.user* archivo. El *. pubxml.user* archivo contiene la contraseña cifrada si ha seleccionado la **Guardar contraseña** opción y se excluye del proyecto de forma predeterminada.

A *.pubxml* archivo contiene los valores que forman parte de un perfil de publicación específicos. Si desea configurar las opciones que se aplican a todos los perfiles, puede crear un *. wpp.targets* archivo. El proceso de compilación importan estos archivos en el *.csproj* o *.vbproj* archivo de proyecto, por lo que la mayoría de las opciones que puede configurar en el archivo de proyecto puede configurarse en estos archivos. Para obtener más información acerca de *.pubxml* archivos y *. wpp.targets* archivos, vea [Cómo: modificar la configuración de implementación en los archivos de un perfil de publicación (.pubxml) y. wpp.targets archivo en Visual Studio Proyectos Web](https://msdn.microsoft.com/library/ff398069.aspx).

1. En **el Explorador de soluciones**, expanda **propiedades** y expanda **PublishProfiles**.
2. Haga clic en *Production.pubxml* y haga clic en **abiertos**.

    ![Abra el archivo .pubxml](deploying-to-production/_static/image13.png)
3. Haga clic en *Production.pubxml* y haga clic en **abiertos**.
4. Agregue las siguientes líneas inmediatamente antes del cierre `PropertyGroup` elemento:

    [!code-xml[Main](deploying-to-production/samples/sample3.xml)]

    El archivo .pubxml ahora es similar al ejemplo siguiente:

    [!code-xml[Main](deploying-to-production/samples/sample4.xml?highlight=18-20)]

    Para obtener más información acerca de cómo excluir archivos y carpetas, consulte [¿se puede excluir determinados archivos o carpetas de implementación?](https://msdn.microsoft.com/library/ee942158.aspx#can_i_exclude_specific_files_or_folders_from_deployment) en el **preguntas más frecuentes de implementación de Web para Visual Studio y ASP.NET** en MSDN.

### <a name="deploy-to-production"></a>Implementar en producción

1. Abra el **publicación Web** Asistente para asegurarse de que el **producción** perfil de publicación está seleccionada y, a continuación, haga clic en **iniciar Preview** en el **Preview**pestaña para comprobar que la *robots.txt* no se copiará el archivo en la aplicación de producción.

    ![Vista previa de los archivos se publiquen en producción](deploying-to-production/_static/image14.png)

    Revise la lista de archivos que se va a copiar. Verá que todos los *.cs* archivos, incluidos *. aspx.cs*, *. aspx.designer.cs*, *Master.cs*, y  *Master.Designer.cs* se omiten los archivos. Todo este código se ha compilado en el *ContosoUniversity.dll* y *ContosUniversity.pdb* archivos que encontrará en el *bin* carpeta. Dado que solo el *.dll* es necesaria para ejecutar la aplicación y se especificó anteriormente que se deben implementar sólo los archivos necesarios para ejecutar la aplicación, no *.cs* archivos se han copiado en el destino entorno. El *obj* carpeta y el *ContosoUniversity.csproj* y *. csproj.user* se omiten los archivos por la misma razón.

    Haga clic en **publicar** para implementar en el entorno de producción.
2. Probar en producción, siguiendo el mismo procedimiento utilizado para el almacenamiento provisional.

    Todo el contenido es idéntico al almacenamiento provisional excepto la dirección URL y la ausencia de la *robots.txt* archivo.

## <a name="summary"></a>Resumen

Ahora ha implementado y probado la aplicación web y está disponible públicamente a través de Internet.

![Producción de la página principal](deploying-to-production/_static/image15.png)

En el siguiente tutorial, podrá actualizar el código de la aplicación e implementar el cambio a los entornos de prueba, ensayo y producción.

> [!NOTE]
> Mientras la aplicación está en uso en el entorno de producción debe implementar un plan de recuperación. Es decir, debe ser periódicamente copias de seguridad las bases de datos desde la aplicación de producción en una ubicación de almacenamiento seguro y debe mantener varias generaciones de estas copias de seguridad. Cuando se actualiza la base de datos, debe realizar una copia de seguridad de inmediatamente antes del cambio. A continuación, si comete un error y no detectar hasta después de haber implementado en producción, aún podrá recuperar la base de datos al estado que tenía antes de resultó dañado. Para obtener más información, consulte [copia de seguridad de base de datos de SQL de Azure y restauración](https://msdn.microsoft.com/library/windowsazure/jj650016.aspx).
> 
> 
> [!NOTE]
> En este tutorial, el servidor SQL Server edición que va a implementar en es la base de datos de SQL Azure. Mientras el proceso de implementación es similar a otras ediciones de SQL Server, una aplicación de producción real podría necesitar un código especial para la base de datos de SQL Azure en algunos escenarios. Para obtener más información, consulte [trabajar con la base de datos de SQL Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdb) y [elegir entre SQL Server y base de datos de SQL Azure](../../../../whitepapers/aspnet-data-access-content-map.md#ssdbchoosing).
> 
> [!div class="step-by-step"]
> [Anterior](setting-folder-permissions.md)
> [Siguiente](deploying-a-code-update.md)
