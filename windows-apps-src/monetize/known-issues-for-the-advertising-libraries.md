---
author: mcleanbyron
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Изучите известные проблемы текущего выпуска библиотек Microsoft Advertising в пакете SDK Microsoft Store Engagement and Monetization.
title: Известные проблемы для библиотек Microsoft Advertising
---

# Известные проблемы для библиотек Microsoft Advertising


\[ Обновлено для приложений UWP в Windows 10. Статьи о Windows 8.x см. в [архиве](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

В этом разделе перечислены известные проблемы текущего выпуска библиотек Microsoft Advertising в пакете SDK Microsoft Store Engagement and Monetization.

## Установка требует инструментов Visual Studio для универсальных приложений для Windows

Для установки пакета [SDK Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) с помощью Visual Studio 2015 должны быть установлены инструменты Visual Studio для универсальных приложений для Windows версии 1.1 или более поздней. Дополнительные сведения см. в [примечаниях к выпуску](http://go.microsoft.com/fwlink/?LinkID=624516) Visual Studio.

## Проекты Silverlight в Windows Phone 8.x

Чтобы получить сборки Microsoft Advertising для проектов Silverlight в Windows Phone 8.x, установите пакет [SDK Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk), откройте проект в Visual Studio и последовательно щелкните **Проект** > **Добавить подключенную службу** > **Рекламный посредник**, чтобы автоматически загрузить сборки. После этого можно удалить ссылки на рекламный посредник из вашего проекта, если в дальнейшем вы не планируете использовать рекламное посредничество. Дополнительные сведения см. в разделе [AdControl в Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

## Интерфейс AdControl неизвестен в XAML

В разметке XAML для элемента [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) может некорректно отображаться синяя курсивная строка, обозначая, что интерфейс неизвестен. Это происходит только в системах x86, ошибку можно игнорировать.

## lastError из предыдущего запроса рекламного объявления

Если с прошлого запроса рекламного объявления осталась ошибка **lastError**, событие может быть запущено дважды во время следующего вызова объявления. Несмотря на то что новый запрос объявления будет обработан и в результате даже может быть получено действительное рекламное объявление, такое поведение может вызывать путаницу.

## Промежуточные рекламы и кнопки навигации на телефонах

На телефонах (или эмуляторах) с программными кнопками **Назад**, **Пуск** и **Поиск** вместо аппаратных таймер отсчета и кнопки прокрутки промежуточной видеорекламы могут не отображаться.

## Недавно созданные объявления не отображаются в приложении

Если вы опубликовали объявление недавно (менее дня), то оно может быть еще недоступно. Если объявление утверждено для редакционного содержимого, то оно отобразится после того, как будет обработано сервером рекламы и станет доступно в числе рекламных объявлений.

## В приложении не отображается реклама

Существует несколько причин этой проблемы, включая ошибки сети. Другие возможные причины:

* Выбор рекламного объявления в Центре разработки Windows большего или меньшего размера чем **AdControl** в коде приложения.

* Реклама не будут отображаться, если вы используете [значение тестового режима](test-mode-values.md) для идентификатора единицы рекламы при запуске реального приложения.

* Если вы создали новый идентификатор рекламной единицы в последние полчаса, реклама может не отображаться до тех пор, пока серверы не распространят новые данные по системе. Существующие идентификаторы, которые показывали рекламу раньше, должны показывать ее немедленно.

Если вы видите тестовые объявления в приложении, то ваш код работает и может отображать рекламу. Если проблемы сохраняются, обратитесь в [службу поддержки по продукту](https://go.microsoft.com/fwlink/p/?LinkId=331508). На этой странице выберите **Реклама в приложении**.

Можно также опубликовать вопрос на [форуме](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## В приложении отображается тестовая реклама, а не реальные объявления

Тестовая реклама может отображаться, даже если вы рассчитываете увидеть настоящую рекламу. Это происходит в следующих случаях.

* Microsoft Advertising не может проверить или найти ИД настоящей рекламы в магазине приложений. В этом случае когда пользователь создает рекламную единицу, она сначала получает статус актуальной (нетестовой), а затем в течение 6 часов после первого рекламного запроса получает статус тестовой. При отсутствии запросов от тестовых приложений в течение 10 дней реклама снова перейдет в разряд актуальных.

* Неопубликованные приложения или приложения, работающие в эмуляторе, не отображают актуальные объявления.

Если реальная рекламная единица показывает тестовые объявления,статус рекламной единицы в Центре разработки Windows — **Активная и показывает тестовые объявления**. В настоящее время это неактуально для телефонных приложений.

## Устаревшие тестовые значения для ИД рекламной единицы и ИД приложения более не работают

Следующие тестовые значения для приложений Windows Phone Silverlight устарели и не работают. Если у вас есть существующий проект, который использует эти тестовые значения, обновите его и используйте значения из раздела [Значения тестового режима](test-mode-values.md).

| Application ID  |  ИД рекламного блока    |
|-----------------|----------------|
| test_client     |  Image320_50   |
| test_client     |  Image300_50   |
| test_client     |  TextAd   |
| test_client     |  Image480_80   |

<span id="reference_errors"/>
## Ошибки ссылок, вызванные ориентацией проекта на любой ЦП

При использовании библиотек Microsoft Libraries невозможно ориентироваться в проекте на **Любой ЦП**. Если проект ориентирован на платформу **Любой ЦП**, после добавления ссылки (см. пример ниже) может отобразиться предупреждение.

![referenceerror\-solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Чтобы убрать это предупреждение, обновите проект, чтобы использовать сборку определенной архитектуры (например, **x86**). Используйте **Диспетчер конфигурации**, чтобы задать целевые объекты платформы для конфигураций отладки и выпуска.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

При создании пакетов приложений для отправки в магазин (см. следующие изображения) обязательно включите архитектуры, на которые вы ориентируетесь. Можно пропустить архитектуры x64, если вы планируете запускать сборки x86 в ОС x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## Z-порядок в приложениях JavaScript и HTML

Приложения JavaScript/HTML не должны помещать элементы в зарезервированный диапазон MAX-10 z-порядка. Единственное исключение — это наложение прерывания, например уведомление о входящем вызове в Skype.

<span id="bkmk-ui"/>
## Не используйте границы

Использование свойств для границ, унаследованных **AdControl** от родительского класса, может стать причиной неверного размещения рекламы.

## Дополнительные сведения


Дополнительные сведения об актуальных известных ошибках и публикации вопросов, связанных с библиотеками Microsoft Advertising, см. на [форуме](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## Поддержка


Чтобы обратиться в службу поддержки продуктов с проблемой, связанной с библиотеками Microsoft Advertising, посетите [страницу поддержки](https://go.microsoft.com/fwlink/p/?LinkId=331508) и выберите **Реклама в приложении**.

 

 


<!--HONumber=May16_HO2-->

