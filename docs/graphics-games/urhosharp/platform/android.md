---
title: Compatibilidad con UrhoSharp Android
description: Este documento describe la configuración específica de Android y la información relacionada con la característica de UrhoSharp. En concreto, se trata de arquitecturas compatibles, cómo crear un proyecto, configurar e iniciar Urho e incrustar personalizado de Urho.
ms.prod: xamarin
ms.assetid: 8409BD81-B1A6-4F5D-AE11-6BBD3F7C6327
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 6e489f52712989b5f94fa52d5ec6f22a13ce6252
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783787"
---
# <a name="urhosharp-android-support"></a>Compatibilidad con UrhoSharp Android

_Características y el programa de instalación específico android_

Mientras se Urho es una biblioteca de clases portables y permite a las mismas API que se usará en la plataforma distintos para la lógica de juego, todavía tiene que inicializar Urho en el controlador específico de plataforma y en algunos casos, puede aprovechar las características específicas de plataforma .

En las páginas siguientes, se asume que `MyGame` es una subclase de la `Application` clase.

## <a name="architectures"></a>Arquitecturas

**Admite arquitecturas**: x86, armeabi, armeabi v7a

## <a name="create-a-project"></a>Crear un proyecto

Crear un proyecto de Android y agregue el paquete UrhoSharp NuGet.

Agregar datos que contienen sus activos para la **activos** directory y asegúrese de que todos los archivos tienen **AndroidAsset** como el **acción de compilación**.

![Configuración del proyecto](android-images/image-3.png "agregar datos que contiene los activos en el directorio de activos")

## <a name="configure-and-launching-urho"></a>Configurar e iniciar Urho

Agregar mediante sentencias para la `Urho` y `Urho.Android` espacios de nombres y, a continuación, agregue este código para inicializar Urho, así como para iniciar la aplicación.

La manera más sencilla de ejecutar un juego, tal y como se implementa en la clase MyGame es llamar a

```csharp
UrhoSurface.RunInActivity<MyGame>();
```

Se abrirá una actividad de pantalla completa con el juego como un contenido.

## <a name="custom-embedding-of-urho"></a>Incrustar personalizado de Urho

Se puede también a tener Urho ocupe la pantalla de toda la aplicación, y para usarlo como un componente de la aplicación, puede crear un `SurfaceView` a través de:

```csharp
var surface = UrhoSurface.CreateSurface<MyGame>(activity)
```

También necesitará reenviar eventos unos por su actividad a UrhoSharp, por ejemplo:

```csharp
protected override void OnPause()
{
    UrhoSurface.OnPause();
    base.OnPause();
}
```

Debe hacer lo mismo: `OnResume`, `OnPause`, `OnLowMemory`, `OnDestroy`, `DispatchKeyEvent` y `OnWindowFocusChanged`.

Esto muestra una actividad típicos que se inicia el juego:

```csharp
[Activity(Label = "MyUrhoApp", MainLauncher = true,
    Icon = "@drawable/icon", Theme = "@android:style/Theme.NoTitleBar.Fullscreen",
    ConfigurationChanges = ConfigChanges.KeyboardHidden | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Portrait)]
public class MainActivity : Activity
{
    protected override void OnCreate(Bundle bundle)
    {
        base.OnCreate(bundle);
        var mLayout = new AbsoluteLayout(this);
        var surface = UrhoSurface.CreateSurface<MyUrhoApp>(this);
        mLayout.AddView(surface);
        SetContentView(mLayout);
    }

    protected override void OnResume()
    {
        UrhoSurface.OnResume();
        base.OnResume();
    }

    protected override void OnPause()
    {
        UrhoSurface.OnPause();
        base.OnPause();
    }

    public override void OnLowMemory()
    {
        UrhoSurface.OnLowMemory();
        base.OnLowMemory();
    }

    protected override void OnDestroy()
    {
        UrhoSurface.OnDestroy();
        base.OnDestroy();
    }

    public override bool DispatchKeyEvent(KeyEvent e)
    {
        if (!UrhoSurface.DispatchKeyEvent(e))
            return false;
        return base.DispatchKeyEvent(e);
    }

    public override void OnWindowFocusChanged(bool hasFocus)
    {
        UrhoSurface.OnWindowFocusChanged(hasFocus);
        base.OnWindowFocusChanged(hasFocus);
    }
}
```

