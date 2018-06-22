---
title: Técnicas de Backgrounding de iOS
description: 'Este documento se incluyen vínculos a guías que describen las distintas técnicas backgrounding en iOS: tareas en segundo plano, servicio transferencia en segundo plano, captura de fondo y las notificaciones remotas.'
ms.prod: xamarin
ms.assetid: 011A8D48-1CDC-486A-A2B0-C4946118E7A9
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: ebf3c07a319a79994093f89f8e54f4cba7402533
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34783760"
---
# <a name="ios-backgrounding-techniques"></a>Técnicas de Backgrounding de iOS

En las secciones siguientes, exploraremos las siguientes características de iOS junto con las opciones de backgrounding existentes:

-  [Tareas en segundo plano oportunista](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background_tasks_in_iOS_7) -preservar la batería mediante la ejecución de tareas en segundo plano en fragmentos oportunistas cuando el dispositivo está activo para otras tareas de procesamiento.
-  [El servicio de transferencia en segundo plano](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md#background-transfers) : forma confiable cargar y descargar archivos independientemente del tamaño de estado o el archivo de red.
-  [Captura de fondo](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#background_fetch) -actualización de una aplicación de segundo plano a intervalos establecidos por el sistema.
-  [Notificaciones remotas](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/updating-an-application-in-the-background.md#remote_notifications) -usar las notificaciones de inserción para desencadenar las actualizaciones de contenido en segundo plano antes de que el usuario abre la aplicación, con una opción para notificar al usuario o actualizar de forma silenciosa.
-  **Las actualizaciones de la interfaz de usuario en segundo plano** : preparar la aplicación de interfaz de usuario para el usuario y actualizar una instantánea de la aplicación, todo ello desde el fondo.
