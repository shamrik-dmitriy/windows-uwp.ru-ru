---
Description: Если ваше приложение не поддерживает доступ с клавиатуры с достаточными возможностями, пользователи с нарушениями зрения или опорно-двигательного аппарата будут испытывать трудности при его использовании или же совсем не смогут его использовать.
ms.assetid: DDAE8C4B-7907-49FE-9645-F105F8DFAD8B
title: Специальные возможности клавиатуры
label: Keyboard accessibility
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 50b9f2a30f529e78773bc40671c9541ff2687b64
ms.sourcegitcommit: 0a319e2e69ef88b55d472b009b3061a7b82e3ab1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/21/2020
ms.locfileid: "77521235"
---
# <a name="keyboard-accessibility"></a>Специальные возможности клавиатуры  



Если ваше приложение не поддерживает доступ с клавиатуры с достаточными возможностями, пользователи с нарушениями зрения или опорно-двигательного аппарата будут испытывать трудности при его использовании или же совсем не смогут его использовать.

<span id="keyboard_navigation_among_UI_elements"/>
<span id="keyboard_navigation_among_ui_elements"/>
<span id="KEYBOARD_NAVIGATION_AMONG_UI_ELEMENTS"/>

## <a name="keyboard-navigation-among-ui-elements"></a>Навигация по элементам пользовательского интерфейса при помощи клавиатуры  
Чтобы можно было использовать клавиатуру с элементом управления, этот элемент управления должен быть снабжен фокусом, а чтобы получить фокус (не используя указатель), элемент управления должен быть доступен в пользовательском интерфейсе посредством навигации клавишей Tab. По умолчанию последовательность табуляции для элементов управления выполняется в таком же порядке, в каком они добавлены в рабочую область конструирования, перечислены в XAML или программно добавлены в контейнер.

В большинстве случаев наилучшим вариантом является порядок по умолчанию, зависящий от того, как вы определили элементы управления в XAML. В частности это связано с тем, что средства чтения с экрана считывают элементы управления именно в таком порядке. Тем не менее порядок по умолчанию не обязательно совпадает с визуальным порядком. Фактическое расположение на экране может зависеть от родительского контейнера макета и определенных свойств, которые можно установить для дочерних элементов, чтобы изменить макет. Чтобы убедиться в правильной последовательности табуляции, проверьте его самостоятельно. Если в макете есть метафора сетки или таблицы, то порядок, в котором читают пользователи, может отличаться от последовательности табуляции. Сама по себе такая ситуация не всегда является проблемой. Однако следует проверить функциональность приложения как при использовании сенсорного пользовательского интерфейса, так и при использовании клавиатуры, чтобы убедиться в его правильной работе.

Регулирование, реализуемое на XAML, позволяет последовательности табуляции соответствовать визуальному порядку. Можно также переопределить последовательность табуляции по умолчанию, установив свойство [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex), как показано в следующем примере макета [**Grid**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid), где используется навигация сначала по столбцам.

XAML
```xml
<!--Custom tab order.-->
<Grid>
  <Grid.RowDefinitions>...</Grid.RowDefinitions>
  <Grid.ColumnDefinitions>...</Grid.ColumnDefinitions>

  <TextBlock Grid.Column="1" HorizontalAlignment="Center">Groom</TextBlock>
  <TextBlock Grid.Column="2" HorizontalAlignment="Center">Bride</TextBlock>

  <TextBlock Grid.Row="1">First name</TextBlock>
  <TextBox x:Name="GroomFirstName" Grid.Row="1" Grid.Column="1" TabIndex="1"/>
  <TextBox x:Name="BrideFirstName" Grid.Row="1" Grid.Column="2" TabIndex="3"/>

  <TextBlock Grid.Row="2">Last name</TextBlock>
  <TextBox x:Name="GroomLastName" Grid.Row="2" Grid.Column="1" TabIndex="2"/>
  <TextBox x:Name="BrideLastName" Grid.Row="2" Grid.Column="2" TabIndex="4"/>
</Grid>
```

Элемент управления можно исключить из последовательности табуляции. Обычно для этого нужно только сделать элемент управления неинтерактивным, например путем установки для свойства [**IsEnabled**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.isenabled) значения **false**. Отключенный элемент управления автоматически не используется в последовательности табуляции. Возможно, потребуется исключить элемент управления из последовательности табуляции, даже если он не отключен. В этом случае можно задать для свойства [**IsTabStop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) значение **false**.

Все элементы, которые могут иметь фокус, обычно включены в последовательность табуляции по умолчанию. Исключением являются некоторые типы отображения текста, например [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Они могут иметь фокус и быть доступными для выделения текста через буфер обмена, однако не включаются в последовательность табуляции, так как не являются статическими текстовыми элементами. Они не являются интерактивными в традиционном смысле (их нельзя вызвать, они не требуют ввода текста, но поддерживают [шаблон элемента управления Text](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-controlpatternsoverview), который обеспечивает поиск и настройку точек выделения в тексте). Не должно подразумеваться, что установка фокуса на тексте сделает возможным какое-то действие. Текстовые элементы будут обнаруживаться специальными возможностями и читаться вслух программами чтения с экрана, но для этого, помимо поиска элементов в фактической последовательности табуляции, используются другие методы.

В процессе изменения значений [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) или использования последовательности перехода по умолчанию применяются следующие правила.

* Элементы пользовательского интерфейса со значением свойства [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex), равным нулю, добавляются в последовательность перехода в порядке, указанном в объявлении на XAML или в коллекциях дочерних объектов.
* Элементы пользовательского интерфейса со значением свойства [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) больше нуля добавляются в последовательность перехода в зависимости от значения **TabIndex**.
* Элементы пользовательского интерфейса со значением свойства [**TabIndex**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.tabindex) меньше нуля добавляются в последовательность перехода и появляются перед любым элементом с нулевым значением. Это может отличаться от обработки атрибута **tabindex** в HTML (кроме того, отрицательный **tabindex** не поддерживался в прежних спецификациях HTML).

<span id="keyboard_navigation_within_a_UI_element"/>
<span id="keyboard_navigation_within_a_ui_element"/>
<span id="KEYBOARD_NAVIGATION_WITHIN_A_UI_ELEMENT"/>

## <a name="keyboard-navigation-within-a-ui-element"></a>Навигация при помощи клавиатуры внутри элемента пользовательского интерфейса  
Важно убедиться в том, что в составных элементах внутренняя навигация по элементам-контейнерам осуществляется правильно. Составной элемент может управлять текущим дочерним объектом, чтобы уменьшить объем служебных данных, направленных на поддержку фокусировки всех дочерних элементов. Подобный составной элемент включается в последовательность табуляции и сам обрабатывает команды навигации с клавиатуры. Многие составные элементы управления уже имеют некоторую внутреннюю логику навигации, встроенную в обработчик событий элемента управления. Например, обход с помощью клавиш со стрелками включается по умолчанию для элементов управления [**ListView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListView), [**GridView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.gridview), [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) и [**FlipView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.FlipView).

<span id="keyboard_activation"/>
<span id="KEYBOARD_ACTIVATION"/>

## <a name="keyboard-alternatives-to-pointer-actions-and-events-for-specific-control-elements"></a>Альтернативные сочетания клавиш для выполнения действий и вызова событий указателя на специальных элементах управления  
Убедитесь, что элементы пользовательского интерфейса, которые можно нажать, также могут вызываться с помощью клавиатуры. Однако чтобы использовать клавиатуру для управления пользовательским интерфейсом, элемент должен иметь фокус. Только классы, наследованные от [**Control**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control), будут поддерживать фокус и навигацию по вкладкам.

Для вызываемых элементов пользовательского интерфейса создайте обработчики событий клавиатуры для клавиш ПРОБЕЛ и ВВОД. Это реализует полную поддержку основных специальных возможностей клавиатуры и позволяет пользователям выполнять основные сценарии приложений, пользуясь только клавиатурой, то есть пользователи имеют доступ ко всем интерактивным элементам интерфейса и могут активировать любую функцию по умолчанию.

В тех случаях, когда элемент, который необходимо использовать в пользовательском интерфейсе, не может иметь фокус, можно создать пользовательский элемент управления. Установите для свойства [**IsTabStop**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.istabstop) значение **true**, чтобы включить фокус и обеспечить визуальную индикацию с фокусом ввода, создав визуальное состояние, отображающее индикатор фокуса в пользовательском интерфейсе. Зачастую проще использовать композицию элементов управления. В этом случае поддержка остановок перехода, фокуса, а также узлов партнера и шаблонов модели автоматизации пользовательского интерфейса осуществляется элементом управления, который вы выбрали для составления вашего содержимого.

Например, вместо того чтобы обрабатывать событие нажатия указателя на [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image), можно использовать [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) в качестве оболочки этого элемента, чтобы реализовать поддержку фокуса, указателя и клавиатуры.

XAML
```xml
<!--Don't do this.-->
<Image Source="sample.jpg" PointerPressed="Image_PointerPressed"/>

<!--Do this instead.-->
<Button Click="Button_Click"><Image Source="sample.jpg"/></Button>
```

<span id="keyboard_shortcuts"/>
<span id="KEYBOARD_SHORTCUTS"/>

## <a name="keyboard-shortcuts"></a>сочетания клавиш  
Помимо реализации навигации и активации при помощи клавиатуры в приложении, рекомендуем назначить сочетания клавиш для функций приложения. Навигация с помощью клавиши Tab обеспечивает хороший базовый уровень поддержки клавиатуры, но в случае сложных форм может потребоваться поддержка быстрого вызова. Это повысит эффективность использования вашего приложения даже для тех, кто использует и клавиатуру и указывающие устройства.

*Сочетание клавиш* — комбинация клавиш, улучшающая производительность путем предоставления пользователям удобного доступа к функциям приложения. Существует два вида ярлыка:

* *Клавиша доступа* — сочетание клавиш, обеспечивающее доступ к элементу интерфейса вашего приложения. Клавиши доступа представляют собой сочетание клавиши ALT и клавиши с буквой.
* *Клавиша вызова* — сочетание клавиш для вызова команды приложения. Ваше приложение может содержать или не содержать элемент интерфейса, который в точности соответствует этой команде. Клавиши вызова представляют собой сочетание клавиши CTRL и клавиши с буквой.

Вы обязательно должны предоставить пользователям, которые пользуются программами чтения с экрана и другими специальными возможностями, простой способ узнать сочетания клавиш для вашего приложения. Предоставьте информацию о сочетании клавиш при помощи всплывающих подсказок, специальных имен, специальных описаний и других видов экранных средств взаимодействия. По крайней мере сочетания клавиш должны быть подробно описаны в документации раздела справки вашего приложения.

Вы можете задокументировать клавиши доступа с помощью устройств чтения с экрана, задав в присоединенном свойстве [**AutomationProperties.AccessKey**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) строку, которая описывает быстрый вызов. Существует также присоединенное свойство [**AutomationProperties.AcceleratorKey**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) для документирования немнемонических быстрых вызовов, хотя устройства чтения с экрана обычно обрабатывают эти свойства одинаково. Быстрые вызовы попробуйте документировать несколькими способами, используя всплывающие подсказки, свойства автоматизации и справочную документацию.

Следующий пример демонстрирует, как задокументировать сочетания клавиш для кнопок воспроизведения, приостановки и остановки мультимедиа.

XAML
```xml
<Grid KeyDown="Grid_KeyDown">

  <Grid.RowDefinitions>
    <RowDefinition Height="Auto" />
    <RowDefinition Height="Auto" />
  </Grid.RowDefinitions>

  <MediaElement x:Name="DemoMovie" Source="xbox.wmv"
    Width="500" Height="500" Margin="20" HorizontalAlignment="Center" />

  <StackPanel Grid.Row="1" Margin="10"
    Orientation="Horizontal" HorizontalAlignment="Center">

    <Button x:Name="PlayButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+P"
      AutomationProperties.AcceleratorKey="Control P">
      <TextBlock>Play</TextBlock>
    </Button>

    <Button x:Name="PauseButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+A"
      AutomationProperties.AcceleratorKey="Control A">
      <TextBlock>Pause</TextBlock>
    </Button>

    <Button x:Name="StopButton" Click="MediaButton_Click"
      ToolTipService.ToolTip="Shortcut key: Ctrl+S"
      AutomationProperties.AcceleratorKey="Control S">
      <TextBlock>Stop</TextBlock>
    </Button>
  </StackPanel>
</Grid>
```

> [!IMPORTANT]
> Установка [**AutomationProperties. акцелераторкэй**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.acceleratorkeyproperty) или [**AutomationProperties. AccessKey**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accesskeyproperty) не включает функцию клавиатуры. При этом инфраструктуре автоматизации пользовательского интерфейса сообщается только то, какие клавиши должны быть использованы, чтобы эта информация могла быть передана пользователям с помощью специальных возможностей. Реализация обработки клавиш все еще должна производиться в коде, а не в XAML. Но вам все равно нужно подключить обработчики для событий [**KeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keydown) или [**KeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.keyup) на соответствующем элементе управления, чтобы фактически реализовать применение сочетания клавиш в вашем приложении. Подчеркивание клавиши доступа не предоставляется автоматически. Вы должны явным образом подчеркнуть текст, относящийся к определенной назначенной клавише в качестве встроенного форматирования [**Underline**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Documents.Underline), если вы хотите, чтобы подчеркнутый текст отображался в пользовательском интерфейсе.

Для упрощения представления в предыдущем примере не используются ресурсы для таких строк, как Ctrl+A. Однако вы также должны учитывать сочетания клавиш при локализации. Локализация сочетания клавиш имеет важное значение, потому что их выбор обычно зависит от видимых текстовых подписей для элемента.

Подробнее о реализации сочетаний клавиш см. в разделе [Сочетания клавиш](https://docs.microsoft.com/windows/win32/uxguide/inter-keyboard?redirectedfrom=MSDN) Справочника Windows по взаимодействию с пользователями.

<span id="Implementing_a_key_event_handler"/>
<span id="implementing_a_key_event_handler"/>
<span id="IMPLEMENTING_A_KEY_EVENT_HANDLER"/>

### <a name="implementing-a-key-event-handler"></a>Реализация обработчика событий для клавиш  
В событиях ввода, таких как события нажатия клавиш, используется концепция, называемая *перенаправленные события*. Перенаправленное событие может перемещаться по дочерним элементам составного элемента управления таким образом, что общий родительский элемент управления будет обрабатывать события для нескольких дочерних элементов. Эта модель событий удобна тем, что позволяет определить действия при нажатии сочетания клавиш для элемента управления, содержащего несколько составных частей, которые конструктивно не могут иметь фокус или входить в последовательность табуляции.

Пример кода для обработчика событий нажатия клавиш с проверкой модификаторов, таких как клавиша CTRL, можно найти в разделе [Взаимодействие с помощью клавиатуры](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions).

<span id="Keyboard_navigation_for_custom_controls"/>
<span id="keyboard_navigation_for_custom_controls"/>
<span id="KEYBOARD_NAVIGATION_FOR_CUSTOM_CONTROLS"/>

## <a name="keyboard-navigation-for-custom-controls"></a>Навигация при помощи клавиатуры для пользовательских элементов управления  
Если дочерние элементы имеют пространственные связи друг с другом, рекомендуется в качестве сочетания клавиш для навигации по дочерним элементам использовать клавиши со стрелками. Если в узлах дерева развертывание/свертывание и активация узла выполняются разными дочерними элементами, используйте клавиши СТРЕЛКА ВЛЕВО и СТРЕЛКА ВПРАВО, чтобы добавить функцию управления развертыванием/свертыванием. При наличии ориентированного элемента управления, поддерживающего направленное отслеживание в пределах управляемого содержимого, используйте соответствующие клавиши со стрелками.

Как правило, реализация собственной обработки нажатия клавиш пользовательского элемента управления осуществляется переопределением методов [**OnKeyDown**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeydown) и [**OnKeyUp**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.control.onkeyup), входящих в состав логики класса.

<span id="An_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="an_example_of_a_visual_state_for_a_focus_indicator"/>
<span id="AN_EXAMPLE_OF_A_VISUAL_STATE_FOR_A_FOCUS_INDICATOR"/>

## <a name="an-example-of-a-visual-state-for-a-focus-indicator"></a>Пример визуального состояния индикатора фокуса  
Ранее мы уже говорили о том, что любой пользовательский элемент управления, позволяющий пользователю фокусировать его, должен иметь визуальный индикатор фокуса. Как правило, создать такой индикатор фокуса так же просто, как начертить прямоугольник, который бы вместил в себя обычный ограничивающий прямоугольник этого элемента управления. Элемент [**Rectangle**](/uwp/api/Windows.UI.Xaml.Shapes.Rectangle) для визуального фокуса является одноранговым элементом для других элементов композиции шаблона элемента управления, но первоначально он задается значением [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) для **Collapsed**, так как элемент управления еще не сфокусирован. Затем, когда у элемента управления появляется фокус, вызывается визуальное состояние, которое специально задает **Visibility** фокуса видимым для **Visible**. Как только фокус смещается, вызывается другое визуальное состояние, и **Visibility** становится **Collapsed**.

Все элементы управления XAML по умолчанию при фокусировке будут отображать соответствующий индикатор визуального фокуса (если для них возможна фокусировка). Они могут выглядеть по-разному в зависимости от выбранной пользователем темы (особенно если пользователь использует тему высокой контрастности). Если вы используете элементы управления XAML в пользовательском интерфейсе и не заменяете шаблоны элементов управления, то для того, чтобы получить индикаторы визуального фокуса на элементах управления, которые работают и отображаются правильно, никаких дополнительных действий не потребуется. Но если вы планируете изменить шаблон элемента управления или хотите узнать, как элементы управления XAML предоставляют индикаторы визуального фокуса, то ниже вы найдете объяснения принципа их работы в XAML и в логике элемента управления.

Вот пример кода XAML, составленного по стандартному шаблону XAML для [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button).

XAML
```xml
<ControlTemplate TargetType="Button">
...
    <Rectangle
      x:Name="FocusVisualWhite"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualWhiteStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="1.5"/>
    <Rectangle
      x:Name="FocusVisualBlack"
      IsHitTestVisible="False"
      Stroke="{ThemeResource FocusVisualBlackStrokeThemeBrush}"
      StrokeEndLineCap="Square"
      StrokeDashArray="1,1"
      Opacity="0"
      StrokeDashOffset="0.5"/>
...
</ControlTemplate>
```

Пока это только композиция. Чтобы корректировать видимость индикатора фокуса, нужно определить визуальные состояния, переключающие свойство [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility). Это делается с помощью вложенного свойства [VisualStateManager](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateManager) и VisualStateManager. VisualStateGroups, которое применяется к корневому элементу, определяющему композицию.

XAML
```xml
<ControlTemplate TargetType="Button">
  <Grid>
    <VisualStateManager.VisualStateGroups>
       <!--other visual state groups here-->
       <VisualStateGroup x:Name="FocusStates">
         <VisualState x:Name="Focused">
           <Storyboard>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualWhite"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
             <DoubleAnimation
               Storyboard.TargetName="FocusVisualBlack"
               Storyboard.TargetProperty="Opacity"
               To="1" Duration="0"/>
         </VisualState>
         <VisualState x:Name="Unfocused" />
         <VisualState x:Name="PointerFocused" />
       </VisualStateGroup>
     <VisualStateManager.VisualStateGroups>
<!--composition is here-->
   </Grid>
</ControlTemplate>
```

Обратите внимание на то, что только одно из названных состояний напрямую настраивает [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility), в то время как другие состояния кажутся пустыми. Визуальные состояния работают следующим образом: как только элемент управления использует другое состояние из того же [**VisualStateGroup**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualStateGroup), любые анимации, применявшиеся предыдущим состоянием, сразу же отменяются. Так как значение по умолчанию **Visibility** композиции равно **Collapsed**, это означает, что прямоугольник не будет отображен. Для этого управляющая логика прослушивает события фокуса, такие как [**GotFocus**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.gotfocus), и изменяет состояния с помощью [**GoToState**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.visualstatemanager.gotostate). Часто эта логика уже реализована, если вы используете элемент управления по умолчанию или ведете настройку на основе элемента, уже обладающего такими возможностями.

<span id="Keyboard_accessibility_and_Windows_Phone"/>
<span id="keyboard_accessibility_and_windows_phone"/>
<span id="KEYBOARD_ACCESSIBILITY_AND_WINDOWS_PHONE"/>

## <a name="keyboard-accessibility-and-windows-phone"></a>Специальные возможности клавиатуры и Windows Phone
Обычно устройство Windows Phone не имеет специальной аппаратной клавиатуры. Однако программная панель ввода (SIP) может поддерживать некоторые сценарии специальных возможностей клавиатуры. Средства чтения с экрана могут читать вслух текст, вводимый с использованием панели ввода **Текст**, а также объявлять о его удалении. Пользователи могут определить положение своих пальцев, поскольку средство чтения с экрана может обнаружить, как пользователь движется по клавишам, и вслух произносить название текущей клавиши. Кроме того, некоторые понятия специальных возможностей для клавиатуры можно сопоставить с отдельными аспектами специальных возможностей, в которых клавиатура не используется. Например, на панели SIP нет клавиши Tab, но экранный диктор поддерживает сенсорный жест, эквивалентный нажатию клавиши Tab, и поэтому для реализации специальных возможностей по-прежнему важно наличие удобной последовательности табуляции по элементам управления. Клавиши со стрелками, служащие для переходов в пределах составных элементов управления, также поддерживаются сенсорными жестами экранного диктора. Если фокус переходит в элемент управления, который не поддерживает текстовый ввод, экранный диктор поддерживает жест, вызывающий действие этого элемента управления.

В приложениях для Windows Phone обычно не используются сочетания клавиш, так как панель SIP не имеет клавиш Ctrl или Alt.

<span id="related_topics"/>

## <a name="related-topics"></a>Связанные разделы

* [Специальные возможности](accessibility.md)
* [Взаимодействие с клавиатурой](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions)
* [Пример сенсорной клавиатуры](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)
* [Пример специальных возможностей XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
