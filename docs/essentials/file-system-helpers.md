---
title: 'Xamarin.Essentials: Aplicaciones auxiliares de sistema de archivos'
description: La clase de sistema de archivos en Xamarin.Essentials contiene una serie de aplicaciones auxiliares encontrar caché de la aplicación y los directorios de datos y abrir archivos dentro del paquete de aplicación.
ms.assetid: B3EC2DE0-EFC0-410C-AF71-7410AE84CF84
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 13293ec05261cbdc1e70fd278002d1af18654851
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782590"
---
# <a name="xamarinessentials-file-system-helpers"></a>Xamarin.Essentials: Aplicaciones auxiliares de sistema de archivos

![La versión preliminar de NuGet](~/media/shared/pre-release.png)

El **FileSystem** clase contiene una serie de aplicaciones auxiliares para buscar los directorios de caché y los datos de la aplicación y abrir archivos dentro del paquete de aplicación.

## <a name="using-file-system-helpers"></a>Usar aplicaciones auxiliares de sistema de archivos

Agregue una referencia a Xamarin.Essentials en la clase:

```csharp
using Xamarin.Essentials;
```

Para obtener el directorio de la aplicación para almacenar **almacenar en caché datos**. Datos de la caché pueden usarse para cualquier dato que deba conservar más datos temporales, pero no debe ser datos que son necesarios para que funcione correctamente.

```csharp
var cacheDir = FileSystem.CacheDirectory;
```

Para obtener el directorio de nivel superior de la aplicación para los archivos que no sean archivos de datos de usuario. Estos archivos se copian con el sistema operativo sincronizando el marco de trabajo. Ver detalles de implementación de plataforma a continuación.

```csharp
var mainDir = FileSystem.AppDataDirectory;
```

Para abrir un archivo que se incluye en el paquete de aplicación:

```csharp
 using (var stream = await FileSystem.OpenAppPackageFileAsync(templateFileName))
 {
    using (var reader = new StreamReader(stream))
    {
        var fileContents = await reader.ReadToEndAsync();
    }
 }
```

## <a name="platform-implementation-specifics"></a>Detalles de implementación de plataforma

# <a name="androidtabandroid"></a>[Android](#tab/android)

- **CacheDirectory** : devuelve la [CacheDir](https://developer.android.com/reference/android/content/Context.html#getCacheDir) del contexto actual.
- **AppDataDirectory** : devuelve la [FilesDir](https://developer.android.com/reference/android/content/Context.html#getFilesDir) del contexto actual y son realizar una copia de [copia de seguridad automática](https://developer.android.com/guide/topics/data/autobackup.html) a partir de API 23 y versiones posteriores.

Agregar cualquier archivo en el **activos** carpeta en el Android del proyecto y marcar la acción de compilación como **AndroidAsset** para utilizarlo con `OpenAppPackageFileAsync`.

# <a name="iostabios"></a>[iOS](#tab/ios)

- **CacheDirectory** : devuelve la [biblioteca/cachés](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directory.
- **AppDataDirectory** : devuelve la [biblioteca](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/FileSystemOverview/FileSystemOverview.html) directorio que está respaldada por iTunes y iCloud.

Agregar cualquier archivo en el **recursos** carpeta en el archivo iOS del proyecto y marcar la acción de compilación como **BundledResource** para utilizarlo con `OpenAppPackageFileAsync`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

- **CacheDirectory** : devuelve la [LocalCacheFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localcachefolder#Windows_Storage_ApplicationData_LocalCacheFolder) directory...
- **AppDataDirectory** : devuelve la [LocalFolder](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdata.localfolder#Windows_Storage_ApplicationData_LocalFolder) directorio que se copia en la nube.

Agregue un archivo en la raíz en el proyecto de UWP y marcar la acción de compilación como **contenido** para utilizarlo con `OpenAppPackageFileAsync`.

--------------

## <a name="api"></a>API

- [Código fuente de aplicaciones auxiliares de sistema de archivos](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/FileSystem)
- [Documentación de la API del sistema de archivos](xref:Xamarin.Essentials.FileSystem)
