---
ms.assetid: B0AD0B8E-867E-4403-9CF6-43C81F3C30CA
description: Используйте этот метод в интерфейсе API отправки Microsoft Store для получения сведений рейсов пакета для приложения, зарегистрированный для вашей учетной записи центра партнеров.
title: Получение тестовых пакетов для приложения
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, API отправки в Microsoft Store, тестируемые возможности, тестовые пакеты
ms.localizationpriority: medium
ms.openlocfilehash: 66e64f2c499835a345bb9563fd005b86a926a4d2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372017"
---
# <a name="get-package-flights-for-an-app"></a>Получение тестовых пакетов для приложения

Используйте этот метод в интерфейсе API отправки Microsoft Store, чтобы отобразить рейсы пакета для приложения, зарегистрированный для вашей учетной записи центра партнеров. Дополнительные сведения о тестовых пакетах см. в разделе [Тестовые пакеты](https://docs.microsoft.com/windows/uwp/publish/package-flights).

## <a name="prerequisites"></a>предварительные требования

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | Универсальный код ресурса (URI) запроса                                                      |
|--------|------------------------------------------------------------------|
| GET    | `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/listflights` |


### <a name="request-header"></a>Заголовок запроса

| Header        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

|  Имя  |  Тип  |  Описание  |  Обязательно  |
|------|------|------|------|
|  applicationId  |  строка  |  Код продукта в Магазине для приложения, по которому требуется получить данные о тестовых пакетах. Подробнее о коде продукта в Магазине см. в статье [Просмотр сведений об идентификации приложений](https://docs.microsoft.com/windows/uwp/publish/view-app-identity-details).  |  Да  |
|  top  |  ssNoversion  |  Число элементов, возвращаемых в запросе (т. е., количество возвращаемых тестовых пакетов). Если количество тестовых пакетов у вашей учетной записи больше значения, указанного в запросе, текст ответа будет содержать относительный путь URI, который можно добавить в URI метода, чтобы запросить следующую страницу данных.  |  Нет  |
|  skip  |  ssNoversion  |  Число элементов, которые требуется пропустить в запросе перед возвратом оставшихся элементов. Используйте этот параметр для постраничного перемещения по наборам данных. Например, если задано top = 10 и skip = 0, извлекаются элементы с 1 по 10; если задано top = 10 и skip = 10, извлекаются элементы с 11 по 20 и т. д.  |  Нет  |


### <a name="request-body"></a>Тело запроса

Предоставлять текст запроса для этого метода не требуется.

### <a name="request-examples"></a>Примеры запросов

В следующем примере показано, как получить список всех тестовых пакетов для приложения.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights HTTP/1.1
Authorization: Bearer <your access token>
```

В следующем примере показано, как получить список, содержащий первый тестовый пакет для приложения.

```json
GET https://manage.devcenter.microsoft.com/v1.0/my/applications/9NBLGGH4R315/listflights?top=1 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

В следующем примере показано тело ответа JSON, возвращаемое успешным запросом первого тестового пакета для приложения, содержащего три тестовых пакета. Дополнительные сведения о значениях, которые могут содержаться в теле ответа, см. в следующем разделе.

```json
{
  "value": [
    {
      "flightId": "7bfc11d5-f710-47c5-8a98-e04bb5aad310",
      "friendlyName": "myflight",
      "lastPublishedFlightSubmission": {
        "id": "1152921504621086517",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621086517"
      },
      "pendingFlightSubmission": {
        "id": "1152921504621215786",
        "resourceLocation": "flights/7bfc11d5-f710-47c5-8a98-e04bb5aad310/submissions/1152921504621215786"
      },
      "groupIds": [
        "1152921504606962205"
      ],
      "rankHigherThan": "Non-flighted submission"
    }
  ],
  "totalCount": 3
}
```

### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание       |
|------------|--------|---------------------|
| @nextLink  | строка | При наличии дополнительных страниц данных эта строка содержит относительный путь, который можно добавить к базовому URI `https://manage.devcenter.microsoft.com/v1.0/my/` запроса, чтобы запросить следующую страницу данных. Например, если для параметра *top* в тексте исходного запроса задано значение 2, но приложение содержит 4 тестовых пакета, тело ответа будет содержать значение @nextLink`applications/{applicationid}/listflights/?skip=2&top=2`, которое указывает, что можно вызвать `https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationid}/listflights/?skip=2&top=2` для запроса следующих 2 тестовых пакетов. |
| value      | Массив  | Массив объектов, которые предоставляют сведения о тестовых пакетах для указанного приложения. Дополнительные сведения о данных в каждом объекте см. в разделе [Ресурс тестируемой возможности](get-app-data.md#flight-object).               |
| totalCount | ssNoversion    | Общее количество строк в данных результата запроса (т. е., общее количество тестовых пакетов для указанного приложения).   |


## <a name="error-codes"></a>Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 404  | Тестовые пакеты не найдены. |
| 409  | Приложение использует функцию центра партнеров, которая [в настоящее время не поддерживается API отправки Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported).  |


## <a name="related-topics"></a>См. также

* [Создание и управление отправкой, с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Получить все приложения](get-all-apps.md)
* [Получить приложения](get-an-app.md)
* [Получение надстроек для приложения](get-add-ons-for-an-app.md)
