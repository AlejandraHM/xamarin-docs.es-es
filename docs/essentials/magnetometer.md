---
title: 'Xamarin.Essentials: Magnetómetro'
description: La clase Magnetómetro en Xamarin.Essentials le permite supervisar el sensor de magnetómetro del dispositivo, lo que indica la orientación del dispositivo en relación con el campo magnético de la tierra.
ms.assetid: 64DD0D41-03E2-40DD-9EC8-101CA0ED852B
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 78ed943a0fa7ca0cb249a4ac030ea6b52dcfb537
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782942"
---
# <a name="xamarinessentials-magnetometer"></a>Xamarin.Essentials: Magnetómetro

![La versión preliminar de NuGet](~/media/shared/pre-release.png)

El **Magnetómetro** clase le permite supervisar el sensor de magnetómetro del dispositivo que indica la orientación del dispositivo en relación con el campo magnético de la tierra.

## <a name="using-magnetometer"></a>Usar Magnetómetro

Agregue una referencia a Xamarin.Essentials en la clase:

```csharp
using Xamarin.Essentials;
```

La funcionalidad de Magnetómetro funciona mediante una llamada a la `Start` y `Stop` métodos para realizar escuchas para los cambios en el magnetómetro. Los cambios se envían a través de la `ReadingChanged` eventos. Este es el ejemplo de uso:

```csharp

public class MagnetometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public MagnetometerTest()
    {
        // Register for reading changes.
        Magnetometer.ReadingChanged += Magnetometer_ReadingChanged;
    }

    void Magnetometerr_ReadingChanged(MagnetometerChangedEventArgs e)
    {
        var data = e.Reading;
        // Process MagneticField X, Y, and Z
        Console.WriteLine($"Reading: X: {data.MagneticField.X}, Y: {data.MagneticField.Y}, Z: {data.MagneticField.Z}");
    }

    public void ToggleMagnetometer()
    {
        try
        {
            if (Magnetometer.IsMonitoring)
              Magnetometer.Stop();
            else
              Magnetometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

Todos los datos se devuelven en microteslas.

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Velocidad de sensor](xref:Xamarin.Essentials.SensorSpeed)

- **Más rápido** : obtener los datos del sensor de mayor velocidad posible (no se garantiza para devolver en el subproceso de interfaz de usuario).
- **Juego** : frecuencia adecuada para juegos (no se garantiza para devolver en el subproceso de interfaz de usuario).
- **Normal** : velocidad predeterminada adecuado para los cambios de orientación de pantalla.
- **Interfaz de usuario** : velocidad apropiada para la interfaz de usuario general.

## <a name="api"></a>API

- [Código fuente de magnetómetro](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Magnetometer)
- [Documentación de API de magnetómetro](xref:Xamarin.Essentials.Magnetometer)
