---
ms.assetid: D677E126-C3D6-46B6-87A5-6237EBEDF1A9
description: Используйте этот метод в API отправки в Microsoft Store для удаления существующей отправки надстройки.
title: Удаление отправки надстройки
ms.date: 04/17/2018
ms.topic: article
keywords: Windows 10, UWP, API отправки в Microsoft Store, отправка надстройки, удаление, продукт внутри приложения, IAP
ms.localizationpriority: medium
ms.openlocfilehash: 4a694846c745fbfbc175781dd76cc983e402b676
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334923"
---
# <a name="delete-an-add-on-submission"></a>Удаление отправки надстройки

Используйте этот метод в API отправки в Microsoft Store, чтобы удалить существующую отправку надстройки (также называется продуктом внутри приложения или IAP).

## <a name="prerequisites"></a>предварительные требования

Для использования этого метода сначала необходимо сделать следующее:

* Если вы еще не сделали этого, выполните все [необходимые условия](create-and-manage-submissions-using-windows-store-services.md#prerequisites) для API отправки в Microsoft Store.
* [Получите маркер доступа Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token), который будет использоваться в заголовке запроса этого метода. После получения токена доступа у вас будет 60 минут, чтобы использовать его до окончания его срока действия. После истечения срока действия токена можно получить новый токен.

## <a name="request"></a>Запрос

У этого метода следующий синтаксис. Примеры использования и описание текста заголовка и запроса приведены в следующих разделах.

| Метод | Универсальный код ресурса (URI) запроса                                                      |
|--------|------------------------------------------------------------------|
| DELETE    | `https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/{inAppProductId}/submissions/{submissionId}` |


### <a name="request-header"></a>Заголовок запроса

| Header        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| Authorization | строка | Обязательный. Маркер доступа Azure AD в форме **носителя** &lt; *маркера*&gt;. |


### <a name="request-parameters"></a>Параметры запроса

| Имя        | Тип   | Описание                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| inAppProductId | строка | Обязательный. Код продукта в Магазине надстройки, содержащей отправку, которую требуется удалить. Идентификатор Store доступен в центре партнеров.  |
| submissionId | строка | Обязательный. Идентификатор отправки для удаления. Этот идентификатор добавляется в данные ответов для запросов на [создание отправки надстройки](create-an-add-on-submission.md). Для отправки, который был создан в центре партнеров этот идентификатор также доступна в URL-АДРЕСЕ для отправки страницы в центре партнеров.  |


### <a name="request-body"></a>Тело запроса

Предоставлять текст запроса для этого метода не требуется.


### <a name="request-example"></a>Пример запроса

В следующем примере показано, как удалить отправку надстройки.

```json
DELETE https://manage.devcenter.microsoft.com/v1.0/my/inappproducts/9NBLGGH4TNMP/submissions/1152921504621230023 HTTP/1.1
Authorization: Bearer <your access token>
```

## <a name="response"></a>Ответ

В случае успеха этот метод возвращает пустой ответ.

## <a name="error-codes"></a>Коды ошибок

Если запрос не удается выполнить, ответ будет содержать один из следующих кодов ошибок HTTP.

| Код ошибки |  Описание   |
|--------|------------------|
| 400  | Недопустимые параметры запроса. |
| 404  | Не удалось найти указанную отправку. |
| 409  | Обнаружен указанный отправки, но его не удалось удалить в ее текущем состоянии или надстройка использует возможности центра партнеров, [в настоящее время не поддерживается API отправки Microsoft Store](create-and-manage-submissions-using-windows-store-services.md#not_supported). |


## <a name="related-topics"></a>См. также

* [Создание и управление отправкой, с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Получить отправки надстройки](get-an-add-on-submission.md)
* [Создайте надстройку отправки](create-an-add-on-submission.md)
* [Зафиксировать отправки надстройки](commit-an-add-on-submission.md)
* [Обновление надстройки отправки](update-an-add-on-submission.md)
* [Получение сведений о состоянии отправки надстройки](get-status-for-an-add-on-submission.md)
