---
Description: Переключатели типа Radio Button позволяют пользователю выбрать один параметр из двух или более предлагаемых вариантов.
title: Руководство по элементам управления Radio Button
ms.assetid: 41E3F928-AA55-42A2-9281-EC3907C4F898
label: Radio buttons
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: ad18426a36503c9a540343565c20297502810b76
ms.sourcegitcommit: af4050f69168c15b0afaaa8eea66a5ee38b88fed
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2020
ms.locfileid: "80081647"
---
# <a name="radio-buttons"></a>Переключатели

Переключатели позволяют пользователям выбирать один вариант из набора. Каждый параметр представлен одним переключателем, и пользователь может выбрать только один переключатель из группы.

(Если вам интересно, откуда взялось название этого переключателя на английском (Radio Button) — оно происходит от названия кнопок настройки каналов на радиоприемниках.)

![Переключатели](images/controls/radio-button.png)

> **API платформы**: [RadioButton class](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton), [Checked event](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.Checked), [IsChecked property](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked)

## <a name="is-this-the-right-control"></a>Выбор правильного элемента управления

Используйте кнопки переключателей, чтобы предлагать пользователям два или более взаимоисключающих параметра.

![Группа переключателей](images/radiobutton_basic.png)

Используйте переключатели, когда пользователям требуется просмотреть все варианты выбора. Так как в переключателях все предлагаемые варианты равнозначны, это может привлекать к ним больше внимания, чем они того заслуживают. Если упомянутые варианты не заслуживают специального внимания со стороны пользователя, попробуйте использовать другие элементы управления. Например, если большинству пользователей в большинстве ситуаций нужен параметр по умолчанию, лучше использовать [раскрывающийся список](lists.md).

![Раскрывающийся список](images/combo_box_collapsed.png)

Если есть лишь два взаимоисключающих варианта, объедините их в единый [флажок](checkbox.md) или [переключатель](toggles.md). Например, используйте флажок с надписью "Принимаю" вместо двух переключателей "Я принимаю" и "Я не принимаю".

![Два способа представления двоичного выбора](images/radiobutton_vs_checkbox.png)

Когда пользователь может выбрать несколько вариантов, используйте вместо этого [флажок](checkbox.md).

![Выбор нескольких вариантов с помощью флажков](images/checkbox2.png)

Когда к выбору предлагаются числа с фиксированной величиной шага (10, 20, 30), используйте элемент управления [ползунок](slider.md).

![Элемент управления "Ползунок"](images/controls/slider.png)

При наличии более восьми вариантов используйте [раскрывающийся список](lists.md) или [список](lists.md).

![Поле со списком](images/combo_box_scroll.png)

Если доступные варианты основаны на текущем контексте приложения или могут динамически варьироваться по другим причинам, используйте [список](lists.md).

## <a name="examples"></a>Примеры

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Если у вас установлено приложение <strong style="font-weight: semi-bold">галереи элементов управления XAML</strong>, щелкните здесь, чтобы <a href="xamlcontrolsgallery:/item/RadioButton">открыть приложение и увидеть RadioButton в действии</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Получить приложение XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Получить исходный код (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

Переключатели в параметрах браузера Microsoft Edge.

![Переключатели в параметрах браузера Microsoft Edge](images/control-examples/radio-buttons-edge.png)

## <a name="create-a-radio-button"></a>Создание переключателя

Переключатели работают в группах. Существует два способа группировки переключателей.
- Размещение в одном родительском контейнере.
- Установка одинакового значения свойства [GroupName](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RadioButton.GroupName) для всех переключателей.

В этом примере первая группа переключателей неявно группируется путем размещения в одной панели стека. Вторая группа разделена между двумя панелями стека, поэтому они явно группируются с помощью свойства GroupName.

```xaml
<StackPanel>
    <StackPanel>
        <TextBlock Text="Background" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <RadioButton Content="Green" Tag="Green" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Yellow" Tag="Yellow" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="Blue" Tag="Blue" Checked="BGRadioButton_Checked"/>
            <RadioButton Content="White" Tag="White" Checked="BGRadioButton_Checked" IsChecked="True"/>
        </StackPanel>
    </StackPanel>
    <StackPanel>
        <TextBlock Text="BorderBrush" Style="{ThemeResource BaseTextBlockStyle}"/>
        <StackPanel Orientation="Horizontal">
            <StackPanel>
                <RadioButton Content="Green" GroupName="BorderBrush" Tag="Green" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="Yellow" GroupName="BorderBrush" Tag="Yellow" Checked="BorderRadioButton_Checked" IsChecked="True"/>
            </StackPanel>
            <StackPanel>
                <RadioButton Content="Blue" GroupName="BorderBrush" Tag="Blue" Checked="BorderRadioButton_Checked"/>
                <RadioButton Content="White" GroupName="BorderBrush" Tag="White"  Checked="BorderRadioButton_Checked"/>
            </StackPanel>
        </StackPanel>
    </StackPanel>
    <Border x:Name="BorderExample1" BorderThickness="10" BorderBrush="#FFFFD700" Background="#FFFFFFFF" Height="50" Margin="0,10,0,10"/>
</StackPanel>
```

```csharp
private void BGRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.Background = new SolidColorBrush(Colors.Yellow);
                break;
            case "Green":
                BorderExample1.Background = new SolidColorBrush(Colors.Green);
                break;
            case "Blue":
                BorderExample1.Background = new SolidColorBrush(Colors.Blue);
                break;
            case "White":
                BorderExample1.Background = new SolidColorBrush(Colors.White);
                break;
        }
    }
}

private void BorderRadioButton_Checked(object sender, RoutedEventArgs e)
{
    RadioButton rb = sender as RadioButton;

    if (rb != null && BorderExample1 != null)
    {
        string colorName = rb.Tag.ToString();
        switch (colorName)
        {
            case "Yellow":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.Gold);
                break;
            case "Green":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkGreen);
                break;
            case "Blue":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.DarkBlue);
                break;
            case "White":
                BorderExample1.BorderBrush = new SolidColorBrush(Colors.White);
                break;
        }
    }
}
```

Группы переключателей выглядят следующим образом.

![Переключатели в двух группах](images/radio-button-groups.png)

Возможных состояния у переключателя два: *выбран* или *не выбран*. Если переключатель выбран, его свойство [IsChecked](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Primitives.ToggleButton.IsChecked) имеет значение **true**. Если переключатель не выбран, его свойство **IsChecked** имеет значение **false**. Выбор переключателя можно отменить, выбрав другой переключатель в той же группе, но нельзя отменить выбор, щелкнув переключатель еще раз. Однако можно отменить выбор переключателя программным способом, установив для свойства IsChecked значение **false**. Фактически можно сравнить свойство **IsChecked** с логическим значением, получив **Value** свойства **IsChecked**.

## <a name="recommendations"></a>Рекомендации

-   Убедитесь, что назначение и текущее состояние набора переключателей ясны и понятны.
-   Текстовое содержимое переключателя следует ограничить одной строкой.
-   Если текстовое содержимое является динамическим, следует подумать о том, как будет изменяться размер кнопки и что произойдет с визуальными элементами вокруг нее.
-   Используйте шрифт по умолчанию, если указания для торговой марки не требуют использования другого шрифта.
-   Не следует размещать две группы переключателей рядом. Когда две группы переключателей находятся рядом, сложно определить, какой переключатель принадлежит к какой группе.

## <a name="additional-usage-guidance"></a>Дополнительные рекомендации по использованию

На этой иллюстрации показано, как правильно расположить переключатели.

![Набор переключателей](images/radiobutton-layout.png)

![Руководство по расстояниям между переключателями](images/radiobutton-redlines.png)

## <a name="get-the-sample-code"></a>Получение примера кода

- [Пример из коллекции элементов управления XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) — ознакомьтесь со всеми элементами управления XAML в интерактивном режиме.

## <a name="related-topics"></a>Связанные темы

### <a name="for-designers"></a>Проектировщикам

- [Кнопки](buttons.md)
- [Тумблеры](toggles.md)
- [Флажки](checkbox.md)
- [Списки и поля со списком](lists.md)
- [Ползунки](slider.md)

### <a name="for-developers-xaml"></a>Для разработчиков (XAML)

- [Класс RadioButton](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.radiobutton)
