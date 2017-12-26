---
author: mcleanbyron
ms.assetid: BAC79A64-445D-4702-8E96-7727FF180245
description: "Используйте этот метод в API аналитики для Магазина Windows, чтобы получить сводные данные отчетов об ошибках драйверов для Windows10 в заданном диапазоне дат или с учетом других дополнительных фильтров. Этот метод предназначен только для независимых поставщиков оборудования."
title: "Получение данных системы отчетов об ошибках для драйверов для Windows10"
ms.author: mcleans
ms.date: 03/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP, службы Магазина, API аналитики для Магазина Windows, ошибки"
ms.openlocfilehash: ea2465feda869a07eef06b57a5ca8b4e04cf99f1
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="get-error-reporting-data-for-windows-10-drivers"></a>Получение данных системы отчетов об ошибках для драйверов для Windows10

Используйте этот метод в API аналитики для Магазина Windows, чтобы получить сводные данные отчетов об ошибках драйверов для Windows10 в заданном диапазоне дат или с учетом других дополнительных фильтров. Вы можете получить дополнительные сведения об ошибках с помощью методов [получения сведений об ошибке драйвера для Windows10](get-details-for-a-windows-10-driver-error.md).

> [!NOTE]
> Этот метод может использоваться только в учетных записях разработчиков, связанных с [программой Центра разработки оборудования для Windows](https://msdn.microsoft.com/windows/hardware/drivers/dashboard/get-started-with-the-hardware-dashboard).

## <a name="prerequisites"></a>Необходимые условия

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](access-analytics-data-using-windows-store-services.md#prerequisites) для API аналитики для Магазина Windows.
* [Получите маркер доступа Azure AD](access-analytics-data-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения маркера доступа у вас будет 60минут, чтобы использовать его до окончания срока действия маркера. После истечения срока действия маркера можно получить новый маркер.

## <a name="request"></a>Запрос


### <a name="request-syntax"></a>Синтаксис запроса

| Метод | URI запроса                                                          |
|--------|----------------------------------------------------------------------|
| GET    | ```https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits``` |

<span/> 

### <a name="request-header"></a>Заголовок запроса

| Заголовок        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | Строка | Обязательное. Маркер доступа Azure AD в формате **Bearer** &lt;*token*&gt;. |

<span/> 

### <a name="request-parameters"></a>Параметры запроса

| Параметр        | Тип   |  Описание      |  Обязательный  
|---------------|--------|---------------|------|
| applicationId   | string  | Значение кода продукта для драйвера, для которого требуется получить данные об ошибках. | Да  |
| startDate | date | Начальная дата диапазона дат, для которого требуется получить данные отчетов об ошибках. По умолчанию используется текущая дата |  Нет  |
| endDate | date | Конечная дата диапазона дат, для которого требуется получить данные отчетов об ошибках. По умолчанию используется текущая дата |  Нет  |
| top | целое число | Количество строк данных, возвращаемых в запросе. Максимальное значение и значение по умолчанию (если параметр не указан) — 10 000. Если в запросе содержится больше строк, то тело ответа будет содержать ссылку «Далее», которую можно использовать для запроса следующей страницы данных |  Нет  |
| skip | int | Количество строк, пропускаемых в запросе. Используйте этот параметр для постраничного перемещения по большим наборам данных. Например, при top=10000 и skip=0 извлекаются первые 10 000 строк данных; при top=10000 и skip=10000 извлекаются следующие 10 000 строк данных и т. д. |  Нет  |
| filter |строка  | Одно или несколько выражений для фильтрации строк в ответе. Каждое выражение содержит имя поля из тела ответа и значение, которое связано с помощью операторов **eq** или **ne**; выражения можно комбинировать, используя операторы **and** или **or**. В параметре *filter* строковые значения должны быть заключены в одиночные кавычки. Вы можете указать следующие поля из тела ответа:<p/><ul><li><strong>date,</strong></li><li><strong>submissionId,</strong></li><li><strong>failureName,</strong></li><li><strong>failureHash,</strong></li><li><strong>symbol,</strong></li><li><strong>osVersion,</strong></li><li><strong>osName,</strong></li><li><strong>eventType,</strong></li><li><strong>market,</strong></li><li><strong>deviceType,</strong></li><li><strong>driverName,</strong></li><li><strong>driverVersion,</strong></li><li><strong>oemName,</strong></li><li><strong>oemModel,</strong></li><li><strong>flightRing,</strong></li><li><strong>architecture.</strong></li></ul> | Нет   |
| aggregationLevel | строка | Определяет диапазон времени, для которого требуется получить сводные данные. Можно использовать следующие строки: <strong>day</strong>, <strong>week</strong> или <strong>month</strong>. Если параметр не задан, значение по умолчанию — <strong>day</strong>. Если указать <strong>week</strong> или <strong>month</strong>, значения <em>failureName</em> и <em>failureHash</em> будут ограничены 1000 сегментов. | Нет |
| orderby | строка | Выражение, которое определяет порядок полученных значений данных. Используется следующий синтаксис: <em>orderby=field [order],field [order],...</em>. Вы можете указать следующие поля из тела ответа:<p/><ul><li><strong>date,</strong></li><li><strong>submissionId,</strong></li><li><strong>failureName,</strong></li><li><strong>failureHash,</strong></li><li><strong>symbol,</strong></li><li><strong>osVersion,</strong></li><li><strong>osName,</strong></li><li><strong>eventType,</strong></li><li><strong>market,</strong></li><li><strong>deviceType,</strong></li><li><strong>driverName,</strong></li><li><strong>driverVersion,</strong></li><li><strong>oemName,</strong></li><li><strong>oemModel,</strong></li><li><strong>flightRing,</strong></li><li><strong>architecture.</strong></li></ul><p>Параметр <em>order</em> является необязательным и может принимать значения <strong>asc</strong> или <strong>desc</strong>, которые указывают, соответственно, порядок сортировки по возрастанию или по убыванию для каждого поля. Значение по умолчанию — <strong>asc</strong>.</p><p>Пример: строка <em>orderby</em>: <em>orderby=date,market</em></p> |  Нет  |
| groupby | строка | Выражение, которое применяет агрегирование данных только к указанным полям. Вы можете указать следующие поля из тела ответа:<p/><ul><li><strong>submissionId,</strong></li><li><strong>failureName,</strong></li><li><strong>failureHash,</strong></li><li><strong>symbol,</strong></li><li><strong>osVersion,</strong></li><li><strong>eventType,</strong></li><li><strong>market,</strong></li><li><strong>deviceType,</strong></li><li><strong>driverName,</strong></li><li><strong>driverVersion,</strong></li><li><strong>oemName,</strong></li><li><strong>oemModel,</strong></li><li><strong>flightRing,</strong></li><li><strong>architecture.</strong></li></ul><p>Возвращенные строки данных будут содержать поля, указанные в параметре <em>groupby</em>, а также:</p><ul><li><strong>date,</strong></li><li><strong>applicationId,</strong></li><li><strong>eventCount.</strong></li></ul><p>Параметр <em>groupby</em> можно использовать вместе с параметром <em>aggregationLevel</em>. Например: <em>&amp;groupby=failureName,market&amp;aggregationLevel=week</em></p></p> |  Нет  |

<span/>


### <a name="request-example"></a>Пример запроса

В следующих примерах показано несколько запросов на получение данных отчетов об ошибках оборудования OEM.

```syntax
GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=1/1/2015&endDate=2/1/2015&top=10&skip=0 HTTP/1.1
Authorization: Bearer <your access token>

GET https://manage.devcenter.microsoft.com/v1.0/my/analytics/driver/failurehits?applicationId=18430592857500421&startDate=8/1/2015&endDate=8/31/2015&skip=0&filter=market eq 'US' and deviceType eq 'PC' HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ


### <a name="response-body"></a>Тело ответа

| Значение      | Тип    | Описание     |
|------------|---------|--------------|
| Value      | array   | Массив объектов, содержащий сводные данные отчетов об ошибках. Дополнительные сведения о данных в каждом объекте см. в следующей таблице.     |
| @nextLink  | string  | При наличии дополнительных страниц данных эта строка содержит URI-адрес, который можно использовать для запроса следующей страницы данных. Например, это значение возвращается в том случае, если параметр **top** запроса имеет значение 10 000, но для данного запроса имеется больше 10 000 строк с информацией об ошибках |
| TotalCount | inumber | Общее количество строк в результирующих данных для запроса.     |

<span/>

Элементы в массиве *Value* содержат следующие значения.

| Значение           | Тип    | Описание        |
|-----------------|---------|---------------------|
| date            | строка  | Первая дата в диапазоне дат, для которого требуется получить данные об ошибках. Если в запросе указан один день, это значение равно соответствующей дате. Если запрос указывает неделю, месяц или другой диапазон дат, это значение равно первой дате в этом диапазоне дат |
| applicationId   | string  | Значение кода продукта для драйвера, для которого требуется извлечь данные об ошибках. |
| submissionId   | string  | Идентификатор отправки, связанный с этим драйвером. |
| failureName     | строка  | Имя ошибки  |
| failureHash     | строка  | Уникальный идентификатор ошибки   |
| symbol          | string  | Символ, назначенный этой ошибке |
| osVersion       | string  | Версия сборки из четырех частей для операционной системы, в которой произошла ошибка.  |
| osName       | string  | Название ОС, в которой произошла ошибка.  |
| eventType       | string  | Тип возникшей ошибки.      |
| market          | строка  | Код страны рынка устройства по стандарту ISO 3166   |
| deviceType      | string  | Одна из следующих строк, указывающая тип устройства, на котором произошла ошибка:<p/><ul><li><strong>PC (компьютер),</strong></li><li><strong>Phone (телефон),</strong></li><li><strong>Console (консоль),</strong></li><li><strong>IoT (Интернет вещей),</strong></li><li><strong>Holographic (голография),</strong></li><li><strong>Unknown (неизвестно).</strong></li></ul>    |
| driverName     | string  | Уникальное имя драйвера, связанного с этой ошибкой.      |
| driverVersion  | string  | Версия драйвера, связанного с этой ошибкой.   |
| architecture | string |  Архитектура устройства, в котором произошла ошибка.  |
| oemName | string | Имя производителя устройства, в котором произошла ошибка. |
| oemModel | string | Название модели устройства, в котором произошла ошибка. |
| flightRing | string | Название тестируемой возможности ОС, в которой произошла ошибка. |
| eventCount      | inumber | Количество событий, которые вызваны этой ошибкой, на указанном уровне агрегирования.      |

<span/> 

### <a name="response-example"></a>Пример ответа

В следующем примере демонстрируется пример тела ответа JSON на данный запрос.

```json
{
  "Value": [
    {
      "date": "2017-02-26",
      "submissionId":"29989500_13736592797500435_1152921504626321174",
      "applicationId":"18430592857500421",
      "driverName": "fastboot.sys",
      "osVersion": "10.0.14393.693",
      "osName": "Windows 10",
      "flightRing": "Windows 10 Retail",
      "oemName": "Contoso",
      "oemModel": "C-122",
      "market": "US",
      "symbol": "fastboot",
      "failureName": "AV_fastboot!unknown_function",
      "failureHash": "f060c0b6-fbe6-463f-a9f1-b7ebc1cc722f",
      "architecture": "x64",
      "driverVersion": "2.1.1.0",
      "deviceType": "Unknown",
      "eventType": "System crash",
      "deviceCount": 1.0,
      "eventCount": 1.0
    }]
}
```

## <a name="related-topics"></a>Связанные разделы

* [Получение сведений об ошибке драйвера для Windows10](get-details-for-a-windows-10-driver-error.md)
* [Скачивание CAB-файла для ошибки драйвера для Windows10](download-the-cab-file-for-a-windows-10-driver-error.md)