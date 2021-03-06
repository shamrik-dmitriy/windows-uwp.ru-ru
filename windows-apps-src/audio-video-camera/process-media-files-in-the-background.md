---
ms.assetid: B5E3A66D-0453-4D95-A3DB-8E650540A300
description: В данной статье показано, как использовать MediaProcessingTrigger и фоновую задачу для обработки файлов мультимедиа в фоновом режиме.
title: Обработка файлов мультимедиа в фоновом режиме
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 992648c8c90a8ad62b772d417b2b1beeb6087c53
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318529"
---
# <a name="process-media-files-in-the-background"></a>Обработка файлов мультимедиа в фоновом режиме



В данной статье показано, как использовать [**MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) и фоновую задачу для обработки файлов мультимедиа в фоновом режиме.

Пример приложения, описанный в этой статье, позволяет пользователю выбрать входной файл мультимедиа для перекодирования и определить выходной файл для результата перекодирования. Затем запускается фоновая задача для выполнения операции перекодирования. [  **MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) предназначен для поддержки многих других сценариев обработки мультимедиа, кроме перекодирования, в том числе записи мультимедийных композиций на диск и передачи обработанных файлов мультимедиа после завершения обработки.

Дополнительные сведения о различных функциях универсального приложения для Windows, применяемых в этом примере, см. в следующих статьях:

-   [Перекодирование файлов мультимедиа](transcode-media-files.md)
-   [Запуск возобновления и фоновых задачах](https://docs.microsoft.com/windows/uwp/launch-resume/index)
-   [Значки плитки и уведомления](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-badges-notifications)

## <a name="create-a-media-processing-background-task"></a>Создание фоновой задачи обработки мультимедиа

Для добавления фоновой задачи в существующее решение в Microsoft Visual Studio введите имя вашего компьютера.

1.  В меню **Файл** выберите команду **Добавить** и затем **Новый проект...** .
2.  Выберите тип проекта **Компонент среды выполнения Windows (универсальные приложения)** .
3.  Введите имя вашего нового проекта компонента. В этом примере используется имя проекта **MediaProcessingBackgroundTask**.
4.  Нажмите кнопку "ОК".

В **обозревателе решений**щелкните правой кнопкой мыши значок созданного по умолчанию файла Class1.cs и выберите пункт **Переименовать**. Переименуйте файл в MediaProcessingTask.cs. Когда Visual Studio запросит, требуется ли переименовать все ссылки на этот класс, выберите **Да**.

В переименованном файле класса добавьте следующие директивы **using**, чтобы включить эти пространства имен в проект.
                                  
[!code-cs[BackgroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundUsing)]

Обновите свое объявление класса, чтобы класс наследовал от [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask).

[!code-cs[BackgroundClass](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundClass)]

Добавьте следующие переменные-члены в класс.

-   [  **IBackgroundTaskInstance**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTaskInstance), который будет использоваться для обновления сведений о ходе выполнения фоновой задачи в приложении переднего плана.
-   [  **BackgroundTaskDeferral**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskDeferral), который не позволяет системе завершить фоновую задачу при выполнении перекодирования мультимедиа в асинхронном режиме.
-   Объект **CancellationTokenSource**, который можно использовать для отмены асинхронной операции перекодирования.
-   Объект [**MediaTranscoder**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.MediaTranscoder), который будет использоваться для перекодирования файлов мультимедиа.

[!code-cs[BackgroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetBackgroundMembers)]

Система вызывает метод [**Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtask.run) фоновой задачи, когда эта задача запускается. Задайте для объекта [**IBackgroundTask**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.IBackgroundTask), передаваемого в метод, соответствующую переменную-член. Зарегистрируйте обработчик для события [**Canceled**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled), которое возникает, когда системе нужно завершить работу фоновой задачи. После этого установите для свойства [**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.progress) нулевое значение.

Затем вызовите метод объекта [**GetDeferral**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.getdeferral) фоновой задачи для получения отсроченного объекта. Это сообщает системе, что завершать задачу не нужно, поскольку выполняются асинхронные операции.

Затем вызовите вспомогательный метод **TranscodeFileAsync**, который определяется в следующем разделе. Если это успешно выполнено, вызывается вспомогательный метод, чтобы открыть всплывающее уведомление для оповещения пользователя о завершении перекодирования.

В конце метода **Run** вызовите [**Complete**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskdeferral.complete) для отсроченного объекта, чтобы сообщить системе, что фоновая задача выполнена и может быть завершена.

[!code-cs[Run](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetRun)]

Во вспомогательном методе **TranscodeFileAsync** имена входного и выходного файлов для операций перекодирования извлекаются из [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) для приложения. Эти значения устанавливаются вашим приложением переднего плана. Создайте объект [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile) для входного и выходного файлов, а затем создайте профиль кодирования для использования во время перекодирования.

Вызовите [**PrepareFileTranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.mediatranscoder.preparefiletranscodeasync), передав входной файл, выходной файл и профиль кодирования. Объект [**PrepareTranscodeResult**](https://docs.microsoft.com/uwp/api/Windows.Media.Transcoding.PrepareTranscodeResult), возвращенный из этого вызова, позволяет узнать, можно ли выполнить перекодирование. Если свойство [**CanTranscode**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.cantranscode) имеет значение true, вызовите [**TranscodeAsync**](https://docs.microsoft.com/uwp/api/windows.media.transcoding.preparetranscoderesult.transcodeasync) для выполнения операции перекодирования.

Метод **AsTask** позволяет отслеживать ход выполнения асинхронной операции или отменить ее. Создайте новый объект **Progress**, указав нужные единицы хода выполнения и имя метода, который вызывается, чтобы уведомлять вас о ходе выполнения задачи. Передайте объект **Progress** в метод **AsTask** вместе с токеном отмены, который позволяет отменить задачу.

[!code-cs[TranscodeFileAsync](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetTranscodeFileAsync)]

В методе, использовавшемся для создания объекта Progress в предыдущем шаге **Progress**, задайте ход выполнения экземпляра фоновой задачи. Это позволит передать данные о ходе выполнения в приложение переднего плана, если оно запущено.

[!code-cs[Progress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetProgress)]

Вспомогательный метод **SendToastNotification** создает новое всплывающее уведомление, получая для него XML-документ шаблона, который содержит только текстовое содержимое. Задается текстовый элемент XML-кода всплывающего уведомления, а затем создается новый объект [**ToastNotification**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.ToastNotification) из XML-документа. И наконец, всплывающее уведомление отображается пользователю путем вызова метода [**ToastNotifier.Show**](https://docs.microsoft.com/uwp/api/windows.ui.notifications.toastnotifier.show).

[!code-cs[SendToastNotification](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetSendToastNotification)]

В обработчике события [**Canceled**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskinstance.canceled), который вызывается, когда система отменяет фоновую задачу, можно записать ошибку в журнал в целях телеметрии.

[!code-cs[OnCanceled](./code/MediaProcessingTriggerWin10/cs/MediaProcessingBackgroundTask/MediaProcessingTask.cs#SnippetOnCanceled)]

## <a name="register-and-launch-the-background-task"></a>Регистрация и запуск фоновой задачи

Прежде чем из приложения переднего плана можно будет запустить фоновую задачу, необходимо обновить файл Package.appmanifest приложения переднего плана, чтобы сообщить системе, что приложение использует фоновую задачу.

1.  Дважды щелкните значок файла Package.appmanifest в **обозревателе решений**, чтобы открыть редактор манифестов.
2.  Выберите вкладку **Объявления**.
3.  В списке **Доступные объявления** выберите **Фоновые задачи** и щелкните **Добавить**.
4.  Убедитесь, что в области **Поддерживаемые объявления** выбран элемент **Фоновые задачи**. В области **Свойства** установите флажок **Обработка мультимедиа**.
5.  В текстовом поле **Точка входа** укажите пространство имен и имя класса для фонового теста, разделив их запятой. В этом примере данная запись имеет следующий вид:
   ```csharp
   MediaProcessingBackgroundTask.MediaProcessingTask
   ```
Далее необходимо добавить ссылку на фоновую задачу в приложение переднего плана.
1.  В области вашего проекта приложения переднего плана в **обозревателе решений** щелкните правой кнопкой мыши папку **Ссылки** и выберите пункт **Добавить ссылку...** .
2.  Разверните узел **Проекты** и выберите **Решение**.
3.  Установите флажок рядом с проектом фоновой задачи и нажмите кнопку **ОК**.

Оставшуюся часть кода в этом примере следует добавить в ваше приложение переднего плана. Для начала необходимо добавить в проект следующие пространства имен.

[!code-cs[ForegroundUsing](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundUsing)]

Далее добавьте следующие переменные-члены, необходимые для регистрации фоновой задачи.

[!code-cs[ForegroundMembers](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetForegroundMembers)]

Вспомогательный метод **PickFilesToTranscode** использует [**FileOpenPicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileOpenPicker) и [**FileSavePicker**](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker), чтобы открыть входной и выходной файлы для перекодирования. Пользователь может выбрать файлы в недоступном для приложения расположении. Чтобы убедиться, что ваша фоновая задача может открыть такие файлы, добавьте их в [**FutureAccessList**](https://docs.microsoft.com/uwp/api/windows.storage.accesscache.storageapplicationpermissions.futureaccesslist) для своего приложения.

В заключение задайте записи для имен входного и выходного файлов в [**LocalSettings**](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.localsettings) для приложения. Фоновая задача извлекает имена файлов из этого расположения.

[!code-cs[PickFilesToTranscode](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetPickFilesToTranscode)]

Для регистрации фоновой задачи создайте новый [**MediaProcessingTrigger**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTrigger) и новый [**BackgroundTaskBuilder**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.BackgroundTaskBuilder). Задайте имя построителя фоновой задачи, чтобы позднее его можно было идентифицировать. Задайте для [**TaskEntryPoint**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.taskentrypoint) ту же строку пространства имен и имени класса, которую использовали в файле манифеста. Задайте для свойства [**Trigger**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.trigger) значение экземпляра **MediaProcessingTrigger**.

Перед регистрацией задачи убедитесь, что отменена регистрация всех ранее зарегистрированных задач, пройдя в цикле по коллекции [**AllTasks**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.alltasks) и вызывая [**Unregister**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.unregister) для всех задач с указанным в свойстве [**BackgroundTaskBuilder.Name**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.name) именем.

Зарегистрируйте фоновую задачу, вызвав [**Register**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskbuilder.register). Зарегистрируйте обработчики для событий [**Completed**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.backgroundtaskregistration.completed) и [**Progress**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.ibackgroundtaskregistration.progress).

[!code-cs[RegisterBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetRegisterBackgroundTask)]

Во время работы приложения первоначально запущенный, например в типичном приложении зарегистрирует их фоновой задачи **OnNavigatedTo** событий.

Запустите фоновую задачу, вызвав метод **MediaProcessingTrigger** объекта [**RequestAsync**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.background.mediaprocessingtrigger.requestasync). Объект [**MediaProcessingTriggerResult**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.MediaProcessingTriggerResult), возвращенный этим методом, позволяет узнать, была ли фоновая задача успешно запущена, а также определить причину, если эта фоновая задача не была запущена. 

[!code-cs[LaunchBackgroundTask](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetLaunchBackgroundTask)]

Типичные приложения будет запущена в фоновой задачи в ответ на взаимодействие с пользователем, такие как **щелкните** события элемента управления пользовательского интерфейса.

Обработчик событий **OnProgress** вызывается каждый раз, когда фоновая задача обновляет данные о ходе выполнения операции. Вы можете использовать эту возможность, чтобы обновить пользовательский интерфейс данными о ходе выполнения.

[!code-cs[OnProgress](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnProgress)]

Обработчик событий **OnCompleted** вызывается каждый раз, когда фоновая задача завершает работу. Это еще одна возможность обновить пользовательский интерфейс для предоставления сведений о состоянии пользователю.

[!code-cs[OnCompleted](./code/MediaProcessingTriggerWin10/cs/MediaProcessingTriggerWin10/MainPage.xaml.cs#SnippetOnCompleted)]


 

 




