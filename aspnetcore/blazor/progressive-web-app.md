---
title: Creación de aplicaciones web progresivas con ASP.NET Core Blazor WebAssembly
author: guardrex
description: Obtenga más información sobre cómo crear aplicaciones web progresivas basadas en Blazor (PWA), aplicaciones web que usan las características modernas del explorador para comportarse como aplicaciones de escritorio.
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/09/2020
no-loc:
- Blazor
- SignalR
uid: blazor/progressive-web-app
ms.openlocfilehash: f1c1ce50f20bf579e67e1d4eb02cc7d9d681e354
ms.sourcegitcommit: 98bcf5fe210931e3eb70f82fd675d8679b33f5d6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/11/2020
ms.locfileid: "79083564"
---
# <a name="build-progressive-web-applications-with-aspnet-core-opno-locblazor-webassembly"></a>Creación de aplicaciones web progresivas con ASP.NET Core Blazor WebAssembly

Por [Steve Sanderson](https://github.com/SteveSandersonMS)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

[!INCLUDE[](~/includes/blazorwasm-3.2-template-article-notice.md)]

Una aplicación web progresiva (PWA) es una aplicación basada en web que usa las API y las capacidades del explorador moderno para comportarse como una aplicación de escritorio. Estas son algunas de ellas:

* Trabajo sin conexión y carga instantánea, independientemente de la velocidad de la red
* Posibilidad de ejecutarse en su propia ventana de la aplicación, no solo en una ventana del explorador
* Apertura desde el menú de inicio del sistema operativo host (SO) o la pantalla principal, o bien mediante acoplamiento
* Recepción de notificaciones de inserción desde un servidor back-end, incluso aunque el usuario no esté usando la aplicación
* Actualización automática en segundo plano

Un usuario podría descubrir y usar la aplicación primero en su explorador web, como cualquier otra aplicación de página única (SPA), y, después, proceder a instalarla en su sistema operativo y habilitar las notificaciones de inserción. Este es el motivo por el que se usa el término *progresivo*.

Blazor WebAssembly es una verdadera plataforma de aplicaciones web de cliente basada en estándares, por lo que puede usar cualquier API del explorador, incluidas las API de PWA necesarias para las funcionalidades indicadas anteriormente. Puede funcionar sin conexión, de la misma manera que cualquier otra tecnología web del lado cliente.

## <a name="pwa-template"></a>Plantilla de PWA

Al crear una nueva aplicación Blazor WebAssembly, se le ofrece la opción de agregar características de PWA. En Visual Studio, la opción se proporciona como una casilla en el cuadro de diálogo de creación del proyecto:

![imagen](https://user-images.githubusercontent.com/1101362/76207411-a6b54200-61f5-11ea-9dfc-6acd87a91428.png)

Si va a crear el proyecto en la línea de comandos, puede usar la marca `--pwa`. Por ejemplo,

```dotnetcli
dotnet new blazorwasm --pwa -o MyNewProject
```

En ambos casos, se puede combinar con la opción "ASP.NET Core hospedada", si quiere, pero no es necesario hacerlo. Las características de PWA son independientes del modelo de hospedaje.

## <a name="installation-and-app-manifest"></a>Instalación y manifiesto de la aplicación

Al visitar una aplicación creada con la opción de plantilla PWA, los usuarios tienen la opción de instalar la aplicación desde el menú inicio o la pantalla principal de su sistema operativo, o bien mediante el acoplamiento.

La forma en la que se presenta esta opción depende del explorador del usuario. Por ejemplo, al usar exploradores basados en Chromium de escritorio, como Edge o Chrome, aparece un botón *Agregar* en la barra de direcciones URL:

![imagen](https://user-images.githubusercontent.com/1101362/76208127-f7796a80-61f6-11ea-8aea-7fba704be787.png)

En iOS, los visitantes pueden instalar la PWA mediante el botón *Compartir* de Safari y la opción que permite la *adición a la pantalla de inicio*. En Chrome para Android, los usuarios deben pulsar el botón *Menú* en la esquina superior derecha y seleccionar *Agregar a la pantalla principal*.

Una vez instalada, la aplicación aparece en su propia ventana, sin ninguna barra de direcciones.

![imagen](https://user-images.githubusercontent.com/1101362/76208588-e2e9a200-61f7-11ea-85e1-8c3fc849b252.png)

Para personalizar el título, la combinación de colores, el icono u otros detalles de la ventana, vea el archivo `manifest.json` en el directorio *wwwroot* del proyecto. El esquema de este archivo se define mediante los estándares web. Para obtener documentación detallada, consulte https://developer.mozilla.org/docs/Web/Manifest.

## <a name="offline-support"></a>Compatibilidad sin conexión

De forma predeterminada, las aplicaciones creadas con la opción de plantilla PWA tienen compatibilidad para ejecutarse sin conexión. Un usuario debe visitar primero la aplicación mientras está en línea y, después, el explorador descargará y almacenará en caché todos los recursos necesarios para trabajar sin conexión.

> [!IMPORTANT]
> La compatibilidad sin conexión solo está habilitada para *aplicaciones* publicadas. No se habilita durante el desarrollo. Esto se debe a que interferiría con el ciclo de desarrollo habitual, que incluye la implementación de cambios y la realización de pruebas.

> [!WARNING]
> Si quiere enviar una PWA habilitada para su uso sin conexión, hay [varias advertencias importantes](#caveats-for-offline-pwas) que debe comprender. Son inherentes a PWA sin conexión y no son específicas de Blazor. Asegúrese de leer y comprender estas advertencias antes de realizar suposiciones sobre cómo funcionará la aplicación habilitada para su uso sin conexión.

Para ver cómo funciona la compatibilidad sin conexión, primero [publique la aplicación](https://docs.microsoft.com/aspnet/core/host-and-deploy/blazor/?view=aspnetcore-3.1&tabs=visual-studio#publish-the-app) y hospédela en un servidor que admita HTTPS. Al visitar la aplicación, debería poder abrir las herramientas de desarrollo del explorador y comprobar que se ha registrado un *trabajo de servicio* para el host:

![imagen](https://user-images.githubusercontent.com/1101362/76210294-bd5e9780-61fb-11ea-9869-65c55c62803d.png)

Además, si vuelve a cargar la página, en la pestaña *Red* debería ver que todos los recursos necesarios para cargar la página se recuperan del *trabajo de servicio* o la *caché de memoria*:

![imagen](https://user-images.githubusercontent.com/1101362/76210472-1d553e00-61fc-11ea-84c6-291644df709e.png)

Esto muestra que el explorador no depende del acceso a la red para cargar la aplicación. Para comprobarlo, puede apagar el servidor web o indicar al explorador que simule el modo sin conexión:

![imagen](https://user-images.githubusercontent.com/1101362/76210556-47a6fb80-61fc-11ea-9d12-20a8f6528744.png)

Ahora, incluso sin acceso al servidor web, debería poder recargar la página y ver que la aplicación todavía se carga y se ejecuta. Del mismo modo, aunque se simule una conexión de red muy lenta, la página se cargará inmediatamente, ya que lo hace con independencia de la red.

### <a name="service-worker"></a>Trabajo de servicio

La compatibilidad sin conexión se logra mediante un trabajo de servicio. Se trata de un estándar web, no específico de Blazor. Para obtener documentación sobre los trabajos de servicio, consulte https://developer.mozilla.org/docs/Web/API/Service_Worker_API. Para obtener más información sobre los patrones de uso comunes de los trabajos de servicio, consulte el excelente artículo [Ciclo de vida de trabajo de servicio](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle).

La plantilla de PWA de Blazor genera dos archivos de trabajo de servicio:

* *wwwroot/service-worker.js*, que se usa durante el desarrollo.
* *wwwroot/service-worker.published.js*, que se usa una vez publicada la aplicación.

Si quiere compartir la lógica entre estos dos archivos, considere la posibilidad de agregar un tercer archivo JavaScript para que contenga la lógica común y use [`self.importScripts`](https://developer.mozilla.org/docs/Web/API/WorkerGlobalScope/importScripts) para cargar dicha lógica en ambos archivos.

#### <a name="cache-first-fetch-strategy"></a>Estrategia de captura desde la caché

El servicio de trabajo integrado *service-worker.published.js* resuelve las solicitudes mediante una estrategia *desde la caché*. Esto significa que siempre prefiere devolver el contenido almacenado en la caché, si está disponible, independientemente de si el usuario tiene acceso de red o de si el contenido nuevo está disponible en el servidor.

Existen dos razones por las que esto es útil:

* **Garantiza la confiabilidad.** El acceso a la red no es un estado booleano. Un usuario no está simplemente "en línea" o "sin conexión". En realidad, el dispositivo del usuario puede creer que está en línea, pero la red puede ser tan lenta como para que no sea factible esperar. O bien, es posible que la red devuelva resultados no válidos para ciertas direcciones URL, como cuando hay un portal WIFI cautivo que está bloqueando o redirigiendo determinadas solicitudes. Esta es la razón por la que la API de `navigator.onLine` del explorador no es confiable y no se debe depender de ella.
* **Garantiza la corrección.** Al compilar una memoria caché de recursos sin conexión, el trabajo de servicio usa el hash de contenido para garantizar que ha capturado una instantánea completa y autocoherente de los recursos en un solo instante en el tiempo. Después, esta caché se utiliza como unidad atómica. Teniendo esto en cuenta, no hay ningún punto que solicite a la red recursos más recientes, ya que las únicas versiones que quiere son las que ya se han almacenado en la memoria caché. Cualquier otra cosa implica un riesgo de incoherencias e incompatibilidad (por ejemplo, intentar usar versiones de ensamblados .NET que no se compilaron juntos).

#### <a name="background-updates"></a>Actualizaciones en segundo plano

Como modelo mental, puede pensar que una PWA sin conexión se comporta como una aplicación móvil que se puede instalar. Siempre se inicia inmediatamente, independientemente de la conectividad de la red, pero la lógica de la aplicación instalada procede de una instantánea de un momento dado que podría no ser la versión más reciente.

La plantilla PWA Blazor genera aplicaciones que intentan actualizarse automáticamente en segundo plano cada vez que el usuario realiza una visita y tiene una conexión de red operativa. Así es como funciona:

* Durante la compilación, el proyecto genera un *manifiesto de recursos de trabajo de servicio*. De forma predeterminada, este se denomina *service-worker-assets.js*. Aquí se enumeran todos los recursos estáticos que la aplicación necesita para funcionar sin conexión, como los ensamblados .NET, los archivos JavaScript, CSS, etc., incluidos sus hash de contenido. Esta lista la carga el trabajo de servicio para que sepa qué recursos almacenar en la caché.
* Cada vez que el usuario visita su aplicación, el explorador vuelve a solicitar *service-worker.js* y *service-worker-assets.js* en segundo plano. Si el servidor devuelve contenido modificado para cualquiera de estos archivos (comparado byte por byte con el trabajo de servicio instalado), el trabajo de servicio intenta instalar una nueva versión de sí mismo.
* Al instalar una nueva versión de sí mismo, el trabajo de servicio crea una nueva memoria caché independiente para los recursos sin conexión y comienza a rellenarla con los recursos enumerados en *service-worker-assets.js*. Esta lógica se implementa en la `onInstall`función dentro de *service-worker.published.js*.
* Si el proceso se completa correctamente (es decir, todos los recursos se cargan sin errores y todos los hash de contenido coinciden), el nuevo trabajo de servicio entra en estado "en espera de activación". En cuanto el usuario cierra la aplicación (es decir, no quedan pestañas o ventanas que muestren la aplicación), el nuevo trabajo de servicio se convierte en "activo" y se usará para las visitas posteriores a la aplicación. El trabajo de servicio anterior y su memoria caché se eliminan.
* Si el proceso no se completa correctamente, se descarta la nueva instancia de trabajo de servicio. El proceso de actualización se volverá a intentar en la próxima visita del usuario; con suerte, tendrá una mejor conexión de red que permita completar las solicitudes.

Puede personalizar cualquier aspecto de este proceso editando la lógica del trabajo de servicio. Ninguna de las opciones anteriores es específica de Blazor, sino que es simplemente una sugerencia proporcionada por la opción de plantilla de PWA. Para obtener más información, consulte la [documentación sobre el trabajo de servicio](https://developer.mozilla.org/docs/Web/API/Service_Worker_API.).

#### <a name="how-requests-are-resolved"></a>Procedimientos para resolver las solicitudes

Tal y como se ha descrito anteriormente, el trabajo de servicio predeterminado utiliza una estrategia *desde la caché*, lo que significa que intenta servir contenido almacenado en la caché cuando esté disponible. Si no hay contenido almacenado en la caché para una dirección URL determinada (por ejemplo, cuando se solicitan datos de una API de back-end), el trabajo de servicio recurre a una solicitud de red normal que solo puede realizarse si el servidor es accesible. Esta lógica se implementa `onFetch`dentro de *service-worker.published.js*.

Si los componentes de Blazor dependen de la solicitud de datos de las API de back-end y quiere proporcionar una experiencia de usuario sencilla en caso de que estas solicitudes generen un error debido a la falta de disponibilidad de la red, debe implementar la lógica dentro de los componentes. Por ejemplo, use solicitudes `try/catch` en torno a `HttpClient`.

#### <a name="support-server-rendered-pages"></a>Compatibilidad de las páginas representadas por el servidor

Tenga en cuenta lo que sucede cuando el usuario navega por primera vez a una dirección URL como `/counter` o cualquier otro vínculo profundo de la aplicación. En estos casos, no quiere devolver el contenido almacenado en la caché como `/counter`, sino que necesita que el explorador cargue el contenido almacenado en la caché como `/index.html` para iniciar la aplicación Blazor WebAssembly. Estas solicitudes iniciales se conocen como solicitudes de *navegación* (en lugar de solicitudes de *subrecursos* de imágenes, CSS, etc., o solicitudes de *captura/XHR* de los datos de la API).

El trabajo de servicio predeterminado contiene lógica de casos especiales para las solicitudes de navegación. Las resuelve devolviendo el contenido almacenado en la caché para `/index.html`, independientemente de la dirección URL solicitada. Esta lógica se implementa en la`onFetch` función dentro de *service-worker.published.js*.

Si la aplicación tiene determinadas direcciones URL que deben devolver el código HTML representado por el servidor (y no servir `/index.html` de la memoria caché), debe editar la lógica en el trabajo del servicio. Por ejemplo, si todas las direcciones URL que contienen `/Identity/` deben controlarse como solicitudes normales solo en línea en el servidor, modifique la lógica de *service-worker.published.js*`onFetch`. Busque el código siguiente:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate';
```

Cambie el código por lo siguiente:

```javascript
const shouldServeIndexHtml = event.request.mode === 'navigate'
    && !event.request.url.includes('/Identity/');
```

Si no lo hace, independientemente de la conectividad de red, el trabajo de servicio interceptará las solicitudes de dichas direcciones URL y las resolverá mediante `/index.html`.

#### <a name="control-asset-caching"></a>Control del almacenamiento en caché de recursos

Si el proyecto define una propiedad de MSBuild llamada `ServiceWorkerAssetsManifest`, las herramientas de compilación de Blazor generarán un manifiesto de recursos de trabajo de servicio con el nombre especificado. La plantilla de PWA predeterminada produce un archivo de proyecto que contiene lo siguiente:

```xml
<ServiceWorkerAssetsManifest>service-worker-assets.js</ServiceWorkerAssetsManifest>
```

El archivo se coloca en el directorio de salida *wwwroot*, por lo que el explorador puede recuperar este archivo solicitando `/service-worker-assets.js`. Para ver el contenido, abra *YourProject\bin\Debug\netstandard2.1\wwwroot\service-worker-assets.js* en un editor de texto. Sin embargo, no edite el archivo, ya que se volverá a generar en cada compilación.

De forma predeterminada, este manifiesto muestra:

* Los recursos administrados por Blazor, como los ensamblados .NET y los archivos en tiempo de ejecución de WebAssembly .NET necesarios para funcionar sin conexión.
* Todos los recursos que se publicarán en el directorio *wwwroot*, como imágenes, archivos CSS y archivos JavaScript. Esto incluye los recursos web estáticos que proporcionan los proyectos externos y los paquetes NuGet.

Puede controlar qué recursos capturará y almacenará en la caché el trabajo de servicio mediante la edición de la lógica`onInstall` en *service-worker.published.js*. De forma predeterminada, capturará y almacenará en la caché los archivos que coincidan con las extensiones de nombre de archivo web típicas, como *.html*, *.css*, *.js*, *.wasm*, etc., además de tipos de archivo específicos de Blazor WebAssembly ( *.dll*, *.pdb*).

Si quiere incluir recursos adicionales que no están presentes en el directorio *wwwroot*, puede hacerlo definiendo entradas de ItemGroup de MSBuild adicionales. Por ejemplo, en el archivo del proyecto, agregue:

```xml
<ItemGroup>
    <ServiceWorkerAssetsManifestItem
        Include="MyDirectory\AnotherFile.json"
        RelativePath="MyDirectory\AnotherFile.json"
        AssetUrl="files/AnotherFile.json" />
</ItemGroup>
```

Los metadatos de `AssetUrl` especifican la dirección URL de base relativa que debe usar el explorador al capturar el recurso en la memoria caché. Puede ser independiente del nombre del archivo de origen en el disco.

> [!IMPORTANT]
> Agregar un elemento `ServiceWorkerAssetsManifestItem` no hace que el archivo se publique en el directorio *wwwroot*. Usted controla la salida de la publicación por separado. El elemento `ServiceWorkerAssetsManifestItem` solo hace que aparezca una entrada adicional en el manifiesto de recursos de trabajo de servicio.

## <a name="push-notifications"></a>Notificaciones de inserción

Al igual que cualquier otra PWA, una PWA Blazor WebAssembly puede recibir notificaciones de inserción de un servidor back-end. El servidor puede enviarlas en cualquier momento, incluso cuando el usuario no esté utilizando activamente la aplicación (por ejemplo, cuando un usuario diferente realice una acción que pueda ser pertinente).

El mecanismo para enviar una notificación de inserción es totalmente independiente de Blazor WebAssembly, ya que lo implementa el servidor back-end, que puede usar cualquier tecnología. Si quiere enviar notificaciones de inserción desde un servidor de ASP.NET Core, considere la posibilidad de [usar una técnica similar a la del taller de Blazing Pizza](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#sending-push-notifications).

El mecanismo para recibir y mostrar una notificación de inserción en el cliente también es independiente de Blazor WebAssembly, ya que se implementa en el trabajo de servicio, que es un archivo de JavaScript. Por ejemplo, puede ver de nuevo el [enfoque que se usa en el taller de Blazing Pizza](https://github.com/dotnet-presentations/blazor-workshop/blob/master/docs/09-progressive-web-app.md#displaying-notifications).

## <a name="caveats-for-offline-pwas"></a>Advertencias de PWA sin conexión

No todas las aplicaciones deben intentar admitir el uso sin conexión. Esto agrega una complejidad significativa, aunque no siempre es pertinente.

La compatibilidad sin conexión normalmente solo es pertinente:

* Si el almacén de datos principal es local para el explorador. Por ejemplo, al compilar una interfaz de usuario para un dispositivo [IoT](https://en.wikipedia.org/wiki/Internet_of_things) que almacena datos en `localStorage` o [IndexedDB](https://developer.mozilla.org/docs/Web/API/IndexedDB_API).

* Si realiza un trabajo significativo para capturar y almacenar en la caché los datos de la API de back-end pertinentes para cada usuario, para que puedan desplazarse por ellos sin conexión. Si admite la edición, también tendrá que crear un sistema para realizar el seguimiento de los cambios y sincronizarlos con el back-end.

* Si el objetivo es garantizar que la aplicación se cargue inmediatamente, independientemente de las condiciones de la red. En ese caso, tendrá que implementar una experiencia de usuario adecuada en torno a las solicitudes de API de back-end para mostrar el progreso de las solicitudes y comportarse correctamente cuando se produzca un error debido a la falta de disponibilidad de la red.

Además, las PWA con capacidad sin conexión deben tratar con una gran variedad de complicaciones adicionales. Los desarrolladores deben familiarizarse cuidadosamente con las siguientes advertencias.

### <a name="offline-support-only-when-published"></a>Compatibilidad sin conexión solo al realizar la publicación

La plantilla de PWA de Blazor solo habilita la compatibilidad sin conexión cuando se publica. Esto se debe a que, durante el desarrollo, normalmente quiere ver cada cambio reflejado inmediatamente en el explorador, sin pasar por un proceso de actualización en segundo plano.

Por lo tanto, al compilar una aplicación que admita el uso sin conexión, no basta con probar la aplicación en modo de desarrollo. Debe probar la aplicación en su estado publicado para entender cómo responderá a condiciones de red diferentes.

### <a name="update-completion-after-user-navigation-away-from-app"></a>Finalización de la actualización tras la navegación del usuario fuera de la aplicación

Las actualizaciones no se completarán hasta que el usuario haya navegado fuera de la aplicación en todas las pestañas. Como se explica en [Actualizaciones en segundo plano](#background-updates), después de implementar una actualización en la aplicación, el explorador capturará los archivos de trabajo de servicio actualizados y comenzará un proceso de actualización.

Lo que sorprende a muchos desarrolladores es que, incluso cuando se completa esta actualización, **no** surtirá efecto hasta que el usuario haya navegado fuera de todas las pestañas. **No** basta con actualizar la pestaña que muestra la aplicación, incluso si se trata de la única pestaña que muestra la aplicación. Hasta que la aplicación se cierre por completo, el nuevo trabajo de servicio permanecerá en el estado "en espera de activación". **Esto no es específico de Blazor, sino que es un comportamiento estándar de la plataforma web.**

Habitualmente, esto trae problemas a los desarrolladores que intentan probar las actualizaciones de su trabajo de servicio o de los recursos almacenados en la caché sin conexión. Si inserta en el repositorio las herramientas de desarrollo del explorador, puede ver algo parecido a lo siguiente:

![imagen](https://user-images.githubusercontent.com/1101362/76226394-b93f7380-6215-11ea-8572-7d52afee2dd8.png)

Mientras la lista de "clientes" (es decir, pestañas o ventanas que muestran la aplicación) no esté vacía, el trabajo seguirá en espera. La razón por la que los trabajos de servicio hacen esto es garantizar la coherencia, es decir, que todos los recursos se capturen de la misma caché atómica.

Al probar los cambios, puede que le resulte conveniente hacer clic en el vínculo "skipWaiting", como se muestra en la captura de pantalla anterior, y volver a cargar la página. Si quiere, puede automatizar este proceso para todos los usuarios mediante la codificación del trabajo de servicio para [omitir la fase de "espera" y activarlo inmediatamente al actualizar](https://developers.google.com/web/fundamentals/primers/service-workers/lifecycle#skip_the_waiting_phase). Sin embargo, si lo hace, ya no tendrá la garantía de que los recursos siempre se capturen de forma coherente desde la misma instancia de la caché.

### <a name="users-may-run-any-historical-version-of-the-app"></a>Posibilidad para los usuarios de ejecutar cualquier versión histórica de la aplicación

Normalmente, los desarrolladores web esperan que los usuarios ejecuten solo la última versión implementada de su aplicación web, ya que es lo habitual en el modelo de distribución web tradicional. Pero una PWA sin conexión es más similar a una aplicación móvil nativa, cuya versión ejecutada por los usuarios no siempre es la más reciente.

Tal como se explica en [Actualizaciones en segundo plano](#background-updates), después de implementar una actualización en la aplicación, **cada usuario existente seguirá usando una versión anterior durante, al menos, una visita adicional** (porque la actualización se produce en segundo plano y no se activa hasta que el usuario se desplace fuera de la aplicación). Además, la versión anterior que se está usando no es necesariamente la anterior que implementó; puede ser *cualquier* versión histórica, en función de cuándo el usuario haya completado una actualización por última vez.

Esto puede ser un problema si las partes de front-end y back-end de la aplicación requieren un acuerdo sobre el esquema para las solicitudes de API. No debe implementar los cambios de esquema de API incompatibles con versiones anteriores hasta que se asegure de que todos los usuarios hayan realizado la actualización, o al menos haya impedido que los usuarios utilicen versiones anteriores incompatibles de la aplicación. Esto es igual que una aplicación móvil nativa. Si implementa un cambio importante en las API de servidor, la aplicación cliente se interrumpirá para las personas que aún no hayan realizado la actualización.

Si es posible, no implemente cambios importantes en las API de back-end. Pero si tiene que hacerlo, considere la posibilidad de usar [API de trabajo de servicio estándar como `ServiceWorkerRegistration`](https://developer.mozilla.org/docs/Web/API/ServiceWorkerRegistration) para determinar si la aplicación está actualizada y, en caso contrario, evitar el uso.

### <a name="interference-with-server-rendered-pages"></a>Interferencias con páginas representadas por el servidor

[Como se describió anteriormente](#support-server-rendered-pages), si quiere omitir el comportamiento del trabajo de servicio de devolver contenido de `/index.html` para todas las solicitudes de navegación, debe editar la lógica en el trabajo de servicio.

### <a name="all-service-worker-asset-manifest-contents-are-cached-by-default"></a>Almacenamiento en caché de todos los contenidos del manifiesto de recursos del trabajo de servicio de forma predeterminada

[Como se describió anteriormente](#control-asset-caching), el archivo *service-worker-assets.js* se genera durante la compilación y enumera todos los recursos que el trabajo de servicio debe capturar y almacenar en la caché.

Puesto que esta lista incluye de forma predeterminada todo lo que se emite para *wwwroot* (incluido el contenido proporcionado por los paquetes y proyectos externos), debe tener cuidado de no incluir demasiado contenido en él. Si, por ejemplo, el directorio *wwwroot* contiene millones de imágenes, el trabajo de servicio intentará recuperarlas y almacenarlas en la caché, lo que consumiría un ancho de banda excesivo y probablemente no se completará correctamente.

Puede implementar una lógica arbitraria para controlar qué subconjunto del contenido del manifiesto se debe capturar y almacenar en la memoria caché mediante la edición de la función `onInstall` en *service-worker.published.js*.

### <a name="interaction-with-authentication"></a>Interacción con la autenticación

Se puede usar la opción de plantilla PWA junto con las opciones de autenticación. Un PWA con capacidad sin conexión también puede admitir la autenticación cuando el usuario tiene conectividad de red.

Sin embargo, cuando un usuario no tiene conectividad de red, no podrá autenticarse ni obtener tokens de acceso. Si intenta visitar la página "Inicio de sesión", se mostrará de forma predeterminada un mensaje que indica "Error de red".

Como tal, su trabajo es diseñar un flujo de interfaz de usuario que permita al usuario hacer cosas útiles mientras está sin conexión sin intentar autenticarse ni obtener tokens de acceso, o al menos que los errores que se produzcan en esos casos sean estables. Si esto no es posible en la aplicación, es posible que no quiera habilitar la compatibilidad sin conexión.
