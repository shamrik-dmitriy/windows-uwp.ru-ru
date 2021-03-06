---
Description: После создания приложения посредством резервирования имени можно начать процесс его публикации. Первый этап — это создание отправки.
title: Отправка приложений
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: контрольный список, windows, uwp, отправка, отправить, игры, приложения
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260025"
---
# <a name="app-submissions"></a>Отправка приложений


После [создания приложения посредством резервирования имени](create-your-app-by-reserving-a-name.md) можно начать процесс его публикации. Первый этап — это создание **отправки**.

Вы можете начать отправку после того, как ваше приложение будет завершено и готово к публикации. Кроме того, можно начать ввод сведений заблаговременно, даже если еще не написано ни единой строчки кода. Обновления, внесенные в отправку, сохраняются, поэтому вы можете вернуться и работать над ним, когда будете готовы.

> [!NOTE]
> Для отправки приложений в Microsoft Store необходимо иметь активную [учетную запись разработчика](https://developer.microsoft.com/store/register) в [центре партнеров](https://partner.microsoft.com/dashboard) .

После публикации приложения можно опубликовать обновленную версию, создав еще одну отправку в центре партнеров. Создание новой отправки позволяет вносить и публиковать любые необходимые изменения будь то при передаче новых пакетов или при простом изменении сведений, например цены или категории. Чтобы создать новую отправку для опубликованного приложения, щелкните **Обновить** рядом с последней отправкой, отображаемой на странице **обзора** . Вы также можете [удалить приложение из магазина](guidance-for-app-package-management.md#removing-an-app-from-the-store) , если это необходимо сделать (а затем снова сделать его доступным позже, если хотите).

> [!NOTE]
> В этом разделе документации описано, как создать отправку приложения в центре партнеров. Кроме того, с помощью [API отправки в Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) можно автоматизировать отправку надстроек.

> [!IMPORTANT]
> Начиная с 31 октября 2018 г. Недавно создаваемые продукты не могут содержать пакеты, предназначенные для Windows 8. x/Windows Phone 8. x или более ранних версий. Дополнительные сведения см. в этой [записи блога](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Контрольный список для отправки приложения

Ниже приведен список сведений, которые необходимо предоставить при создании отправки приложения, а также ссылки на дополнительные сведения.

Элементы, которые следует предоставить или указать в обязательном порядке, приведены ниже. Некоторые пункты являются необязательными или в них уже подставлены значения по умолчанию, которые можно изменить по своему выбору. Вам не нужно работать с этими разделами в порядке, указанном здесь.

### <a name="pricing-and-availability-page"></a>Страница сведений о стоимости и доступности
| Имя поля                    | Примечания                                       | Дополнительные сведения                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Рынков**                   | Параметр по умолчанию: "Все возможные страны".  | [Определение цен и выбора рынка](define-pricing-and-market-selection.md)         |
| **Компиляцию**                | Значение "Общедоступная аудитория" используется по умолчанию | [Компиляцию](choose-visibility-options.md#audience) |
| **Возможность обнаружения**                | Параметр по умолчанию: "Сделать это приложение доступным и обнаруживаемым в Store". | [Возможность обнаружения](choose-visibility-options.md#discoverability) |
| **Расписание**                  | Параметр по умолчанию: "Выпуск как можно скорее".        | [Настройка точного планирования выпуска](configure-precise-release-scheduling.md) |
| **Базовая цена**                | Обязательно                                    | [Настройка и планирование цен приложений](set-and-schedule-app-pricing.md)              |
| **Бесплатная пробная версия**                | Стандартно: нет бесплатной пробной версии                      | [Бесплатная пробная версия](set-app-pricing-and-availability.md#free-trial)              |
| **Цены распродажи**              | Необязательный                                    | [Выставление приложений и надстроек на продажу](put-apps-and-add-ons-on-sale.md)           |
| **Лицензирование Организации**    | По умолчанию: разрешить получение томов по организациям | [Варианты лицензирования Организации](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Страница свойств

| Имя поля                    | Примечания                                       | Дополнительные сведения                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Категория и Подкатегория**  | Обязательно                                    | [Таблица категорий и подкатегорий](category-and-subcategory-table.md)       |
| **URL-адрес политики конфиденциальности**            | Требуется для многих приложений. См. [Соглашение с разработчиком приложений](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) и [Политики Microsoft Store](store-policies.md#105-personal-information) | [URL-адрес политики конфиденциальности](enter-app-properties.md#privacy-policy-url)        |
| **Веб**                   | Необязательный                                    | [Веб](enter-app-properties.md#website)                   |
| **Контактные данные службы поддержки**      | Требуется, если ваш продукт доступен на консоли Xbox; в противном случае это необязательно (однако рекомендуется)                                   | [Контактные данные службы поддержки](enter-app-properties.md#support-contact-info)              |
| **Параметры игры**             | Необязательно (применимо только к играм)         | [Параметры игры](enter-app-properties.md#game-settings) |
| **Режим экрана**             | Необязательный                   | [Режим экрана](enter-app-properties.md#display-mode) |
| **Объявления продуктов**          | Стандартно: пользователи могут установить это приложение на съемный носитель или другой диск. Windows может включать данные этого приложения в автоматическое резервное копирование в OneDrive | [Объявления продуктов](app-declarations.md) |
| **Требования к системе**      | Необязательный                                    | [Требования к системе](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Страница "Возрастные категории"

| Имя поля                    | Примечания                                       | Дополнительные сведения                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Возрастная категория**               | Обязательно                                    | [Возрастная категория](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Страница "Пакеты"

| Имя поля                    | Примечания                                  | Дополнительные сведения                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Элемент управления отправки пакета**    | Обязательно (хотя бы один пакет)        | [Отправка пакетов приложений](upload-app-packages.md) |
| **Доступность семейства устройств** | По умолчанию: зависит от пакетов       | [Доступность семейства устройств](device-family-availability.md) |
| **Постепенное развертывание пакетов**   | Необязательно (только для обновлений)            | [Постепенное развертывание пакетов](gradual-package-rollout.md) |
| **Обязательное обновление**          | Необязательно (только для обновлений)            | [Обязательное обновление](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Описания в Магазине

Все необходимые сведения потребуются по крайней мере для одного из языков, поддерживаемых вашим приложением. Мы рекомендуем предоставить [описание в Магазине](create-app-store-listings.md) на всех языках, поддерживаемых приложением. Вы также можете [предоставить описание в Магазине на дополнительных языках](create-app-store-listings.md#store-listing-languages). Чтобы упростить управление несколькими описания одного и того же продукта, вы можете [импортировать и экспортировать описания в Store](import-and-export-store-listings.md).

| Имя поля                    | Примечания                                       | Дополнительные сведения                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Описание**               | Обязательно                                    | [Напишите отличное описание приложения](write-a-great-app-description.md) |
| **Новые возможности в этой версии**   | Необязательный                                 | [Заметки о выпуске](create-app-store-listings.md#whats-new-in-this-version)       |
| **Функции приложения**              | Необязательный                                    | [Функции продукта](create-app-store-listings.md#product-features)         |
| **Снимки экрана**               | Обязательный (по крайней мере один снимок экрана; рекомендуется — 4 или более)          | [Снимки экрана](app-screenshots-and-images.md#screenshots)          |
| **Логотипы магазина**               | Рекомендуется; требуется для некоторых версий ОС | [Логотипы магазина](app-screenshots-and-images.md#store-logos)             |
| **Производим**                  | Необязательный                                    | [Производим](app-screenshots-and-images.md#trailers)                | 
| **Windows 10 и образ Xbox (16:9 супер Hero Art)**     | Рекомендуем        | [Windows 10 и образ Xbox (16:9 супер Hero Art)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Образы Xbox**     | Требуется для правильного вывода при публикации в Xbox        | [Образы Xbox](app-screenshots-and-images.md#xbox-images) |
| **Дополнительные поля**  | Необязательный                                    | [Дополнительные поля](create-app-store-listings.md#supplemental-fields) 
| **Условия поиска**              | Необязательный                                    | [Условия поиска](create-app-store-listings.md#search-terms)         |
| **Сведения об авторских правах и товарных знаках** | Необязательный                                 | [Сведения об авторских правах и товарных знаках](create-app-store-listings.md#copyright-and-trademark-info) |
| **Дополнительные условия лицензии**  | Необязательный                                    | [Дополнительные условия лицензии](create-app-store-listings.md#additional-license-terms) |
| **Создано**              | Необязательный                                    | [Создано](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Страница "Параметры отправки"

| Имя поля                    | Примечания                                       | Дополнительные сведения                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Параметры удержания публикации**     | По умолчанию: опубликуйте эту отправку, как только она пройдет сертификацию (или в соответствии с датами, выбранными в разделе "Расписание")      | [Параметры удержания публикации](manage-submission-options.md#publishing-hold-options)    
| **Примечания для сертификации**     | Рекомендуем          | [Примечания для сертификации](notes-for-certification.md)             |
| **Ограниченные возможности**     | Требуется, если ваш продукт объявляет [ограниченные возможности](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Ограниченные возможности](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Сведения о публикации бизнес-приложений (LOB) непосредственно для предприятий см. в разделе [Распространение бизнес-приложений для предприятий](distribute-lob-apps-to-enterprises.md).
