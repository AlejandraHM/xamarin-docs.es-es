---
title: Enlace de Objective-C
description: Este documento proporciona vínculos a diversas guías que describen cómo crear enlaces de C# para código de Objective-C, permitiendo a los desarrolladores consumir bibliotecas listos para usar en aplicaciones de Xamarin.
ms.prod: xamarin
ms.assetid: DBBAA086-BB0F-8161-DF44-632F4F5DFE5D
author: asb3993
ms.author: amburns
ms.date: 01/25/2016
ms.openlocfilehash: 3f1e1ce324e849c0c939d936eb6ee1470cf24a3b
ms.sourcegitcommit: ec50c626613f2f9af51a9f4a52781129bcbf3fcb
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/05/2018
ms.locfileid: "37855161"
---
# <a name="binding-objective-c"></a>Enlace de Objective-C

Esta sección incluye una variedad de documentos que tratan la creación de enlaces a las bibliotecas de Objective-C, por lo que se puede llamar desde aplicaciones de C# creadas con Xamarin.iOS o Xamarin.Mac.

##  <a name="overviewcross-platformmaciosbindingoverviewmd"></a>[Información general](~/cross-platform/macios/binding/overview.md)

Este documento contiene algunos de los aspectos internos de cómo realiza un enlace. Es un documento con algo de información técnica avanzado.

##  <a name="binding-objective-c-librariescross-platformmaciosbindingobjective-c-librariesmd"></a>[Enlace de bibliotecas de Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)

Este documento describe el proceso usado para crear enlaces de C# de API de Objective-C y cómo se asignan las expresiones en Objective-C para las expresiones que se usan en. NET.
Si va a enlazar sólo las API de C, debe usar el mecanismo estándar de .NET para esto, el marco de trabajo de P/Invoke.

##  <a name="binding-definition-reference-guidecross-platformmaciosbindingbinding-types-referencemd"></a>[Guía de referencia de definición de enlace](~/cross-platform/macios/binding/binding-types-reference.md)

Esta es la Guía de referencia que describe todos los atributos disponibles para los autores de enlace para controlar el proceso de generación de enlace.


## <a name="objective-sharpiecross-platformmaciosbindingobjective-sharpieindexmd"></a>[Objective Sharpie](~/cross-platform/macios/binding/objective-sharpie/index.md)

Sharpie objetivo es una herramienta de línea de comandos para ayudar a arrancar el primer paso de un enlace. Funciona mediante el análisis de los archivos de encabezado de una biblioteca nativa para asignar a la API pública en el [definición de enlace](~/cross-platform/macios/binding/objective-c-libraries.md) (un proceso que también se puede realizar manualmente).

## <a name="ios"></a>iOS

El [página enlace de iOS](~/ios/platform/binding-objective-c/index.md) vínculos volver a estos recursos comunes de enlace, además a los ejemplos siguientes.

### <a name="walkthrough-binding-an-objective-c-libraryiosplatformbinding-objective-cwalkthroughmd"></a>[Tutorial: Enlazar a una biblioteca de Objective-C](~/ios/platform/binding-objective-c/walkthrough.md)

Este artículo proporciona un tutorial paso a paso de creación de un proyecto de enlace con el código abierto [InfColorPicker](https://github.com/InfinitApps/InfColorPicker) proyecto Objective-C como ejemplo. La biblioteca de InfColorPicker proporciona un controlador de vista reutilizables que permiten al usuario seleccionar un color basado en su representación HSB, lo más fácil de usar la selección de color. Objetivo Sharpie se usará para ayudar en el proceso de enlace.

### <a name="binding-sampleshttpsgithubcommonomonotouch-bindings"></a>[Ejemplos de enlace](https://github.com/mono/monotouch-bindings)

Una colección de enlaces de terceros que se puede usa una referencia al crear nuevos proyectos de enlace.

## <a name="mac"></a>Mac

Históricamente [enlace Mac](~/mac/platform/binding.md) ha sido un proceso manual. Actualmente no hay un [preview descargable](https://forums.xamarin.com/discussion/59760/xamarin-mac-binding-project-preview) de soporte técnico de proyecto de enlace de Mac para una versión futura de Visual Studio para Mac.



## <a name="related-links"></a>Vínculos relacionados

- [iOS enlace](~/ios/platform/binding-objective-c/index.md)
- [Enlace de Mac](~/mac/platform/binding.md)
- [Curso de Xamarin University: Creación de una biblioteca de enlaces de Objective-c.](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Curso de Xamarin University: Compilar una biblioteca de enlaces de Objective-C con Sharpie objetivo](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)
