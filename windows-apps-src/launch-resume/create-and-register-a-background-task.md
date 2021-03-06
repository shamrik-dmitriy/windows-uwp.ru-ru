---
title: Создание и регистрация внепроцессной фоновой задачи
description: Создайте класс фоновой задачи, выполняемой вне процесса, и зарегистрируйте его для выполнения, когда приложение не работает на переднем плане.
ms.assetid: 4F98F6A3-0D3D-4EFB-BA8E-30ED37AE098B
ms.date: 02/27/2019
ms.topic: article
keywords: Windows 10, UWP, фоновая задача
ms.localizationpriority: medium
dev_langs:
- csharp
- cppwinrt
- cpp
ms.openlocfilehash: 2124a7141740ef9c16273714864587feff4268f3
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258690"
---
# <a name="create-and-register-an-out-of-process-background-task"></a>Создание и регистрация внепроцессной фоновой задачи

**Важные API**

-   [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask)
-   [**баккграундтаскбуилдер**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder)
-   [**баккграундтасккомплетедевенсандлер**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler)

Создайте класс фоновой задачи и зарегистрируйте его выполнение, когда приложение не работает на переднем плане. В этом разделе рассказывается, как создать и зарегистрировать фоновую задачу, которая будет запускаться в отдельном процессе, а не в процессе вашего приложения. Руководство по реализации выполнения фоновой задачи непосредственно в приложении переднего плана см. в разделе [Создание и регистрация фоновой задачи, выполняемой внутри процесса](create-and-register-an-inproc-background-task.md).

> [!NOTE]
> Если фоновая задача используется для воспроизведения мультимедиа в фоновом режиме, см. раздел [Воспроизведение мультимедиа в фоновом режиме](https://docs.microsoft.com/windows/uwp/audio-video-camera/background-audio), где приведены сведения об улучшениях в Windows 10 версии 1607, которые значительно упрощают работу.

## <a name="create-the-background-task-class"></a>Создание класса фоновой задачи

Для выполнения кода в фоновом режиме можно создавать классы, в которых реализован интерфейс [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask). Этот код выполняется при запуске определенного события с помощью, например, [**системтригжер**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) или [**маинтенанцетригжер**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MaintenanceTrigger).

Далее будет показано, как создать новый класс, реализующий интерфейс [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

1.  Создайте новый проект для фоновых задач и добавьте его в решение. Для этого щелкните правой кнопкой мыши узел решения в **Обозреватель решений** и выберите **Добавить** \> **Новый проект**. Затем выберите тип проекта **компонент Среда выполнения Windows** , присвойте проекту имя и нажмите кнопку ОК.
2.  Создайте ссылку на проект фоновых задач в проекте приложения UWP. Для приложения C# или C++ в проекте приложения щелкните правой кнопкой мыши **ссылки** и выберите команду **Добавить новую ссылку**. В разделе **Решение** выберите **Проекты**, затем выберите имя своего проекта фоновых задач и нажмите **ОК**.
3.  В проект фоновые задачи добавьте новый класс, реализующий интерфейс [**IBackgroundTask**](/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask) . Метод [**IBackgroundTask. Run**](/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) является обязательной точкой входа, которая будет вызываться при активации указанного события. Этот метод является обязательным в каждой фоновой задаче.

> [!NOTE]
> Класс фоновой задачи&mdash;и все остальные классы в проекте фоновой задачи&mdash;должны быть **открытыми** классами, которые являются **запечатанными** (или **последними**).

В следующем образце кода показана самая простая начальная точка для класса фоновой задачи.

```csharp
// ExampleBackgroundTask.cs
using Windows.ApplicationModel.Background;

namespace Tasks
{
    public sealed class ExampleBackgroundTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            
        }        
    }
}
```

```cppwinrt
// First, add ExampleBackgroundTask.idl, and then build.
// ExampleBackgroundTask.idl
namespace Tasks
{
    [default_interface]
    runtimeclass ExampleBackgroundTask : Windows.ApplicationModel.Background.IBackgroundTask
    {
        ExampleBackgroundTask();
    }
}

// ExampleBackgroundTask.h
#pragma once

#include "ExampleBackgroundTask.g.h"

namespace winrt::Tasks::implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask>
    {
        ExampleBackgroundTask() = default;

        void Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance);
    };
}

namespace winrt::Tasks::factory_implementation
{
    struct ExampleBackgroundTask : ExampleBackgroundTaskT<ExampleBackgroundTask, implementation::ExampleBackgroundTask>
    {
    };
}

// ExampleBackgroundTask.cpp
#include "pch.h"
#include "ExampleBackgroundTask.h"

namespace winrt::Tasks::implementation
{
    void ExampleBackgroundTask::Run(Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
    {
        throw hresult_not_implemented();
    }
}
```

```cpp
// ExampleBackgroundTask.h
#pragma once

using namespace Windows::ApplicationModel::Background;

namespace Tasks
{
    public ref class ExampleBackgroundTask sealed : public IBackgroundTask
    {

    public:
        ExampleBackgroundTask();

        virtual void Run(IBackgroundTaskInstance^ taskInstance);
        void OnCompleted(
            BackgroundTaskRegistration^ task,
            BackgroundTaskCompletedEventArgs^ args
        );
    };
}

// ExampleBackgroundTask.cpp
#include "ExampleBackgroundTask.h"

using namespace Tasks;

void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
}
```

4.  Если в вашей фоновой задаче выполняется асинхронный код, для нее необходимо использовать задержку. Если вы не используете РБП, процесс фоновой задачи может неожиданно завершиться, если метод **Run** возвращает значение до завершения выполнения любой асинхронной работы.

Запросите РБП в методе **Run** перед вызовом асинхронного метода. Сохраните РБП в члене данных класса, чтобы к нему можно было получить доступ из асинхронного метода. Объявите задержку завершенной после выполнения асинхронного кода.

Следующий пример кода получает РБП, сохраняет его и освобождает после завершения асинхронного кода.

```csharp
BackgroundTaskDeferral _deferral; // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation
public async void Run(IBackgroundTaskInstance taskInstance)
{
    _deferral = taskInstance.GetDeferral();
    //
    // TODO: Insert code to start one or more asynchronous methods using the
    //       await keyword, for example:
    //
    // await ExampleMethodAsync();
    //

    _deferral.Complete();
}
```

```cppwinrt
// ExampleBackgroundTask.h
...
private:
    Windows::ApplicationModel::Background::BackgroundTaskDeferral m_deferral{ nullptr };

// ExampleBackgroundTask.cpp
...
Windows::Foundation::IAsyncAction ExampleBackgroundTask::Run(
    Windows::ApplicationModel::Background::IBackgroundTaskInstance const& taskInstance)
{
    m_deferral = taskInstance.GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.
    // TODO: Modify the following line of code to call a real async function.
    co_await ExampleCoroutineAsync(); // Run returns at this point, and resumes when ExampleCoroutineAsync completes.
    m_deferral.Complete();
}
```

```cpp
void ExampleBackgroundTask::Run(IBackgroundTaskInstance^ taskInstance)
{
    m_deferral = taskInstance->GetDeferral(); // Note: defined at class scope so that we can mark it complete inside the OnCancel() callback if we choose to support cancellation.

    //
    // TODO: Modify the following line of code to call a real async function.
    //       Note that the task<void> return type applies only to async
    //       actions. If you need to call an async operation instead, replace
    //       task<void> with the correct return type.
    //
    task<void> myTask(ExampleFunctionAsync());

    myTask.then([=]() {
        m_deferral->Complete();
    });
}
```

> [!NOTE]
> В C# асинхронные методы вашей фоновой задачи можно вызвать с помощью ключевых слов **async/await**. В C++языке/CX аналогичный результат может быть достигнут с помощью цепочки задач.

Подробнее о шаблонах асинхронных операций см. в разделе [Асинхронное программирование](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-universal-windows-platform-apps). Дополнительные примеры использования задержек для предотвращения преждевременной остановки фоновой задачи см. в [примере фоновой задачи](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

Следующие действия выполняются в одном из классов вашего приложения (например, MainPage.xaml.cs).

> [!NOTE]
> Можно также создать функцию, предназначенную для регистрации фоновых задач&mdash;см. раздел [Регистрация фоновой задачи](register-a-background-task.md). В этом случае вместо выполнения следующих трех шагов можно просто создать триггер и предоставить его функции регистрации вместе с именем задачи, точкой входа задачи и (необязательно) условием.

## <a name="register-the-background-task-to-run"></a>Регистрация фоновой задачи для запуска

1.  Определите, зарегистрирована ли фоновая задача путем прохода по свойству [**баккграундтаскрегистратион. аллтаскс**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) . Это важный шаг: если приложение не проверяет, зарегистрирована ли уже фоновая задача, оно может выполнить регистрацию несколько раз, вызывая проблемы производительности и полное использование доступного задаче времени ЦП до завершения работы.

В следующем примере выполняется итерация свойства **аллтаскс** и устанавливается значение true для переменной флага, если задача уже зарегистрирована.

```csharp
var taskRegistered = false;
var exampleTaskName = "ExampleBackgroundTask";

foreach (var task in BackgroundTaskRegistration.AllTasks)
{
    if (task.Value.Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}
```

```cppwinrt
std::wstring exampleTaskName{ L"ExampleBackgroundTask" };

auto allTasks{ Windows::ApplicationModel::Background::BackgroundTaskRegistration::AllTasks() };

bool taskRegistered{ false };
for (auto const& task : allTasks)
{
    if (task.Value().Name() == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }
}

// The code in the next step goes here.
```

```cpp
boolean taskRegistered = false;
Platform::String^ exampleTaskName = "ExampleBackgroundTask";

auto iter = BackgroundTaskRegistration::AllTasks->First();
auto hascur = iter->HasCurrent;

while (hascur)
{
    auto cur = iter->Current->Value;

    if(cur->Name == exampleTaskName)
    {
        taskRegistered = true;
        break;
    }

    hascur = iter->MoveNext();
}
```

2.  Если фоновая задача еще не зарегистрирована, используйте [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder) для создания экземпляра фоновой задачи. Точка входа задачи является именем класса фоновых задач, перед которым располагается пространство имен.

Триггер фоновой задачи определяет, когда должна быть запущена фоновая задача. Список возможных триггеров см. в статье [**SystemTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType).

Например, этот код создает новую фоновую задачу и задает ее выполнение при наступлении триггера **тимезонечанжед** :

```csharp
var builder = new BackgroundTaskBuilder();

builder.Name = exampleTaskName;
builder.TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder.SetTrigger(new SystemTrigger(SystemTriggerType.TimeZoneChange, false));
```

```cppwinrt
if (!taskRegistered)
{
    Windows::ApplicationModel::Background::BackgroundTaskBuilder builder;
    builder.Name(exampleTaskName);
    builder.TaskEntryPoint(L"Tasks.ExampleBackgroundTask");
    builder.SetTrigger(Windows::ApplicationModel::Background::SystemTrigger{
        Windows::ApplicationModel::Background::SystemTriggerType::TimeZoneChange, false });
    // The code in the next step goes here.
}
```

```cpp
auto builder = ref new BackgroundTaskBuilder();

builder->Name = exampleTaskName;
builder->TaskEntryPoint = "Tasks.ExampleBackgroundTask";
builder->SetTrigger(ref new SystemTrigger(SystemTriggerType::TimeZoneChange, false));
```

3.  Вы можете добавить условие, чтобы контролировать, в какой момент времени после возникновения события триггера запустится ваша задача (не обязательно). Например, если вы не хотите, чтобы задача запускалась в отсутствие пользователя, используйте условие **UserPresent**. Список возможных условий см. в статье [**SystemConditionType**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemConditionType).

Следующий пример кода назначает условие, при котором необходимо присутствие пользователя:

```csharp
builder.AddCondition(new SystemCondition(SystemConditionType.UserPresent));
```

```cppwinrt
builder.AddCondition(Windows::ApplicationModel::Background::SystemCondition{ Windows::ApplicationModel::Background::SystemConditionType::UserPresent });
// The code in the next step goes here.
```

```cpp
builder->AddCondition(ref new SystemCondition(SystemConditionType::UserPresent));
```

4.  Зарегистрируйте фоновую задачу, вызвав метод Register в объекте [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Сохраните результат выполнения [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration), чтобы использовать его в следующем шаге.

Следующий код регистрирует фоновую задачу и сохраняет результат.

```csharp
BackgroundTaskRegistration task = builder.Register();
```

```cppwinrt
Windows::ApplicationModel::Background::BackgroundTaskRegistration task{ builder.Register() };
```

```cpp
BackgroundTaskRegistration^ task = builder->Register();
```

> [!NOTE]
> Универсальные приложения для Windows должны вызвать [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync) перед регистрацией любых типов фоновых триггеров.

Чтобы универсальное приложение для Windows продолжало корректно работать после выпуска обновления, необходимо использовать триггер **ServicingComplete** (см. раздел [SystemTriggerType](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType)), чтобы внести любые изменения в конфигурацию после обновления, такие как перенос базы данных приложения и регистрация фоновых задач. В настоящий момент рекомендуется отменить регистрацию фоновых задач, связанных с предыдущей версией приложения (см. раздел [**RemoveAccess**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.removeaccess)), и регистрировать фоновые задачи для новой версии приложения (см. раздел [**RequestAccessAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundexecutionmanager.requestaccessasync)).

Дополнительные сведения см. в разделе [Руководство по фоновым задачам](guidelines-for-background-tasks.md).

## <a name="handle-background-task-completion-using-event-handlers"></a>Обработка завершения фоновой задачи с помощью обработчиков событий

Следует зарегистрировать метод с помощью [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler), чтобы ваше приложение могло получить результаты от фоновой задачи. При запуске или возобновлении работы приложения помеченный метод будет вызываться, если фоновая задача была завершена с момента последнего времени, когда приложение находилось на переднем плане. (Метод OnCompleted будет вызван немедленно, если фоновая задача завершается во время работы приложения на переднем плане в настоящее время.)

1.  Создайте метод OnCompleted для обработки завершения фоновых задач. Например, результат фоновой задачи может быть причиной обновления пользовательского интерфейса. Представленный здесь объем памяти метода необходим для метода обработчика событий OnCompleted, даже если в этом примере не используется параметр *args*.

Следующий пример кода распознает завершение фоновой задачи и вызывает пример метода (принимающего строку сообщения) для обновления пользовательского интерфейса.

```csharp
private void OnCompleted(IBackgroundTaskRegistration task, BackgroundTaskCompletedEventArgs args)
{
    var settings = Windows.Storage.ApplicationData.Current.LocalSettings;
    var key = task.TaskId.ToString();
    var message = settings.Values[key].ToString();
    UpdateUI(message);
}
```

```cppwinrt
void UpdateUI(winrt::hstring const& message)
{
    MyTextBlock().Dispatcher().RunAsync(Windows::UI::Core::CoreDispatcherPriority::Normal, [=]()
    {
        MyTextBlock().Text(message);
    });
}

void OnCompleted(
    Windows::ApplicationModel::Background::BackgroundTaskRegistration const& sender,
    Windows::ApplicationModel::Background::BackgroundTaskCompletedEventArgs const& /* args */)
{
    // You'll previously have inserted this key into local settings.
    auto settings{ Windows::Storage::ApplicationData::Current().LocalSettings().Values() };
    auto key{ winrt::to_hstring(sender.TaskId()) };
    auto message{ winrt::unbox_value<winrt::hstring>(settings.Lookup(key)) };

    UpdateUI(message);
}
```

```cpp
void MainPage::OnCompleted(BackgroundTaskRegistration^ task, BackgroundTaskCompletedEventArgs^ args)
{
    auto settings = ApplicationData::Current->LocalSettings->Values;
    auto key = task->TaskId.ToString();
    auto message = dynamic_cast<String^>(settings->Lookup(key));
    UpdateUI(message);
}
```

> [!NOTE]
> Обновления пользовательского интерфейса должны выполняться асинхронно, чтобы избежать остановки потока пользовательского интерфейса. Пример см. в методе UpdateUI в [образце фоновой задачи](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask).

2.  Вернитесь к тому месту, где вы регистрировали фоновую задачу. После этой строки кода добавьте новый объект [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler). Предоставьте свой метод OnCompleted в качестве параметра для конструктора **BackgroundTaskCompletedEventHandler**.

Следующий пример кода добавляет [**BackgroundTaskCompletedEventHandler**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskcompletedeventhandler) в [**BackgroundTaskRegistration**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskRegistration).

```csharp
task.Completed += new BackgroundTaskCompletedEventHandler(OnCompleted);
```

```cppwinrt
task.Completed({ this, &MainPage::OnCompleted });
```

```cpp
task->Completed += ref new BackgroundTaskCompletedEventHandler(this, &MainPage::OnCompleted);
```

## <a name="declare-in-the-app-manifest-that-your-app-uses-background-tasks"></a>Объявите в манифесте приложения, что приложение использует фоновые задачи

Чтобы приложение могло выполнять фоновые задачи, вам необходимо сперва объявить каждую такую задачу в манифесте приложения. Если приложение пытается зарегистрировать фоновую задачу с триггером, который не указан в манифесте, регистрация фоновой задачи завершится ошибкой с ошибкой "класс среды выполнения не зарегистрирован".

1.  Откройте конструктор манифеста пакета, запустив файл Package.appxmanifest.
2.  Откройте вкладку **Объявления**.
3.  В раскрывающемся списке **Доступные объявления** выберите **Фоновые задачи** и щелкните **Добавить**.
4.  Установите флажок **Системное событие**.
5.  В текстовом поле **точка входа:** введите пространство имен и имя класса фонового рисунка, например Tasks. ексамплебаккграундтаск.
6.  Закройте конструктор манифестов.

Следующий элемент расширений добавляется в файл Package.appxmanifest для регистрации фоновой задачи:

```xml
<Extensions>
  <Extension Category="windows.backgroundTasks" EntryPoint="Tasks.ExampleBackgroundTask">
    <BackgroundTasks>
      <Task Type="systemEvent" />
    </BackgroundTasks>
  </Extension>
</Extensions>
```

## <a name="summary-and-next-steps"></a>Краткая сводка и дальнейшие действия

Теперь вы понимаете, как создавать класс фоновой задачи, как регистрировать фоновую задачу из приложения и как сделать так, чтобы приложение распознавало ее завершение. Вы также знаете, как обновить манифест приложения, чтобы приложение могло успешно регистрировать фоновые задачи.

> [!NOTE]
> Скачайте [пример фоновой задачи](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/BackgroundTask), чтобы увидеть похожие примеры кода в контексте полного и надежного приложения UWP, использующего фоновые задачи.

В статьях ниже можно найти справочник по API, концептуальное руководство по фоновым задачам и подробные инструкции по созданию приложений, использующих фоновые задачи.

## <a name="related-topics"></a>См. также

**Подробные разделы с инструкциями по фоновым задачам**

* [Реагирование на системные события с помощью фоновых задач](respond-to-system-events-with-background-tasks.md)
* [Регистрация фоновой задачи](register-a-background-task.md)
* [Задание условий выполнения фоновой задачи](set-conditions-for-running-a-background-task.md)
* [Использование триггера обслуживания](use-a-maintenance-trigger.md)
* [Обработка отмененной фоновой задачи](handle-a-cancelled-background-task.md)
* [Отслеживание хода выполнения и завершения фоновых задач](monitor-background-task-progress-and-completion.md)
* [Запуск фоновой задачи по таймеру](run-a-background-task-on-a-timer-.md)
* [Создание и регистрация внутрипроцессной фоновой задачи](create-and-register-an-inproc-background-task.md)
* [Преобразование незавершенного фонового задания в фоновую задачу внутри процесса](convert-out-of-process-background-task.md)  

**Руководство по фоновым задачам**

* [Руководство по работе с фоновыми задачами](guidelines-for-background-tasks.md)
* [Отладка фоновой задачи](debug-a-background-task.md)
* [Как активировать события приостановки, возобновления и фоновых событий в приложениях UWP (при отладке)](https://msdn.microsoft.com/library/windows/apps/hh974425(v=vs.110).aspx)

**Справочник по API фоновых задач**

* [**Windows. ApplicationModel. Background**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background)
