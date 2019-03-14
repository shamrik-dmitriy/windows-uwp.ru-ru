---
title: Проверка параметров прав пользователя в Unity
description: Узнайте, как проверка параметров привилегий для со знаком в учетную запись Xbox Live.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: Xbox live, xbox, игры, универсальной платформы Windows, windows 10, xbox, один, учетные записи, тестовые учетные записи, родительского контроля, пользователю привилегии, эти принудительного применения, продаже более дорогого продукта
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620409"
---
# <a name="check-user-privilege-settings-in-unity"></a>Проверка параметров прав пользователя в Unity
В Xbox Live каждого прошедшего проверку подлинности пользователя учетной записи связанные привилегии. Привилегии контролировать, какие функции Xbox Live имеет доступ пользователь в определенный момент времени. Некоторые из этих привилегий, для возможностей, управляемых system, а другие может быть связан игр или расширения подписки. Кроме того родительского контроля и эти выданный командой принудительного применения Xbox Live можно ограничить привилегии пользователя. Эти привилегии охватывают несколько распространенных сценариев, включая многопользовательские, доступ к свойству создаваемое пользователями содержимое, беседы, или потокового видео. Игры использовать эту информацию для принятия решений контроль и персонализация доступа.

## <a name="prerequisites"></a>Предварительные условия
Чтобы определить параметры прав пользователей, необходимо настроить свою игру для проверки подлинности с помощью Xbox Live и успешного входа пользователя.

>[!IMPORTANT]
> При тестировании свою игру в редакторе Unity, игры не подключен к службе Xbox Live и использует фиктивными данными для имитации подключения. В результате значение null при проверке наличия прав. Чтобы протестировать с реальными данными, выполнить сборку универсальной платформы Windows игры Unity и откройте созданный файл проекта в Visual Studio.

Следующие статьи описаны шаги, которые можно предпринять:

* [Войдите в Xbox Live в Unity (сборки и тестирования входа в систему)](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Тестирование сборки игры Unity в Visual Studio](test-visual-studio-build.md)

## <a name="determine-privileges"></a>Определить права
Права пользователя передаются в маркер, полученный во время проверки подлинности. В Unity, можно открыть список прав, доступные пользователю в `XboxLiveUser` класса, после того как пользователь успешно выполнил. Привилегии, хранятся в виде одной строки, разделенные пробелом. Например может появиться пользователя со следующими правами:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

Если вы хотите искать определенные разрешения, можно проверить на предмет `Privileges` свойство содержит соответствующий код:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>Коды привилегий
Ниже приведен список кодов возможным уровнем полномочий, которые могут быть возвращены.

| Код  | Privilege (Привилегия)  | Описание   |
|------ |-----------------------------  |-------------------    |
| 190   | Рассылка             | Может выполнять широковещательную рассылку динамической игры.     |
| 197   | view_friends_list     | Можно просмотреть список друзей другого пользователя.   |
| 198   | game_dvr              | Можно передачи записываются в игре видео в облаке.      |
| 199   | share_kinect_content          | Kinect, записанный контент можно передать в облако для пользователя и получить доступ к любой пользователь. |
| 203   | multiplayer_parties           | Можно подключиться к сеансу субъекта.     |
| 205   | communication_voice_ingame    | Может участвовать в разговора во время сторон и многопользовательские игровые сеансы.    |
| 206   | communication_voice_skype     | Можно использовать голосовой связи с Скайп на Xbox One.   |
| 207   | cloud_gaming_manage_session   | Можно выделить для и управление ими вычислительного кластера облачной размещенной игр сеанса.    |
| 208   | cloud_gaming_join_session     | Можно подключиться к сеансу облачных вычислений.     |
| 209   | cloud_saved_games     | Можно сохранить игры в облачном хранилище title.    |
| 211   | share_content     | Можно использовать содержимое совместно с другими пользователями.    |
| 214   | premium_content   | Можно приобрести, скачайте и запустите содержимого уровня "премиум", доступные в Xbox Live Gold подписки.     |
| 219   | subscription_content  | Можно приобрести и загрузить содержимое подписки уровня "премиум" и использовать функции подписки уровня "премиум".     |
| 220   | social_network_sharing    | Доступ к информации о ходе выполнения в социальных сетях.    |
| 224   | premium_video     | Можно получить доступ к видео служб "премиум".    |
| 235   | purchase_content  | Можно приобрести содержимого.     |
| 247   | user_created_content  | Можно скачать и просмотреть интерактивного пользователя, создания содержимого.    |
| 249   | profile_viewing   | Можно просматривать профили других пользователей.   |
| 252   | Связи    | Можно использовать асинхронные текст, обмен сообщениями с кем угодно.    |
| 254   | multiplayer_sessions  | Можно присоединить многопользовательские сеансов в игре.   |
| 255   | add_friend    | Можно подписаться на других пользователей Xbox Live и добавлять друзей в Xbox Live.   |