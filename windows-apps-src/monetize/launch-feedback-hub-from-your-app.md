---
Description: Вы можете подтолкнуть пользователей к тому, чтобы оставить отзыв, запустив Центр отзывов из вашего приложения.
title: Запуск Центра отзывов из приложения
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, центр отзывов, запуск
ms.localizationpriority: medium
ms.openlocfilehash: 589cad0c5055f0bfbade457e035702ee2cffd43d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259249"
---
# <a name="launch-feedback-hub-from-your-app"></a>Запуск Центра отзывов из приложения

Вы можете подтолкнуть пользователей к тому, чтобы оставить отзыв, добавив элемент управления (например, кнопку) в приложение универсальной платофрмы Windows (UWP), который открывает Центр отзывов. Центр отзывов — это предустановленное приложение, в котором собираются отзывы о Windows и установленных приложениях. Все отзывы клиентов, отправленные для вашего приложения через центр обратной связи, собираются и отображаются в [отчете о отзывах](../publish/feedback-report.md) в центре партнеров, что позволяет просматривать проблемы, предложения и отзывы, отправленные клиентами в одном отчете.

Чтобы запустить Центр отзывов из вашего приложения, используйте API-интерфейс в пакете [Microsoft Store Services SDK](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftStoreServicesSDK). Мы рекомендуем применять этот API для запуска Центра отзывов из элемента интерфейса в приложении, который соответствует нашим рекомендациям по оформлению.

> [!NOTE]
> Центр отзывов доступен только на устройствах под управлением версии 10.0.14271 или более поздней версии ОС Windows 10, основанной на настольном и мобильном [семействах устройств](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide). Мы рекомендуем показывать элемент управления отзывами в приложении, только если Центр отзывов доступен на устройстве пользователя. В этом разделе представлен пример кода, позволяющий реализовать это.

## <a name="how-to-launch-feedback-hub-from-your-app"></a>Запуск Центра отзывов из приложения

Запуск Центра отзывов из приложения:

1. [Установите пакет Microsoft Store Services SDK](microsoft-store-services-sdk.md#install-the-sdk).
2. Откройте проект в Visual Studio.
3. В обозревателе решений щелкните правой кнопкой мыши узел **Ссылки** вашего проекта и выберите команду **Добавить ссылку**.
4. В диалоговом окне **Диспетчер ссылок** разверните список **Универсальная платформа Windows** и выберите **Расширения**.
5. В списке пакетов SDK установите флажок рядом с пунктом **Microsoft Engagement Framework** и нажмите кнопку **ОК**.
6. Добавьте в проект элемент управления, который позволит пользователям запустить Центр отзывов, например кнопку. Мы рекомендуем настроить этот элемент управления следующим образом.
  * Выберите для содержимого элемента управления шрифт **Segoe MDL2 Assets**.
  * Добавьте в текст элемента управления шестнадцатеричный код символа Юникода E939. Это код символа рекомендуемого значка отзыва в шрифте **Segoe MDL2 Assets**.
  * Сделайте элемент управления скрытым.
    > [!NOTE]
    > Мы рекомендуем скрыть элемент управления отзывами по умолчанию и показывать его в коде инициализации, только если Центр отзывов доступен на устройстве пользователя. Далее показано, как это сделать.

    В следующем примере кода демонстрируется XAML-определение [кнопки](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button), которая настроена, как показано выше.

    ```XML
    <Button x:Name="feedbackButton" FontFamily="Segoe MDL2 Assets" Content="&#xE939;" HorizontalAlignment="Left" Margin="138,352,0,0" VerticalAlignment="Top" Visibility="Collapsed"  Click="feedbackButton_Click"/>
    ```

7. В коде инициализации страницы приложения, на которой размещен элемент управления отзывами, используйте статический метод [IsSupported](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.issupported) класса [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher), чтобы определить, доступен ли Центр отзывов на устройстве пользователя. Центр отзывов доступен только на устройствах под управлением версии 10.0.14271 или более поздней версии ОС Windows 10, основанной на настольном и мобильном [семействах устройств](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

    Если это свойство возвращает значение **true**, сделайте элемент управления видимым. В следующем примере кода показано, как это сделать для [кнопки](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.button).

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#ToggleFeedbackVisibility)]
      > [!NOTE]
      > Хотя в настоящее время Центр отзывов не поддерживается на устройствах Xbox, свойство **IsSupported** возвращает значение **true** на устройствах Xbox под управлением Windows 10 версии 10.0.14271 или более поздней. Это известная проблема, которая будет устранена в следующей версии пакета Microsoft Store Services SDK.  

8. В обработчике событий, который запускается, когда пользователь щелкает элемент управления, получите объект [StoreServicesFeedbackLauncher](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher) и вызовите метод [LaunchAsync](https://docs.microsoft.com/uwp/api/microsoft.services.store.engagement.storeservicesfeedbacklauncher.launchasync), чтобы открыть приложение Центр отзывов. У этого метода есть две перегруженные версии: одна без параметров и другая версия, которая принимает словарь пар «ключ-значение», содержащий метаданные, которые нужно связать с отзывом. В следующем примере показано, как запустить Центр отзывов в обработчике событий [Click](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.primitives.buttonbase.click) элемента управления [Button](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button).

    [!code-csharp[LaunchFeedback](./code/StoreSDKSamples/cs/FeedbackPage.xaml.cs#FeedbackButtonClick)]

## <a name="design-recommendations-for-your-feedback-ui"></a>Рекомендации по оформлению пользовательского интерфейса отзывов

Для запуска Центра отзывов мы рекомендуем добавить в приложение элемент пользовательского интерфейса (например, кнопку), который отображает следующий стандартный значок отзыва шрифта Segoe MDL2 Assets и символ с кодом E939.

![Значок отзыва](images/feedback_icon.PNG)

Мы также рекомендуем использовать один или несколько следующих вариантов размещения для привязки Центра отзывов в вашем приложении.
* **Непосредственно на панели приложения**. В зависимости от реализации вы можете использовать только значок или можете добавить текст (как показано ниже).

  ![Значок отзыва](images/feedback_appbar_placement.png)

* **В параметрах приложения**. Это более тонкий способ предоставить доступ к Центру отзывов. В примере ниже ссылка на отзыв отображается как одна из ссылок в приложении.

  ![Значок отзыва](images/feedback_settings_placement.png)

* **Во всплывающем элементе на основе событий**. Это полезно, если вам требуется задать пользователям вопрос перед запуском Центра отзывов о Windows. Например, после использования определенной функции в приложении вы можете задать вопрос о ней. Если пользователь решает ответить на него, приложение запускает Центр отзывов.


## <a name="related-topics"></a>См. также

* [Отчет об отзывах](../publish/feedback-report.md)
