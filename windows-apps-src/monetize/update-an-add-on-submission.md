---
author: mcleanbyron
ms.assetid: 8C63D33B-557D-436E-9DDA-11F7A5BFA2D7
description: "Используйте этот метод в API отправки Магазина Windows для обновления существующей отправки надстройки."
title: "Обновление отправки надстройки с помощью API отправки Магазина Windows"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: b7a8e1d39d5ee0a0858382b84ab00cc2c481da5d

---

# Обновление отправки надстройки с помощью API отправки Магазина Windows




Используйте этот метод в API отправки Магазина Windows для обновления данных для существующей отправки надстройки (также называется внутренним продуктом приложения или IAP). После успешного обновления отправки с помощью этого метода необходимо [зафиксировать отправку](commit-an-add-on-submission.md) для проверки и публикации.

Дополнительные сведения об использовании этого метода в процессе создания отправки надстройки с помощью API отправки Магазина Windows см. в разделе [Управление отправками надстроек](manage-add-on-submissions.md).

## Необходимые условия

Для использования этого метода необходимо выполнить следующие действия:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки Магазина Windows.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.
* Создайте отправку надстройки для приложения в учетной записи центра разработки. Это можно сделать на информационной панели центра разработки или с помощью метода [Создание отправки надстройки](create-an-add-on-submission.md).

>**Примечание.**&nbsp;&nbsp;Этот метод можно использовать только для учетных записей Центра разработки для Windows, у которых есть разрешение на использование API отправки Магазина Windows. Такое разрешение имеется не у всех учетных записей.

## Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | URI запроса                                                      |
|--------|------------------------------------------------------------------|
| PUT    | ```https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId} ``` |

<span/>
 

### Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | строка | Обязательный. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |

<span/>

### Параметры запроса

| Имя        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | строка | Обязательный. Код продукта в Магазине для надстройки, отправку которой необходимо обновить. Этот код отображается на панели мониторинга Центра разработки, а также добавляется в данные ответов для запросов на [Создание надстройки](create-an-add-on.md) или [Получение сведений о надстройке](get-all-add-ons.md).  |
| submissionId | строка | Обязательный. Идентификатор отправки для обновления. Этот идентификатор отображается на панели мониторинга Центра разработки, а также добавляется в данные ответов для запросов на [Создание отправки надстройки](create-an-add-on-submission.md).  |

<span/>

### Тело запроса

Тело запроса содержит следующие параметры.

| Value      | Type   | Description                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| contentType           | строка  |  [Тип содержимого](../publish/enter-iap-properties.md#content-type), которое предоставляется в надстройке. Может принимать одно из следующих значений. <ul><li>NotSet (Не задано)</li><li>BookDownload (Загрузка книги)</li><li>EMagazine (Электронный журнал)</li><li>ENewspaper (Электронная газета)</li><li>MusicDownload (Загрузка музыки)</li><li>MusicStream (Потоковая передача музыки)</li><li>OnlineDataStorage (Сетевое хранилище данных)</li><li>VideoDownload (Загрузка видео)</li><li>VideoStream (Потоковая передача видео)</li><li>Asp</li><li>OnlineDownload (Загрузка по Интернету)</li></ul> |  
| keywords           | массив  | Массив строк, содержащих до 10 [ключевых слов](../publish/enter-iap-properties.md#keywords) для надстройки. Приложение может запрашивать надстройки с помощью этих ключевых слов.   |
| lifetime           | строка  |  Время существования надстройки. Может принимать одно из следующих значений. <ul><li>Forever (Навсегда)</li><li>OneDay (Один день)</li><li>ThreeDays (3 дня)</li><li>FiveDays (5 дней)</li><li>OneWeek (Одна неделя)</li><li>TwoWeeks (Две недели)</li><li>OneMonth (Один месяц)</li><li>TwoMonths (Два месяца)</li><li>ThreeMonths (Три месяца)</li><li>SixMonths (Шесть месяцев)</li><li>OneYear (Один год)</li></ul> |
| listings           | объект  | Объект, содержащий сведения об описании надстройки. Дополнительные сведения см. в разделе [Ресурс описания](manage-add-on-submissions.md#listing-object).  |
| pricing           | объект  | Объект, содержащий сведения о цене надстройки. Дополнительные сведения см. в разделе [Ресурс цены](manage-add-on-submissions.md#pricing-object).  |
| targetPublishMode           | строка  | Режим публикации для отправки. Может принимать одно из следующих значений. <ul><li>Immediate (Незамедлительно)</li><li>Manual (Вручную)</li><li>SpecificDate (Указанная дата)</li></ul> |
| targetPublishDate           | строка  | Дата публикации отправки в формате ISO 8601, если для *targetPublishMode* задано значение SpecificDate.  |
| tag           | строка  |  [Тег](../publish/enter-iap-properties.md#tag) для надстройки.   |
| visibility  | строка  |  Видимость надстройки. Может принимать одно из следующих значений. <ul><li>Hidden (Скрыто)</li><li>Public (Общее)</li><li>Private (Частное)</li><li>NotSet (Не задано)</li></ul>  |

<span/>

### Пример запроса

В следующем примере показано обновление отправки надстройки.

```json
PUT https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
Content-Type: application/json
{
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
}
```

## Ответ

В следующем примере представлен текст ответа JSON в случае успешного вызова этого метода. В теле ответа содержатся сведения об обновленной отправке. Дополнительные сведения о значениях в тексте ответа см. в разделе [Ресурс отправки надстройки](manage-add-on-submissions.md#add-on-submission-object).

```json
{
  "id": "1152921504621243680",
  "contentType": "EMagazine",
  "keywords": [
    "books"
  ],
  "lifetime": "FiveDays",
  "listings": {
    "en": {
      "description": "English add-on description",
      "icon": {
        "fileName": "add-on-en-us-listing2.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (English)"
    },
    "ru": {
      "description": "Russian add-on description",
      "icon": {
        "fileName": "add-on-ru-listing.png",
        "fileStatus": "Uploaded"
      },
      "title": "Add-on Title (Russian)"
    }
  },
  "pricing": {
    "marketSpecificPricings": {
      "RU": "Tier3",
      "US": "Tier4",
    },
    "sales": [
      {
         "name": "Sale1",
         "basePriceId": "Free",
         "startDate": "2016-05-21T18:40:11.7369008Z",
         "endDate": "2016-05-22T18:40:11.7369008Z",
         "marketSpecificPricings": {
            "RU": "NotAvailable"
         }
      }
    ],
    "priceId": "Free"
  },
  "targetPublishDate": "2016-03-15T05:10:58.047Z",
  "targetPublishMode": "Immediate",
  "tag": "SampleTag",
  "visibility": "Public",
  "status": "PendingCommit",
  "statusDetails": {
    "errors": [
      {
        "code": "None",
        "details": "string"
      }
    ],
    "warnings": [
      {
        "code": "ListingOptOutWarning",
        "details": "You have removed listing language(s): []"
      }
    ],
    "certificationReports": [
      {
      }
    ]
  },
  "fileUploadUrl": "https://productingestionbin1.blob.core.windows.net/ingestion/26920f66-b592-4439-9a9d-fb0f014902ec?sv=2014-02-14&sr=b&sig=usAN0kNFNnYE2tGQBI%2BARQWejX1Guiz7hdFtRhyK%2Bog%3D&se=2016-06-17T20:45:51Z&sp=rwl",
  "friendlyName": "Submission 2"
}
```

## Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 400  | Не удалось обновить отправку. Недопустимый запрос. |
| 409  | Не удалось обновить отправку из-за текущего состояния надстройки или в связи с тем, что надстройка использует компонент информационной панели Центра разработки, [который в настоящее время не поддерживается API отправки Магазина Windows](create-and-manage-submissions-using-windows-store-services.md#not_supported). |   

<span/>


## Связанные статьи

* [Создание отправок и управление ими с помощью служб Магазина Windows](create-and-manage-submissions-using-windows-store-services.md)
* [Управление отправками надстроек](manage-add-on-submissions.md)
* [Получение отправки надстройки](get-an-add-on-submission.md)
* [Создание отправки надстройки](create-an-add-on-submission.md)
* [Подтверждение отправки надстройки](commit-an-add-on-submission.md)
* [Удаление отправки надстройки](delete-an-add-on-submission.md)
* [Получение состоянии отправки надстройки](get-status-for-an-add-on-submission.md)



<!--HONumber=Aug16_HO5-->

