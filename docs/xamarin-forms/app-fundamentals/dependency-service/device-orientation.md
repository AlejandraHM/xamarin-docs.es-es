---
title: Comprobación de orientación del dispositivo
description: En este artículo se explica cómo usar la clase Xamarin.Forms DependencyService para tener acceso a la orientación del dispositivo desde código compartido.
ms.prod: xamarin
ms.assetid: 5F60975F-47DB-4361-B97C-2290D6F77D2F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/09/2016
ms.openlocfilehash: e21531b7f39d3876d91eea8fa6cb9e409a9deffa
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35240061"
---
# <a name="checking-device-orientation"></a>Comprobación de orientación del dispositivo

En este artículo le ayudará a usar [ `DependencyService` ](https://developer.xamarin.com/api/type/Xamarin.Forms.DependencyService/) para comprobar la orientación del dispositivo desde código compartido con las API nativas en cada plataforma. En este tutorial se basa en las existentes `DeviceOrientation` complemento por Ali Özgür. Consulte la [repositorio de GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) para obtener más información.

- **[Creación de la interfaz](#Creating_the_Interface)**  &ndash; entender cómo la interfaz se crea en código compartido.
- **[Implementación de iOS](#iOS_Implementation)**  &ndash; obtener información sobre cómo implementar la interfaz en código nativo para iOS.
- **[Implementación de Android](#Android_Implementation)**  &ndash; obtener información sobre cómo implementar la interfaz en código nativo para Android.
- **[Implementación de UWP](#WindowsImplementation)**  &ndash; obtener información sobre cómo implementar la interfaz en código nativo para la plataforma Universal de Windows (UWP).
- **[Implementar en el código compartido](#Implementing_in_Shared_Code)**  &ndash; obtener información sobre cómo usar `DependencyService` para llamar a la implementación nativa desde código compartido.

La aplicación que utiliza `DependencyService` tendrá la estructura siguiente:

![](device-orientation-images/orientation-diagram.png "Estructura de aplicación DependencyService")

> [!NOTE]
> Es posible detectar si el dispositivo está en orientación vertical u horizontal en el código compartido, como se muestra en [dispositivo Orientation]/guides/xamarin-forms/user-interface/layouts/device-orientation/#changes-in-orientation). El método descrito en este artículo usa las características nativas para obtener más información acerca de la orientación, incluso si el dispositivo está boca abajo.

<a name="Creating_the_Interface" />

## <a name="creating-the-interface"></a>Creación de la interfaz

En primer lugar, cree una interfaz en el código compartido que expresa la funcionalidad que desee implementar. En este ejemplo, la interfaz contiene un único método:

```csharp
namespace DependencyServiceSample.Abstractions
{
    public enum DeviceOrientations
    {
        Undefined,
        Landscape,
        Portrait
    }

    public interface IDeviceOrientation
    {
        DeviceOrientations GetOrientation();
    }
}
```

Codificar en esta interfaz en el código compartido le permitirá que la aplicación de Xamarin.Forms tener acceso a la orientación del dispositivo las API en cada plataforma.

> [!NOTE]
> Las clases que implementan la interfaz deben tener un constructor sin parámetros para que funcione con el `DependencyService`.

<a name="iOS_Implementation" />

## <a name="ios-implementation"></a>Implementación de iOS

La interfaz se debe implementar en cada proyecto de aplicación específico de la plataforma. Tenga en cuenta que la clase tiene un constructor sin parámetros para que la `DependencyService` puede crear nuevas instancias:

```csharp
using UIKit;
using Foundation;

namespace DependencyServiceSample.iOS
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation(){ }

        public DeviceOrientations GetOrientation()
        {
            var currentOrientation = UIApplication.SharedApplication.StatusBarOrientation;
            bool isPortrait = currentOrientation == UIInterfaceOrientation.Portrait
                || currentOrientation == UIInterfaceOrientation.PortraitUpsideDown;

            return isPortrait ? DeviceOrientations.Portrait: DeviceOrientations.Landscape;
        }
    }
}
```

Por último, agregue esta `[assembly]` atributo por encima de la clase (y fuera de los espacios de nombres que se han definido), incluido cualquier necesario `using` instrucciones:

```csharp
using UIKit;
using Foundation;
using DependencyServiceSample.iOS; //enables registration outside of namespace

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.iOS {
    ...
```

Este atributo registra la clase como una implementación de la `IDeviceOrientation` interfaz, lo que significa que `DependencyService.Get<IDeviceOrientation>` puede utilizarse en el código compartido para crear una instancia del mismo.

<a name="Android_Implementation" />

## <a name="android-implementation"></a>Implementación de Android

El código siguiente implementa `IDeviceOrientation` en Android:

```csharp
using DependencyServiceSample.Droid;
using Android.Hardware;

namespace DependencyServiceSample.Droid
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public static void Init() { }

        public DeviceOrientations GetOrientation()
        {
            IWindowManager windowManager = Android.App.Application.Context.GetSystemService(Context.WindowService).JavaCast<IWindowManager>();

            var rotation = windowManager.DefaultDisplay.Rotation;
            bool isLandscape = rotation == SurfaceOrientation.Rotation90 || rotation == SurfaceOrientation.Rotation270;
            return isLandscape ? DeviceOrientations.Landscape : DeviceOrientations.Portrait;
        }
    }
}
```

Agregue el código `[assembly]` atributo por encima de la clase (y fuera de los espacios de nombres que se han definido), incluido cualquier necesario `using` instrucciones:

```csharp
using DependencyServiceSample.Droid; //enables registration outside of namespace
using Android.Hardware;

[assembly: Xamarin.Forms.Dependency (typeof (DeviceOrientationImplementation))]
namespace DependencyServiceSample.Droid {
    ...
```

Este atributo registra la clase como una implementación de la `IDeviceOrientaiton` interfaz, lo que significa que `DependencyService.Get<IDeviceOrientation>` pueden usarse en el código compartido puede crear una instancia del mismo.

<a name="WindowsImplementation" />

## <a name="universal-windows-platform-implementation"></a>Implementación de la plataforma universal de Windows

El código siguiente implementa la `IDeviceOrientation` interfaz en la plataforma Universal de Windows:

```csharp
namespace DependencyServiceSample.WindowsPhone
{
    public class DeviceOrientationImplementation : IDeviceOrientation
    {
        public DeviceOrientationImplementation() { }

        public DeviceOrientations GetOrientation()
        {
            var orientation = Windows.UI.ViewManagement.ApplicationView.GetForCurrentView().Orientation;
            if (orientation == Windows.UI.ViewManagement.ApplicationViewOrientation.Landscape) {
                return DeviceOrientations.Landscape;
            }
            else {
                return DeviceOrientations.Portrait;
            }
        }
    }
}
```

Agregar el `[assembly]` atributo por encima de la clase (y fuera de los espacios de nombres que se han definido), incluido cualquier necesario `using` instrucciones:

```csharp
using DependencyServiceSample.WindowsPhone; //enables registration outside of namespace

[assembly: Dependency(typeof(DeviceOrientationImplementation))]
namespace DependencyServiceSample.WindowsPhone {
    ...
```

Este atributo registra la clase como una implementación de la `DeviceOrientationImplementation` interfaz, lo que significa que `DependencyService.Get<IDeviceOrientation>` pueden usarse en el código compartido puede crear una instancia del mismo.

<a name="Implementing_in_Shared_Code" />

## <a name="implementing-in-shared-code"></a>Implementar en el código compartido

Ahora podemos escribir y probar el código compartido que tiene acceso a la `IDeviceOrientation` interfaz. Esta página simple incluye un botón que actualice su propio texto en función de la orientación del dispositivo. Usa el `DependencyService` para obtener una instancia de la `IDeviceOrientation` interfaz &ndash; en tiempo de ejecución esta instancia será la implementación específica de la plataforma que tiene acceso completo a lo SDK nativo:

```csharp
public MainPage ()
{
    var orient = new Button {
        Text = "Get Orientation",
        VerticalOptions = LayoutOptions.CenterAndExpand,
        HorizontalOptions = LayoutOptions.CenterAndExpand,
    };
    orient.Clicked += (sender, e) => {
       var orientation = DependencyService.Get<IDeviceOrientation>().GetOrientation();
       switch(orientation){
           case DeviceOrientations.Undefined:
                orient.Text = "Undefined";
                break;
           case DeviceOrientations.Landscape:
                orient.Text = "Landscape";
                break;
           case DeviceOrientations.Portrait:
                orient.Text = "Portrait";
                break;
       }
    };
    Content = orient;
}
```

Ejecute esta aplicación en iOS, Android o las plataformas de Windows y al presionar el botón se producirán en el texto del botón Actualizar con la orientación del dispositivo.

![](device-orientation-images/orientation.png "Ejemplo de orientación de dispositivo")


## <a name="related-links"></a>Vínculos relacionados

- [Uso de DependencyService (ejemplo)](https://developer.xamarin.com/samples/UsingDependencyService)
- [DependencyService (ejemplo)](https://developer.xamarin.com/samples/DependencyService/DependencyServiceSample/)
- [Xamarin.Forms Samples](https://github.com/xamarin/xamarin-forms-samples) (Ejemplos de Xamarin.Forms)
