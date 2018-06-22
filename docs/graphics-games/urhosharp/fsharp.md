---
title: 'Programación UrhoSharp con F #'
description: 'Este documento describe cómo crear una aplicación de UrhoSharp de simple hello world con F # en Visual Studio para Mac.'
ms.prod: xamarin
ms.assetid: F976AB09-0697-4408-999A-633977FEFF64
author: charlespetzold
ms.author: chape
ms.date: 03/29/2017
ms.openlocfilehash: 64d69de70d6bc6f23b9907b498622b00c42b6f50
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783277"
---
# <a name="programming-urhosharp-with-f"></a>Programación UrhoSharp con F #

Puede programar UrhoSharp con F # utilizando las mismas bibliotecas y conceptos que se usan por programadores de C#. El [UrhoSharp utilizando](~/graphics-games/urhosharp/using.md) artículo proporciona información general sobre el motor de UrhoSharp y se debe leer antes de este artículo.

Al igual que muchas de las bibliotecas que se produjeron en el mundo de C++, muchas funciones de UrhoSharp devuelven valores booleanos o enteros que indica éxito o error. Debe usar `|> ignore` para pasar por alto estos valores.

El [programa de ejemplo](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) es un "Hello World" para UrhoSharp de F #.

## <a name="creating-an-empty-project"></a>Crear un proyecto vacío

No hay ninguna plantilla de F # para UrhoSharp todavía está disponible, por lo tanto, para crear su propio proyecto UrhoSharp puede empezar con el [ejemplo](https://github.com/xamarin/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp) o siga estos pasos:

1. Desde Visual Studio para Mac, cree un nuevo **solución**. Elija **iOS > aplicaciones > ver solo la aplicación** y seleccione **F #** como el lenguaje de implementación. 
1. Eliminar el **Main.storyboard** archivo. Abra la **Info.plist** archivo y en el **iPhone / iPod información de implementación** panel, eliminar el `Main` de cadena en el **interfaz principal** lista desplegable.
1. Eliminar el **ViewController.fs** archivo también.

## <a name="building-hello-world-in-urho"></a>Creación de Hello World en Urho

Ahora está listo para empezar a definir las clases de su juego. Como mínimo, debe definir una subclase de `Urho.Application` e invalidar sus `Start` método. Para crear este archivo, haga doble clic en el proyecto de F #, elija **Agregar nuevo archivo...**  y agregue una clase de F # vacía al proyecto. El nuevo archivo se agregará al final de la lista de archivos en el proyecto, pero debe arrastrar para que parezca *antes de* se utiliza en **AppDelegate.fs**.

1. Agregue una referencia al paquete Urho NuGet.
1. Desde un proyecto existente de Urho, copie los directorios (superior) **CoreData /** y **datos /** en el proyecto **recursos /** directory. En el proyecto de F #, haga doble clic en el **recursos** carpeta y utilice **agregar / Agregar carpeta existente** para agregar todos estos archivos al proyecto.

La estructura del proyecto ahora debería ser similar:

![](fsharp-images/solutionpane.png "La estructura del proyecto debe parecerse")

Definir la clase recién creado como un subtipo de `Urho.Application` e invalidar sus `Start` método:

```csharp
namespace HelloWorldUrho1

open Urho
open Urho.Gui
open Urho.iOS

type HelloWorld(o : ApplicationOptions) =
    inherit Urho.Application(o) 

override this.Start() = 
        let cache = this.ResourceCache
        let helloText = new Text()

        helloText.Value <- "Hello World from Urho3D, Mono, and F#"
        helloText.HorizontalAlignment <- HorizontalAlignment.Center
        helloText.VerticalAlignment <- VerticalAlignment.Center

        helloText.SetColor (new Color(0.f, 1.f, 0.f))
        let f = cache.GetFont("Fonts/Anonymous Pro.ttf")

        helloText.SetFont(f, 30) |> ignore
                  
        this.UI.Root.AddChild(helloText)
            
```

El código es muy sencillo. Usa el `Urho.Gui.Text` clase para mostrar una cadena centrado con un determinado tamaño de fuente y color. 

Antes de poder ejecutar este código, sin embargo, se debe inicializar UrhoSharp. 

Abra el archivo AppDelegate.fs y modificar el `FinishedLaunching` método tal como se indica a continuación:

```csharp
namespace HelloWorldUrho1

open System
open UIKit
open Foundation
open Urho
open Urho.iOS

[<Register ("AppDelegate")>]
type AppDelegate () =
    inherit UIApplicationDelegate ()

    override this.FinishedLaunching (app, options) =
        let o = ApplicationOptions.Default
     
        let g = new HelloWorld(o)
        g.Run() |> ignore
       
        true
```

El `ApplicationOptions.Default` proporciona las opciones predeterminadas para una aplicación en modo horizontal. Pasar estos `ApplicationOptions` al constructor predeterminado para su `Application` subclase (tenga en cuenta que, una vez definido el `HelloWorld` de la clase, la línea `inherit Application(o)` llama al constructor de clase base). 

El `Run` método de su `Application` inicia el programa. Se define como devolver un `int`, que se puede canalizar a `ignore`. 

El programa resultante debería ser similar:

![](fsharp-images/helloworldfsharp.png "El programa resultante debe ser similar a")








## <a name="related-links"></a>Vínculos relacionados

- [Busque en GitHub (ejemplo)](https://github.com/xamarinhttps://developer.xamarin.com/recipes/tree/master/cross-platform/urho/urho-fsharp/HelloWorldUrhoFsharp)
