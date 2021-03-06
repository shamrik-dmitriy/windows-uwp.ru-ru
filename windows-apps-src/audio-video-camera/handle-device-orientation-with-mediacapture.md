---
ms.assetid: af3941c0-3508-4ba2-a79e-fc71657c605f
description: В этой статье показано, как обрабатывать ориентацию устройства при захвате фотографий и видео с помощью вспомогательного класса.
title: Обработка ориентации устройства с помощью MediaCapture
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e49bd31ff005799e1c3fc7f0bc44be8bd7c216c
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/21/2019
ms.locfileid: "67318417"
---
# <a name="handle-device-orientation-with-mediacapture"></a>Обработка ориентации устройства с помощью MediaCapture
Когда приложение захватывает фото или видео, предназначенные для просмотр за пределами приложения, например для сохранения в файл на устройстве пользователя или совместного использования в сети, важно закодировать изображение, используя правильные метаданные ориентации, чтобы при отображении в другом приложении или на другом устройстве изображение было правильно ориентировано. Определить правильную ориентацию данных для включения в файл мультимедиа — непростая задача, потому что нужно принять во внимание несколько переменных, включая ориентацию корпуса устройства, ориентацию дисплея и размещение камеры в корпусе (фронтальная или тыловая). 

Чтобы упростить работу с ориентацией изображений, рекомендуется использовать вспомогательный класс **CameraRotationHelper**, полное определение которого дано в конце этой статьи. Можно добавить этот класс в свой проект, а затем выполнить пошаговую инструкцию из этой статьи, чтобы добавить поддержку ориентации в приложение камеры. Кроме того, вспомогательный класс упрощает поворот элементов управления в пользовательском интерфейсе камеры, так что они правильно отображаются для пользователя.

> [!NOTE] 
> Эта статья основана на примерах кода и понятиях, рассматриваемых в статье [**Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). Рекомендуется ознакомиться с базовыми принципами использования класса **MediaCapture**, прежде чем добавлять поддержку ориентации в приложение.

## <a name="namespaces-used-in-this-article"></a>Пространства имен, используемые в этой статье
В примере кода из этой статьи используются API из следующих пространств имен, которые необходимо включить в код. 

[!code-cs[OrientationUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOrientationUsing)]

Первым шагом для добавления поддержки ориентации в приложение является блокировка дисплея, чтобы он не поворачивался автоматически при повороте устройства. Автоматический поворот пользовательского интерфейса подходит для большинства типов приложений, но пользователям непонятен поворот образца для предварительного просмотра с камеры. Чтобы заблокировать ориентацию дисплея, присвойте свойству [**DisplayInformation.AutoRotationPreferences**](https://docs.microsoft.com/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences) значение [**DisplayOrientations.Landscape**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayOrientations). 

[!code-cs[AutoRotationPreference](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetAutoRotationPreference)]

## <a name="tracking-the-camera-device-location"></a>Отслеживание расположения устройства камеры
Чтобы вычислить правильную ориентацию записанного мультимедиа, приложение должно определить расположение камеры на корпусе устройства. Добавьте логическую переменную-член, чтобы отследить, является ли камера внешней по отношению к устройств, например веб-камерой USB. Добавьте другую логическую переменную, чтобы отследить, требуется ли зеркальное отражение предварительного просмотра (как в случае с фронтальной камерой). Добавьте переменную для хранения объекта **DeviceInformation**, который представляет выбранную камеру.

[!code-cs[CameraDeviceLocationBools](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCameraDeviceLocationBools)]
[!code-cs[DeclareCameraDevice](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareCameraDevice)]

## <a name="select-a-camera-device-and-initialize-the-mediacapture-object"></a>Выберите камеру и инициализируйте объект MediaCapture
В статье [**Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) показано, как инициализировать объект **MediaCapture**, используя всего пару строк кода. Чтобы обеспечить поддержку ориентации камеры, добавим еще несколько шагов в процесс инициализации.

Сначала вызовем метод [**DeviceInformation.FindAllAsync**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.deviceinformation.findallasync), передав средство выбора устройств [**DeviceClass.VideoCapture**](https://docs.microsoft.com/uwp/api/Windows.Devices.Enumeration.DeviceClass), чтобы получить список всех доступных устройств видеозахвата. Затем выберите первое устройство в списке, если известно расположение панели камеры и если оно соответствует указанному значению (в данном примере это фронтальная камера). Если на нужной панели камера не обнаружена, используется первая доступная камера или камера по умолчанию.

Если устройство камеры обнаружено, создается новый объект [**MediaCaptureInitializationSettings**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings), а для выбранного устройства задается свойство [**VideoDeviceId**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videodeviceid). Затем создайте объект MediaCapture и вызовите метод [**InitializeAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.initializeasync), передав объект параметров, чтобы система использовала выбранную камеру.

Наконец, проверьте соответствует ли выбранной панели устройства значение null или «Неизвестно». Если это так, то камера является внешней по отношению к устройству, что означает, что ее поворот не связан с поворотом устройства. Если панель известна и находится в передней части корпуса устройства, мы понимаем, что требуется зеркальное отображение предварительного просмотра, поэтому настраивается отслеживание переменной.

[!code-cs[InitMediaCaptureWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitMediaCaptureWithOrientation)]
## <a name="initialize-the-camerarotationhelper-class"></a>Инициализация класса CameraRotationHelper

Итак, приступим к использованию класса **CameraRotationHelper**. Объявите переменную-член класса для хранения объекта. Вызовите конструктор, передав расположение корпуса выбранной камеры. Вспомогательный класс использует эти данные для вычисления нужной ориентации записанного мультимедиа, потока предварительного просмотра и пользовательского интерфейса. Зарегистрируйте обработчик для события **OrientationChanged** вспомогательного класса, который будет вызываться при необходимости обновления ориентации пользовательского интерфейса или потока предварительного просмотра.

[!code-cs[DeclareRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareRotationHelper)]

[!code-cs[InitRotationHelper](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetInitRotationHelper)]

## <a name="add-orientation-data-to-the-camera-preview-stream"></a>Добавление данных об ориентации в поток предварительного просмотра камеры
Добавление правильных сведений об ориентации в метаданные потока предварительного просмотра не влияет на то, как предварительный просмотр отображается пользователю, но помогает системе правильно закодировать все кадры, захватываемые из потока предварительного просмотра.

Чтобы запустить предварительный просмотр камеры, вызовите метод [**MediaCapture.StartPreviewAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Перед этим проверьте переменную-член, чтобы узнать, требуется ли зеркальное отображение предварительного просмотра (для фронтальной камеры). В этом случае задайте свойству [**FlowDirection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.frameworkelement.flowdirection) элемента [**CaptureElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) (в этом примере оно называется *PreviewControl*) значение [**FlowDirection.RightToLeft**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.FlowDirection). После запуска предварительного просмотра вызовите вспомогательный метод **SetPreviewRotationAsync**, чтобы задать поворот предварительного просмотра. Ниже показана реализация этого метода.

[!code-cs[StartPreviewWithRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewWithRotationAsync)]

Поворот предварительного просмотра задается в отдельном методе, чтобы была возможность выполнить обновление при изменении ориентации телефона, не инициализируя поток предварительного просмотра повторно. Если камера является внешней по отношению к устройству, ничего не происходит. В противном случае вызывается метод **CameraRotationHelper** **GetCameraPreviewOrientation**, который возвращает правильную ориентацию для потока предварительного просмотра. 

Чтобы задать метаданные, свойства потока предварительного просмотра извлекаются вызовом метода [**VideoDeviceController.GetMediaStreamProperties**](https://docs.microsoft.com/uwp/api/windows.media.devices.videodevicecontroller.getmediastreamproperties). Затем создайте GUID, представляющий атрибут Media Foundation Transform (MFT) для поворота видеопотока. В C++ можно использовать константу [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation), однако в C# значение GUID необходимо задать вручную. 

Добавьте значение свойства в объект свойств потока, задав GUID в качестве ключа, а поворот предварительного просмотра — в качестве значения. Это свойство принимает значения в градусах против часовой стрелки, поэтому для преобразования простого значения ориентации используется метод **CameraRotationHelper** **ConvertSimpleOrientationToClockwiseDegrees**. Наконец, вызовите метод [**SetEncodingPropertiesAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_SetEncodingPropertiesAsync_Windows_Media_Capture_MediaStreamType_Windows_Media_MediaProperties_IMediaEncodingProperties_Windows_Media_MediaProperties_MediaPropertySet_), чтобы применить к потоку новое свойство поворота.

[!code-cs[SetPreviewRotationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSetPreviewRotationAsync)]

Затем добавьте обработчик для события **CameraRotationHelper.OrientationChanged**. Это событие передает аргумент, который указывает, требуется ли поворачивать поток предварительного просмотра. Если ориентация устройства была изменена лицевой стороной вверх или лицевой стороной вниз, это значение будет равно false. Если поворачивать предварительный просмотр не требуется, вызовите ранее определенный метод **SetPreviewRotationAsync**.

Затем в обработчике событий **OrientationChanged** при необходимости обновите пользовательский интерфейс. Получите текущую рекомендуемую ориентацию пользовательского интерфейса от вспомогательного класса, вызвав метод **GetUIOrientation**, и преобразуйте значение в градусы по часовой стрелке (эта система используется для преобразований XAML). Создайте объект [**RotateTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.RotateTransform) на основе значения ориентации и настройте свойство [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform) элементов управления XAML. В зависимости от структуры пользовательского интерфейса в дополнение к обычному повороту элементов управления могут потребоваться дополнительные корректировки. Кроме того, помните, что все обновления пользовательского интерфейса должны выполняться в потоке пользовательского интерфейса, поэтому этот код нужно разместить в вызове метода [**RunAsync**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreDispatcher#Windows_UI_Core_CoreDispatcher_RunAsync_Windows_UI_Core_CoreDispatcherPriority_Windows_UI_Core_DispatchedHandler_).  

[!code-cs[HelperOrientationChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetHelperOrientationChanged)]

## <a name="capture-a-photo-with-orientation-data"></a>Захват фото с данными об ориентации
В статье [**Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md) показано, как выполнить захват фото в файл. Сначала выполняется захват в поток в памяти, а затем данные изображения считываются из потока с помощью декодера и перекодируются в файл с помощью кодировщика. Данные об ориентации, полученные из класса **CameraRotationHelper**, можно добавить в файл изображения во время перекодировки.

В следующем примере выполняется захват фото в поток [**InMemoryRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.InMemoryRandomAccessStream) с вызовом метода [**CapturePhotoToStreamAsync**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.capturephototostreamasync), а из потока создается [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder). Затем создается и открывается файл [**StorageFile**](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile), чтобы извлечь поток [**IRandomAccessStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IRandomAccessStream) для записи в файл. 

Перед перекодированием файла ориентация фото извлекается из метода **GetCameraCaptureOrientation** вспомогательного класса. Этот метод возвращает объект [**SimpleOrientation**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientation), который преобразуется в [**PhotoOrientation**](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.PhotoOrientation) с вспомогательным методом **ConvertSimpleOrientationToPhotoOrientation**. Затем создается новый объект **BitmapPropertySet** и добавляется свойство с ключом System.Photo.Orientation и значением, указывающим на ориентацию фото (значение выражается как **BitmapTypedValue**). System.Photo.Orientation — это одно из многих свойств Windows, которые можно добавить в файл изображения в качестве метаданных. Список всех связанных с фото свойств см. в разделе [**Свойства Windows — фото**](https://docs.microsoft.com/previous-versions/ff516600(v=vs.85)). Дополнительные сведения о работе с метаданными в изображениях см. в разделе [**Метаданные изображений**](image-metadata.md).

Наконец, для кодировщика задается набор свойств, который включает данные об ориентации. Для этого используется вызов метода [**SetPropertiesAsync**](https://docs.microsoft.com/windows/uwp/develop/), а для перекодировки изображения — метод [**FlushAsync**](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder.flushasync).

[!code-cs[CapturePhotoWithOrientation](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCapturePhotoWithOrientation)]

## <a name="capture-a-video-with-orientation-data"></a>Захват видео с данными об ориентации
Основы захвата видео описаны в статье [**Основные принципы фото-, аудио- и видеозахвата с помощью MediaCapture**](basic-photo-video-and-audio-capture-with-mediacapture.md). Данные об ориентации добавляются в кодировку захваченного видео с использованием той же техники, которая была описана ранее в разделе о добавлении данных об ориентации в поток предварительного просмотра.

В следующем примере создается файл, в который будет записано захваченное видео. Профиль кодировки MP4 создается с использованием статического метода [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4). Класс **CameraRotationHelper** позволяет получить данные о правильной ориентации видео вызовом метода **GetCameraCaptureOrientation**. Поскольку свойство поворота требует выражения ориентации в градусах против часовой стрелки, вспомогательный метод **ConvertSimpleOrientationToClockwiseDegrees** вызывается для преобразования значения ориентации. Затем создайте GUID, представляющий атрибут Media Foundation Transform (MFT) для поворота видеопотока. В C++ можно использовать константу [**MF_MT_VIDEO_ROTATION**](https://docs.microsoft.com/windows/desktop/medfound/mf-mt-video-rotation), однако в C# значение GUID необходимо задать вручную. Добавьте значение свойства в объект свойств потока, задав GUID в качестве ключа, а поворот — в качестве значения. Вызовите метод [**StartRecordToStorageFileAsync**](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture#Windows_Media_Capture_MediaCapture_StartRecordToStorageFileAsync_Windows_Media_MediaProperties_MediaEncodingProfile_Windows_Storage_IStorageFile_), чтобы начать запись видео, кодировка которого содержит данные об ориентации.

[!code-cs[StartRecordingWithOrientationAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartRecordingWithOrientationAsync)]

## <a name="camerarotationhelper-full-code-listing"></a>Полный текст кода для метода CameraRotationHelper
В следующем фрагменте кода представлен полный код класса **CameraRotationHelper**, который управляет датчиками ориентации оборудования, вычисляет правильные значения ориентации для фото и видео и предоставляет вспомогательные методы для преобразования разных представлений ориентации, используемых разными компонентами Windows. Если следовать инструкциям в статье выше, можно добавить этот класс свой проект «как есть», не внося никаких изменений. Конечно, в код можно внести изменения в соответствии с конкретными потребностями.

В этом вспомогательном классе датчик [**SimpleOrientationSensor**](https://docs.microsoft.com/uwp/api/Windows.Devices.Sensors.SimpleOrientationSensor) устройства используется, чтобы определить текущую ориентацию корпуса устройства, а класс [**DisplayInformation**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Display.DisplayInformation) — чтобы определить текущую ориентацию дисплея. Каждый из этих классов предоставляет события, вызываемые при изменении текущей ориентации. Панель, на которой установлено устройство захвата (фронтальная, тыловая или внешняя), используется, чтобы установить, требуется ли зеркальное отражение потока предварительного просмотра. Кром того, свойство [**EnclosureLocation.RotationAngleInDegreesClockwise**](https://docs.microsoft.com/uwp/api/windows.devices.enumeration.enclosurelocation.rotationangleindegreesclockwise), поддерживаемое некоторыми устройствами, используется для определения ориентации установленной камеры в корпусе.

Чтобы получить рекомендованные значения ориентации для указанных задач приложения камеры, можно использовать следующие методы.

* **GetUIOrientation**: возвращает рекомендуемую ориентацию элементов пользовательского интерфейса камеры.
* **GetCameraCaptureOrientation**: возвращает рекомендуемую ориентацию для внесения в метаданные изображения в качестве кода.
* **GetCameraPreviewOrientation**: возвращает рекомендуемую ориентацию для потока предварительного просмотра, чтобы сделать пользовательский интерфейс более естественным.

[!code-cs[CameraRotationHelperFull](./code/SimpleCameraPreview_Win10/cs/CameraRotationHelper.cs#SnippetCameraRotationHelperFull)]



## <a name="related-topics"></a>См. также

* [Камера](camera.md)
* [Основные фото, видео и аудио захвата с MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




