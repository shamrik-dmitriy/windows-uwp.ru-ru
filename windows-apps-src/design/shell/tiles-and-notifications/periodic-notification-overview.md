---
Description: Периодические, или опросные, уведомления обновляют плитки и индикаторы событий через фиксированные интервалы, скачивая содержимое напрямую из облачной службы.
title: Обзор периодических уведомлений
ms.assetid: 1EB79BF6-4B94-451F-9FAB-0A1B45B4D01C
template: detail.hbs
ms.date: 05/19/2017
ms.topic: article
keywords: Windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 617b5d013c8452733fae2a1fa7c16180d37fbe57
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259712"
---
# <a name="periodic-notification-overview"></a>Обзор периодических уведомлений
 


Периодические, или опросные, уведомления обновляют плитки и индикаторы событий через фиксированные интервалы, скачивая содержимое напрямую из облачной службы. Чтобы использовать периодические уведомления, код вашего приложения должен предоставить два фрагмента информации:

-   Универсальный код ресурса (URI) местоположения в Интернете, которое Windows будет опрашивать для обновления плиток или индикаторов событий для приложения
-   интервал опроса URI.

Периодические уведомления позволяют приложению обновлять живые плитки с минимальным участием облачной службы и клиента. Периодические уведомления удобно использовать для распространения одного и того же содержимого среди широкой аудитории.

**Обратите внимание** ,   вы можете узнать больше, загрузив [Пример push-уведомлений и периодического уведомления](https://code.msdn.microsoft.com/windowsapps/push-and-periodic-de225603) для Windows 8.1 и повторно используя его исходный код в приложении Windows 10.

 

## <a name="how-it-works"></a>Принцип работы


Для периодических уведомлений требуется, чтобы ваше приложение размещало облачную службу. Эта служба будет периодически опрашиваться всеми пользователями, у которых установлено приложение. При каждом интервале опроса (допустим, раз в час) Windows отправляет HTTP-запрос GET для URI, скачивает запрошенное содержимое плитки или индикатора (в формате XML), которое предоставляется в ответ на запрос, и отображает это содержимое на плитке приложения.

Обратите внимание, что периодические обновления нельзя использовать вместе со всплывающими уведомлениями. Последние лучше предоставлять через [запланированные](https://docs.microsoft.com/previous-versions/windows/apps/hh465417(v=win.10)) или [push-уведомления](https://docs.microsoft.com/previous-versions/windows/apps/hh868252(v=win.10)).

## <a name="uri-location-and-xml-content"></a>Местоположение URI и содержимое XML


В качестве URI для опроса можно использовать любые действующие веб-адреса HTTP или HTTPS.

Ответ облачного сервера включает в себя скачанное содержимое. Содержимое, возвращаемое URI, должно соответствовать спецификациям схемы XML [Плитка](adaptive-tiles-schema.md) или [Индикатор](https://docs.microsoft.com/uwp/schemas/tiles/badgeschema/schema-root) и должно быть закодировано в UTF-8. Вы можете использовать определенные заголовки HTTP для указания [срока действия](#expiration-of-tile-and-badge-notifications) или тега для уведомления.

## <a name="polling-behavior"></a>Поведение опроса


Чтобы начать опрос, вызовите один из следующих методов:

-   [**Стартпериодикупдате**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (плитка)
-   [**Стартпериодикупдате**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.BadgeUpdater#Windows_UI_Notifications_BadgeUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (эмблема)
-   [**Стартпериодикупдатебатч**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) (плитка)

При вызове одного из этих методов указанный в вызове URI немедленно опрашивается. Полученное содержимое используется для обновления плитки либо индикатора событий. После первоначального опроса Windows продолжает предоставлять обновления с указанной периодичностью. Опрос продолжается до явной остановки (с помощью [**TileUpdater.StopPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater.StopPeriodicUpdate)), удаления приложения или, если это вспомогательная плитка, — до удаления плитки. В противном случае Windows продолжит опрос для обновлений плитки или индикатора событий, даже если приложение никогда не будет запущено снова.

### <a name="the-recurrence-interval"></a>Интервал повторения

Интервал повторения передается перечисленным выше методам в виде параметра. Учтите, что хотя Windows стремится выполнять запросы с указанной периодичностью, интервал не является точным. Windows может отложить опрос на время до 15 минут.

### <a name="the-start-time"></a>Время начала

Дополнительно можно указать конкретное время дня для опроса. Представим себе приложение, меняющее содержимое своей плитки один раз в день. В этом случае мы рекомендуем выполнять опрос через небольшой промежуток времени после обновления вашей облачной службы. Например, если интернет-магазин публикует актуальные предложения дня в 8 часов утра, опрос на наличие нового содержимого для плиток лучше всего выполнять чуть позже 8 часов.

Если вы указали время начала опроса, то первый вызов метода сразу же запрашивает содержимое. Затем периодический опрос будет начинаться в течение 15 минут от указанного времени начала.

### <a name="automatic-retry-behavior"></a>Поведение автоматического повтора

Универсальный код ресурса (URI) опрашивается, только если у устройства есть доступ к сети. Если сеть доступна, но связь с URI по какой-либо причине невозможна, данный цикл опроса будет пропущен, и URI будет опрошен снова в течение следующего интервала. Если на момент запланированного опроса устройство выключено, находится в спящем режиме или в режиме гибернации, то URI будет опрошен после выхода устройства из выключенного состояния или спящего режима.

### <a name="handling-app-updates"></a>Обработка обновлений приложений

При выпуске обновления приложения, которое изменяет URI опроса, необходимо добавить ежедневный [временной триггер фоновой задачи,](../../../launch-resume/run-a-background-task-on-a-timer-.md) который вызывает метод StartPeriodicUpdate с новым URI, чтобы обеспечить его использование плитками. В противном случае если пользователи получат обновление для вашего приложения, но не запустят приложение, их плитки будут по-прежнему использовать старые URI и могут не отобразиться, если URI недействителен или если в возвращаемых полезных данных присутствует ссылка на локальное изображение, которое больше не существует.

## <a name="expiration-of-tile-and-badge-notifications"></a>Истечение сроков действий уведомлений на плитках и индикаторах событий


По умолчанию срок действия периодических уведомлений на индикаторах событий и плитках истекает через три дня с момента скачивания уведомлений. По окончании срока действия уведомления содержимое удаляется с плитки или из очереди и более не показывается пользователю. Рекомендуется явным образом установить срок действия для всех периодических уведомлений на плитках и индикаторах событий. Исходя из особенностей вашего приложения или уведомления, следует выбрать время, позволяющее гарантировать, что содержание вашей плитки не будет сохраняться после того, как утратит актуальность. Явное указание срока действия важно для содержимого с определенной продолжительностью существования. Это также гарантирует удаление устаревшего содержимого, если облачная служба стала недоступной или если пользователь отключился от сети на продолжительное время.

Облачная служба устанавливает срок действия и время уведомления добавлением HTTP-заголовка X-WNS-Expires в полезные данные HTTP-отклика. HTTP-заголовок X-WNS-Expires соответствует формату [HTTP-date](https://www.w3.org/Protocols/rfc2616/rfc2616-sec3.html#sec3.3.1). Подробнее: [**StartPeriodicUpdate**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdate_Windows_Foundation_Uri_Windows_Foundation_DateTime_Windows_UI_Notifications_PeriodicUpdateRecurrence_) или [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_).

Например, во время торгового дня на бирже срок действия для обновлений курсов акций можно установить равным двойному интервалу опроса (допустим, через час после получения при опросе, выполняемом каждые полчаса). Рассмотрим другой пример: для новостного приложения можно определить, что один день является подходящим временем для истечения срока действия обновлений плитки ежедневных новостей.

## <a name="periodic-notifications-in-the-notification-queue"></a>Периодические уведомления в очереди уведомлений


Вы можете использовать периодические обновления плиток вместе с [цикличностью уведомлений](https://docs.microsoft.com/previous-versions/windows/apps/hh781199(v=win.10)). По умолчанию плитка на начальном экране показывает содержимое одного уведомления, пока его не заменит новое уведомление. При включении цикличности в ней сохраняется до пяти уведомлений, и плитка отображает их одно за другим.

Если в очереди уже есть пять уведомлений, следующее новое уведомление заменит самое старое уведомление в очереди. Однако задав теги уведомления, вы можете повлиять на политику замены в очереди. Тег — это относящаяся к определенному приложению строка без учета регистра длиной до 16 алфавитно-цифровых символов, указанная в HTTP-заголовке [X-WNS-Tag](https://docs.microsoft.com/previous-versions/windows/apps/hh465435(v=win.10)) в полезных данных HTTP-отклика. Windows сравнивает тег входящего уведомления с тегами всех уведомлений в очереди. При совпадении тегов новое уведомление заменяет уведомление из очереди с тем же тегом. Если совпадение не найдено, применяется стандартное правило замены, и новое уведомление заменяет самое старое уведомление в очереди.

Очереди и теги уведомлений вы можете использовать для реализации различных комплексных сценариев уведомлений. Например, биржевое приложение может отправлять пять уведомлений, каждое из которых относится к различным акциям и содержит тег с названием акции. Это предотвратит появление в очереди двух уведомлений об одних и тех же акциях, более раннее из которых устарело.

Дополнительные сведения см. в разделе [Использование очереди уведомлений](https://docs.microsoft.com/previous-versions/windows/apps/hh781199(v=win.10)).

### <a name="enabling-the-notification-queue"></a>Включение очереди уведомлений

Для реализации очереди уведомлений сначала включите очередь для плитки (см. раздел [Использование очереди уведомлений в случае локальных уведомлений](https://blogs.msdn.microsoft.com/tiles_and_toasts/2016/01/05/quickstart-how-to-use-the-tile-notification-queue-with-local-notifications/)). Вызов для включения очереди достаточно выполнить однократно в течение жизненного цикла приложения, но можно также выполнять его при каждом запуске приложения.

### <a name="polling-for-more-than-one-notification-at-a-time"></a>Выполнение опроса для нескольких уведомлений за раз

Для каждого уведомления, которое Windows должна скачивать для вашей плитки, вам нужно предоставить уникальный URI. Использование метода [**StartPeriodicUpdateBatch**](https://docs.microsoft.com/uwp/api/Windows.UI.Notifications.TileUpdater#Windows_UI_Notifications_TileUpdater_StartPeriodicUpdateBatch_Windows_Foundation_Collections_IIterable_1_Windows_UI_Notifications_PeriodicUpdateRecurrence_) позволяет предоставить для работы с очередью уведомлений до пяти URI одновременно. Все URI опрашиваются на наличие отдельных полезных данных уведомления одновременно или почти одновременно. Каждый опрошенный URI может возвращать собственные срок действия и значение тега.

## <a name="related-topics"></a>См. также


* [Рекомендации для периодических уведомлений](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-periodic-notification-overview)
* [Настройка периодических уведомлений для эмблем](https://docs.microsoft.com/previous-versions/windows/apps/hh761476(v=win.10))
* [Настройка периодических уведомлений для плиток](https://docs.microsoft.com/previous-versions/windows/apps/hh761476(v=win.10))
 
