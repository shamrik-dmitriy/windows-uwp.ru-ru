---
Description: Узнайте, как повысить удобство использования и улучшить специальные возможности приложения UWP, предоставляя пользователям интуитивно понятный способ навигации и взаимодействия с видимым пользовательским интерфейсом приложения с помощью клавиатуры вместо указывающего устройства (например, сенсорного ввода или мыши).
title: Рекомендации по проектированию клавиш доступа
label: Access keys design guidelines
keywords: клавиатура, клавиша доступа, подсказка клавиши, специальные возможности, навигация, фокус, текст, ввод, взаимодействие с пользователем
template: detail.hbs
ms.date: 06/08/2018
ms.topic: article
pm-contact: miguelrb
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f73bb15d2746dbac4403ca6daffb603588cf7a58
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71340094"
---
# <a name="access-keys"></a>Ключи доступа

Клавиши доступа — это сочетания клавиш, которые могут повысить удобство использования и улучшить специальные возможности Windows-приложений, предоставляя пользователям интуитивно понятный способ навигации и взаимодействия с видимым пользовательским интерфейсом приложения с помощью клавиатуры вместо указывающего устройства (например, сенсорного ввода или мыши).

Подробные сведения о вызове стандартных действий в Windows-приложении с помощью сочетаний клавиш см. в разделе [Ускорители клавиатуры](keyboard-accelerators.md). 

> [!NOTE]
> Клавиатура незаменима для пользователей с определенными ограниченными способностями (см. [Специальные возможности клавиатуры](https://docs.microsoft.com/windows/uwp/accessibility/keyboard-accessibility)), а также является важным средством для тех, кто считает ее более эффективным способом для взаимодействия с приложением.

Универсальная платформа Windows (UWP) обеспечивает встроенную поддержку элементов управления платформы для доступа на основе сочетания клавиш и соответствующей обратной связи пользовательского интерфейса через визуальные подсказки клавиш.

## <a name="overview"></a>Обзор

Клавиша доступа — это сочетание клавиши Alt и одной из буквенно-цифровых клавиш, иногда называемых *назначенными*. Обычно их нажимают последовательно, а не одновременно.

Подсказки клавиш — это значки, которые отображаются рядом с элементами управления в помощь пользователю при нажатии клавиши Alt. Каждая подсказка клавиши содержит буквенно-цифровые ключи, которые активируют связанный элемент управления.

> [!NOTE]
> Сочетания клавиш поддерживаются автоматически для клавиши доступа с одним алфавитно-цифровым символом. Например, при одновременном нажатии клавиш Alt + F в Word открывается меню "Файл" без отображения подсказок клавиш.

При нажатии клавиши Alt инициализируется функция клавиши доступа и отображаются все доступные сочетания клавиш в подсказках клавиш. Последующие нажатия клавиш обрабатываются платформой клавиши доступа, отклоняющей недопустимые клавиши, пока не будет нажата допустимая клавиша либо ВВОД, ESC, TAB или клавиши со стрелками для отключения клавиш доступа. После этого нажатия клавиш будут снова обрабатываться приложением.

Приложения Microsoft Office предоставляют широкие возможности для клавиш доступа. На изображении ниже показана вкладка "Главная" в текстовом редакторе Word с активированными клавишами доступа (обратите внимание как на числа, так и на множественные нажатия клавиш).

![Значки подсказок клавиш доступа в Microsoft Word](images/accesskeys/keytip-badges-word.png)

_KeyTip эмблемы для клавиш доступа в Microsoft Word_

Чтобы добавить к элементу управления клавишу доступа, используйте **свойство AccessKey**. С помощью значения этого свойства определяются последовательность клавиш доступа, сочетание клавиш (если одна буквенно-цифровая) и подсказка клавиши.

``` xaml
<Button Content="Accept" AccessKey="A" Click="AcceptButtonClick" />
```

## <a name="when-to-use-access-keys"></a>Когда следует использовать клавиши доступа

Рекомендуется указывать клавиши доступа всегда, когда это уместно в пользовательском интерфейсе, и предусматривать поддержку клавиш доступа во всех пользовательских элементах управления.

1.  **За счет клавиш доступа улучшаются специальные возможности приложений** для пользователей с нарушениями двигательных функций, в том числе таких пользователей, которые могут нажимать только одну клавишу за один раз или испытывают трудности при работе с мышью.

    Правильно разработанный пользовательский интерфейс клавиатуры является важным аспектом доступности программы. Он позволяет людям с ослабленным зрением или с определенными нарушениями двигательных способностей осуществлять переходы в приложении и взаимодействовать с его компонентами. Такие пользователи могут не иметь возможности использовать мышь. Вместо этого они могут рассчитывать на различные специальные возможности, такие как средства расширения функций клавиатуры, экранные клавиатуры, экранные лупы, средства чтения с экрана и средства голосового управления. Для этих пользователей крайне важно иметь доступ ко всем командам.

2.  **Клавиши доступа делают приложение более удобным** для опытных пользователей, предпочитающих взаимодействовать с помощью клавиатуры.

    Опытные пользователи часто применяют клавиатуру, так как с ее помощью можно быстрее вводить команды, при этом не требуется убирать руки с клавиатуры. Для таких пользователей эффективность и согласованность критически важна. Полнота важна только для наиболее часто используемых команд.

## <a name="set-access-key-scope"></a>Определение области клавиши доступа

Если на экране имеется много элементов с поддержкой клавиш доступа, рекомендуется ограничить применение клавиш доступа для облегчения **восприятия информации**. Это уменьшает количество клавиш доступа на экране, что облегчает их поиск, повышает производительность и эффективность.

Например, Microsoft Word предоставляет две ключевые области доступа: основную для вкладок ленты и вторичную для команд на вкладке выбранного.

На следующих изображениях показаны две области клавиш доступа в Word. В первой отображаются основные клавиши доступа, с помощью которых пользователь может выбрать вкладку и команды верхнего уровня, а во второй отображаются дополнительные клавиши доступа для вкладки "Главная".

![первичные ключи доступа в Microsoft Word](images/accesskeys/primary-access-keys-word.png)
_первичные ключи доступа в Microsoft Word_

![вторичные ключи доступа в Microsoft Word](images/accesskeys/secondary-access-keys-word.png)
_вторичные ключи доступа в Microsoft Word_

Клавиши доступа можно дублировать для элементов в различных областях. В предыдущем примере "2" — это клавиша доступа для команды "Отмена" в основной области, а также "Курсив" в дополнительной области.

Здесь мы покажем, как определить область клавиши доступа.

``` xaml
<CommandBar x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</CommandBar>
```

![Основные клавиши доступа для CommandBar](images/accesskeys/primary-access-keys-commandbar.png)

_Основная область CommandBar и Поддерживаемые ключи доступа_

![Дополнительные клавиши доступа для CommandBar](images/accesskeys/secondary-access-keys-commandbar.png)

_Дополнительная область CommandBar и Поддерживаемые ключи доступа_

### <a name="windows-10-creators-update-and-older"></a>Windows 10 Creators Update и более ранние версии

До выхода Windows 10 Fall Creators Update некоторые элементы управления, например CommandBar, не поддерживали встроенные области клавиш доступа.

В примере ниже рассматривается, как поддерживать SecondaryCommands в CommandBar с клавишами доступа, доступными после вызова родительской команды (аналогично ленте в Microsoft Word).

```xaml
<local:CommandBarHack x:Name="MainCommandBar" AccessKey="M" >
    <AppBarButton AccessKey="G" Icon="Globe" Label="Go"/>
    <AppBarButton AccessKey="S" Icon="Stop" Label="Stop"/>
    <AppBarSeparator/>
    <AppBarButton AccessKey="R" Icon="Refresh" Label="Refresh" IsAccessKeyScope="True">
        <AppBarButton.Flyout>
            <MenuFlyout>
                <MenuFlyoutItem AccessKey="A" Icon="Globe" Text="Refresh A" />
                <MenuFlyoutItem AccessKey="B" Icon="Globe" Text="Refresh B" />
                <MenuFlyoutItem AccessKey="C" Icon="Globe" Text="Refresh C" />
                <MenuFlyoutItem AccessKey="D" Icon="Globe" Text="Refresh D" />
            </MenuFlyout>
        </AppBarButton.Flyout>
    </AppBarButton>
    <AppBarButton AccessKey="B" Icon="Back" Label="Back"/>
    <AppBarButton AccessKey="F" Icon="Forward" Label="Forward"/>
    <AppBarSeparator/>
    <AppBarToggleButton AccessKey="V" Icon="Favorite" Label="Favorite"/>
    <CommandBar.SecondaryCommands>
        <AppBarToggleButton Icon="Like" AccessKey="L" Label="Like"/>
        <AppBarButton Icon="Setting" AccessKey="T" Label="Settings" />
    </CommandBar.SecondaryCommands>
</local:CommandBarHack>
```

```csharp
public class CommandBarHack : CommandBar
{
    CommandBarOverflowPresenter secondaryItemsControl;
    Popup overflowPopup;

    public CommandBarHack()
    {
        this.ExitDisplayModeOnAccessKeyInvoked = false;
        AccessKeyInvoked += OnAccessKeyInvoked;
    }

    protected override void OnApplyTemplate()
    {
        base.OnApplyTemplate();

        Button moreButton = GetTemplateChild("MoreButton") as Button;
        moreButton.SetValue(Control.IsTemplateKeyTipTargetProperty, true);
        moreButton.IsAccessKeyScope = true;

        // SecondaryItemsControl changes
        secondaryItemsControl = GetTemplateChild("SecondaryItemsControl") as CommandBarOverflowPresenter;
        secondaryItemsControl.AccessKeyScopeOwner = moreButton;

        overflowPopup = GetTemplateChild("OverflowPopup") as Popup;
    }

    private void OnAccessKeyInvoked(UIElement sender, AccessKeyInvokedEventArgs args)
    {
        if (overflowPopup != null)
        {
            overflowPopup.Opened += SecondaryMenuOpened;
        }
    }

    private void SecondaryMenuOpened(object sender, object e)
    {
        //This is not neccesay given we are automatically pushing the scope.
        var item = secondaryItemsControl.Items.First();
        if (item != null && item is Control)
        {
            (item as Control).Focus(FocusState.Keyboard);
        }
        overflowPopup.Opened -= SecondaryMenuOpened;
    }
}
```

## <a name="avoid-access-key-collisions"></a>Предотвращение конфликтов клавиш доступа

Конфликты клавиш доступа возникают, когда два или более элементов в одной и той же области имеют дублирующиеся клавиши доступа или начинаются с одинаковых алфавитно-цифровых символов.

Система решает проблему с дублирующимися клавишами доступа, обрабатывая клавишу доступа первого элемента, добавленного в визуальное дерево, и игнорируя все остальные.

Если несколько клавиш доступа начинаются с одного и того же символа (например, "A", "A1" и "AB"), система обрабатывает один символ клавиши доступа и игнорирует все остальные.

Во избежание конфликтов используйте уникальные клавиши доступа или разделяйте команды по областям.

## <a name="choose-access-keys"></a>Выбор клавиш доступа

При выборе клавиш доступа придерживайтесь следующих рекомендаций:

-   Используйте один символ, чтобы уменьшить количество нажатий клавиш и поддерживать сочетания клавиш по умолчанию (Alt + AccessKey).
-   Не используйте более двух символов.
-   Избегайте конфликтов клавиш доступа.
-   Избегайте символов, которые сложно отличить от других (например, буква "З" и цифра "3", буква "O" и цифра "0").
-   Используйте известные варианты из других популярных приложений, таких как Word ("Ф" для "Файл", "Я" для "Главная" и т. д.).
-   Используйте первый символ имени команды или символ, тесно связанный с командой, который поможет вспомнить ее название.
    -   Если первая буква уже назначена, используйте букву, максимально близкую к первой букве имени команды ("С" для "Вставка").
    -   Используйте четкий согласный, содержащийся в имени команды ("К" для "Ссылки").
    -   Используйте гласный, содержащийся в имени команды.

## <a name="localize-access-keys"></a>Локализация клавиш доступа

Если приложение будет локализовано на нескольких языков, следует **рассмотреть локализацию клавиш доступа**. Например, "H" для "Home" в en-US и "Я" для "Главная" в ru-RU.

Используйте расширение x:Uid в разметке, чтобы применить локализованные ресурсы, как показано здесь:

``` xaml
<Button Content="Home" AccessKey="H" x:Uid="HomeButton" />
```
Ресурсы для каждого языка добавляются в соответствующие папки строк в проекте:

![Папки строк английских и испанских ресурсов](images/accesskeys/resource-string-folders.png)

_Папки с английским и испанским строками ресурсов_

Локализованные клавиши доступа указаны в файле resources.resw проектов:

![Укажите свойство AccessKey, указанное в файле resources.resw](images/accesskeys/resource-resw-file.png)

_Укажите свойство AccessKey, указанное в файле resources. resw_

Дополнительные сведения см. на странице [Перевод ресурсов интерфейса](https://docs.microsoft.com/previous-versions/windows/apps/hh965329(v=win.10))

## <a name="key-tip-positioning"></a>Положение подсказок клавиш

Подсказки клавиш отображаются в виде плавающих значков, связанных с соответствующими элементами пользовательского интерфейса. При этом учитывается наличие других элементов пользовательского интерфейса, других подсказок клавиш и край экрана.

Как правило, расположение подсказки клавиши по умолчанию достаточное и обеспечивает встроенную поддержку для адаптивного пользовательского интерфейса.

![Пример размещения автоматической подсказки клавиши](images/accesskeys/auto-keytip-position.png)

_Пример автоматического размещения ключевых подсказок_

Но если нужен дополнительный контроль над размещением подсказки клавиши, рекомендуется следующее:

1.  **Принцип очевидной связи**: пользователь может легко связать элемент управления с подсказкой клавиши.

    а)  Подсказка KeyTip должна находиться **рядом** с элементом, для которого есть клавиша доступа (владельцем).  
    б.  Подсказка KeyTip не должна **закрывать включенные элементы**, имеющие клавиши доступа.   
    в.  Если подсказку KeyTip невозможно разместить рядом с ее владельцем, она должна перекрывать владельца. 

2.  **Обнаружение**: пользователь может быстро найти элемент управления с подсказкой клавиши.

    а)  Подсказка KeyTip никогда не должна **перекрывать** другие подсказки клавиш.  

3.  **Простой поиск:** пользователь может легко просмотреть подсказки клавиш.

    а)  Подсказки клавиш должны быть **совмещены** друг с другом и элементом пользовательского интерфейса.
    б.  Подсказки клавиш должны быть максимально **сгруппированы**. 

### <a name="relative-position"></a>Относительное положение

Используйте свойство **KeyTipPlacementMode** для настройки местоположения подсказки клавиши каждого элемента или группы.

Режимы размещения: вверху, внизу, справа, слева, скрыто, по центру и автоматически.

![Режимы размещения подсказки клавиши](images/accesskeys/keytip-postion-modes.png)

_Режимы размещения ключевых советов_

Строка центра элемента управления используется для расчета выравнивания KeyTip по горизонтали и вертикали.

В следующем примере показано, как задать расположение подсказки клавиши для группы элементов с помощью свойства KeyTipPlacementMode контейнера StackPanel.

``` xaml
<StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}" KeyTipPlacementMode="Top">
  <Button Content="File" AccessKey="F" />
  <Button Content="Home" AccessKey="H" />
  <Button Content="Insert" AccessKey="N" />
</StackPanel>
```

### <a name="offsets"></a>Смещения

Используйте свойства KeyTipHorizontalOffset и KeyTipVerticalOffset элемента для более точного размещения подсказки клавиши.

> [!NOTE]
> Невозможно установить смещения, если для KeyTipPlacementMode установлено значение Auto.

Свойство KeyTipHorizontalOffset указывает, насколько далеко будет перемещена подсказка клавиши влево или вправо. В примере показано, как задать смещения подсказки клавиши для кнопки.

![Режимы размещения подсказки клавиши](images/accesskeys/keytip-offsets.png)

_Задание вертикальных и горизонтальных смещений для ключевых подсказок_

``` xaml
<Button
  Content="File"
  AccessKey="F"
  KeyTipPlacementMode="Bottom"
  KeyTipHorizontalOffset="20"
  KeyTipVerticalOffset="-8" />
```

### <a name="screen-edge-alignment-screen-edge-alignment-listparagraph"></a>Выравнивание по краю экрана {#screen-edge-alignment .ListParagraph}

Расположение подсказки клавиши автоматически корректируется на основе края экрана, чтобы подсказка клавиши была полностью видна. В этом случае расстояние между элементом управления и точка выравнивания подсказки ключа могут отличаться от значений, указанных для смещений по горизонтали и вертикали.

![Режимы размещения подсказки клавиши](images/accesskeys/keytips-screen-edge.png)

_Граница экрана приводит к тому, что подсказка автоматически перемещает себя_

## <a name="key-tip-style"></a>Стиль подсказок клавиш

Рекомендуем использовать встроенную в подсказке клавиш поддержку тем платформы, включая высокую контрастность.

Если необходимо указать собственные стили подсказки клавиши, используйте ресурсы приложения, такие как KeyTipFontSize (размер шрифта), KeyTipFontFamily (семейство шрифтов), KeyTipBackground (фон), KeyTipForeground (передний план), KeyTipPadding (заполнения), KeyTipBorderBrush (цвет границы) и KeyTipBorderThemeThickness (толщина границы).

![Режимы размещения подсказки клавиши](images/accesskeys/keytip-customization.png)

_Параметры настройки подсказок_

В этом примере показано, как изменять эти ресурсы приложения:

 ```xaml  
<Application.Resources>
  <SolidColorBrush Color="DarkGray" x:Key="MyBackgroundColor" />
  <SolidColorBrush Color="White" x:Key="MyForegroundColor" />
  <SolidColorBrush Color="Black" x:Key="MyBorderColor" />
  <StaticResource x:Key="KeyTipBackground" ResourceKey="MyBackgroundColor" />
  <StaticResource x:Key="KeyTipForeground" ResourceKey="MyForegroundColor" />
  <StaticResource x:Key="KeyTipBorderBrush" ResourceKey="MyBorderColor"/>
  <FontFamily x:Key="KeyTipFontFamily">Consolas</FontFamily>
  <x:Double x:Key="KeyTipContentThemeFontSize">18</x:Double>
  <Thickness x:Key="KeyTipBorderThemeThickness">2</Thickness>
  <Thickness x:Key="KeyTipThemePadding">4,4,4,4</Thickness>
</Application.Resources>
```

## <a name="access-keys-and-narrator"></a>Клавиши доступа и экранный диктор

Платформа XAML предоставляет свойства автоматизации, с помощью которых клиенты автоматизации пользовательского интерфейса могут получать информацию об элементах пользовательского интерфейса.

Если задать свойство AccessKey на элементе управления UIElement или TextElement, можно использовать свойство [AutomationProperties.AccessKey](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.accesskey) для получения этого значения. Помощники, такие как экранный диктор, считывают значение этого свойства каждый раз, когда элемент получает фокус.

## <a name="related-articles"></a>Связанные статьи

* [Взаимодействие с клавиатурой](keyboard-interactions.md)
* [Сочетания клавиш](keyboard-accelerators.md)

**Примеры**
* [Коллекция элементов управления XAML (Ксамлуибасикс)](https://github.com/Microsoft/Windows-universal-samples/tree/c2aeaa588d9b134466bbd2cc387c8ff4018f151e/Samples/XamlUIBasics)


