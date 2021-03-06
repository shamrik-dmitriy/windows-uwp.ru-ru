---
ms.assetid: DE5B084C-DAC1-430B-A15B-5B3D5FB698F7
title: Оптимизация анимаций, мультимедиа и изображений
description: Создавайте приложения универсальной платформы Windows (UWP) с плавными анимациями, высокой частотой кадров, захватом и воспроизведением высокопроизводительных файлов мультимедиа.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 579772bba55c93de38c3c43538ad14253dbc2572
ms.sourcegitcommit: a20457776064c95a74804f519993f36b87df911e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/27/2019
ms.locfileid: "71339895"
---
# <a name="optimize-animations-media-and-images"></a>Оптимизация анимаций, мультимедиа и изображений


Создавайте приложения универсальной платформы Windows (UWP) с плавными анимациями, высокой частотой кадров, захватом и воспроизведением высокопроизводительных файлов мультимедиа.

## <a name="make-animations-smooth"></a>Создание плавных анимаций

Ключевой аспект приложений UWP — это плавные взаимодействия. К ним относятся манипуляции с помощью касаний, при которых элементы "прилипают к пальцу", плавные переходы и анимации, а также небольшие движения, обеспечивающие отклик на ввод данных. В платформе XAML есть поток под названием "поток компоновки", предназначенный для компоновки и анимации визуальных элементов приложения. Так как поток компоновки отделен от потока пользовательского интерфейса, в котором выполняется платформа и код разработчиков, приложения могут достичь согласованной частоты кадров и плавности анимаций независимо от сложности этапов разметки или расширенных вычислений. В этом разделе демонстрируется использование потока компоновки для обеспечения плавности анимаций в приложении. Дополнительную информацию об анимациях см. в статье [Обзор анимаций](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Дополнительную информацию об улучшении скорости отклика приложения во время интенсивных вычислений см. в статье [Поддержка скорости отклика потока пользовательского интерфейса](keep-the-ui-thread-responsive.md).

### <a name="use-independent-instead-of-dependent-animations"></a>Используйте независимые анимации вместо зависимых

Независимые анимации можно вычислить от начала до конца во время их создания, так как изменения анимируемого свойства не влияют на остальные объекты сцены. Независимые анимации могут выполняться в потоке компоновки, а не в потоке пользовательского интерфейса. Это гарантирует сохранение их плавности, так как поток компоновки регулярно согласованно обновляется.

Перечисленные ниже типы анимаций гарантировано независимы.

-   Анимации объектов с использованием ключевых кадров
-   Анимации нулевой продолжительности
-   Анимации для свойств [**Canvas.Left**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.left) и [**Canvas.Top**](https://docs.microsoft.com/dotnet/api/system.windows.controls.canvas.top)
-   Анимации для свойства [**UIElement.Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity)
-   Анимация для свойств типа [**Brush**](/uwp/api/Windows.UI.Xaml.Media.Brush) при нацеливании на подсвойство [**SolidColorBrush.Color**](/uwp/api/Windows.UI.Xaml.Media.SolidColorBrush.Color)
-   Анимации для следующих свойств [**UIElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.UIElement) при нацеливании на подсвойства данных типов возвращаемых значений:

    -   [**RenderTransform**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.rendertransform)
    -   [**Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d)
    -   [**Projection**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.projection)
    -   [**Clip**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.clip)

Зависимые анимации влияют на компоновку, которую поэтому нельзя вычислить без дополнительного ввода данных из потока пользовательского интерфейса. Зависимые анимации включают изменения таких свойств, как [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) и [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height). По умолчанию зависимые анимации не выполняются, для этого требуется вмешательство разработчика приложения. Включенные анимации выполняются плавно, если поток пользовательского интерфейса не заблокирован, однако если платформа и приложение выполняют много другой работы в потоке пользовательского интерфейса, могут начаться перебои.

Почти все анимации в платформе XAML независимы по умолчанию, но такую оптимизацию можно свести на нет некоторыми действиями. В частности, остерегайтесь следующих сценариев:

-   Задание свойства [**EnableDependentAnimation**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.pointanimation.enabledependentanimation), чтобы позволить зависимым анимациям выполняться в потоке пользовательского интерфейса. Преобразуйте такие анимации в независимые версии. Например, анимируйте [**ScaleTransform.ScaleX**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scalex) и [**ScaleTransform.ScaleY**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.scaletransform.scaley) вместо [**Width**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Width) и [**Height**](/uwp/api/Windows.UI.Xaml.FrameworkElement.Height) объекта. Не бойтесь масштабировать такие объекты, как изображения и текст. Платформа применяет билинейное масштабирование только во время анимации [**ScaleTransform**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.ScaleTransform). Изображение и текст повторно преобразовываются в растровый формат при конечном размере, чтобы гарантировать их чистоту.
-   Создание обновлений для каждого кадра, что фактически соответствует зависимым анимациям. Пример этого — применение преобразований в обработчике события [**CompositonTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering).
-   Выполнение любой анимации, считающейся независимой, в элементе, в котором свойству [**CacheMode**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.cachemode) присвоено значение **BitmapCache**. Такая анимация считается зависимой, так как кэш необходимо повторно преобразовать в растровый формат для каждого кадра.

### <a name="dont-animate-a-webview-or-mediaplayerelement"></a>Не анимируйте WebView или MediaPlayerElement

Веб-содержимое в элементе управления [**WebView**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.WebView) не обрабатывается напрямую платформой XAML. Требуется дополнительная работа, чтобы включить его в композицию остальной сцены. Эти дополнительные усилия прикладываются при анимации элемента управления на экране. Потенциально они могут привести к появлению проблем синхронизации (например, HTML-содержимое может двигаться несинхронно с остальным содержимым XAML на странице). Если необходимо анимировать элемент управления **WebView**, замените его элементом [**WebViewBrush**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.webviewbrush) на время анимации.

Анимирование [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) — тоже не очень хорошая идея. Это может отрицательно повлиять на производительность, а также привести к появлению разрывов или других артефактов в воспроизводимом видео.

> **Примечание.**   Рекомендации в этой статье для **MediaPlayerElement** также применимы к [**MediaElement**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement). **MediaPlayerElement** доступен только в Windows 10 версии 1607, поэтому при создании приложения для более ранней версии Windows следует использовать **MediaElement**.

### <a name="use-infinite-animations-sparingly"></a>Используйте бесконечно повторяемые анимации как можно реже

Большинство анимаций выполняется в течение определенного времени, но если параметру [**Timeline.Duration**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.animation.timeline.duration) задать значение Forever, анимация может выполняться неопределенно долго. Рекомендуется свести к минимуму применение бесконечно повторяемых анимаций, поскольку они постоянно потребляют ресурсы ЦП и могут помешать переходу процессора в режим пониженного энергопотребления или в состояние простоя, ускоряя разрядку аккумулятора.

Добавление обработчика события [**CompositionTarget.Rendering**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.compositiontarget.rendering) аналогично выполнению бесконечно повторяемой анимации. Обычно поток пользовательского интерфейса активен, когда есть работа. Однако добавление обработчика этого события вынуждает его запускаться для каждого кадра. Удаляйте этот обработчик, когда нет работы, и регистрируйте повторно, когда в нем снова возникает необходимость.

### <a name="use-the-animation-library"></a>Используйте библиотеку анимации

Пространство имен [**Windows.UI.Xaml.Media.Animation**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Animation) содержит библиотеку высокопроизводительных плавных анимаций, которые имеют внешний вид, согласованный с другими анимациями Windows. В именах соответствующих классов присутствует слово "Theme". Они описаны в статье [Обзор анимаций](https://docs.microsoft.com/windows/uwp/graphics/animations-overview). Эта библиотека поддерживает многие распространенные сценарии анимации (например, анимацию первого представления приложения, а также создание переходов состояний и содержимого). Мы рекомендуем использовать эту библиотеку анимации везде, где это возможно, чтобы повысить производительность и согласованность для пользовательского интерфейса UWP.

> **Примечание.**   Не все свойства можно анимировать с помощью библиотеки анимации. Сценарии XAML, в которых библиотека анимации не применяется, см. в статье [Раскадрованные анимации](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations).


### <a name="animate-compositetransform3d-properties-independently"></a>Независимая анимация свойств CompositeTransform3D

Можно анимировать каждое свойство [**CompositeTransform3D**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Media3D.CompositeTransform3D) независимо, поэтому применяйте только те анимации, которые вам нужны. Примеры и дополнительные сведения см. в разделе [**UIElement.Transform3D**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.transform3d). Дополнительные сведения о преобразованиях анимации см. в разделах [Раскадрованные анимации](https://docs.microsoft.com/windows/uwp/graphics/storyboarded-animations) и [Анимации по ключевым кадрам и на основе функций для реалистичной анимации](https://docs.microsoft.com/windows/uwp/graphics/key-frame-and-easing-function-animations).

## <a name="optimize-media-resources"></a>Оптимизация ресурсов мультимедиа

Звук, видео и изображения являются привлекательными формами содержимого и используются в большинстве приложений. По мере увеличения скорости захвата мультимедиа и перехода четкости содержимого от стандартной к высокой увеличивается потребность в ресурсах, необходимых для хранения, декодирования и воспроизведения этого содержимого. Платформа XAML выполняет сборку на основе последних возможностей, добавленных в модули мультимедиа UWP, поэтому эти улучшения появятся в приложениях бесплатно. Здесь рассказано о некоторых дополнительных приемах, с помощью которых вы сможете максимально эффективно использовать мультимедиа в вашем приложении UWP.

### <a name="release-media-streams"></a>Освобождение потоков мультимедиа

Файлы мультимедиа — это наиболее распространенные и дорогие ресурсы, используемые приложениями. Поскольку ресурсы файла мультимедиа могут значительно увеличить размер памяти, занимаемой приложением, необходимо помнить об освобождении дескриптора мультимедиа, как только приложение завершит его использование.

Например, если ваше приложение работает с классом [**RandomAccessStream**](/uwp/api/Windows.Storage.Streams.RandomAccessStream) или объектом [**IInputStream**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.IInputStream), обязательно вызовите метод Close для объекта после того, как приложение завершило его использование, чтобы освободить базовый объект.

### <a name="display-full-screen-video-playback-when-possible"></a>Отображение полноэкранного воспроизведения видео, когда это возможно

В приложениях UWP всегда используйте свойство [**IsFullWindow**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.isfullwindow) в [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) для включения и отключения полнооконной прорисовки. Это гарантирует оптимизацию на уровне системы при воспроизведении файлов мультимедиа.

Платформа XAML может оптимизировать отображение видео в случае, когда обрабатывается только оно, в результате чего потребляется меньше электроэнергии и кадры выдаются с более высокой частотой. Для максимальной оптимизации воспроизведения мультимедиа установите размер объекта **MediaPlayerElement** равным ширине и высоте экрана и не отображайте другие элементы XAML.

Для наложения элементов XAML на **MediaPlayerElement** в полноэкранном режиме могут быть веские причины, например реализация скрытых субтитров или кратковременных элементов управления воспроизведением. Эти элементы необходимо скрыть (задать свойство `Visibility="Collapsed"`), когда они не нужны, чтобы вернуть воспроизведение медиафайла в максимально эффективное состояние.

### <a name="display-deactivation-and-conserving-power"></a>Отключение дисплея и экономия энергии

Чтобы предотвратить отключение дисплея, когда пользователь не работает (например, при воспроизведении видео), можно вызвать [**DisplayRequest.RequestActive**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestactive).

Чтобы не подавать запросы отображения, если это больше не требуется, а также для экономии энергии и уровня заряда батареи необходимо вызвать [**DisplayRequest.RequestRelease**](https://docs.microsoft.com/uwp/api/windows.system.display.displayrequest.requestrelease).

Вот некоторые из ситуаций, при которых необходимо высвобождать запросы отображения:

-   Воспроизведение видео приостановлено. Например, действием пользователя, буферизацией или из-за ограниченной пропускной способности сети.
-   Воспроизведение остановлено. Например, файл видео закончился или презентация завершена.
-   Произошла ошибка воспроизведения. Например, из-за проблем подключения к сети или поврежденного файла.

### <a name="put-other-elements-to-the-side-of-embedded-video"></a>Размещение прочих элементов по краю встроенного видео

В приложениях часто используется встроенный просмотр видео, когда оно воспроизводится на странице. Очевидно, что в этом случае полноэкранная оптимизация становится недоступной, так как размер [**MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) не совпадает с размером страницы и присутствуют другие объекты XAML. Постарайтесь случайно не войти в этот режим, нарисовав границу вокруг **MediaPlayerElement**.

Не рисуйте элементы XAML поверх встроенного видео. Если вы это сделаете, платформа должна будет провести дополнительную работу, чтобы составить сцену. Размещение элементов управления воспроизведением под встроенным элементом мультимедиа, а не над ним, является в данном случае удачным вариантом оптимизации. Красная полоса на этом рисунке обозначает набор элементов управления воспроизведением (воспроизведение, приостановка, остановка и т. д.).

![MediaPlayerElement с наложенными элементами](images/videowithoverlay.png)

Не размещайте эти элементы управления поверх мультимедиа, не использующего полноэкранный режим. Лучше разместите элементы управления воспроизведением вне области обработки мультимедиа. На следующем рисунке эти элементы управления размещены под медиафайлом.

![MediaPlayerElement с элементами, расположенными рядом](images/videowithneighbors.png)

### <a name="delay-setting-the-source-for-a-mediaplayerelement"></a>Откладывание установки источника MediaPlayerElement

Модули мультимедиа представляют собой крупные объекты, и платформа XAML откладывает загрузку DLL и создание больших объектов на максимально возможное время. [  **MediaPlayerElement**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement) вынужден выполнять эту работу, после того как его источник задан с помощью свойства [**Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.source). Его установка в момент, когда пользователь готов воспроизвести мультимедиа, откладывает большинство затрат, связанных с **MediaPlayerElement**, на максимально возможное время.

### <a name="set-mediaplayerelementpostersource"></a>Установка MediaPlayerElement.PosterSource

Установка [**MediaPlayerElement.PosterSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.postersource) позволяет XAML освободить некоторые ресурсы GPU, которые в противном случае были бы заняты. Этот API позволяет приложению использовать минимальный возможный объем памяти.

### <a name="improve-media-scrubbing"></a>Усовершенствование протаскивания мультимедиа

Реализация эффективно работающего протаскивания для платформ мультимедиа — всегда непростая задача. Обычно это делается путем изменения значения ползунка. Вот несколько советов, позволяющих повысить эффективность этой процедуры.

-   Обновите значение [**ползунка**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Slider) в зависимости от таймера, который запрашивает [**положение**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.position) в [**MediaPlayerElement.MediaPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.mediaplayerelement.mediaplayer). Используйте приемлемую частоту обновления таймера. Свойство **Position** обновляется только каждые 250 миллисекунд во время воспроизведения.
-   Частота шага на шкале ползунка должна подстраиваться под длительность видео.
-   Подпишитесь на события ползунка [**PointerPressed**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerpressed), [**PointerMoved**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointermoved) и [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased), чтобы установить значение 0 для свойства [**PlaybackRate**](https://docs.microsoft.com/uwp/api/windows.media.playback.mediaplaybacksession.playbackrate), когда пользователь перетаскивает бегунок по шкале.
-   В обработчике событий [**PointerReleased**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.pointerreleased) вручную присвойте значению положения воспроизведения значение положения ползунка, чтобы добиться оптимальной привязки бегунка при перемотке.

### <a name="match-video-resolution-to-device-resolution"></a>Соответствие разрешения видео разрешению устройства

Декодирование видео потребляет значительные ресурсы памяти и графического процессора, поэтому выберите формат видео, близкий к разрешению, при котором он будет отображаться. Если размер видео будет уменьшен, не имеет смысла тратить ресурсы на декодирование видео высокой четкости (1080). Многие приложения не включают одно и то же видео, закодированное в различных разрешениях; но если оно доступно, используйте кодировку, близкую к разрешению, в котором оно будет демонстрироваться.

### <a name="choose-recommended-formats"></a>Выбор рекомендуемых форматов

Выбор формата мультимедиа нередко является деликатной темой и определяется соображениями коммерческого свойства. С точки зрения быстродействия UWP рекомендуется выбрать формат видео H.264 в качестве основного формата видео, а AAC и MP3 — в качестве основных форматов аудио. Для воспроизведения локальных видеофайлов рекомендуются файлы формата MP4. При использовании большинства моделей последнего графического оборудования декодирование видео в формате H.264 ускоряется. Также, несмотря на то что аппаратное ускорение для декодирования VC-1 широко доступно большей части графического оборудования на рынке, во многих случаях оно ограничено частичным ускорением (IDCT) и не предоставляет полноценную разгрузку на оборудование (как, скажем, режим VLD).

Если вы полностью контролируете создание видео, вам необходимо подумать, как сохранить баланс между эффективностью сжатия и структурой GOP. GOP относительно меньшего размера с B-кадрами может увеличить быстродействие в режиме поиска или спецэффектов.

При включении коротких аудиоэффектов с небольшой задержкой, например в играх, используйте WAV-файлы с несжатыми значениями ИКМ, чтобы снизить вычислительную нагрузку по сравнению с обычной для сжатых аудиоформатов.


## <a name="optimize-image-resources"></a>Оптимизация ресурсов изображений

### <a name="scale-images-to-the-appropriate-size"></a>Изменение масштаба изображения до необходимого размера

Изображения захватываются в очень высоких разрешениях, что может привести к потреблению приложениями большего количества ресурсов ЦП при декодировании данных изображения и большего объема памяти после загрузки изображений с диска. Декодирование и сохранение в памяти изображения с высоким разрешением не имеет смысла, если это изображение будет показано в меньшем размере, чем оригинал. Вместо этого создайте версию изображения размера, который будет отображаться на экране, при помощи свойств [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) и [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight).

Нельзя:

```xaml
<Image Source="ms-appx:///Assets/highresCar.jpg"
       Width="300" Height="200"/>    <!-- BAD CODE DO NOT USE.-->
```

Вместо этого сделайте следующее:

```xaml
<Image>
    <Image.Source>
    <BitmapImage UriSource="ms-appx:///Assets/highresCar.jpg"
                 DecodePixelWidth="300" DecodePixelHeight="200"/>
    </Image.Source>
</Image>
```

По умолчанию единицы измерения [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) и [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) — физические пиксели. Свойство [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) можно использовать, чтобы изменить такое поведение: если задать для параметра **DecodePixelType** значение **Logical**, размер декодирования автоматически будет учитывать текущий коэффициент масштабирования системы, аналогично другому содержимому XAML. Таким образом, в общем случае стоит задать для параметра **DecodePixelType** значение **Logical**, например, если вы хотите, чтобы **DecodePixelWidth** и **DecodePixelHeight** соответствовали свойствам Height и Width элемента управления Image, в котором будет показано изображение. Поскольку по умолчанию используются физические пиксели, вы должны самостоятельно учитывать текущий коэффициент масштабирования системы; необходимо ожидать уведомления об изменении масштаба на случай, если пользователь изменит настройки экрана.

Если DecodePixelWidth или DecodePixelHeight явным образом заданы больше, чем ширина и высота изображения, которое будет показано на экране, то приложение без необходимости будет использовать дополнительный объем памяти до 4 байтов на пиксель, что быстро станет слишком затратным для больших изображений. Кроме того, изображение будет уменьшено с помощью билинейного масштабирования, отчего может выглядеть расплывчатым при больших коэффициентах масштабирования.

Если DecodePixelWidth или DecodePixelHeight явным образом заданы меньше, чем ширина и высота изображения, которое будет показано на экране, то изображение будет увеличено и может выглядеть пикселизированным.

В некоторых случаях, если определить нужный размер декодирования заранее нельзя, необходимо положиться на функцию автоматического определения нужного размера декодирования XAML, которая попытается декодировать изображение в нужном размере, если DecodePixelWidth или DecodePixelHeight не заданы явным образом.

Если вы знаете размер содержимого изображения заранее, необходимо задать размер декодирования явным образом. Вы также должны параллельно задать для параметра [**DecodePixelType**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixeltype) значение **Logical**, если предоставленный размер декодирования задан относительно размеров других элементов XAML. Например, если вы в явном виде задали размер содержимого с помощью Image.Width и Image.Height, вы можете задать для параметра DecodePixelType значение DecodePixelType.Logical, обеспечив использование тех же размеров в логических пикселях, что и для элемента управления «Изображение», а затем в явном виде использовать BitmapImage.DecodePixelWidth или BitmapImage.DecodePixelHeight, чтобы контролировать размер изображения для потенциально большей экономии памяти.

Обратите внимание на необходимость учета Image.Stretch при определении размера декодированного содержимого.

### <a name="right-sized-decoding"></a>Декодирование с определением нужного размера

Если вы не задали размер декодирования в явном виде, XAML попытается сэкономить память, декодировав изображение в том размере, в котором оно появится на экране, в соответствии с начальным макетом содержащей изображение страницы. Рекомендуется по возможности использовать эту функцию при написании приложения. Эта функция будет отключена при любом из нижеперечисленных условий.

-   Класс [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) подключен к дереву XAML в режиме реального времени после настройки содержимого с помощью [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) или [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource).
-   Изображение декодируется с использованием синхронного декодирования, например [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource).
-   Изображение скрыто, т. к. для параметра [**Opacity**](/uwp/api/Windows.UI.Xaml.UIElement.Opacity) установлено значение 0 или для параметра [**Visibility**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.visibility) — значение **Collapsed** на элементе основного изображения, или кисти, или любого родительского элемента.
-   Элемент управления Image или кисть использует для параметра [**Stretch**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Stretch) значение **None**.
-   Изображение используется как [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   Для элемента изображения или любого родительского элемента установлен `CacheMode="BitmapCache"`.
-   Кисть изображения непрямоугольная (например, при применении к фигуре или тексту).

В вышеприведенных сценариях единственный способ экономии памяти — задать размер декодирования в явном виде.

Обязательно подключайте [**BitmapImage**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.BitmapImage) к дереву в режиме реального времени до задания источника. Это необходимо делать каждый раз, когда элемент изображения или кисть задается в разметке. Примеры приведены ниже под заголовком "Примеры динамического дерева". Старайтесь как можно меньше использовать [**SetSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsource) и вместо этого использовать [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) при задании источника потока. Кроме того, по возможности лучше не скрывать визуальное содержимое (при помощи нулевой непрозрачности или свернутой видимости) во время ожидания события [**ImageOpened**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.imageopened). Делайте это на свой страх и риск — это помешает выиграть от автоматического определения нужного размера декодирования. Если приложение должно скрывать визуальное содержимое изначально, необходимо по возможности задать размер декодирования в явном виде.

**Примеры динамического дерева**

Пример 1 (как надо) — универсальный код ресурса (URI) задан в разметке.

```xaml
<Image x:Name="myImage" UriSource="Assets/cool-image.png"/>
```

Пример 2, разметка — URI задан в коде программной части.

```xaml
<Image x:Name="myImage"/>
```

Пример 2, код программной части (как надо) — подключение BitmapImage к дереву до задания его UriSource.

```csharp
var bitmapImage = new BitmapImage();
myImage.Source = bitmapImage;
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
```

Пример 2, код программной части (как не надо) — задание UriSource для BitmapImage до его подключения к дереву.

```csharp
var bitmapImage = new BitmapImage();
bitmapImage.UriSource = new URI("ms-appx:///Assets/cool-image.png", UriKind.RelativeOrAbsolute);
myImage.Source = bitmapImage;
```

### <a name="caching-optimizations"></a>Оптимизация кэширования

Оптимизация кэширования действует для изображений, которые используют [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) для загрузки содержимого из пакета приложения или из Интернета. URI используется для уникальной идентификации базового содержимого, и платформа XAML не будет скачивать или выполнять декодирование содержимого несколько раз при внутреннем использовании. Вместо этого она будет использовать программные или аппаратные ресурсы из кэша для показа содержимого несколько раз.

Исключение из этой оптимизации — если изображение отображается несколько раз в различных разрешениях (которые можно определить в явном виде или при помощи автоматического определения нужного размера декодирования). В каждой записи кэша также хранится разрешение изображения, и если XAML не может найти изображение с URI источника, которое соответствует требуемому разрешению, то будет выполнено декодирование новой версии в этом размере. Однако кодированные данные изображения скачаны заново не будут.

Следовательно, необходимо пользоваться [**UriSource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.urisource) при загрузке изображений из пакета приложения и избегать использования потока файла и [**SetSourceAsync**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync) без необходимости.

### <a name="images-in-virtualized-panels-listview-for-instance"></a>Изображения на виртуализированных панелях (например, ListView)

Если изображение удалено из дерева (удалено явным образом в приложении или неявным образом в результате прокручивания, оставаясь при этом частью современной виртуализированной панели), XAML оптимизирует использование памяти путем высвобождения аппаратных ресурсов, поскольку для показа изображения они больше не требуются. Память освобождается не мгновенно, а, скорее, во время обновления кадра, которое происходит через одну секунду после удаления элемента изображения из дерева.

Следовательно, необходимо по возможности использовать современные виртуализированные панели для размещения списков содержимого изображения.

### <a name="software-rasterized-images"></a>Программно преобразованные в растровый формат изображения

Если изображение используется для непрямоугольной кисти или [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid), изображение будет использовать программный путь растеризации, который вообще не масштабирует изображения. Кроме того, приложение должно сохранить копию изображения как в программной, так и в аппаратной памяти. Например, если изображение используется в качестве эллиптической кисти, то потенциально большое полное изображение будет сохранено внутренне дважды. При использовании **NineGrid** или непрямоугольной кисти ваше приложение должно предварительно масштабировать изображения примерно соответственно размеру, в котором они будут отрисованы.

### <a name="background-thread-image-loading"></a>Загрузка изображений фонового потока

XAML содержит внутреннюю оптимизацию, которая позволяет декодировать содержимое изображения асинхронно относительно поверхности в аппаратной памяти без необходимости промежуточной поверхности в программной памяти. Это сокращает пиковое потребление памяти и задержку отрисовки. Эта функция будет отключена при любом из нижеперечисленных условий.

-   Изображение используется как [**NineGrid**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.ninegrid).
-   Для элемента изображения или любого родительского элемента установлен `CacheMode="BitmapCache"`.
-   Кисть изображения непрямоугольная (например, при применении к фигуре или тексту).

### <a name="softwarebitmapsource"></a>SoftwareBitmapSource

Класс [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) используется для обмена совместимыми несжатыми изображениями между различными пространствами имен WinRT, такими как [**BitmapDecoder**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapDecoder), API камеры и XAML. Этот класс устраняет дополнительную копию, которая, как правило, нужна при использовании [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap), что помогает сократить пиковое потребление памяти и задержку «источник — экран».

Класс [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap), поставляющий исходные сведения, также можно настроить для использования пользовательского [**IWICBitmap**](https://docs.microsoft.com/windows/desktop/api/wincodec/nn-wincodec-iwicbitmap), чтобы создать перезагружаемое резервное хранилище, позволяющее приложению перераспределять память на свое усмотрение. Это вариант использования C++ для опытных пользователей.

Ваше приложение должно использовать классы [**SoftwareBitmap**](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap) и [**SoftwareBitmapSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.SoftwareBitmapSource) для взаимодействия с другими API WinRT, которые создают и используют изображения. И ваше приложение должно использовать **SoftwareBitmapSource** вместо [**WriteableBitmap**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Media.Imaging.WriteableBitmap) при загрузке несжатых данных изображения.

### <a name="use-getthumbnailasync-for-thumbnails"></a>Использование GetThumbnailAsync для получения эскизов

Одним из вариантов использования масштабирования изображений является создание эскизов. Несмотря на то что вы можете использовать [**DecodePixelWidth**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelwidth) и [**DecodePixelHeight**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.media.imaging.bitmapimage.decodepixelheight) для создания уменьшенных версий изображений, UWP предлагает еще более эффективные API для получения эскизов. [**GetThumbnailAsync**](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.getthumbnailasync) создает эскизы изображений, кэшированных в файловой системе. В этом случае производительность становится еще выше, чем при использовании API XAML, так как изображение не нужно открывать или декодировать.

> [!div class="tabbedCodeSnippets"]
> ```csharp
> FileOpenPicker picker = new FileOpenPicker();
> picker.FileTypeFilter.Add(".bmp");
> picker.FileTypeFilter.Add(".jpg");
> picker.FileTypeFilter.Add(".jpeg");
> picker.FileTypeFilter.Add(".png");
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary;
>
> StorageFile file = await picker.PickSingleFileAsync();
>
> StorageItemThumbnail fileThumbnail = await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64);
>
> BitmapImage bmp = new BitmapImage();
> bmp.SetSource(fileThumbnail);
>
> Image img = new Image();
> img.Source = bmp;
> ```
> ```vb
> Dim picker As New FileOpenPicker()
> picker.FileTypeFilter.Add(".bmp")
> picker.FileTypeFilter.Add(".jpg")
> picker.FileTypeFilter.Add(".jpeg")
> picker.FileTypeFilter.Add(".png")
> picker.SuggestedStartLocation = PickerLocationId.PicturesLibrary
>
> Dim file As StorageFile = Await picker.PickSingleFileAsync()
>
> Dim fileThumbnail As StorageItemThumbnail = Await file.GetThumbnailAsync(ThumbnailMode.SingleItem, 64)
>
> Dim bmp As New BitmapImage()
> bmp.SetSource(fileThumbnail)
>
> Dim img As New Image()
> img.Source = bmp
> ```

### <a name="decode-images-once"></a>Однократное декодирование изображений

Чтобы изображения не декодировались более одного раза, назначьте свойство [**Image.Source**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.image.source) из универсального кода ресурса (URI) вместо использования потоков в памяти. Платформа XAML может связывать один и тот же универсальный код ресурса (URI) с одним декодированным изображением в разных местах, но это становится невозможным при использовании нескольких потоков в памяти, содержащих одинаковые данные, поэтому она создает отдельное декодированное изображение для каждого потока в памяти.
