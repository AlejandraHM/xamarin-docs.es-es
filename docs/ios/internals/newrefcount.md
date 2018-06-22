---
title: Sistema de Xamarin.iOS de recuento de referencias de nuevo
description: Este documento describe el sistema, habilitada de forma predeterminada en todas las aplicaciones de Xamarin.iOS de recuento de referencias mejorados de Xamarin.
ms.prod: xamarin
ms.assetid: 0221ED8C-5382-4C1C-B182-6C3F3AA47DB1
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: f2e40ca1fdd4a02d62e45004b75f3abefda781a5
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34786257"
---
# <a name="new-reference-counting-system-in-xamarinios"></a>Sistema de Xamarin.iOS de recuento de referencias de nuevo

Xamarin.iOS 9.2.1 introdujo la referencia mejorada recuento de sistema para todas las aplicaciones de forma predeterminada. Se puede utilizar para eliminar muchos problemas de memoria que eran difíciles de realizar un seguimiento y corregir en versiones anteriores de Xamarin.iOS.

## <a name="enabling-the-new-reference-counting-system"></a>Habilitar el nuevo sistema de recuento de referencias

A partir de Xamarin 9.2.1 está habilitada el ref nuevo recuento de sistema para **todos los** las aplicaciones de forma predeterminada.

Si está desarrollando una aplicación existente, puede comprobar el archivo .csproj para asegurarse de que todas las apariciones de `MtouchUseRefCounting` se establecen en `true`, como a continuación:

```xml
<MtouchUseRefCounting>true</MtouchUseRefCounting>
```

Si se establece en `false` la aplicación no esté usando las herramientas de nuevo.

### <a name="using-older-versions-of-xamarin"></a>Con versiones anteriores de Xamarin

Xamarin.iOS 7.2.1 y sobre características de una vista previa mejorada de nuestro nuevo sistema de recuento de referencias.

**API clásico:**

Para habilitar este nuevo sistema de recuento de referencia, compruebe el **usar la extensión de recuento de referencias** casilla de verificación que se encuentra en la **avanzadas** ficha de su proyecto **opciones de compilación de iOS** , tal y como se muestra a continuación: 

[![](newrefcount-images/image1.png "Habilitar el nuevo sistema de recuento de referencia")](newrefcount-images/image1.png#lightbox)

Tenga en cuenta que estas opciones se han quitado en versiones más recientes de Visual Studio para Mac.

 **[API unificada:](~/cross-platform/macios/unified/index.md)**

 La nueva referencia contar la extensión es necesaria para la API unificada y debe habilitarse de forma predeterminada. Las versiones anteriores de su IDE no pueden tener este valor comprueban automáticamente y es posible que deba colocar una comprobación en él.

    
> [!IMPORTANT]
> Una versión anterior de esta característica ha estado presente desde MonoTouch 5.2, pero solo estaba disponible para **sgen** como una vista previa experimental. Esta versión nueva y mejorada ahora también está disponible para el **Boehm** recolector de elementos no utilizados.


Históricamente ha habido dos tipos de objetos administrados por Xamarin.iOS: aquellos que fueron simplemente un contenedor alrededor de un objeto nativo (objetos del mismo nivel) y aquellos que extendidos o incorpora nuevas funciones (objetos derivados) - normalmente por mantener el estado en memoria adicional. Anteriormente, era posible que se puede aumentar un objeto del mismo nivel con estado (por ejemplo al agregar un controlador de eventos de C#) pero que se permita que el objeto vaya sin referencia y, a continuación, recopilados. Esto podría provocar un bloqueo más adelante (por ejemplo, si el tiempo de ejecución de C de objetivo de llamada en el objeto administrado).

El nuevo sistema actualiza automáticamente los objetos del mismo nivel en objetos administrados por el tiempo de ejecución cuando almacena cualquier información adicional.

Así resuelve varios bloqueos que se produjeron en situaciones como esta:

```csharp
class MyTableSource : UITableViewSource {
   public override UITableViewCell GetCell (UITableView tableView, NSIndexPath indexPath) {
        var cell = tableView.DequeueReusableCell ("myId");
        if (cell != null)
                return cell;

        cell = new UITableViewCell (UITableViewCellStyle.Default, "myId");
        var txt = new UITextField ();
        txt.TouchDown += delegate { Console.WriteLine ("...."); };
        cell.ContentView.AddSubview (txt);
        return cell;
   }
}
```

Sin la extensión de recuento de referencia este código podría bloquearse porque `cell` pasa a ser recopilable de modo que su `TouchDown` delegar, que se traducirá en un puntero a pendiente.

La extensión de recuento de referencia garantiza el objeto administrado permanece activo e impide que su colección, siempre se retiene el objeto nativo mediante código nativo.

El nuevo sistema también elimina la necesidad de *mayoría* privada de realizar una copia de los campos que se utilizan en enlaces - que es el método predeterminado para mantener activa la instancia. El vinculador administrado es lo suficientemente inteligente como para quitar todos los *innecesarios* campos desde las aplicaciones que usan la nueva referencia contar la extensión.

Esto significa que cada instancia de objeto administrado consume menos memoria que antes. También resuelve un problema relacionado que algunos campos de respaldo contendría las referencias que no eran necesarios ya el Objective-C en tiempo de ejecución, lo que resultaría difícil reclamar memoria.
