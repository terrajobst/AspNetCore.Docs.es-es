---
uid: aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
title: 'Laboratorio práctico: fácil de mantener sitios Web de Azure: administración de cambios y escala | Microsoft Docs'
author: rick-anderson
description: En este laboratorio, obtenga información sobre cómo Microsoft Azure facilita a compilar e implementar sitios Web en producción.
ms.author: aspnetcontent
ms.date: 07/16/2014
ms.assetid: ecfd0eb4-c4ad-44e6-9db9-a2a66611ff6a
msc.legacyurl: /aspnet/overview/developing-apps-with-windows-azure/maintainable-azure-websites-managing-change-and-scale
msc.type: authoredcontent
ms.openlocfilehash: 3a118cdd7e3f3878976e4f8480ce2236b8d3ba88
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824801"
---
<a name="hands-on-lab-maintainable-azure-websites-managing-change-and-scale"></a>Laboratorio práctico: fácil de mantener sitios Web de Azure: administración de cambios y escala
====================
por [campamentos Web Team](https://twitter.com/webcamps)

[Descargue el Kit de aprendizaje de campamentos de Web](http://aka.ms/webcamps-training-kit)

> Microsoft Azure facilita la creación e implementación de sitios Web en producción. Pero no ha terminado cuando la aplicación está en funcionamiento, está empezando. Deberá controlar los cambiantes requisitos, las actualizaciones de la base de datos, escala y mucho más. Afortunadamente, Azure App Service le puede ayudar, con una gran cantidad de características para ayudarle a mantener sus sitios que se ejecutan sin problemas.
> 
> Azure ofrece desarrollo seguro y flexible, implementación y desarrollo para cualquier aplicación web de tamaño. Aproveche las herramientas para crear e implementar aplicaciones sin la complicación de administrar la infraestructura existentes.
> 
> Aprovisionar una aplicación web de producción por sí mismo en cuestión de minutos por implementar fácilmente el contenido creado con la herramienta de desarrollo favorita. Puede implementar un sitio existente directamente desde el control de código fuente con compatibilidad para **Git**, **GitHub**, **Bitbucket**, **TFS**y incluso **DropBox**. Implementar directamente desde su IDE favorito o desde scripts mediante **PowerShell** en Windows o **CLI** herramientas que se ejecutan en cualquier sistema operativo. Una vez implementado, mantenga sus sitios constantemente actualizados con compatibilidad para la implementación continua.
> 
> Azure proporciona almacenamiento en la nube escalables y duraderas, copia de seguridad y las soluciones de recuperación de datos, grandes o pequeños. Al implementar aplicaciones en un entorno de producción, servicios de almacenamiento, como tablas, Blobs y bases de datos SQL, ayudarle a escalar la aplicación en la nube.
> 
> Con las bases de datos de SQL, es importante mantener la base de datos productiva actualizada al implementar nuevas versiones de la aplicación. Gracias a **migraciones de Entity Framework Code First**, desarrollo e implementación del modelo de datos se ha simplificado para actualizar los entornos en cuestión de minutos. Este seminario práctico le mostrará los diferentes temas que podría producirse al implementar la aplicación web en entornos de producción en Microsoft Azure.
> 
> Todo el código de ejemplo y fragmentos de código se incluyen en el Kit de entrenamiento campamentos de Web, que está disponible en [ http://aka.ms/webcamps-training-kit ](http://aka.ms/webcamps-training-kit).
> 
> Para obtener más información detallada de este tema, consulte el [Building Real-World Cloud Apps with e-book de Azure](building-real-world-cloud-apps-with-windows-azure/introduction.md).


<a id="Overview"></a>
## <a name="overview"></a>Información general

<a id="Objectives"></a>
### <a name="objectives"></a>Objetivos

En este laboratorio práctico, aprenderá cómo:

- Habilitar migraciones de Entity Framework con un modelo existente
- Actualizar el modelo de objetos y la base de datos según corresponda mediante migraciones de Entity Framework
- Implementar en Azure App Service mediante Git
- Revertir a una implementación anterior mediante el portal de administración de Azure
- Usar el almacenamiento de Azure para escalar una aplicación web
- Configurar el escalado automático para una aplicación web mediante el Portal de administración de Azure
- Crear y configurar un proyecto de prueba de carga en Visual Studio

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>Requisitos previos

El siguiente es necesario para completar este laboratorio práctico:

- [Visual Studio Express 2013 para Web](https://www.microsoft.com/visualstudio/) o superior
- [Azure SDK para .NET 2.2](https://www.microsoft.com/windowsazure/sdk/)
- [Sistema de Control de versiones GIT](http://git-scm.com/download)
- Una suscripción de Microsoft Azure 

    - Registrarse para obtener un [evaluación gratuita](http://aka.ms/watk-freetrial)
    - Si está un Visual Studio Professional, Test Professional, Premium o Ultimate con MSDN o plataformas de MSDN, activar su [las ventajas de MSDN](http://aka.ms/watk-msdn) ahora para empezar a desarrollar y probar en Azure
    - [BizSpark](http://aka.ms/watk-bizspark) los miembros reciben automáticamente Azure beneficio a través de sus de Visual Studio Ultimate con suscripciones a MSDN
    - Los miembros de la [Microsoft Partner Network](http://aka.ms/watk-mpn) programa Cloud Essentials reciba créditos mensuales de Azure sin cargo

<a id="Setup"></a>
### <a name="setup"></a>Programa de instalación

Para poder ejecutar los ejercicios en este laboratorio práctico, deberá configurar primero el entorno.

1. Abra el Explorador de Windows y vaya a la práctica **origen** carpeta.
2. Haga doble clic en **Setup.cmd** y seleccione **ejecutar como administrador** para iniciar el proceso de instalación que se configure su entorno e instalará los fragmentos de código de Visual Studio para este laboratorio.
3. Si se muestra el cuadro de diálogo Control de cuentas de usuario, confirme la acción para continuar.

> [!NOTE]
> Asegúrese de que ha comprobado todas las dependencias para este laboratorio antes de ejecutar el programa de instalación.


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>Uso de los fragmentos de código

En todo el documento de laboratorio, se le pedirá que inserte los bloques de código. Para su comodidad, la mayoría de este código se proporciona como fragmentos de código de Visual Studio, puede acceder desde dentro de Visual Studio 2013 para evitar tener que agregarla manualmente.

> [!NOTE]
> Cada ejercicio viene acompañado por una solución inicial ubicada en el **comenzar** carpeta del ejercicio que le permite seguir cada ejercicio independientemente de los demás. Ten en cuenta que los fragmentos de código que se agregan durante un ejercicio faltan en estos a partir de las soluciones y es posible que no funcione hasta que haya completado el ejercicio. En el código fuente para un ejercicio, también encontrará un **final** carpeta que contiene una solución de Visual Studio con el código que se obtiene al completar los pasos descritos en el ejercicio correspondiente. Puede usar estas soluciones como instrucciones si necesita más ayuda mientras se trabaja a través de este laboratorio práctico.


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>Ejercicios

Este laboratorio práctico incluye los ejercicios siguientes:

1. [Uso de migraciones de Entity Framework](#Exercise1)
2. [Implementar una aplicación Web en el almacenamiento provisional](#Exercise2)
3. [Realizar reversión de la implementación en producción](#Exercise3)
4. [Escalado con almacenamiento de Azure](#Exercise4)
5. [Usar el escalado automático para las aplicaciones Web](#Exercise5) (opcional para Visual Studio 2013 Ultimate edition)

Tiempo estimado para completar esta práctica: **75 minutos**

> [!NOTE]
> Primera vez que inicie Visual Studio, debe seleccionar una de las colecciones de configuraciones predefinidas. Cada colección predefinida está diseñado para que coincida con un estilo de desarrollo determinado y determina los diseños de ventana, comportamiento del editor, fragmentos de código de IntelliSense y opciones del cuadro de diálogo. Los procedimientos de este laboratorio describen las acciones necesarias para realizar una tarea concreta en Visual Studio cuando se usa el **configuración General de desarrollo** colección. Si elige una colección de configuraciones diferentes para el entorno de desarrollo, puede haber diferencias en los pasos que debe tener en cuenta.


<a id="Exercise1"></a>
### <a name="exercise-1-using-entity-framework-migrations"></a>Ejercicio 1: Usar migraciones de Entity Framework

Cuando está desarrollando una aplicación, puede cambiar el modelo de datos con el tiempo. Estos cambios podrían afectar a un modelo existente en la base de datos (si está creando una nueva versión) y es importante mantener actualizados para evitar errores de la base de datos.

Para simplificar el seguimiento de estos cambios en el modelo, **migraciones de Entity Framework Code First** automáticamente detecta los cambios al comparar los modelos con el esquema de base de datos y genera código específico para actualizar la base de datos creación de nuevos *versiones* de la base de datos.

Este ejercicio muestra cómo habilitar **migraciones** para su aplicación y cómo puede detectar y generar cambios para actualizar las bases de datos fácilmente.

<a id="Ex1Task1"></a>
#### <a name="task-1--enabling-migrations"></a>Tarea 1: habilitación de migraciones

En esta tarea, recorrerá los pasos necesarios para habilitar **migraciones de Entity Framework Code First** a la **"geek" Quiz** base de datos, cambiar el modelo y entender cómo estos cambios se reflejan en el base de datos.

1. Abra Visual Studio y abra el **GeekQuiz.sln** archivo de solución de **Source\Ex1 UsingEntityFrameworkMigrations\Begin**.
2. Compile la solución con el fin de descargar e instalar el **NuGet** dependencias del paquete. Para ello, haga clic en la solución y haga clic en **compilar solución** o presione **Ctrl + Mayús + B**.
3. Desde el **herramientas** menú en Visual Studio, seleccione **Administrador de paquetes de biblioteca**y, a continuación, haga clic en **Package Manager Console**.
4. En el **Package Manager Console**, escriba el siguiente comando y, a continuación, presione **ENTRAR**. Se creará una migración inicial según el modelo existente.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample1.ps1)]

    ![Habilitar migraciones](maintainable-azure-websites-managing-change-and-scale/_static/image1.png "habilitar migraciones")

    *Habilitación de migraciones*

    > [!NOTE]
    > Este comando agrega un **migraciones** carpeta al proyecto de prueba "geek" que contiene un archivo denominado **Configuration.cs**. El **configuración** clase le permite configurar el comportamiento de las migraciones para su contexto.
5. Con las migraciones habilitadas, deberá actualizar el **configuración** clase para rellenar la base de datos con los datos iniciales que **"geek" Quiz** requiere. En **migraciones**, reemplace el **Configuration.cs** archivo con el que se encuentra en la **Source\Assets** carpeta de este laboratorio.

    > [!NOTE]
    > Puesto que **migraciones** llamará el **inicialización** método con todas las actualizaciones de base de datos, deberá asegurarse de que los registros no se duplican en la base de datos. El **AddOrUpdate** método le ayudará a evitar datos duplicados.
6. Para agregar una migración inicial, escriba el siguiente comando y, a continuación, presione **ENTRAR**.

    > [!NOTE]
    > Asegúrese de que no hay ninguna base de datos denominada &quot;GeekQuizProd&quot; en la instancia de LocalDB.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample2.ps1)]

    ![Adición de migración del esquema base](maintainable-azure-websites-managing-change-and-scale/_static/image2.png "migración del esquema de base de adición")

    *Adición de migración de esquema de base*

    > [!NOTE]
    > **Add-Migration** aplicará la técnica scaffolding la siguiente migración según los cambios realizados en el modelo desde que se creó la última migración. En este caso, ya que es la primera migración del proyecto, agregará las secuencias de comandos para crear todas las tablas definidas en el **TriviaContext** clase.
7. Ejecutar la migración para actualizar la base de datos, ejecute el comando siguiente. Para este comando especificará el **detallado** marca para ver las instrucciones SQL que se aplica a la base de datos de destino.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample3.ps1)]

    ![Crear la base de datos inicial](maintainable-azure-websites-managing-change-and-scale/_static/image3.png "crear base de datos inicial")

    *Creación de base de datos inicial*

    > [!NOTE]
    > **Actualizar base de datos** pendiente las migraciones se aplicará a la base de datos. En este caso, creará la base de datos mediante la cadena de conexión definida en el archivo web.config.
8. Vaya a **vista** menu y open **Explorador de objetos de SQL Server**.

    ![Abrir en el Explorador de objetos SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image4.png "abierto en el Explorador de objetos SQL Server")

    *Abrir en el Explorador de objetos SQL Server*
9. En el **Explorador de objetos de SQL Server** ventana, conectarse a la instancia de LocalDB haciendo clic con el **SQL Server** nodo y seleccione **agregar SQL Server...**  opción.

    ![Adición de una instancia de SQL Server](maintainable-azure-websites-managing-change-and-scale/_static/image5.png "agregar una instancia de SQL Server")

    *Adición de una instancia de SQL Server en el Explorador de objetos de SQL Server*
10. Establecer el **nombre del servidor** a *(localdb) \v11.0* y dejar **Windows autenticación** como el modo de autenticación. Haga clic en **Connect** para continuar.

    ![Conectarse a LocalDB](maintainable-azure-websites-managing-change-and-scale/_static/image6.png "conectarse a LocalDB")

    *Conectarse a LocalDB*
11. Abra el **GeekQuizProd** de base de datos y expanda el **tablas** nodo. Como puede ver, el **Update-Database** comando genera todas las tablas definidas en el **TriviaContext** clase. Busque el **dbo. TriviaQuestions** de tabla y abra el nodo de columnas. En la siguiente tarea, agregará una nueva columna a esta tabla y actualizar la base de datos mediante **migraciones**.

    ![Las curiosidades preguntas columnas](maintainable-azure-websites-managing-change-and-scale/_static/image7.png "curiosidades preguntas de columnas")

    *Las curiosidades de preguntas de columnas*

<a id="Ex1Task2"></a>
#### <a name="task-2--updating-database-schema-using-migrations"></a>Tarea 2: actualizar esquema de base de datos mediante migraciones

En esta tarea, utilizará **migraciones de Entity Framework Code First** para detectar un cambio en el modelo y generar el código necesario para actualizar la base de datos. Se actualizará la **TriviaQuestions** entidad agregando una nueva propiedad a él. A continuación, se ejecutará los comandos para crear una nueva migración para incluir la nueva columna en la tabla.

1. En **el Explorador de soluciones**, haga doble clic en el **TriviaQuestion.cs** archivo se encuentra dentro de la **modelos** carpeta.
2. Agregar una nueva propiedad denominada **sugerencia**, tal y como se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample4.cs)]
3. En el **Package Manager Console**, escriba el siguiente comando y, a continuación, presione **ENTRAR**. Se creará una nueva migración que refleja el cambio en nuestro modelo.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample5.ps1)]

    ![Add-Migration](maintainable-azure-websites-managing-change-and-scale/_static/image8.png "Add-Migration")

    *Add-Migration*

    > [!NOTE]
    > Un archivo de migración se compone de dos métodos, **seguridad** y **abajo**.
    > 
    > - El **seguridad** método se usará para especificar qué cambios de la versión actual de nuestra necesidad de aplicación se aplican a la base de datos.
    > - El **abajo** se usa para revertir los cambios que hemos agregado a la **seguridad** método.
    > 
    > Cuando la migración de base de datos se actualiza la base de datos, ejecutará todas las migraciones en el orden de marca de tiempo y solo aquellos que no se han utilizado desde la última actualización (el \_tabla MigrationHistory realiza un seguimiento de las migraciones que se han aplicado). El **seguridad** método de todas las migraciones se llamará y hará que los cambios que hemos especificado a la base de datos. Si se decide volver a una migración anterior, el **abajo** método se llamará para rehacer los cambios en un orden inverso.
4. En el **Package Manager Console**, escriba el siguiente comando y, a continuación, presione **ENTRAR**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample6.ps1)]
5. La salida de la **Update-Database** comando genera un **Alter Table** instrucción SQL para agregar una nueva columna a la **TriviaQuestions** de tabla, tal como se muestra en la imagen siguiente.

    ![Agregue la instrucción SQL de columna generada](maintainable-azure-websites-managing-change-and-scale/_static/image9.png "agregar la instrucción SQL de columna generada")

    *Agregue la instrucción SQL de columna generada*
6. En **Explorador de objetos de SQL Server**, actualice el **dbo. TriviaQuestions** de tabla y compruebe que el nuevo **sugerencia** se muestra la columna.

    ![Que muestra la nueva columna sugerencia](maintainable-azure-websites-managing-change-and-scale/_static/image10.png "que muestra la nueva columna de sugerencia")

    *Que muestra la nueva columna de sugerencia*
7. En el **TriviaQuestion.cs** editor, agregue un **StringLength** restricción a la *sugerencia* propiedad, como se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample7.cs)]
8. En el **Package Manager Console**, escriba el siguiente comando y, a continuación, presione **ENTRAR**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample8.ps1)]
9. En el **Package Manager Console**, escriba el siguiente comando y, a continuación, presione **ENTRAR**.

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample9.ps1)]
10. La salida de la **Update-Database** comando genera un **Alter Table** instrucción SQL para actualizar el *sugerencia* tipo de columna de la **TriviaQuestions** de tabla, tal como se muestra en la imagen siguiente.

    ![Modificar la instrucción SQL de columna generada](maintainable-azure-websites-managing-change-and-scale/_static/image11.png "modificar instrucción de columna SQL generado")

    *Modificar la instrucción SQL de columna generada*
11. En **Explorador de objetos de SQL Server**, actualice el **dbo. TriviaQuestions** de tabla y compruebe que la **sugerencia** es de tipo de columna **nvarchar(150)**.

    ![Que muestra la nueva restricción](maintainable-azure-websites-managing-change-and-scale/_static/image12.png "que muestra la nueva restricción")

    *Que muestra la nueva restricción*

<a id="Exercise2"></a>
### <a name="exercise-2-deploying-a-web-app-to-staging"></a>Ejercicio 2: Implementar una aplicación Web en el almacenamiento provisional

**Web Apps en Azure App Service** le permite realizar la publicación de ensayo. Publicación de ensayo crea una ranura de sitio de ensayo para cada sitio de producción predeterminado y le permite intercambiar estas ranuras sin tiempo de inactividad. Esto es realmente útil para validar los cambios antes de liberar al público, incrementalmente integrar el contenido del sitio y se revierte aunque los cambios no funcionan según lo previsto.

En este ejercicio, implementará el **"geek" Quiz** aplicación al entorno de ensayo de la aplicación web mediante el control de código fuente de Git. Para ello, creará la aplicación web y aprovisionar los componentes necesarios en el portal de administración, configure un **Git** repositorio e inserte la aplicación de código fuente desde el equipo local para la ranura de ensayo. También se actualizará la base de datos de producción con el **migraciones de Code First** que creó en el ejercicio anterior. A continuación, se ejecutará la aplicación en este entorno de prueba para comprobar su funcionamiento. Cuando esté satisfecho con su funcionamiento según sus expectativas, se promoverá la aplicación en producción.

> [!NOTE]
> Para habilitar la publicación de ensayo, la aplicación web debe estar en **modo estándar**. Tenga en cuenta que si cambia la aplicación web al modo estándar se incurrirá en cargos adicionales. Para obtener más información sobre los precios, consulte [precios de App Service](https://azure.microsoft.com/pricing/details/app-service/).


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-web-app-in-azure-app-service"></a>Tarea 1: crear una aplicación Web en Azure App Service

En esta tarea, creará una aplicación web en **Azure App Service** desde el portal de administración. También configurará un **base de datos SQL** para conservar los datos de aplicación y configurar un repositorio de Git local para el control de código fuente.

1. Vaya a la [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con la cuenta Microsoft asociada con su suscripción.

    ![Inicie sesión en el portal de administración de Azure](maintainable-azure-websites-managing-change-and-scale/_static/image13.png)

    *Inicie sesión en el portal de administración de Azure*
2. Haga clic en **New** en la barra de comandos en la parte inferior de la página.

    ![Crear una nueva aplicación web](maintainable-azure-websites-managing-change-and-scale/_static/image14.png "crear una nueva aplicación web")

    *Crear una nueva aplicación web*
3. Haga clic en **proceso**, **sitio Web** y, a continuación, **creación personalizada**.

    ![Crear una nueva aplicación web mediante la creación personalizada](maintainable-azure-websites-managing-change-and-scale/_static/image15.png "crear una nueva aplicación web mediante la creación personalizada")

    *Crear una nueva aplicación web mediante la creación personalizada*
4. En el **nuevo sitio Web - Creación personalizada** diálogo cuadro, proporcione un disponibles **URL** (p. ej. *"geek" cuestionario*), seleccione una ubicación en la **región** lista desplegable y seleccione **crear una nueva base de datos SQL** en el **base de datos** lista desplegable. Por último, seleccione el **publicar desde control de código fuente** casilla de verificación y haga clic en **siguiente**.

    ![Personalización de la nueva aplicación web](maintainable-azure-websites-managing-change-and-scale/_static/image16.png)

    *Personalización de la nueva aplicación web*
5. Especifique la siguiente información para la configuración de la base de datos:

   - En el **nombre** texto, escriba un nombre de base de datos (por ejemplo, *geekquiz\_db*)
   - En el servidor **desplegable** lista, seleccione **servidor de base de datos SQL nueva**. Como alternativa, puede seleccionar un servidor existente.
   - En el **nombre de usuario de base de datos** y **contraseña de la base de datos** cuadros, escriba el nombre de usuario administrador y la contraseña para el servidor de base de datos SQL. Si selecciona un servidor ya ha creado, se le pedirá la contraseña.

     ![Especificar la configuración de la base de datos](maintainable-azure-websites-managing-change-and-scale/_static/image17.png)

     *Especificar la configuración de la base de datos*
6. Haga clic en **Siguiente** para continuar.
7. Seleccione **repositorio de Git Local** para usar y haga clic en el control de código fuente **siguiente**.

    > [!NOTE]
    > Se le pedirá las credenciales de implementación (un nombre de usuario y contraseña).

    ![Crear el repositorio de Git](maintainable-azure-websites-managing-change-and-scale/_static/image18.png)

    *Crear el repositorio de Git*
8. Espere hasta que se crea la nueva aplicación web.

    > [!NOTE]
    > De forma predeterminada, Azure proporciona dominios en *azurewebsites.net* sino que también ofrece la posibilidad de establecer dominios personalizados mediante el portal de administración de Azure. Sin embargo, solo puede administrar dominios personalizados si usas ciertos modos de Azure App Service.
    > 
    > Azure App Service está disponible en las ediciones gratis, compartido, básico, estándar y Premium. En modo de gratis y compartido, todas las aplicaciones web se ejecutan en un entorno de varios inquilinos y tienen cuotas de uso de CPU, memoria y red. El número máximo de aplicaciones gratuitas puede variar con el plan. En el modo estándar, elija los recursos de proceso que las aplicaciones que se ejecutan en máquinas virtuales dedicadas que corresponden al estándar de Azure. Puede encontrar la configuración del modo de aplicación web en el **escala** menú de la aplicación web.
    > 
    > ![Modos de Azure App Service](maintainable-azure-websites-managing-change-and-scale/_static/image19.png "modos de Azure App Service")
    > 
    > Si usas **Shared** o **estándar** modo, podrá administrar dominios personalizados para la aplicación web, vaya a la aplicación **configurar** menú y haga clic en **Administrar dominios** en *nombres de dominio*.
    > 
    > ![Administrar dominios](maintainable-azure-websites-managing-change-and-scale/_static/image20.png "administrar dominios")
    > 
    > ![Administrar dominios personalizados](maintainable-azure-websites-managing-change-and-scale/_static/image21.png "administrar dominios personalizados")
9. Una vez creada la aplicación web, haga clic en el vínculo situado bajo el **URL** columna para comprobar que la nueva aplicación web se está ejecutando.

    ![Vaya a la nueva aplicación web](maintainable-azure-websites-managing-change-and-scale/_static/image22.png)

    *Vaya a la nueva aplicación web*

    ![aplicación Web que se ejecuta](maintainable-azure-websites-managing-change-and-scale/_static/image23.png)

    *aplicación Web que se ejecuta*

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-the-production-sql-database"></a>Tarea 2: creación de la base de datos SQL de producción

En esta tarea, utilizará el **migraciones de Entity Framework Code First** para crear la base de datos como destino el **Azure SQL Database** instancia que ha creado en la tarea anterior.

1. En el Portal de administración, vaya a la aplicación web que creó en la tarea anterior y vaya a su **panel**.
2. En el **panel** página, haga clic en **ver cadenas de conexión** vincular bajo el **vista rápida** sección.

    ![Ver cadenas de conexión](maintainable-azure-websites-managing-change-and-scale/_static/image24.png "ver cadenas de conexión")

    *Ver cadenas de conexión*
3. Copia el **cadena de conexión** valor y cerrar el cuadro de diálogo.

    ![Cadena de conexión en el Portal de administración de Azure](maintainable-azure-websites-managing-change-and-scale/_static/image25.png "cadena de conexión en el Portal de administración de Azure")

    *Cadena de conexión en el Portal de administración de Azure*
4. Haga clic en **bases de datos SQL** para ver la lista de bases de datos SQL Azure

    ![Menú de la base de datos SQL](maintainable-azure-websites-managing-change-and-scale/_static/image26.png "menú de la base de datos SQL")

    *Menú de la base de datos SQL*
5. Busque la base de datos que creó en la tarea anterior y haga clic en el servidor.

    ![Servidor de base de datos SQL](maintainable-azure-websites-managing-change-and-scale/_static/image27.png "el servidor de base de datos SQL")

    *Servidor de base de datos SQL*
6. En el **inicio rápido** página del servidor, haga clic en **configurar**.

    ![Menú Configurar](maintainable-azure-websites-managing-change-and-scale/_static/image28.png "menú Configurar")

    *Configurar el menú*
7. En el **direcciones IP permitidas** sección, haga clic en **agregar a las direcciones IP permitidas** vínculo para habilitar la dirección IP para conectarse al servidor de base de datos SQL.

    ![Direcciones IP permitidas](maintainable-azure-websites-managing-change-and-scale/_static/image29.png "direcciones IP permitidas")

    *Direcciones IP permitidas*
8. Haga clic en **guardar** en la parte inferior de la página para completar el paso.
9. Cambie a Visual Studio.
10. En el **Package Manager Console**, ejecute el comando siguiente reemplazando *[YOUR-CONNECTION-STRING]* marcador de posición con la cadena de conexión que copió de Azure

    [!code-powershell[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample10.ps1)]

    ![Actualizar base de datos destinadas a Windows Azure SQL Database](maintainable-azure-websites-managing-change-and-scale/_static/image30.png "Actualizar base de datos destinadas a Windows Azure SQL Database")

    *Actualizar base de datos destinadas a Azure SQL Database*

<a id="Ex2Task3"></a>
#### <a name="task-3--deploying-geek-quiz-to-staging-using-git"></a>Tarea 3: implementación cuestionario "geek" en el almacenamiento provisional mediante Git

En esta tarea, habilitará la publicación de ensayo en su aplicación web. A continuación, usará Git para publicar la aplicación "geek" prueba directamente desde su equipo local en el entorno de ensayo de la aplicación web.

1. Vuelva al portal y haga clic en el nombre de la aplicación web en el **nombre** columna para mostrar las páginas de administración.

    ![Abrir las páginas de administración de aplicaciones web](maintainable-azure-websites-managing-change-and-scale/_static/image31.png)

    *Abrir las páginas de administración de aplicaciones web*
2. Navegue hasta la **escala** página. En el **general** sección, seleccione **estándar** para la configuración y haga clic en **guardar** en la barra de comandos.

    > [!NOTE]
    > Para ejecutar todas las aplicaciones web en la región actual y la suscripción en **estándar** modo, deje el **seleccionar todo** casilla seleccionada en el **elegir sitios** configuración. De lo contrario, desactive el **seleccionar todo** casilla de verificación.

    ![Actualizar la aplicación web al modo estándar](maintainable-azure-websites-managing-change-and-scale/_static/image32.png "actualizar la aplicación web al modo estándar")

    *Actualizar la aplicación Web al modo estándar*
3. Haga clic en **Sí** para confirmar los cambios.

    ![Confirmar el cambio al modo estándar](maintainable-azure-websites-managing-change-and-scale/_static/image33.png "continuar con los cambios en el modo de aplicación web")

    *Confirmar el cambio al modo estándar*
4. Vaya a la **panel** página y haga clic en **habilitar la publicación de ensayo** bajo el **vista rápida** sección.

    ![Habilitar la publicación de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image34.png "habilitar la publicación de ensayo")

    *Habilitar la publicación de ensayo*
5. Haga clic en **Sí** para habilitar la publicación de ensayo.

    ![Confirmar publicación de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image35.png "al hacer clic en Sí para habilitar la publicación de ensayo")

    *Confirmar publicación de ensayo*
6. En la lista de aplicaciones web, expanda la marca a la izquierda de su nombre de la aplicación web para mostrar la ranura de sitio de ensayo. Tiene el nombre de la aplicación web, seguido por ***(ensayo)***. Haga clic en el sitio de ensayo para ir a la página de administración.

    ![Vaya a la aplicación web de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image36.png "navegar a la aplicación web de ensayo")

    *Vaya a la aplicación de ensayo*
7. Tenga en cuenta que esa página de administración aspecto de la página de cualquier otra aplicación web administración. Navegue hasta la **implementaciones** página y copie el **dirección URL de Git** valor. La usará más adelante en este ejercicio.

    ![Copiar el valor de dirección URL de Git](maintainable-azure-websites-managing-change-and-scale/_static/image37.png)

    *Copiar el valor de dirección URL de Git*
8. Abra una nueva **Git Bash** consola y ejecute los comandos siguientes. Actualización el *[YOUR-APPLICATION-PATH]* marcador de posición con la ruta de acceso a la **GeekQuiz** solución, que se encuentra en la **Source\Ex1 DeployingWebSiteToStaging\Begin** carpeta de Este laboratorio.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample11.cmd)]

    ![Inicialización de GIT y la primera confirmación](maintainable-azure-websites-managing-change-and-scale/_static/image38.png)

    *Inicialización de GIT y la primera confirmación*
9. Ejecute el siguiente comando para insertar la aplicación web en el servidor remoto **Git** repositorio. Reemplace el marcador de posición por la dirección URL que obtuvo en el portal de administración. Se le pedirá la contraseña de la implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample12.cmd)]

    ![Inserción en Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image39.png)

    *Inserción en Azure*

    > [!NOTE]
    > Al implementar contenido en el host FTP o el repositorio de GIT de una aplicación web, debe autenticarse mediante el **credenciales de implementación** que creó a partir de la aplicación web **inicio rápido** o **panel**  páginas de administración. Si no conoce las credenciales de implementación puede restablecer fácilmente mediante el portal de administración. Abra la aplicación web **panel** página y haga clic en el **restablezca sus credenciales de implementación** vínculo. Proporcione una contraseña nueva y haga clic en **Aceptar**. Credenciales de implementación son válidas para su uso con todas las aplicaciones web asociadas con la suscripción.
10. Para comprobar que la aplicación web se ha insertado correctamente en Azure, vuelva al portal de administración y haga clic en **sitios Web**.
11. Busque la aplicación web y expanda la entrada para mostrar la ranura de sitio de ensayo. Haga clic en su **nombre** para ir a la página de administración.
12. Haga clic en **implementaciones** para ver el **historial de implementación**. Compruebe que hay un **implementación activa** con su  *&quot;confirmación inicial&quot;*.

    ![Implementación activa](maintainable-azure-websites-managing-change-and-scale/_static/image40.png)

    *Implementación activa*
13. Por último, haga clic en **examinar** en la barra de comandos para ir a la aplicación web.

    ![Examen de la aplicación web](maintainable-azure-websites-managing-change-and-scale/_static/image41.png)

    *Examen de la aplicación web*
14. Si la aplicación se implementa correctamente, verá la página de inicio de sesión "geek" Quiz.

    > [!NOTE]
    > La dirección URL de la aplicación implementada contiene el nombre de la aplicación web, seguido por *-ensayo*.

    ![Aplicación que se ejecuta en el entorno de ensayo](maintainable-azure-websites-managing-change-and-scale/_static/image42.png)

    *Aplicación que se ejecuta en el entorno de ensayo*
15. Si desea explorar la aplicación, haga clic en **registrar** para registrar un nuevo usuario. Complete los detalles de la cuenta, escriba un nombre de usuario, dirección de correo electrónico y una contraseña. A continuación, la aplicación muestra la primera pregunta de la prueba. Responda a algunas preguntas para asegurarse de que funciona según lo previsto.

    ![Aplicación lista para usarse](maintainable-azure-websites-managing-change-and-scale/_static/image43.png)

    *Aplicación lista para usarse*

<a id="Ex2Task4"></a>
#### <a name="task-4--promoting-the-web-app-to-production"></a>Tarea 4: promoción de la aplicación Web en producción

Ahora que ha comprobado que la aplicación web funciona correctamente en el entorno de ensayo, está listo para pasar a producción. En esta tarea, se intercambiará la ranura de sitio de ensayo con la ranura de sitio de producción.

1. Vuelva al portal de administración y seleccione la ranura de sitio de ensayo. Haga clic en **intercambiar** en la barra de comandos.

    ![Pase a producción](maintainable-azure-websites-managing-change-and-scale/_static/image44.png)

    *Pase a producción*
2. Haga clic en **Sí** en el cuadro de diálogo de confirmación para continuar con la operación de intercambio. Azure inmediatamente intercambiará el contenido del sitio de producción con el contenido del sitio de ensayo.

    > [!NOTE]
    > Algunas opciones de configuración de la versión de ensayo se copiarán automáticamente en la versión de producción (por ejemplo, cadena de conexión invalidaciones, asignaciones de controlador, etc.), pero otros valores de configuración no cambiará (por ejemplo, puntos de conexión DNS, los enlaces SSL, etcetera).

    ![Confirmar operación de intercambio](maintainable-azure-websites-managing-change-and-scale/_static/image45.png)

    *Confirmar operación de intercambio*
3. Una vez completado el intercambio, seleccione la ranura de producción y haga clic en **examinar** en la barra de comandos para abrir el sitio de producción. Tenga en cuenta la dirección URL en la barra de direcciones.

    > [!NOTE]
    > Necesita actualizar el explorador para borrar la memoria caché. En Internet Explorer, puede hacer esto presionando **CTRL+R**.

    ![Aplicación Web que se ejecutan en el entorno de producción](maintainable-azure-websites-managing-change-and-scale/_static/image46.png)
4. En el **GitBash** de consola, actualice la dirección URL remota para el repositorio de Git local para la ranura de producción de destino. Para ello, ejecute el comando siguiente, reemplazando los marcadores de posición por su nombre de usuario de implementación y el nombre de la aplicación web.

    > [!NOTE]
    > En los siguientes ejercicios, insertará cambios en el sitio de producción en lugar de ensayo solo por motivos de simplicidad del laboratorio. En un escenario real, se recomienda para comprobar los cambios en el entorno de ensayo antes de promover a producción.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample13.cmd)]

<a id="Exercise3"></a>
### <a name="exercise-3-performing-deployment-rollback-in-production"></a>Ejercicio 3: Realizar la reversión de la implementación en producción

Hay escenarios donde no haya una ranura de ensayo para realizar un intercambio directo entre ensayo y producción, por ejemplo, si está trabajando con **gratis** o **Shared** modo. En esos escenarios, debe probar la aplicación en un entorno de prueba: ya sea localmente o en un sitio remoto: antes de implementarlo en producción. Sin embargo, es posible que puedan surgir un problema no detectado durante la fase de pruebas en el sitio de producción. En este caso, es importante disponer de un mecanismo para pasar fácilmente a una versión anterior y más estable de la aplicación tan pronto como sea posible.

En **Azure App Service**, la implementación continua desde el control de código fuente hace que esta posible gracias a la **volver a implementar** acción disponible en el portal de administración. Azure realiza un seguimiento de las implementaciones asociadas con las confirmaciones que insertó en el repositorio y proporciona una opción para volver a implementar la aplicación utilizando cualquiera de las implementaciones anteriores, en cualquier momento.

En este ejercicio llevará a cabo un cambio en el código en el **"geek" Quiz** aplicación que inserta intencionadamente un *error*. Se implementará la aplicación en producción para ver el error y, a continuación, se aprovechará la característica de reimplementación para volver al estado anterior.

<a id="Ex3Task1"></a>
#### <a name="task-1--updating-the-geek-quiz-application"></a>Tarea 1: actualizar la aplicación de prueba "geek"

En esta tarea, va a refactorizar un pequeño fragmento de código de la **TriviaController** clase para extraer la parte de la lógica que recupera la opción de prueba seleccionado de la base de datos en un nuevo método.

1. Cambie a la instancia de Visual Studio con el **GeekQuiz** solución del ejercicio anterior.
2. En **el Explorador de soluciones**, abra el **TriviaController.cs** archivo dentro de la **controladores** carpeta.
3. Busque el **StoreAsync** método y seleccione el código resaltado en la ilustración siguiente.

    ![Seleccione el código](maintainable-azure-websites-managing-change-and-scale/_static/image47.png)

    *Seleccione el código*
4. Haga clic en el código seleccionado, expanda el **refactorizar** menú y seleccione **Extraer método...** .

    ![Extraer el código como un nuevo método](maintainable-azure-websites-managing-change-and-scale/_static/image48.png)

    *Seleccionar método de extracción*
5. En el **Extraer método** cuadro de diálogo, el nombre del nuevo método *MatchesOption* y haga clic en **Aceptar**.

    ![Especifica el nombre del método](maintainable-azure-websites-managing-change-and-scale/_static/image49.png)

    *Especifique el nombre para el método extraído*
6. A continuación, se extrae el código seleccionado en el **MatchesOption** método. El código resultante se muestra en el siguiente fragmento de código.

    [!code-csharp[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample14.cs)]
7. Presione **CTRL + S** para guardar los cambios.

<a id="Ex3Task2"></a>
#### <a name="task-2--redeploying-the-geek-quiz-application"></a>Tarea 2: volver a implementar la aplicación de prueba "geek"

Ahora insertará los cambios realizados en la tarea anterior en el repositorio, lo que desencadenará una nueva implementación en el entorno de producción. A continuación, que se soluciona un problema usando el **herramientas de desarrollo F12** proporcionada por Internet Explorer y, a continuación, realizar una reversión a la implementación anterior desde el portal de administración de Azure.

1. Abra una nueva **Git Bash** consola para implementar la aplicación actualizada en Azure App Service.
2. Ejecute los siguientes comandos para insertar los cambios en Azure. Actualización de la *[YOUR-APPLICATION-PATH]* marcador de posición con la ruta de acceso a la **GeekQuiz** solución. Se le pedirá la contraseña de la implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample15.cmd)]

    ![Inserción de código refactorizado en Azure](maintainable-azure-websites-managing-change-and-scale/_static/image50.png)

    *Inserción de código refactorizado en Azure*
3. Abra Internet Explorer y vaya a la aplicación web (por ejemplo, `http://<your-web-site>.azurewebsites.net`). Inicie sesión con las credenciales creadas anteriormente.
4. Presione **F12** para iniciar las herramientas de desarrollo, seleccione el **red** ficha y haga clic en el **reproducir** botón para iniciar la grabación.

    ![Iniciar grabación red](maintainable-azure-websites-managing-change-and-scale/_static/image51.png "Iniciar grabación de red")

    *Iniciar grabación de red*
5. Seleccione cualquier opción de la prueba. Verá que no sucede nada.
6. En el **F12** ventana, muestra la entrada correspondiente a la solicitud HTTP POST HTTP **500** resultado.

    ![HTTP 500 error](maintainable-azure-websites-managing-change-and-scale/_static/image52.png)

    *HTTP 500 error*
7. Seleccione el **consola** ficha. Se registra un error con los detalles de la causa.

    ![Error registrados](maintainable-azure-websites-managing-change-and-scale/_static/image53.png)

    *Error registrados*
8. Busque la sección de detalles del error. Claramente, este error se produce porque el código de refactorización se confirman en los pasos anteriores.

    `Details: LINQ to Entities does not recognize the method 'Boolean MatchesOption ...`.
9. No cierre el explorador.
10. En una nueva instancia del explorador, navegue hasta la [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con la cuenta Microsoft asociada con su suscripción.
11. Seleccione **Websites** y haga clic en la aplicación web que creó en el ejercicio 2.
12. Navegue hasta la **implementaciones** página. Tenga en cuenta que se enumeran todas las confirmaciones realizadas en el historial de implementación.

    ![Lista de las implementaciones existentes](maintainable-azure-websites-managing-change-and-scale/_static/image54.png)

    *Lista de las implementaciones existentes*
13. Seleccione la confirmación anterior y haga clic en **volver a implementar** en la barra de comandos.

    ![Volver a implementar la confirmación anterior](maintainable-azure-websites-managing-change-and-scale/_static/image55.png)

    *Volver a implementar la confirmación anterior*
14. Cuando se le pida que confirme, haga clic en **Sí**.

    ![Confirmar la reimplementación](maintainable-azure-websites-managing-change-and-scale/_static/image56.png)
15. Cuando se completa la implementación, cambie a la instancia del explorador con la aplicación web y presione **CTRL + F5**.
16. Haga clic en cualquiera de las opciones. La animación flip ahora tendrá lugar y el resultado (*correcta o incorrecta*) se mostrarán.
17. (Opcional) Cambie a la **Git Bash** consola y ejecute los comandos siguientes para volver a la confirmación anterior.

    > [!NOTE]
    > Estos comandos crean una nueva confirmación que deshace todos los cambios en el repositorio de Git que se realizaron en la confirmación incorrecta. Azure, a continuación, volverá a implementar la aplicación con la nueva confirmación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample16.cmd)]

<a id="Exercise4"></a>
### <a name="exercise-4-scaling-using-azure-storage"></a>Ejercicio 4: Escalado con almacenamiento de Azure

**Los blobs** son la manera más sencilla de almacenar grandes cantidades de texto no estructurado o datos binarios tales como vídeo, audio e imágenes. Mover el contenido estático de la aplicación en el almacenamiento, ayuda a escalar su aplicación mediante el servicio de imágenes o documentos directamente en el explorador.

En este ejercicio, se moverá el contenido estático de la aplicación en un contenedor de blobs. A continuación, configurará la aplicación para agregar un **regla de reescritura de direcciones URL de ASP.NET** en el **Web.config** para redirigir el contenido en el contenedor de blobs.

<a id="Ex4Task1"></a>
#### <a name="task-1--creating-an-azure-storage-account"></a>Tarea 1: crear una cuenta de almacenamiento de Azure

En esta tarea obtendrá información sobre cómo crear una nueva cuenta de almacenamiento mediante el portal de administración.

1. Navegue hasta la [portal de administración de Azure](https://manage.windowsazure.com) e inicie sesión con la cuenta Microsoft asociada con su suscripción.
2. Seleccione **nuevo | Servicios de datos | Almacenamiento de información | Creación rápida** para comenzar a crear una nueva cuenta de almacenamiento. Escriba un nombre único para la cuenta y seleccione un **región** en la lista. Haga clic en **crear cuenta de almacenamiento** para continuar.

    ![Crear una nueva cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image57.png "crear una nueva cuenta de almacenamiento")

    *Crear una nueva cuenta de almacenamiento*
3. En el **almacenamiento** sección, espere a que el estado de la nueva cuenta de almacenamiento cambia a *Online* para poder continuar con el paso siguiente.

    ![Cuenta de almacenamiento creada](maintainable-azure-websites-managing-change-and-scale/_static/image58.png "cuenta de almacenamiento creada")

    *Cuenta de almacenamiento creada*
4. Haga clic en el nombre de cuenta de almacenamiento y, a continuación, haga clic en el **panel** vínculo en la parte superior de la página. El **panel** página le proporcionará información sobre el estado de la cuenta y los puntos de conexión de servicio que se pueden usar dentro de sus aplicaciones.

    ![Mostrar el panel de la cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image59.png "mostrar el panel de la cuenta de almacenamiento")

    *Mostrar el panel de la cuenta de almacenamiento*
5. Haga clic en el **administrar claves de acceso** botón en la barra de navegación.

    ![Botón de administrar las claves de acceso](maintainable-azure-websites-managing-change-and-scale/_static/image60.png "botón administrar claves de acceso")

    *Administrar claves de acceso*
6. En el **administrar claves de acceso** cuadro de diálogo, copie el **Nombredecuentadealmacenamiento** y **clave de acceso principal** ya que lo necesitará en el ejercicio siguiente. A continuación, cierre el cuadro de diálogo.

    ![Cuadro de diálogo Administrar clave de acceso](maintainable-azure-websites-managing-change-and-scale/_static/image61.png "cuadro de diálogo Administrar clave de acceso")

    *Administrar el cuadro de diálogo de la clave de acceso*

<a id="Ex4Task2"></a>
#### <a name="task-2--uploading-an-asset-to-azure-blob-storage"></a>Tarea 2: cargar un activo a Azure Blob Storage

En esta tarea, usará la ventana Explorador de servidores de Visual Studio para conectarse a su cuenta de almacenamiento. A continuación, creará un contenedor de blobs y cargar un archivo con el logotipo de cuestionarios "geek" en el contenedor.

1. Cambie a la instancia de Visual Studio con el **GeekQuiz** solución del ejercicio anterior.
2. En la barra de menús, seleccione **vista** y, a continuación, haga clic en **Explorador de servidores**.
3. En **Explorador de servidores**, haga clic en el **Azure** nodo y seleccione **conectar a Azure...** . Inicie sesión con la cuenta Microsoft asociada con su suscripción.

    ![Conectarse a Windows Azure](maintainable-azure-websites-managing-change-and-scale/_static/image62.png)

    *Conectarse a Azure*
4. Expanda el **Azure** nodo, haga clic en **almacenamiento** y seleccione **asociar almacenamiento externo...** .
5. En el **agregar una nueva cuenta de almacenamiento** diálogo cuadro, escriba el **nombreCuenta** y **clave de cuenta** que obtuvo en la tarea anterior y haga clic en **Aceptar**.

    ![Agregar cuadro de diálogo de nueva cuenta de almacenamiento](maintainable-azure-websites-managing-change-and-scale/_static/image63.png)

    *Agregar cuadro de diálogo de nueva cuenta de almacenamiento*
6. La cuenta de almacenamiento debería aparecer bajo el **almacenamiento** nodo. Expanda la cuenta de almacenamiento, haga clic en **Blobs** y seleccione **crear contenedor de blobs...** .

    ![Crear contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image64.png "crear contenedor de blobs")

    *Crear contenedor de blobs*
7. En el **crear contenedor de blobs** diálogo cuadro, escriba un nombre para el contenedor de blobs y haga clic en **Aceptar**.

    ![Cuadro de diálogo Crear contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image65.png "cuadro de diálogo Crear contenedor de blobs")

    *Crear el cuadro de diálogo contenedor de blobs*
8. El nuevo contenedor de blobs debe agregarse a la **Blobs** nodo. Cambiar los permisos de acceso en el contenedor para hacer público el contenedor. Para ello, haga clic en el **imágenes** contenedor y seleccione **propiedades**.

    ![las imágenes de las propiedades del contenedor](maintainable-azure-websites-managing-change-and-scale/_static/image66.png "las imágenes de las propiedades del contenedor")

    *Propiedades del contenedor de imágenes*
9. En el **propiedades** ventana, establezca el **acceso de lectura público** a **contenedor**.

    ![Cambio de propiedad de acceso de lectura público](maintainable-azure-websites-managing-change-and-scale/_static/image67.png "cambio de propiedad de acceso de lectura público")

    *Cambio de propiedad de acceso de lectura público*
10. Cuando se le pregunte si está seguro de que desea cambiar la propiedad de acceso público, haga clic en **Sí**.

    ![Advertencia de Microsoft Visual Studio](maintainable-azure-websites-managing-change-and-scale/_static/image68.png "advertencia de Microsoft Visual Studio")

    *Advertencia de Microsoft Visual Studio*
11. En **Explorador de servidores**, haga clic en el **imágenes** contenedor de blobs y seleccione **ver contenedor de Blob**.

    ![Ver contenedor de blobs](maintainable-azure-websites-managing-change-and-scale/_static/image69.png "ver contenedor de blobs")

    *Ver contenedor de blobs*
12. Debe abrir el contenedor de imágenes en una nueva ventana y se debe mostrar una leyenda sin entradas. Haga clic en el **cargar** icono para cargar un archivo en el contenedor de blobs.

    ![El contenedor de imágenes sin entradas](maintainable-azure-websites-managing-change-and-scale/_static/image70.png "el contenedor de imágenes sin entradas")

    *Contenedor de imágenes sin entradas*
13. En el **cargar Blob** diálogo cuadro, vaya a la **activos** carpeta del laboratorio. Seleccione el **logotipo big.png** de archivo y haga clic en **abierto**.
14. Espere hasta que se carga el archivo. Cuando se complete la carga, el archivo debe aparecer en el contenedor de imágenes. Haga clic en la entrada del archivo y seleccione **copiar la dirección URL**.

    ![Copie la dirección URL del blob](maintainable-azure-websites-managing-change-and-scale/_static/image71.png "Copiar dirección URL del archivo de blob")

    *Copie la dirección URL del blob*
15. Abra Internet Explorer y pegue la dirección URL. La siguiente imagen se debe mostrar en el explorador.

    ![imagen de logotipo big.png desde el almacenamiento de blobs de Windows](maintainable-azure-websites-managing-change-and-scale/_static/image72.png "big.png del logotipo de imagen del almacenamiento")

    *imagen de logotipo big.png desde Azure Blob Storage*

<a id="Ex4Task3"></a>
#### <a name="task-3--updating-the-solution-to-consume-static-content-from-azure-blob-storage"></a>Tarea 3: actualizar la solución para consumir contenido estático desde Azure Blob Storage

En esta tarea, configurará la **GeekQuiz** solución para consumir la imagen se carga en Azure Blob Storage (en lugar de la imagen que se encuentra en la aplicación web) mediante la adición de una regla de reescritura de direcciones URL de ASP.NET en el **web.config**archivo.

1. En Visual Studio, abra el **Web.config** archivo dentro de la **GeekQuiz** del proyecto y busque el **&lt;system.webServer&gt;** elemento.
2. Agregue el código siguiente para agregar una dirección URL de la regla de reescritura, actualizar el marcador de posición con el nombre de cuenta de almacenamiento.

    (Código de fragmento de código - *UrlRewriteRule WebSitesInProduction - Ex4 -*)

    [!code-xml[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample17.xml)]

    > [!NOTE]
    > Reescritura de URL es el proceso de interceptar una solicitud Web entrante y redireccionar la solicitud a un recurso diferente. La reglas de reescritura de URL indica al motor de reescritura cuando debe redirigir una solicitud y donde debe redirigirse. Una regla de reescritura se compone de dos cadenas: el patrón que se busca en la dirección URL solicitada (por lo general, uso de expresiones regulares), y la cadena para reemplazar el patrón, si se encuentra. Para obtener más información, consulte [la reescritura de URL en ASP.NET](https://msdn.microsoft.com/library/ms972974.aspx).
3. Presione **CTRL + S** para guardar los cambios.
4. Abra una nueva **Git Bash** consola para implementar la aplicación actualizada en Azure App Service.
5. Ejecute los siguientes comandos para insertar los cambios en Azure. Actualización de la *[YOUR-APPLICATION-PATH]* marcador de posición con la ruta de acceso a la **GeekQuiz** solución. Se le pedirá la contraseña de la implementación.

    [!code-console[Main](maintainable-azure-websites-managing-change-and-scale/samples/sample18.cmd)]

    ![Actualización de la implementación en Azure](maintainable-azure-websites-managing-change-and-scale/_static/image73.png)

    *Actualización de la implementación en Azure*

<a id="Ex4Task4"></a>
#### <a name="task-4--verification"></a>Tarea 4: verificación

En esta tarea usará **Internet Explorer** para examinar el **"geek" Quiz** aplicación y compruebe que la dirección URL, vuelva a escribir la regla para el funcionamiento de las imágenes y se le redirigirá a la imagen hospedada en **blobs de Azure Almacenamiento**.

1. Abra Internet Explorer y vaya a la aplicación web (por ejemplo, `http://<your-web-site>.azurewebsites.net`). Inicie sesión con las credenciales creadas anteriormente.

    ![Mostrando la aplicación web de cuestionarios "geek" con la imagen](maintainable-azure-websites-managing-change-and-scale/_static/image74.png "mostrando la aplicación web de cuestionarios \"geek\" con la imagen")

    *Mostrando la aplicación web de cuestionarios "geek" con la imagen*
2. Presione **F12** para iniciar las herramientas de desarrollo, seleccione el **red** pestaña y empiece a grabar.

    ![Iniciar grabación red](maintainable-azure-websites-managing-change-and-scale/_static/image75.png "Iniciar grabación de red")

    *Iniciar grabación de red*
3. Presione **CTRL + F5** para actualizar la página web.
4. Una vez que la página ha terminado de cargarse, debería ver una solicitud HTTP para el **/img/logo-big.png** dirección URL con un HTTP **301** resultado (Redireccionamiento) y otra solicitud para `http://[YOUR-STORAGE-ACCOUNT].blob.core.windows.net/images/logo-big.png` dirección URL con un HTTP **200** resultado.

    ![Comprobando la redirección de URL](maintainable-azure-websites-managing-change-and-scale/_static/image76.png "que muestra la redirección de las herramientas de desarrollo")

    *Comprobando la redirección de URL*

<a id="Exercise5"></a>
### <a name="exercise-5-using-autoscale-for-web-apps"></a>Ejercicio 5: Usar el escalado automático para las aplicaciones Web

> [!NOTE]
> Este ejercicio es opcional, ya que requiere compatibilidad con la carga de Web &amp; las pruebas de rendimiento que solo está disponible para **Visual Studio 2013 Ultimate Edition**. Para obtener más información sobre características específicas de Visual Studio 2013, comparar versiones [aquí](https://www.microsoft.com/visualstudio/eng/products/compare).


**Azure App Service Web Apps** proporciona la característica de escalado automático para las aplicaciones web que se ejecutan en **modo estándar**. El escalado automático permite que Azure escale automáticamente el recuento de instancias de la aplicación web según la carga. Cuando se habilita el escalado automático, Azure comprueba la CPU de la aplicación web una vez cada cinco minutos y agrega instancias según sea necesario en ese momento dado. Si el uso de CPU es bajo, Azure quitará instancias una vez cada dos horas para asegurarse de que no se degrada el rendimiento de la aplicación web.

En este ejercicio recorrerá los pasos necesarios para configurar el **escalado automático** cuentan para el **"geek" Quiz** aplicación web. Esta característica se comprobará mediante la ejecución de una prueba de carga de Visual Studio para generar la carga de CPU necesaria en la aplicación para desencadenar una actualización de la instancia.

<a id="Ex5Task1"></a>
#### <a name="task-1--configuring-autoscale-based-on-the-cpu-metric"></a>Tarea 1: configuración de escalado automático en función de la métrica de CPU

En esta tarea usará el portal de administración de Azure para habilitar la característica de escalado automático para la aplicación web que creó en el ejercicio 2.

1. En el [portal de administración de Azure](https://manage.windowsazure.com/), seleccione **Websites** y haga clic en la aplicación web que creó en el ejercicio 2.
2. Navegue hasta la **escala** página. En el **capacidad** sección, seleccione **CPU** para el **escalar por métrica** configuración.

    > [!NOTE]
    > Al escalar por CPU, Azure ajusta dinámicamente el número de instancias que usa la aplicación si cambia el uso de CPU.

    ![Seleccionar la opción para escalar por CPU](maintainable-azure-websites-managing-change-and-scale/_static/image77.png "seleccione la métrica de CPU para el escalado automático")

    *Seleccionar la opción para escalar por CPU*
3. Cambiar el **CPU de destino** configuración **20**-**40** por ciento.

    > [!NOTE]
    > Este intervalo representa el uso medio de CPU de la aplicación web. Azure agregará o quitará instancias para mantener la aplicación web en este intervalo. El número mínimo y máximo de instancias que se usarán para el escalado se especifica en el **recuento de instancias** configuración. Azure nunca pasará por encima o más allá de ese límite.
    > 
    > El valor predeterminado **CPU de destino** se modifican los valores solo para los fines de este laboratorio. Al configurar el intervalo de CPU con valores pequeños, aumentan las posibilidades de desencadenador de escalado automático cuando se coloca una carga moderada en la aplicación.

    ![Cambiar el destino de la CPU entre 20 y 40 por ciento](maintainable-azure-websites-managing-change-and-scale/_static/image78.png "cambiando la CPU de destino para que esté entre 20 y 40 por ciento")

    *Cambio de la CPU de destino para que esté entre 20 y 40 por ciento*
4. Haga clic en **guardar** en la barra de comandos para guardar los cambios.

<a id="Ex5Task2"></a>
#### <a name="task-2--load-testing-with-visual-studio"></a>Tarea 2: pruebas de carga con Visual Studio

Ahora que **escalado automático** ha sido configurado, se creará un **proyecto de prueba de carga y rendimiento Web** en Visual Studio para generar la carga de la CPU en la aplicación web.

1. Abra **Visual Studio Ultimate 2013** y seleccione **archivo | Nuevo | Proyecto...**  para iniciar una nueva solución.

    ![Crear un nuevo proyecto](maintainable-azure-websites-managing-change-and-scale/_static/image79.png "crear un nuevo proyecto")

    *Crear un nuevo proyecto*
2. En el **nuevo proyecto** cuadro de diálogo, seleccione **proyecto de prueba de carga y rendimiento Web** bajo el **Visual C# | Prueba** ficha. Asegúrese de que **.NET Framework 4.5** está seleccionado, denomine el proyecto *WebAndLoadTestProject*, elija un **ubicación** y haga clic en **Aceptar**.

    ![Crear un nuevo proyecto de prueba de carga y Web](maintainable-azure-websites-managing-change-and-scale/_static/image80.png "crear un nuevo proyecto de prueba de carga y Web")

    *Crear un nuevo proyecto de prueba de carga y Web*
3. En el **WebTest1.webtest** haga clic en el **WebTest1** nodo y haga clic en **Agregar solicitud**.

    ![Adición de una solicitud a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image81.png "adición de una solicitud a WebTest1")

    *Adición de una solicitud a WebTest1*
4. En el **propiedades** ventana del nuevo nodo de solicitud, actualice el **Url** propiedad para que apunte a la dirección URL de la aplicación web (por ejemplo, *[ http://geek-quiz.azurewebsites.net/ ](http://geek-quiz.azurewebsites.net/)*).

    ![Cambiar la propiedad Url](maintainable-azure-websites-managing-change-and-scale/_static/image82.png "cambiar la propiedad de dirección Url")

    *Cambiar la propiedad de dirección Url*
5. En el **WebTest1.webtest** ventana, haga clic en **WebTest1** y haga clic en **agregar bucle...** .

    ![Agregar un bucle a WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image83.png "agregar un bucle a WebTest1")

    *Agregar un bucle a WebTest1*
6. En el **Agregar regla condicional y elementos al bucle** cuadro de diálogo, seleccione el **de bucles for** de regla y modificar las propiedades siguientes.

   1. **Valor de finalización:** 1000
   2. **Nombre de parámetro de contexto:** iterador
   3. **Valor de incremento:** 1

      ![Seleccione la regla de bucles for y actualizar las propiedades](maintainable-azure-websites-managing-change-and-scale/_static/image84.png "seleccionando la regla de bucles for y actualizar las propiedades")

      *Seleccione la regla de bucles for y actualizar las propiedades*
7. En el **elementos del bucle** , seleccione la solicitud que creó anteriormente para que sea el primer y último elemento del bucle. Haga clic en **Aceptar** para continuar.

    ![Seleccionar el primer y último elemento del bucle](maintainable-azure-websites-managing-change-and-scale/_static/image85.png "selecciona los elementos primeros y últimos del bucle")

    *Seleccionar el primer y último elemento del bucle*
8. En **el Explorador de soluciones**, haga clic en el **WebAndLoadTestProject** del proyecto, expanda el **agregar** menú y seleccione **prueba de carga...** .

    ![Agregar una prueba de carga al proyecto WebAndLoadTestProject](maintainable-azure-websites-managing-change-and-scale/_static/image86.png "agregar una prueba de carga al proyecto WebAndLoadTestProject")

    *Agregar una prueba de carga al proyecto WebAndLoadTestProject*
9. En el **Asistente para prueba de carga nueva** cuadro de diálogo, haga clic en **siguiente**.

    ![Asistente para prueba de carga nueva](maintainable-azure-websites-managing-change-and-scale/_static/image87.png "Asistente para nueva prueba de carga")

    *Asistente para nueva prueba de carga*
10. En el **escenario** página, seleccione **no usar tiempos de reflexión** y haga clic en **siguiente**.

    ![Cuando se selecciona no se debe utilizar tiempos de reflexión](maintainable-azure-websites-managing-change-and-scale/_static/image88.png "cuando se selecciona no se debe utilizar tiempos de reflexión")

    *Seleccionar que no desean para utilizar tiempos de reflexión*
11. En el **modelo de carga** página, asegúrese de que el **carga constante** está seleccionada. Cambiar el **recuento de usuarios** si se establece en **250** a los usuarios y haga clic en **siguiente**.

    ![Cambiar el número de usuarios a 250](maintainable-azure-websites-managing-change-and-scale/_static/image89.png "cambiar el número de usuarios a 250")

    *Cambiar el número de usuarios a 250*
12. En el **modelo de combinación de pruebas** página, seleccione **basado en pruebas por orden secuencial** y haga clic en **siguiente**.

    ![Seleccionar el modelo de combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image90.png "seleccionando el modelo de combinación de pruebas")

    *Seleccionar el modelo de combinación de pruebas*
13. En el **modelo de combinación de pruebas** página, haga clic en **agregar...**  para agregar una prueba a la combinación.

    ![Agregar una prueba a la combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image91.png "agregar una prueba a la combinación de pruebas")

    *Agregar una prueba a la combinación de pruebas*
14. En el **agregar pruebas** cuadro de diálogo, haga doble clic en **WebTest1** para agregar la prueba para el **pruebas seleccionadas** lista. Haga clic en **Aceptar** para continuar.

    ![Adición de la prueba WebTest1](maintainable-azure-websites-managing-change-and-scale/_static/image92.png "agregar la prueba WebTest1")

    *Adición de la prueba WebTest1*
15. En el **combinación de pruebas** página, haga clic en **siguiente**.

    ![Completar la página de la combinación de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image93.png "completar la página de la combinación de pruebas")

    *Completar la página de la combinación de pruebas*
16. En el **combinación de redes** página, haga clic en **siguiente**.

    ![Al hacer clic en siguiente en la página de la combinación de redes](maintainable-azure-websites-managing-change-and-scale/_static/image94.png "al hacer clic en siguiente en la página de la combinación de redes")

    *Haga clic en siguiente en la página de la combinación de redes*
17. En el **combinación de exploradores** página, seleccione **Internet Explorer 10.0** como el tipo de explorador y haga clic en **siguiente**.

    ![Seleccionar el tipo de explorador](maintainable-azure-websites-managing-change-and-scale/_static/image95.png "seleccionando el tipo de explorador")

    *Seleccionar el tipo de explorador*
18. En el **conjuntos de contadores** página, haga clic en **siguiente**.

    ![Haga clic en siguiente en la página de conjuntos de contadores](maintainable-azure-websites-managing-change-and-scale/_static/image96.png "al hacer clic en siguiente en la página de conjuntos de contadores")

    *Haga clic en siguiente en la página de conjuntos de contadores*
19. En el **parámetros de ejecución** , establezca el **duración de la prueba de carga** a **5 minutos** y haga clic en **finalizar**.

    ![Establecer la duración de la prueba de carga en 5 minutos](maintainable-azure-websites-managing-change-and-scale/_static/image97.png "establecer la duración de la prueba de carga en 5 minutos")

    *Establecer la duración de la prueba de carga en 5 minutos*
20. En **el Explorador de soluciones**, haga doble clic en el **Local.settings** archivo para explorar la configuración de pruebas. De forma predeterminada, Visual Studio usa el equipo local para ejecutar las pruebas.

    > [!NOTE]
    > Como alternativa, puede configurar el proyecto de prueba para ejecutar las pruebas de carga en la nube mediante **Visual Studio Online (VSO)**. VSO proporciona una carga en la nube las pruebas de servicio que simula una carga más realista, evitando las restricciones del entorno local, como la capacidad de CPU, memoria y ancho de banda de red. Para obtener más información sobre el uso de VSO para ejecutar pruebas de carga, consulte [en este artículo](https://www.visualstudio.com/get-started/load-test-your-app-vs).

    ![Configuración de pruebas](maintainable-azure-websites-managing-change-and-scale/_static/image98.png)

<a id="Ex5Task3"></a>
#### <a name="task-3--autoscale-verification"></a>Tarea 3: comprobación de escalado automático

Ahora ejecute la prueba de carga que creó en la tarea anterior y ver cómo se comporta la aplicación web bajo carga.

1. En **el Explorador de soluciones**, haga doble clic en **LoadTest1.loadtest** para abrir la prueba de carga.

    ![Abrir LoadTest1.loadtest](maintainable-azure-websites-managing-change-and-scale/_static/image99.png "abrir LoadTest1.loadtest")

    *Apertura LoadTest1.loadtest*
2. En el **LoadTest1.loadtest** ventana, haga clic en el primer botón en el cuadro de herramientas para ejecutar la prueba de carga.

    ![Ejecutar pruebas de carga](maintainable-azure-websites-managing-change-and-scale/_static/image100.png "ejecutando la prueba de carga")

    *Ejecutar pruebas de carga*
3. Espere hasta que se complete la prueba de carga.

    > [!NOTE]
    > La prueba de carga simula varios usuarios que envían simultáneamente solicitudes a la aplicación web. Cuando se ejecuta la prueba, puede supervisar los contadores disponibles para detectar los errores, advertencias u otra información relacionada con la ejecución de pruebas de carga.

    ![Ejecución de pruebas de carga](maintainable-azure-websites-managing-change-and-scale/_static/image101.png "espera hasta que se complete la prueba de carga")

    *Ejecuta la prueba de carga*
4. Una vez finalizada la prueba, vuelva al portal de administración y navegue hasta la **escala** página de la aplicación web. En el **capacidad** sección, debería ver en el gráfico que automáticamente se ha implementado una nueva instancia.

    ![Nueva instancia implementada automáticamente](maintainable-azure-websites-managing-change-and-scale/_static/image102.png)

    *Nueva instancia implementada automáticamente*

    > [!NOTE]
    > Puede tardar varios minutos para que aparezcan en el gráfico de los cambios (presione **CTRL + F5** periódicamente para actualizar la página). Si no ve los cambios, puede intentar lo siguiente:
    > 
    > - Aumente la duración de la prueba de carga (por ejemplo, para **10 minutos**)
    > - Reducir los valores máximo y mínimos de la **CPU de destino** rango en la configuración de escalado automático de la aplicación web
    > - Ejecutar la prueba de carga en la nube con **Visual Studio Online**. Obtener más información [aquí](https://www.visualstudio.com/get-started/load-test-your-app-vs.aspx)

* * *

<a id="Summary"></a>
## <a name="summary"></a>Resumen

En este laboratorio práctico, ha aprendido a configurar e implementar la aplicación para aplicaciones web de producción en Azure. Se inició al detectar y actualizar bases de datos mediante **migraciones de Entity Framework Code First**, continuado, a continuación, al implementar nuevas versiones de su sitio con **Git** y la realización de reversiones a la versión estable más reciente de su sitio. Además, ha aprendido a escalar la aplicación usa el almacenamiento para mover el contenido estático en un contenedor de blobs.
