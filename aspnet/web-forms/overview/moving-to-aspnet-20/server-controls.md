---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Controles de servidor | Documentos de Microsoft
author: microsoft
description: "ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, hablaremos sobre algunos de los cambios de arquitectura para el modo en ASP.NET 2.0 y Visual Studio 200..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 09f1a2e4de024e5778e69fdd691d9cb0040459f3
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/10/2017
---
<a name="server-controls"></a>Controles de servidor
====================
por [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, hablaremos sobre algunos de los cambios de arquitectura en la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupa de los controles de servidor.


ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, hablaremos sobre algunos de los cambios de arquitectura en la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupa de los controles de servidor.

## <a name="view-state"></a>Estado de vista

El cambio principal en estado de vista en ASP.NET 2.0 es una reducción notable de tamaño. Considere la posibilidad de una página con un control de calendario en él. Este es el estado de vista en ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Ahora aquí es el estado de vista en una página idéntica en ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Es un cambio muy significativo, y teniendo en cuenta que el estado de vista se traslada y hacia atrás la conexión, este cambio puede proporcionar a los programadores un aumento significativo del rendimiento. La reducción del tamaño del estado de vista es en gran medida debido al modo en el que se administran internamente. Recuerde que la vista estado es un Base64 codificado cadena. Para entender mejor el cambio de estado de vista en ASP.NET 2.0, echemos un vistazo a los valores descodificados de los ejemplos anteriores.

Este es el estado de 1.1 vista descodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Esto puede parecer un poco un texto incomprensible, pero no hay un patrón aquí. En ASP.NET 1.x, se usan caracteres individuales para identificar tipos de datos y uso de valores delimitados por la &lt; &gt; caracteres. La "t" en el ejemplo de estado de vista anterior representa a una terna. La terna contiene un par de matrices ("l" representa una lista de matrices). Uno de los objetos de ArrayList contiene un valor Int32 ("i") con un valor de 1 y el otro contiene otro terna. La terna contiene un par de matrices, etcetera. Lo importante es recordar es que se usan tres dígitos que contienen pares, se identifican los tipos de datos a través de una letra, y usamos el &lt; y &gt; caracteres como delimitadores.

En ASP.NET 2.0, el estado de vista descodificado tiene un aspecto un poco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Debe observar un cambio en el aspecto del estado de vista descodificado. Este cambio tiene varias Fundamentos de la arquitectura. Estado de vista en ASP.NET 1.x usa el LosFormatter para serializar datos. En 2.0, se utiliza la nueva clase ObjectStateFormatter. Esta clase se ha diseñado específicamente para ayudar en la serialización y deserialización del estado de vista y el estado de control. (El estado del control se tratará en la siguiente sección). Hay muchas ventajas que se obtienen al cambiar el método mediante el cual serialización y deserialización tienen lugar. Uno de los más importantes es el hecho de que, a diferencia de la LosFormatter que usa un TextWriter, el ObjectStateFormatter usa BinaryWriter. Esto permite que ASP.NET 2.0 almacenar el estado de vista una serie de bytes en lugar de cadenas. Piense, por ejemplo, un número entero. En ASP.NET 1.1, un entero requiere 4 bytes del estado de vista. En ASP.NET 2.0, ese mismo entero requiere solo 1 byte. Otras mejoras realizadas para reducir la cantidad de estado de vista que se almacena. Valores de fecha y hora, por ejemplo, ahora se almacenan utilizando un TickCount en lugar de una cadena.

Como si todo esto no fuera suficiente, se presta atención especial al hecho de que uno de los mayores consumidores de estado de vista en 1.x era la cuadrícula de datos y controles similares. Una desventaja principal de controles, como el control DataGrid que se refiere el estado de vista es que a menudo contiene grandes cantidades de información repetida. En ASP.NET 1.x, que repite simplemente se almacena información sobre y sobre lo que resulta en un estado de vista inflan. En ASP.NET 2.0, usamos la nueva clase IndexedString para almacenar estos datos. Si se repite una cadena, simplemente se almacena el token para el IndexedString y el índice dentro de una tabla de objetos de IndexedString ejecución.

## <a name="control-state"></a>Estado del control

Uno de los principales gripes que los programadores tenían con estado de vista era el tamaño que agrega a la carga HTTP. Como se mencionó anteriormente, uno de los mayores consumidores del estado de vista es el control DataGrid. Para evitar las grandes cantidades de estado de vista generado por un elemento DataGrid, muchos desarrolladores simplemente vista estado deshabilitado de ese control. Por desgracia, esta solución no siempre una buena. Estado de vista en ASP.NET 1.x contiene no solo los datos necesarios para la funcionalidad correcta del control. También contiene información sobre el estado de la interfaz de usuario del control. Esto significa que si desea permitir para la paginación en un control DataGrid, debe habilitar el estado de vista incluso si no necesita toda la información de la interfaz de usuario que ver estado contiene. Es un escenario de todo o nada.

En ASP.NET 2.0, estado del control resuelve el problema bien a través de la introducción del estado del control. Estado del control contiene los datos que sea absolutamente necesarios para el correcto funcionamiento de un control. A diferencia del estado de vista, no se puede deshabilitar el estado del control. Por lo tanto, es importante que los datos que se almacena en el estado de control se controlan cuidadosamente.

> [!NOTE]
> Estado de control se mantiene junto con el estado de vista en el \_ \_campo de formulario oculto de estado de vista.


Este vídeo es un tutorial del estado de vista y el estado de control.


![](server-controls/_static/image1.png)


[Abra vídeo de pantalla completa](server-controls/_static/state1.wmv)


En orden para un control de servidor leer y escribir en el estado de control, debe realizar tres pasos.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Paso 1: Llamar al método RegisterRequiresControlState

El método RegisterRequiresControlState informa a ASP.NET que un control debe conservar el estado de control. Toma un argumento de tipo de Control que es el control que se va a registrar.

Es importante tener en cuenta que el registro no se conserva para solicitar. Por lo tanto, este método debe llamarse en cada solicitud, si un control consiste en conservar el estado de control. Se recomienda que se llama al método en OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Paso 2: Reemplazar SaveControlState

El método SaveControlState guarda el control de cambios de estado de un control desde la última devolución de llamada. Devuelve un objeto que representa el estado del control.

## <a name="step-3-override-loadcontrolstate"></a>Paso 3: Invalidar LoadControlState

El método LoadControlState carga el estado guardado en un control. El método toma un argumento de tipo Object que contiene el estado guardado del control.

## <a name="full-xhtml-compliance"></a>Cumplimiento de XHTML completa

Cualquier desarrollador Web conoce la importancia de normas en las aplicaciones Web. Para mantener un entorno de desarrollo basado en estándares, ASP.NET 2.0 es totalmente compatible con XHTML. Por lo tanto, todas las etiquetas son representado según los estándares XHTML en exploradores que admiten HTML 4.0 o superior.

La definición de tipo de documento en ASP.NET 1.1 fue el siguiente:

[!code-html[Main](server-controls/samples/sample6.html)]

En ASP.NET 2.0, la definición de tipo de documento predeterminado es el siguiente:

[!code-html[Main](server-controls/samples/sample7.html)]

Si elige, puede modificar la compatibilidad de XHML de forma predeterminada a través del nodo xhtmlConformance en el archivo de configuración. Por ejemplo, el nodo siguiente en el archivo web.config cambiará el cumplimiento de XHTML a XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si elige, también puede configurar ASP.NET para usar la configuración heredada que se usa en ASP.NET 1.x como sigue:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Usar adaptadores de representación adaptable

En ASP.NET 1.x, el archivo de configuración contenido un &lt;browserCaps&gt; sección que rellena un objeto HttpBrowserCapabilities. Este objeto permite que un desarrollador determinar qué dispositivo está realizando una solicitud determinada y representar código adecuadamente. En ASP.NET 2.0, el modelo se ha mejorado y ahora usa esta nueva clase. Esta clase invalida los eventos de ciclo de vida del control y controla la representación de controles en función de las capacidades de agente de usuario. Las capacidades de un agente de usuario específico se definen mediante un archivo de definición de explorador (un archivo con una extensión de archivo Browser) almacenado en el c:\windows\microsoft.net\framework\v2.0. \* \* \* \*Carpeta \CONFIG\Browsers.

> [!NOTE]
> Esta clase es una clase abstracta.


Muy parecida a la &lt;browserCaps&gt; sección en 1.x, el archivo de definición de explorador utiliza una expresión Regular para analizar la cadena de agente de usuario con el fin de identificar el explorador que lo solicitado. Se les define las funciones concretas de ese agente de usuario. ControlAdapter representa el control mediante el método Render. Por lo tanto, si invalida el método Render, no debería llamar Render en la clase base. Si lo hace, puede provocar la representación para que se produzca dos veces, una vez para el adaptador y otra para el propio control.

## <a name="developing-a-custom-adapter"></a>Desarrollar un adaptador personalizado

Puede desarrollar un adaptador personalizado propio heredando de ControlAdapter. Además, puede heredar de la clase abstracta PageAdapter en casos donde se necesita un adaptador para una página. Asignación de controles para el adaptador personalizado se realiza a través de la &lt;controlAdapters&gt; elemento en el archivo de definición de explorador. Por ejemplo, el siguiente código XML de un archivo de definición de explorador asigna el control de menú a la clase MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Mediante este modelo, resulta bastante sencillo para que un desarrollador de control para tener como destino un determinado dispositivo o explorador. También es muy sencillo para que un desarrollador tiene control completo sobre cómo representan páginas en todos los dispositivos.

## <a name="per-device-rendering"></a>Representación por dispositivo

Propiedades de control de servidor en ASP.NET 2.0 pueden ser especificada utilizando un prefijo específico del explorador por dispositivo. Por ejemplo, el código siguiente cambiará el texto de una etiqueta según el dispositivo al que se utiliza para explorar la página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Cuando la página que contiene esta etiqueta se examinarán desde Internet Explorer, la etiqueta mostrará el texto que dice "Examina desde Internet Explorer." Cuando se explora la página de Firefox, la etiqueta mostrará el texto "Examina de Firefox." Cuando la página se examinarán desde cualquier otro dispositivo, se mostrará "Está explorando desde un dispositivo desconocido". Cualquier propiedad puede especificarse mediante esta sintaxis especial.

## <a name="setting-focus"></a>Establecimiento del foco

Los programadores de ASP.NET 1.x más frecuentes acerca de cómo establecer el foco inicial en un control determinado. Por ejemplo, en una página de inicio de sesión, es útil para que el cuadro de texto Id. de usuario obtenga el foco cuando se carga la página por primera vez. En ASP.NET 1.x, esto requiere escribir algunas secuencias de comandos de cliente. Aunque este tipo de script es una tarea sencilla, ya no es necesario en ASP.NET 2.0 gracias al método SetFocus. El método SetFocus toma un argumento que indica el control que debe recibir el foco. Este argumento puede ser el identificador de cliente del control como una cadena o el nombre del control de servidor como un objeto de Control. Por ejemplo, para establecer el foco inicial a un control de cuadro de texto denominado txtUserID cuando se carga la página por primera vez, agregue el código siguiente a la página\_carga:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utiliza el controlador Webresource.axd (descrito anteriormente) para representar una función de cliente que establece el foco. El nombre de la función de cliente es WebForm\_enfoque automático tal y como se muestra aquí:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, puede utilizar el método de foco para un control para establecer el foco inicial a ese control. El método Focus se deriva de la clase de Control y está disponible para todos los controles de ASP.NET 2.0. También es posible establecer el foco a un control determinado cuando se produce un error de validación. Que se tratarán en un módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Nuevos controles de servidor en ASP.NET 2.0

Los siguientes son nuevos controles de servidor en ASP.NET 2.0. Seguiremos con más detalle algunas de ellas en módulos posteriores.

## <a name="imagemap-control"></a>Control ImageMap

El control de mapa de imágenes permite agregar puntos de conexión a una imagen que se puede iniciar una devolución de datos o navegar a una dirección URL. Existen tres tipos de puntos de conexión; CircleHotSpot, RectangleHotSpot y PolygonHotSpot. Los puntos de conexión se agregan a través de un editor de colecciones en Visual Studio o mediante programación en código. No hay ninguna interfaz de usuario disponible para dibujar los puntos de conexión en una imagen. Las coordenadas y el tamaño o el radio de la zona activa se debe especificar mediante declaración. No hay ninguna representación visual de una zona activa en el diseñador. Si se configura una zona activa para navegar a una dirección URL, se especifica la dirección URL a través de la propiedad NavigateUrl de la zona activa. En el caso de una entrada de blog hacer copia de zona activa, el PostBackValue propiedad le permite pasar una cadena en la devolución de llamada que se pueden recuperar en el código de servidor.


![Editor de la colección de hotSpot en Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor de la colección de HotSpot en Visual Studio


## <a name="bulletedlist-control"></a>Control BulletedList

El control BulletedList es una lista con viñetas que puede enlazar a datos con facilidad. La lista se puede ordenar (numerados) o desordenada a través de la propiedad BulletStyle. Cada elemento de la lista se representa mediante un objeto ListItem.


![Control BulletedList en Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: Control BulletedList en Visual Studio


## <a name="hiddenfield-control"></a>Control HiddenField

El control HiddenField agrega un campo de formulario oculto a la página, el valor de los cuales está disponible en el código de servidor. Por lo general se espera que el valor de un campo de formulario oculto permanezca inalterada entre devoluciones de datos. Sin embargo, es posible que un usuario malintencionado cambiar la antes de valor para devuelven datos. Si esto ocurre, el control HiddenField provocará el evento ValueChanged. Si tiene información confidencial en el control HiddenField y desea asegurarse de que permanece sin cambios, debe controlar el evento ValueChanged en el código.

## <a name="fileupload-control"></a>Control fileUpload

El control FileUpload en ASP.NET 2.0 permite cargar archivos en un servidor Web a través de una página ASP.NET. Este control es muy similar a la clase ASP.NET 1.x HtmlInputFile con unas pocas excepciones. En ASP.NET 1.x, se recomienda que la propiedad PostedFile comprobará si hay valores null con el fin de determinar si hubiera un archivo buena. El control FileUpload en ASP.NET 2.0 agrega una nueva propiedad HasFile que puede usar para el mismo propósito y es un poco más eficaz.

La propiedad PostedFile sigue estando disponible para el acceso a un objeto HttpPostedFile, pero algunas de las funciones de la HttpPostedFile ahora está disponible de forma intrínseca con el control FileUpload. Por ejemplo, para guardar un archivo cargado en ASP.NET 1.x, se puede llamar al método SaveAs en el objeto HttpPostedFile. Con el control FileUpload en ASP.NET 2.0, podría llamar al método SaveAs en el propio control FileUpload.

Otro cambio significativo en el comportamiento 2.0 (y es probable que el cambio más significativo) es que ya no es necesario cargar un archivo cargado todo en la memoria antes de guardarla. En 1.x, se guarda cualquier archivo que se cargó completamente en la memoria antes de que se está escribiendo en el disco. Esta arquitectura evita que la carga de archivos grandes.

En ASP.NET 2.0, el atributo requestLengthDiskThreshold del elemento httpRuntime le permite configurar la cantidad de Kilobytes se mantienen en un búfer en memoria antes de que se está escribiendo en el disco.

**IMPORTANTE**: documentación de MSDN (y documentación en otra parte) especifica que este valor se muestra en bytes (no en Kilobytes) y que el valor predeterminado es 256. El valor se especifica realmente en Kilobytes y el valor predeterminado es 80. Si tiene un valor predeterminado de K de 80, es asegurarse de que el búfer no terminar en el montón de objetos grandes.

## <a name="wizard-control"></a>Control de Asistente

Es bastante habitual encontrar los programadores de ASP.NET dificultades con intentando recopilar información de una serie de "páginas" mediante paneles o mediante la transferencia de una página a otra. Más a menudo que no es así, el esfuerzo es frustrante y lleva mucho tiempo. El nuevo control de asistente resuelve los problemas por lo que no es lineal y etapas de una interfaz de Asistente para que los usuarios están familiarizados con. El control de asistente presenta los formularios de entrada en una serie de pasos. Cada paso es de un tipo determinado, especificado por la propiedad StepType del control. Los tipos de paso disponibles son los siguientes:

| **Tipo de paso** | **Explicación** |
| --- | --- |
| Automático | El asistente determina automáticamente el tipo de paso en función de su posición dentro de la jerarquía de paso. |
| Iniciar | El primer paso, a menudo se usa para presentar una instrucción de introducción. |
| Paso | Un paso normal. |
| Finalizar | El último paso, que normalmente se utiliza para presentar un botón para finalizar al asistente. |
| Completado | Presenta un mensaje comunica finalizó correctamente o no. |

> [!NOTE]
> El control de asistente realiza un seguimiento de su estado usando el estado de control ASP.NET. Por lo tanto, la propiedad puede establecerse en false sin perjuicio alguno.


Este vídeo es un tutorial del control Wizard.


![](server-controls/_static/image2.png)


[Abra vídeo de pantalla completa](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localizar el Control

El control Localize es similar a un control del Literal. Sin embargo, el control Localize tiene un **modo** propiedad que controla cómo se representa el marcado que se agrega a ella. La propiedad Mode admite los siguientes valores:

| **Modo** | **Explicación** |
| --- | --- |
| Transformación | Marcado se transforma según el protocolo del explorador que realiza la solicitud. |
| Acceso directo | Marcado que se representa como-es. |
| Codificar | Marcado que se agrega al control se codifica mediante HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView y controles de vista

El control MultiView actúa como un contenedor para los controles de vista y el control de vista actúa como contenedor para otros controles (igual que un control de Panel). Cada vista de un control MultiView se representa mediante un control de vista único. El primer control de vista en el MultiView es vista 0, el segundo es 1, la vista etcetera. Se puede cambiar de vista especificando ActiveViewIndex del control MultiView.

## <a name="substitution-control"></a>Control de sustitución

El control de sustitución se utiliza junto con el almacenamiento en caché de ASP.NET. En casos donde desea aprovechar las ventajas del almacenamiento en caché, pero tiene algunas partes de una página que se debe actualizar en cada solicitud (es decir, las partes de una página que están exentos de almacenamiento en caché), el componente de sustitución proporciona una gran solución. El control realmente no representa ningún resultado por sí mismo. En su lugar, se enlaza a un método en el código de servidor. Cuando se solicita la página, se llama al método y la marca devuelta se representa en lugar del control de sustitución.

El método al que está enlazado el control de sustitución se especifica a través de la **MethodName** propiedad. Este método debe cumplir los siguientes criterios:

- Debe ser un método estático (compartido en VB).
- Acepta un parámetro de tipo HttpContext.
- Devuelve una cadena que representa el marcado que se debe reemplazar el control en la página.

El control de sustitución no tiene la capacidad de modificar cualquier otro control en la página, pero tiene acceso a HttpContext actual a través de su parámetro.

## <a name="gridview-control"></a>Control GridView

El control GridView es el reemplazo de DataGrid (control). Este control se tratará con más detalle en un módulo posterior.

## <a name="detailsview-control"></a>Control DetailsView

El control DetailsView permite mostrar un único registro de un origen de datos y editarlo o eliminarlo. Se trata con más detalle en un módulo posterior.

## <a name="formview-control"></a>Control FormView

El control FormView se utiliza para mostrar un único registro de un origen de datos en una interfaz configurable. Se trata con más detalle en un módulo posterior.

## <a name="accessdatasource-control"></a>Control AccessDataSource

Se utiliza para el control AccessDataSource datos enlazar una base de datos de Access. Se trata con más detalle en un módulo posterior.

## <a name="objectdatasource-control"></a>Control ObjectDataSource

El control ObjectDataSource se usa para admitir una arquitectura de tres niveles, que controles pueden estar enlazado a datos a un objeto comercial de nivel medio en lugar de un modelo de dos niveles donde los controles se enlazan directamente al origen de datos. Se explicará con más detalle en un módulo posterior.

## <a name="xmldatasource-control"></a>Control XmlDataSource

El control XmlDataSource se utiliza para enlazar los datos a un origen de datos XML. Se trata con más detalle en un módulo posterior.

## <a name="sitemapdatasource-control"></a>Control SiteMapDataSource

El control SiteMapDataSource proporciona enlace de datos para controles de navegación de sitio basado en un mapa del sitio. Se explicará con más detalle en un módulo posterior.

## <a name="sitemappath-control"></a>Control SiteMapPath

El control SiteMapPath muestra una serie de vínculos de navegación que se conoce normalmente como rutas. Se trata con más detalle en un módulo posterior.

## <a name="menu-control"></a>Control Menu

El control de menú muestra menús dinámicos con DHTML. Se trata con más detalle en un módulo posterior.

## <a name="treeview-control"></a>TreeView (Control)

TreeView (control) se utiliza para mostrar una vista de árbol jerárquico de datos. Se trata con más detalle en un módulo posterior.

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona un mecanismo iniciar sesión en un sitio Web. Se trata con más detalle en un módulo posterior.

## <a name="loginview-control"></a>Control LoginView

El control LoginView permite la visualización de plantillas diferentes según el estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="passwordrecovery-control"></a>Control PasswordRecovery

El control PasswordRecovery se usa para recuperar contraseñas olvidadas por los usuarios de una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="loginstatus"></a>LoginStatus

El control LoginStatus muestra el estado de inicio de sesión de un usuario. Se trata con más detalle en un módulo posterior.

## <a name="loginname"></a>LoginName

El control LoginName muestra un nombre de usuario después de que se va a iniciar sesión en una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard es un asistente configurable que proporciona a los usuarios la capacidad para crear una cuenta de pertenencia a ASP.NET para su uso en una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="changepassword"></a>ChangePassword

El control ChangePassword permite a los usuarios cambiar su contraseña para una aplicación ASP.NET. Se trata con más detalle en un módulo posterior.

## <a name="various-webparts"></a>Varios elementos Web

ASP.NET 2.0 se suministra con varios elementos Web. Se explicará en detalle en un módulo posterior.
