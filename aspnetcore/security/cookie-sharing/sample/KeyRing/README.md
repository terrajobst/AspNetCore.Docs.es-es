# <a name="data-protection-key-folder"></a>Carpeta de clave de protección de datos

Este archivo es un marcador de posición para crear una carpeta compartida para las claves de protección de datos.

En una implementación de producción, coloque las claves fuera de la raíz de desarrollo y nunca en el repositorio los archivos en este directorio en el control de código fuente. Proteger las claves de protección de datos en los archivos con DPAPI o un X509Certificate.

Consulte [protección de datos en ASP.NET Core: API de consumidor, configuración, API de extensibilidad e implementación](https://docs.microsoft.com/aspnet/core/security/data-protection/) para obtener más información.
