---
title: /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
assetID: f7de98c3-e6d1-2c40-00f0-d45e418af8bf
permalink: en-us/docs/xboxlive/rest/uri-untrustedplatformusersxuidscidssciddatapathandfilenametype.html
description: " /untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, игры, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: cdb1d9d96d28e5aadc9c017f6f13e51bdf066f73
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656289"
---
# <a name="untrustedplatformusersxuidxuidscidssciddatapathandfilenametype"></a>/untrustedplatform/users/xuid({xuid})/scids/{scid}/data/{pathAndFileName},{type}
Загружает и выгружает удаляет файл. Доменом для таких URI является `titlestorage.xboxlive.com`.
 
  * [Параметры URI](#ID4EV)
 
<a id="ID4EV"></a>

 
## <a name="uri-parameters"></a>Параметры универсального кода ресурса (URI)
 
| Параметр| Тип| Описание| 
| --- | --- | --- | 
| xuid| 64-разрядных целых беззнаковых| Xbox идентификатора пользователя (XUID) исполнителя, выполняющего запрос.| 
| scid| guid| Идентификатор конфигурации службы для поиска.| 
| pathAndFileName| Строка| Путь и имя файла для элемента, чтобы получить доступ. Допустимые символы для части пути (вплоть до косой черты) содержат прописные буквы (A-Z), строчные буквы (a – z), цифры (0 – 9), подчеркивание (_) и косая черта (/). Компонент пути может быть пустым. Допустимые символы для части имени файла (все, что после косой черты) содержат прописные буквы (A-Z), строчные буквы (a – z), цифры (0 – 9), подчеркивание (_), точка (.) и дефис (-). Имя файла не может быть пустым, заканчиваться точкой или содержать две точки подряд.| 
| type| Строка| Формат данных. Возможные значения: двоичный файл или json.| 
  
<a id="ID4EOC"></a>

 
## <a name="valid-methods"></a>Допустимые методы

[УДАЛЕНИЕ](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-delete.md)

&nbsp;&nbsp;Удаляет файл. 

[ПОЛУЧИТЬ](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-get.md)

&nbsp;&nbsp;Загружает файл.

[PUT](uri-untrustedplatformusersxuidscidssciddatapathandfilenametype-put.md)

&nbsp;&nbsp;Передает файл. Данные могут быть отправлены в полную отправку, в которой данные и метаданные отправляются в одном сообщении, а также отправка нескольких блоков, в которой данные и метаданные отправляются в серии из небольших блоков. В одном сообщении могут отправляться только файлы размером меньше 4 МБ. Отправка нескольких блоков не поддерживается для данных типа json. 
 
<a id="ID4E5C"></a>

 
## <a name="see-also"></a>См. также
 
<a id="ID4EAD"></a>

 
##### <a name="parent"></a>Parent 

[Идентификаторы URI хранилища Title](atoc-reference-storagev2.md)

   