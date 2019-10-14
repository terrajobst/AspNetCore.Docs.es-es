# <a name="upload-files-sample-app"></a>Aplicación de ejemplo de carga de archivos

En esta aplicación de ejemplo se muestran los conceptos descritos en el tema [Carga de archivos en ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="security-considerations"></a>Consideraciones de seguridad

Tenga precaución al proporcionar a los usuarios la capacidad de cargar archivos en un servidor. Los atacantes pueden ejecutar ataques por [denegación de servicio](/windows-hardware/drivers/ifs/denial-of-service), intentar cargar virus o malware o intentar poner en riesgo redes y servidores de otras maneras.

Estos son algunos de los pasos de seguridad con los que se reduce la probabilidad de sufrir ataques:

* Cargue los archivos a un área de carga de archivos dedicada del sistema, de preferencia una unidad que no sea de sistema. El uso de una ubicación dedicada permite imponer de manera más sencilla medidas de seguridad en los archivos cargados. Deshabilite la ejecución de los permisos en la ubicación de carga de archivos.&dagger;
* Los archivos cargados nunca deben persistir en el mismo árbol de directorio que la aplicación.&dagger;
* Use un nombre de archivo seguro determinado por la aplicación. No use un nombre de archivo proporcionado por una entrada del usuario ni el nombre de archivo que no es de confianza del archivo cargado.&dagger;
* Permita únicamente un conjunto específico de extensiones de archivo aprobadas.&dagger;
* Compruebe la firma de formato de archivo para evitar que un usuario cargue un archivo enmascarado (por ejemplo, la carga de un archivo *.exe* con una extensión *.txt*).&dagger;
* Compruebe que también se llevan a cabo comprobaciones de cliente en el servidor. Las comprobaciones de cliente son fáciles de sortear.&dagger;
* Compruebe el tamaño de la carga y evite las cargas de mayor tamaño de lo esperado.&dagger;
* Cuando un archivo cargado con el mismo nombre no deba sobrescribir los archivos, vuelva a comprobar el nombre de archivo en la base de datos o en el almacenamiento físico antes de cargarlo.
* **Ejecute un detector de virus o malware en el contenido cargado antes de que se almacene el archivo.**

&dagger;La aplicación de ejemplo muestra un enfoque que cumple los criterios.

> [!WARNING]
> La carga de código malintencionado en un sistema suele ser el primer paso para ejecutar código que puede:
>
> * Tomar todo el poder en un sistema.
> * Sobrecargar un sistema de manera que el sistema se bloquea.
> * Poner en peligro los datos del usuario o del sistema.
> * Aplicar grafitis a una interfaz de usuario pública.
>
> Vea los siguientes recursos para más información sobre cómo reducir el área expuesta de ataques al aceptar archivos de los usuarios:
>
> * [Unrestricted File Upload](https://www.owasp.org/index.php/Unrestricted_File_Upload) (Carga de archivos sin restricciones)
> * [Azure Security: Asegúrese de que los controles adecuados estén en vigor al aceptar archivos de usuarios](/azure/security/azure-security-threat-modeling-tool-input-validation#controls-users)

Para información adicional, consulte [Carga de archivos en ASP.NET Core](https://docs.microsoft.com/aspnet/core/mvc/models/file-uploads).

## <a name="how-to-use-the-sample"></a>Uso del ejemplo

En el archivo *appsettings.json*:

1. Establezca la ruta de acceso de los archivos almacenados (`StoredFilesPath`).

   * La aplicación de ejemplo establece el valor en `c:\\files`, que presupone que una carpeta denominada *files* existe en la raíz de la unidad C: del sistema.
   * Debe existir la ruta de acceso. Cree una carpeta *files* en la unidad C: del sistema o establezca la ruta de acceso en una ubicación adecuada.
   * El proceso de la aplicación requiere permisos de lectura y escritura sobre la ruta de acceso.
   * **IMPORTANTE** Deshabilite los permisos de ejecución para todos los usuarios en la ruta de acceso.

1. Establezca el límite de tamaño de archivo (`FileSizeLimit`) en bytes. El valor predeterminado de la aplicación de ejemplo de `2097152` (2 097 152 bytes) permite cargas de archivo de hasta 2 MB.
