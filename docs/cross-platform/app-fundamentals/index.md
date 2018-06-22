---
title: Para compartir el código en varias plataformas
description: Vínculos de este documento diversas guías que describen técnicas para compartir código, incluidas las bibliotecas de clases portables, los proyectos compartidos, estándar de .NET y NuGet.
ms.prod: xamarin
ms.assetid: 7D179ACF-09A6-46EE-B49D-E27AB5F09CD4
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 61377afa61e2c2006c2fdf8ef9b21fe7d567b3de
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780079"
---
# <a name="sharing-code-on-multiple-platforms"></a>Para compartir el código en varias plataformas

Esta sección proporciona a una guía sobre algunas de las tareas de cosas o conceptos que los desarrolladores deben tener en cuenta al desarrollar aplicaciones móviles más comunes.

## <a name="code-sharing-overviewcode-sharingmd"></a>[Introducción al uso compartido de código](code-sharing.md)

Obtenga información acerca del código de diferentes opciones disponibles para los proyectos de Xamarin, incluidas las bibliotecas de clases portables (PCLs), proyectos compartidos y bibliotecas estándar de .NET de uso compartido.


##  <a name="portable-class-librariescross-platformapp-fundamentalspclmd"></a>[Bibliotecas de clases portables](~/cross-platform/app-fundamentals/pcl.md)

Proyectos de biblioteca de clases Portable le permiten crear y distribuir ensamblados que contienen código compartido que se ejecuta en varias plataformas. Para crear una biblioteca de clases Portable (o "PCL") selecciona qué plataformas de destino y, después, escribir el código en un subjuego de .NET Framework que está disponible en el perfil definido para esas plataformas. Este documento describe cómo crear y usar PCLs con Xamarin.

##  <a name="shared-projectscross-platformapp-fundamentalsshared-projectsmd"></a>[Proyectos compartidos](~/cross-platform/app-fundamentals/shared-projects.md)

Los proyectos compartidos le permiten escribir el código común que se hace referencia mediante un número de proyectos de aplicación diferente. El código se compila como parte de cada proyecto que hace referencia y puede incluir directivas de compilador para ayudar a incorporar la funcionalidad específica de la plataforma en la base de código compartido. Este artículo describe cómo funcionan los proyectos compartidos y cómo crearlos y utilizarlos con los proyectos de Xamarin.

##  <a name="net-standardcross-platformapp-fundamentalsnet-standardmd"></a>[.NET Standard](~/cross-platform/app-fundamentals/net-standard.md)

Estándar de .NET es una opción nueva para compartir código entre plataformas. Funciona de forma similar a las bibliotecas de clases portables; código se basa en una versión específica (actualmente 1.0 a 1.6) y, a continuación, puede ser utilizado por otros proyectos que admitan ese nivel o superior. Proyectos de .NET estándar son compatibles con Xamarin Studio 6.2, Visual Studio para Windows y Visual Studio para Mac.

##  <a name="nuget-projects-multiplatform-libraries-for-code-sharingcross-platformapp-fundamentalsnuget-multiplatform-librariesindexmd"></a>[Proyectos de NuGet: Bibliotecas multiplataforma para uso compartido de código](~/cross-platform/app-fundamentals/nuget-multiplatform-libraries/index.md)

Se pueden generar automáticamente los paquetes de NuGet de PCL o .NET proyectos estándares; y proyectos compartidos pueden empaquetarse en paquetes de NuGet "gancho" con el tipo de proyecto de NuGet independiente. Esta sección explica cómo crear paquetes de NuGet para cada escenario de uso compartido de código.

##  <a name="manually-creating-nuget-packages-for-xamarincross-platformapp-fundamentalsnuget-manualmd"></a>[Creación manual de paquetes de NuGet para Xamarin](~/cross-platform/app-fundamentals/nuget-manual.md)

Sugerencias para crear paquetes de NuGet que funcionan con la plataforma de Xamarin.
