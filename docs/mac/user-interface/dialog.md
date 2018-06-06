---
title: Cuadros de diálogo en Xamarin.Mac
description: Este artículo explica cómo trabajar con cuadros de diálogo y ventanas modales en una aplicación Xamarin.Mac. Describe cómo crear ventanas modales en Xcode y la interfaz de generador, trabajar con cuadros de diálogo estándar e interactuar con estos controles en código C#.
ms.prod: xamarin
ms.assetid: 55451990-B77B-4D44-B8BB-F874EC503B0C
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 7d9a93c8503d7e25f098e871378a22455b597e90
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: es-ES
ms.lasthandoff: 06/05/2018
ms.locfileid: "34792699"
---
# <a name="dialogs-in-xamarinmac"></a>Cuadros de diálogo en Xamarin.Mac

Cuando se trabaja con C# y .NET en una aplicación Xamarin.Mac, tendrá acceso a los mismos cuadros de diálogo y ventanas modales que un desarrollador que trabaja *Objective-C* y *Xcode* does. Dado que Xamarin.Mac se integra directamente en Xcode, puede usar del Xcode _interfaz generador_ para crear y mantener las ventanas modales (o si lo desea crearlos directamente en código de C#).

Un cuadro de diálogo aparece en respuesta a una acción del usuario y normalmente se proporciona a los usuarios de maneras pueden completar la acción. Un cuadro de diálogo requiere una respuesta del usuario antes de poder cerrar.

Windows puede ser usado en un estado no modal (por ejemplo, un editor de texto que puede tener varios documentos abiertos a la vez) o Modal (por ejemplo, un cuadro de diálogo de exportación que se debe descartar para que pueda continuar la aplicación).

[![](dialog-images/dialog03.png "Un cuadro de diálogo Abrir")](dialog-images/dialog03.png#lightbox)

En este artículo, se tratarán los conceptos básicos sobre cómo trabajar con cuadros de diálogo y ventanas modales en una aplicación Xamarin.Mac. Se recomienda trabajar a través de la [Hola, Mac](~/mac/get-started/hello-mac.md) artículo en primer lugar, específicamente el [Introducción a Xcode y el generador de interfaz](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) y [distribuidores y acciones](~/mac/get-started/hello-mac.md#Outlets_and_Actions) secciones, tal como se explica conceptos clave y técnicas que usaremos en este artículo.

Puede echar un vistazo a la [clases de C# exponer / métodos para Objective-C](~/mac/internals/how-it-works.md) sección de la [Xamarin.Mac Internals](~/mac/internals/how-it-works.md) documento también se explica la `Register` y `Export` comandos Usar conexión de seguridad de las clases de C# para Objective-C objetos y elementos de interfaz de usuario.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Introducción a los cuadros de diálogo

Un cuadro de diálogo aparece en respuesta a una acción del usuario (por ejemplo, para guardar un archivo) y proporciona una manera para que los usuarios realizar esa acción. Un cuadro de diálogo requiere una respuesta del usuario antes de poder cerrar.

Función de Apple, hay tres formas de presentar un cuadro de diálogo:

- **Documentar Modal** -cuadro de diálogo Modal de un documento impide que el usuario realice cualquier otra cosa dentro de un documento determinado hasta que se cierra.
- **Aplicación Modal** : una aplicación de cuadro de diálogo Modal impide al usuario interactuar con la aplicación hasta que se cierra.
- **No modal** el cuadro de diálogo de A no modal permite a los usuarios cambiar la configuración en el cuadro de diálogo mientras todavía interactuar con la ventana de documento.

### <a name="modal-window"></a>Ventana modal

Norma `NSWindow` puede usarse como un cuadro de diálogo personalizado al mostrar de forma modal:

[![](dialog-images/modal01.png "Una ventana modal de ejemplo")](dialog-images/modal01.png#lightbox)

### <a name="document-modal-dialog-sheets"></a>Hojas de cuadro de diálogo Modal de documento

A _hoja_ es un cuadro de diálogo modal que se adjunta a una ventana de documento determinado, impide que los usuarios interactuar con la ventana hasta que descarte el cuadro de diálogo. Una hoja se adjunta a la ventana desde la que se desprende y solo una hoja puede estar abierta para una ventana en cualquier momento.

[![](dialog-images/sheet08.png "Un ejemplo de hoja de modal")](dialog-images/sheet08.png#lightbox)

### <a name="preferences-windows"></a>Ventanas Preferencias

Una ventana de preferencias es un cuadro de diálogo no modal que contiene la configuración de la aplicación que el usuario cambia con poca frecuencia. A menudo, las preferencias de Windows incluyen una barra de herramientas que permite al usuario cambiar entre distintos grupos de configuración:

[![](dialog-images/dialog02.png "Una ventana de preferencias de ejemplo")](dialog-images/dialog02.png#lightbox)

### <a name="open-dialog"></a>Cuadro de diálogo Abrir

El cuadro de diálogo Abrir proporciona a los usuarios una manera coherente para buscar y abrir un elemento en una aplicación:

[![](dialog-images/dialog03.png "Un cuadro de diálogo Abrir")](dialog-images/dialog03.png#lightbox)


### <a name="print-and-page-setup-dialogs"></a>Impresión y los cuadros de diálogo Configurar página

macOS proporciona estándar de impresión y página Configuración de cuadros de diálogo que puede mostrar la aplicación para que los usuarios pueden tener una impresión coherente en todas las aplicaciones que utilizan la experiencia.

El cuadro de diálogo de impresión se pueden mostrar como ambos un cuadro de diálogo flotante gratuita:

[![](dialog-images/print01.png "Un cuadro de diálogo de impresión")](dialog-images/print01.png#lightbox)

O bien, puede mostrarse como una hoja:

[![](dialog-images/print02.png "Una hoja de impresión")](dialog-images/print02.png#lightbox)

El cuadro de diálogo de configuración de página se pueden mostrar como ambos un cuadro de diálogo flotante gratuita:

[![](dialog-images/print03.png "Un cuadro de diálogo de configuración de página")](dialog-images/print03.png#lightbox)

O bien, puede mostrarse como una hoja:

[![](dialog-images/print04.png "Una hoja de configuración de página")](dialog-images/print04.png#lightbox)

### <a name="save-dialogs"></a>Guardar los cuadros de diálogo

El cuadro de diálogo Guardar ofrece a los usuarios una manera coherente para guardar un elemento en una aplicación. El cuadro de diálogo Guardar tiene dos estados: **mínimo** (también conocido como contraído):

[![](dialog-images/save01.png "Un diálogo Guardar")](dialog-images/save01.png#lightbox)

Y el **expandida** estado:

[![](dialog-images/save02.png "Cuadro de diálogo Guardar expandido")](dialog-images/save02.png#lightbox)

El **mínimo** también se pueden mostrar el cuadro de diálogo Guardar como una hoja:

[![](dialog-images/save03.png "Un número mínimo de guardado hoja")](dialog-images/save03.png#lightbox)

Igual que el **expandida** cuadro de diálogo Guardar:

[![](dialog-images/save04.png "Guarde la hoja expandido")](dialog-images/save04.png#lightbox)

Para obtener más información, consulte el [cuadros de diálogo](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) sección de Apple [directrices de interfaz humana de OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Agregar una ventana Modal a un proyecto

Además de la ventana del documento principal, una aplicación Xamarin.Mac deba mostrar otros tipos de windows para el usuario, como las preferencias de Inspector de paneles.

Para agregar una nueva ventana, haga lo siguiente:

1. En el **el Explorador de soluciones**, abra el `Main.storyboard` archivo para su edición en el generador de interfaz de Xcode.
2. Arrastre una nueva **View Controller** en la superficie de diseño:

    [![](dialog-images/new01.png "Seleccionar un controlador de vista de la biblioteca")](dialog-images/new01.png#lightbox)
3. En el **identidad Inspector**, escriba `CustomDialogController` para el **nombre de la clase**: 

    [![](dialog-images/new02.png "Establecer el nombre de clase")](dialog-images/new02.png#lightbox)
4. Cambie a Visual Studio para Mac, permitir que se sincronice con Xcode y crear el `CustomDialogController.h` archivo.
5. Volver a Xcode y diseñar la interfaz: 

    [![](dialog-images/new03.png "Diseñar la interfaz de usuario en Xcode.")](dialog-images/new03.png#lightbox)
6. Crear un **desplazará tranquilamente Modal** desde la ventana principal de la aplicación en el nuevo controlador de vista, arrastre control desde el elemento de interfaz de usuario que se abrirá el cuadro de diálogo para la ventana del cuadro de diálogo. Asigne el **identificador** `ModalSegue`: 

    [![](dialog-images/new06.png "Un segue modal")](dialog-images/new06.png#lightbox)
6. El cable telefónico cualquiera **acciones** y **tomas**: 

    [![](dialog-images/new04.png "Configurar una acción")](dialog-images/new04.png#lightbox)
6. Guarde los cambios y vuelva a Visual Studio para Mac para la sincronización con Xcode.

Realizar el `CustomDialogController.cs` archivo aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Este código expone algunas propiedades para establecer el título y la descripción del cuadro de diálogo y algunos eventos para reaccionar ante el cuadro de diálogo que se va a cancelar o aceptado.

A continuación, edite la `ViewController.cs` de archivos, invalide el `PrepareForSegue` método y darle un aspecto similar al siguiente:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Este código inicializa el segue que hemos definido en el generador de interfaz de Xcode a nuestro cuadro de diálogo y establece el título y la descripción. También controla la opción que hace que el usuario en el cuadro de diálogo.

Podemos ejecutar la aplicación y mostrar el cuadro de diálogo personalizado:

[![](dialog-images/new05.png "Un cuadro de diálogo de ejemplo")](dialog-images/new05.png#lightbox)

Para obtener más información acerca del uso de windows en una aplicación Xamarin.Mac, visite nuestro [trabajar con ventanas](~/mac/user-interface/window.md) documentación.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Crear una hoja de personalizado

A _hoja_ es un cuadro de diálogo modal que se adjunta a una ventana de documento determinado, impide que los usuarios interactuar con la ventana hasta que descarte el cuadro de diálogo. Una hoja se adjunta a la ventana desde la que se desprende y solo una hoja puede estar abierta para una ventana en cualquier momento. 

Para crear una hoja de personalizado en Xamarin.Mac, vamos a hacer lo siguiente:

1. En el **el Explorador de soluciones**, abra el `Main.storyboard` archivo para su edición en el generador de interfaz de Xcode.
2. Arrastre una nueva **View Controller** en la superficie de diseño:

    [![](dialog-images/new01.png "Seleccionar un controlador de vista de la biblioteca")](dialog-images/new01.png#lightbox)
2. Diseñar la interfaz de usuario:

    [![](dialog-images/sheet01.png "El diseño de interfaz de usuario")](dialog-images/sheet01.png#lightbox)
3. Crear un **hoja desplazará tranquilamente** desde la ventana principal en el nuevo controlador de vista: 

    [![](dialog-images/sheet02.png "Seleccionar el tipo de segue hoja")](dialog-images/sheet02.png#lightbox)
4. En el **identidad Inspector**, el nombre del controlador de vista **clase** `SheetViewController`: 

    [![](dialog-images/sheet03.png "Establecer el nombre de clase")](dialog-images/sheet03.png#lightbox)
5. Cualquiera sea necesario definir **tomas** y **acciones**: 

    [![](dialog-images/sheet04.png "Definir las salidas y las acciones necesarias")](dialog-images/sheet04.png#lightbox)
6. Guarde los cambios y vuelva a Visual Studio para Mac sincronizar.

A continuación, edite la `SheetViewController.cs` de archivos y darle un aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

A continuación, edite la `ViewController.cs` de archivos, edite el `PrepareForSegue` método y darle un aspecto similar al siguiente:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Si se ejecuta la aplicación y abra la hoja, se adjuntará a la ventana:

[![](dialog-images/sheet08.png "Una hoja de ejemplo")](dialog-images/sheet08.png#lightbox)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Crear un cuadro de diálogo de preferencias

Antes de que se estructurará la vista de preferencia en el generador de interfaz, necesitamos agregar un tipo de segue personalizado para controlar el cambio de las preferencias. Agregue una nueva clase al proyecto y llámelo `ReplaceViewSeque `. Modifique la clase y darle un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Con segue personalizado que se creó, podemos agregar una nueva ventana en el generador de interfaz de Xcode para administrar nuestras preferencias.

Para agregar una nueva ventana, haga lo siguiente:

1. En el **el Explorador de soluciones**, abra el `Main.storyboard` archivo para su edición en el generador de interfaz de Xcode.
2. Arrastre una nueva **ventana controlador** en la superficie de diseño:

    [![](dialog-images/pref01.png "Seleccione un controlador de la ventana de la biblioteca")](dialog-images/pref01.png#lightbox)
3. Organizar la ventana cerca de la **barra de menús** diseñador:

    [![](dialog-images/pref02.png "Agregar la nueva ventana")](dialog-images/pref02.png#lightbox)
4. Crear copias del controlador de vista adjunto como habrá pestañas en la vista de preferencia:

    [![](dialog-images/pref03.png "Agregar los controladores necesarios de vista")](dialog-images/pref03.png#lightbox)
5. Arrastre una nueva **controlador de la barra de herramientas** desde el **biblioteca**:

    [![](dialog-images/pref04.png "Seleccione un controlador de la barra de herramientas de la biblioteca")](dialog-images/pref04.png#lightbox)
6. Y colóquela en la ventana en la superficie de diseño:

    [![](dialog-images/pref05.png "Agregar un nuevo controlador de la barra de herramientas")](dialog-images/pref05.png#lightbox)
7. El diseño de la barra de herramientas de diseño:

    [![](dialog-images/pref06.png "Diseño de la barra de herramientas")](dialog-images/pref06.png#lightbox)
8. Control y haga clic y arrastre desde cada uno de ellos **botón de barra de herramientas** a las vistas que creó anteriormente. Seleccione un **personalizado** desplazará tranquilamente tipo:

    [![](dialog-images/pref07.png "Establecer el tipo de segue")](dialog-images/pref07.png#lightbox)
9. Seleccione el nuevo Segue y establezca el **clase** a `ReplaceViewSegue`:

    [![](dialog-images/pref08.png "Configuración de la clase segue")](dialog-images/pref08.png#lightbox)
10. En el **Menubar diseñador** en la superficie de diseño, en el menú de la aplicación seleccione **preferencias...** , control y haga clic y arrastre hasta la ventana de preferencias para crear un **mostrar** desplazará tranquilamente:

    [![](dialog-images/pref09.png "Establecer el tipo de segue")](dialog-images/pref09.png#lightbox)
11. Guarde los cambios y vuelva a Visual Studio para Mac sincronizar.

Si se ejecuta el código y seleccione el **preferencias...**  desde el **menú aplicación**, se mostrará la ventana:

[![](dialog-images/pref10.png "Una ventana de preferencias de ejemplo")](dialog-images/pref10.png#lightbox)

Para obtener más información sobre cómo trabajar con ventanas y barras de herramientas, visite nuestro [Windows](~/mac/user-interface/window.md) y [las barras de herramientas](~/mac/user-interface/toolbar.md) documentación.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Guardar y cargar las preferencias

En una aplicación de macOS típico, cuando el usuario realice cambios en cualquiera de las preferencias del usuario de la aplicación, los cambios se guardan automáticamente. La manera más fácil de controlar esto en una aplicación Xamarin.Mac, es crear una sola clase para administrar todas las preferencias del usuario y compartirlo todo el sistema.

En primer lugar, agregue un nuevo `AppPreferences` clase al proyecto y hereda de `NSObject`. Las preferencias que se va a diseñar para usar [enlace de datos y valores de clave de codificación](~/mac/app-fundamentals/databinding.md) que hará que el proceso de creación y mantenimiento de la preferencia de forma mucho más fácil. Puesto que las preferencias constará de una pequeña cantidad de tipos de datos simples, usar el elemento integrado en `NSUserDefaults` para almacenar y recuperar valores.

Editar la `AppPreferences.cs` de archivos y darle un aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Esta clase contiene rutinas auxiliares unos como `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, etc. que facilitan el trabajo con `NSUserDefaults` más fácil. Además, dado que `NSUserDefaults` no tiene un medio integrado para administrar `NSColors`, `NSColorToHexString` y `NSColorFromHexString` métodos se usan para convertir colores a cadenas hexadecimales basada en web (`#RRGGBBAA` donde `AA` es la transparencia alfa) que puede ser fácilmente almacenados y recuperados.

En el `AppDelegate.cs` , cree una instancia de la **AppPreferences** objeto que se utilizará toda la aplicación:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Preferencias de conexión a las vistas de preferencia

A continuación, conectar clase preferencia a los elementos de interfaz de usuario en la ventana de preferencias y crear vistas anteriormente. En el generador de interfaz, seleccione un controlador de vista de preferencia y cambie a la **identidad Inspector**, cree una clase personalizada para el controlador: 

[![](dialog-images/prefs12.png "El Inspector de identidad")](dialog-images/prefs12.png#lightbox)

Cambie a Visual Studio para Mac sincronizar los cambios y abrir la clase recién creada para su edición. Haga que la clase de aspecto similar al siguiente:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Tenga en cuenta que esta clase ha realizado estas dos cosas: en primer lugar, hay una aplicación auxiliar `App` propiedad para facilitar el acceso a la **AppDelegate** más fácil. Segundo, el `Preferences` propiedad expone la información global de **AppPreferences** clase para el enlace de datos con los controles de interfaz de usuario se coloca en esta vista.

A continuación, haga doble clic en el archivo del guión gráfico para abrirla de nuevo en el generador de interfaz (y ver los cambios realizados anteriormente). Arrastre los controles de interfaz de usuario necesarios para generar la interfaz de preferencias en la vista. Para cada control, cambie a la **enlace Inspector** y enlazar a las propiedades individuales de la **AppPreference** clase:

[![](dialog-images/prefs13.png "El Inspector de enlace")](dialog-images/prefs13.png#lightbox)

Repita los pasos anteriores para todos los paneles (controladores de la vista) y preferencias propiedades necesarias.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Aplicar preferencias cambia a todas las ventanas abiertas

Como se mencionó anteriormente, en un macOS típica se guardan automáticamente y se aplican a las ventanas de aplicación, cuando el usuario realice cambios en cualquiera de las preferencias del usuario de la aplicación, esos cambios al usuario tenga abiertos en la aplicación.

Una cuidadosa planeación y diseño de windows y las preferencias de la aplicación le permitirá este proceso que se produzca sin problemas y de forma transparente para el usuario final, con una cantidad mínima de trabajo de codificación.

En cualquier ventana que van a consumir las preferencias de aplicación, agregue la siguiente propiedad de la aplicación auxiliar a su controlador de vista de contenido para facilitar el acceso a nuestro **AppDelegate** más fácil:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

A continuación, agregue una clase para configurar el contenido o el comportamiento en función de las preferencias del usuario:

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Debe llamar al método de configuración cuando la ventana se abre por primera vez para asegurarse de que se ajusta a las preferencias del usuario:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

A continuación, edite la `AppDelegate.cs` de archivos y agregue el método siguiente a las preferencias se aplican cambios en todas las ventanas abiertas:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

A continuación, agregue un `PreferenceWindowDelegate` clase al proyecto y darle un aspecto similar al siguiente:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

Esto hará que los cambios de preferencias que se envía a todas las ventanas abiertas cuando la preferencia de la ventana se cierra.

Por último, modifique el controlador de la ventana de preferencias y agregue al delegado creado anteriormente:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Con todos estos cambios en su lugar, si el usuario edita las preferencias de la aplicación y cierra la ventana de preferencias, los cambios se aplicarán a todas las ventanas abiertas:

[![](dialog-images/prefs14.png "Una ventana de preferencias de ejemplo")](dialog-images/prefs14.png#lightbox)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>El cuadro de diálogo Abrir

El cuadro de diálogo Abrir proporciona a los usuarios una manera coherente para buscar y abrir un elemento en una aplicación. Para mostrar un cuadro de diálogo Abrir en una aplicación Xamarin.Mac, utilice el código siguiente:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

En el código anterior, se está abriendo una nueva ventana de documento para mostrar el contenido del archivo. Debe reemplazar este código con la funcionalidad se requiere la aplicación.

Las propiedades siguientes están disponibles cuando se trabaja con un `NSOpenPanel`:

- **CanChooseFiles** : si `true` el usuario puede seleccionar archivos.
- **CanChooseDirectories** : si `true` el usuario puede seleccionar directorios.
- **AllowsMultipleSelection** : si `true` el usuario puede seleccionar más de un archivo a la vez.
- **ResolveAliases** : si `true` seleccionando y alias, lo resuelve como ruta de acceso del archivo original.
- **AllowedFileTypes** -es una matriz de cadenas de tipos de archivo que el usuario puede seleccionar como cualquier extensión o _UTI_. El valor predeterminado es `null`, que permite que cualquier archivo que desea abrir.

El `RunModal ()` método muestra el cuadro de diálogo Abrir y permite al usuario seleccionar archivos o directorios (según lo especificado por las propiedades) y devuelve `1` si el usuario hace clic en el **abiertos** botón.

El cuadro de diálogo Abrir devuelve el usuario archivos o directorios seleccionados como una matriz de direcciones URL en el `URL` propiedad.

Si se ejecuta el programa y seleccione el **abrir...**  de elemento de la **archivo** se muestra el menú, lo siguiente: 

[![](dialog-images/dialog03.png "Un cuadro de diálogo Abrir")](dialog-images/dialog03.png#lightbox)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>La impresión y los cuadros de diálogo Configurar página

macOS proporciona estándar de impresión y página Configuración de cuadros de diálogo que puede mostrar la aplicación para que los usuarios pueden tener una impresión coherente en todas las aplicaciones que utilizan la experiencia.

El código siguiente muestra el cuadro de diálogo Imprimir estándar:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Si se establece la `ShowPrintAsSheet` propiedad `false`, ejecute la aplicación y mostrar el cuadro de diálogo de impresión, se mostrará lo siguiente:

[![](dialog-images/print01.png "Un cuadro de diálogo de impresión")](dialog-images/print01.png#lightbox)

Si establece la `ShowPrintAsSheet` propiedad `true`, ejecute la aplicación y mostrar el cuadro de diálogo de impresión, se mostrará lo siguiente:

[![](dialog-images/print02.png "Una hoja de impresión")](dialog-images/print02.png#lightbox)

El código siguiente muestra el cuadro de diálogo de diseño de página:

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Si se establece la `ShowPrintAsSheet` propiedad `false`, ejecute la aplicación y mostrar el cuadro de diálogo Diseño de impresión, se mostrará lo siguiente:

[![](dialog-images/print03.png "Un cuadro de diálogo de configuración de página")](dialog-images/print03.png#lightbox)

Si establece la `ShowPrintAsSheet` propiedad `true`, ejecute la aplicación y mostrar el cuadro de diálogo Diseño de impresión, se mostrará lo siguiente:

[![](dialog-images/print04.png "Una hoja de configuración de página")](dialog-images/print04.png#lightbox)

Para obtener más información sobre cómo trabajar con la impresión y cuadros de diálogo de configuración de página, vea de Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) y [Introducción a la impresión](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) documentación.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>La operación de guardar cuadro de diálogo

El cuadro de diálogo Guardar ofrece a los usuarios una manera coherente para guardar un elemento en una aplicación.

El código siguiente muestra el cuadro de diálogo Guardar estándar:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

El `AllowedFileTypes` propiedad es una matriz de tipos de archivo que el usuario puede seleccionar para guardar el archivo como cadenas. El tipo de archivo se puede especificar ya sea como una extensión o _UTI_. El valor predeterminado es `null`, lo que permite cualquier tipo de archivo que se usará.

Si se establece la `ShowSaveAsSheet` propiedad `false`, ejecute la aplicación y seleccione **Guardar como...**  desde el **archivo** menú, se mostrará lo siguiente:

[![](dialog-images/save01.png "Un cuadro de diálogo Guardar")](dialog-images/save01.png#lightbox)

El usuario puede expandir el cuadro de diálogo:

[![](dialog-images/save02.png "Cuadro de diálogo Guardar expandido")](dialog-images/save02.png#lightbox)

Si se establece la `ShowSaveAsSheet` propiedad `true`, ejecute la aplicación y seleccione **Guardar como...**  desde el **archivo** menú, se mostrará lo siguiente:

[![](dialog-images/save03.png "Guarda la hoja")](dialog-images/save03.png#lightbox)

El usuario puede expandir el cuadro de diálogo:

[![](dialog-images/save04.png "Guarde la hoja expandido")](dialog-images/save04.png#lightbox)

Para obtener más información sobre cómo trabajar con el cuadro de diálogo Guardar, vea de Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) documentación.

<a name="Summary" />

## <a name="summary"></a>Resumen

En este artículo ha tomado una visión detallada de trabajar con ventanas modales, hojas y lo cuadros de diálogo estándar del sistema en una aplicación Xamarin.Mac. Hemos visto los diferentes tipos y usos de ventanas modales, hojas y cuadros de diálogo, cómo crear y mantener ventanas modales y las hojas en Xcode del generador de interfaz y cómo trabajar con ventanas modales, hojas y cuadros de diálogo en el código C#.

## <a name="related-links"></a>Vínculos relacionados

- [MacWindows (ejemplo)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Hello, Mac](~/mac/get-started/hello-mac.md)
- [Menús](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Barras de herramientas](~/mac/user-interface/toolbar.md)
- [OS X Human Interface Guidelines](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/) (Directrices de interfaz humana de OS X)
- [Introducción a Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Introducción a las hojas](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Introducción a la impresión](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
