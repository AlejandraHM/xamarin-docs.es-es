---
title: Aplicaciones de Xamarin.Forms de estilo con estilos de XAML
description: Esta guía explica cómo personalizar la apariencia de una aplicación de Xamarin.Forms mediante estilos XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f439e3ba888b67ac1752eae95149adcf55055943
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245877"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Aplicaciones de Xamarin.Forms de estilo con estilos de XAML

## <a name="introductionintroductionmd"></a>[Introducción](introduction.md)

Aplicaciones de Xamarin.Forms suelen contengan varios controles que tienen un aspecto idéntico. Configuración de la apariencia de cada control individual puede ser repetitiva y es proclive a errores. En su lugar, estilos pueden crearse que personalizan la apariencia de los controles por agrupación y propiedades de configuración disponibles en el tipo de control.

## <a name="explicit-stylesexplicitmd"></a>[Estilos explícitos](explicit.md)

Un *explícita* estilo es aquel que se aplica de manera selectiva a los controles estableciendo sus [ `Style` ](https://developer.xamarin.com/api/property/Xamarin.Forms.VisualElement.Style/) propiedades.

## <a name="implicit-stylesimplicitmd"></a>[Estilos implícitos](implicit.md)

Un *implícita* estilo es aquel que se usa por todos los controles de la misma [ `TargetType` ](https://developer.xamarin.com/api/property/Xamarin.Forms.Style.TargetType/), sin necesidad de cada control para hacer referencia al estilo.

## <a name="global-stylesapplicationmd"></a>[Estilos globales](application.md)

Estilos pueden ponerse a disposición globalmente agregándolos a la aplicación [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/). Esto ayuda a evitar la duplicación de estilos a través de páginas o controles.

## <a name="style-inheritanceinheritancemd"></a>[Herencia de estilo](inheritance.md)

Estilos pueden heredar de otros estilos para reducir la duplicación y volver a usar.

## <a name="dynamic-stylesdynamicmd"></a>[Estilos dinámicos](dynamic.md)

Estilos no responder a los cambios de propiedad y permanecen sin cambios para la duración de una aplicación. Sin embargo, las aplicaciones pueden responder a los cambios de estilo dinámicamente en tiempo de ejecución mediante el uso de recursos dinámicos.

## <a name="device-stylesdevicemd"></a>[Estilos de dispositivo](device.md)

Xamarin.Forms incluye seis *dinámica* estilos, conocidos como *dispositivo* estilos, en la [ `Devices.Styles` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Device+Styles/) clase. Todos los seis estilos pueden aplicarse a [ `Label` ](https://developer.xamarin.com/api/type/Xamarin.Forms.Label/) solo instancias.
