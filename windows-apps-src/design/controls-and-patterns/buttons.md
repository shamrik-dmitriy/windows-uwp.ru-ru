---
author: serenaz
Description: A button gives the user a way to trigger an immediate action.
title: Кнопки
label: Buttons
template: detail.hbs
ms.author: sezhen
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f04d1a3c-7dcd-4bc8-9586-3396923b312e
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: f0bf7731a8480fb4f6d81368227ad6259780381b
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/22/2018
ms.locfileid: "2790293"
---
# <a name="buttons"></a>Кнопки

> [!IMPORTANT]
> В этой статье описана еще не выпущенная функция, которая может быть существенно изменена до коммерческого выпуска. Майкрософт не дает никаких гарантий, явных или подразумеваемых, в отношении предоставленной здесь информации. Функции предварительной версии или их версии требуется [последние построения предварительного просмотра внутренних 10 Windows и пакет SDK для](https://insider.windows.com/for-developers/) [Библиотеки пользовательского интерфейса Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

Кнопка предоставляет пользователю возможность вызвать немедленное действие. Некоторые кнопки специализированные для конкретных задач, таких как навигации, выполняемые действия или презентации меню.

![Примеры кнопок](images/controls/button.png)

Платформа XAML предоставляет стандартной кнопки управления, а также несколько специализированных кнопок.

Элемент управления | Описание
------- | -----------
[Кнопка](/uwp/api/windows.ui.xaml.controls.button) | Инициирует немедленных действий. Можно использовать с событие Click и привязка команды.
[RepeatButton](/uwp/api/windows.ui.xaml.controls.primitives.repeatbutton) | Кнопка, которая вызывает событие Click постоянно нажатой кнопке.
[HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) | A кнопку, которая со стилем как гиперссылка, используемые для навигации. Подробнее см. на странице [Гиперссылки](hyperlinks.md).
[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) | (Предварительная версия) Кнопка со шеврон для открытия вложенные всплывающее окно.
[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) | (Предварительная версия) Кнопка с двух сторон. Одной стороны выполняется определенное действие, и другая сторона откроется меню.
[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) | (Предварительная версия) Выключатель с двух сторон. Одной из сторон Включение и отключение, а другая сторона открывает меню.

| **Получение библиотека интерфейса пользователя Windows** |
| - |
| DropDownButton, кнопкой-разделителем и ToggleSplitButton являются частью библиотеки пользовательского интерфейса Windows NuGet пакет, содержащий новые элементы управления и возможности пользовательского интерфейса для UWP приложений. Дополнительные сведения, включая инструкции по установке в разделе [Общие сведения о библиотеке пользовательского интерфейса Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

| **Интерфейсы API платформы** | **Windows API-интерфейсы библиотеки пользовательского интерфейса** |
| - | - |
| [Событие click](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click), [свойство Command](/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.command) | [Класс DropDownButton](/uwp/api/microsoft.ui.xaml.controls.dropdownbutton), [класс SplitButton](/uwp/api/microsoft.ui.xaml.controls.splitbutton) [класс ToggleSplitButton](/uwp/api/microsoft.ui.xaml.controls.togglesplitbutton) |

## <a name="is-this-the-right-control"></a>Выбор подходящего элемента управления

Используйте **кнопки** позволяют инициировать немедленных действий, таких как отправка формы.

Не используйте кнопки при выполнении действия для перехода на другую страницу; Вместо этого используйте [HyperlinkButton](/uwp/api/windows.ui.xaml.controls.hyperlinkbutton) . Подробнее см. на странице [Гиперссылки](hyperlinks.md).
> Исключение: для навигации по мастеру используйте кнопки "Вперед" и "Назад". Для других типов обратной навигации или навигации на верхнем уровне, можно использовать [Кнопка возврата](../basics/navigation-history-and-backwards-navigation.md).

Используйте **RepeatButton** , когда пользователь может понадобиться несколько раз вызвать действие. Например используйте RepeatButton для увеличения или уменьшения значения в счетчик.

Используйте **DropDownButton** кнопка имеет всплывающее окно, содержащее дополнительные параметры. Значок по умолчанию предоставляет visual сигнал о том, что кнопки включает в себя всплывающее окно.

Используйте **SplitButton** , если пользователю необходимо иметь возможность запуска Немедленные действия или выберите один из дополнительных параметров независимо друг от друга.

## <a name="examples"></a>Примеры.

<table>
<th align="left">Галерея элементов управления XAML<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Если у вас установлено приложение <strong style="font-weight: semi-bold">галереи элементов управления XAML</strong>, щелкните здесь, чтобы <a href="xamlcontrolsgallery:/item/Button">открыть приложение и увидеть Button в действии</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Скачать приложение галереи элементов управления XAML (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Получить исходный код (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

В этом примере используются две кнопки "Разрешить" и "Блокировать" в диалоговом окне, запрашивающем расположение.

![Пример кнопок, используемых в диалоговом окне](images/dialogs/dialog_RS2_two_button.png)

## <a name="create-a-button"></a>Создание кнопки

В этом примере рассматривается реакция кнопки на щелчок.

Создайте кнопку в XAML.

```xaml
<Button Content="Subscribe" Click="SubscribeButton_Click"/>
```

Либо создайте кнопку в коде.

```csharp
Button subscribeButton = new Button();
subscribeButton.Content = "Subscribe";
subscribeButton.Click += SubscribeButton_Click;

// Add the button to a parent container in the visual tree.
stackPanel1.Children.Add(subscribeButton);
```

Обработайте событие "Click".

```csharp
private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
{
    // Call app specific code to subscribe to the service. For example:
    ContentDialog subscribeDialog = new ContentDialog
    {
        Title = "Subscribe to App Service?",
        Content = "Listen, watch, and play in high definition for only $9.99/month. Free to try, cancel anytime.",
        CloseButtonText = "Not Now",
        PrimaryButtonText = "Subscribe",
        SecondaryButtonText = "Try it"
    };

    ContentDialogResult result = await subscribeDialog.ShowAsync();
}
```

### <a name="button-interaction"></a>Взаимодействие с кнопкой

Если коснуться кнопки пальцем или стилусом либо навести на нее указатель и нажать левую кнопку мыши, кнопка вызывает событие [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx). Если для кнопки предусмотрен фокус клавиатуры, при нажатии клавиши ВВОД или ПРОБЕЛ также происходит вызов события "Click".

Как правило, нельзя обрабатывать низкоуровневые события [PointerPressed](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.pointerpressed.aspx) с помощью элемента "Button", поскольку для него предусмотрено поведение "Click". Дополнительные сведения см. в разделе [Общие сведения о событиях и перенаправленных событиях](https://msdn.microsoft.com/library/windows/apps/mt185584.aspx).

Можно выбирать порядок вызова кнопкой события "Click" путем изменения свойства [ClickMode](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.controls.clickmode). Значением ClickMode по умолчанию является **Release**, однако режиму кнопки ClickMode можно задать значения **Hover** или **Press**. Если для параметра ClickMode выбрано значение **Hover**, событие "Click" невозможно вызвать нажатием клавиши или касанием.


### <a name="button-content"></a>Содержимое кнопки

Кнопка представляет собой [ContentControl](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.aspx). Ее свойство содержимого XAML — [Content](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.controls.contentcontrol.content.aspx), благодаря чему возможно использование подобного синтаксиса для XAML: `<Button>A button's content</Button>`. В качестве содержимого кнопки можно задать любой объект. Если содержимым является класс [UIElement](https://msdn.microsoft.com/library/windows/apps/xaml/windows.ui.xaml.uielement.aspx), он обрабатывается для просмотра в кнопке. Если содержимым является другой тип объекта, в кнопке отображается его строковое представление.

Содержимое кнопки обычно представляет собой текст. Ниже приведены рекомендации по проектированию для кнопок с текстовым содержимым:
-   Используйте краткий, конкретный, не требующий разъяснений текст, который четко описывает действие, выполняемое кнопкой. Обычно текст надписи на кнопке состоит из одного слова — как правило, глагола.
-   Используйте шрифт, заданный по умолчанию, если в соответствии со стилем торговой марки не требуется использовать другой.
-   При размещении короткого текста избегайте использования узких кнопок. Минимальная ширина кнопки должна составлять 120 пикселей.
- При размещении длинного текста избегайте использования широких кнопок. Длина текста должна составлять не более 26 символов.
-   Если текст надписи на кнопке динамический (то есть он [локализуемый](../globalizing/globalizing-portal.md)), продумайте, как кнопка сможет изменять свои размеры и как это повлияет на элементы управления, окружающие ее.

<table>
<tr>
<td> <b>Необходимо исправить:</b><br> Кнопки со переполняющим текстом. </td>
<td> <img src="images/button-wraptext.png"/> </td>
</tr>
<tr>
<td> <b>Вариант 1.</b><br> Увеличьте ширину кнопки, разместите кнопки в виде стопки и перенесите текст, если его длина превышает 26 символов. </td>
<td> <img src="images/button-wraptext1.png"> </td>
</tr>
<tr>
<td> <b>Вариант 2.</b><br> Увеличьте высоту кнопки и перенесите текст. </td>
<td> <img src="images/button-wraptext2.png"> </td>
</tr>
</table>

Можно также настроить визуальные элементы, составляющие внешний вид кнопки. Например, можно заменить текст значком или использовать значок и текст.

Здесь класс **StackPanel**, содержащий изображение и текст, задан в качестве содержимого кнопки.

```xaml
<Button Click="Button_Click"
        Background="LightGray"
        Height="100" Width="80">
    <StackPanel>
        <Image Source="Assets/Photo.png" Height="62"/>
        <TextBlock Text="Photos" Foreground="Black"
                   HorizontalAlignment="Center"/>
    </StackPanel>
</Button>
```

Кнопка выглядит следующим образом.

![Кнопка с содержимым в виде изображения и текста](images/button-orange.png)

## <a name="create-a-repeat-button"></a>Создание кнопки повтора

[RepeatButton](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.aspx) — это кнопка, при нажатии которой события [Click](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.buttonbase.click.aspx) повторяются до тех пор, пока она не будет отпущена. Задайте свойство [Delay](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.delay.aspx), чтобы указать время задержки после нажатия кнопки, по прошествии которого начнется повторение действия щелчка. Задайте свойство [Interval](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.primitives.repeatbutton.interval.aspx), чтобы указать время между повторениями действия щелчка. Время для обоих свойств указывается в миллисекундах.

В следующем примере показаны два элемента управления RepeatButton, чьи соответствующие события "Click" используются для увеличения и уменьшения значения, приведенного в блоке текста.

```xaml
<StackPanel>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Increase_Click">Increase</RepeatButton>
    <RepeatButton Width="100" Delay="500" Interval="100" Click="Decrease_Click">Decrease</RepeatButton>
    <TextBlock x:Name="clickTextBlock" Text="Number of Clicks:" />
</StackPanel>
```

```csharp
private static int _clicks = 0;
private void Increase_Click(object sender, RoutedEventArgs e)
{
    _clicks += 1;
    clickTextBlock.Text = "Number of Clicks: " + _clicks;
}

private void Decrease_Click(object sender, RoutedEventArgs e)
{
    if(_clicks > 0)
    {
        _clicks -= 1;
        clickTextBlock.Text = "Number of Clicks: " + _clicks;
    }
}
```

## <a name="create-a-drop-down-button"></a>Создать раскрывающийся список кнопки

> **Предварительная версия**: DropDownButton требуются [последние построения предварительного просмотра внутренних 10 Windows и пакет SDK](https://insider.windows.com/for-developers/) или [Библиотека пользовательского интерфейса Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

[DropDownButton](/uwp/api/windows.ui.xaml.controls.dropdownbutton) — это кнопка, показывающий шеврон визуальный индикатор, что она имеет вложенные всплывающее окно, содержащий дополнительные параметры. Она имеет отличается от стандартной кнопки с всплывающее окно; только внешний вид отличается.

Раскрывающийся список кнопки наследует событие Click, но обычно она не используется. Вместо этого используйте свойство всплывающего для подключения всплывающее окно и вызова действий с помощью меню параметров в всплывающее окно. Всплывающее окно открывается автоматически при нажатии кнопки.

> [!TIP]
> Дополнительные сведения о всплывающие окна в разделе [меню и контекстные меню](menus.md).

### <a name="example---drop-down-button"></a>Пример — раскрывающийся список кнопки

В этом примере показано, как создать раскрывающийся список кнопки с всплывающее окно, содержащее команды для выравнивания абзаца в RichEditBox. (Дополнительные сведения и код, см. [поля форматированного редактирования](rich-edit-box.md)).

![Раскрывающийся список кнопки с помощью команды выравнивания](images/drop-down-button-align.png)

```xaml
<DropDownButton ToolTipService.ToolTip="Alignment">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8E4;"/>
    <DropDownButton.Flyout>
        <MenuFlyout Placement="BottomEdgeAlignedLeft">
            <MenuFlyoutItem Text="Left" Icon="AlignLeft" Tag="left"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Center" Icon="AlignCenter" Tag="center"
                            Click="AlignmentMenuFlyoutItem_Click"/>
            <MenuFlyoutItem Text="Right" Icon="AlignRight" Tag="right"
                            Click="AlignmentMenuFlyoutItem_Click"/>
        </MenuFlyout>
    </DropDownButton.Flyout>
</DropDownButton>
```

```csharp
private void AlignmentMenuFlyoutItem_Click(object sender, RoutedEventArgs e)
{
    var option = ((MenuFlyoutItem)sender).Tag.ToString();

    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the alignment to the selected paragraphs.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (option == "left")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Left;
        }
        else if (option == "center")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Center;
        }
        else if (option == "right")
        {
            paragraphFormatting.Alignment = Windows.UI.Text.ParagraphAlignment.Right;
        }
    }
}
```

## <a name="create-a-split-button"></a>Создание Разворачивающаяся кнопка

> **Предварительная версия**: SplitButton требуются [последние построения предварительного просмотра внутренних 10 Windows и пакет SDK](https://insider.windows.com/for-developers/) или [Библиотека пользовательского интерфейса Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

[SplitButton](/uwp/api/windows.ui.xaml.controls.splitbutton) состоит из двух частей, которые могут быть вызваны отдельно. Одна часть ведет себя как стандартной кнопки и вызывает интерпретации действие. Другие части вызывает всплывающее окно, которое содержит дополнительные параметры, которые пользователь может выбрать из.

> [!NOTE]
> При вызове нажатием кнопки разделения ведет себя как раскрывающийся список кнопки. Оба половины кнопку вызова всплывающее окно. Для других способов ввода пользователь может вызвать любой половины кнопки отдельно.

Типичное поведение для Разворачивающаяся кнопка — это:

- При нажатии кнопки часть обработайте событие Click, чтобы вызвать параметра, выбранного в раскрывающемся списке.
- При открытом раскрывающемся списке дескриптор вызов элементов в раскрывающейся обоих изменений, которые параметр выбран, а затем вызвать. Очень важно для вызова всплывающего элемента, так как событие не возникает при использовании сенсорного ввода нажатия кнопки.

> [!TIP]
> Существует множество способов поместить элементы в раскрывающемся списке и обрабатывать их вызова. Если вы используете списка автофильтра или GridView, можно обрабатывать события SelectionChanged. После этого, установите [SingleSelectionFollowsFocus](/uwp/api/windows.ui.xaml.controls.listviewbase.singleselectionfollowsfocus) значение **false**. Это позволяет пользователям выберите параметры, с помощью клавиатуры без вызова элемента на каждого изменения.

### <a name="example---split-button"></a>Пример — Разворачивающаяся кнопка

В этом примере показано, как создать Разворачивающаяся кнопка, которая используется для изменения цвета переднего плана выделенного текста в RichEditBox. (Дополнительные сведения и код, см. [поля форматированного редактирования](rich-edit-box.md)).

![Разворачивающаяся кнопка для выбора цвета переднего плана](images/split-button-rtb.png)

```xaml
<SplitButton ToolTipService.ToolTip="Foreground color"
             Click="BrushButtonClick">
    <Border x:Name="SelectedColorBorder" Width="20" Height="20"/>
    <SplitButton.Flyout>
        <Flyout x:Name="BrushFlyout" Placement="BottomEdgeAlignedLeft">
            <!-- Set SingleSelectionFollowsFocus="False"
                 so that keyboard navigation works correctly. -->
            <GridView ItemsSource="{x:Bind ColorOptions}" 
                      SelectionChanged="BrushSelectionChanged"
                      SingleSelectionFollowsFocus="False"
                      SelectedIndex="0" Padding="0">
                <GridView.ItemTemplate>
                    <DataTemplate>
                        <Rectangle Fill="{Binding}" Width="20" Height="20"/>
                    </DataTemplate>
                </GridView.ItemTemplate>
                <GridView.ItemContainerStyle>
                    <Style TargetType="GridViewItem">
                        <Setter Property="Margin" Value="2"/>
                        <Setter Property="MinWidth" Value="0"/>
                        <Setter Property="MinHeight" Value="0"/>
                    </Style>
                </GridView.ItemContainerStyle>
            </GridView>
        </Flyout>
    </SplitButton.Flyout>
</SplitButton>
```

```csharp
public sealed partial class MainPage : Page
{
    // Color options that are bound to the grid in the split button flyout.
    private List<SolidColorBrush> ColorOptions = new List<SolidColorBrush>();
    private SolidColorBrush CurrentColorBrush = null;

    public MainPage()
    {
        this.InitializeComponent();

        // Add color brushes to the collection.
        ColorOptions.Add(new SolidColorBrush(Colors.Black));
        ColorOptions.Add(new SolidColorBrush(Colors.Red));
        ColorOptions.Add(new SolidColorBrush(Colors.Orange));
        ColorOptions.Add(new SolidColorBrush(Colors.Yellow));
        ColorOptions.Add(new SolidColorBrush(Colors.Green));
        ColorOptions.Add(new SolidColorBrush(Colors.Blue));
        ColorOptions.Add(new SolidColorBrush(Colors.Indigo));
        ColorOptions.Add(new SolidColorBrush(Colors.Violet));
        ColorOptions.Add(new SolidColorBrush(Colors.White));
    }

    private void BrushButtonClick(object sender, object e)
    {
        // When the button part of the split button is clicked,
        // apply the selected color.
        ChangeColor();
    }

    private void BrushSelectionChanged(object sender, SelectionChangedEventArgs e)
    {
        // When the flyout part of the split button is opened and the user selects
        // an option, set their choice as the current color, apply it, then close the flyout.
        CurrentColorBrush = (SolidColorBrush)e.AddedItems[0];
        SelectedColorBorder.Background = CurrentColorBrush;
        ChangeColor();
        BrushFlyout.Hide();
    }

    private void ChangeColor()
    {
        // Apply the color to the selected text in a RichEditBox.
        Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
        if (selectedText != null)
        {
            Windows.UI.Text.ITextCharacterFormat charFormatting = selectedText.CharacterFormat;
            charFormatting.ForegroundColor = CurrentColorBrush.Color;
            selectedText.CharacterFormat = charFormatting;
        }
    }
}
```

## <a name="create-a-toggle-split-button"></a>Создание разделения переключателя (en)

> **Предварительная версия**: ToggleSplitButton требуются [последние построения предварительного просмотра внутренних 10 Windows и пакет SDK](https://insider.windows.com/for-developers/) или [Библиотека пользовательского интерфейса Windows](https://docs.microsoft.com/uwp/toolkits/winui/).

[ToggleSplitButton](/uwp/api/windows.ui.xaml.controls.togglesplitbutton) состоит из двух частей, которые могут быть вызваны отдельно. Одна часть действует как переключателя (en), которое может быть включено или отключено. Другие части вызывает всплывающее окно, которое содержит дополнительные параметры, которые пользователь может выбрать из.

Разделение переключателя обычно используется для включения или отключения функциональная возможность, если компонент имеет несколько параметров, которые пользователь может выбрать из. Например в редакторе документов, он может использоваться для Включение и отключение, списки во время раскрывающийся список используется для выбора стиля элемента списка.

> [!NOTE]
> При вызове нажатием кнопки разделения ведет себя как раскрывающийся список кнопки. Для других способов ввода пользователь может вызвать любой половины кнопки отдельно. С помощью сенсорного ввода как части кнопки вызова всплывающее окно. Таким образом необходимо включить параметр в всплывающего контента для включения и отключения кнопки включено или отключено.

### <a name="differences-with-togglebutton"></a>Различия с ToggleButton

В отличие от [ToggleButton](/uwp/api/windows.ui.xaml.controls.primitives.togglebutton)ToggleSplitButton не имеет неопределенное состояние. В результате вы должны помнить следующие отличия:

- ToggleSplitButton не имеет свойство **IsThreeState** или **неопределенный** события.
- Свойство [ToggleSplitButton.IsChecked](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischecked) — это просто **bool**, не **допускает значение NULL bool**.
- ToggleSplitButton имеет только [IsCheckedChanged](/uwp/api/windows.ui.xaml.controls.togglesplitbutton.ischeckedchanged) события; он не имеет отдельные **установлен** или **снят** мероприятиях.

### <a name="example---toggle-split-button"></a>Пример — переключателя Разворачивающаяся кнопка

В следующем примере показано, как можно использовать для Включение или выключение форматирования списка и изменение стиля списка в RichEditBox переключатель Разворачивающаяся кнопка. (Дополнительные сведения и код, см. [поля форматированного редактирования](rich-edit-box.md)).

![Переключатель Разворачивающаяся кнопка для выбора стилей списка](images/toggle-split-button-open.png)

```xaml
<ToggleSplitButton x:Name="ListButton"
                   ToolTipService.ToolTip="List style"
                   Click="ListButton_Click"
                   IsCheckedChanged="ListStyleButton_IsCheckedChanged">
    <TextBlock FontFamily="Segoe MDL2 Assets" FontSize="16" Text="&#xE8FD;"/>
    <ToggleSplitButton.Flyout>
        <Flyout Placement="BottomEdgeAlignedLeft">
            <ListView x:Name="ListStylesListView"
                      SelectionChanged="ListStylesListView_SelectionChanged" 
                      SingleSelectionFollowsFocus="False">
                <StackPanel Tag="bullet" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xE7C8;"/>
                    <TextBlock Text="Bullet" Margin="8,0"/>
                </StackPanel>
                <StackPanel Tag="alpha" Orientation="Horizontal">
                    <TextBlock Text="A" FontSize="24" Margin="2,0"/>
                    <TextBlock Text="Alpha" Margin="8"/>
                </StackPanel>
                <StackPanel Tag="numeric" Orientation="Horizontal">
                    <FontIcon FontFamily="Segoe MDL2 Assets" Glyph="&#xF146;"/>
                    <TextBlock Text="Numeric" Margin="8,0"/>
                </StackPanel>
                <TextBlock Tag="none" Text="None" Margin="28,0"/>
            </ListView>
        </Flyout>
    </ToggleSplitButton.Flyout>
</ToggleSplitButton>
```

```csharp
private void ListStyleButton_IsCheckedChanged(ToggleSplitButton sender, ToggleSplitButtonIsCheckedChangedEventArgs args)
{
    // Use the toggle button to turn the selected list style on or off.
    if (((ToggleSplitButton)sender).IsChecked == true)
    {
        // On. Apply the list style selected in the drop down to the selected text.
        var listStyle = ((FrameworkElement)(ListStylesListView.SelectedItem)).Tag.ToString();
        ApplyListStyle(listStyle);
    }
    else
    {
        // Off. Make the selected text not a list,
        // but don't change the list style selected in the drop down.
        ApplyListStyle("none");
    }
}

private void ListStylesListView_SelectionChanged(object sender, SelectionChangedEventArgs e)
{
    var listStyle = ((FrameworkElement)(e.AddedItems[0])).Tag.ToString();

    if (ListButton.IsChecked == true)
    {
        // Toggle button is on. Turn it off...
        if (listStyle == "none")
        {
            ListButton.IsChecked = false;
        }
        else
        {
            // or apply the new selection.
            ApplyListStyle(listStyle);
        }
    }
    else
    {
        // Toggle button is off. Turn it on, which will apply the selection
        // in the IsCheckedChanged event handler.
        ListButton.IsChecked = true;
    }
}

private void ApplyListStyle(string listStyle)
{
    Windows.UI.Text.ITextSelection selectedText = editor.Document.Selection;
    if (selectedText != null)
    {
        // Apply the list style to the selected text.
        var paragraphFormatting = selectedText.ParagraphFormat;
        if (listStyle == "none")
        {  
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.None;
        }
        else if (listStyle == "bullet")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Bullet;
        }
        else if (listStyle == "numeric")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.Arabic;
        }
        else if (listStyle == "alpha")
        {
            paragraphFormatting.ListType = Windows.UI.Text.MarkerType.UppercaseEnglishLetter;
        }
        selectedText.ParagraphFormat = paragraphFormatting;
    }
}
```

## <a name="recommendations"></a>Рекомендации

- Сделайте так, чтобы назначение и состояние кнопки было понятно пользователю.
- Если для одного решения предусмотрено несколько кнопок (например, в диалоговом окне подтверждения), расположите эти кнопки в таком порядке, где [Выполнить] и [Не выполнять] — конкретные варианты ответа на основную инструкцию:
    - ОК/[Выполнить]/Да
    - [Не выполнять]/Нет
    - Отмена
- Сделайте одновременно доступными пользователю только одну или две кнопки, например "Принять" и "Отклонить". Если требуется предоставить пользователю больше действий, введите в интерфейс [флажки](checkbox.md) или [переключатели](radio-button.md), с помощью которых пользователь сможет выбрать нужные действия, а затем путем нажатия одной кнопки начать выполнение всех этих действий.
- Для действия, которое требуется выполнить над множеством страниц вашего приложения, лучше использовать [нижнюю панель приложения](app-bars.md), а не повторять соответствующую кнопку на каждой странице.

### <a name="recommended-single-button-layout"></a>Рекомендуется макет с одной кнопкой

Если в макете требуется только одна кнопка, она должна быть выровнена либо по левому, либо по правому краю на основе контекста его контейнера.

- Диалоговые окна только с одной кнопкой должны выравнивать кнопку **по правому краю**. Если диалоговое окно содержит только одну кнопку, убедитесь, что кнопка выполняет безопасное, обратимое действие. Если вы используете [ContentDialog](dialogs.md) и укажете одну кнопку, она будет автоматически выровнена по правому краю.

![Кнопка в диалоговом окне](images/pushbutton_doc_dialog.png)

- Если кнопка отображается внутри контейнера пользовательского интерфейса (например, во всплывающем уведомлении, всплывающем элементе или представлении элемента списка), следует выровнять кнопку в контейнере **по правому краю**.

![Кнопка внутри контейнера](images/pushbutton_doc_container.png)

- На страницах, содержащих одну кнопку (например, кнопку "Применить" в нижней части страницы параметров), вы должны выровнять кнопку **по левому краю**. Это гарантирует, что кнопка соответствует остальной части содержимого страницы.

![Кнопка на странице](images/pushbutton_doc_page.png)

## <a name="back-buttons"></a>Кнопки "Назад"

Кнопка "Назад" — это предоставленный системой элемент пользовательского интерфейса для обратной навигации через обратный стек или журнал навигации пользователя. Вы не обязаны создавать собственную кнопку "Назад", но вы можете выполнить определенные действия, чтобы реализовать хорошие возможности обратной навигации. Дополнительные сведения см. в разделе [Журнал и навигация в обратном направлении](../basics/navigation-history-and-backwards-navigation.md)

## <a name="get-the-sample-code"></a>Получить пример кода

- [Образец галереи элементов управления XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) — ознакомьтесь со всеми элементами управления XAML в интерактивном формате.

## <a name="related-articles"></a>Еще по теме

- [Класс Button](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.button.aspx)
- [Переключатели](radio-button.md)
- [Флажки](checkbox.md)
- [Тумблеры](toggles.md)