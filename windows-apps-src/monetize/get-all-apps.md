---
ms.assetid: 2BCFF687-DC12-49CA-97E4-ACEC72BFCD9B
description: Используйте этот метод в интерфейсе API отправки Microsoft Store для получения сведений о всех приложениях, зарегистрированных для учетной записи центра партнеров.
title: Получение всех приложений
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API отправки в Microsoft Store, приложения
ms.localizationpriority: medium
ms.openlocfilehash: 267e1d4de3917ae332cdfe15309f3871ef7b6647
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57641989"
---
# <a name="get-all-apps"></a>Получение всех приложений


Используйте этот метод в интерфейсе API отправки Microsoft Store для получения данных для приложений, зарегистрированных для учетной записи центра партнеров.

## <a name="prerequisites"></a>Предварительные условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | Универсальный код ресурса (URI) запроса                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

Для данного метода все параметры запроса являются необязательными. При вызове этого метода без параметров, ответ содержит данные для первых 10 приложений, зарегистрированных для учетной записи.

|  Параметр  |  Тип  |  Описание  |  Обязательно  |
|------|------|------|------|
|  top  |  int  |  Число элементов, возвращаемых в запросе (т. е., количество возвращаемых приложений). Если количество приложений в вашей учетной записи больше значения, указанного в запросе, текст ответа будет содержать относительный путь URI, который можно добавить в URI метода, чтобы запросить следующую страницу данных.  |  Нет  |
|  skip  |  int  |  Число элементов, которые требуется пропустить в запросе перед возвратом оставшихся элементов. Используйте этот параметр для постраничного перемещения по наборам данных. Например, если задано top = 10 и skip = 0, извлекаются элементы с 1 по 10; если задано top = 10 и skip = 10, извлекаются элементы с 11 по 20 и т. д.  |  Нет  |


### <a name="request-body"></a>Тело запроса

Предоставлять текст запроса для этого метода не требуется.

### <a name="request-examples"></a>Примеры запросов

В следующем примере демонстрируется способ извлечения первых 10 приложений, которые зарегистрированы в вашей учетной записи.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications HTTP/1.1
Authorization: Bearer <your access token>
```

В следующем примере демонстрируется способ извлечения информации о всех приложениях, которые зарегистрированы в вашей учетной записи. Сначала необходимо получите первые 10 приложений:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Затем рекурсивно вызываю `GET https://manage.devcenter.microsoft.com/v1.0/my/{@nextLink}` пока `{@nextlink}` имеет значение null или не существует в ответе. Например:

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```
  
```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=20&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=30&top=10 HTTP/1.1
Authorization: Bearer <your access token>
```

Если вы уже знаете, общее число приложений, у вас есть в вашей учетной записи, можно просто передать это число в **верхней** для получения информации о всех приложениях.

```http
GET https://manage.devcenter.microsoft.com/v1.0/my/applications?top=23 HTTP/1.1
Authorization: Bearer <your access token>
```


## <a name="response"></a>Ответ

В следующем примере показано тело ответа JSON, возвращаемое успешным запросом первых 10 приложений, которые зарегистрированы в учетной записи разработчика, содержащей всего 21 приложение. Для краткости в этом примере показаны данные только первых двух приложений, возвращенных в запросе. Дополнительные сведения о значениях, которые могут содержаться в теле ответа, см. в следующем разделе.

```json
{
  "@nextLink": "applications?skip=10&top=10",
  "value": [
    {
      "id": "9NBLGGH4R315",
      "primaryName": "Contoso sample app",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-11T01:32:11.0747851Z",
      "pendingApplicationSubmission": {
        "id": "1152921504621134883",
        "resourceLocation": "applications/9NBLGGH4R315/submissions/1152921504621134883"
      }
    },
    {
      "id": "9NBLGGH29DM8",
      "primaryName": "Contoso sample app 2",
      "packageFamilyName": "5224ContosoDeveloper.ContosoSampleApp2_ng6try80pwt52",
      "packageIdentityName": "5224ContosoDeveloper.ContosoSampleApp2",
      "publisherName": "CN=…",
      "firstPublishedDate": "2016-03-12T01:49:11.0747851Z",
      "lastPublishedApplicationSubmission": {
        "id": "1152921504621225621",
        "resourceLocation": "applications/9NBLGGH29DM8/submissions/1152921504621225621"
      }
      // Next 8 apps are omitted for brevity ...
    }
  ],
  "totalCount": 21
}
```

### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание                                                                                                                                                                                                                                                                         |
|------------|--------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| value      | Массив  | Массив объектов, содержащих сведения о каждом приложении, зарегистрированном в вашей учетной записи. Дополнительные сведения о данных в каждом объекте см. в разделе [Ресурс приложения](get-app-data.md#application_object).                                                                                                                           |
| @nextLink  | Строка | При наличии дополнительных страниц данных эта строка содержит относительный путь, который можно добавить к базовому URI `https://manage.devcenter.microsoft.com/v1.0/my/` запроса, чтобы запросить следующую страницу данных. Например, если для параметра *top* в тексте исходного запроса задано значение 10, но в учетной записи зарегистрировано 20 приложений, тело ответа будет содержать значение @nextLink`applications?skip=10&top=10`, которое указывает, что можно вызвать `https://manage.devcenter.microsoft.com/v1.0/my/applications?skip=10&top=10` для запроса следующих 10 приложений. |
| totalCount | int    | Общее количество строк в результирующих данных для запроса (т. е., общее число приложений, которые зарегистрированы в вашей учетной записи).                                                |


## <a name="error-codes"></a>Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 404  | Приложения не найдены. |
| 409  | Приложения использовать возможности центра партнеров, которые являются [в настоящее время не поддерживается API отправки Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>Статьи по теме

* [Создание и управление отправкой, с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Получить приложения](get-an-app.md)
* [Получить пакет авиарейсов для приложения](get-flights-for-an-app.md)
* [Получение надстроек для приложения](get-add-ons-for-an-app.md)
