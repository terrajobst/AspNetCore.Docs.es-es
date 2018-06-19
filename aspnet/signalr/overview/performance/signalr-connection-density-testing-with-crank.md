---
uid: signalr/overview/performance/signalr-connection-density-testing-with-crank
title: Densidad de la conexión de SignalR pruebas con cigüeñal | Documentos de Microsoft
author: tfitzmac
description: Densidad de la conexión de SignalR pruebas con cigüeñal
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/22/2015
ms.topic: article
ms.assetid: 148d9ca7-1af1-44b6-a9fb-91e261b9b463
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/performance/signalr-connection-density-testing-with-crank
msc.type: authoredcontent
ms.openlocfilehash: a3e8fdb47bd80d819358f9c48cd82fd51d6c0400
ms.sourcegitcommit: fe880bf4ed1c8116071c0e47c0babf3623b7f44a
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 11/21/2017
ms.locfileid: "26535334"
---
<a name="signalr-connection-density-testing-with-crank"></a>Densidad de la conexión de SignalR pruebas con cigüeñal
====================
por [Tom FitzMacken](https://github.com/tfitzmac)

> Este artículo describe cómo usar la herramienta cigüeñal para probar una aplicación con varios clientes simulados.


Una vez que la aplicación se ejecuta en su entorno de hospedaje (ya sea un Azure rol, IIS, web u hospeda a sí mismo con Owin), puede probar la respuesta de la aplicación a un alto nivel de densidad de la conexión con la herramienta cigüeñal. El entorno de hospedaje puede ser un servidor de Internet Information Services (IIS), un host de Owin o un rol web de Azure. (Nota: los contadores de rendimiento no están disponibles en aplicaciones de Web de servicio de aplicación de Azure, por lo que no podrán obtener datos de rendimiento de una prueba de densidad de la conexión.)

Densidad de conexión hace referencia al número de conexiones TCP simultáneas que pueden establecerse en un servidor. Cada conexión TCP conlleva su propia sobrecarga y abrir un gran número de conexiones inactivas para crear un cuello de botella de memoria.

[Codebase SignalR](https://github.com/signalr/signalr) incluye una herramienta de pruebas de carga denominada **de manivela**. La versión más reciente de cigüeñal puede encontrarse en [la bifurcación Dev](https://github.com/SignalR/signalr/tree/dev) en GitHub. Puede descargar un archivo Zip codebase de archivo de la bifurcación Dev de SignalR [aquí](https://github.com/SignalR/SignalR/archive/dev.zip).

Cigüeñal puede utilizarse para totalmente saturar la memoria del servidor con el fin de calcular el número total de conexiones inactivas posibles en el hardware del servidor. Como alternativa, también puede usar cigüeñal para prueba de carga el servidor con una cierta cantidad de presión de memoria, por refuerza conexiones hasta que se alcance un número concreto o un umbral de memoria concreta.

Al realizar pruebas, es importante usar clientes remotos para evitar cualquier competición por los recursos (es decir, las conexiones TCP y memoria). Supervisar los clientes para asegurarse de que no están alcanzando cuellos de botella que pueden impedir que el servidor de alcanzar su capacidad máxima (memoria o CPU). Puede que necesite aumentar el número de clientes con el fin de cargar totalmente el servidor.

### <a name="running-a-connection-density-test"></a>Ejecutar una prueba de densidad de la conexión

Esta sección describen los pasos necesarios para ejecutar una prueba de densidad de la conexión en una aplicación de SignalR.

1. Descargar y generar los [codebase bifurcación Dev de SignalR](https://github.com/SignalR/SignalR/archive/dev.zip). En un símbolo del sistema, vaya a &lt;directorio del proyecto&gt;\src\Microsoft.AspNet.SignalR.Crank\bin\debug.
2. Implementar la aplicación en su entorno de hospedaje previsto. Tome nota del punto de conexión que utiliza la aplicación; Por ejemplo, en la aplicación creada en el [tutorial de introducción](../getting-started/tutorial-getting-started-with-signalr.md), el extremo es `http://<yourhost>:8080/signalr`.
3. Instalar [contadores de rendimiento de SignalR](signalr-performance.md#perfcounters) en el servidor. Si la aplicación se ejecuta en Azure, consulte [uso de contadores de rendimiento de SignalR en un rol Web de Azure](using-signalr-performance-counters-in-an-azure-web-role.md).

Una vez que haya descargado y compila el código base e instala los contadores de rendimiento en el host, la herramienta de línea de comandos cigüeñal puede encontrarse en el `src\Microsoft.AspNet.SignalR.Crank\bin\Debug` carpeta.

Las opciones disponibles para la herramienta cigüeñal incluyen:

- **/?** : Muestra la pantalla de ayuda. También se muestran las opciones disponibles si el **Url** se omite el parámetro.
- **/ Dirección Url**: la dirección URL para las conexiones de SignalR. Este parámetro es necesario. Para una aplicación de SignalR mediante la asignación predeterminada, la ruta de acceso va a finalizar en "/ signalr".
- **/ Transporte**: el nombre del transporte utilizado. El valor predeterminado es `auto`, que se seleccionará el mejor protocolo disponible. Las opciones incluyen `WebSockets`, `ServerSentEvents`, y `LongPolling` (`ForeverFrame` no es una opción para cigüeñal, desde el cliente de .NET en lugar de Internet Explorer se usa). Para obtener más información sobre cómo SignalR selecciona transportes, consulte [transportes y reservas](../getting-started/introduction-to-signalr.md#transports).
- **/ BatchSize**: el número de clientes que se agregó en cada lote. El valor predeterminado es 50.
- **/ ConnectInterval**: el intervalo en milisegundos entre la adición de conexiones. El valor predeterminado es 500.
- **/ Las conexiones**: el número de conexiones que se usan para la aplicación de prueba de carga. El valor predeterminado es 100.000.
- **/ ConnectTimeout**: el tiempo de espera en segundos antes de anular la prueba. El valor predeterminado es 300.
- **MinServerMBytes**: el número de megabytes de servidor mínima para alcanzar. El valor predeterminado es 500.
- **SendBytes**: el tamaño de la carga enviada al servidor en bytes. El valor predeterminado es 0.
- **SendInterval**: el retraso en milisegundos entre los mensajes en el servidor. El valor predeterminado es 500.
- **SendTimeout**: el tiempo de espera en milisegundos para los mensajes en el servidor. El valor predeterminado es 300.
- **ControllerUrl**: la dirección Url donde un cliente va a hospedar un concentrador de controlador. El valor predeterminado es null (no hay ningún centro de controlador). El concentrador de controladores se inicia cuando se inicia la sesión cigüeñal; ningún otro contacto entre el concentrador (hub) y los cigüeñales se realiza.
- **NumClients**: el número de clientes simulados para conectarse a la aplicación. El valor predeterminado es uno.
- **Archivo de registro**: el nombre de archivo para el archivo de registro para la ejecución de pruebas. De manera predeterminada, es `crank.csv`.
- **SampleInterval**: el tiempo en milisegundos entre las muestras del contador de rendimiento. El valor predeterminado es 1000.
- **SignalRInstance**: el nombre de instancia para los contadores de rendimiento en el servidor. El valor predeterminado es usar el estado de conexión de cliente.

### <a name="example"></a>Ejemplo

El siguiente comando probará un sitio denominado `pfsignalr` en Azure que hospeda una aplicación en el puerto 8080 con un concentrador con el nombre "ControllerHub", usando 100 conexiones.

`crank /Connections:100 /Url:http://pfsignalr.cloudapp.net:8080/signalr`
