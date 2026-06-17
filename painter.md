```mermaid
classDiagram
    direction TB
    
    %% Базовые типы и инфраструктура
    class Window { <<external>> }
    class MenuCategory {
        <<enumeration>>
        File
        Settings
    }
    class Services {
        <<static>>
        +GetMainWindow() Window\(+GetImageController() IImageController\)
        +GetImageSettings() ImageSettings\(+GetPalette() Palette\)
    }

    %% Клиентская часть и контроллеры
    class MainWindow {
        -int MenuSize
        -InitializeComponent() void
        -CreateSettingsManager() SettingsManager\$
    }
    Window <|-- MainWindow : Наследует
    MainWindow ..> Services : Разрешает зависимости

    class IUiAction {
        <<interface>>
        +MenuCategory Category
        +string Name
        +CanExecute(object) bool
        +Execute(object) void
    }
    MainWindow --> IUiAction : Содержит список действий
    IUiAction --> MenuCategory : Фильтрует расположение

    %% Действия
    class ImageSettingsAction {
        -IImageController imageController
        -ImageSettings imageSettings
        +Execute(object) void
    }
    class SaveImageAction {
        -IImageController imageController
        +Execute(object) void
    }
    class PaletteSettingsAction {
        -Palette palette
        +Execute(object) void
    }
    
    IUiAction <|.. ImageSettingsAction
    IUiAction <|.. SaveImageAction
    IUiAction <|.. PaletteSettingsAction

    %% Доменная модель и внешние формы
    class ImageSettings {
        +int Width
        +int Height
    }
    class Palette { <<external>> }
    class SettingsForm {
        <<external>>
        +SettingsForm(object)
        +ShowDialog(Window) Task
    }

    class IImageController {
        <<interface>>
        +RecreateImage(ImageSettings) void
        +SaveImage(string) void
    }
    class AvaloniaImageController {
        +SetControl(ImageControl) void
        +RecreateImage(ImageSettings) void
    }
    IImageController <|.. AvaloniaImageController
    MainWindow --> AvaloniaImageController : Управляет выводом

    %% Отрегулированные связи нижнего уровня 
    ImageSettingsAction ---> IImageController : Обновляет холст
    ImageSettingsAction ---> ImageSettings : Изменяет размеры
    ImageSettingsAction ...> SettingsForm : Открывает модально
    
    SaveImageAction ---> IImageController : Вызывает сохранение
    
    PaletteSettingsAction ---> Palette : Передает для редактирования
    PaletteSettingsAction ...> SettingsForm : Открывает модально
```
