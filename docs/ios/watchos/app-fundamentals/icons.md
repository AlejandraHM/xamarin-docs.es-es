---
title: Trabajar con iconos en Xamarin watchOS
description: Este documento describe los distintos iconos necesarios para una aplicación watchOS y cómo configurar una solución para incluir estos iconos.
ms.prod: xamarin
ms.assetid: EE3D45BD-8091-4C04-BA83-371371D8BEB9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 150cca754de26edffcf97bb5d39b26166662c75b
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34790671"
---
# <a name="working-with-watchos-icons-in-xamarin"></a>Trabajar con iconos en Xamarin watchOS

Soluciones de Apple Watch requieren dos conjuntos de iconos:

* Los iconos de aplicación de iOS que aparecerán en el iPhone.
* Iconos de Apple Watch que se representará en un círculo en el menú de inspección y en las pantallas de notificación. El icono de la aplicación de inspección también aparece en la [de Apple Watch](~/ios/watchos/app-fundamentals/settings.md) aplicación iOS.

## <a name="apple-watch-icons"></a>Iconos de Apple Watch

| | | |
|-|-|-|
|Icono de la aplicación de iOS|Aparece en el iPhone e inicia la aplicación principal|![](icons-images/icon-ios.png)|
|Icono de la aplicación de inspección|Aparece en la pantalla principal de Apple Watch|![](icons-images/icon-home.png)|
||Aparece en las notificaciones de inspección|![](icons-images/notification-icon.png)|
||Aparece en el [Apple Watch aplicación iOS](~/ios/watchos/app-fundamentals/settings.md)|![](icons-images/watch-app-sml.png)|

## <a name="configuring-your-solution"></a>Configuración de la solución

Para asegurarse de que la aplicación de iOS y aplicación de inspección mostrar el icono y el nombre correcto, siga estas instrucciones para cada proyecto:

### <a name="ios-app"></a>Aplicación iOS

Hacer referencia a la [iconos de aplicación de iOS guía](~/ios/app-fundamentals/images-icons/app-icons.md) para asegurarse de que los iconos de la aplicación iOS están configurados correctamente.

#### <a name="infoplist"></a>Info.plist

La cadena que aparece junto a la aplicación de inspección en la [aplicación de configuración de Apple Watch](~/ios/watchos/app-fundamentals/settings.md) está configurado en el **Info.plist de la aplicación de iOS**.

Confirme que su **Info.plist** tiene un `CFBundleName` clave y valor (Nota: Esto es diferente a la `CFBundleDisplayName`, puede tener ambos):

```xml
<key>CFBundleName</key>
<string>Your App Name</string>
```

### <a name="apple-watch-app"></a>Aplicación de Apple Watch

Una vez el [aplicación primaria](~/ios/watchos/app-fundamentals/parent-app.md) ha configurado sus iconos, debe agregar un catálogo de activos de icono de aplicación a la aplicación de inspección.

1. Haga doble clic en el proyecto de aplicación de inspección y seleccione **archivo > Agregar > nuevo archivo... > iOS > catálogo de activos** para agregar un catálogo de activos para el proyecto.

 ![](icons-images/newasset.png "Agregar un catálogo de activos para el proyecto")

2. Haga doble clic en el **AppIcons.appiconset/Contents.json** archivo

  ![](icons-images/xcassets-iconset-sml.png "El contenido de AppIcons")

3. Agregue todas las imágenes de watchOS, como se muestra en esta captura de pantalla:

  [![](icons-images/appicons-sml.png "Agregue todas las imágenes de watchOS, como se muestra en esta captura de pantalla")](icons-images/appicons.png#lightbox)

  Hacer referencia a [instrucciones de icono de Apple](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html) para conocer los tamaños necesarios (las dimensiones también se muestran en la pantalla). Recuerde que se recortarán automáticamente estos iconos para representar en un círculo.

  La lista de iconos debe tener un aspecto similar al siguiente:

  ![](icons-images/xcassets-complete-sml.png "La lista de iconos en el Explorador de soluciones")

4. Para asegurarse de que el catálogo de activos se incluye en la aplicación, agregue la siguiente clave y el valor para el **Info.plist de la aplicación de inspección**:

```xml
<key>XSAppIconAssets</key>
<string>Images.xcassets/AppIcons.appiconset</string>
```

Puede comprobar los iconos se configuran correcta mediante la comprobación de la [aplicación de configuración de Apple Watch](~/ios/watchos/app-fundamentals/settings.md) en el simulador, iPhone o generar un [notificación](~/ios/watchos/platform/notifications.md) y confirmar el icono aparece en la notificación pantalla.

> [!NOTE]
> Iconos no pueden tener un canal alfa (la aplicación se rechazarán durante el envío de la tienda de aplicaciones si hay un canal alfa). Puede comprobar si un canal alfa existe y quítela [mediante la aplicación de vista previa en Mac OS X](~/ios/watchos/troubleshooting.md#noalpha).


## <a name="related-links"></a>Vínculos relacionados

- [Guían de Apple icono & imágenes](https://developer.apple.com/library/prerelease/ios/documentation/UserExperience/Conceptual/WatchHumanInterfaceGuidelines/IconandImageSizes.html)
