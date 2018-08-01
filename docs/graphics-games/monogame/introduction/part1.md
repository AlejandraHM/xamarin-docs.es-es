---
title: 'Parte 1: crear un MonoGame multiplataforma'
description: Este tutorial muestra cómo crear un nuevo proyecto para iOS y Android mediante la opción MonoGame. El resultado es un Visual Studio para la solución de Mac con un proyecto de código compartido multiplataforma, así como un proyecto para cada plataforma. Este proyecto mostrará una pantalla azul vacía cuando se ejecuta.
ms.prod: xamarin
ms.assetid: FC69E69B-04D4-45DF-9BBF-2A6CDEAD9B2F
author: charlespetzold
ms.author: chape
ms.date: 03/28/2017
ms.openlocfilehash: bd7990b94e678c205f9ce636f4eb0d28180fc6ec
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 05/09/2018
ms.locfileid: "33921994"
---
# <a name="part-1--creating-a-cross-platform-monogame"></a>Parte 1: crear un MonoGame multiplataforma

_Este tutorial muestra cómo crear un nuevo proyecto para iOS y Android mediante la opción MonoGame. El resultado es un Visual Studio para la solución de Mac con un proyecto de código compartido multiplataforma, así como un proyecto para cada plataforma. Este proyecto mostrará una pantalla azul vacía cuando se ejecuta._

MonoGame permite el desarrollo de juegos multiplataforma con gran parte de reutilización del código. En este tutorial se centrará en configurar una solución que contiene proyectos para iOS y Android, así como un proyecto de código compartido para el código multiplataforma.

Cuando hayamos terminados, comenzaremos tiene un proyecto que tiene la estructura adecuada para realizar la lógica de actualización del juego y lógica de dibujo en 30 fotogramas por segundo del juego. Se puede utilizar como el proyecto de base de cualquier proyecto de MonoGame. Nuestro proyecto tendrá un aspecto similar al siguiente cuando se ejecuta:

![Pantalla azul en blanco](part1-images/image1.png)

## <a name="adding-monogame-to-visual-studio-for-mac"></a>Agregar MonoGame a Visual Studio para Mac

MonoGame puede agregarse como un complemento de Visual Studio para Mac. En el equipo Mac, seleccione **Visual Studio para Mac** > **Administrador de complementos...**  . En Windows, seleccione ** Herramientas ** > **Administrador de complementos...**  . Seleccione el **galería** , expanda la **desarrollo de juegos** categoría y seleccione **MonoGame Addin**, a continuación, haga clic en **instalar**:

![Visual Studio para seleccionar MonoGame Galería de extensiones de Mac](part1-images/image2.png)

> [!IMPORTANT]
> **Tenga en cuenta**: si el **desarrollo de juegos** sección no aparece en el Administrador de complementos, manualmente puede descargar e instalar la versión más reciente desde aquí: http://www.monogame.net/downloads/. Debe reiniciar Visual Studio para Mac para las plantillas que aparecen.

Una vez instalado, MonoGame plantillas aparecerán en Visual Studio para Mac, tal como se verá en la siguiente sección.

## <a name="creating-a-new-solution"></a>Crear una nueva solución

En Visual Studio Seleccione Mac **archivo > nueva solución**. En el **nuevo proyecto** cuadro de diálogo, haga clic en **varios**, desplácese hasta la **General** sección, seleccione la ** aplicación Universal MonoGame Mobile ** opción y haga clic en siguiente.

![Cuadro de diálogo proyecto nuevo crear una aplicación MonoGame](part1-images/image3.png)

Denomine el proyecto WalkingGame y haga clic en crear:

![Nuevo cuadro de diálogo de proyecto eligiendo un nombre y ubicación](part1-images/image4.png)

Ahora se ejecutará nuestro proyecto igual que cualquier otro de proyecto de Android o iOS. Mostrar un fondo azul aciano debe ejecutar el proyecto:

![Fondo de la aplicación azul en blanco](part1-images/image5.png)

## <a name="fixing-android-compile-errors"></a>Corrección de errores de compilación de Android

La versión actual de plantillas de MonoGame incluye algunos errores de sintaxis en el Android `Activity1.cs` archivo. Para solucionar estos problemas, reemplace el `OnCreate` función con los siguientes:

```csharp
protected override void OnCreate (Bundle bundle)
{
    base.OnCreate (bundle);

    var g = new Game1();
    SetContentView((View)g.Services.GetService(typeof(View)));
    g.Run();
}
```

## <a name="summary"></a>Resumen

Este tutorial trata cómo crear un proyecto de MonoGame multiplataforma con Visual Studio para Mac. El resultado de esto es una pantalla azul vacía. Este proyecto se puede utilizar como punto de partida para cualquier iOS y Android juego.

## <a name="related-links"></a>Vínculos relacionados

- [NuGet MonoGame Android](https://www.nuget.org/packages/MonoGame.Framework.Android/)
- [IOS MonoGame NuGet](https://www.nuget.org/packages/MonoGame.Framework.iOS/)
- [Documentación de API de MonoGame](http://www.monogame.net/documentation/?page=main)
