---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
title: "Implementación de Web ASP.NET con Visual Studio: implementar una actualización de código | Documentos de Microsoft"
author: tdykstra
description: "Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, usa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: c76dbc35-a914-4ee3-919c-4f4d1fa05104
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update
msc.type: authoredcontent
ms.openlocfilehash: f6861c702c1ccb19e5a4eee484a622079e205f86
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-code-update"></a>Implementación de Web ASP.NET con Visual Studio: implementar una actualización del código
====================
Por [Tom Dykstra](https://github.com/tdykstra)

[Descargar proyecto de inicio](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> Esta serie de tutoriales muestra cómo implementar (publicar) un ASP.NET web de aplicación para aplicaciones de Web del servicio de aplicación de Azure o en un proveedor de hospedaje de terceros, mediante Visual Studio 2012 o Visual Studio 2010. Para obtener información acerca de la serie, consulte [el primer tutorial de la serie](introduction.md).


## <a name="overview"></a>Información general

Tras la implementación inicial, continúa el trabajo de mantenimiento y desarrollo de su sitio web y, en poco tiempo desea implementar una actualización. Este tutorial le guiará por el proceso de implementar una actualización en el código de aplicación. La actualización que implementar e instalar en este tutorial no implica un cambio de la base de datos; podrá ver lo que es diferente sobre la implementación de un cambio de base de datos en el siguiente tutorial.

Aviso: Si aparece un mensaje de error o algo no funciona a medida que avances en el tutorial, asegúrese de comprobar la [página solución de problemas](troubleshooting.md).

## <a name="make-a-code-change"></a>Realizar un código de cambio

Como ejemplo sencillo de una actualización a la aplicación, agregará a la **instructores** página una lista de cursos que imparte el instructor seleccionado.

Si ejecuta el **instructores** página, observará que no hay **seleccione** vínculos en la cuadrícula, pero estos no hagan algo distinto de marca la fila segundo plano se volverán grises.

![Página de instructores con selección](deploying-a-code-update/_static/image1.png)

Ahora agregará código que se ejecuta cuando el **seleccione** vínculo se hace clic en y muestra una lista de cursos que imparte el instructor seleccionado.

1. En *Instructors.aspx*, agregue el siguiente marcado inmediatamente después de la **ErrorMessageLabel** `Label` control:

    [!code-aspx[Main](deploying-a-code-update/samples/sample1.aspx)]
2. Ejecute la página y seleccione un instructor. Ver una lista de cursos imparten por ese instructor.

    ![Página de instructores con cursos impartida](deploying-a-code-update/_static/image2.png)
3. Cierre el explorador.

## <a name="deploy-the-code-update-to-the-test-environment"></a>Implemente la actualización de código en el entorno de prueba

Para poder usar los perfiles de publicación para implementar en la prueba, ensayo y producción, debe cambiar las opciones de publicación de base de datos. Ya no es necesario ejecutar los scripts de implementación grant y datos de la base de datos de pertenencia.

1. Abra la **Publicar Web** Asistente haciendo clic en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Haga clic en el **prueba** perfil en el **perfil** lista desplegable.
3. Haga clic en el **configuración** ficha.
4. En **DefaultConnection** en el **bases de datos** sección, desactive el **Actualizar base de datos** casilla de verificación.
5. Haga clic en el **perfil** ficha y, a continuación, haga clic en el **ensayo** perfil en el **perfil** lista desplegable.
6. Cuando se le pregunte si desea guardar los cambios realizados en el **prueba** de perfil, haga clic en **Sí**.
7. Realizar el mismo cambio en el perfil de almacenamiento provisional.
8. Repita el proceso para realizar el mismo cambio en el perfil de producción.
9. Cerrar la **Publicar Web** asistente.

Implementación en el entorno de prueba es ahora un sencillo de la ejecución con un solo clic, vuelva a publicar. Para facilitar este proceso más rápido, puede usar el **Web uno haga clic en publicar** barra de herramientas.

1. En el **vista** menú, elija **las barras de herramientas** y, a continuación, seleccione **Web uno haga clic en publicar**.

    ![Selecting_One_Click_Publish_toolbar](deploying-a-code-update/_static/image3.png)
2. En **el Explorador de soluciones**, seleccione el proyecto ContosoUniversity.
3. el **Web uno haga clic en publicar** barra de herramientas, elija la **prueba** perfil de publicación y, a continuación, haga clic en **Publicar Web** (el icono con flechas que apuntan a izquierda y derecha).

    ![Web_One_Click_Publish_toolbar](deploying-a-code-update/_static/image4.png)
4. Visual Studio implementa la aplicación actualizada, y el explorador se abre automáticamente en la página principal.
5. Ejecute la página de instructores y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.

Lo haría normalmente también hacer pruebas de regresión (es decir, probar el resto del sitio para asegurarse de que el nuevo cambio no interrumpe cualquier funcionalidad existente). Pero en este tutorial podrá omitir este paso y continuar para implementar la actualización a ensayo y producción.

Al volver a implementar, Web Deploy determina automáticamente qué archivos han cambiado y copias de solo archivos cambiados en el servidor. De forma predeterminada, Web Deploy utiliza cambiado última fechas en los archivos para determinar cuáles han cambiado. Algunos sistemas de control de código fuente cambian el archivo fechas incluso cuando no cambie el contenido del archivo. En ese caso, puede configurar Web Deploy para usar las sumas de comprobación de archivo para determinar qué archivos han cambiado. Para obtener más información, consulte [¿por qué todos mis archivos obtener volvió a implementar aunque no cambiarlas?](https://msdn.microsoft.com/library/ee942158.aspx#use_checksum) en las preguntas más frecuentes de implementación de ASP.NET.

## <a name="take-the-application-offline-during-deployment"></a>Hacer que la aplicación sin conexión durante la implementación

El cambio que se va a implementar es ahora un cambio simple en una sola página. Pero a veces implementar cambios mayores, o implementar cambios de código y de base de datos y el sitio de comportamiento podría ser incorrecto si un usuario solicita una página antes de que finalice la implementación. Para evitar que los usuarios tienen acceso al sitio durante la implementación está en curso, puede usar un *aplicación\_offline.htm* archivo. Cuando coloca un archivo denominado *aplicación\_offline.htm* en la carpeta raíz de la aplicación, IIS muestra automáticamente ese archivo en lugar de ejecutar la aplicación. Por lo que para evitar el acceso durante la implementación, coloca *aplicación\_offline.htm* en la carpeta raíz, ejecute el proceso de implementación y, a continuación, quitar *aplicación\_offline.htm* después correcta implementación.

Puede configurar Web Deploy para poner automáticamente el valor predeterminado es *aplicación\_offline.htm* en el servidor de archivos cuando se inicia la implementación y quitarlo cuando termina. Para hacer que todo lo que debe hacer es agregar el elemento XML siguiente en el archivo de perfil (.pubxml) de la publicación:

[!code-xml[Main](deploying-a-code-update/samples/sample2.xml)]

Para este tutorial, verá cómo crear y utilizar un personalizado *aplicación\_offline.htm* archivo.

Usar *aplicación\_offline.htm* en el sitio de ensayo no es necesario, porque no tiene usuarios que acceden al sitio de ensayo. Pero es recomendable usar el almacenamiento provisional para probar todo lo que tiene previsto implementar en producción.

### <a name="create-appofflinehtm"></a>Crear aplicación\_offline.htm

1. En **el Explorador de soluciones**, haga clic en la solución y haga clic en **agregar**y, a continuación, haga clic en **nuevo elemento**.
2. Crear un **página HTML** denominado *aplicación\_offline.htm* (eliminar la última "l" en el *.html* extensión de Visual Studio crea de forma predeterminada).
3. Reemplace el marcado de plantilla con el siguiente marcado:

    [!code-html[Main](deploying-a-code-update/samples/sample3.html)]
4. Guarde y cierre el archivo.

### <a name="copy-appofflinehtm-to-the-root-folder-of-the-web-site"></a>Aplicación de copia\_offline.htm a la carpeta raíz del sitio web

Puede utilizar cualquier herramienta FTP para copiar archivos en el sitio web. [FileZilla](http://filezilla-project.org/) es una herramienta popular de FTP y se muestra en las capturas de pantalla.

Para usar una herramienta FTP, necesita tres cosas: la dirección URL de FTP, el nombre de usuario y la contraseña.

La dirección URL se muestra en la página del panel del sitio web en el Portal de administración de Azure, y el nombre de usuario y la contraseña de FTP pueden encontrarse en el *.publishsettings* archivo que ha descargado anteriormente. Los pasos siguientes muestran cómo obtener estos valores.

1. En el Portal de administración de Azure, haga clic en **sitios Web** ficha y, a continuación, haga clic en el sitio web de ensayo.
2. En el **panel** página, desplácese hacia abajo, busque el nombre de host FTP en el **vista rápida** sección.

    ![Nombre de host FTP](deploying-a-code-update/_static/image5.png)
3. Abra el almacenamiento provisional *.publishsettings* archivo en el Bloc de notas u otro editor de texto.
4. Buscar el `publishProfile` (elemento) para el perfil de FTP.
5. Copia la `userName` y `userPWD` valores.

    ![Credenciales FTP](deploying-a-code-update/_static/image6.png)
6. Abra la herramienta FTP e inicie sesión en la dirección URL de FTP.
7. Copia *aplicación\_offline.htm* desde la carpeta de soluciones para la */sitio/wwwroot* carpeta en el sitio de ensayo.

    ![Copie app_offline](deploying-a-code-update/_static/image7.png)
8. Vaya a la dirección URL de su sitio de ensayo. Verá que la *aplicación\_offline.htm* página aparece ahora en lugar de la página principal.

    ![App_offline.htm en la ventana del explorador](deploying-a-code-update/_static/image8.png)

Ahora está listo para implementar en almacenamiento provisional.

## <a name="deploy-the-code-update-to-staging-and-production"></a>Implementar la actualización del código de ensayo y producción

1. En el **Web uno haga clic en publicar** barra de herramientas, elija la **ensayo** perfil de publicación y, a continuación, haga clic en **Publicar Web**.

    Visual Studio implementa la aplicación actualizada y abre el explorador a la página principal del sitio. El *aplicación\_offline.htm* se muestra el archivo. Para poder probar para comprobar una implementación correcta, debe quitar la *aplicación\_offline.htm* archivo.
2. Volver a la herramienta FTP y eliminar **aplicación\_offline.htm** desde el sitio de ensayo.
3. En el explorador, abra la página de instructores en el sitio de ensayo y seleccione un instructor para comprobar que la actualización se ha implementado correctamente.
4. Siga el mismo procedimiento para la producción como hizo el almacenamiento provisional.

<a id="specificfiles"></a>

## <a name="reviewing-changes-and-deploying-specific-files"></a>Revisar los cambios e implementar archivos específicos

Visual Studio 2012 también ofrece la posibilidad de implementar los archivos individuales. Para un archivo seleccionado puede ver las diferencias entre la versión local y la versión implementada, implemente el archivo en el entorno de destino o copie el archivo desde el entorno de destino en el proyecto local. En esta sección del tutorial, verá cómo usar estas características.

### <a name="make-a-change-to-deploy"></a>Realiza un cambio para implementar

1. Abra *Content/Site.css*y busque el bloque para el `body` etiqueta.
2. Cambie el valor de `background-color` de `#fff` a `darkblue`.

    [!code-css[Main](deploying-a-code-update/samples/sample4.css?highlight=2)]

### <a name="view-the-change-in-the-publish-preview-window"></a>Ver el cambio en la ventana de vista previa de publicación

Cuando se usa el **Publicar Web** Asistente para publicar el proyecto, puede ver qué cambios que se van a publicar haciendo doble clic en el archivo en el **vista previa** ventana.

1. Haga clic en el proyecto ContosoUniversity y haga clic en **publicar**.
2. Desde el **perfil** lista desplegable, seleccione la **prueba** perfil de publicación.
3. Haga clic en **vista previa**y, a continuación, haga clic en **iniciar Preview**.
4. En el **vista previa** panel, haga doble clic en **Site.css**.

    ![Haga doble clic en Site.css](deploying-a-code-update/_static/image9.png)

    El **obtener una vista previa de cambios** cuadro de diálogo muestra una vista previa de los cambios que se va a implementar.

    ![Vista previa de cambios para Site.css](deploying-a-code-update/_static/image10.png)

    Si hace doble clic en el *Web.config* archivo, el **obtener una vista previa de cambios** cuadro de diálogo muestra el efecto de la compilación transformaciones de la configuración y las transformaciones de perfil de publicación. En este momento no lo haga todo lo que provocaría que el *Web.config* archivo en el servidor para cambiar, por lo que espera no ver ningún cambio. Sin embargo, el **obtener una vista previa de cambios** ventana muestra dos cambios de forma incorrecta. Parece que dos elementos XML que se va a quitar. Estos elementos se agregan mediante el proceso de publicación cuando se selecciona **ejecutar migraciones de Code First al iniciarse la aplicación** para una clase de contexto de Code First. La comparación se realiza antes de que el proceso de publicación agrega los elementos, por lo que parece se quitan aunque no se quitarán. Este error se corregirá en una versión futura.
5. Haga clic en **Cerrar**.
6. Haga clic en **Publicar**.
7. Cuando el explorador se abre en la página principal del sitio de prueba, presione CTRL + F5 para hacer que la actualización de una disco duro con el fin de ver el efecto del cambio CSS.

    ![Efecto del cambio de CSS](deploying-a-code-update/_static/image11.png)
8. Cierre el explorador.

### <a name="publish-specific-files-from-solution-explorer"></a>Publicar archivos específicos desde el Explorador de soluciones

Supongamos que no igual que el fondo azul y desea revertir al color original. En esta sección, podrá restaurar la configuración original mediante la publicación de un archivo específico directamente desde **el Explorador de soluciones**.

1. Abra *Content/Site.css* y restaurar la `background-color` si se establece en `#fff`.
2. En **el Explorador de soluciones**, haga clic en el *Content/Site.css* archivo.

    El menú contextual muestra que tres opciones de publicación.

    ![Publicar opciones desde el Explorador de soluciones](deploying-a-code-update/_static/image12.png)
3. Haga clic en **vista previa cambia a Site.css**.

    Abre una ventana para mostrar las diferencias entre el archivo local y la versión del mismo en el entorno de destino.

    ![Diferencias-contenido/Site.css](deploying-a-code-update/_static/image13.png)
4. En **el Explorador de soluciones**, haga clic en **Site.css** nuevo y haga clic en **publicar Site.css**.

    El **Web publicar actividad** ventana muestra que se ha publicado el archivo.

    ![Ventana de actividad Publicar Web](deploying-a-code-update/_static/image14.png)
5. Abra un explorador en la `http://localhost/contosouniversity` dirección URL y a continuación, presione CTRL + F5 para hacer que un disco duro actualizar para ver el efecto de las reglas CSS cambiar.

    ![Página de inicio con CSS normal](deploying-a-code-update/_static/image15.png)
6. Cierre el explorador.

## <a name="summary"></a>Resumen

Ahora que ha visto varias maneras de implementar una actualización de la aplicación que implican un cambio de base de datos y ha visto cómo obtener una vista previa de los cambios para comprobar que lo que se actualizará es lo esperado. La página de instructores tiene ahora un **cursos imparten** sección.

![Página de instructores con cursos impartida](deploying-a-code-update/_static/image16.png)

El siguiente tutorial muestra cómo implementar un cambio de base de datos: se agregará un campo de fecha de nacimiento a la base de datos y a la página de instructores.

>[!div class="step-by-step"]
[Anterior](deploying-to-production.md)
[Siguiente](deploying-a-database-update.md)
