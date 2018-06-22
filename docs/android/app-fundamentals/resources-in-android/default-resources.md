---
title: Recursos predeterminados
ms.prod: xamarin
ms.assetid: 762572F0-173A-D994-0510-8F36BEF3D487
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 03/13/2018
ms.openlocfilehash: 119aa8b967ace858ee56f521624f6356e08a8b80
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 04/04/2018
ms.locfileid: "30765953"
---
# <a name="default-resources"></a>Recursos predeterminados

Recursos predeterminados son elementos que no son específicos a cualquier dispositivo en particular o el factor de forma y, por lo tanto, son la opción predeterminada por el sistema operativo Android si no hay más específica puede encontrar recursos. Por lo tanto, son el tipo más común de recurso que se va a crear. Que están organizados en subdirectorios de la **recursos** directorio según su tipo de recurso:

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Archivos de recursos predeterminados](default-resources-images/01-resource-files-vs.png)

En la imagen anterior, el proyecto tiene valores predeterminados para los recursos con estas características, diseños y valores (archivos XML que contienen valores simples).

A continuación se proporciona una lista completa de tipos de recursos:

-  **animación** &ndash; archivos XML que describen las animaciones de propiedad.
   Animaciones de propiedad se introdujeron en el nivel de API 11 (Android 3.0) y se proporciona para la animación de propiedades en un objeto. Las animaciones de propiedad son una manera más flexible y eficaz para describir las animaciones en cualquier tipo de objeto.

-  **anim** &ndash; archivos XML que describen *interpolación* animaciones. Las animaciones de interpolaciones son una serie de instrucciones de animación para realizar transformaciones en el contenido de una rotación de objeto o de ejemplo, la vista de una imagen o aumentar el tamaño del texto. Las animaciones de interpolaciones están limitadas para ver únicamente los objetos.

-  **color** &ndash; archivos XML que describen una lista de estado de colores. Para entender las listas de estado de color, considere la posibilidad de un widget de interfaz de usuario como un botón.
   Puede tener distintos estados como presionado o deshabilitado y el botón puede cambiar de color con cada cambio de estado. La lista se expresa en una lista de estado.

-  **puede dibujar** &ndash; puede dibujar recursos son un concepto general para los gráficos que se pueden compilar en la aplicación y, a continuación, tiene acceso con llamadas a la API o hace referencia a otros recursos XML.
   Algunos ejemplos de drawables son archivos de mapa de bits (.png, .gif, .jpg), mapas de bits puede cambiar el tamaño especial conocida como [nueve revisiones](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), muestra el estado genéricos formas definidas en XML, etcetera.
 
-  **diseño** &ndash; archivos XML que describen un diseño de interfaz de usuario, por ejemplo, una actividad o una fila de una lista.

-  **menú** &ndash; archivos XML que describen los menús de la aplicación como *opciones menús*, *menús contextuales*, y *submenús*. Para obtener un ejemplo de menús, vea el [demostración del menú emergente](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) o [controles estándar](https://developer.xamarin.com/samples/mobile/StandardControls/) ejemplo.

-  **sin formato** &ndash; archivos arbitrarios que se guardan en su forma sin formato, binario. Estos archivos se compilan en una aplicación de Android en un formato binario.

-  **valores** &ndash; archivos XML que contienen valores simples. Un archivo XML en el directorio de valores no define un único recurso, pero en su lugar, puede definir varios recursos. Por ejemplo, un archivo XML puede albergar una lista de valores de cadena, mientras que otro archivo XML puede albergar una lista de valores de color.

-  **XML** &ndash; archivos XML que son una función similar a los archivos de configuración. NET. Se trata de código XML arbitrario que se pueden leer en tiempo de ejecución por la aplicación


# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio para Mac](#tab/vsmac)

![Archivos de recursos predeterminados](default-resources-images/01-resource-files-xs.png)

En la imagen anterior, el proyecto tiene valores predeterminados para los recursos con estas características, diseños y valores (archivos XML que contienen valores simples).

A continuación se proporciona una lista completa de tipos de recursos:

-  **animación** &ndash; archivos XML que describen las animaciones de propiedad.
   Animaciones de propiedad se introdujeron en el nivel de API 11 (Android 3.0) y se proporciona para la animación de propiedades en un objeto. Las animaciones de propiedad son una manera más flexible y eficaz para describir las animaciones en cualquier tipo de objeto.

-  **anim** &ndash; archivos XML que describen *interpolación* animaciones. Las animaciones de interpolaciones son una serie de instrucciones de animación para realizar transformaciones en el contenido de una rotación de objeto o de ejemplo, la vista de una imagen o aumentar el tamaño del texto. Las animaciones de interpolaciones están limitadas para ver únicamente los objetos.

-  **color** &ndash; archivos XML que describen una lista de estado de colores. Para entender las listas de estado de color, considere la posibilidad de un widget de interfaz de usuario como un botón.
   Puede que tenga tiene distintos estados como presionado o deshabilitado y el botón puede cambiar de color con cada cambio de estado. La lista se expresa en una lista de estado.

-  **fuente** &ndash; a partir de nivel de API 26, es posible incrustar fuentes como un recurso en una aplicación Android. El 26 de biblioteca de soporte técnico le fuentes de atrás a nivel de API 14. La incrustación de fuentes permite a las aplicaciones

-  **asignación MIP** &ndash; puede dibujar recursos son un concepto general para los gráficos que se pueden compilar en la aplicación y, a continuación, tiene acceso con llamadas a la API o hace referencia a otros recursos XML.
   Algunos ejemplos de drawables son archivos de mapa de bits (.png, .gif, .jpg), mapas de bits puede cambiar el tamaño especial conocida como [nueve revisiones](https://developer.android.com/guide/topics/graphics/2d-graphics.html#nine-patch), muestra el estado genéricos formas definidas en XML, etcetera.

-  **diseño** &ndash; archivos XML que describen un diseño de interfaz de usuario, por ejemplo, una actividad o una fila de una lista.

-  **menú** &ndash; archivos XML que describen los menús de la aplicación como *opciones menús*, *menús contextuales*, y *submenús*. Para obtener un ejemplo de menús, vea el [demostración del menú emergente](https://developer.xamarin.com/samples/monodroid/PopupMenuDemo/) o [controles estándar](https://developer.xamarin.com/samples/mobile/StandardControls/) ejemplo.

-  **sin formato** &ndash; archivos arbitrarios que se guardan en su forma sin formato, binario. Estos archivos se compilan en una aplicación de Android en un formato binario.

-  **valores** &ndash; archivos XML que contienen valores simples. Un archivo XML en el directorio de valores no define un único recurso, pero en su lugar, puede definir varios recursos. Por ejemplo, un archivo XML puede albergar una lista de valores de cadena, mientras que otro archivo XML puede albergar una lista de valores de color.

-  **XML** &ndash; archivos XML que son una función similar a los archivos de configuración. NET. Se trata de código XML arbitrario que se pueden leer en tiempo de ejecución por la aplicación

-----
