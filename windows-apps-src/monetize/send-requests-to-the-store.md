---
author: mcleanbyron
Description: "Можно использовать метод SendRequestAsync для отправки запросов в Магазин Windows для операций, еще не имеющих API, доступного в Windows SDK."
title: "Отправка запросов в Магазин Windows"
ms.assetid: 070B9CA4-6D70-4116-9B18-FBF246716EF0
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, StoreRequestHelper, SendRequestAsync
ms.openlocfilehash: a949b3c93bb5b4a056f9e1fa4679e8ddca05d899
ms.sourcegitcommit: a9e4be98688b3a6125fd5dd126190fcfcd764f95
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/21/2017
---
# <a name="send-requests-to-the-windows-store"></a>Отправка запросов в Магазин Windows

Начиная с Windows 10 версии 1607, пакет Windows SDK содержит API для операций, связанных с Магазином (например, покупки из приложения) в пространстве имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store). Но хотя службы, поддерживающие работу с Магазином, постоянно обновляются, расширяются и улучшаются от одного выпуска ОС к другому, новые API обычно добавляются в пакет Windows SDK только во время основных выпусков ОС.

Мы предоставляем метод [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_) как гибкий способ сделать новые операции Магазина доступными для приложений универсальной платформы Windows (UWP) перед выпуском новой версии Windows SDK. Этот метод можно использовать для отправки запросов в Магазин для новых операций, у которых еще нет соответствующих API, доступных в последнем выпуске пакета Windows SDK.

> [!NOTE]
> Метод **SendRequestAsync** доступен только для приложений, предназначенных для Windows 10 версии 1607 или выше. Некоторые запросы, поддерживаемые этим методом, поддерживаются только в выпусках, появившихся после Windows 10 версии 1607.

**SendRequestAsync** — это статический метод класса [StoreRequestHelper](https://docs.microsoft.com/uwp/api/windows.services.store.storerequesthelper). Для вызова этого метода необходимо передать ему указанную ниже информацию.
* Объект [StoreContext](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext), предоставляющий информацию о пользователе, для которого выполняется операция. Дополнительные сведения об этом объекте см. в разделе [Начало работы с классом StoreContext](in-app-purchases-and-trials.md#get-started-with-the-storecontext-class).
* Целое число, определяющее запрос, который необходимо отправить в Магазин.
* Если запрос поддерживает любые аргументы, можно также передавать строку формата JSON, содержащую аргументы, передаваемые вместе с запросом.

В следующем примере показан вызов этого метода. В этом примере требуется использование операторов для пространств имен **Windows.Services.Store** и **System.Threading.Tasks**.

```csharp
public async Task<bool> AddUserToFlightGroup()
{
    StoreSendRequestResult result = await StoreRequestHelper.SendRequestAsync(
        StoreContext.GetDefault(), 8,
        "{ \"type\": \"AddToFlightGroup\", \"parameters\": \"{ \"flightGroupId\": \"your group ID\" }\" }");

    if (result.ExtendedError == null)
    {
        return true;
    }

    return false;
}
```

См. в следующих разделах сведения о запросах, которые в настоящее время доступны через метод **SendRequestAsync**. Мы обновим эту статью после добавления поддержки новых запросов.

## <a name="requests-for-flight-group-scenarios"></a>Запросы для сценариев группы тестируемой возможности

> [!IMPORTANT]
> Все запросы группы тестируемой возможности, описанные в этом разделе, недоступны в настоящее время большинству учетных записей разработчика. Эти запросы не будут выполнены, если учетная запись разработчика не предоставлена специально корпорацией Майкрософт.

Метод **SendRequestAsync** поддерживает набор запросов для сценариев группы тестируемой возможности, например добавления пользователя или устройства в группу тестируемой возможности. Чтобы отправить эти запросы, передайте значение 7 или 8 в параметр *requestKind*, а также строку формата JSON в параметр *parametersAsJson*, указывающий запрос, который вы хотите отправить вместе с какими-либо связанными аргументами. Эти значения *requestKind* отличаются следующим образом.

|  Значение типа запроса  |  Описание  |
|----------------------|---------------|
|  7                   |  Запросы выполняются в контексте текущего устройства. Это значение может использоваться только в Windows 10 версии 1703 или более поздней.  |
|  8                   |  Запросы выполняются в контексте пользователя, который в данный момент вошел в Магазин. Это значение может использоваться в Windows 10 версии 1607 или более поздней.  |

В настоящее время реализованы перечисленные ниже запросы группы тестовой возможности.

### <a name="retrieve-remote-variables-for-the-highest-ranked-flight-group"></a>Получение удаленных переменных для группы тестируемой возможности с самым высоким приоритетом.

> [!IMPORTANT]
> Этот запрос на данный момент недоступен для большинства учетных записей разработчиков. Этот запрос не будет выполнен, если учетная запись разработчика не предоставлена специально корпорацией Майкрософт.

Этот запрос извлекает удаленные переменные для группы тестируемой возможности с самым высоким приоритетом для текущего пользователя или устройства. Чтобы отправить этот запрос, передайте указанную ниже информацию в параметры *requestKind* и *parametersAsJson* метода **SendRequestAsync**.

|  Параметр  |  Описание  |
|----------------------|---------------|
|  *requestKind*                   |  Укажите 7 для возврата группы тестируемой возможности с самым высоким приоритетом для устройства или 8 для возврата группы тестируемой возможности с самым высоким приоритетом для текущего пользователя и устройства. Рекомендуется использовать значение 8 для параметра *requestKind*, поскольку это значение будет возвращать группу тестируемой возможности с самым высоким приоритетом через членство и для текущего пользователя, и для устройства.  |
|  *parametersAsJson*                   |  Передайте строку формата JSON, содержащую данные, как показано в примере ниже.  |

В следующем примере показан формат данных JSON для передачи в *parametersAsJson*. Поле *type* должно быть назначено строке *GetRemoteVariables*. Назначьте поле *projectId* идентификатору проекта, в котором вы определили удаленные переменные на информационной панели Центра разработки для Windows.

```json
{ 
    "type": "GetRemoteVariables", 
    "parameters": "{ \"projectId\": \"your project ID\" }" 
}
```

После отправки этого запроса свойство [Response](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_Response) возвращаемого значения [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) содержит строку формата JSON со следующими полями.

|  Поле  |  Описание  |
|----------------------|---------------|
|  *anonymous*                   |  Логическое значение, где **true** указывает, что удостоверение пользователя или устройства не было представлено в запросе, а **false** указывает, что удостоверение пользователя или устройства было представлено в запросе.  |
|  *name*                   |  Строка, содержащая имя группы тестируемой возможности с самым высоким приоритетом, к которой принадлежит устройство или пользователь.  |
|  *settings*                   |  Словарь пар "ключ— значение", содержащий имя и значение удаленных переменных, которые разработчик настроил для группы тестируемой возможности.  |

В следующем примере показано возвращаемое значение для этого запроса.

```json
{ 
  "anonymous": false, 
  "name": "Insider Slow",
  "settings":
  {
      "Audience": "Slow"
      ...
  }
}
```

### <a name="add-the-current-device-or-user-to-a-flight-group"></a>Добавление текущего устройства или пользователя в группу тестируемой возможности

> [!IMPORTANT]
> Этот запрос на данный момент недоступен для большинства учетных записей разработчиков. Этот запрос не будет выполнен, если учетная запись разработчика не предоставлена специально корпорацией Майкрософт.

Чтобы отправить этот запрос, передайте указанную ниже информацию в параметры *requestKind* и *parametersAsJson* метода **SendRequestAsync**.

|  Параметр  |  Описание  |
|----------------------|---------------|
|  *requestKind*                   |  Укажите 7, чтобы добавить устройство в группу тестируемой возможности, или 8, чтобы добавить в эту группу пользователя, который в данный момент вошел в Магазин.  |
|  *parametersAsJson*                   |  Передайте строку формата JSON, содержащую данные, как показано в примере ниже.  |

В следующем примере показан формат данных JSON для передачи в *parametersAsJson*. Поле *type* должно быть назначено строке *AddToFlightGroup*. Назначьте поле *flightGroupId* идентификатору группы тестируемой возможности, к которому вы хотите добавить устройство или пользователя.

```json
{ 
    "type": "AddToFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Если возникает ошибка с запросом, свойство [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) возвращаемого значения [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) содержит код ответа.

### <a name="remove-the-current-device-or-user-from-a-flight-group"></a>Удаление текущего устройства или пользователя из группы тестируемой возможности

> [!IMPORTANT]
> Этот запрос на данный момент недоступен для большинства учетных записей разработчиков. Этот запрос не будет выполнен, если учетная запись разработчика не предоставлена специально корпорацией Майкрософт.

Чтобы отправить этот запрос, передайте указанную ниже информацию в параметры *requestKind* и *parametersAsJson* метода **SendRequestAsync**.

|  Параметр  |  Описание  |
|----------------------|---------------|
|  *requestKind*                   |  Укажите 7, чтобы удалить устройство из группы тестируемой возможности, или 8, чтобы удалить из этой группы пользователя, который в данный момент вошел в Магазин.  |
|  *parametersAsJson*                   |  Передайте строку формата JSON, содержащую данные, как показано в примере ниже.  |

В следующем примере показан формат данных JSON для передачи в *parametersAsJson*. Поле *type* должно быть назначено строке *RemoveFromFlightGroup*. Назначьте поле *flightGroupId* идентификатору группы тестируемой возможности, из которой вы хотите удалить устройство или пользователя.

```json
{ 
    "type": "RemoveFromFlightGroup", 
    "parameters": "{ \"flightGroupId\": \"your group ID\" }" 
}
```

Если возникает ошибка с запросом, свойство [HttpStatusCode](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult#Windows_Services_Store_StoreSendRequestResult_HttpStatusCode) возвращаемого значения [StoreSendRequestResult](https://docs.microsoft.com/uwp/api/windows.services.store.storesendrequestresult) содержит код ответа.

## <a name="related-topics"></a>Статьи по теме

* [SendRequestAsync](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreRequestHelper#Windows_Services_Store_StoreRequestHelper_SendRequestAsync_Windows_Services_Store_StoreContext_System_UInt32_System_String_)