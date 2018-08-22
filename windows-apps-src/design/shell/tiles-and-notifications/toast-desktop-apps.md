---
author: anbare
Description: Discover the different options desktop Win32 apps have for sending toast notifications
title: Всплывающие уведомления из классических приложений
label: Toast notifications from desktop apps
template: detail.hbs
ms.author: mijacobs
ms.date: 05/01/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, win32, классический, всплывающие уведомления, мост для классических приложений, параметры для отправки всплывающих уведомлений, com-сервер, com-активатор, com, фиктивный com, не com, без com, отправка всплывающего уведомления
ms.localizationpriority: medium
ms.openlocfilehash: 8ffd6cfaeeaa8cc5f2b166a7749c565c252fbd26
ms.sourcegitcommit: f91aa1e402f1bc093b48a03fbae583318fc7e05d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/24/2018
ms.locfileid: "1917795"
---
# <a name="toast-notifications-from-desktop-apps"></a>Всплывающие уведомления из классических приложений

Классические приложения (мост для классических приложений и классические приложения Win32) могут отправлять интерактивные всплывающие уведомления так же, как приложения универсальной платформы Windows (UWP). Однако существует несколько разных параметров поддержки классических приложений из-за разных схем активации.

В этой статье приведен список параметров, которые можно использовать для отправки всплывающих уведомлений в Windows 10. Каждый параметр полностью поддерживает...

* Сохранение в центре уведомлений
* Возможность активации как из всплывающего меню, так и из центра уведомлений
* Возможность активации, когда EXE-файл не запущен

## <a name="all-options"></a>Все параметры

В таблице ниже представлены параметры поддержки всплывающих уведомлений в классическом приложении и соответствующие поддерживаемые функции. Вы можете выбрать наиболее подходящий для вашего сценария параметр.<br/><br/>

| Параметр | Визуальные элементы | Действия | Элементы ввода данных | Активация внутри процесса |
| -- | -- | -- | -- | -- |
| [Активатор COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Без COM / CLSID объекта-заглушки](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Предпочитаемый параметр — активатор COM

Это предпочитаемый параметр, который работает как с приложениями моста для классических приложений, так и с классическими приложениями Win32, и поддерживает все функции уведомлений. Не бойтесь использовать "активатор COM"; у нас есть библиотека для приложений [C#](send-local-toast-desktop.md) и [C++](send-local-toast-desktop-cpp-wrl.md), которая упрощает работу, даже если вы никогда не писали COM-сервер до этого.<br/><br/>

| Визуальные элементы | Действия | Элементы ввода данных | Активация внутри процесса |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

С помощью параметра "Активатор COM" в приложении можно использовать следующие шаблоны уведомлений и типы активации.<br/><br/>

| Шаблон и тип активации | Мост для классических приложений | Классическое приложение Win32 |
| -- | -- | -- |
| ToastGeneric (передний план) | ✔️ | ✔️ |
| ToastGeneric (фон) | ✔️ | ✔️ |
| ToastGeneric (протокол) | ✔️ | ✔️ |
| Традиционные шаблоны | ✔️ | ❌ |

> [!NOTE]
> При добавлении активатора COM в существующее приложение моста для классических приложений активации уведомлений переднего плана/фона и традиционные активации уведомлений будут активировать активатор COM вместо командной строки.

Чтобы узнать, как использовать этот параметр, см. разделы [Отправка локального всплывающего уведомлений из классических приложений C#](send-local-toast-desktop.md) или [Отправка локального всплывающего уведомлений из классических приложений C++ WRL](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Альтернативный параметр — без COM / CLSID объекта-заглушки

Если невозможно реализовать активатор COM, используйте этот альтернативный параметр. Однако вам придется пожертвовать некоторыми функциями, такими как поддержка ввода (текстовые поля на всплывающих уведомлениях) и активация внутри процесса.<br/><br/>

| Визуальные элементы | Действия | Элементы ввода данных | Активация внутри процесса |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Использование этого параметра (при реализации поддержки классических приложений Win32) накладывает значительные ограничения на доступные шаблоны уведомлений и типы активации, как показано ниже.<br/><br/>

| Шаблон и тип активации | Мост для классических приложений | Классическое приложение Win32 |
| -- | -- | -- |
| ToastGeneric (передний план) | ✔️ | ❌ |
| ToastGeneric (фон) | ✔️ | ❌ |
| ToastGeneric (протокол) | ✔️ | ✔️ |
| Традиционные шаблоны | ✔️ | ❌ |

В будущем мы опубликуем документацию с инструкциями по использованию этого параметра. Фактически отправка всплывающих уведомлений для приложений моста для классических приложений выполняется так же, как и для приложений UWP. Когда пользователь щелкает всплывающее уведомление, приложение запускается из командной строки с аргументами запуска, которые вы указали во всплывающем уведомлении.

Для классических приложений Win32 задайте AUMID, чтобы отправлять всплывающие уведомления. Также необходимо задать CLSID на ярлыке. Это может быть любой произвольный идентификатор GUID. Не добавляйте COM-сервер/активатор. Также необходимо добавить COM CLSID "объекта-заглушки", за счет чего центр уведомлений будет хранить уведомление. Обратите внимание, что можно использовать только всплывающие уведомления активации протокола, так как CLSID объекта-заглушки будет прерывать активацию любых других активаций всплывающих уведомлений. Таким образом необходимо обновить приложение для обеспечения поддержки активации протокола и активировать ваше приложение с помощью протокола всплывающих уведомлений.


## <a name="resources"></a>Ресурсы

* [Отправка локального всплывающего уведомлений из классических приложений C#](send-local-toast-desktop.md)
* [Отправка локального всплывающего уведомлений из классических приложений C++ WRL](send-local-toast-desktop-cpp-wrl.md)
* [Документация по содержимому всплывающего уведомления](adaptive-interactive-toasts.md)