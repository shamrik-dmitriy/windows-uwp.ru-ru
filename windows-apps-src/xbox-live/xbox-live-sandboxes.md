---
title: Xbox Live "песочницы"
description: Дополнительные сведения о "песочницы" для разработки Xbox Live.
ms.assetid: a5acb5bf-dc11-4dff-aa94-6d1f01472d2a
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, игры, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: ee284550a9b508a8d46556bf0353bd75d55014f3
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57599479"
---
# <a name="xbox-live-sandboxes-intro"></a>Общие сведения о Xbox Live "песочницы"

В [конфигурации-службы Xbox Live](xbox-live-service-configuration.md), объяснялось, что необходимо настроить сведения о заголовок вашей сети, обычно в [центра партнеров](https://partner.microsoft.com/dashboard).  Эти сведения включают такие списки лидеров, название хочет отобразить, достижения, которые можно разблокировать игроков, подбор игроков конфигурации и т. д.

При внесении изменений в конфигурацию службы, они должны публиковаться из центра партнеров, прежде чем изменения выбираются оставшаяся часть Xbox Live и можно увидеть, название.

Публикации так называемый изолированной среды для разработки.  Это позволяет вам работать на изменения для заголовка в изолированной среде.  Эти решения предоставляют ряд преимуществ, описанные в разделе ниже.

По умолчанию один консолей Xbox и ПК Windows 10 находятся в розничной ТОРГОВЛИ "песочницы".

## <a name="benefits"></a>Преимущества

Разработки "песочницы" предоставляют несколько преимуществ:


1. Вы можете переключаться на изменения для обновления названия без влияния на настоящее время доступна версия.
2. Некоторые инструменты работают только в изолированной среде разработки по соображениям безопасности.
3. Некоторые разработчики в вашей команде может потребоваться «ветви» и тестирования изменений конфигурации службы без влияния на конфигурацию основной службы в разработке.
4. Другие издатели не могут видеть, что вы работаете над без предоставления доступа к вашей "песочницы".

Вы также можете **при необходимости** Создание тестовых учетных записей.  Их можно использовать, если вы не хотите использовать для тестирования заголовок вашей учетной записи регулярных Xbox Live, или несколько учетных записей для проверки сценариев, таких как социальное взаимодействие (например: Просмотр статистики друга) или многопользовательской.

Тестовые учетные записи можно только войти в "песочницах" разработки и объясняются в разделе ниже.

## <a name="finding-out-about-your-sandbox"></a>Поиск информации о изолированную среду

Большинство разработчиков требуется только одна «песочница».  К счастью "песочницу" создается автоматически при создании заголовка.

1. Вы узнали об изолированную среду, перейдя в центр партнеров здесь: ![](images/getting_started/first_xbltitle_dashboard.png)

1. Нажмите кнопку на заголовке: ![](images/getting_started/first_xbltitle_dashboard_overview.png)

1. Наконец щелкните служб -> Xbox Live в меню слева ![](images/getting_started/first_xbltitle_leftnav.png)

1. Теперь вы видите ниже приведен список изолированную среду ![](images/getting_started/devcenter_sandbox_id.png)

## <a name="how-your-sandbox-impacts-your-workflow"></a>Влияние изолированную среду рабочего процесса

Обычно вы работаете "песочницы" одним из следующих способов:

1. (Однократно) Переключитесь на ПК или на Xbox One изолированную среду разработки.
2. (Многие и время) После внесения изменений в конфигурацию службы, будет публиковать изменения в изолированную среду разработки.  Изменения конфигурации службы, например определение достижения, списки лидеров добавления или изменения шаблона сеанса многопользовательскую.
3. (Несколько раз) Если вы работаете с другими членами команды, вы можете предоставить им доступ к изолированную среду
4. (Однократно) Если вам нужно проверить что-то в розничной ТОРГОВЛЕ, или вы хотите сделать перерыв играть любимые игры Xbox, необходимо будет снова перевести изолированную среду розничной ТОРГОВЛИ.

Эти сценарии будут описаны более подробно ниже.  Процесс имеет ряд отличий, на ПК и консолей, поэтому существуют отдельные разделы для каждого.

## <a name="switch-your-pcs-development-sandbox"></a>Переключение "песочницы" для компьютера разработки

Если необходимо переключиться "песочницы" для компьютера разработки, рекомендуемый способ сделать это с помощью Windows Device Portal (WDP).  Также вы можете с помощью командной строки.  Мы опишем оба способа.

### <a name="windows-device-portal"></a>Портал устройств Windows

Если вы еще не включили WDP на Компьютере, следуйте этим инструкциям, чтобы сделать это. [Настройка портала устройства на рабочем столе Windows](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal-desktop)

Это сделано, откройте портал разработчика Windows, подключение к нему в веб-браузере, как описано в указанной выше статье.

Затем можно щелкнуть «Xbox Live» для перехода к соответствующему разделу, как показано ниже.

![](images/getting_started/wdp_switch_sandbox.png)

Можно ввести в "песочницы", который вы получили, выполнив действия в *поиск Out Your "песочницы"* и нажмите кнопку «Изменить».

Переключиться обратно на розничной ТОРГОВЛИ, здесь можно ввести розничной ТОРГОВЛИ.

### <a name="powershell-module"></a>Модуль PowerShell

[Модуль PowerShell Live Xbox](https://github.com/Microsoft/xbox-live-powershell-module/blob/master/docs/XboxLivePsModule.md) XboxlivePSModule содержит различные служебные программы, чтобы помочь разработчикам Xbox Live, включая изменение "песочницы", на ПК или консоли.

* Чтобы использовать ее [коллекции PowerShell](https://www.powershellgallery.com/packages/XboxlivePSModule), откройте окно PowerShell:
    1. Скачайте и установите модуль: `Install-Module XboxlivePSModule -Scope CurrentUser`
    2. Запустить с помощью, выполнив `Import-Module XboxlivePSModule`
    3. Выполните командлеты, т. е. XDKS.1 Set XblSandbox или Get-XblSandbox

* Чтобы использовать его из ZIP-файл по [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools), откройте окно PowerShell,
    1. Выполните команду `Import-Module <path to unzipped folder>\XboxLivePsModule\XboxLivePsModule.psd1`.
    2. Выполните командлеты, т. е. XDKS.1 Set XblSandbox или Get-XblSandbox

### <a name="command-prompt-script"></a>Сценарий командной строки

Загрузите Xbox Live пакет средств в [ https://aka.ms/xboxliveuwptools ](https://aka.ms/xboxliveuwptools) и извлеките его содержимое.  Вы найдете в пакетном файле SwitchSandbox.cmd.

Выполните этот код в режиме администратора, чтобы переключиться в "песочницы".  Первым аргументом является «песочницы».  Например, если вы пытаетесь переключиться в режим "песочницы" XDKS.1, это делается:

```
SwitchSandbox.cmd XDKS.1
```

Чтобы вернуться к розничной ТОРГОВЛИ, достаточно просто указать, как второй аргумент.

```
SwitchSandbox.cmd RETAIL
```

## <a name="switch-your-xbox-one-console-development-sandbox"></a>Переключение Xbox One "песочницы" консоли разработки

### <a name="using-windows-dev-portal"></a>С помощью портала разработки Windows

На портале разработки Windows можно использовать для изменения «песочницы» на консоли.  Чтобы сделать это, перейдите к «Home Dev» на консоли и включить его.

После этого можно ввести IP-адрес, на веб-браузер на Компьютере для подключения к консоли.  Затем можно щелкнуть «Xbox Live» и введите в текстовом поле существует "песочницы".

### <a name="using-xbox-one-manager"></a>С помощью одного менеджера Xbox

Xbox One Manager позволяет администрировать определенные аспекты консоль с компьютера.  Это включает в себя перезагрузку, управление установленных приложений и изменение изолированную среду.

Щелкните правой кнопкой мыши в консоли для "песочницы" для изменения и перейти к «Параметры...»

Затем можно ввести "песочницу" существует.

### <a name="using-xbox-one-console-ui"></a>С помощью консоли Xbox One пользовательского интерфейса

Если вы хотите изменить изолированной среды для разработки прямо из консоли, можно перейти к «Параметры».  Перейдите к «Параметры разработчика», и вы увидите вариант изолированную среду.

## <a name="sandbox-uses"></a>Использует "песочницы"

### <a name="data-that-is-sandboxed"></a>Данные, которые хранятся в изолированной среде
Функции "песочницы" можно использовать для управления доступом между разработчиками в вашей группе, во время разработки.  Например можно изолировать данные между разработчиками и тест-инженеров.

Изолированной данные включают в себя:
- Достижения, списки лидеров и статистика для пользователя.  Достижений, накопленные для пользователя в одна «песочница» не преобразуются в другой "песочницы".
- Многопользовательскую и подбор игроков.  Пользователям не удается воспроизвести многопользовательскую игру с кем-то является другой изолированной среде.
- Конфигурация службы.  При добавлении нового награду с названием в одна «песочница» не отображается в другой изолированной среде.  Это относится ко всем данным конфигурации службы.

Non изолированной данные являются преимущественно социальных сведений.  Поэтому, например, если пользователь соответствует другому пользователю, что связь является независимой от "песочницы".

### <a name="examples"></a>Примеры
Некоторые примеры будет предоставляться ниже, чтобы проиллюстрировать некоторые из преимуществ почему может потребоваться использование нескольких "песочницы".

> **Примечание**. Если вы находитесь в программы создателей Xbox, может иметь только одна «песочница».  Если вам нужно создать несколько "песочницы", примените к ID@Xbox программы.

#### <a name="service-config-isolation"></a>Изоляция конфигурации службы
Как упоминалось выше, определенных "песочницы" используется конфигурация службы.  Следовательно, *разработки* "песочницы" и *тестирования* "песочницы".  Когда вы предоставляете построения заголовка таким образом, чтобы тест-инженерам, можно опубликовать ваш [конфигурации службы](xbox-live-service-configuration.md) для *тестирования* "песочницы".

И в то же время, можно добавить достижений, или многопользовательской различных типов, чтобы ваши *разработки* "песочницы", не влияя на конфигурации службы, видите тест-инженерам.

#### <a name="multiplayer"></a>Многопользовательский режим
Рассмотрим в качестве примера выше с *разработки* и *тестирования* "песочницы".  Возможно конфигурацию службы является одинаковым в разных "песочницы", но разработчики при создании многопользовательские функции и хотите тестирования координирующему друг с другом.  Также тестируете многопользовательские тест-инженерам.

В данном случае разработчикам не требуется службы Xbox Live Координирующему для сопоставления с ними с помощью инженеров-испытателей, так как при отладке проблем отдельно.  Хороший способ избежать этого, будет для разработчиков в *разработки* "песочницы" и инженеров-испытателей в отдельном *тестирования* "песочницы".  Это действие оставляет обе группы изолированной.

## <a name="advanced"></a>Дополнительно

Чтобы упростить процесс разработки, начать с изолированную среду по умолчанию и добавление новых "песочниц" осмотрительно.

Найдя вашего доступа данных и управления изоляции потребностей растет, вы увидите [Advanced Xbox Live "песочницы"](advanced-xbox-live-sandboxes.md) статьи.  