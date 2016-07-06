---
description: "Здесь вы найдете инструкции по проектированию и программированию для добавления элементов управления и шаблонов в приложение UWP."
title: "Элементы управления и шаблоны — разработка приложений для Windows"
author: mijacobs
translationtype: Human Translation
ms.sourcegitcommit: 9f75c39d26bd0c8858f404ab4fcd3d23562ea033
ms.openlocfilehash: 0562e3df0c2abbb0808df5f75ff4fe0b96eb6d7e

---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 


# Элементы управления и шаблоны для приложений UWP

В разработке приложений UWP <i>элемент управления</i> — это элемент пользовательского интерфейса, отображающий содержимое или обеспечивающий взаимодействие. Элементы управления — это основа пользовательского интерфейса. Мы предоставляем более 45 элементов управления — от простых кнопок до мощных элементов управления данными, таких как представление сетки. <i>Шаблон</i> — это макет для объединения нескольких элементов управления, позволяющий получить что-то новое.

В статьях в этом разделе представлены инструкции по проектированию и программированию для добавления элементов управления и шаблонов в приложение UWP. 

## Введение

Общие инструкции и примеры кода для добавления и настройки стиля элементов управления на языках XAML и C#.

<div class="side-by-side">
<div class="side-by-side-content">
  <div class="side-by-side-content-left">
   <p><b>[Добавление элементов управления и обработка событий](controls-and-events-intro.md)</b> <br/>
Есть три основных этапа добавления элементов управления в приложение: добавление элемента управления в пользовательский интерфейс приложения, установка свойств элемента управления и добавление кода в обработчики событий элемента управления, чтобы он начал функционировать.</li>
</ul> 
</p>
  </div>
  <div class="side-by-side-content-right">
   <p><b>[Настройка стиля элементов управления](styling-controls.md)</b> <br/>
Платформа XAML предоставляет множество способов настройки внешнего вида приложений. С помощью стилей вы можете настраивать свойства элементов управления и многократно использовать эти параметры, чтобы придать единообразный внешний вид нескольким элементам управления.</p>
  </div>
</div>
</div>

## Алфавитный указатель 

Подробные сведения о конкретных элементах управления и шаблонах.

(Список, отсортированный по функциям, см. в разделе [Элементы управления по функциям](controls-by-function.md).)

<div class="uwpd-list-of-links">
<ul>

<li>[Активный холст](active-canvas.md)</li>

<li>[Поле автозаполнения](auto-suggest-box.md)</li>

<li>[Панели](app-bars.md)</li>

<li>[Кнопки](buttons.md)</li>

<li>[Флажок ](checkbox.md)</li>

<li>[Элементы управления датой и временем](date-and-time.md)
<ul>

<li>[Выбор даты в календаре](calendar-date-picker.md)</li>

<li>[Представление календаря](calendar-view.md)</li>

<li>[Выбор даты](date-picker.md)</li>

<li>[Выбор времени](time-picker.md)</li>
</ul>
</li>


<li>[Диалоговые окна, всплывающие окна и меню](dialogs-popups-menus.md)</li>

<li>[Представление пролистывания](flipview.md)</li>

<li>[Концентратор](hub.md)</li>

<li>[Гиперссылки](hyperlinks.md)</li>

<li>[Изображения и кисти изображений](images-imagebrushes.md)</li>

<li>[Списки](lists.md)</li>

<li>[Основные и подробные данные](master-details.md)</li>

<li>[Воспроизведение мультимедиа](media-playback.md)</li>

<li>[Пользовательские элементы управления транспортировкой](custom-transport-controls.md)</li>

<li>[Панель навигации](nav-pane.md)</li>

<li>[Элементы управления "Ход выполнения"](progress-controls.md)</li>

<li>[Переключатель](radio-button.md)</li>

<li>[Элементы управления прокруткой и панорамированием](scroll-controls.md)</li>

<li>[Поиск](search.md)</li>

<li>[Контекстное масштабирование](semantic-zoom.md)</li>

<li>[Ползунок](slider.md)</li>

<li>[Комбинированный режим](split-view.md)</li>

<li>[Вкладки и сводки](tabs-pivot.md)</li>

<li>[Элементы управления текстом](text-controls.md)
<ul>

<li>[Шрифты](fonts.md)</li>
<li>[Метки](labels.md)</li>

<li>[Поля пароля](password-box.md)</li>

<li>[Поля расширенного редактирования](rich-edit-box.md)</li>

<li>[Блоки форматированного текста](rich-text-block.md)</li>

<li>[Значки Segoe MDL2](segoe-ui-symbol-font.md)</li>

<li>[Проверка правописания и прогнозирование](spell-checking-and-prediction.md)</li>

<li>[Блок текста](text-block.md)</li>

<li>[Текстовое поле](text-box.md)</li>
</ul>
</li>



<li>[Плитки, индикаторы событий и уведомления](tiles-badges-notifications.md)
<ul>

<li>[Плитки](tiles-and-notifications-creating-tiles.md)</li>

<li>[Адаптивные плитки](tiles-and-notifications-create-adaptive-tiles.md)</li>

<li>[Схема адаптивных плиток](tiles-and-notifications-adaptive-tiles-schema.md)</li>

<li>[Рекомендации по ресурсам](tiles-and-notifications-app-assets.md)</li>

<li>[Специальные шаблоны плиток](tiles-and-notifications-special-tile-templates-catalog.md)</li>

<li>[Адаптивные и интерактивные всплывающие уведомления](tiles-and-notifications-adaptive-interactive-toasts.md)</li>

<li>[Визуализатор уведомлений](tiles-and-notifications-notifications-visualizer.md)</li>

<li>[Методы доставки уведомлений](tiles-and-notifications-choosing-a-notification-delivery-method.md)</li>

<li>[Локальные уведомления плиток](tiles-and-notifications-sending-a-local-tile-notification.md)</li>

<li>[Периодические уведомления](tiles-and-notifications-periodic-notification-overview.md)</li>

<li>[WNS](tiles-and-notifications-windows-push-notification-services--wns--overview.md)</li>

<li>[Необработанные уведомления](tiles-and-notifications-raw-notification-overview.md)</li>
</ul>
</li>


<li>[Переключение](toggles.md)</li>
<li>[Подсказки](tooltips.md)</li>

<li>[Представление веб-страницы](web-view.md)</li>
</ul>
</div>



<!--HONumber=Jun16_HO4-->

