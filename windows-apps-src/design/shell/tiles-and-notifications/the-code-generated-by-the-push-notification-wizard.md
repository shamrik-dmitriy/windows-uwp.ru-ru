---
author: mijacobs
Description: By using a wizard in Visual Studio, you can generate push notifications from a mobile service that was created with Azure Mobile Services.
title: Код, генерируемый мастером push-уведомлений
ms.assetid: 340F55C1-0DDF-4233-A8E4-C15EF9030785
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 43c90d3d32876bdfd6b91f597ad94eba33907e90
ms.sourcegitcommit: 2470c6596d67e1f5ca26b44fad56a2f89773e9cc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
ms.locfileid: "1675201"
---
# <a name="code-generated-by-the-push-notification-wizard"></a>Код, генерируемый мастером push-уведомлений
 

С помощью мастера Visual Studio можно формировать push-уведомления из мобильной службы, созданной средствами мобильных служб Windows Azure. Мастер Visual Studio формирует код, облегчая начало работы. В этом разделе объясняется, как мастер изменяет ваш проект, что делает сформированный код, как его использовать и что надо сделать дальше для максимально полного использования преимуществ push-уведомлений. См. [общую информацию о службах push-уведомлений Windows (WNS)](windows-push-notification-services--wns--overview.md).

## <a name="how-the-wizard-modifies-your-project"></a>Изменения, вносимые мастером в ваш проект


Мастер push-уведомлений изменяет ваш проект следующим образом.

-   Добавление ссылки на управляемый клиент мобильных служб (MobileServicesManagedClient.dll). Неприменимо для проектов на JavaScript.
-   Добавление файла в подпапку служб и присвоение ему имени push.register.cs, push.register.vb, push.register.cpp или push.register.js.
-   Создание для мобильной службы таблицы каналов на сервере базы данных. Таблица содержит сведения, необходимые для отправки push-уведомлений в экземпляры программы.
-   Создание сценариев для четырех функций: удаление, вставка, чтение и обновление.
-   Создание сценария notifyallusers.js с пользовательским API для отправки push-уведомлений всем клиентам.
-   Добавление объявления в файл App.xaml.cs, App.xaml.vb или App.xaml.cpp либо добавление объявления в новый файл service.js (для проектов на JavaScript). Объявляется объект MobileServiceClient, который содержит необходимую информацию для подключения к мобильной службе. Обращаться к объекту MobileServiceClient с именем *MyServiceName*Client можно с любой страницы приложения, используя имя App.*MyServiceName*Client.

Файл services.js содержит следующий код:

```js
var <mobile-service-name>Client = new Microsoft.WindowsAzure.MobileServices.MobileServiceClient(
                "https://<mobile-service-name>.azure-mobile.net/",
                "<your client secret>");
```

## <a name="registration-for-push-notifications"></a>Регистрация для push-уведомлений


В файле push.register.\* метод UploadChannel регистрирует устройство для получения push-уведомлений. Магазин отслеживает установленные экземпляры вашего приложения и обеспечивает канал для push-уведомлений. См. [**PushNotificationChannelManager**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager).

Клиентский код напоминает серверные компоненты JavaScript и .NET. По умолчанию при добавлении push-уведомлений для службы серверного компонента JavaScript образец вызова notifyAllUsers пользовательского API вставляется в метод UploadChannel.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Net.Http;
using System.Text;
using System.Threading.Tasks;
using Microsoft.WindowsAzure.MobileServices;
using Newtonsoft.Json.Linq;

namespace App2
{
    internal class mymobileservice1234Push
    {
        public async static void UploadChannel()
        {
            var channel = await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            try
            {
                await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri);
                await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers");
            }
            catch (Exception exception)
            {
                HandleRegisterException(exception);
            }
        }

        private static void HandleRegisterException(Exception exception)
        {
            
        }
    }
}
```

```vb
Imports Microsoft.WindowsAzure.MobileServices
Imports Newtonsoft.Json.Linq

Friend Class mymobileservice1234Push
    Public Shared Async Sub UploadChannel()
        Dim channel = Await Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync()

        Try
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri)
            Await App.mymobileservice1234Client.GetPush().RegisterNativeAsync(channel.Uri, New String() {"tag1", "tag2"})
            Await App.mymobileservice1234Client.InvokeApiAsync("notifyAllUsers")
        Catch exception As Exception
            HandleRegisterException(exception)
        End Try
    End Sub

    Private Shared Sub HandleRegisterException(exception As Exception)

    End Sub
End Class
```

```c++
#include "pch.h"
#include "services\mobile services\mymobileservice1234\mymobileservice1234Push.h"

using namespace AzureMobileHelper;

using namespace web;
using namespace concurrency;

using namespace Windows::Networking::PushNotifications;

void mymobileservice1234Push::UploadChannel()
{
    create_task(PushNotificationChannelManager::CreatePushNotificationChannelForApplicationAsync()).
    then([] (PushNotificationChannel^ newChannel) 
    {
        return mymobileservice1234MobileService::GetClient().get_push().register_native(newChannel->Uri->Data());
    }).then([]()
    {
        return mymobileservice1234MobileService::GetClient().invoke_api(L"notifyAllUsers");
    }).then([](task<json::value> result)
    {
        try
        {
            result.wait();
        }
        catch(...)
        {
            HandleExceptionsComingFromTheServer();
        }
    });
}

void mymobileservice1234Push::HandleExceptionsComingFromTheServer()
{
}
```

```js
(function () {
    "use strict";

    var app = WinJS.Application;
    var activation = Windows.ApplicationModel.Activation;

    app.addEventListener("activated", function (args) {
        if (args.detail.kind == activation.ActivationKind.launch) {
            Windows.Networking.PushNotifications.PushNotificationChannelManager.createPushNotificationChannelForApplicationAsync()
                .then(function (channel) {
                    mymobileserviceclient1234Client.push.registerNative(channel.Uri, new Array("tag1", "tag2"))
                    return mymobileservice1234Client.push.registerNative(channel.uri);
                })
                .done(function (registration) {
                    return mymobileservice1234Client.invokeApi("notifyAllUsers");
                }, function (error) {
                    // Error

                });
        }
    });
})();
```

Теги push-уведомлений предоставляют способ ограничить уведомления до подмножества клиентов. Можно использовать для метода registerNative (или RegisterNativeAsync), чтобы зарегистрироваться на все push-уведомления без указания тегов, или можно зарегистрироваться с тегами, предоставив второй аргумент — массив тегов. При регистрации с одним или несколькими тегами вы получите только уведомления, соответствующие этим тегам.

## <a name="server-side-scripts-javascript-backend-only"></a>Серверные сценарии (только внутренний сервер JavaScript)


Для мобильных служб, которые используют внутренний сервер JavaScript, серверные сценарии выполняются только при удалении, вставке, чтении или обновлении. Сценарии не осуществляют эти операции, но выполняются после того, как соответствующие события активируются вызовом API REST в Windows Mobile со стороны клиента. После этого сценарии передают управление самим операциям, вызывая request.execute или request.respond, чтобы обеспечить ответ на контекст вызова. См. [Справочник по REST API для мобильных служб Azure](http://go.microsoft.com/fwlink/p/?linkid=511139).

В серверных сценариях доступны разнообразные функции. См. раздел [Регистрация операций с таблицами в мобильных службах Azure](http://go.microsoft.com/fwlink/p/?linkid=511140). Информацию обо всех доступных функциях см. в [справочнике по серверным сценариям мобильных служб](http://go.microsoft.com/fwlink/p/?linkid=257676).

Также создается следующий код пользовательского API в файле Notifyallusers.js:

```js
exports.post = function(request, response) {
    response.send(statusCodes.OK,{ message : 'Hello World!' })
    
    // The following call is for illustration purpose only
    // The call and function body should be moved to a script in your app
    // where you want to send a notification
    sendNotifications(request);
};

// The following code should be moved to appropriate script in your app where notification is sent
function sendNotifications(request) {
    var payload = '<?xml version="1.0" encoding="utf-8"?><toast><visual><binding template="ToastText01">' +
        '<text id="1">Sample Toast</text></binding></visual></toast>';
    var push = request.service.push; 
    push.wns.send(null,
        payload,
        'wns/toast', {
            success: function (pushResponse) {
                console.log("Sent push:", pushResponse);
            }
        });
}
```

Функция sendNotifications отправляет единичное уведомление в виде всплывающего уведомления. Можно также использовать другие типы push-уведомлений.

**Совет.**  Сведения о получении справки по редактированию сценариев см. в разделе [Активация IntelliSense для серверного JavaScript](http://go.microsoft.com/fwlink/p/?LinkId=309275).

 

## <a name="push-notification-types"></a>Типы push-уведомлений


Windows поддерживает уведомления, которые не относятся к push-уведомлениям. Общие сведения об уведомлениях см. в разделе [Выбор метода доставки уведомлений](choosing-a-notification-delivery-method.md).

Всплывающие уведомления просты в использовании; вы можете ознакомиться с примером в сформированном коде insert.js в таблице каналов. Если вы планируете использовать уведомления на плитке или в индикаторе событий, необходимо создать XML-шаблон для плитки и индикатора событий, а также указать кодировку упакованных данных в шаблоне. См. раздел [Работа с плитками, индикаторами событий и всплывающими уведомлениями](https://msdn.microsoft.com/library/windows/apps/xaml/hh868259).

Поскольку Windows отвечает на push-уведомления, эта ОС может обрабатывать большую часть таких уведомлений, пока приложение неактивно. Например, push-уведомление позволяет сообщить пользователю о поступлении нового сообщения по электронной почте, даже если локальный почтовый клиент не запущен. Windows обрабатывает всплывающее уведомление, отображая сообщение, например первую строку текстового сообщения. Windows обрабатывает плиточное уведомление или уведомление на индикаторе событий путем обновления живой плитки приложения, отображая количество новых сообщений в почте. Таким образом можно приглашать пользователей приложения проверить наличие новой информации. Ваше приложение может получать необработанные уведомления после его запуска, и их можно использовать для отправки данных в приложение. Если приложение не запущено, можно создать фоновую задачу для контроля push-уведомлений.

Push-уведомления следует использовать согласно рекомендациям для приложений универсальной платформы для Windows (UWP), так как эти уведомления задействуют ресурсы пользователя, а при чрезмерном использовании могут отвлекать внимание. См. [Руководство и контрольный список для push-уведомлений](https://msdn.microsoft.com/library/windows/apps/hh761462).

В случае обновления живых плиток с помощью push-уведомлений рекомендуется также следовать указаниям в [руководстве и контрольном списке по плиткам и индикаторам событий](https://msdn.microsoft.com/library/windows/apps/hh465403).

## <a name="next-steps"></a>Следующие действия


### <a name="using-the-windows-push-notification-services-wns"></a>Использование служб push-уведомлений Windows (WNS)

Службы push-уведомлений Windows (WNS) можно вызывать напрямую, если мобильные службы не обеспечивают достаточную гибкость, если вы хотите написать код для сервера на C# или Visual Basic либо если у вас уже есть облачная служба и вы хотите отправлять push-уведомления из нее. Вызывая WNS напрямую, можно отправлять push-уведомления из собственной облачной службы — например, из рабочей роли, которая контролирует данные, поступающие из базы данных или от другой веб-службы. Чтобы отправлять push-уведомления в приложения, ваша облачная служба должна пройти проверку подлинности WNS. См. раздел [Проверка подлинности с помощью службы push-уведомлений Windows (JavaScript)](https://msdn.microsoft.com/library/windows/apps/hh465407) или [(C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/hh868206).

Можно также отправлять push-уведомления с помощью плановой задачи в мобильной службе. См. раздел [Планирование повторяющихся заданий в мобильных службах](http://go.microsoft.com/fwlink/p/?linkid=301694).

**Внимание!**  После запуска мастера push-уведомлений не запускайте его вторично для добавления кода регистрации для другой мобильной службы. Если запустить мастер в одном проекте несколько раз, то в созданном коде появятся перекрывающиеся вызовы метода [**CreatePushNotificationChannelForApplicationAsync**](https://docs.microsoft.com/uwp/api/Windows.Networking.PushNotifications.PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync), что вызовет исключение во время выполнения. Чтобы зарегистрироваться для получения push-уведомлений в нескольких мобильных службах, запустите мастер один раз, а затем перепишите код регистрации, чтобы гарантировать, что вызовы **CreatePushNotificationChannelForApplicationAsync** не будут совершаться одновременно. Например, для этого можно переместить код, созданный мастером в файле push.register.\* (в том числе вызов **CreatePushNotificationChannelForApplicationAsync**), за пределы события OnLaunched, но конкретная реализация зависит от архитектуры приложения.

 

## <a name="related-topics"></a>Ссылки по теме


* [Обзор служб push-уведомлений Windows (WNS)](windows-push-notification-services--wns--overview.md)
* [Общие сведения о необработанных уведомлениях](raw-notification-overview.md)
* [Подключение к мобильным службам Windows Azure (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263160)
* [Подключение к мобильным службам Windows Azure (C#/C++/VB)](https://msdn.microsoft.com/library/windows/apps/xaml/dn263175)
* [Краткое руководство: добавление push-уведомлений в мобильную службу (JavaScript)](https://msdn.microsoft.com/library/windows/apps/dn263163)
 

 



