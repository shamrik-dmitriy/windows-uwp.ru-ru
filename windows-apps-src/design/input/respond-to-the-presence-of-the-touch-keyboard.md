---
Description: Узнайте, как адаптировать пользовательский интерфейс приложения при отображении или скрытии сенсорной клавиатуры.
title: Реакция на наличие сенсорной клавиатуры
ms.assetid: 70C6130E-23A2-4F9D-88E7-7060062DA988
label: Respond to the presence of the touch keyboard
template: detail.hbs
keywords: клавиатура, специальные возможности, навигация, фокус, текст, ввод, взаимодействия с пользователем
ms.date: 07/13/2018
ms.topic: article
ms.openlocfilehash: e44b7cf5a61a795e52490f6d603aea0bcf87bea2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57658299"
---
# <a name="respond-to-the-presence-of-the-touch-keyboard"></a>Реакция на наличие сенсорной клавиатуры

Узнайте, как адаптировать пользовательский интерфейс приложения при отображении или скрытии сенсорной клавиатуры.

### <a name="important-apis"></a>Важные API

- [AutomationPeer](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer)
- [InputPane](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane)

![сенсорная клавиатура в режиме раскладки по умолчанию](images/keyboard/default.png)

<sup>Сенсорное устройство клавиатуры по умолчанию режим макета</sup>

С помощью сенсорной клавиатуры можно вводить текст на устройствах, поддерживающих сенсорный ввод. В универсальной платформе Windows (UWP) элементы управления для ввода текста по умолчанию вызывают сенсорную клавиатуру, когда пользователь касается редактируемого поля ввода. Обычно сенсорная клавиатура остается видимой, когда пользователь переходит по элементам управления в форме, но это поведение может меняться в зависимости от имеющихся в форме элементов управления других типов.

Для поддержки соответствующего поведение сенсорной клавиатуры в пользовательского ввода текста, не является производным от элемент управления для ввода стандартного текста, необходимо использовать <a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer">AutomationPeer</a> класса для предоставления элементов управления автоматизации пользовательского интерфейса и Реализуйте правильный шаблоны элементов управления модели автоматизации пользовательского интерфейса. См. статьи [Специальные возможности клавиатуры](https://docs.microsoft.com/windows/uwp/design/accessibility/keyboard-accessibility) и [Настраиваемые одноранговые элементы автоматизации](https://docs.microsoft.com/windows/uwp/design/accessibility/custom-automation-peers).

После добавления такой поддержки в пользовательский элемент управления приложение сможет правильно реагировать на наличие сенсорной клавиатуры.

**Предварительные требования:**

В данной статье использованы материалы статьи [Взаимодействие с помощью клавиатуры](keyboard-interactions.md).

Для работы со статьей необходимо иметь базовое представление о стандартных взаимодействиях клавиатуры, обработке событий клавиатуры и данных, вводимых с клавиатуры, а также о модели автоматизации пользовательского интерфейса.

Если вы — начинающий разработчик приложений универсальной платформы Windows (UWP), прочитайте указанные ниже статьи, чтобы ознакомиться с описанными здесь технологиями.

- [Создание первого приложения](https://docs.microsoft.com/windows/uwp/get-started/your-first-app)
- Дополнительную информацию о событиях см. в разделе [Общие сведения о событиях и перенаправленных событиях](https://docs.microsoft.com/windows/uwp/xaml-platform/events-and-routed-events-overview).

**Рекомендации по пользовательскому интерфейсу:**

Полезные советы по разработке полезных и привлекательных приложений, оптимизированный для ввода с клавиатуры, см. в разделе [сочетания взаимодействия](https://docs.microsoft.com/windows/uwp/design/input/keyboard-interactions) .

## <a name="touch-keyboard-and-a-custom-ui"></a>Сенсорная клавиатура и настраиваемый пользовательский интерфейс

Ниже перечислены несколько основных рекомендаций для пользовательских элементов управления для ввода текста.

- Сенсорная клавиатура должна отображаться в течение всего времени взаимодействия пользователя с формой.

- Убедитесь, что пользовательские элементы управления имеют соответствующие модели автоматизации пользовательского интерфейса [AutomationControlType](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType) на клавиатуре, сохранять, когда фокус перемещается из поля ввода текста в контексте ввод текста. Например, если в середине сценария текстового ввода открывается меню, а клавиатура должна по-прежнему отображаться, меню должно принадлежать к типу элемента управления меню **AutomationControlType**.

- Не изменяйте свойства модели автоматизации пользовательского интерфейса для управления сенсорной клавиатурой. От точности настройки этих свойств зависят другие средства специальных возможностей.

- Убедитесь, что пользователю всегда видно поле ввода, с которым он взаимодействует.

    Так как сенсорная клавиатура занимает большую часть экрана, UWP делает так, что поле ввода, находящееся в фокусе, всегда будет в поле зрения пользователя, когда тот переходит по элементам управления в форме, включая те элементы управления, которые в данный момент находятся вне поля зрения.

    При настройке пользовательского интерфейса, предоставлять подобное поведение на внешний вид сенсорной клавиатуре при обработке [отображение](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) и [скрытие](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) события, предоставляемые [ **InputPane** ](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.InputPane) объекта.

    ![Форма с сенсорной клавиатурой и без нее](images/touch-keyboard-pan1.png)

    В ряде случаев некоторые элементы пользовательского интерфейса должны присутствовать на экране постоянно. При разработке пользовательского интерфейса следите за тем, чтобы элементы управления формы находились в области панорамирования, а важные элементы пользовательского интерфейса были статичными. Например:

    ![Форма, содержащая области, которые всегда должны быть в поле зрения](images/touch-keyboard-pan2.png)

## <a name="handling-the-showing-and-hiding-events"></a>Обработка событий отображения и скрытия

Ниже приведен пример присоединения обработчиков событий для [отображение](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.showing) и [скрытие](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.inputpane.hiding) события сенсорной клавиатуры.

```csharp
using Windows.UI.ViewManagement;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Media;
using Windows.Foundation;
using Windows.UI.Xaml.Navigation;

namespace SDKTemplate
{
    /// <summary>
    /// Sample page to subscribe show/hide event of Touch Keyboard.
    /// </summary>
    public sealed partial class Scenario2_ShowHideEvents : Page
    {
        public Scenario2_ShowHideEvents()
        {
            this.InitializeComponent();
        }

        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Subscribe to Showing/Hiding events
            currentInputPane.Showing += OnShowing;
            currentInputPane.Hiding += OnHiding;
        }

        protected override void OnNavigatedFrom(NavigationEventArgs e)
        {
            InputPane currentInputPane = InputPane.GetForCurrentView();
            // Unsubscribe from Showing/Hiding events
            currentInputPane.Showing -= OnShowing;
            currentInputPane.Hiding -= OnHiding;
        }

        void OnShowing(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Showing";
        }
        
        void OnHiding(InputPane sender, InputPaneVisibilityEventArgs e)
        {
            LastInputPaneEventRun.Text = "Hiding";
        }
    }
}
```

```cppwinrt
...
#include <winrt/Windows.UI.ViewManagement.h>
...
private:
    winrt::event_token m_showingEventToken;
    winrt::event_token m_hidingEventToken;
...
Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Subscribe to Showing/Hiding events
    m_showingEventToken = inputPane.Showing({ this, &Scenario2_ShowHideEvents::OnShowing });
    m_hidingEventToken = inputPane.Hiding({ this, &Scenario2_ShowHideEvents::OnHiding });
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(Windows::UI::Xaml::Navigation::NavigationEventArgs const& e)
{
    auto inputPane{ Windows::UI::ViewManagement::InputPane::GetForCurrentView() };
    // Unsubscribe from Showing/Hiding events
    inputPane.Showing(m_showingEventToken);
    inputPane.Hiding(m_hidingEventToken);
}

void Scenario2_ShowHideEvents::OnShowing(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Showing");
}

void Scenario2_ShowHideEvents::OnHiding(Windows::UI::ViewManagement::InputPane const& /*sender*/, Windows::UI::ViewManagement::InputPaneVisibilityEventArgs const& /*args*/)
{
    LastInputPaneEventRun().Text(L"Hiding");
}
```

```cpp
#include "pch.h"
#include "Scenario2_ShowHideEvents.xaml.h"

using namespace SDKTemplate;
using namespace Platform;
using namespace Windows::Foundation;
using namespace Windows::UI::ViewManagement;
using namespace Windows::UI::Xaml;
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::UI::Xaml::Navigation;

Scenario2_ShowHideEvents::Scenario2_ShowHideEvents()
{
    InitializeComponent();
}

void Scenario2_ShowHideEvents::OnNavigatedTo(NavigationEventArgs^ e)
{
    auto inputPane = InputPane::GetForCurrentView();
    // Subscribe to Showing/Hiding events
    showingEventToken = inputPane->Showing +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnShowing);
    hidingEventToken = inputPane->Hiding +=
        ref new TypedEventHandler<InputPane^, InputPaneVisibilityEventArgs^>(this, &Scenario2_ShowHideEvents::OnHiding);
}

void Scenario2_ShowHideEvents::OnNavigatedFrom(NavigationEventArgs^ e)
{
    auto inputPane = Windows::UI::ViewManagement::InputPane::GetForCurrentView();
    // Unsubscribe from Showing/Hiding events
    inputPane->Showing -= showingEventToken;
    inputPane->Hiding -= hidingEventToken;
}

void Scenario2_ShowHideEvents::OnShowing(InputPane^ /*sender*/, InputPaneVisibilityEventArgs^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Showing";
}

void Scenario2_ShowHideEvents::OnHiding(InputPane^ /*sender*/, InputPaneVisibilityEventArgs ^ /*args*/)
{
    LastInputPaneEventRun->Text = L"Hiding";
}
```

## <a name="related-articles"></a>Связанные статьи

- [Взаимодействия пользователей с клавиатурой](keyboard-interactions.md)
- [Специальные возможности клавиатуры](https://msdn.microsoft.com/library/windows/apps/mt244347)
- [Настраиваемые одноранговые классы автоматизации](https://msdn.microsoft.com/library/windows/apps/mt297667)

**Примеры**

- [Пример сенсорной клавиатуры](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/TouchKeyboard)

**Примеры из архива**

- [Входные данные: Пример сенсорной клавиатуры](https://go.microsoft.com/fwlink/p/?linkid=246019)
- [Реагирование на внешний вид экранную клавиатуру образца](https://go.microsoft.com/fwlink/p/?linkid=231633)
- [Пример редактирования текста XAML](https://go.microsoft.com/fwlink/p/?LinkID=251417)
- [XAML примера специальных возможностей](https://go.microsoft.com/fwlink/p/?linkid=238570)