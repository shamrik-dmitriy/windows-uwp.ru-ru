---
description: Используйте этот метод в API аналитики для Microsoft Store для получения сводных данных о конверсиях по каждому каналу для приложения в заданном диапазоне дат или с учетом других дополнительных фильтров.
title: Получение преобразований приложения по каналу
ms.date: 08/04/2017
ms.topic: article
keywords: windows 10, uwp, службы Store, API аналитики для Microsoft Store, конверсии приложения, канал
ms.localizationpriority: medium
ms.openlocfilehash: d196ffeebcda7653e7464358b772def48c17cefb
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624269"
---
# <a name="get-app-conversions-by-channel"></a>Получение преобразований приложения по каналу

Используйте этот метод в API аналитики для Microsoft Store для получения сводных сведений о конверсиях по каждому каналу для приложения в заданном диапазоне дат или с учетом других дополнительных фильтров.

* *Конверсия* означает, что пользователь (выполнивший вход с помощью учетной записи Майкрософт) получил новую лицензию на ваше приложение (независимо от того, берете ли вы за это деньги или предоставляете лицензию бесплатно).
* *Канал* — это способ, с использованием которого пользователь оказался на странице описания вашего приложения (например, через Магазин или [настраиваемую кампанию по продвижению приложения](../publish/create-a-custom-app-promotion-campaign.md)).

Эта информация также доступна в [отчет о приобретениях](../publish/acquisitions-report.md#app-page-views-and-conversions-by-channel) в центре партнеров.

## <a name="prerequisites"></a>Предварительные условия


Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Microsoft Store.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | Универсальный код ресурса (URI) запроса       |
|--------|----------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions``` |


### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

| Параметр        | Тип   |  Описание      |  Обязательно  
|---------------|--------|---------------|------|
| applicationId | Строка | [Код продукта в Магазине](in-app-purchases-and-trials.md#store-ids) для приложения, по которому требуется получить данные о конверсиях. Пример кода продукта в Магазине: 9WZDNCRFJ3Q8. |  Да  |
| startDate | date | Начальная дата диапазона дат, для которого требуется получить данные о конверсиях. Значение по умолчанию: 1/1/2016. |  Нет  |
| endDate | date | Конечная дата диапазона дат, для которого требуется получить данные о конверсиях. По умолчанию используется текущая дата. |  Нет  |
| top | int | Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку "Далее", которую можно использовать для запроса следующей страницы данных |  Нет  |
| skip | int | Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10000 и skip=0 извлекаются первые 10 000 строк данных; при top=10000 и skip=10000 извлекаются следующие 10 000 строк данных и т. д. |  Нет  |
| filter | Строка  | Одно или несколько выражений для фильтрации текста ответа. Каждый оператор может использовать операторы **eq** или **ne**; кроме того, операторы можно объединять с помощью **и** или **или**. Можно указать следующие строки в инструкциях фильтра. Описания см. в разделе [значения конверсии](#conversion-values) в этой статье. <ul><li><strong>ApplicationName</strong></li><li><strong>Тип</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>клиентам магазинов</strong></li><li><strong>типа устройства</strong></li><li><strong>на рынке</strong></li></ul><p>Вот пример параметра *filter*: <em>filter=deviceType eq 'PC'</em>.</p> | Нет   |
| aggregationLevel | Строка | Определяет диапазон времени, для которого требуется получить сводные данные. Можно использовать следующие строки: <strong>day</strong>, <strong>week</strong> или <strong>month</strong>. Если параметр не задан, значение по умолчанию — <strong>day</strong>. | Нет |
| orderby | Строка | Оператор, который определяет порядок полученных значений данных для каждой конверсии. Используется следующий синтаксис: <em>orderby=field [порядк.],field [порядк.],...</em>. Параметр <em>field</em> может быть одной из следующих строк:<ul><li><strong>Дата</strong></li><li><strong>ApplicationName</strong></li><li><strong>Тип</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>клиентам магазинов</strong></li><li><strong>типа устройства</strong></li><li><strong>на рынке</strong></li></ul><p>Параметр <em>order</em> является необязательным и может принимать значения <strong>asc</strong> или <strong>desc</strong>, которые указывают, соответственно, порядок сортировки по возрастанию или по убыванию для каждого поля. Значение по умолчанию — <strong>asc</strong>.</p><p>Пример: строка <em>orderby</em>: <em>orderby=date,market</em></p> |  Нет  |
| groupby | Строка | Оператор, который применяет агрегирование данных только к указанным полям. Можно указать следующие поля:<ul><li><strong>Дата</strong></li><li><strong>ApplicationName</strong></li><li><strong>Тип</strong></li><li><strong>customCampaignId</strong></li><li><strong>referrerUriDomain</strong></li><li><strong>channelType</strong></li><li><strong>клиентам магазинов</strong></li><li><strong>типа устройства</strong></li><li><strong>на рынке</strong></li></ul><p>Возвращенные строки данных будут содержать поля, указанные в параметре <em>groupby</em>, а также:</p><ul><li><strong>Дата</strong></li><li><strong>Идентификатор приложения</strong></li><li><strong>conversionCount</strong></li><li><strong>clickCount</strong></li></ul><p>Параметр <em>groupby</em> можно использовать вместе с параметром <em>aggregationLevel</em>. Например: <em>groupby=ageGroup,market&amp;aggregationLevel=week</em></p> |  Нет  |


### <a name="request-example"></a>Пример запроса

В следующем примере демонстрируются несколько запросов на получение информации о конверсиях приложения. Замените значение *applicationId* кодом продукта в Магазине для вашего приложения.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=2/1/2017&top=10&skip=0  HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/appchannelconversions?applicationId=9NBLGGGZ5QDR&startDate=1/1/2017&endDate=4/31/2017&skip=0&filter=market eq 'US'  HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип   | Описание                  |
|------------|--------|-------------------------------------------------------|
| Значение      | Массив  | Массив объектов, содержащий сводную информацию о конверсиях для данного приложения. Дополнительные сведения о данных в каждом объекте см. далее в разделе [Значения конверсии](#conversion-values).                                                                                                                      |
| @nextLink  | Строка | При наличии дополнительных страниц данных эта строка содержит универсальный код ресурса (URI), который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10 000, но для данного запроса имеется больше 10 000 строк с информацией о конверсиях. |
| TotalCount | int    | Общее количество строк в результирующих данных для запроса.      |


### <a name="conversion-values"></a>Значения конверсии

Объекты в массиве *Value* содержат следующие значения.

| Значение               | Тип   | Описание                           |
|---------------------|--------|-------------------------------------------|
| date                | Строка | Первая дата в диапазоне дат, для которого требуется получить данные о конверсиях. Если в запросе указан один день, это значение равно соответствующей дате. Если запрос указывает неделю, месяц или другой диапазон дат, это значение равно первой дате в этом диапазоне дат |
| applicationId       | Строка | [Код продукта в Магазине](in-app-purchases-and-trials.md#store-ids) для приложения, по которому запрашиваются данные о конверсиях.     |
| applicationName     | Строка | Отображаемое название приложения, по которому запрашиваются данные о конверсиях.        |
| appType          | Строка |  Тип продукта, для которого запрашиваются данные о конверсиях. Для этого метода единственным поддерживаемым значением является **App**.            |
| customCampaignId           | Строка |  Строка идентификатора для [пользовательской кампании по продвижению приложения](../publish/create-a-custom-app-promotion-campaign.md), связанной с этим приложением.   |
| referrerUriDomain           | Строка |  Указывает домен, на котором было активировано описание приложения с идентификатором настраиваемой кампании по продвижению приложения.   |
| channelType           | Строка |  Одно из следующих строковых значений, определяющих канал конверсии:<ul><li><strong>customCampaignId</strong></li><li><strong>Трафик Store</strong></li><li><strong>Другие</strong></li></ul>    |
| storeClient         | Строка | Версия Магазина, в котором произведена конверсия. На данный момент единственным поддерживаемым значением является **SFC**.    |
| deviceType          | Строка | Одна из следующих строк:<ul><li><strong>ПК</strong></li><li><strong>Телефон</strong></li><li><strong>Консоль</strong></li><li><strong>Интернета вещей</strong></li><li><strong>Holographic</strong></li><li><strong>Неизвестный</strong></li></ul>            |
| market              | Строка | Код страны по стандарту ISO 3166 для рынка, на котором произошла конверсия.    |
| clickCount              | number  |     Число пользователей, щелкнувших по ссылке на описание вашего приложения.      |           
| conversionCount            | number  |   Количество конверсий пользователей.         |          


### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "date": "2016-01-01",
      "applicationId": "9NBLGGGZ5QDR",
      "applicationName": "Contoso App",
      "appType": "App",
      "customCampaignId": "",
      "referrerUriDomain": "Universal Client Store",
      "channelType": "Store Traffic",
      "storeClient": "SFC",
      "deviceType": "PC",
      "market": "US",
      "clickCount": 7,
      "conversionCount": 0
    }
  ],
  "@nextLink": null,
  "TotalCount": 1
}
```

## <a name="related-topics"></a>Статьи по теме

* [Отчет о приобретениях](../publish/acquisitions-report.md)
* [Доступ к данным аналитики с помощью служб Microsoft Store](access-analytics-data-using-windows-store-services.md)
* [Получить приобретения приложений](get-app-acquisitions.md)
