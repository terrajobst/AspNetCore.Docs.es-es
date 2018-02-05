---
uid: web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
title: Perfiles, temas y elementos Web | Documentos de Microsoft
author: microsoft
description: "Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan pr..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 92df4051-77c6-492c-bd34-23d24189cea4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/profiles-themes-and-web-parts
msc.type: authoredcontent
ms.openlocfilehash: 2c6ba11799a5a9be3d8c0037fad5d79d8177c0e8
ms.sourcegitcommit: d8aa1d314891e981460b5e5c912afb730adbb3ad
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 02/05/2018
---
<a name="profiles-themes-and-web-parts"></a>Perfiles, temas y elementos Web
====================
por [Microsoft](https://github.com/microsoft)

> Hay cambios importantes en la configuración y la instrumentación en ASP.NET 2.0. La nueva API de configuración de ASP.NET permite cambios de configuración que se realizan mediante programación. Además, existen muchas nuevas opciones de configuración Permitir nuevas configuraciones e instrumentación de.


ASP.NET 2.0 representa una mejora considerable en el área de sitios Web personalizados. Además de las características que pertenencia hayamos ya cubierto, los perfiles de ASP.NET, los temas y elementos Web mejoran de forma considerable personalización en sitios Web.

## <a name="aspnet-profiles"></a>Perfiles de ASP.NET

Los perfiles ASP.NET son similares a las sesiones. La diferencia es que un perfil es persistente, mientras que una sesión se pierde cuando se cierra el explorador. Otra diferencia importante entre las sesiones y perfiles es que los perfiles están fuertemente tipados, por lo tanto, proporciona con IntelliSense durante el proceso de desarrollo.

Un perfil se define en el archivo de configuración de máquinas o el archivo web.config para la aplicación. (No se puede definir un perfil en un archivo web.config de las subcarpetas). El código siguiente define un perfil para almacenar los visitantes del sitio Web en primer lugar y el apellido.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample1.xml)]

El tipo de datos predeterminado para una propiedad de perfil es System.String. En el ejemplo anterior, se ha especificado ningún tipo de datos. Por lo tanto, las propiedades FirstName y LastName son de tipo String. Como se mencionó anteriormente, perfil de propiedades están fuertemente tipadas. El código siguiente agrega una nueva propiedad para una duración que es de tipo Int32.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample2.xml)]

Perfiles suelen usar con autenticación de formularios de ASP.NET. Cuando se utiliza en combinación con la autenticación de formularios, cada usuario tiene un perfil independiente asociado con su identificador de usuario. Sin embargo, también es posible para permitir el uso de perfiles en una aplicación anónima mediante el &lt;anonymousIdentification&gt; elemento en el archivo de configuración junto con el **allowAnonymous** atributo como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample3.xml)]

Cuando un usuario anónimo explora el sitio, ASP.NET crea una instancia de **ProfileCommon** para el usuario. Este perfil utiliza un identificador único que se almacena en una cookie en el explorador para identificar al usuario como un visitante único. De esta manera, puede almacenar información de perfil para los usuarios que están explorando de forma anónima.

## <a name="profile-groups"></a>Grupos de perfiles

Es posible a las propiedades de grupo de perfiles. Propiedades de agrupación, es posible simular varios perfiles para una aplicación específica.

La configuración siguiente define una propiedad FirstName y LastName para dos grupos; Los compradores y posibles clientes.

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample4.xml)]

A continuación, es posible establecer propiedades en un grupo determinado, como se indica a continuación:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample5.cs)]

## <a name="storing-complex-objects"></a>Almacenar los objetos complejos

Hasta ahora, los ejemplos que hemos analizado almacenan tipos de datos simples en un perfil. También es posible almacenar tipos de datos complejos en un perfil especificando el método de serialización mediante el **serializeAs** atributo como se indica a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample6.xml)]

En este caso, el tipo es PurchaseInvoice. La clase PurchaseInvoice debe estar marcado como serializable y puede contener cualquier número de propiedades. Por ejemplo, si PurchaseInvoice tiene una propiedad denominada **NumItemsPurchased**, puede hacer referencia a esa propiedad en el código como sigue:

[!code-css[Main](profiles-themes-and-web-parts/samples/sample7.css)]

## <a name="profile-inheritance"></a>Herencia de perfil

Es posible crear un perfil para su uso en varias aplicaciones. Mediante la creación de una clase de perfil que se deriva de ProfileBase, puede volver a usar un perfil en varias aplicaciones mediante el uso de la **hereda** atributo tal y como se muestra a continuación:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample8.xml)]

En este caso, la clase **PurchasingProfile** sería así:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample9.cs)]

## <a name="profile-providers"></a>Proveedores de perfiles

Los perfiles de ASP.NET utilizan el modelo de proveedor. El proveedor predeterminado almacena la información en una base de datos de SQL Server Express en la aplicación\_carpeta de datos de la aplicación Web utilizando el proveedor de SqlProfileProvider. Si no existe la base de datos, ASP.NET lo creará automáticamente cuando el perfil intenta almacenar información.

En algunos casos, sin embargo, puede desarrollar su propio proveedor de perfil. La característica de perfiles ASP.NET permite utilizar fácilmente distintos proveedores.

Crear un proveedor de perfil personalizado cuando:

- Se necesita almacenar información de perfil en un origen de datos, como en una base de datos de FoxPro o en una base de datos de Oracle, no es compatible con los proveedores de perfiles incluidos con .NET Framework.
- Debe administrar la información de perfil mediante un esquema de base de datos que es diferente del esquema de base de datos utilizado por los proveedores incluidos con .NET Framework. Un ejemplo común es que desea integrar información de perfil con los datos de usuario en una base de datos de SQL Server existente.

### <a name="required-classes"></a>Clases necesarias

Para implementar un proveedor de perfil, cree una clase que hereda la clase abstracta System.Web.Profile.ProfileProvider. El **ProfileProvider** clase abstracta a su vez hereda la clase abstracta System.Configuration.SettingsProvider, que hereda la clase abstracta System.Configuration.Provider.ProviderBase. Debido a esta cadena de herencia, además de los miembros necesarios de la **ProfileProvider** (clase), debe implementar los miembros necesarios de la **SettingsProvider** y  **ProviderBase** clases.

Las tablas siguientes describen las propiedades y métodos que debe implementar desde el **ProviderBase**, **SettingsProvider**, y **ProfileProvider** abstracta clases.

### <a name="providerbase-members"></a>Miembros de ProviderBase

| **Miembro** | **Descripción** |
| --- | --- |
| Initialize (método) | Toma como entrada el nombre de la instancia del proveedor y un NameValueCollection de valores de configuración. Se usa para establecer opciones y los valores de propiedad para la instancia del proveedor, incluidos los valores específicos de la implementación y las opciones especificadas en la configuración de la máquina o el archivo Web.config. |

### <a name="settingsprovider-members"></a>Miembros de SettingsProvider

| **Miembro** | **Descripción** |
| --- | --- |
| Propiedad ApplicationName | El nombre de la aplicación que se almacena con cada perfil. El proveedor de perfil usa el nombre de la aplicación para almacenar información de perfil por separado para cada aplicación. Esto permite que varias aplicaciones de ASP.NET usar el mismo origen de datos sin un conflicto si se crea el mismo nombre de usuario en aplicaciones diferentes. O bien, varias aplicaciones ASP.NET pueden compartir un origen de datos de perfil especificando el mismo nombre de aplicación. |
| Omitiendo GetPropertyValues (método) | Toma como entrada un SettingsContext y un objeto SettingsPropertyCollection. El **SettingsContext** proporciona información sobre el usuario. Puede utilizar la información como una clave principal para recuperar información de la propiedad de perfil para el usuario. Use la **SettingsContext** objeto que se va a obtener el nombre de usuario y si el usuario está autenticado o anónimo. El **SettingsPropertyCollection** contiene una colección de objetos SettingsProperty. Cada **SettingsProperty** objeto proporciona el nombre y el tipo de propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El **omitiendo GetPropertyValues** método rellena una SettingsPropertyValueCollection con objetos SettingsPropertyValue basándose en la **SettingsProperty** objetos proporcionados como entrada. Los valores del origen de datos para el usuario especificado se asignan a las propiedades de PropertyValue para cada **SettingsPropertyValue** toda la colección se devolvió un objeto. Una llamada al método, también actualiza el valor de LastActivityDate para el perfil de usuario especificado para la fecha y hora actuales. |
| SetPropertyValues (método) | Toma como entrada un **SettingsContext** y un **SettingsPropertyValueCollection** objeto. El **SettingsContext** proporciona información sobre el usuario. Puede utilizar la información como una clave principal para recuperar información de la propiedad de perfil para el usuario. Use la **SettingsContext** objeto que se va a obtener el nombre de usuario y si el usuario está autenticado o anónimo. El **SettingsPropertyValueCollection** contiene una colección de **SettingsPropertyValue** objetos. Cada **SettingsPropertyValue** objeto proporciona el nombre, el tipo y el valor de la propiedad, así como información adicional, como el valor predeterminado de la propiedad y si la propiedad es de solo lectura. El **SetPropertyValues** método actualiza los valores de propiedad de perfil en el origen de datos para el usuario especificado. Una llamada al método también las actualizaciones de la **LastActivityDate** y LastUpdatedDate valores para el perfil de usuario especificado para la fecha y hora actuales. |

### <a name="profileprovider-members"></a>Miembros de ProfileProvider

| **Miembro** | **Descripción** |
| --- | --- |
| DeleteProfiles (método) | Toma como entrada una matriz de cadenas de usuario, nombres y elimina del origen de datos de todos los valores de propiedad y la información de perfil para los nombres especificados que coincide con el nombre de la aplicación la **ApplicationName** valor de propiedad. Si el origen de datos admite transacciones, se recomienda que incluya todas las operaciones de eliminación en una transacción y que revierta la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| DeleteProfiles (método) | Toma como entrada una colección de ProfileInfo objetos y todos los valores de propiedad y la información de perfil para cada perfil elimina del origen de datos que coincide con el nombre de la aplicación la **ApplicationName** valor de propiedad. Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| DeleteInactiveProfiles (método) | Toma como entrada un valor ProfileAuthenticationOption y toda la información de perfil del origen de un objeto DateTime y eliminaciones de los datos y valores de propiedad donde la última fecha de actividad es menor o igual que la fecha y hora especificadas y el nombre de la aplicación coincide con el **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles están va a eliminar. Si el origen de datos admite transacciones, se recomienda incluir todas las operaciones de eliminación en una transacción y revertir la transacción y producir una excepción si se produce un error en cualquier operación de eliminación. |
| GetAllProfiles (método) | Toma como entrada un **ProfileAuthenticationOption** valor, un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el número total de perfiles. Devuelve un ProfileInfoCollection que contiene **ProfileInfo** objetos para todos los perfiles del origen de datos que coincide con el nombre de la aplicación la **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles se va a devolver. Los resultados devueltos por la **GetAllProfiles** método están limitados por los valores de tamaño de página y el índice de la página. El valor de tamaño de página especifica el número máximo de **ProfileInfo** objetos que se va a devolver en la **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados que se va a devolver, donde 1 representa la primera página. El parámetro para el número total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles de la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, la **ProfileInfoCollection** devuelto contendrá del sexto al décimo perfil. Cuando el método vuelve, el valor del número total de registros se establece en 13. |
| GetAllInactiveProfiles (método) | Toma como entrada un **ProfileAuthenticationOption** valor, un **DateTime** objeto, un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá Para obtener el número total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles del origen de datos donde la última fecha de actividad es menor o igual que el especificado **DateTime**  y donde el nombre de aplicación coincide con el **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles se va a devolver. Los resultados devueltos por la **GetAllInactiveProfiles** método están limitados por los valores de tamaño de página y el índice de la página. El valor de tamaño de página especifica el número máximo de **ProfileInfo** objetos que se va a devolver en la **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados que se va a devolver, donde 1 representa la primera página. El parámetro para el número total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles de la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, la **ProfileInfoCollection** devuelto contendrá del sexto al décimo perfil. Cuando el método vuelve, el valor del número total de registros se establece en 13. |
| FindProfilesByUserName (método) | Toma como entrada un **ProfileAuthenticationOption** valor, una cadena que contiene un nombre de usuario, un entero que especifica el índice de la página, un entero que especifica el tamaño de página y una referencia a un entero que se establecerá en el número total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles de los datos de origen donde el nombre de usuario coincide con el nombre de usuario especificado y que coincide con el nombre de la aplicación el **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles se va a devolver. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar más amplias capacidades de búsqueda para nombres de usuario. Los resultados devueltos por la **FindProfilesByUserName** método están limitados por los valores de tamaño de página y el índice de la página. El valor de tamaño de página especifica el número máximo de **ProfileInfo** objetos que se va a devolver en la **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados que se va a devolver, donde 1 representa la primera página. El parámetro para el número total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles de la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, la **ProfileInfoCollection** devuelto contendrá del sexto al décimo perfil. Cuando el método vuelve, el valor del número total de registros se establece en 13. |
| FindInactiveProfilesByUserName (método) | Toma como entrada un **ProfileAuthenticationOption** valor, una cadena que contiene un nombre de usuario, un **DateTime** objeto, un entero que especifica el índice de la página, un entero que especifica el tamaño de página y un hacer referencia a un entero que se establecerá en el número total de perfiles. Devuelve un **ProfileInfoCollection** que contiene **ProfileInfo** objetos para todos los perfiles del origen de datos donde el nombre de usuario coincide con el nombre de usuario especificado, donde la última fecha de actividad es menor que o igual que el especificado **DateTime**, y donde el nombre de aplicación coincide con el **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles se va a devolver. Si el origen de datos admite capacidades de búsqueda adicionales, como caracteres comodín, puede proporcionar más amplias capacidades de búsqueda para nombres de usuario. Los resultados devueltos por la **FindInactiveProfilesByUserName** método están limitados por los valores de tamaño de página y el índice de la página. El valor de tamaño de página especifica el número máximo de **ProfileInfo** objetos que se va a devolver en la **ProfileInfoCollection**. El valor de índice de página especifica qué página de resultados que se va a devolver, donde 1 representa la primera página. El parámetro para el número total de registros es un parámetro de salida (puede usar **ByRef** en Visual Basic) que se establece en el número total de perfiles. Por ejemplo, si el almacén de datos contiene 13 perfiles de la aplicación y el valor de índice de página es 2 con un tamaño de página de 5, la **ProfileInfoCollection** devuelto contendrá del sexto al décimo perfil. Cuando el método vuelve, el valor del número total de registros se establece en 13. |
| GetNumberOfInActiveProfiles (método) | Toma como entrada un **ProfileAuthenticationOption** valor y un **DateTime** de objetos y devuelve un recuento de todos los perfiles del origen de datos donde la última fecha de actividad es menor o igual que el especificado **Fecha y hora** y donde el nombre de aplicación coincide con el **ApplicationName** valor de propiedad. El **ProfileAuthenticationOption** parámetro especifica si sólo perfiles anónimos, sólo autenticados perfiles, o todos los perfiles son para contabilizarse. |

### <a name="applicationname"></a>ApplicationName

Como los proveedores de perfiles almacenan información de perfil por separado para cada aplicación, debe asegurarse de que el esquema de datos incluye el nombre de la aplicación y que las consultas y actualizaciones también incluyen el nombre de la aplicación. Por ejemplo, el siguiente comando se utiliza para recuperar un valor de propiedad de una base de datos según el nombre de usuario y si el perfil es anónimo y se asegura de que el **ApplicationName** valor está incluido en la consulta.

[!code-sql[Main](profiles-themes-and-web-parts/samples/sample10.sql)]

## <a name="aspnet-themes"></a>Temas de ASP.NET

## <a name="what-are-aspnet-20-themes"></a>¿Cuáles son los temas de ASP.NET 2.0?

Uno de los aspectos más importantes de una aplicación Web es una apariencia coherente en todo el sitio. Los programadores de ASP.NET 1.x suelen utilizan hojas de estilos en cascada (CSS) para implementar una apariencia coherente. ASP.NET 2.0 temas mejoran significativamente en CSS porque proporcionan la capacidad para definir la apariencia de los controles de servidor ASP.NET, así como elementos HTML el desarrollo de ASP.NET. Los temas de ASP.NET pueden aplicarse a controles individuales, una determinada página Web o una aplicación Web completa. Temas utilizan una combinación de archivos CSS, un archivo de máscara opcional y un directorio de imágenes opcional si se necesitan las imágenes. El archivo de máscara controla la apariencia visual de los controles de servidor ASP.NET.

## <a name="where-are-themes-stored"></a>¿Dónde están almacenados los temas?

La ubicación donde se almacenan los temas difiere en función de su ámbito. Los temas que se pueden aplicar a cualquier aplicación se almacenan en la siguiente carpeta:

`C:\WINDOWS\Microsoft.NET\Framework\v2.x.xxxxx\ASP.NETClientFiles\Themes\<Theme_Name>`

Un tema es específico de una aplicación determinada se almacena en un `App\_Themes\<Theme\_Name>` directorio en la raíz del sitio Web.

> [!NOTE]
> Un archivo de máscara sólo debe modificar las propiedades de control de servidor que afectan a la apariencia.

Un tema global es un tema que se pueden aplicar a cualquier aplicación o sitio Web que se ejecuta en el servidor Web. Estos temas se almacenan de forma predeterminada en el directorio ASP.NETClientfiles\Themes dentro del directorio v2.x.xxxxx. Como alternativa, puede mover los archivos de temas en aspnet\_/sistema cliente\_web / /Themes/ [versión] [tema\_nombre] carpeta en la raíz del sitio Web.

Los temas específicos de la aplicación solo pueden aplicarse a la aplicación en el que residen los archivos. Estos archivos se almacenan en la `App\_Themes/<theme\_name>` directorio en la raíz del sitio Web.

## <a name="the-components-of-a-theme"></a>Los componentes de un tema

Un tema se compone de uno o más archivos CSS, un archivo de máscara opcional y una carpeta de imágenes opcional. Los archivos CSS pueden ser cualquier nombre que desee (es decir, default.css o theme.css, etc.) y debe estar en la raíz de la carpeta de temas. Los archivos CSS se utilizan para definir las clases CSS normales y los atributos de selectores específicos. Para aplicar una de las clases CSS a un elemento de página, el **CSSClass** se utiliza la propiedad.

El archivo de máscara es un archivo XML que contiene las definiciones de propiedad para controles de servidor ASP.NET. El código que aparece a continuación es un ejemplo del archivo de máscara.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample11.aspx)]

**Figura 1** siguiente muestra una página ASP.NET pequeña examinarse sin un tema aplicado. **Figura 2** muestra el mismo archivo con un tema aplicado. El color de fondo y el color del texto se configuran a través de un archivo CSS. La apariencia del botón y el cuadro de texto se configuran mediante el archivo de máscara mencionado anteriormente.


![Ningún tema](profiles-themes-and-web-parts/_static/image1.gif)

**Figura 1**: ningún tema


![Tema aplicado](profiles-themes-and-web-parts/_static/image2.gif)

**Figura 2**: tema aplicado


El archivo de máscara mencionado anteriormente define una máscara predeterminada para todos los controles de cuadro de texto y controles de botón. Esto significa que cada control de cuadro de texto y el control de botón insertado en una página tendrá este aspecto. También puede definir una máscara que se pueden aplicar a instancias específicas de estos controles mediante el **SkinID** propiedad del control.

El código siguiente define una máscara para un control de botón. Botón solo se controla con una **SkinID** propiedad de **goButton** tendrá el aspecto de la máscara.

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample12.aspx)]

Solo puede tener un aspecto predeterminado por tipo de control de servidor. Si necesita otras personalizaciones adicionales, debe usar la propiedad SkinID.

## <a name="applying-themes-to-pages"></a>Aplicar temas a páginas

Puede aplicar un tema utilizando cualquiera de los métodos siguientes:

- En el &lt;páginas&gt; elemento del archivo web.config
- En el @Page la directiva de una página
- Mediante programación

## <a name="applying-a-theme-in-the-configuration-file"></a>Aplicar un tema en el archivo de configuración

Para aplicar un tema en el archivo de configuración de aplicaciones, use la sintaxis siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample13.xml)]

El nombre del tema especificado aquí debe coincidir con el nombre de la carpeta de temas. Esta carpeta puede existir en cualquiera de las ubicaciones mencionadas anteriormente en este curso. Si se intenta aplicar un tema que no existe, se producirá un error de configuración.

## <a name="applying-a-theme-in-the-page-directive"></a>Aplicar un tema en la directiva de página

También puede aplicar un tema en la directiva @ Page. Este método permite utilizar un tema para una página específica.

Para aplicar un tema en el @Page directiva, use la siguiente sintaxis:

[!code-aspx[Main](profiles-themes-and-web-parts/samples/sample14.aspx)]

Una vez más, el tema especificado aquí debe coincidir con la carpeta de temas, como se mencionó anteriormente. Si se intenta aplicar un tema que no existe, se producirá un error de compilación. Visual Studio también se resalte el atributo y avisarle de que no existe ningún tema de este tipo.

## <a name="applying-a-theme-programmatically"></a>Aplicar un tema mediante programación

Para aplicar un tema mediante programación, debe especificar el **tema** propiedad de la página en el **página\_PreInit** método.

Para aplicar un tema mediante programación, use la sintaxis siguiente:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample15.cs)]

Es necesario aplicar el tema en el método PreInit debido al ciclo de vida de la página. Si se aplica después de ese punto, habrá que el tema de páginas ya se ha aplicado por el tiempo de ejecución y un cambio en ese momento es demasiado tarde en el ciclo de vida. Si aplica un tema que no existe, un **HttpException** se produce. Cuando se aplica un tema mediante programación, se producirá una advertencia de compilación si los controles de servidor tienen una propiedad SkinID especificada. Esta advertencia está diseñada para informarle de que ningún tema se aplica mediante declaración y puede omitirse.

## <a name="exercise-1--applying-a-theme"></a>Ejercicio 1: Aplicar un tema

En este ejercicio, se aplicará un tema de ASP.NET a un sitio Web.

> [!IMPORTANT]
> Si está utilizando Microsoft Word para introducir información en un archivo de máscara, asegúrese de que no va a reemplazar comillas normales por comillas tipográficas. Las comillas tipográficas, surgirán problemas con los archivos de máscaras.

1. Cree un nuevo sitio web ASP.NET.
2. Haga doble clic en el proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.
3. Elija el archivo de configuración Web de la lista de archivos y haga clic en Agregar.
4. Haga doble clic en el proyecto en el Explorador de soluciones y elija Agregar nuevo elemento.
5. Elija la máscara de archivo y haga clic en Agregar.
6. Haga clic en Sí cuando se le pregunte si desea colocar el archivo dentro de la aplicación, como youd\_carpeta de temas.
7. Haga doble clic en la carpeta SkinFile dentro de la aplicación\_carpeta de temas en el Explorador de soluciones y elija Agregar nuevo elemento.
8. Elija la hoja de estilos de la lista de archivos y haga clic en Agregar. Ahora dispone de todos los archivos necesarios para implementar el nuevo tema. Sin embargo, Visual Studio ha nombrado a la carpeta de temas SkinFile. Haga doble clic en esa carpeta y cambie el nombre a CoolTheme.
9. Abra el archivo SkinFile.skin y agregue el código siguiente al final del archivo: 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample16.aspx)]
10. Guarde el archivo SkinFile.skin.
11. Abra el StyleSheet.css.
12. Reemplace todo el texto en él por lo siguiente: 

    [!code-css[Main](profiles-themes-and-web-parts/samples/sample17.css)]
13. Guarde el archivo StyleSheet.css.
14. Abra la página Default.aspx.
15. Agregue un control de cuadro de texto y un control de botón.
16. Guarde la página. Ahora busque la página Default.aspx. Debe mostrar como un formulario Web normal.
17. Abra el archivo web.config.
18. Agregue lo siguiente justo debajo de la apertura `<system.web>` etiqueta: 

    [!code-xml[Main](profiles-themes-and-web-parts/samples/sample18.xml)]
19. Guarde el archivo web.config. Ahora busque la página Default.aspx. Debería mostrar con el tema aplicado.
20. Si no está ya abierto, abra la página Default.aspx en Visual Studio.
21. Seleccione el botón.
22. Cambiar el **SkinID** propiedad goButton. Tenga en cuenta que Visual Studio proporciona una lista desplegable con valores de SkinID válidos para un control de botón.
23. Guarde la página. Ahora vista previa de la página en el Explorador de nuevo. El botón debe indicar "go" y debe ser mayor de apariencia.

Mediante el **SkinID** propiedad, puede configurar diferentes máscaras para distintas instancias de un determinado tipo de control de servidor con facilidad.

## <a name="the-stylesheettheme-property"></a>StyleSheetTheme (propiedad)

Hasta ahora, hemos hablado solo sobre la aplicación de temas mediante la propiedad del tema. Cuando se usa la propiedad de tema, el archivo de máscara invalidará la configuración declarativa para controles de servidor. Por ejemplo, en el ejercicio 1, especifica un SkinID de "goButton" para el control de botón y que cambia el texto del botón "Ir". Es podrán que haya observado que la propiedad Text del botón en el diseñador se estableció en "Button", pero el tema reemplazó. El tema siempre invalida cualquier configuración de propiedad en el diseñador.

Si desea invalidar las propiedades definidas en el archivo de máscara del tema con propiedades especificado en el diseñador, puede usar el **StyleSheetTheme** propiedad en lugar de la propiedad del tema. La propiedad StyleSheetTheme es igual que la propiedad de tema salvo que no reemplaza todos los valores de propiedad explícitos como hace la propiedad del tema.

Para ver esto en acción, abra el archivo web.config del proyecto en el ejercicio 1 y cambiar la `<pages>` elemento a la siguiente:

[!code-xml[Main](profiles-themes-and-web-parts/samples/sample19.xml)]

Vaya ahora la página Default.aspx y verá que el control de botón tiene una propiedad de texto "Button" de nuevo. Eso es porque el valor de propiedad explícitos en el diseñador está invalidando la propiedad de texto establecida por el goButton SkinID.

## <a name="overriding-themes"></a>Reemplazar temas

Un tema global puede reemplazarse aplicando un tema con el mismo nombre en la aplicación\_carpeta de temas de la aplicación. Sin embargo, no se aplica el tema en un escenario de invalidación es true. Si el tiempo de ejecución encuentra archivos de tema en la aplicación\_carpeta de temas, que se aplicará el tema con esos archivos y se omitirá el tema global.

La propiedad StyleSheetTheme es reemplazable y puede reemplazarse en código como se indica a continuación:

[!code-csharp[Main](profiles-themes-and-web-parts/samples/sample20.cs)]

## <a name="web-parts"></a>Elementos Web

Elementos Web de ASP.NET es un conjunto integrado de controles para crear sitios Web que permiten a los usuarios finales modificar el contenido, la apariencia y el comportamiento de las páginas Web directamente desde un explorador. Las modificaciones se pueden aplicar a todos los usuarios en el sitio o a usuarios individuales. Cuando los usuarios modifican páginas y controles, puede guardar la configuración para conservar preferencias personales de un usuario en futuras sesiones del explorador, una característica denominada personalización. Estas capacidades de elementos Web significan que los desarrolladores pueden permitir a los usuarios finales personalicen dinámicamente, una aplicación Web sin intervención del desarrollador o administrador.

Con el conjunto de controles de elementos Web, como un desarrollador puede habilitar a los usuarios finales:

- Personalizar el contenido de página. Los usuarios pueden agregar nuevos controles de elementos Web a una página, quitarlos, ocultarlos o minimícelas como las ventanas normales.
- Personalizar el diseño de página. Los usuarios pueden arrastrar un control de elementos Web a una zona diferente en una página o cambiar su apariencia, propiedades y comportamiento.
- Exportar e importar controles. Los usuarios pueden importar o exportar la configuración de control de elementos Web para su uso en otras páginas o sitios, conservando la apariencia, incluso los datos de los controles y las propiedades. Esto reduce las demandas de entrada y la configuración de datos en los usuarios finales.
- Crear conexiones. Los usuarios pueden establecer conexiones entre los controles que, por ejemplo, un control gráfico podría mostrar un gráfico para los datos en un control de cotización bursátil. Los usuarios pueden personalizar no sólo la propia conexión, pero la apariencia y los detalles de cómo el control chart muestra los datos.
- Administrar y personalizar la configuración del nivel de sitio. Los usuarios autorizados pueden configurar el nivel de sitio, se determinan quién puede tener acceso a un sitio o una página, configurar el acceso basado en roles a los controles y así sucesivamente. Por ejemplo, un usuario en un rol administrativo podría establecer un control de elementos Web debe ser compartida por todos los usuarios y evitar que los usuarios que no sean administradores personalicen el control compartido.

Normalmente trabajará con elementos Web en una de estas tres maneras: creación de páginas que utilizan controles de elementos Web, crear controles de elementos Web individuales o crear personalizables, completadas las aplicaciones Web, como un portal.

## <a name="page-development"></a>Desarrollo de la página

Los desarrolladores de páginas pueden utilizar herramientas de diseño visual como Microsoft Visual Studio 2005 para crear las páginas que utilizan elementos Web. Una ventaja de utilizar una herramienta como Visual Studio es que el conjunto de controles de los elementos Web proporciona características para la creación de arrastrar y colocar y la configuración de controles de elementos Web en un diseñador visual. Por ejemplo, puede utilizar el diseñador para arrastrar una zona de elementos Web o un control de editor de elementos Web, a la superficie de diseño y, a continuación, configure el control a la derecha en el diseñador utilizando la interfaz de usuario proporcionada por los elementos Web de conjunto de controles. Esto puede acelerar el desarrollo de aplicaciones de elementos Web y reducir la cantidad de código que se debe escribir.

## <a name="control-development"></a>Desarrollo de controles

Puede utilizar cualquier control ASP.NET existente como un control de elementos Web, incluidos los controles de servidor Web estándar, los controles de servidor personalizados y controles de usuario. Para el máximo control mediante programación de su entorno, también puede crear controles de elementos Web personalizados que derivan de la clase de elemento Web. Para el desarrollo de control de elementos Web individual, se suele ya sea crear un control de usuario y utilizarlo como un control de elementos Web o desarrollar un control personalizado de elementos Web.

Como ejemplo de desarrollar un control de elementos Web personalizado, puede crear un control para proporcionar cualquiera de las características proporcionadas por otros controles de servidor ASP.NET que pueden resultar útiles al paquete como un control de elementos Web personalizable: calendarios, listas, información financiera, noticias, calculadoras, controles de texto enriquecido para actualizar contenidas, puede editables las cuadrículas que se conectan a bases de datos, gráficos que dinámicamente actualización sus presentaciones o weather e información sobre el viaje. Si proporciona un diseñador visual con el control, cualquier desarrollador de páginas con Visual Studio simplemente puede arrastrar el control a una zona de elementos Web y configurarlo en tiempo de diseño sin tener que escribir código adicional.

La personalización es la base de la característica de elementos Web. Permite a los usuarios modificar, o personalizar--el diseño, la apariencia y el comportamiento de los controles de elementos Web en una página. La configuración personalizada es duradera: se conservan no solo durante la sesión del explorador actual (al igual que con el estado de vista), sino también en el almacenamiento a largo plazo, para que la configuración de un usuario se guarda para futuras sesiones del explorador también. Personalización está habilitada de forma predeterminada para las páginas de elementos Web.

Los componentes estructurales de la interfaz de usuario se basan en la personalización y proporcionan la estructura de núcleo y los servicios necesarios para todos los controles de elementos Web. Un componente estructural de la interfaz de usuario necesario en cada página de elementos Web es el control WebPartManager. Aunque nunca está visible, este control tiene la tarea esencial de coordinar todos los controles de elementos Web en una página. Por ejemplo, realiza un seguimiento de todos los controles de elementos Web individuales. Administra las zonas de elementos Web (áreas que contienen controles de elementos Web en una página), y qué controles están en las zonas. También realiza un seguimiento y controla los modos de presentación diferentes que puede ser una página de tal modo de exploración, edición o modo de catálogo, y si los cambios de personalización se aplican a todos los usuarios o a usuarios individuales. Por último, se inicia y realiza un seguimiento de las conexiones y la comunicación entre los controles de elementos Web.

El segundo tipo de componente estructural de la interfaz de usuario es la zona. Las zonas actúan como administradores de diseño en una página de elementos Web. Que contienen y organizan los controles que derivan de la clase Part (controles) y proporcionan la capacidad de realizar un diseño de página modular en orientación vertical u horizontal. Las zonas también proporcionan elementos de interfaz de usuario común y coherentes (por ejemplo, el estilo del encabezado y pie de página, título, estilo de borde, botones de acción y así sucesivamente) para cada control que contienen, estos elementos comunes se denominan el cromo de un control. Varios tipos especializados de zonas se utilizan en los modos de presentación diferentes y con distintos controles. Los distintos tipos de zonas se describen en la siguiente sección de controles esenciales de elementos Web.

Los controles de IU de los elementos Web, todos los cuales se derivan de la **parte** de clases, que conforman la interfaz de usuario principal en una página de elementos Web. El conjunto de controles de elementos Web es flexible e inclusivo en las opciones que ofrece para la creación de controles de elementos. Además de crear sus propios controles de elementos Web personalizados, se pueden usar los controles de servidor ASP.NET existentes, los controles de usuario o controles de servidor personalizados como controles de elementos Web. Los controles esenciales que se usan normalmente para crear páginas de elementos Web se describen en la sección siguiente.

## <a name="web-parts-essential-controls"></a>Controles esenciales de elementos Web

El conjunto de controles de elementos Web es exhaustiva, pero algunos controles son esenciales porque son necesarios para los elementos Web para que funcione, o porque son los controles que se usan con mayor frecuencia en las páginas de elementos Web. Empezar a usar elementos Web y crear páginas de elementos Web básicas, resulta útil estar familiarizado con los controles de elementos Web esenciales que se describe en la tabla siguiente.

| **Controles de elementos Web** | **Descripción** |
| --- | --- |
| WebPartManager | Administra todos los controles de elementos Web en una página. Uno (y solamente uno) **WebPartManager** control es necesario para cada página de elementos Web. |
| CatalogZone | Contiene controles CatalogPart. Utilice esta zona para crear un catálogo de controles de elementos Web desde el que los usuarios pueden seleccionar controles para agregar a una página. |
| EditorZone | Contiene controles EditorPart. Utilice esta zona para permitir que los usuarios puedan editar y personalizar los controles de elementos Web en una página. |
| WebPartZone | Contiene y proporciona el diseño global para los controles de elemento Web que forman la interfaz de usuario principal de una página. Utilice esta zona siempre que cree páginas con controles de elementos Web. Las páginas pueden contener una o más zonas. |
| ConnectionsZone | Contiene controles WebPartConnection y proporciona una interfaz de usuario para administrar las conexiones. |
| Elemento Web (GenericWebPart) | Representa la interfaz de usuario principal; la mayoría de los controles de IU de los elementos Web entran en esta categoría. Para el máximo control mediante programación, puede crear controles de elementos Web personalizados que derivan de la base de **elemento Web** control. También puede usar controles de servidor existentes, controles de usuario o controles personalizados como controles de elementos Web. Cada vez que alguno de estos controles se colocan en una zona, la **WebPartManager** control ajusta de forma automática con **GenericWebPart** controles en tiempo de ejecución para que se puede usar con la funcionalidad de elementos Web. |
| CatalogPart | Contiene una lista de controles de elementos Web disponibles que los usuarios pueden agregar a la página. |
| WebPartConnection | Crea una conexión entre dos controles de elementos Web en una página. La conexión define uno de los controles de elementos Web como un proveedor (de datos) y el otro como un consumidor. |
| EditorPart | Actúa como clase base para los controles de editor especializados. |
| Controles EditorPart (AppearanceEditorPart, LayoutEditorPart, BehaviorEditorPart y PropertyGridEditorPart) | Permitir a los usuarios personalizar diversos aspectos de los controles de IU de los elementos Web en una página |

## <a name="lab-create-a-web-part-page"></a>Laboratorio: Crear una página de elementos Web

En este laboratorio, creará una página de elementos Web que se conservará la información a través de los perfiles de ASP.NET.

### <a name="creating-a-simple-page-with-web-parts"></a>Crear una página sencilla con elementos Web

En esta parte del tutorial, creará una página que utiliza controles de elementos Web para mostrar el contenido estático. El primer paso para trabajar con elementos Web es crear una página con dos elementos estructurales necesarios. En primer lugar, una página de elementos Web necesita un control WebPartManager para realizar el seguimiento y coordinar todos los controles de elementos Web. En segundo lugar, una página de elementos Web necesita uno o más zonas, que son controles compuestos que contienen el elemento Web u otros controles de servidor y ocupan una región especificada de una página.

> [!NOTE]
> No es necesario hacer nada para habilitar la personalización de elementos Web; se habilita de forma predeterminada para el conjunto de controles de elementos Web. Cuando se ejecuta por primera vez una página de elementos Web en un sitio, ASP.NET configura un proveedor de personalización predeterminado para almacenar la configuración de personalización de usuario. Para obtener más información sobre la personalización, vea información general sobre la personalización de elementos Web.


### <a name="to-create-a-page-for-containing-web-parts-controls"></a>Para crear una página que contiene controles de elementos Web

1. Cierre la página predeterminada y agregue una nueva página en el sitio denominado WebPartsDemo.aspx.
2. Cambie a **diseño** vista.
3. Desde el **vista** menú, asegúrese de que el **controles no visuales** y **detalles** opciones están seleccionadas para que pueda ver etiquetas de diseño y los controles que no tienen una interfaz de usuario.
4. Coloque el punto de inserción antes de la `<div>` etiquetas en la superficie de diseño y presione ENTRAR para agregar una nueva línea. Coloque el punto de inserción antes del carácter de nueva línea, haga clic en el **formato de bloque** lista desplegable de control en el menú y seleccione el **título 1** opción. En el encabezado, agregue el texto **página de demostración de elementos Web**.
5. Desde el **WebParts** ficha del cuadro de herramientas, arrastre un **WebPartManager** control en la página, colóquela justo después del carácter de nueva línea y antes de la `<div>`etiquetas.   
  
 El **WebPartManager** control no produce ningún resultado, por lo que aparece como un cuadro gris en la superficie del diseñador.
6. Coloque el punto de inserción en el `<div>` etiquetas.
7. En el **diseño** menú, haga clic en **Insertar tabla**y crear una nueva tabla que tiene una fila y tres columnas. Haga clic en el **propiedades de celda** botón, seleccione **arriba** desde el **alineación Vertical** la lista desplegable, haga clic en **Aceptar**y haga clic en **Aceptar** nuevo para crear la tabla.
8. Arrastre un control WebPartZone en la columna de tabla de la izquierda. Haga clic en el **WebPartZone** de control, elija **propiedades**y establezca las siguientes propiedades:   
  
 Id.: SidebarZone   
  
 HeaderText: Sidebar
9. Arrastre una segunda **WebPartZone** controlar en la columna central de la tabla y establezca las siguientes propiedades:   
  
 Id.: MainZone   
  
 HeaderText: principal
10. Guarde el archivo.

La página tiene ahora dos zonas distintas que se pueden controlar por separado. Sin embargo, ninguna zona tiene contenido, por lo que el paso siguiente consiste en crear contenido. En este tutorial, trabajará con controles de elementos Web que muestran solo contenido estático.

El diseño de una zona de elementos Web especificado por un &lt;zonetemplate&gt; elemento. Dentro de la plantilla de zona, puede agregar cualquier control ASP.NET, ya sea un control de elementos Web personalizado, un control de usuario o un control de servidor existente. Observe que aquí se está utilizando el control de etiqueta y, a la que se va a agregar simplemente texto estático. Cuando coloca un control normal del servidor en un **WebPartZone** zona, ASP.NET lo trata como un control de elementos Web en tiempo de ejecución, lo que habilita características de elementos Web en el control.

**Para crear contenido para la zona principal**

1. En **diseño** ver, arrastre un **etiqueta** controlar desde la **estándar** ficha del cuadro de herramientas al área de contenido de la zona cuyo **identificador** propiedad se establece en MainZone.
2. Cambie a **origen** vista. Tenga en cuenta que un &lt;zonetemplate&gt; se agregó el elemento que va a contener la **etiqueta** control en MainZone.
3. Agregue un atributo denominado **título** a la &lt;asp: label&gt; elemento y establezca su valor en contenido. Quite el texto = atributo "Etiqueta" de la &lt;asp: label&gt; elemento. Entre las etiquetas apertura y cierre de la &lt;asp: label&gt; elemento, agregue texto como **Bienvenido a la página principal** dentro de un par de &lt;h2&gt; etiquetas de elemento. El código debe tener el aspecto siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample21.aspx)]
4. Guarde el archivo.

A continuación, cree un control de usuario que también se pueden agregar a la página como un control de elementos Web.

### <a name="to-create-a-user-control"></a>Crear un control de usuario

1. Agregar un nuevo control de usuario Web al sitio para que actúe como un control de búsqueda. Anule la selección de la opción de **colocar código fuente en un archivo independiente**. Agregar en el mismo directorio que la página WebPartsDemo.aspx y asígnele el nombre SearchUserControl.ascx.   
  
    > [!NOTE]
    > El control de usuario para este tutorial no implementa la funcionalidad de búsqueda real; sirve únicamente para demostrar las características de elementos Web.
2. Cambie a **diseño** vista. Desde el **estándar** ficha del cuadro de herramientas, arrastre un control de cuadro de texto a la página.
3. Sitúe el cursor después del cuadro de texto que acaba de agregar y presione ENTRAR para agregar una nueva línea.
4. Arrastre un control de botón a la página en la nueva línea debajo del cuadro de texto que acaba de agregar.
5. Cambie a **origen** vista. Asegúrese de que el código fuente para el control de usuario es similar al ejemplo siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample22.aspx)]
6. Guarde y cierre el archivo.

Ahora puede agregar controles de elementos Web a la zona de Sidebar. Va a agregar dos controles a la zona de Sidebar, uno que contiene una lista de vínculos y otro que es el control de usuario que creó en el procedimiento anterior. Los vínculos se agregan como un estándar **etiqueta** control de servidor, similar a la manera en que se creó el texto estático para la zona principal. Sin embargo, aunque los controles de servidor individuales contenidos en el control de usuario podría estar contenido directamente en la zona (por ejemplo, el control de etiqueta), en este caso no son. En su lugar, forman parte del control de usuario que creó en el procedimiento anterior. Esto muestra una manera habitual de empaquetar los controles y la funcionalidad adicional que desee en un control de usuario y, a continuación, hacer referencia a ese control en una zona como un control de elementos Web.

En tiempo de ejecución, el conjunto de controles de elementos Web contiene ambos controles con controles de GenericWebPart. Cuando un **GenericWebPart** control contiene un control de servidor Web, el control del elemento genérico es el control principal y tener acceso al control de servidor a través de la propiedad de ChildControl del control primario. Este uso de controles de elementos genéricos permite que los controles de servidor Web estándar tienen el mismo comportamiento básico y los atributos como controles de elementos Web que se derivan de la **elemento Web** clase.

### <a name="to-add-web-parts-controls-to-the-sidebar-zone"></a>Para agregar controles de elementos Web a la zona de sidebar

1. Abra la página WebPartsDemo.aspx.
2. Cambie a **diseño** vista.
3. Arrastre la página de control de usuario que creó, SearchUserControl.ascx, desde **el Explorador de soluciones** en la zona cuyo **identificador** propiedad está establecida en SidebarZone y colóquelo ahí.
4. Guarde la página WebPartsDemo.aspx.
5. Cambie a **origen** vista.
6. Dentro de la &lt;asp: webpartzone&gt; elemento SidebarZone, justo encima de la referencia al control de usuario, agregue un &lt;asp: label&gt; elemento con contenido de vínculos, tal como se muestra en el ejemplo siguiente. Además, agregue un **título** atributo a la etiqueta de control de usuario, con un valor de **búsqueda**, tal y como se muestra. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample23.aspx)]
7. Guarde y cierre el archivo.

Ahora puede probar la página buscándolo en el explorador. La página muestra las dos zonas. La captura de pantalla siguiente muestra la página.

**Página de demostración de elementos Web con dos zonas**


![Captura de pantalla del tutorial 1 de partes VS de Web](profiles-themes-and-web-parts/_static/image3.gif)

**Figura 3**: Web partes VS captura de pantalla de tutorial 1


En el título de la barra de cada control es una flecha hacia abajo que proporciona acceso a un menú de verbos de las acciones disponibles que puede realizar en un control. Haga clic en el menú de verbos de uno de los controles, a continuación, haga clic en el **minimizar** verbo y observe que el control se minimiza. En el menú de verbos, haga clic en **restaurar**, y el control vuelve a su tamaño normal.

### <a name="enabling-users-to-edit-pages-and-change-layout"></a>Permitir a los usuarios editar páginas y cambiar el diseño

Elementos Web proporciona la funcionalidad que permite a los usuarios cambiar el diseño de controles de elementos Web arrastrándolos de una zona a otro. Además de permitir que los usuarios se desplacen **elemento Web** controles de una zona a otra, puede permitir que los usuarios los editen diversas características de los controles, incluyendo su apariencia, el diseño y el comportamiento. El conjunto de controles de elementos Web proporciona funcionalidad de edición básica **elemento Web** controles. Aunque no lo hará en este tutorial, también puede crear controles de editor personalizados que permiten a los usuarios editar las características de **elemento Web** controles. Al igual que con cambiar la ubicación de un **elemento Web** (control), editar las propiedades de un control se basa en la personalización de ASP.NET para guardar los cambios realizados por los usuarios.

En esta parte del tutorial, agregará la posibilidad de que los usuarios modifiquen las características básicas de cualquier **elemento Web** control en la página. Para habilitar estas características, agregue otro control de usuario personalizado a la página, junto con un &lt;asp: editorzone&gt; elemento y dos controles de edición.

### <a name="to-create-a-user-control-that-enables-changing-page-layout"></a>Para crear un control de usuario que permite cambiar el diseño de página

1. En Visual Studio, en el **archivo** menú, seleccione la **New** submenú y haga clic en el **archivo** opción.
2. En el **Agregar nuevo elemento** cuadro de diálogo, seleccione **Control de usuario Web**. Nombre del nuevo archivo DisplayModeMenu.ascx. Anule la selección de la opción de **colocar código fuente en un archivo independiente**.
3. Haga clic en Agregar para crear el nuevo control.
4. Cambie a **origen** vista.
5. Quite todo el código existente en el nuevo archivo y péguelo en el código siguiente. Este código de control de usuario usa características del conjunto de controles de elementos Web que permiten a una página Cambiar la vista o el modo de visualización y también le permite cambiar la apariencia física y diseño de la página mientras el equipo están en determinados modos de presentación. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample24.aspx)]
6. Guarde el archivo haciendo clic en la operación de guardar icono en la barra de herramientas o seleccionando **guardar** en el **archivo** menú.

### <a name="to-enable-users-to-change-the-layout"></a>Permitir a los usuarios cambiar el diseño

1. Abra la página WebPartsDemo.aspx y cambie a **diseño** vista.
2. Coloque el punto de inserción en el **diseño** justo después de ver el **WebPartManager** control que agregó anteriormente. Agregue un retorno de carro después del texto para que haya una línea en blanco después de la **WebPartManager** control. Coloque el punto de inserción en la línea en blanco.
3. Arrastre el control de usuario que acaba de crear (el archivo se denomina DisplayModeMenu.ascx) en el WebPartsDemo.aspx página y colóquela en la línea en blanco.
4. Arrastre un control EditorZone desde el **WebParts** sección del cuadro de herramientas hasta la celda de tabla abierta queda en la página WebPartsDemo.aspx.
5. Desde el **WebParts** sección del cuadro de herramientas, arrastre un control AppearanceEditorPart y un control LayoutEditorPart en el **EditorZone** control.
6. Cambie a **origen** vista. El código resultante en la celda de tabla debe ser similar al código siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample25.aspx)]
7. Guarde el archivo WebPartsDemo.aspx. Ha creado un control de usuario que permite cambiar los modos de presentación y cambiar el diseño de página y se ha hecho referencia el control en la página Web principal.

Ahora puede probar la capacidad de editar páginas y cambiar el diseño.

### <a name="to-test-layout-changes"></a>Para probar los cambios de diseño

1. Cargar la página en un explorador.
2. Haga clic en el **modo de presentación** menú desplegable y seleccione **editar**. Se muestran los títulos de zona.
3. Arrastre el **Mis vínculos** control por su barra de título de la zona de la barra lateral en la parte inferior de la zona principal. La página debería parecerse a la siguiente captura de pantalla.

### <a name="web-parts-demo-page-with-my-links-control-moved"></a>Página de demostración de elementos Web con el control Mis vínculos desplazado


![Captura de pantalla del tutorial 2 de partes VS de Web](profiles-themes-and-web-parts/_static/image4.gif)

**Figura 4**: captura de pantalla de tutorial 2 partes VS de Web


1. Haga clic en el **modo de presentación** menú desplegable y seleccione **examinar**. Se actualiza la página, los nombres de zona desaparecen y el **Mis vínculos** controlar permanece donde se colocó inicialmente.
2. Para demostrar que la personalización funciona, cierre el explorador y, a continuación, vuelva a cargar la página. Los cambios realizados se guardan para futuras sesiones del explorador.
3. Desde el **modo de presentación** menú, seleccione **editar**.   
  
 Ahora, cada control en la página se muestra con una flecha hacia abajo en la barra de título que contiene el menú desplegable de verbos.
4. Haga clic en la flecha para mostrar el menú de verbos en el **Mis vínculos** control. Haga clic en el **editar** verbo.   
  
 El **EditorZone** que aparezca el control, mostrando la EditorPart controles que ha agregado.
5. En el **apariencia** sección del control de edición, cambie el **título** en Mis favoritos, utilice la **tipo de cromo** la lista desplegable para seleccionar **título sólo**y, a continuación, haga clic en **aplicar**. La captura de pantalla siguiente muestra la página en modo de edición.

### <a name="web-parts-demo-page-in-edit-mode"></a>Página de demostración de elementos Web en modo de edición


![Captura de pantalla del tutorial 3 de partes VS de Web](profiles-themes-and-web-parts/_static/image5.gif)

**Figura 5**: Web partes VS captura de pantalla de tutorial 3


1. Haga clic en el **modo de presentación** menú y seleccione **examinar** para volver al modo de exploración.
2. El control tiene ahora un título actualizado y ningún borde, como se muestra en la siguiente captura de pantalla.

### <a name="edited-web-parts-demo-page"></a>Página de demostración de elementos Web modificada


![Captura de pantalla de Web partes VS 4 del tutorial](profiles-themes-and-web-parts/_static/image6.gif)

**Figura 4**: captura de pantalla de tutorial 4 partes VS de Web


### <a name="adding-web-parts-at-run-time"></a>Agregar elementos Web en tiempo de ejecución

También puede permitir a los usuarios agregar controles de elementos Web a su página en tiempo de ejecución. Para ello, configure la página con un catálogo de elementos Web, que contiene una lista de controles de elementos Web que desea poner a disposición de los usuarios.

**Para permitir a los usuarios agregar elementos Web en tiempo de ejecución**

1. Abra la página WebPartsDemo.aspx y cambie a **diseño** vista.
2. Desde el **WebParts** ficha del cuadro de herramientas, arrastre un control CatalogZone en la columna derecha de la tabla, debajo del **EditorZone** control.   
  
 Ambos controles pueden estar en la misma celda de la tabla porque no se mostrará al mismo tiempo.
3. En el panel Propiedades, asigne la cadena **agregar elementos Web** a la propiedad HeaderText de la **CatalogZone** control.
4. Desde el **WebParts** sección del cuadro de herramientas, arrastre un control DeclarativeCatalogPart en el área de contenido de la **CatalogZone** control.
5. Haga clic en la flecha situada en la esquina superior derecha de la **DeclarativeCatalogPart** control para exponer su menú de tareas y, a continuación, seleccione **editar plantillas**.
6. Desde el **estándar** sección del cuadro de herramientas, arrastre un **FileUpload** control y un **calendario** controlar en el **WebPartsTemplate** sección de la **DeclarativeCatalogPart** control.
7. Cambie a **origen** vista. Inspeccionar el código fuente de la &lt;asp: catalogzone&gt; elemento. Tenga en cuenta que la **DeclarativeCatalogPart** control contiene un &lt;webpartstemplate&gt; elemento con los dos controles de servidor contenidos que podrá agregar a la página desde el catálogo.
8. Agregar un **título** propiedad a cada uno de los controles que agregó en el catálogo, con el valor de cadena que se muestra para cada título en el ejemplo de código siguiente. Aunque el título no es una propiedad normalmente puede establecer en estas dos controles de servidor en tiempo de diseño, cuando un usuario agrega estos controles a una **WebPartZone** zona desde el catálogo en tiempo de ejecución, son cada una incluida con un  **GenericWebPart** control. Esto les permite actuar como controles de elementos Web, por lo que podrán mostrar títulos.   
  
 El código de los dos controles incluidos en el **DeclarativeCatalogPart** control debe ser similar al siguiente. 

    [!code-aspx[Main](profiles-themes-and-web-parts/samples/sample26.aspx)]
9. Guarde la página.

Ahora puede probar el catálogo.

### <a name="to-test-the-web-parts-catalog"></a>Para probar el catálogo de elementos Web

1. Cargar la página en un explorador.
2. Haga clic en el **modo de presentación** menú desplegable y seleccione **catálogo**.   
  
 El catálogo titulado **agregar elementos Web** se muestra.
3. Arrastre el **Mis favoritos** controlar desde la zona principal hacia la parte superior de la zona de Sidebar y colóquelo ahí.
4. En el **agregar elementos Web** de catálogo, seleccione las casillas de verificación y, a continuación, seleccione **Main** en la lista desplegable que contiene las zonas disponibles.
5. Haga clic en **agregar** en el catálogo. Los controles se agregan a la zona principal. Si lo desea, puede agregar varias instancias de controles desde el catálogo a la página.   
  
 La captura de pantalla siguiente muestra la página con el control de carga de archivos y el calendario en la zona principal. 

![Controles agregados a la zona principal desde el catálogo](profiles-themes-and-web-parts/_static/image7.gif)

    **Figure 5**: Controls added to Main zone from the catalog
6. Haga clic en el **modo de presentación** menú desplegable y seleccione **examinar**. El catálogo desaparece y se actualiza la página.
7. Cierre el explorador. Vuelva a cargar la página. Los cambios realizados se conservan.
