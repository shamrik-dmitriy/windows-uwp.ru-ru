---
Description: Изучите этот список, чтобы избежать часто возникающих проблем, из-за которых приложения не проходят сертификацию, а также тех проблем, которые могут быть обнаружены при выборочной проверке после публикации приложения.
title: Недопущение распространенных ошибок при сертификации
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0032a5bbaafabab3c847b2b7c48536873f4532dd
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826167"
---
# <a name="avoid-common-certification-failures"></a>Недопущение распространенных ошибок при сертификации


Изучите этот список, чтобы избежать часто возникающих проблем, из-за которых приложения не проходят сертификацию, а также тех проблем, которые могут быть обнаружены при выборочной проверке после публикации приложения.

> [!NOTE]
> Обязательно просмотрите [политики Microsoft Store](store-policies.md) чтобы ваше приложение удовлетворяет всем требованиям, в списке.

-   Отправляйте приложение, только когда оно закончено. Вы можете упоминать о планируемых возможностях в описании приложения, однако необходимо исключить из приложения незавершенные разделы, ссылки на веб-страницы, находящиеся в разработке, и прочие элементы, которые вызовут у пользователя ощущение незаконченности приложения.

-   Перед тем как отправить приложение, [протестируйте его с помощью комплекта сертификации приложений для Windows](../debug-test-perf/windows-app-certification-kit.md).

-   Проверьте работу приложения в нескольких разных конфигурациях, чтобы убедиться, что оно работает максимально стабильно.

-   Проверьте, чтобы ваше приложение не завершается аварийно при отсутствии подключения к сети. Даже если подключение необходимо для работы приложения, оно должно корректно обрабатывать ситуацию с отсутствием подключения.

-   [Предоставьте всю информацию](notes-for-certification.md), необходимую для использования приложения, в том числе имя пользователя и пароль тестовой учетной записи, если приложение должно выполнять вход в сетевую службу, а также описание всех действий, необходимых для доступа к скрытым или заблокированным возможностям.

-   Включить [URL-адрес политики конфиденциальности](enter-app-properties.md#privacy-policy-url) Если вашему приложению требуется один; например, если приложение обращается к любые персональные данные любым способом, или в противном случае требуется по закону. Чтобы определить, если вашему приложению требуется политика конфиденциальности, просмотрите [соглашение с разработчиком приложений](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) и [политики Microsoft Store](store-policies.md).

-   Убедитесь, что описание приложения четко отражает его функции. В этом вам поможет руководство по [созданию эффективного описания приложения](write-a-great-app-description.md).

-   Предоставьте полные и точные ответы на все вопросы в разделе [Возрастные категории](age-ratings.md).

-   Не [объявляйте приложение поддерживающим специальные возможности](product-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), если вы не спроектировали его таким намеренно и не проверили его в сценариях использования специальных возможностей.

-   Если в приложении используются коммерческие API Магазина Windows из пространства имен [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), обязательно протестируйте приложение и удостоверьтесь в том, что обрабатываются типичные исключения. Кроме того, убедитесь, что в приложении используется класс [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp), а не класс [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), который предназначен только для тестирования. (Обратите внимание, что если ваше приложение предназначено для Windows 10 версии 1607 или более поздней, для управления покупками из приложения рекомендуется использовать элементы, входящие в пространство имен [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store), а не Windows.ApplicationModel.Store.)


 

 




