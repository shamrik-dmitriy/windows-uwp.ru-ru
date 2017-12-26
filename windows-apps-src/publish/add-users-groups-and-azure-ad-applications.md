---
author: jnHs
Description: "Вы можете добавлять пользователей, группы и приложения AzureAD в свою учетную запись Центра разработки."
title: "Добавление пользователей, групп и приложений AzureAD в учетную запись Центра разработки"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows10, UWP
ms.openlocfilehash: 32f62d6022d075e71ce6bcc15e1603a2e11a68a5
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/19/2017
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>Добавление пользователей, групп и приложений AzureAD в учетную запись Центра разработки

Раздел [Управление пользователями](manage-account-users.md) Центра разработки позволяет использовать Azure Active Directory для добавления пользователей в учетную запись Центра разработки. Каждому пользователю назначается роль (или набор пользовательских разрешений), которая определяет доступ пользователя к учетной записи. Вы также можете добавлять [группы пользователей](#groups) и [приложения Azure AD](#azure-ad-applications) для предоставления им доступа к вашей учетной записи Центра разработки.

После добавления пользователей к учетной записи вы можете [изменить сведения учетной записи](#edit), изменить [роли и разрешения](set-custom-permissions-for-account-users.md) или [удалить пользователей](#remove).

> [!IMPORTANT]
> Чтобы добавить пользователей в учетную запись, сначала нужно [связать учетную запись Центра разработки с клиентом Azure Active Directory вашей организации](associate-azure-ad-with-dev-center.md). 

При добавлении пользователей учитывайте следующее. (Это правила применимы к группам и приложениям Azure AD, а также к отдельным пользователям).

-   Все пользователи Центра разработки должны иметь активную учетную запись в каталоге вашей организации. При создании нового пользователя в Центре разработки также создается учетная запись для этого пользователя в клиенте Azure AD вашей организации.
-   [При изменении](#edit) имени пользователя в Центре разработки эти изменения будут отражены и в клиенте Azure Active Directory вашей организации.
-   При добавлении пользователей необходимо определить их доступ к учетной записи Центра разработки, назначив им [роль или набор пользовательских разрешений](set-custom-permissions-for-account-users.md). 

> [!NOTE]
> Если ваша организация использует [интеграцию каталогов](http://go.microsoft.com/fwlink/p/?LinkID=724033) для синхронизации локальной службы каталогов с Azure AD, вы не сможете создавать новых пользователей, группы и приложения Azure AD в Центре разработки. Вам (или другому администратору локального каталога) придется создавать их непосредственно в локальном каталоге, чтобы их можно было добавить в Центр разработки.


<span id="users" />
## <a name="add-users-to-your-dev-center-account"></a>Добавление пользователей в учетную запись Центра разработки

Добавлять отдельных пользователей в учетную запись Центра разработки можно любым из следующих способов.
-   Добавление пользователей, которые уже существуют в каталоге вашей организации
-   Создание нового пользователя с его последующим добавлением в учетную запись в Центре разработки и в каталог вашей организации
-   Добавление существующих пользователей, которые в настоящее время отсутствуют в каталоге вашей организации


<span id="from-directory" />
### <a name="add-users-from-your-organizations-directory"></a>Добавление пользователей из каталога организации

1.  На странице **Управление пользователями** выберите пункт **Добавить пользователей**.
2.  В появившемся списке выберите одного или нескольких пользователей. Чтобы найти определенных пользователей, можно воспользоваться полем поиска.
    > [!TIP]
    > При выборе более одного пользователя для добавления в учетную запись Центра разработки, необходимо назначить им одинаковые роль или набор пользовательских разрешений. Чтобы добавить несколько пользователей с разными ролями/разрешениями, выполните описанные ниже шаги повторно для каждой роли или набора пользовательских разрешений.

3.  Выбрав пользователей, нажмите **Добавить выбранные**.
4.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для выбранных пользователей.
5.  Нажмите кнопку **Сохранить**.


<span id="new-user" />
### <a name="create-a-new-user-account-in-your-organizations-directory-and-add-them-to-your-dev-center-account"></a>Создание новой учетной записи пользователя в каталоге вашей организации с ее последующим добавлением в учетную запись в Центре разработки

Если вы хотите предоставить Центру разработки доступ к новой учетной записи в клиенте Azure AD, можно создать ее в разделе **Управление пользователями**, нажав кнопку **Создать пользователя**. 

> [!IMPORTANT]
> Чтобы можно было добавлять новых пользователей в каталог, необходимо войти в клиент Azure AD с учетной записью глобального администратора.

Если необходимо предоставить новому пользователю [учетную запись глобального администратора](http://go.microsoft.com/fwlink/p/?LinkId=746654) в каталоге вашей организации, установите флажок **Сделать этого пользователя глобальным администратором в Azure AD с полным контролем над всеми ресурсами каталогов**. Это даст пользователю полный доступ ко всем административным функциям в Azure AD вашей компании. Он сможет добавлять пользователей и управлять ими в каталоге вашей организации (но не в Центре разработки, если вы не назначите ему соответствующие [роли и разрешения](set-custom-permissions-for-account-users.md)). При установке этого флажка необходимо будет предоставить **Электронную почту для восстановления пароля** для этого пользователя.

1.  На странице **Управление пользователями** выберите пункт **Добавить пользователей**.
2.  На следующей странице нажмите кнопку **Создать пользователя**.
3.  Убедитесь, что включен переключатель **Добавить в Azure AD**. За счет этого будет создана новая учетная запись в каталоге вашей организации, и пользователя будет добавлен в вашу учетную запись Центра разработки. 
4.  Введите для нового пользователя имя, фамилию и имя пользователя.
5.  Введите адрес электронной почты, который пользователь сможет использовать, если потребуется восстановить пароль. Это требуется, только если установлен флажок **Сделать этого пользователя глобальным администратором в Azure AD**.
6.  В разделе **Членство в группах** выберите группы, в которые необходимо добавить нового пользователя.
7.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для выбранных пользователей.
8.  Нажмите кнопку **Сохранить**.
9.  На странице подтверждения вы увидите учетные данные нового пользователя, в том числе временный пароль. Не забудьте записать эти сведения и предоставить их новому пользователю, так как вы не сможете найти временный пароль, после того как покинете эту страницу.


<span id="email" />
### <a name="add-a-user-from-outside-of-your-azure-ad-tenant-to-your-dev-center-account-and-your-directory"></a>Добавление пользователя, находящегося за пределами клиента Azure AD, в учетную запись Центра разработки и каталог

Чтобы пригласить пользователей, которые в настоящее время не являются частью вашего клиента Azure AD, в свою учетную запись, выполните следующие действия. Пользователи получат по электронной почте сообщение с приглашением присоединиться к учетной записи, и в клиенте Azure AD для них будет создана новая [гостевая учетная запись](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b). 

1.  На странице **Управление пользователями** выберите пункт **Добавить пользователей**.
2.  На следующей странице нажмите кнопку **Создать пользователя**.
3.  Включите переключатель **Пригласить пользователей по электронной почте**.
3.  Введите один или несколько адресов электронной почты (до 10), разделенные запятыми или точками с запятой.
4.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для выбранных пользователей.
6.  Нажмите кнопку **Сохранить**.

Приглашенные вами пользователи получат сообщение по электронной почте с приглашением на получение доступа к вашей учетной записи Центра разработки. Каждому пользователю необходимо будет принять свое приглашение, чтобы получить доступ к вашей учетной записи.

Если необходимо отправить приглашение повторно, найдите пользователя на своей странице **Управление пользователями** и выберите адрес электронной почты (или текст, сообщающий, что **Приглашение требует подтверждения**), чтобы изменить учетную запись. Затем в нижней части страницы нажмите **Повторно отправить приглашение**.


### <a name="changing-a-users-directory-password"></a>Изменение пароля пользователя для каталога

Если вы хотите изменить пароль для учетной записи пользователя, добавленной в вашу учетную запись Центра разработки, перейдите в раздел **Управление пользователями**. Обратите внимание, что это приведет к изменению пароля пользователя в вашем клиенте Azure AD, а также пароля, который пользователь использует для доступа к Центру разработки. 

Если вы указали **Электронную почту для восстановления пароля** при создании учетной записи пользователя, он сможет изменить свой пароль. Вы можете также изменить пароль пользователя, выполнив указанные ниже действия.

1.  На странице **Управление пользователями** щелкните имя учетной записи пользователя, которую хотите изменить.
2.  Внизу страницы нажмите кнопку **Сбросить пароль**.
3.  Откроется страница подтверждения с учетными данными пользователя и временным паролем.
    > [!IMPORTANT]
    >  Не забудьте распечатать или скопировать эти сведения и предоставить их пользователю, так как вы не сможете найти временный пароль после того, как покинете эту страницу.

<span id="groups" />
## <a name="add-groups-to-your-dev-center-account"></a>Добавление групп в учетную запись Центра разработки

Вы можете добавить группу из каталога организации в свою учетную запись Центра разработки. В этом случае у каждого пользователя, являющегося членом группы, будет возможность доступа к ней с разрешениями, связанными с назначенной группе ролью.

### <a name="add-groups-from-your-organizations-directory"></a>Добавление групп из каталога вашей организации

1.  На странице **Управление пользователями** выберите пункт **Добавить группы**.
2.  В появившемся списке выберите одну или несколько групп. Чтобы найти определенные группы, можно воспользоваться полем поиска.
    > [!TIP]
    > При выборе более одной группы для добавления в учетную запись Центра разработки, необходимо назначить им одинаковые роль или набор пользовательских разрешений. Чтобы добавить несколько групп с разными ролями/разрешениями, выполните описанные ниже шаги повторно для каждой роли или набора пользовательских разрешений.

3.  Выбрав группы, нажмите **Добавить выбранные**.
4.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для выбранных групп. Все члены группы будут иметь доступ к вашей учетной записи Центра разработки с разрешениями, которые вы применили к группе, независимо от того, какие роли и разрешения связаны с их отдельной учетной записью.
5.  Нажмите кнопку **Сохранить**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Создание новой учетной записи группы в каталоге вашей организации с ее последующим добавлением в учетную запись в Центре разработки

Если вы хотите предоставить доступ к Центру разработки новой учетной записи группы, можно создать группу в разделе **Управление пользователями**. Обратите внимание, что группа будет создана как в учетной записи Центра разработки, так и в каталоге вашей организации.

1.  На странице **Управление пользователями** выберите пункт **Добавить группы**.
2.  На следующей странице выберите пункт **Создать группу**.
3.  Введите отображаемое имя для новой группы.
4.  Укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для группы. Все члены группы будут иметь доступ к вашей учетной записи Центра разработки с разрешениями, которые вы применили к группе, независимо от того, какие роли и разрешения связаны с их отдельной учетной записью.
5.  В отобразившемся списке выберите пользователей, которые будут назначены новой группе. Чтобы найти определенных пользователей, можно воспользоваться полем поиска.
6.  Завершив выбор пользователей, нажмите кнопку **Добавить выбранное**, чтобы добавить пользователей в новую группу.
7.  Нажмите кнопку **Сохранить**.


<span id="azure-ad-applications" />
## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>Добавление приложений AzureAD в учетную запись Центра разработки

Можно разрешить приложениям или службам, которые являются частью Azure AD вашей организации, получать доступ к вашей учетной записи Центра разработки.

### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Добавление приложений Azure Active Directory из каталога организации

1.  На странице **Управление пользователями** выберите пункт **Добавить приложения Azure AD**.
2.  Из появившегося списка выберите одно или несколько приложений Azure Active Directory. Чтобы найти определенные приложения Azure Active Directory, можно воспользоваться полем поиска.
    > [!TIP]
    > При выборе более одного приложения Azure AD для добавления в учетную запись Центра разработки, необходимо назначить им одинаковые роль или набор пользовательских разрешений. Чтобы добавить несколько приложений Azure AD с разными ролями/разрешениями, выполните описанные ниже шаги повторно для каждой роли или набора пользовательских разрешений.

3.  Выбрав приложения Azure AD, нажмите **Добавить выбранные**.
4.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для выбранных приложений Azure AD.
5.  Нажмите кнопку **Сохранить**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Создание новой учетной записи приложения Azure AD в каталоге вашей организации с ее последующим добавлением в учетную запись в Центре разработки

Если вы хотите предоставить Центру разработки доступ к новой учетной записи приложения Azure AD, можно создать ее в разделе **Управление пользователями**. Обратите внимание: учетная запись будет создана как в учетной записи Центра разработки, так и в каталоге вашей организации.

> [!TIP]
> Если это приложение Azure AD в основном используется для проверки подлинности Центра разработки и прямой доступ пользователей к нему не требуется, можно ввести любой действительный адрес для параметров **URL-адрес ответа** и **URI кода приложения**, если эти значения не используются никакими другими приложениями Azure AD в вашем каталоге.

1.  На странице **Управление пользователями** выберите пункт **Добавить приложения Azure AD**.
2.  На следующей странице выберите пункт **Новое приложение Azure AD**.
3.  Введите **URL-адрес ответа** для нового приложения Azure AD. Это URL-адрес, по которому пользователи смогут входить и использовать ваше приложение Azure AD (иногда также называется URL-адресом приложения или URL-адресом входа). Длина **URL-адреса ответа** не может превышать 256 символов.
4.  Введите **URI-адрес идентификатора приложения** для нового приложения Azure AD. Это логический идентификатор приложения Azure AD, который предоставляется, когда оно отправляет запрос единого входа в Azure AD. Обратите внимание: **URI-адрес идентификатора приложения** должен быть уникальным для каждого приложения Azure AD в каталоге и не должен содержать больше 256 символов.
5.  В разделе **Роли** укажите [роли или пользовательские разрешения](set-custom-permissions-for-account-users.md) для приложения Azure AD.
6.  Нажмите кнопку **Сохранить**.

После добавления или создания приложения Azure AD можно вернуться в раздел **Управление пользователями** и щелкнуть имя приложения, чтобы просмотреть его параметры, включая идентификатор владельца, идентификатор клиента, URL-адрес ответа и URI кода приложения.

> [!NOTE]
> Если предполагается использовать интерфейсы API REST, предоставляемые [службами Магазина Windows](../monetize/using-windows-store-services.md), отображаемые на этой странице значения идентификатора владельца и идентификатора клиента потребуются для получения маркера доступа Azure AD, который используется для проверки подлинности при вызовах служб.   

<span id="manage-keys" />
### <a name="manage-keys-for-an-azure-ad-application"></a>Управление ключами для приложения Azure AD

Если ваше приложение Azure AD считывает и записывает данные в Microsoft Azure AD, ему понадобится ключ. Можно создавать ключи для приложения Azure AD, изменяя его информацию в Центре разработки. Вы можете также удалять ключи, которые больше не используются.

1.  На странице **Управление пользователями** выберите имя приложения Azure AD.
    > [!TIP]
    > Выбрав имя приложения Azure AD, вы увидите все активные ключи для этого приложения, включая дату, когда был создан ключ, и срок окончания его действия. Чтобы удалить ключ, который больше не используется, нажмите **Удалить**.

2.  Чтобы добавить новый ключ, нажмите **Добавить новый ключ**.
3.  Вы увидите экран, на котором отображаются значения **Код клиента** и **Ключ**.
    > [!IMPORTANT]
    > Не забудьте распечатать или скопировать эти сведения, так как вы не сможете получить к ним доступ после того, как покинете эту страницу.

4.  Если необходимо создать больше ключей, нажмите **Добавить ключ**.

<span id="edit" />
## <a name="edit-a-user-group-or-azure-ad-application"></a>Изменение пользователя, группы или приложения Azure AD

После добавления пользователей, групп и/или приложений Azure AD в учетную запись Центра разработки можно внести изменения в сведения их учетной записи. 

> [!IMPORTANT]
> Изменения, внесенные в роли или разрешения, влияют только на доступ в Центре разработки. Все другие изменения (например, изменение имени пользователя или членства в группе, или параметров "URL-адрес ответа" и "URI кода приложения" для приложения Azure AD) будут отражены в клиенте Azure AD вашей организации, а также в вашей учетной записи Центра разработки. 

1.  На странице **Управление пользователями** выберите имя пользователя, группу или учетную запись приложения Azure AD, которые хотите изменить.
2.  Внесите необходимые изменения. Элементы, которые можно редактировать:
    -   Для элемента **пользователь** допускается редактирование имени и фамилии пользователя, а также пользовательского имени. Вы также можете выбирать группы в разделе **Членство в группе** или отменять их выбор для обновления членства в группе.
    -   Для элемента **группа** можно изменять имя группы. (Чтобы обновить членство в группе, измените пользователей, которых необходимо добавить в группу или удалить из нее, и внесите изменения в разделе **Членство в группе**.)
    -   Для элемента **Приложение Azure AD** можно задавать новые значения параметрам **URL-адрес ответа** или **URI кода приложения**.
    Обратите внимание, что эти изменения затрагивают как каталог вашей организации, так и учетную запись Центра разработки.
3.  Чтобы внести изменения, связанные с доступом к Центру разработки, выберите роли, которые требуется применить (или отмените выбор ненужных ролей), или выберите **Настройка разрешений доступа** и внесите нужные изменения. Эти изменения повлияют только на доступ к Центру разработки и не затронут никакие разрешения в рамках клиента Azure AD вашей организации.
3.  Нажмите кнопку **Сохранить**.


## <a name="view-history-for-account-users"></a>Просмотр журнала для пользователей учетной записи

Являясь владельцем учетной записи, вы можете просматривать подробную историю браузера для всех дополнительных пользователей, которых вы добавили в учетную запись.

На странице **Управление пользователями** перейдите по ссылке в разделе **Последние действия**, относящейся к пользователю, историю браузера которого требуется просмотреть. Вы можете просматривать URL-адреса всех страниц, которые пользователь посетил за последние 30 дней.

<span id="remove" />
## <a name="remove-users-groups-and-azure-ad-applications"></a>Удаление пользователей, групп и приложений Azure Active Directory

Чтобы удалить пользователя, группу или приложение Azure Active Directory из учетной записи Центра разработки, выберите ссылку **Удалить**, которая появляется рядом с их именами на странице **Управление пользователями**. После подтверждения удаления учетной записи этот пользователь, группа или приложение Azure AD больше не смогут получать доступ к вашей учетной записи Центра разработки (если вы не добавите такой доступ позднее).

> [!IMPORTANT] 
> При удалении пользователь, группа или приложение Azure Active Directory теряет доступ к вашей учетной записи Центра разработки. При этом пользователь, группа или приложение Azure Active Directory **не** удаляются из каталога вашей организации.

 
