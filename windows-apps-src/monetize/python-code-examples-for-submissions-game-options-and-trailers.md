---
description: Используйте приведенные в этом разделе примеры кода на Python, чтобы подробнее ознакомиться с использованием API отправки в Microsoft Store для отправки трейлеров и параметров игры.
title: Пример на языке Python — отправка приложения с трейлерами и параметрами игры
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API отправки в Microsoft Store, примеры кода, параметры игры, трейлеры, дополнительные предложения, python
ms.localizationpriority: medium
ms.openlocfilehash: 5d68058d1bf53d936c9d88a3725c7c1dd9a3f443
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334992"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Пример на языке Python: отправка приложения с трейлерами и параметрами игры

В этой статье представлены примеры кода на Python по использованию [API отправки в Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) для решения этих задач.

* Получите маркер доступа Azure AD для использования с API отправки в Microsoft Store.
* Создание отправки приложения
* Настройте данные описания в Магазине для отправки приложения, в том числе дополнительные параметры описания для [игр](manage-app-submissions.md#gaming-options-object) и [трейлеров](manage-app-submissions.md#trailer-object).
* Выложите ZIP-файл, содержащий пакеты, изображения для описания и файлы трейлера для отправки приложения.
* Передайте отправку приложения.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Создание отправки приложения

Этот код вызывает другие класса и функции из примера по использованию API отправки в Microsoft Store для создания и передачи отправки приложения, содержащей трейлер и параметры игры. Адаптация кода для собственного использования.

* Назначьте переменной `tenant` идентификатор арендатора для своего приложения и назначьте переменным `client` и `secret` идентификатор и ключ клиента для своего приложения. Дополнительные сведения см. в разделе [как связать приложение Azure AD с помощью учетной записи центра партнеров](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Назначьте переменной `application_id`[код продукта в Магазине](in-app-purchases-and-trials.md#store-ids) для приложения, отправку которого необходимо создать.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Получите маркер доступа Azure AD и вызовите API отправки

В следующем примере определяются следующие классы:

* Класс `DevCenterAccessTokenClient` определяет вспомогательный метод, который использует ваши значения `tenantId`, `clientId` и `clientSecret` для создания маркера доступа Azure AD для использования с API отправки в Microsoft Store.
* Класс `DevCenterClient` определяет вспомогательные методы, которые вызывают различные методы в API отправки в Microsoft Store и выкладывают ZIP-файл, содержащий пакеты, изображения для описания и файлы трейлера для отправки приложения.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>Получение данных описания отправки приложения

В следующем примере определяются вспомогательные функции, которые возвращают данные описания в формате JSON для примера новой отправки приложения.

> [!div class="tabbedCodeSnippets"]
[!code-python[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>См. также

* [Создание и управление отправкой, с помощью служб Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
