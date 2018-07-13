---
title: Conceptos básicos de mapa de bits de SkiaSharp
description: En este artículo se explica cómo cargar mapas de bits de SkiaSharp procedentes de diversos orígenes y mostrarlos en las aplicaciones de Xamarin.Forms y esto se muestra con código de ejemplo.
ms.prod: xamarin
ms.technology: xamarin-forms
ms.assetid: 32C95DFF-9065-42D7-966C-D3DBD16906B3
author: charlespetzold
ms.author: chape
ms.date: 04/03/2017
ms.openlocfilehash: dec6fa1534f14836ae98677ad33e280ff510fb97
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995195"
---
# <a name="bitmap-basics-in-skiasharp"></a>Conceptos básicos de mapa de bits de SkiaSharp

_Cargar mapas de bits de varios orígenes y mostrarlos._

La compatibilidad de mapas de bits de SkiaSharp es bastante extensa. Este artículo trata solo de los conceptos básicos &mdash; cargar mapas de bits y cómo mostrarlos:

![](bitmaps-images/bitmapssample.png "La presentación de dos mapas de bits")

Un mapa de bits de SkiaSharp es un objeto de tipo [ `SKBitmap` ](https://developer.xamarin.com/api/type/SkiaSharp.SKBitmap/). Hay muchas maneras de crear un mapa de bits, pero en este artículo se restringe a sí mismo a la [ `SKBitmap.Decode` ](https://developer.xamarin.com/api/member/SkiaSharp.SKBitmap.Decode/p/SkiaSharp.SKStream/) método, que carga el mapa de bits desde un [ `SKStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKStream/) objeto que hace referencia a un archivo de mapa de bits. Es conveniente usar el [ `SKManagedStream` ](https://developer.xamarin.com/api/type/SkiaSharp.SKManagedStream/) clase que derive de `SKStream` porque tiene un constructor que acepta un .NET [ `Stream` ](xref:System.IO.Stream) objeto.

El **básica de mapas de bits** página en el **SkiaSharpFormsDemos** programa muestra cómo cargar mapas de bits de tres orígenes diferentes:

- Más de Internet
- Desde un recurso incrustado en el archivo ejecutable
- Desde la biblioteca de fotos del usuario

Tres `SKBitmap` objetos para estos tres orígenes se definen como campos en el [ `BasicBitmapsPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/BasicBitmapsPage.cs) clase:

```csharp
public class BasicBitmapsPage : ContentPage
{
    SKCanvasView canvasView;
    SKBitmap webBitmap;
    SKBitmap resourceBitmap;
    SKBitmap libraryBitmap;

    public BasicBitmapsPage()
    {
        Title = "Basic Bitmaps";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
        ...
    }
    ...
}
```

## <a name="loading-a-bitmap-from-the-web"></a>Carga un mapa de bits de la Web

Para cargar un mapa de bits en función de una dirección URL, puede usar el [ `WebRequest` ](xref:System.Net.WebRequest) clase, como se muestra en el siguiente código ejecutado en el `BasicBitmapsPage` constructor. Aquí la dirección URL apunta a un área en el sitio web de Xamarin con algunos mapas de bits de ejemplo. Un paquete en el sitio web permite anexar una especificación para cambiar el tamaño de mapa de bits a un ancho determinado:

```csharp
Uri uri = new Uri("http://developer.xamarin.com/demo/IMG_3256.JPG?width=480");
WebRequest request = WebRequest.Create(uri);
request.BeginGetResponse((IAsyncResult arg) =>
{
    try
    {
        using (Stream stream = request.EndGetResponse(arg).GetResponseStream())
        using (MemoryStream memStream = new MemoryStream())
        {
            stream.CopyTo(memStream);
            memStream.Seek(0, SeekOrigin.Begin);

            using (SKManagedStream skStream = new SKManagedStream(memStream))
            {
                webBitmap = SKBitmap.Decode(skStream);
            }
        }
    }
    catch
    {
    }

    Device.BeginInvokeOnMainThread(() => canvasView.InvalidateSurface());

}, null);
```

Cuando el mapa de bits se ha descargado correctamente, se pasa al método de devolución de llamada para el `BeginGetResponse` ejecuciones del método. El `EndGetResponse` llamada debe estar en un `try` bloquear en caso de que se ha producido un error. El `Stream` objeto obtenida `GetResponseStream` no es adecuada en algunas plataformas, por lo que se copia el contenido de mapa de bits en un `MemoryStream` objeto. En este momento, el `SKManagedStream` se puede crear el objeto. Esto ahora hace referencia al archivo de mapa de bits, que es probable que un archivo JPEG o PNG. El `SKBitmap.Decode` método descodifica el archivo de mapa de bits y almacena los resultados en un formato interno de SkiaSharp.

El método de devolución de llamada se pasa a `BeginGetResponse` se ejecuta después de que el constructor ha terminado de ejecutarse, lo que significa que el `SKCanvasView` tiene que invalidar para permitir el `PaintSurface` controlador para actualizar la pantalla. Sin embargo, el `BeginGetResponse` devolución de llamada se ejecuta en un subproceso secundario de la ejecución, por lo que es necesario utilizar `Device.BeginInvokeOnMainThread` para ejecutar el `InvalidateSurface` método en el subproceso de interfaz de usuario.

## <a name="loading-a-bitmap-resource"></a>Cargar un recurso de mapa de bits

En términos de código, el enfoque más sencillo a la carga de los mapas de bits está incluido un recurso de mapa de bits directamente en la aplicación. El **SkiaSharpFormsDemos** programa incluye una carpeta denominada **Media** que contiene un archivo de mapa de bits llamado **monkey.png**. En el **propiedades** cuadro de diálogo para este archivo, debe asignar a este archivo una **acción de compilación** de **recurso incrustado**!

Cada recurso incrustado tiene un *Id. de recurso* formado por el nombre del proyecto, la carpeta y el nombre de archivo, todo conectado por puntos: **SkiaSharpFormsDemos.Media.monkey.png**. Puede obtener acceso a este recurso mediante la especificación de ese recurso identificador como argumento a la [ `GetManifestResourceStream` ](xref:System.Reflection.Assembly.GetManifestResourceStream(System.String)) método de la [ `Assembly` ](xref:System.Reflection.Assembly) clase:

```csharp
string resourceID = "SkiaSharpFormsDemos.Media.monkey.png";
Assembly assembly = GetType().GetTypeInfo().Assembly;

using (Stream stream = assembly.GetManifestResourceStream(resourceID))
using (SKManagedStream skStream = new SKManagedStream(stream))
{
    resourceBitmap = SKBitmap.Decode(skStream);
}
```

Esto `Stream` objeto se puede convertir directamente a un `SKManagedStream` objeto.

## <a name="loading-a-bitmap-from-the-photo-library"></a>Cargar un mapa de bits de la biblioteca de fotografías

También es posible que el usuario cargar una foto de la biblioteca de imágenes del dispositivo. Esta función no se proporciona mediante Xamarin.Forms propio. El trabajo requiere un servicio de dependencia, como se describe en el artículo [seleccionar una foto de la biblioteca de imágenes](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md).

El **IPicturePicker.cs** archivo y los tres **PicturePickerImplementation.cs** se han copiado los archivos de ese artículo a los distintos proyectos de la **SkiaSharpFormsDemos**solución y les da un nombre de espacio de nombres nuevo. Además, el Android **MainActivity.cs** se ha modificado el archivo como se describe en el artículo y el proyecto de iOS se haya concedido permiso para acceder a la biblioteca de fotos con dos líneas hacia la parte inferior de la **info.plist**  archivo.

El `BasicBitmapsPage` constructor agrega un `TapGestureRecognizer` a la `SKCanvasView` para recibir una notificación de pesos. Tras la recepción de un toque, el `Tapped` controlador obtiene acceso al servicio de dependencia de selector de imagen y las llamadas `GetImageStreamAsync`. Si un `Stream` se devuelve el objeto y, después, el contenido se copia en un `MemoryStream`, según sea necesario por algunas de las plataformas. El resto del código es similar a las otras dos técnicas:

```csharp
// Add tap gesture recognizer
TapGestureRecognizer tapRecognizer = new TapGestureRecognizer();
tapRecognizer.Tapped += async (sender, args) =>
{
    // Load bitmap from photo library
    IPicturePicker picturePicker = DependencyService.Get<IPicturePicker>();

    using (Stream stream = await picturePicker.GetImageStreamAsync())
    {
        if (stream != null)
        {
            using (MemoryStream memStream = new MemoryStream())
            {
                stream.CopyTo(memStream);
                memStream.Seek(0, SeekOrigin.Begin);

                using (SKManagedStream skStream = new SKManagedStream(memStream))
                {
                    libraryBitmap = SKBitmap.Decode(skStream);
                }
            }
            canvasView.InvalidateSurface();
        }
    }
};
canvasView.GestureRecognizers.Add(tapRecognizer);
```

Tenga en cuenta que el `Tapped` llamadas del controlador de la `InvalidateSurface` método de la `SKCanvasView` objeto. Esto genera una nueva llamada a la `PaintSurface` controlador.

## <a name="displaying-the-bitmaps"></a>Mostrar los mapas de bits

El `PaintSurface` controlador necesita para mostrar tres mapas de bits. El controlador se da por supuesto que el teléfono está en modo vertical y el lienzo divide verticalmente en tres partes iguales.

Se muestra el mapa de bits primero con el más sencillo [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/System.Single/System.Single/SkiaSharp.SKPaint/) método. Todo lo que necesita especificar son las coordenadas X e Y donde es se coloca la esquina superior izquierda del mapa de bits:

```csharp
public void DrawBitmap (SKBitmap bitmap, Single x, Single y, SKPaint paint = null)
```

Aunque un `SKPaint` parámetro está definido, tiene un valor predeterminado de `null` y puede ignorarlo. Los píxeles del mapa de bits simplemente se transfieren a los píxeles de la superficie de pantalla con una asignación uno a uno.

Un programa puede obtener las dimensiones en píxeles del mapa de bits con el [ `Width` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Width/) y [ `Height` ](https://developer.xamarin.com/api/property/SkiaSharp.SKBitmap.Height/) propiedades. Estas propiedades permiten que el programa calcular las coordenadas para colocar el mapa de bits en el centro de la tercera parte superior del lienzo:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    if (webBitmap != null)
    {
        float x = (info.Width - webBitmap.Width) / 2;
        float y = (info.Height / 3 - webBitmap.Height) / 2;
        canvas.DrawBitmap(webBitmap, x, y);
    }
    ...
}
```

Los otros dos mapas de bits se muestran con una versión de [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKPaint/) con un `SKRect` parámetro:

```csharp
public void DrawBitmap (SKBitmap bitmap, SKRect dest, SKPaint paint = null)
```

Una tercera versión de [ `DrawBitmap` ](https://developer.xamarin.com/api/member/SkiaSharp.SKCanvas.DrawBitmap/p/SkiaSharp.SKBitmap/SkiaSharp.SKRect/SkiaSharp.SKRect/SkiaSharp.SKPaint/) tiene dos `SKRect` argumentos para especificar un subconjunto rectangular del mapa de bits para mostrar, pero esa versión no se usa en este artículo.

Este es el código para mostrar el mapa de bits que se cargan desde un mapa de bits del recurso incrustado:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (resourceBitmap != null)
    {
        canvas.DrawBitmap(resourceBitmap,
            new SKRect(0, info.Height / 3, info.Width, 2 * info.Height / 3));
    }
    ...
}
```

El mapa de bits se ajusta a las dimensiones del rectángulo, que es el motivo por el objeto monkey se expande horizontalmente en estas capturas de pantalla:

[![](bitmaps-images/basicbitmaps-small.png "Captura de pantalla triple de la página de mapas de bits básica")](bitmaps-images/basicbitmaps-large.png#lightbox "triple captura de pantalla de la página de mapas de bits básica")

La tercera imagen &mdash; que solo puede ver si ejecuta el programa y cargar una foto de su propia biblioteca de imágenes &mdash; también se muestra dentro de un rectángulo, pero el rectángulo se ajustan el tamaño y posición para mantener la relación de aspecto del mapa de bits. Este cálculo es un poco más complicado porque requiere calcular un factor en función del tamaño del mapa de bits y el rectángulo de destino de escala y centrar el rectángulo en esa área:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    if (libraryBitmap != null)
    {
        float scale = Math.Min((float)info.Width / libraryBitmap.Width,
                               info.Height / 3f / libraryBitmap.Height);

        float left = (info.Width - scale * libraryBitmap.Width) / 2;
        float top = (info.Height / 3 - scale * libraryBitmap.Height) / 2;
        float right = left + scale * libraryBitmap.Width;
        float bottom = top + scale * libraryBitmap.Height;
        SKRect rect = new SKRect(left, top, right, bottom);
        rect.Offset(0, 2 * info.Height / 3);

        canvas.DrawBitmap(libraryBitmap, rect);
    }
    else
    {
        using (SKPaint paint = new SKPaint())
        {
            paint.Color = SKColors.Blue;
            paint.TextAlign = SKTextAlign.Center;
            paint.TextSize = 48;

            canvas.DrawText("Tap to load bitmap",
                info.Width / 2, 5 * info.Height / 6, paint);
        }
    }
}
```

Si ningún mapa de bits se ha cargado desde la biblioteca de imágenes, el `else` bloque muestra algún texto para pedir al usuario que pulse en la pantalla.


## <a name="related-links"></a>Vínculos relacionados

- [API de SkiaSharp](https://developer.xamarin.com/api/root/SkiaSharp/)
- [SkiaSharpFormsDemos (ejemplo)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
- [Seleccionar una foto de la biblioteca de imágenes](~/xamarin-forms/app-fundamentals/dependency-service/photo-picker.md)
