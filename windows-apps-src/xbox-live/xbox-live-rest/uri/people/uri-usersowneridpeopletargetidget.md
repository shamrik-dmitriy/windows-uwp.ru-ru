---
title: GET (/users/{ownerId}/people/{targetid})
assetID: 2fd37b8e-b886-14f2-3399-59f530d85e4e
permalink: en-us/docs/xboxlive/rest/uri-usersowneridpeopletargetidget.html
description: " GET (/users/{ownerId}/people/{targetid})"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, игры, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 408b4df30f53e27b04e2a1e654e9686d2b359637
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632679"
---
# <a name="get-usersowneridpeopletargetid"></a>GET (/users/{ownerId}/people/{targetid})
Получает пользователя по Идентификатору целевой объект из коллекции людей вызывающей стороны. Доменом для таких URI является `social.xboxlive.com`.
 
  * ["Примечания"](#ID4EV)
  * [Параметры URI](#ID4E5)
  * [Авторизации](#ID4EJB)
  * [Требуемые заголовки запросов](#ID4ERC)
  * [Необязательные заголовки запросов](#ID4EQD)
  * [Текст запроса](#ID4EWE)
  * [Коды состояния HTTP](#ID4EBF)
  * [Заголовки ответа требуется](#ID4EDH)
  * [Текст ответа](#ID4EQAAC)
 
<a id="ID4EV"></a>

 
## <a name="remarks"></a>Замечания
 
Операции GET не будет изменять любые ресурсы, поэтому это будет дают одинаковые результаты, если выполняется один раз или несколько раз.
  
<a id="ID4E5"></a>

 
## <a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)
 
| Параметр| Тип| Описание| 
| --- | --- | --- | 
| ownerId| Строка| Идентификатор пользователя, ресурсом которого осуществляется. Должно соответствовать авторизованного пользователя. Возможными значениями являются «me», xuid({xuid}) или gt({gamertag}).| 
| targetID| Строка| Идентификатор пользователя, в которых данные извлекаются из списка людей владельца, идентификатор пользователя Xbox (XUID) или тег игрока. Примеры значений: xuid(2603643534573581), gt(SomeGamertag).| 
  
<a id="ID4EJB"></a>

 
## <a name="authorization"></a>Authorization
 
| Тип| Обязательно| Описание| Ответ, если он отсутствует| 
| --- | --- | --- | --- | --- | --- | --- | 
| XUID| Да| Вызывающий оператор имеет идентификатор пользователя пользователя Xbox (XUID).| 401 Нет доступа| 
  
<a id="ID4ERC"></a>

 
## <a name="required-request-headers"></a>Требуемые заголовки запросов
 
| Заголовок| Описание| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Authorization| Строка. Данные авторизации для Xbox LIVE. Обычно это зашифрованный маркер XSTS. Пример значения: <b>XBL3.0 x =&lt;userhash >;&lt; Token ></b>.| 
  
<a id="ID4EQD"></a>

 
## <a name="optional-request-headers"></a>Необязательные заголовки запросов
 
| Заголовок| Описание| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| X-RequestedServiceVersion| Имя или номер службы Xbox LIVE, к которому следует направлять этот запрос сборки. Запрос будет перенаправлен только к этой службе после проверки допустимости заголовок, утверждения в маркер проверки подлинности и т. д. Значение по умолчанию: 1.| 
| Принять| Строка. Типы содержимого, которые вызывающий объект принимает в ответе. Все ответы принимаются <b>application/json</b>.| 
  
<a id="ID4EWE"></a>

 
## <a name="request-body"></a>Тело запроса
 
Объекты не будут отправлены в тексте этого запроса.
  
<a id="ID4EBF"></a>

 
## <a name="http-status-codes"></a>Коды состояния HTTP
 
Служба возвращает один из кодов состояния, в этом разделе, в ответ на запрос, сделанный с помощью этого метода по данному ресурсу. Полный список стандартных кодов состояния HTTP, используемых со службами Xbox Live Services, см. в разделе [коды состояния HTTP стандартный](../../additional/httpstatuscodes.md).
 
| Код| Фраза причины| Описание| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 200| ОК| Успешное выполнение.| 
| 400| Неверный запрос| Идентификаторы пользователей были имеет неправильный формат.| 
| 403| Запрещено| Не удалось проанализировать XUID утверждения из заголовка авторизации.| 
| 404| Не найден| Целевой пользователь не найден в списке людей владельца.| 
  
<a id="ID4EDH"></a>

 
## <a name="required-response-headers"></a>Заголовки ответа требуется
 
| Заголовок| Тип| Описание| 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| Content-Length| 32-разрядного целого числа без знака| Длина текста ответа в байтах. Пример значения: 22.| 
| Content-Type| Строка| Тип MIME тела ответа. Это значение всегда равно <b>application/json</b>.| 
  
<a id="ID4EQAAC"></a>

 
## <a name="response-body"></a>Тело ответа
 
Если вызов прошел успешно, то служба возвращает целевого пользователя. См. в разделе [Person (JSON)](../../json/json-person.md).
 
<a id="ID4E3AAC"></a>

 
### <a name="sample-response"></a>Пример ответа
 

```cpp
{
    "xuid": "2603643534573581",
    "isFavorite": false,
    "isFollowingCaller": false,
    "socialNetworks": ["LegacyXboxLive"]
}
         
```

   
<a id="ID4EGBAC"></a>

 
## <a name="see-also"></a>См. также
 
<a id="ID4EIBAC"></a>

 
##### <a name="parent"></a>Parent 

[/users/{ownerId}/people/{targetid}](uri-usersowneridpeopletargetid.md)

   