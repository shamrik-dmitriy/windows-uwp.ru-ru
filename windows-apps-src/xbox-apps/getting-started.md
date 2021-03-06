---
title: Начало разработки приложений UWP на Xbox One
description: Настройка компьютера и Xbox One для разработки приложений UWP.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 19756730177485c6d16ad9a42ff1174eba8ca3b9
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820312"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Начало разработки приложений UWP на Xbox One

**Внимательно** выполните эти действия для успешной настройки компьютера и консоли Xbox One для разработки приложений универсальной платформы Windows (UWP). После настройки вы можете подробнее изучить режим разработчика на Xbox One и создание приложений UWP на странице [UWP для Xbox One](index.md). 

## <a name="before-you-start"></a>Прежде чем начать

Перед началом работы выполните следующие действия.
-   Настройка ПК с последней версией Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2019.

    > [!NOTE]
    > Visual Studio 2019 is required if you are using the Windows 10, build 15063 SDK. -->

- Выделите как минимум пять гигабайт свободного места на консоли Xbox One.

## <a name="setting-up-your-development-pc"></a>Настройка компьютера разработки

1.  Установите Visual Studio 2015 с обновлением 3, Visual Studio 2017 или Visual Studio 2019.

    При установке Visual Studio 2015 с обновлением 3, убедитесь, что выбран **Custom** установки и выберите **средства разработки универсальных приложений Windows** флажок – это не установить частью по умолчанию. Если вы разработчик на языке C++, обязательно выберите вариант **Выборочная установка** и выберите **C++** .

    При установке Visual Studio 2017 или Visual Studio 2019 г., убедитесь, что выбран **разработки универсальной платформы Windows** рабочей нагрузки. Если вы являетесь разработчиком на C++, в **Сводка** панели справа в разделе **разработки универсальной платформы Windows**, убедитесь, что **средства универсальной платформы Windows C++** флажок. Он не является частью установки по умолчанию.

    Дополнительные сведения см. в разделе [Настройка вашей универсальной платформы Windows в среде разработки Xbox](development-environment-setup.md).

2.  Установите последнюю версию [пакета SDK для Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Включить режим разработчика для Компьютере разработки (**параметры / обновление и безопасность / для разработчиков или использовать возможности для разработчиков и режим разработчика**).

Теперь, когда компьютер для разработки готов, можно посмотреть это видео или продолжить чтение инструкций по настройке Xbox One для разработки, созданию и развертыванию приложения UWP на консоли.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Настройка консоли Xbox One

1.  Включите режим разработчика на Xbox One. Загрузка приложения, получить код активации и затем введите его в **консолей управления Xbox One** страницы в учетной записи разработчика приложений центра партнеров. Дополнительные сведения см. на странице [Активация режима разработчика Xbox One](devkit-activation.md). 

2.  Откройте **активация режима разработки** приложении и выберите **коммутатора и перезапустите**. Поздравляем, теперь ваша консоль Xbox One находится в режиме разработчика!
  
  > [!NOTE]
  > Ваши коммерческие игры и приложения не будут запускаться в режиме разработчика, но созданные вами игры и приложения — будут. Переключитесь в коммерческий режим для запуска любимых игр и приложений.
    
  > [!NOTE]
  > Перед развертыванием приложения на Xbox One в режиме разработчика необходимо, чтобы пользователь выполнил вход на консоли. Можно использовать существующую учетную запись Xbox Live или создать новую учетную запись для консоли в режиме разработчика. 

## <a name="creating-your-first-project-in-visual-studio"></a>Создание первого проекта в Visual Studio

Дополнительные сведения см. в разделе [Настройка вашей универсальной платформы Windows в среде разработки Xbox](development-environment-setup.md).

1.  **Для C#** : Создайте новый проект универсальной Windows, а затем в **обозревателе решений**, щелкните правой кнопкой мыши проект и выберите **свойства**. Выберите **Отладка** вкладке **целевое устройство** для **удаленный компьютер**, введите IP-адрес или имя узла в консоли Xbox One **удаленный компьютер**  и выберите **универсальный (незашифрованный протокол)** в **режим проверки подлинности** стрелку раскрывающегося списка.   

    Чтобы узнать IP-адрес Xbox One, запустите Dev Home на консоли (большая плитка справа от плитки главной страницы) и посмотрите на левый верхний угол. Дополнительные сведения о Dev Home см. в разделе [Вводные сведения об инструментах Xbox One](introduction-to-xbox-tools.md).  

2.  **Для проектов C++ и HTML/Javascript**: Выполните аналогичную путь C# проекты, но в свойствах проекта перейти к **Отладка** выберите **удаленный компьютер** в отладчике, чтобы открыть список раскрывающегося списка, введите IP-адрес или имя узла консоль в **имя машины** и выберите **универсальный (незашифрованный протокол)** в **тип проверки подлинности** поля.

3. Выберите **x64** из раскрывающегося списка слева от зеленую кнопку воспроизведения в верхней строке меню.
   
4.  Если нажать F5, приложение будет построено и развернуто на Xbox One.
  
5.  При первой попытке Visual Studio запросит PIN-код для Xbox One. ПИН-код можно получить, запустив домашней разработки на Xbox One и выбрав **ПИН-кода в Visual Studio, Показать** кнопки.
  
6.  После привязки начнется развертывание приложения. При первой попытке процесс может быть немного медленным (необходимо скопировать все инструменты на консоль Xbox), но если он занимает более, чем несколько минут, что-то не так. Убедитесь, что вы следовали всем инструкциям выше (в том числе выбрали для параметра **Режим проверки подлинности** значение **Универсальный**) и используете проводное подключение к Xbox One.  

7. Откиньтесь на спинку и расслабьтесь. Наслаждайтесь своим первым приложением на консоли!  

## <a name="thats-it"></a>Вот и все!

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>См. также  
- [Вопросы и ответы](frequently-asked-questions.md)  
- [Известные проблемы с универсальной платформы Windows на программе для разработчиков Xbox](known-issues.md)
- [Приложения UWP для Xbox One](index.md) 
