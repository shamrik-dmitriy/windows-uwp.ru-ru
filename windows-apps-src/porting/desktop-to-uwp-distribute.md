---
author: normesta
Description: "Распространение упакованного классического приложения (мост для классических приложений)"
Search.Product: eADQiWindows 10XVcnh
title: "Опубликуйте ваше упакованное классическое приложение в Магазине Windows или загрузите его в неопубликованном виде на одно или несколько устройств."
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.openlocfilehash: 24a005309271a91d669322787fb8d341e1a6d6ad
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="distribute-a-packaged-desktop-app-desktop-bridge"></a>Распространение упакованного классического приложения (мост для классических приложений)

Опубликуйте ваше упакованное классическое приложение в Магазине Windows или загрузите его в неопубликованном виде на одно или несколько устройств.  

> [!NOTE]
> У вас есть план, как перевести пользователей на упакованное приложение? Перед распространением ознакомьтесь с разделом [Перевод пользователей на приложение, перенесенное из классического приложения](#transition-users) данного руководства, чтобы получить несколько идей.

## <a name="distribute-your-app-by-publishing-it-to-the-windows-store"></a>Распространение приложения за счет его публикации в Магазине Windows

[Магазин Windows](https://www.microsoft.com/store/apps)— это самый удобный для пользователей способ получить ваше приложение.

<div style="float: left; padding: 10px">
    ![Значок Магазина](images/desktop-to-uwp/store.png)
</div>
Опубликуйте ваше приложение в этом Магазине, чтобы охватить самую широкую аудиторию. Кроме того, корпоративные клиенты могут приобрести ваше приложение через [Магазин Windows для бизнеса](https://www.microsoft.com/business-store), чтобы распространять его внутри своих организаций.

Если вы планируете опубликовать приложение в Магазине Windows, но еще не обращались к нам, заполните [эту форму](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge), и корпорация Майкрософт свяжется с вами, чтобы начать процесс подключения.

Подписывать приложение перед отправкой в Магазин не требуется.

>[!IMPORTANT]
> Если вы планируете опубликовать приложение в Магазине Windows, проверьте исправность его работы на устройствах под управлением Windows 10 S. Это требование Магазина. См. статью [Тестирование приложения для Windows на Windows 10 S](desktop-to-uwp-test-windows-s.md).

## <a name="distribute-your-app-without-placing-it-onto-the-windows-store"></a>Распространение приложения без его размещения в Магазине Windows

Если вы не хотите использовать Магазин, можно распространять приложения на одно или несколько устройств вручную.

Это имеет смысл, если вам необходим более жесткий контроль над процессом распространения, либо если вы не хотите участвовать в процессе сертификации Магазина Windows.

Чтобы распространить приложение на другие устройства, не размещая его в Магазине, необходимо получить сертификат, подписать приложение с помощью этого сертификата, а затем загрузить неопубликованное приложение на эти устройства.

Вы можете [создать сертификат](../packaging/create-certificate-package-signing.md) или получить его от известного поставщика, такого как [Verisign](https://www.verisign.com/).

Если ваше приложение будет распространяться на устройства под управлением Windows10S, оно должно быть подписано Магазином Windows, поэтому вам придется отправить его в Магазин перед распространением.

Если вы создаете сертификат, его необходимо установить в хранилище сертификатов **Доверенные корневые** или **Доверенные лица** на каждом устройстве, которое запускает ваше приложение. Если сертификат получен от известного поставщика, в других системах необходимо установить только ваше приложение.  

> [!IMPORTANT]
> Убедитесь, что имя издателя на сертификате совпадает с именем издателя вашего приложения.

Чтобы подписать приложение с помощью этого сертификата, см. статью [Подписание пакета приложения с помощью SignTool](../packaging/sign-app-package-using-signtool.md).

Для загрузки неопубликованного приложения на другие устройства см. статью [Загрузка неопубликованных бизнес-приложений в Windows10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10).

**Видео**

|Публикация приложения в Магазине Windows |Распространение корпоративного приложения  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<span id="transition-users" />
## <a name="transition-users-to-your-desktop-bridge-app"></a>Перевод пользователей на приложение, перенесенное из классического приложения

Перед распространением приложения рекомендуется добавить несколько расширений в ваш манифест пакета, чтобы помочь пользователям привыкнуть использовать ваше приложение, перенесенное из классического приложения. Вот пара советов, что можно сделать.

* Определите существующие плитки начального экрана и кнопки панели задач в качестве указателей на приложение, перенесенное из классического приложения.
* Создайте ассоциацию вашего упакованного приложения с набором типов файлов.
* Обязуйте ваше приложение, перенесенное из классического приложения, открывать определенные типы файлов по умолчанию.

Полный перечень расширений и рекомендации по их использованию см. в разделе [Переход пользователей на ваше приложение](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Кроме того, в приложение, перенесенное из классического приложения, рекомендуется добавить код, который выполняет следующие задачи:

* Переносит пользовательские данные, связанные с классическим приложением, в соответствующие расположения папок перенесенного приложения.
* Позволяет пользователям удалить классическую версию вашего приложения.

Рассмотрим каждую из этих задач. Начнем с переноса пользовательских данных.

### <a name="migrate-user-data"></a>Перенос пользовательских данных

Если вы собираетесь добавить код, который переносит пользовательские данные, то лучше выполнить его только при первом запуске приложения. Прежде чем переносить данные, отобразите пользователю диалоговое окно, в котором сообщите о том, что происходит, почему этим рекомендуется воспользоваться и что случится с их имеющимися данными.

Вот пример того, как это можно выполнить в приложении, перенесенном из классического приложения на основе .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        String migrateMessage =
            "Would you like to migrate your data from the previous version of this app?";

        DialogResult migrateResult = MessageBox.Show
            (migrateMessage, "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Удаление классической версии вашего приложения

Желательно не удалять классическое приложение, предварительно не запросив разрешение у пользователя. Отобразите диалоговое окно с запросом соответствующего разрешения. Пользователи могут решить не удалять классическую версию. В этом случае вам необходимо решить, блокировать использование классического приложения или поддерживать параллельное использование обоих.

Вот пример того, как это можно выполнить в приложении, перенесенном из классического приложения на основе .NET.

Полный контекст этого фрагмента см. в файле **MainWindow.cs** примера [Средство просмотра изображений WPF с переходом, переносом и удалением](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop App is installed.
    if (uninstallString != null)
    {
        String uninstallMessage = "To have the best experience, consider uninstalling the "
            +" previous version of this app. Would you like to do that now?";

        DialogResult uninstallResult = MessageBox.Show
            (uninstallMessage, "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the app here.
            }
        }
    }

}
```

### <a name="video"></a>Видео

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Следующие шаги

**Поиск ответов на конкретные вопросы**

Наша команда следит за этими [тегами StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Оставьте отзыв об этой статье**

Используйте раздел комментариев ниже.