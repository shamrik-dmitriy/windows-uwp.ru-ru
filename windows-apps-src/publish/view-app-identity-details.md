---
Description: Просмотрите сведения, связанные с уникальным удостоверением, назначенным вашему приложению, с помощью Microsoft Store и получите ссылку на список магазинов вашего приложения.
title: Просмотр сведений об идентификации приложений
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 07c2d3308d204d37e246a9a56c0a7203a1340dc0
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210430"
---
# <a name="view-app-identity-details"></a>Просмотр сведений об идентификации приложений


Сведения, относящиеся к уникальному удостоверению, назначенному приложению, можно просмотреть Microsoft Store на страницах **удостоверений приложения** . Вы также можете получить ссылку на список магазинов вашего приложения на этой странице.

Чтобы найти эти сведения, перейдите к одному из своих приложений и разверните пункт **Управление приложениями** в меню навигации слева. Выберите **Удостоверение приложения** для просмотра этих сведений.


## <a name="values-to-include-in-your-app-package-manifest"></a>Значения, которые должны быть включены в манифест пакета приложения

В манифест пакета должны быть добавлены следующие значения. Если вы [используете Microsoft Visual Studio для сборки пакетов](/windows/msix/package/packaging-uwp-apps) и выполнили вход, используя ту же учетную запись Майкрософт, которая связана с вашей учетной записью разработчика, эти сведения будут включены автоматически. Если вы создаете пакет вручную, необходимо добавить следующие сведения.

-   **Пакет/удостоверение/имя**
-   **Пакет, удостоверение или издатель**
-   **Пакет/свойства/PublisherDisplayName**

Дополнительные сведения см. в разделе [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) [справки по схеме манифеста пакета](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

В совокупности эти элементы являются идентификацией вашего приложения, формируя "семейство пакетов", к которому принадлежат все соответствующие пакеты. Отдельные пакеты содержат дополнительные сведения, например архитектуру и версию.


## <a name="additional-values-for-package-family"></a>Дополнительные значения для семейства пакетов

Следующие значения являются дополнительными значениями, которые относится к семейству пакетов вашего приложения, но не включены в манифест.

-   **Имя семейства пакетов (PFN)** : это значение используется в некоторых API Windows.
-   **Идентификатор безопасности пакета**: это значение понадобится для отправки уведомлений WNS в ваше приложение. Дополнительные сведения см. в [обзоре служб push-уведомлений Windows (WNS)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).


## <a name="link-to-your-apps-listing"></a>Ссылка на страницу приложения

Прямая ссылка на страницу приложения может быть предоставлена пользователям для упрощения поиска приложения в Магазине. Эта ссылка имеет формат **`https://www.microsoft.com/store/apps/<your app's Store ID>`** . Когда пользователь щелкнет эту ссылку, откроется веб-страница с описанием вашего приложения. На устройствах с Windows приложение Магазина также запустит и отобразит страницу вашего приложения.

В этом разделе также отображается **идентификатор Магазина** вашего приложения. Идентификатор Магазина можно использовать для [создания эмблем Магазина](https://developer.microsoft.com/store/badges) или идентификации вашего приложения иным образом.

**Ссылку протокола Магазина** можно использовать для создания прямой ссылки на ваше приложение в Магазине без открытия браузера, например при установке ссылки из приложения. Подробнее см. в разделе [Ссылка на приложение](link-to-your-app.md).



 

 




