---
title: Портирование игрового цикла
description: Здесь показано, как реализовать окно для игрового приложения UWP и перенести игровой цикл, включая создание интерфейса IFrameworkView для управления полноэкранным CoreWindow.
ms.assetid: 070dd802-cb27-4672-12ba-a7f036ff495c
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, игры, портирование, игровой цикл, direct3d 9, directx 11
ms.localizationpriority: medium
ms.openlocfilehash: 9b3a18d9ee63a2ecded07f8b779195d5274b6210
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141829"
---
# <a name="port-the-game-loop"></a>Портирование игрового цикла



**Сводка**

-   [Часть 1. Инициализировать Direct3D 11](simple-port-from-direct3d-9-to-11-1-part-1--initializing-direct3d.md)
-   [Часть 2. Преобразовать платформу для отображения](simple-port-from-direct3d-9-to-11-1-part-2--rendering.md)
-   Часть 3. Портирование игрового цикла


Здесь показано, как реализовать окно для игрового приложения UWP и перенести игровой цикл, включая создание интерфейса [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) для управления полноэкранным [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). [Пошаговое руководство: портирование простого приложения Direct3D 9 на DirectX 11 и UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md), часть 3.

## <a name="create-a-window"></a>Создание окна


Чтобы создать окно рабочего стола с окном просмотра Direct3D 9, нам приходилось реализовывать традиционную оконную инфраструктуру классических приложений. В частности, нужно было создавать HWND, устанавливать размер окна, предоставлять обратный вызов обработки окон, делать окно видимым и т. д.

В приложениях UWP все гораздо проще. Вместо того чтобы создавать традиционное окно, в игре Microsoft Store на базе DirectX реализуется интерфейс [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView). Этот интерфейс предназначен для того, чтобы выполнять игры и приложения на базе DirectX непосредственно в объекте [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow) внутри контейнера приложения.

> **Примечание**    Windows предоставляет управляемые указатели на ресурсы, такие как исходный объект приложения и [ **CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). См. в разделе [ **оператор дескриптора объекта (^)** ](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx).

 

«Main» класс должен наследоваться от [ **IFrameworkView** ](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) и реализовать пять **IFrameworkView** методы: [**Инициализировать**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.initialize), [ **SetWindow**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.setwindow), [ **нагрузки**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.load), [ **запуска** ](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run), и [ **отменить инициализацию**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.uninitialize). Помимо создания интерфейса **IFrameworkView**, внутри которого будет существовать ваша игра, вам необходимо реализовать фабрику классов, которая создает экземпляр вашего **IFrameworkView**. У вашей игры по-прежнему будет исполняемый файл с методом под названием **main()** , но все, что этот метод будет делать, – создавать экземпляр **IFrameworkView** с помощью фабрики.

Функция main

```cpp
//-----------------------------------------------------------------------------
// Required method for a DirectX-only app.
// The main function is only used to initialize the app's IFrameworkView class.
//-----------------------------------------------------------------------------
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto direct3DApplicationSource = ref new Direct3DApplicationSource();
    CoreApplication::Run(direct3DApplicationSource);
    return 0;
}
```

Фабрика IFrameworkView

```cpp
//-----------------------------------------------------------------------------
// This class creates our IFrameworkView.
//-----------------------------------------------------------------------------
ref class Direct3DApplicationSource sealed : 
    Windows::ApplicationModel::Core::IFrameworkViewSource
{
public:
    virtual Windows::ApplicationModel::Core::IFrameworkView^ CreateView()
    {
        return ref new Cube11();
    };
};
```

## <a name="port-the-game-loop"></a>Портирование игрового цикла


Посмотрим на игровой цикл из нашей игры, реализованной на базе Direct3D 9. Этот код существует внутри функции main приложения. Каждая итерация игрового цикла обрабатывает сообщение от окна или преобразует кадр.

Игровой цикл в классической игре на базе Direct3D 9

```cpp
while(WM_QUIT != msg.message)
{
    // Process window events.
    // Use PeekMessage() so we can use idle time to render the scene. 
    bGotMsg = (PeekMessage(&msg, NULL, 0U, 0U, PM_REMOVE) != 0);

    if(bGotMsg)
    {
        // Translate and dispatch the message
        TranslateMessage(&msg);
        DispatchMessage(&msg);
    }
    else
    {
        // Render a new frame.
        // Render frames during idle time (when no messages are waiting).
        RenderFrame();
    }
}
```

Игровой цикл в версии нашей игры UWP устроен сходным образом, но несколько проще:

Игровой цикл выполняется внутри метода [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run), а не **main()** , потому что наша игра работает внутри класса [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView).

Вместо того чтобы реализовать инфраструктуру обработки сообщений и вызывать [**PeekMessage**](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-peekmessagea), мы можем вызвать встроенный метод [**ProcessEvents**](https://docs.microsoft.com/uwp/api/windows.ui.core.coredispatcher.processevents) класса [**CoreDispatcher**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher) окна нашего приложения. В игровом цикле не требуется ветвление для обработки сообщений – достаточно вызвать метод **ProcessEvents**.

Игровой цикл в игре Microsoft Store на базе Direct3D 11

```cpp
// UWP apps should not exit. Use app lifecycle events instead.
while (true)
{
    // Process window events.
    auto dispatcher = CoreWindow::GetForCurrentThread()->Dispatcher;
    dispatcher->ProcessEvents(CoreProcessEventsOption::ProcessAllIfPresent);

    // Render a new frame.
    RenderFrame();
}
```

Теперь у нас есть приложение UWP, которое создает ту же самую базовую графическую инфраструктуру и обрабатывает тот же цветной куб, что и пример с использованием DirectX 9.

## <a name="where-do-i-go-from-here"></a>Что дальше?


Добавьте себе в закладки раздел с [вопросами и ответами по переносу на DirectX 11](directx-porting-faq.md).

Шаблоны UWP на базе DirectX включают надежную инфраструктуру устройств Direct3D, готовую к использованию в игровых приложениях. Инструкции по выбору шаблона см. в разделе [Создание проекта игры DirectX на основе шаблона](user-interface.md).

Посетите следующие подробные статьи разработки игр Microsoft Store:

-   [Пошаговое руководство: простой игрой UWP с DirectX](tutorial--create-your-first-uwp-directx-game.md)
-   [Аудио для игр](working-with-audio-in-your-directx-game.md)
-   [Перемещение внешний вид элементов управления для игр](tutorial--adding-move-look-controls-to-your-directx-game.md)

 

 




