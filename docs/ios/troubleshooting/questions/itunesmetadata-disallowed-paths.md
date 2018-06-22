---
title: '¿Por qué se realiza el envío de mi aplicación por error con: no se permiten rutas de acceso (iTunesMetadata.plist) encontrados en?'
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: AE1BBDC6-4D3A-4471-876B-FE28B6E59A24
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: 3b53a967a522c63413bb136fa4d3622c6f11ef16
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2018
ms.locfileid: "30778167"
---
# <a name="why-does-my-app-submission-fail-with-disallowed-paths--itunesmetadataplist--found-at--"></a>¿Por qué se realiza el envío de mi aplicación por error con: no se permiten rutas de acceso (iTunesMetadata.plist) encontrados en?

> ERROR: ERROR ITMS-90047: "no se permiten rutas de acceso ("iTunesMetadata.plist") se encuentra en: Payload/iPhoneApp1.app"

Este error es el resultado de un cambio en el proceso de comprobación de App Store de Apple para impedir que los usuarios teniendo problemas como [error 29180](https://bugzilla.xamarin.com/show_bug.cgi?id=29180). Este error específico es _no_ relacionado con la versión concreta de Xamarin que se ha instalado, por lo que degradar le _no_ ayuda.

Vea el foro de debate [aquí](https://forums.xamarin.com/discussion/40388/disallowed-paths-itunesmetadata-plist-found-at-when-submitting-to-app-store/p1) para obtener información de solución y las actualizaciones más recientes para este problema.
