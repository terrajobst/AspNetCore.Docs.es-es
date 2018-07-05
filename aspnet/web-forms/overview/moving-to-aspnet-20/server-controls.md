---
uid: web-forms/overview/moving-to-aspnet-20/server-controls
title: Los controles de servidor | Microsoft Docs
author: microsoft
description: ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios de arquitectura para el modo en ASP.NET 2.0 y Visual Studio 200...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 43f6ac47-76fc-4cf7-8e9f-c18ce673dfd8
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/server-controls
msc.type: authoredcontent
ms.openlocfilehash: 989986c45e76a65582f35172c7d837a78484d09a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374229"
---
<a name="server-controls"></a>Controles de servidor
====================
por [Microsoft](https://github.com/microsoft)

> ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios de arquitectura a la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupa de los controles de servidor.


ASP.NET 2.0 mejora los controles de servidor de muchas maneras. En este módulo, trataremos algunos de los cambios de arquitectura a la forma en que ASP.NET 2.0 y Visual Studio 2005 se ocupa de los controles de servidor.

## <a name="view-state"></a>Estado de vista

El principal cambio de estado de vista en ASP.NET 2.0 es una reducción notable de tamaño. Considere la posibilidad de una página con un control de calendario en él. Este es el estado de vista en ASP.NET 1.1.

[!code-css[Main](server-controls/samples/sample1.css)]

Ahora este es el estado de vista en una página idéntica en ASP.NET 2.0.

[!code-css[Main](server-controls/samples/sample2.css)]

Es un cambio muy importante, y teniendo en cuenta que se lleve a estado de vista y hacia atrás a través del cable, este cambio puede brindar a los desarrolladores un aumento significativo del rendimiento. La reducción del tamaño del estado de vista es en gran medida debido al modo en que se administran internamente. Recuerde que la cadena codificada de la vista de estado es un Base64. Para comprender mejor el cambio de estado de vista en ASP.NET 2.0, echemos un vistazo a los valores descodificados de los ejemplos anteriores.

Este es el estado de 1.1 vista puede descodificado:

[!code-css[Main](server-controls/samples/sample3.css)]

Esto puede parecer un poco un galimatías pero hay un patrón aquí. En ASP.NET 1.x, se usa solo caracteres para identificar los tipos de datos y uso de valores delimitados por la &lt; &gt; caracteres. La "t" en el ejemplo de estado de vista anterior representa a una terna. La terna contiene un par de ArrayLists (la "l" representa una lista de matrices). Uno de esos ArrayLists contiene un valor Int32 ("i") con un valor de 1 y el otro contiene otro terna. La terna contiene un par de objetos ArrayList, etcetera. Lo importante que debe recordar es que usamos Triplets que contienen pares, se identifican los tipos de datos a través de una letra y usamos el &lt; y &gt; caracteres como delimitadores.

En ASP.NET 2.0, el estado de vista descodificado parece un poco diferente.

[!code-powershell[Main](server-controls/samples/sample4.ps1)]

Debe observar un enorme cambio en el aspecto del estado de vista descodificado. Este cambio tiene varios puntales arquitectónicos. Estado de vista en ASP.NET 1.x usa el LosFormatter para serializar los datos. En 2.0, se usa la nueva clase ObjectStateFormatter. Esta clase se diseñó específicamente para ayudar en la serialización y deserialización del estado de vista y el estado de control. (El estado de control se tratarán en la sección siguiente). Hay muchas ventajas obtenidas al cambiar el método mediante el cual serialización y deserialización tienen lugar. Uno de los más importantes es el hecho de que, a diferencia de la LosFormatter que usa un TextWriter, el ObjectStateFormatter usa BinaryWriter. Esto permite que ASP.NET 2.0 almacenar el estado de vista una serie de bytes en lugar de cadenas. Por ejemplo, tome un entero. En ASP.NET 1.1, un entero requiere 4 bytes de estado de vista. En ASP.NET 2.0, ese mismo entero requiere solo 1 byte. Se han realizado otras mejoras para reducir la cantidad de estado de vista que se almacena. Los valores de fecha y hora, por ejemplo, ahora se almacenan con un TickCount en lugar de una cadena.

Como si todo eso no fuese suficiente, se presta atención especial al hecho de que uno de los mayores consumidores de estado de vista en la versión 1.x era de la cuadrícula de datos y controles similares. Un gran inconveniente de controles, como la cuadrícula de datos donde se ocupa el estado de vista es que a menudo contiene grandes cantidades de información repetida. En ASP.NET 1.x, que repite simplemente se almacenó información a través y lo que resulta en un estado de vista recargada. En ASP.NET 2.0, se usa la nueva clase IndexedString para almacenar estos datos. Si se repite una cadena, simplemente se almacena el token para el IndexedString y el índice dentro de una tabla de objetos IndexedString ejecución.

## <a name="control-state"></a>Estado de control

Uno de los principales problemas que los desarrolladores tenían con estado de vista era el tamaño que agrega a la carga HTTP. Como se mencionó anteriormente, uno de los mayores consumidores de estado de vista es el control DataGrid. Para evitar las grandes cantidades de estado de vista generada por un control DataGrid, muchos desarrolladores simplemente deshabilitado el estado de vista para ese control. Lamentablemente, esta solución no era siempre una buena. Estado de vista en ASP.NET 1.x contiene no solo los datos necesarios para la correcta funcionalidad del control. También contiene información sobre el estado de la interfaz de usuario del control. Esto significa que, si desea permitir para la paginación en un control DataGrid, debe habilitar el estado de vista incluso si no necesita toda la información de la interfaz de usuario que ve contiene estado. Es un escenario de todo o nada.

En ASP.NET 2.0, el estado de control resuelve el problema bien a través de la introducción del estado de control. Estado del control contiene los datos que sea absolutamente necesarios para el correcto funcionamiento de un control. A diferencia del estado de vista, no se puede deshabilitar el estado de control. Por lo tanto, es importante que los datos que se almacena en el estado de control se controlan con cuidado.

> [!NOTE]
> Se conserva el estado de control junto con el estado de vista en el \_ \_campo de formulario oculto de VIEWSTATE.


Este vídeo es un tutorial sobre el estado de vista y estado de control.


![](server-controls/_static/image1.png)


[Abra vídeo de pantalla completa](server-controls/_static/state1.wmv)


En el orden de un control de servidor leer y escribir para controlar el estado, debe realizar tres pasos.

## <a name="step-1-call-the-registerrequirescontrolstate-method"></a>Paso 1: Llame al método RegisterRequiresControlState

El método RegisterRequiresControlState informa a ASP.NET que un control debe conservar el estado de control. Toma un argumento de tipo de Control que es el control que se va a registrar.

Es importante tener en cuenta que el registro no se conserva para solicitar. Por lo tanto, este método debe llamarse en cada solicitud, si es un control para conservar el estado de control. Se recomienda que se llama al método en OnInit.

[!code-csharp[Main](server-controls/samples/sample5.cs)]

## <a name="step-2-override-savecontrolstate"></a>Paso 2: Reemplazar SaveControlState

El método SaveControlState guarda los cambios de estado para un control de control desde el último postback. Devuelve un objeto que representa el estado del control.

## <a name="step-3-override-loadcontrolstate"></a>Paso 3: Invalidar LoadControlState

El método LoadControlState carga el estado guardado en un control. El método toma un argumento de tipo Object que contiene el estado guardado del control.

## <a name="full-xhtml-compliance"></a>Compatibilidad con XHTML completa

Cualquier desarrollador Web sabe la importancia de los estándares en aplicaciones Web. Para mantener un entorno de desarrollo basado en estándares, ASP.NET 2.0 es totalmente compatible con XHTML. Por lo tanto, todas las etiquetas son representado según los estándares de XHTML en exploradores que admiten HTML 4.0 o superior.

La definición de tipo de documento en ASP.NET 1.1 fue el siguiente:

[!code-html[Main](server-controls/samples/sample6.html)]

En ASP.NET 2.0, la definición de tipo de documento predeterminada es como sigue:

[!code-html[Main](server-controls/samples/sample7.html)]

Si elige, puede modificar el cumplimiento de XHML predeterminada a través del nodo xhtmlConformance en el archivo de configuración. Por ejemplo, el siguiente nodo en el archivo web.config cambiará el cumplimiento de XHTML a XHTML 1.0 Strict:

[!code-xml[Main](server-controls/samples/sample8.xml)]

Si elige, también puede configurar ASP.NET para usar la configuración heredada que se usa en ASP.NET 1.x como sigue:

[!code-xml[Main](server-controls/samples/sample9.xml)]

## <a name="adaptive-rendering-using-adapters"></a>Adaptación de procesamiento con adaptadores

En ASP.NET 1.x, el archivo de configuración contenido un &lt;browserCaps&gt; sección que rellena un objeto HttpBrowserCapabilities. Este objeto permite que un desarrollador determinar qué dispositivo está realizando una solicitud determinada y representar código adecuadamente. En ASP.NET 2.0, el modelo ha mejorado y ahora usa esta nueva clase. Esta clase invalida los eventos de ciclo de vida del control y controla la representación de controles en función de las capacidades del agente de usuario. Las capacidades de un agente de usuario concreto se definen mediante un archivo de definición de explorador (un archivo con una extensión de archivo .browser) almacenado en el c:\windows\microsoft.net\framework\v2.0. \* \* \* \*Carpeta \CONFIG\Browsers.

> [!NOTE]
> Esta clase es una clase abstracta.


De forma similar a la &lt;browserCaps&gt; sección en la versión 1.x, el archivo de definición de explorador usa una expresión Regular para analizar la cadena de agente de usuario con el fin de identificar el explorador solicitante. Se les define funciones concretas para que el agente de usuario. ControlAdapter representa el control mediante el método Render. Por lo tanto, si invalida el método Render, no debe llamar Render en la clase base. Si lo hace, para que se realicen dos veces, una vez para el adaptador y otra para el propio control de representación.

## <a name="developing-a-custom-adapter"></a>Desarrollar un adaptador personalizado

Puede desarrollar un adaptador personalizado propio heredando de ControlAdapter. Además, puede heredar de la clase abstracta PageAdapter en casos donde se necesita un adaptador para una página. Asignación de controles para el adaptador personalizado se realiza a través de la &lt;controlAdapters&gt; elemento en el archivo de definición de explorador. Por ejemplo, el siguiente código XML desde un archivo de definición de explorador asigna el control de menú a la clase MenuAdapter:

[!code-html[Main](server-controls/samples/sample10.html)]

Con este modelo, resulta bastante fácil para un desarrollador de control para tener como destino un determinado dispositivo o explorador. También es bastante sencillo para que un desarrollador tiene control completo sobre cómo se representan las páginas en todos los dispositivos.

## <a name="per-device-rendering"></a>Representación por dispositivo

Propiedades de control de servidor en ASP.NET 2.0 pueden ser especificada utilizando un prefijo específico del explorador por dispositivo. Por ejemplo, el código siguiente cambiará el texto de una etiqueta según el dispositivo al que se utiliza para examinar la página.

[!code-aspx[Main](server-controls/samples/sample11.aspx)]

Cuando la página que contengan esta etiqueta se examinarán desde Internet Explorer, la etiqueta mostrará el texto que dice "Está explorando desde Internet Explorer". Cuando se examina la página de Firefox, la etiqueta mostrará el texto "Está explorando desde Firefox". Cuando la página se examinarán desde cualquier otro dispositivo, se mostrará "Está explorando desde un dispositivo desconocido". Con esta sintaxis especial, se puede especificar cualquier propiedad.

## <a name="setting-focus"></a>Establecer el foco

Los desarrolladores de ASP.NET 1.x más frecuentes acerca de cómo establecer el foco inicial en un control determinado. Por ejemplo, en una página de inicio de sesión, es útil disponer de recibir el foco cuando la página se carga por primera vez el cuadro de texto Id. de usuario. En ASP.NET 1.x, esto requiere escribir algún script del lado cliente. Aunque este tipo de script es una tarea trivial, ya no es necesario en ASP.NET 2.0 gracias al método SetFocus. El método SetFocus toma un argumento que indica el control que debe recibir el foco. Este argumento puede ser el identificador de cliente del control como una cadena o el nombre del control de servidor como un objeto de Control. Por ejemplo, para establecer el foco inicial en un control de cuadro de texto denominado txtUserID cuando la página se carga por primera vez, agregue el código siguiente a la página\_carga:

[!code-csharp[Main](server-controls/samples/sample12.cs)]

--o

[!code-csharp[Main](server-controls/samples/sample13.cs)]

ASP.NET 2.0 utiliza el controlador Webresource.axd (tratado anteriormente) para representar una función del lado cliente que establece el foco. El nombre de la función del lado cliente es WebForm\_AutoFocus como se muestra aquí:

[!code-html[Main](server-controls/samples/sample14.html)]

Como alternativa, puede usar el método de foco para un control para establecer el foco inicial a ese control. El método de foco se deriva de la clase de Control y está disponible para todos los controles de ASP.NET 2.0. También es posible establecer el foco en un control determinado cuando se produce un error de validación. Que se tratarán en un módulo posterior.

## <a name="new-server-controls-in-aspnet-20"></a>Nuevos controles de servidor en ASP.NET 2.0

Los siguientes son nuevos controles de servidor en ASP.NET 2.0. Describiremos con más detalle en algunas de ellas en módulos posteriores.

## <a name="imagemap-control"></a>Control de mapa de imágenes

El control de mapa de imágenes permite agregar puntos de conexión a una imagen que se puede iniciar una devolución de datos o navegar a una dirección URL. Hay tres tipos de puntos de conexión disponibles. CircleHotSpot, RectangleHotSpot y PolygonHotSpot. Puntos de conexión se agregan a través de un editor de colecciones en Visual Studio o mediante programación en el código. No hay ninguna interfaz de usuario disponibles para dibujar los puntos de conexión en una imagen. Las coordenadas y el tamaño o el radio de la zona activa se debe especificar mediante declaración. No hay ninguna representación visual de un punto de conexión en el diseñador. Si un punto de conexión está configurado para navegar a una dirección URL, se especifica la dirección URL a través de la propiedad NavigateUrl de zona activa. En el caso de una publicación de realizar una copia de zona activa, el PostBackValue propiedad le permite pasar una cadena en la devolución de llamada que se puede recuperar en el código del lado servidor.


![Editor de colección de hotSpot en Visual Studio](server-controls/_static/image1.jpg)

**Figura 1**: Editor de colección de HotSpot en Visual Studio


## <a name="bulletedlist-control"></a>Control BulletedList

El control BulletedList es una lista con viñetas que fácilmente se puede enlazar a datos. La lista puede ordenarse (numerados) o sin ordenar de a través de la propiedad BulletStyle. Cada elemento de la lista se representa mediante un objeto ListItem.


![Control BulletedList en Visual Studio](server-controls/_static/image1.gif)

**Figura 2**: Control BulletedList en Visual Studio


## <a name="hiddenfield-control"></a>Control HiddenField

El control HiddenField agrega un campo de formulario oculto a la página, el valor de los cuales está disponible en el código del lado servidor. Por lo general se espera que el valor de un campo de formulario oculto permanecen sin cambios entre devoluciones de datos. Sin embargo, es posible que un usuario malintencionado cambiar el antes de valor para registrar back. Si esto ocurre, el control HiddenField generará el evento ValueChanged. Si tiene información confidencial en el control HiddenField y desea asegurarse de que permanecen sin cambios, debe controlar el evento ValueChanged en el código.

## <a name="fileupload-control"></a>Control fileUpload

El control FileUpload en ASP.NET 2.0 permite cargar archivos en un servidor Web a través de una página ASP.NET. Este control es bastante similar a la clase ASP.NET 1.x HtmlInputFile con unas pocas excepciones. En ASP.NET 1.x, se recomienda que la propiedad PostedFile comprobarse valores NULL con el fin de determinar si tenía un archivo buena. El control FileUpload en ASP.NET 2.0 agrega una nueva propiedad HasFile que puede usar para la misma finalidad y es un poco más eficaz.

La propiedad PostedFile sigue estando disponible para el acceso a un objeto HttpPostedFile, pero algunas de las funcionalidades de la HttpPostedFile ahora está disponible intrínsecamente con el control FileUpload. Por ejemplo, para guardar un archivo cargado en ASP.NET 1.x, llamar al método SaveAs en el objeto HttpPostedFile. Mediante el control FileUpload en ASP.NET 2.0, llamar al método SaveAs en el propio control FileUpload.

Otro cambio significativo en el comportamiento 2.0 (y probablemente el cambio más significativo) es que ya no es necesario cargar un archivo cargado todo en memoria antes de guardarlo. En la versión 1.x, se guarda cualquier archivo que se cargó completamente en memoria antes de que se escriben en el disco. Esta arquitectura evita la carga de archivos grandes.

En ASP.NET 2.0, el atributo requestLengthDiskThreshold del elemento httpRuntime le permite configurar cuántos Kilobytes se mantienen en un búfer en memoria antes de que se escriben en el disco.

**IMPORTANTE**: documentación de MSDN (y documentación en otros lugares) especifican que este valor está en bytes (no en Kilobytes) y que el valor predeterminado es 256. El valor realmente se especifica en Kilobytes y el valor predeterminado es 80. Al tener un valor predeterminado de 80 KB, nos aseguramos de que el búfer no terminen en el montón de objetos grandes.

## <a name="wizard-control"></a>Control de Asistente

Es bastante habitual encontrar los desarrolladores de ASP.NET debe enfrentarse al intentar recopilar información de una serie de "páginas" mediante paneles o mediante la transferencia de una página a otra. Más a menudo que no, el esfuerzo es frustrante y lleva mucho tiempo. El nuevo control de asistente resuelve los problemas permitiendo no lineal y pasos de una interfaz de asistente que están familiarizados con los usuarios. El control de asistente presenta los formularios de entrada en una serie de pasos. Cada paso es de un tipo concreto especificado por la propiedad StepType del control. Los tipos de pasos disponibles son los siguientes:

| **Tipo de paso** | **Explicación** |
| --- | --- |
| Automático | El asistente determina automáticamente el tipo de paso en función de su posición dentro de la jerarquía de paso. |
| Iniciar | El primer paso, a menudo se usa para presentar una instrucción de introducción. |
| Paso | Un paso normal. |
| Finalizar | El último paso, que normalmente se usa para presentar un botón para finalizar al asistente. |
| Completar | Presenta un mensaje de comunicar el éxito o error. |

> [!NOTE]
> El control de asistente realiza un seguimiento de su estado usando el estado de control ASP.NET. Por lo tanto, se puede establecer la propiedad EnableViewState en false sin perjuicio alguno.


Este vídeo es un tutorial sobre el control de asistente.


![](server-controls/_static/image2.png)


[Abra vídeo de pantalla completa](server-controls/_static/wizard1.wmv)


## <a name="localize-control"></a>Localizar el Control

El control Localize es similar a un control Literal. Sin embargo, el control Localize tiene un **modo** propiedad que controla cómo se representa el marcado que se agrega a él. La propiedad Mode admite los siguientes valores:

| **Modo** | **Explicación** |
| --- | --- |
| Transformación | Marcado se transforma según el protocolo del explorador que realiza la solicitud. |
| Acceso directo | Marcado se representa como-es. |
| Codificar | Marcado que se agrega al control se codifica mediante HtmlEncode. |

## <a name="multiview-and-view-controls"></a>MultiView y controles de vista

El control MultiView actúa como contenedor de controles de vista y el control de vista actúa como contenedor (como un control de Panel) para otros controles. Cada vista en un control MultiView se representa mediante un control de vista único. El primer control de vista en el MultiView es vista 0, la segunda vista 1, etcetera. Puede intercambiar las vistas mediante la especificación del control MultiView ActiveViewIndex.

## <a name="substitution-control"></a>Control de sustitución

El control de sustitución se usa junto con el almacenamiento en caché de ASP.NET. En casos donde desea aprovechar las ventajas del almacenamiento en caché, pero tiene las partes de una página que se deben actualizar en cada solicitud (en otras palabras, las partes de una página que están exentos de almacenamiento en caché), el componente de sustitución proporciona una excelente solución. El control en realidad no representa ningún resultado en su propio. En su lugar, está enlazado a un método en el código del lado servidor. Cuando se solicita la página, se llama al método y la marca devuelta se representa en lugar del control de sustitución.

El método al que está enlazado el control de sustitución se especifica a través de la **MethodName** propiedad. Ese método debe cumplir los siguientes criterios:

- Debe ser un método estático (compartido en VB).
- Acepta un parámetro de tipo HttpContext.
- Devuelve una cadena que representa el marcado que se debe reemplazar el control en la página.

El control de sustitución no tiene la capacidad de modificar cualquier otro control en la página, pero tiene acceso a HttpContext actual mediante su parámetro.

## <a name="gridview-control"></a>Control GridView

El control GridView es el reemplazo para el control DataGrid. Este control se tratarán con más detalle en un módulo posterior.

## <a name="detailsview-control"></a>Control DetailsView

El control DetailsView permite mostrar un único registro de un origen de datos y editarlo o eliminarlo. Se trata en detalle en un módulo posterior.

## <a name="formview-control"></a>Control FormView

El control FormView se usa para mostrar un único registro de un origen de datos en una interfaz configurable. Se trata en detalle en un módulo posterior.

## <a name="accessdatasource-control"></a>Control AccessDataSource

El control AccessDataSource se usa para enlazar datos de una base de datos de Access. Se trata en detalle en un módulo posterior.

## <a name="objectdatasource-control"></a>Control ObjectDataSource

Se utiliza el control ObjectDataSource para admitir una arquitectura de tres niveles para que los controles pueden ser enlazados a datos a un objeto de negocios de nivel intermedio en lugar de un modelo de dos niveles donde los controles se enlazan directamente al origen de datos. Se tratarán con más detalle en un módulo posterior.

## <a name="xmldatasource-control"></a>Control XmlDataSource

El control XmlDataSource se utiliza para enlazar datos a un origen de datos XML. Se trata en detalle en un módulo posterior.

## <a name="sitemapdatasource-control"></a>Control SiteMapDataSource

El control SiteMapDataSource proporciona el enlace de datos para controles de navegación del sitio según un mapa del sitio. Se tratarán con más detalle en un módulo posterior.

## <a name="sitemappath-control"></a>Control SiteMapPath

El control SiteMapPath muestra una serie de vínculos de navegación que se conoce comúnmente como rutas de navegación. Se trata en detalle en un módulo posterior.

## <a name="menu-control"></a>Control Menu

El control de menú muestra menús dinámicos con DHTML. Se trata en detalle en un módulo posterior.

## <a name="treeview-control"></a>TreeView (Control)

TreeView (control) se utiliza para mostrar una vista de árbol jerárquica de los datos. Se trata en detalle en un módulo posterior.

## <a name="login-control"></a>Control de inicio de sesión

El control de inicio de sesión proporciona un mecanismo iniciar sesión en un sitio Web. Se trata en detalle en un módulo posterior.

## <a name="loginview-control"></a>Control LoginView

El control LoginView permite la visualización de plantillas diferentes según el estado de inicio de sesión del usuario. Se trata en detalle en un módulo posterior.

## <a name="passwordrecovery-control"></a>Control PasswordRecovery

El control PasswordRecovery sirve para recuperar contraseñas olvidadas por los usuarios de una aplicación ASP.NET. Se trata en detalle en un módulo posterior.

## <a name="loginstatus"></a>LoginStatus

El control LoginStatus muestra el estado de inicio de sesión del usuario. Se trata en detalle en un módulo posterior.

## <a name="loginname"></a>LoginName

El control LoginName muestra un nombre de usuario después de que se va a iniciar sesión en una aplicación ASP.NET. Se trata en detalle en un módulo posterior.

## <a name="createuserwizard"></a>CreateUserWizard

CreateUserWizard es un asistente configurable que les ofrece la capacidad para crear una cuenta de pertenencia a ASP.NET para su uso en una aplicación ASP.NET. Se trata en detalle en un módulo posterior.

## <a name="changepassword"></a>ChangePassword

El control ChangePassword permite a los usuarios cambiar su contraseña para una aplicación ASP.NET. Se trata en detalle en un módulo posterior.

## <a name="various-webparts"></a>Diversos elementos de Web

ASP.NET 2.0 incluye varios elementos Web. Se tratarán en detalle en un módulo posterior.
