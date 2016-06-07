---
author: mcleanbyron
ms.assetid: 4e7c2388-b94e-4828-a104-14fa33f6eb2d
description: Узнайте, как использовать класс AdControl для показа баннерной рекламы в приложении на XAML для Windows 10 (UWP), Windows 8.1 или Windows Phone 8.1.
title: AdControl в XAML и .NET
---

# AdControl в XAML и .NET


\[ Обновлено для приложений UWP в Windows 10. Статьи для Windows 8.x см. в [архиве](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

В этом пошаговом руководстве рассказывается, как использовать класс [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) для показа баннерной рекламы в приложении на XAML для Windows 10 (UWP), Windows 8.1 или Windows Phone 8.1. В этом руководстве не используется **AdMediatorControl** и рекламный посредник.

Полный пример с проектом, демонстрирующим способы добавления баннерной рекламы в приложение на XAML с использованием C# и C++, см. в разделе [примеров рекламы на GitHub](http://aka.ms/githubads).

## Необходимые условия

* Установите [пакет SDK Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) для Visual Studio 2015 или Visual Studio 2013.

## Написание кода

1. В Visual Studio откройте свой проект либо создайте новый.

2. Если ваш проект направлен на работу на **Любом ЦП**, обновите его, чтобы он использовал результаты сборки, предназначенные для определенной архитектуры (например **x86**). Если ваш проект направлен на работу на **Любом ЦП**, вам не удастся надлежащим образом добавить ссылку на Microsoft Advertising в приведенных ниже шагах. Дополнительные сведения см. в разделе [Ошибки, вызванные указанием Любого ЦП как целевого в вашем проекте](known-issues-for-the-advertising-libraries.md#reference_errors).

1.  В **Обозревателе решений** щелкните правой кнопкой мыши элемент **Ссылки** и выберите **Добавить ссылку...**.

2.  В **Диспетчере ссылок** выберите одну из следующих ссылок в зависимости от типа проекта.

    -   Для проекта универсальной платформы Windows (UWP): разверните **Универсальные для Windows**, щелкните **Расширения**, а затем установите флажок **Microsoft Advertising SDK для XAML** (версия 10.0).

    -   Для проекта Windows 8.1: разверните **Windows 8.1**, нажмите **Расширения**, а затем установите флажок **Ad Mediator SDK для XAML в Windows 8.1**. Этот параметр добавляет библиотеки и Microsoft Advertising, и Ad Mediator в проект, но вы можете не обращать внимания на библиотеки Ad Mediator.

    -   Для проекта Windows Phone 8.1: разверните **Windows Phone 8.1**, нажмите **Расширения**, затем установите флажок **Ad Mediator SDK для XAML в Windows Phone 8.1**. Этот параметр добавляет библиотеки и Microsoft Advertising, и Ad Mediator в проект, но вы можете не обращать внимания на библиотеки Ad Mediator.

  ![addreferences](images/13-a84c026e-b283-44f2-8816-f950a1ef89aa.png)

    > **Примечание**  Это изображение относится к Visual Studio 2015, где создается проект UWP для Windows 10. Если выполняется сборка приложения для Windows 8.1 или Windows Phone 8.1 либо с использованием Visual Studio 2013, экран будет выглядеть иначе.

3.  В **Диспетчере ссылок** нажмите "ОК".
4.  Измените код XAML для страницы, где вы размещаете рекламу, для включения пространства имен **Microsoft.Advertising.WinRT.UI**. Например, в образце приложения по умолчанию, созданного Visual Studio (с названием MyAdFundedWindows10AppXAML в этом приложении), страницей XAML является **MainPage.xaml**.

    Раздел **Page** файла MainPage.xaml, созданного Visual Studio, содержит следующий код.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

    Добавьте ссылку на пространство имен **Microsoft.Advertising.WinRT.UI**, чтобы раздел **Page** файла MainPage.xaml содержал следующий код.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        </Grid>
    </Page>
    ```

5.  В раздел под тегом **Grid** добавьте код для **AdControl**.

    1.  Назначьте свойствам [ApplicationId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.applicationid.aspx) и [AdUnitId](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.adunitid.aspx) в разделе **Page** тестовые значения, приведенные в разделе [Значения тестового режима](test-mode-values.md).

        > **Примечание**   Перед отправкой приложения замените тестовые значения действительными.

    2.  Измените высоту и ширину элемента управления, чтобы эти параметры соответствовали одному из [поддерживаемых размеров объявлений для баннерной рекламы](supported-ad-sizes-for-banner-ads.md).

    Полностью код раздела под тегом **Grid** выглядит следующим образом.

    ``` syntax
    <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
    </Grid>
    ```

    Полный код для файла MainPage.xaml должен выглядеть так.

    ``` syntax
    <Page
        x:Class="MyAdFundedWindows10AppXAML.MainPage"
        xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MyAdFundedWindows10AppXAML"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        xmlns:UI="using:Microsoft.Advertising.WinRT.UI"
        mc:Ignorable="d">

        <Grid Background="{StaticResource ApplicationPageBackgroundThemeBrush}">

            <UI:AdControl ApplicationId="3f83fe91-d6be-434d-a0ae-7351c5a997f1"
                          AdUnitId="10865270"
                          HorizontalAlignment="Left"
                          Height="250"
                          VerticalAlignment="Top"
                          Width="300"/>
        </Grid>
    </Page>
    ```

6.  Скомпилируйте и запустите приложение, чтобы увидеть его с объявлением.

## Выпуск приложения с динамической рекламой с помощью Центра разработки для Windows


1.  На информационной панели Центра разработки для Windows, перейдите на страницу **Получение дохода**&gt;**Получение дохода с помощью рекламы** вашего приложения и [создайте автономный блок Microsoft Advertising](../publish/monetize-with-ads.md). В качестве типа рекламного блока укажите **Баннер**. Запишите и идентификатор рекламного блока, и идентификатор приложения.

2.  В своем коде замените тестовые значения (**ApplicationId** и **AdUnitId**) рекламного блока действительными значениями, сгенерированными в Центре разработки.

3.  [Отправьте приложение](../publish/app-submissions.md) в Магазин с помощью информационной панели в Центре разработки.

4.  Изучите [отчеты по показателям рекламы](../publish/advertising-performance-report.md) на информационной панели Центра разработки.

## Примечания

C#: см. [пример свойств XAML](xaml-properties-example.md), чтобы ознакомиться со способами назначения обработчиков событиям **AdControl**. Затем ознакомьтесь с примером кода [событий AdControl в C#](adcontrol-events-in-c.md), где приводятся обработчики событий, написанные C#.

Visual Basic: см. [пример свойств XAML](xaml-properties-example.md), чтобы ознакомиться со способами назначения обработчиков событиям **AdControl**.

C++: текущий выпуск библиотек Microsoft Advertising поддерживает C++. **AdControl** загружает CLR и использует управляемый C++.

Обработка ошибок. Чтобы узнать, как обрабатывать ошибки, см. раздел [обработка ошибок AdControl](adcontrol-error-handling.md).

## Ссылки по теме

* [Примеры рекламы на GitHub](http://aka.ms/githubads)

 


<!--HONumber=May16_HO2-->

